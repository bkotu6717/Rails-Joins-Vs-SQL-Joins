# Rails-Joins-Vs-SQL-Joins
<p>This repo gives understanding about writing SQL joins equivalent to Rails ActiveRecord Join Queries</p>

  ![SQL Joins](http://www.postgresqltutorial.com/wp-content/uploads/2018/12/PostgreSQL-Joins.png)


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


## Get employees who are getting paid high in each department

      select e.emp_no, e.first_name||' '||e.last_name as emp_name, dp.dept_name, s.salary as max_salary from employees e
      inner join salaries s on e.emp_no = s.emp_no
      inner join dept_emp de on e.emp_no = de.emp_no
      inner join departments dp on de.dept_no = dp.dept_no
      inner join (select dp.dept_name dept_name, max(s.salary) as max_salary from employees e
      inner join dept_emp de on e.emp_no = de.emp_no
      inner join departments dp on de.dept_no = dp.dept_no
      inner join salaries s on e.emp_no = s.emp_no
      group by dp.dept_name) temp_table on s.salary = temp_table.max_salary and dp.dept_name = temp_table.dept_name;

      
## All types of joins
      -- Select all rows from table A
      select * from basket_a;
       id |  fruit   
      ----+----------
        1 | Apple
        2 | Orange
        3 | Banana
        4 | Cucumber
      (4 rows)
      -- Select all rows from table B
      select * from basket_b;
       id |   fruit    
      ----+------------
        1 | Orange
        2 | Apple
        3 | Watermelon
        4 | Pear
      (4 rows)
      -- Inner Join

### -- Inner Join
      -- Select rows common to both tables (A & B)
      select * from basket_a ba inner join basket_b bb on bb.fruit = ba.fruit;

      id | fruit  | id | fruit  
      ----+--------+----+--------
        1 | Apple  |  2 | Apple
        2 | Orange |  1 | Orange
      (2 rows)
### -- Left join or Left outer join
      -- First select rows common to both A & B
      -- 2nd select rows only belongs to A
      -- Non Matching rows will be shown with values null on table B data
      
      select * from basket_a ba left join basket_b bb on bb.fruit = ba.fruit;
      
      id |  fruit   |   id     | fruit  
      ----+----------+----+-------------
        1 | Apple    |    2     | Apple
        2 | Orange   |    1     | Orange
        3 | Banana   |  [null]  | [null]
        4 | Cucumber |  [null]  | [null]
      (4 rows)
### -- Left join or Left outer join
    -- Select rows unique to A i.e exclude common rows between A and B
    
       select * from basket_a ba left join basket_b bb on bb.fruit = ba.fruit where bb.fruit IS NULL;
       
       id |  fruit   | id       | fruit 
      ----+----------+----------+-------
        3 | Banana   |  [null]  |  [null]
        4 | Cucumber |  [null]  |  [null]
      
### -- Right join or Right outer join
      
      -- Right Join
      -- First select rows common to both A & B
      -- 2nd select rows only belongs to B
      select * from basket_a ba right join basket_b bb on bb.fruit = ba.fruit;

       id | fruit  | id |   fruit    
      -------+--------+----+------------
        2    | Orange |  1 | Orange
        1    | Apple  |  2 | Apple
      [null] | [null] |  3 | Watermelon
      [null] | [null] |  4 | Pear
      (4 rows)
### - Right join or Right Outer join
      -- Right Join
      -- Select rows uniq to B
      
      select * from basket_a ba right join basket_b bb on bb.fruit = ba.fruit where ba.fruit is NULL;

        id       | fruit       | id |   fruit    
       ----------+-------------+----+------------
       [null]    | [null]      |  3 | Watermelon
       [null]    | [null]      |  4 | Pear
       (2 rows)
### - Full outer join
      -- Full join
      -- Common to both A & B
      -- Uniq to A
      -- Uniq to B
      select * from basket_a ba full join basket_b bb on bb.fruit = ba.fruit;
       id     |  fruit   | id       |   fruit    
      --------+----------+----------+---------------
        1     | Apple    |  2       | Apple
        2     | Orange   |  1       | Orange
        3     | Banana   |  [null]  | 
        4     | Cucumber |  [null]  | 
       [null] | [null]   |  3       | Watermelon
       [null] | [null]   |  4       | Pear
      (6 rows)

## - Full join
      -- Uniq to A & B Using Full Join
      select * from basket_a ba full join basket_b bb on bb.fruit = ba.fruit where ba.fruit IS NULL or bb.fruit is NULL;
       id       |  fruit   | id       |   fruit    
      ----------+----------+----------+------------
        3       | Banana   | [null]   |  [null]
        4       | Cucumber | [null]   |  [null]
        [null]  | [null]   |  3       | Watermelon
        [null]  | [null]   |  4       | Pear
      (4 rows)

      

