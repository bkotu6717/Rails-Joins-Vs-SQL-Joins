# Rails-Joins-Vs-SQL-Joins
<p>This repo gives understanding about writing SQL joins equivalent to Rails ActiveRecord Join Queries</p>

## Joins introduction
      A = {1,2,3,4,5}
      B = {6,7,8,9,10}
      A U B = {1,2,3,4,5,6,7,8,9,10}
      A Intersection B = {} # There are no common elements between A and B
      A-B, B-A are the remaining operations

## Joins uses
<ul>
<li>Joins helps in RDBMS to combine and retrieve data from muliple tables which are connected through relationships.<l/i>
<li>The queries are actually faster when we write in native sql language rather than with any wrapper language(eg: Rails ActiveRecord)</li>
<li>We can perform join operations on more than one table always provided they are connected.</li>
</ul>


## Types Of Joins
  1. Inner Join
  2. Left Outer Join or Left Join
  3. Right Outer Join or Right Join
  4. Full Outer Join
  5. Self Join

## 1. Inner Join
      Inner Join is nothing but Mathematical set Intersection operation.
      This joins brings the records from all the tables's columns from all the table used to join by default.
      The inner join performed on multiple tables is AND operation.

      class Post < ActiveRecord::Base
        has_many :comments
        has_many :likes
        has_many :shares
      end

      class Comment < ActiveRecord::Base
        belongs_to :post
      end
      class Like < ActiveRecord::Base
        belongs_to :post
      end
      class Share < ActiveRecord::Base
        belongs_to :post
      end

      Rails Inner Join for above
      
      Post.joins(:comments, :likes, :shares)
      ### Important notes
      Posts with comments, likes and shares only will be retried
      Posts with absence of atleast one of comments, likes or shares won't be retrieved


      SQL Inner join for above

      SELECT * FROM posts
      INNER JOIN comments ON comments.post_id = posts.id
      INNER JOIN likes ON likes.post_id = posts.id
      INNER JOIN shares ON shares.post_id = posts.id;

      ### **Important notes**
      Posts with comments, likes and shares only will be retried
      Posts with absence of atleast one of comments, likes or shares won't be retrieved


## 1. Get employees who are getting paid high in each department

      select e.emp_no, e.first_name||' '||e.last_name as emp_name, dp.dept_name, s.salary as max_salary from employees e
      inner join salaries s on e.emp_no = s.emp_no
      inner join dept_emp de on e.emp_no = de.emp_no
      inner join departments dp on de.dept_no = dp.dept_no
      inner join (select dp.dept_name dept_name, max(s.salary) as max_salary from employees e
      inner join dept_emp de on e.emp_no = de.emp_no
      inner join departments dp on de.dept_no = dp.dept_no
      inner join salaries s on e.emp_no = s.emp_no
      group by dp.dept_name) temp_table on s.salary = temp_table.max_salary and dp.dept_name = temp_table.dept_name;

      
