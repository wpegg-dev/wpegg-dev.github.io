---
layout: post
title:  "Hierarchical (Tree) Traversal In Oracle SQL"
date:   2015-05-17 19:14:20
categories: programming
---
![sql_tree](https://cloud.githubusercontent.com/assets/11460318/7672895/0343c916-fcd3-11e4-95df-795fb6cecaf6.png)
I have been working on a project for the past few weeks using a technology that I am not very familiar with, Oracle Forms, and have had the opportunity to learn quite a bit of PL-SQL seeing as how the whole framework is built on it.
I am familiar with SQL as I have been using it for many years in various other projects but I ran into a pretty significant issue while working on a piece of code last week. How do I handle passing information from a parent record to it's children records if
they exist in the same table? Doing a quick Google search you'll probably find a lot of people who are using a special syntax for this scenario. They are using a [CONNECTED BY](http://docs.oracle.com/cd/B19306_01/server.102/b14200/queries003.htm) clause. 
<pre>
	SELECT emp_id, full_name, manager_id
	FROM emp
	CONNECTED BY PRIOR emp_id = manager_id;
</pre>
<br><br>
I did a few quick testes and much to my surprise it worked pretty well! I was able to specify a parent or root item and the query would return all of the children that are related to it. I did run into a problem where
there were some cases when a tree would make a circuit/cycle/loop and when that happens Oracle would issue an error message. Luckily, Oracle had thought about this and allowed for the use of an additional clause which can be used called
[NOCYCLE](http://www.dba-oracle.com/t_advanced_sql_connect_by_loop.htm). 
<pre>
	SELECT emp_id, full_name, manager_id
	FROM emp
	CONNECTED BY NOCYCLE PRIOR emp_id = manager_id;
</pre>
By using this, Oracle will handle the error and continue to return you all of the relatives of the specified root.
The point of what I was working on was to take some information about the root node and cascade it to all of the children. With this query working I thought it would be easy to just combine it with an UPDATE query 
and poof all done! Not so much. As it turns out working with hierarchical data in Oracle SQL is very taxing on performance and can get VERY slow when using large datasets 
and/or large tree structures. Both of theses scenarios happen to describe the data that I was working with. So it was back to the drawing board!
<br><br>
After a few hours of brainstorming with a colleague we came up with an idea. It is by no means revolutionary but we figured it was worth a shot. We knew that the dataset that we were dealing with was pretty large (500K - 1M rows of data)
but that the tree structures themselves hardly ever got below the 6th [level](http://en.wikipedia.org/wiki/Tree_%28data_structure%29) of the relationship so why not go from leaf to root? We also knew that there was one column of 
each row which we could use to help constrain the subset of data that we were using. With these two pieces of information we set off to see what we could come up with.
<br><br>
After several hours of trial and error we came up with a solution that allowed us to achieve our goal in 2% of the time using the standard built in Oracle hierarchy syntax! Our solution was to build out two functions. One which was used
to identify all of the rows which were not identifiable as a root. Each node would then be passed into a new recursive function which would find the highest parent part in the tree which was confined to our newly identified columns.
Once the parent was located and returned to the calling function, an UPDATE was performed to update the current data's columns with the parents information. 
<br><br>
<pre>
--this will help us to not create cycles when searching data
g_parents_list LONG;

FUNCTION get_parent(leaf_id IN VARCHAR2) RETURN VARCHAR2 IS
	v_parent_id VARCHAR2(10);
	v_return VARCHAR2(10);
BEGIN
	SELECT relational_column INTO v_parent_id
	FROM table_name
	WHERE column_id = leaf_id
	AND relational_column IS NOT NULL;
	
	IF v_parent_id IS NOT NULL THEN
		-- keep from running into problem with cycles
		IF INSRT(g_parents_list,v_parent_id) < 0 THEN
			g_parents_list := g_parents_list||','||leaf_id;
			v_return := my_package.get_parent(v_parent_id);     
		ELSE
			v_return := leaf_id;
			g_parents_list := '';
			return v_return;
	ELSE
		return leaf_id;
	END IF;
	
	EXCEPTION
		WHEN OTHERS THEN
			return 'Error';
END get_parent;

PROCEDURE update_hierarchy IS
	CURSOR leaf_data
	SELECT * 
	FROM table_name 
	WHERE relational_column IS NOT NULL 
	AND relational_column <> column_id; 
	--^This will ensure that we don't run into the cycle problem!
	v_parent_id VARCHAR2(10);
BEGIN
	FOR leaves IN leaf_data
	LOOP
		v_parent_id := my_package.get_parent(leaves.column_id);
		FOR parent_data IN (
			SELECT * 
			FROM table_name 
			WHERE column_id = v_parent_id
		)
		LOOP
			UPDATE table_name 
			SET column_name_1 = parent_data.column_name_1, 
			column_name_2 = parent_data.column_name_2
			WHERE column_id = leaves.column_id;
		END LOOP;
	END LOOP;
END update_hierarchy;
</pre>
<br><br>
Like I said not revolutionary but it was my first experience with such a scenario and it was amazing to see the improvement in performance! If anyone is using large datasets and is running into a problem when trying to do
hierarchical data navigation I would ABSOLUTELY recommend going from leave to root if at all possible!

{% include disqus.html %}