# test_php_10_28
## Question 1
  A client has called and said that they're noticing performance problems on their database when searching for a user by email address. You've checked, and the following query is running:
  ```
  SELECT * FROM users WHERE email = 'user@test.com';
  ```
  You run the EXPLAIN command and get the following results:
  ```
  +----+-------------+-------+------+---------------+------+---------+------+-------+-------------+
  | id | select_type | table | type | possible_keys | key  | key_len | ref  | rows  | Extra       |
  +----+-------------+-------+------+---------------+------+---------+------+-------+-------------+
  |  1 | SIMPLE      | users | ALL  | NULL          | NULL | NULL    | NULL | 10320 | Using where |
  +----+-------------+-------+------+---------------+------+---------+------+-------+-------------+
  ```
  Offer a theory as to why the performance is slow.

 <p>Answer:</p>
 <p>There is no key.</p>
  
## Question 2
You're given a sorted index array that contains no keys. The array contains only integers, and your task is to identify whether or not the integer you're looking for is in the array. Come up with a function that searches for the integer and returns true or false based on whether the integer is present. Describe how you arrived at your solution.

Answer:
```
  <?php
      function searchInArray($arrayList, $findInt){
          in_array($findInt, $arrayList, true);
      }

      $givenArray = [3, 5, 8, 11, 12, 50, 100, 111, 123];

      var_dump(searchInArray($givenArray, 11));
  ?>
 ```
## Question 3
     During a large data migration, you get the following error: Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate 54 bytes). </p>
     You've traced the problem to the following snippet of code:
    ```
    $stmt = $pdo->prepare('SELECT * FROM largeTable');
    $stmt->execute();
    $results = $stmt->fetchAll(PDO::FETCH_ASSOC);
    foreach ($results as $result) {
      // manipulate the data here
    }
    ```
    Refactor this code so that it stops triggering a memory error.
    
 Answer:
  ```
  $pdo->setAttribute(PDO::MYSQL_ATTR_USE_BUFFERED_QUERY, false);  //PDO::MYSQL_ATTR_USE_BUFFERED_QUERY(available in MySQL):Use buffered queries
  $stmt = $pdo->prepare('SELECT * FROM largeTable');
  $stmt->execute();
  $results = $stmt->fetchAll(PDO::FETCH_ASSOC);
  ```
## Question 4
  Write a function that takes a phone number in any form and formats it using a delimiter supplied by the developer. The delimiter is optional; if one is not supplied, use a dash (-). Your function should accept a phone number in any format (e.g. 123-456-7890, (123) 456-7890, 1234567890, etc) and format it according to the 3-3-4 US block standard, using the delimiter specified. Assume foreign phone numbers and country codes are out of scope.

  Note: This question CAN be solved using a regular expression, but one is not REQUIRED as a solution. Focus instead on cleanliness and effectiveness of the code, and take into account phone numbers that may not pass a sanity check.

Answer:
  ```
  <?php
    function getFilteredPNumber($pStr){
        $count = strlen($pStr);
        $filteredNumber = '';

        for ($i = 0; $i < $count; $i++) {
            $specialChar = substr($pStr, $i, 1);
            if (is_numeric($specialChar)) {
                $filteredNumber .= $specialChar;
            }
        }

        return $filteredNumber;
    }

    function reformatPNumber($pNum, $delimiter){
        $firstPart = substr($pNum, 0, 3);
        $secondPart = substr($pNum,3, 3);
        $thirdPart = substr($pNum, 6);

        return $firstPart . $delimiter . $secondPart . $delimiter . $thirdPart;
    }
    
    function transformPNum($phoneNum, $delimiter){
        if(empty($phoneNum)){
            return false;
        }

        $filteredPhoneNum = getFilteredPNumber($phoneNum);

        if(empty($filteredPhoneNum)){
            return false;
        }

        if( strlen($filteredPhoneNum) != 10 ){
            return false;
        }

        return reformatPNumber($filteredPhoneNum, $delimiter);
    }

    

    $phoneStr = '(111) 222-3333';
    $delimiter = '-';
    echo transformPNum($phoneStr, $delimiter);
  ?>
  ```
  
## Question 5
In production, we'll be caching to memcache. On staging, we'll be caching to APC. In development, we won't be caching at all. Design a library that allows you to store and retrieve data from the cache (only two methods required) and fits the requirements of all three environments. Consider making use of anything appropriate (e.g. traits, classes, interfaces, abstract classes, closures, etc) to solve this problem.

Note: This is an architecture question. Please focus on the design of your library, rather than implementation or the specific caches I've described.

<p>Answer:</p>

  ````
  <?php

    interface iCaching {
        public function setCache($data);
        public function getCache();
    }

    /**
     * Class exMemCache
     */
    class exMemCache implements iCaching {

        private $exCache;

        public function __construct() {
            // create instance for Memcached
            $this->exCache = new Memcached();
        }

        public function setCache($data) {
            // do something to store into exMemCache

        }

        public function getCache() {
            // do something to retrieve from exMemCache

        }
    }

    /**
     * Class exAPCCache
     */
    class exAPCCache implements iCaching {
        public function __construct() {

        }

        public function setCache($data) {
            // do something to store into APCCache

        }

        public function getCache() {
            // do something to retrieve from APCCache

        }
    }

    /**
     * Class exNoneCache
     */
    class exNoneCache implements iCaching {

        private $exCache;

        public function __construct() {

        }

        public function setCache($data) {
            return true;
        }

        public function getCache() {
            return true;
        }
    }

    /**
     * Class exCache
     */
    class exCache {

        private $cache;

        public function __construct($env) {
            if ($env == 'PRODUCT') {
                $this->cache = new exMemCache();
            } else if ($env == 'STAGING') {
                $this->cache = new exAPCCache();
            } else {
                $this->cache = new exNoneCache();
            }

        }

        public function setCache($data) {
            $this->cache->setCache($data);
        }

        public function getCache() {
            return $this->cache->getCache();
        }
    }

    /**
     * Example
     */
    $cache = new exCache('PRODUCT');
    $cache->setCache($data);
    $data = $cache->getCache();
  ?>
  ````
  
##  Question 6
  Write a complete set of unit tests for the following code:
  ```
  function fizzBuzz($start = 1, $stop = 100)
  {
	$string = '';
	
	if($stop < $start || $start < 0 || $stop < 0) {
		throw new InvalidArgumentException;
	}
	
	for($i = $start; $i <= $stop; $i++) {
		if($i % 3 == 0 && $i % 5 == 0) {
			$string .= 'FizzBuzz';
			continue;
		}
		
		if($i % 3 == 0) {
			$string .= 'Fizz';
			continue;
		}
		
		if ($i % 5 == 0) {
			$string .= 'Buzz';
			continue;
		}
		
		$string .= $i;
	}
	
	return $string;
}
  ```
  
  Answer:
  ````
  <?php
	  require './fizzBuzz.php';
	  class Test extends \PHPUnit_Framework_TestCase
	  {
	    public function testFizzBuzz()
	    {
		$this->assertEquals('Fizz', fizzBuzz(3, 3), 'Multiples of 3 should return Fizz');
		$this->assertEquals('Buzz', fizzBuzz(5, 5), 'Multiples of 5 should return Buzz');
		$this->assertEquals('FizzBuzz', fizzBuzz(15, 15), 'Multiples of 3 and 5 should return FizzBuzz');
		$this->assertEquals('2', fizzBuzz(2, 2), 'Non-multiples of 3 or 5 should return input');
	    }
	  }
  ?>
  ````
