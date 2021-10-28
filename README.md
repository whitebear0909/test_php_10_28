# test_php_10_28
## Question 1
  A client has called and said that they're noticing performance problems on their database when searching for a user by email address. You've checked, and the following query is running:

  SELECT * FROM users WHERE email = 'user@test.com';
  You run the EXPLAIN command and get the following results:

  +----+-------------+-------+------+---------------+------+---------+------+-------+-------------+
  | id | select_type | table | type | possible_keys | key  | key_len | ref  | rows  | Extra       |
  +----+-------------+-------+------+---------------+------+---------+------+-------+-------------+
  |  1 | SIMPLE      | users | ALL  | NULL          | NULL | NULL    | NULL | 10320 | Using where |
  +----+-------------+-------+------+---------------+------+---------+------+-------+-------------+
  Offer a theory as to why the performance is slow.

Answer:
  There is no key.
  
## Question 2
You're given a sorted index array that contains no keys. The array contains only integers, and your task is to identify whether or not the integer you're looking for is in the array. Come up with a function that searches for the integer and returns true or false based on whether the integer is present. Describe how you arrived at your solution.

Answer:
  <?php
      function searchInArray($arrayList, $findInt){
          in_array($findInt, $arrayList, true);
      }

      $givenArray = [3, 5, 8, 11, 12, 50, 100, 111, 123];

      var_dump(searchInArray($givenArray, 11));
  ?>
## Question 3
    During a large data migration, you get the following error: Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate 54 bytes). You've traced the
     problem to the following snippet of code:

    $stmt = $pdo->prepare('SELECT * FROM largeTable');
    $stmt->execute();
    $results = $stmt->fetchAll(PDO::FETCH_ASSOC);
    foreach ($results as $result) {
      // manipulate the data here
    }
    Refactor this code so that it stops triggering a memory error.
    
 Answer:
  $pdo->setAttribute(PDO::MYSQL_ATTR_USE_BUFFERED_QUERY, false);  //PDO::MYSQL_ATTR_USE_BUFFERED_QUERY(available in MySQL):Use buffered queries
  $stmt = $pdo->prepare('SELECT * FROM largeTable');
  $stmt->execute();
  $results = $stmt->fetchAll(PDO::FETCH_ASSOC);
  ....
## Question 4
  Write a function that takes a phone number in any form and formats it using a delimiter supplied by the developer. The delimiter is optional; if one is not supplied, use a dash (-). Your function should accept a phone number in any format (e.g. 123-456-7890, (123) 456-7890, 1234567890, etc) and format it according to the 3-3-4 US block standard, using the delimiter specified. Assume foreign phone numbers and country codes are out of scope.

  Note: This question CAN be solved using a regular expression, but one is not REQUIRED as a solution. Focus instead on cleanliness and effectiveness of the code, and take into account phone numbers that may not pass a sanity check.

Answer:
  
