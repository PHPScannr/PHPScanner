# PHPScanner
PHPScanner is a small and simple script which scans uploaded content for known scripts like c99 before your PHP script handles the request

## Usage
This script is intended to be included at your php.ini level, example:
```
auto_prepend_file =/etc/php5/extensions/PHPScanner/scanner.php
```

The benefit of this, is you do not need to remember to include it for each project, as long as that project is running on the same web server this script will be included.

You can alternatively include this manually in your PHP script
```
<?php
require( 'path/to/PHPScanner/scanner.php' );
```

## Example Output
There is no need to change how your uploads are handled (not even for WordPress, Joomla or other Content Management Systems).


### Example 1 - Clean file
Here's an example output for `var_dump( $_FILES );`

```
array(1) {
  ["my_file"]=>
  array(7) {
    ["name"]=>
    string(11) "Capture.JPG"
    ["type"]=>
    string(10) "image/jpeg"
    ["tmp_name"]=>
    string(14) "/tmp/phpJ1kFGr"
    ["error"]=>
    int(0)
    ["size"]=>
    int(127218)
    ["scan_results"]=>
    string(2) "OK"
    ["scan_details"]=>
    array(0) {
    }
  }
}
```

As you can see, there are two additional keys `scan_results` and `scan_details`.

1. `scan_results` is a string, with two values available

   **OK** - Scan completed and the file was considered clean
   
   **PUP** - Scan completed and the file was considered a Potentially Unwanted Program
   
2. `scan_details` is an array, if the file was considered a PUP, this array is filled with the strings matched
   
   It is then down to the Developer to programmatically decide if the file is indeed dangerous.
   

### Example 2 - RootKit file
Here's an example output for `var_dump( $_FILES );`

```
array(1) {
  ["my_file"]=>
  array(7) {
    ["name"]=>
    string(8) "test.txt"
    ["type"]=>
    string(10) "text/plain"
    ["tmp_name"]=>
    string(26) "/tmp/phpDrv7Jh_VIRUS_FOUND"
    ["error"]=>
    int(8)
    ["size"]=>
    int(165477)
    ["scan_results"]=>
    string(3) "PUP"
    ["scan_details"]=>
    array(7) {
      [0]=>
      array(2) {
        ["vun_id"]=>
        int(0)
        ["vun_string"]=>
        string(23) "-type f -name .htpasswd"
      }
      [1]=>
      array(2) {
        ["vun_id"]=>
        int(2)
        ["vun_string"]=>
        string(7) "netstat"
      }
      [2]=>
      array(2) {
        ["vun_id"]=>
        int(4)
        ["vun_string"]=>
        string(6) "find /"
      }
      [3]=>
      array(2) {
        ["vun_id"]=>
        int(5)
        ["vun_string"]=>
        string(9) "ccteam.ru"
      }
      [4]=>
      array(2) {
        ["vun_id"]=>
        int(10)
        ["vun_string"]=>
        string(6) "psyBNC"
      }
      [5]=>
      array(2) {
        ["vun_id"]=>
        int(13)
        ["vun_string"]=>
        string(11) "/etc/passwd"
      }
      [6]=>
      array(2) {
        ["vun_id"]=>
        int(14)
        ["vun_string"]=>
        string(23) "packetstormsecurity.org"
      }
    }
  }
}
```