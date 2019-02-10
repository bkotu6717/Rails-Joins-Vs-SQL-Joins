## Rails-Joins-Vs-SQL-Joins
This repo gives understanding about writing SQL joins equivalent to Rails ActiveRecord Join Queries

### Joins introduction
  <p>
      SQL joins are nothing but mathematical set operations
      If we assume A and B are two sets

      A = {1,2,3,4,5}
      B = {6,7,8,9,10}
      A U B = {1,2,3,4,5,6,7,8,9,10}

      A Intersection B = {} # There are no common elements between A and B
      A-B, B-A are the remaining operations
  </p>

### Joins uses

<p>
  <ul>
   <li>Joins helps in RDBMS to combine and retrieve data from muliple tables which are connected through relationships.<l/i>
   <li>The queries are actually faster when we write in native sql language rather than with any wrapper language(eg: Rails ActiveRecord)</li>
   <li>We can perform join operations on more than one table always provided they are connected.</li>
  </ul>
</p>


### Types Of Joins

1. <a href="https://github.com/bkotu6717/Rails-Joins-Vs-SQL-Joins/blob/master/Inner%20Join">Inner Join</a>
2. Left Outer Join or Left Join
3. Right Outer Join or Right Join
4. Full Outer Join
5. Self Join
