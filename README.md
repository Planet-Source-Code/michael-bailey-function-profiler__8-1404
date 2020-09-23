<div align="center">

## Function Profiler


</div>

### Description

If your scripts are running too slow, it may be that you have a function that's taking too long to run or that gets called too often. This function profiler watches the all of the function calls and records how much time is spent in each function. All of the documentation is included at the top of the source code.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Michael Bailey](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/michael-bailey.md)
**Level**          |Intermediate
**User Rating**    |5.0 (10 globes from 2 users)
**Compatibility**  |PHP 4\.0
**Category**       |[Debugging and Error Handling](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/debugging-and-error-handling__8-6.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/michael-bailey-function-profiler__8-1404/archive/master.zip)





### Source Code

```
<?
/**
* The best way to use this script is to place
* it in a separate file, say: watcher.php.
* At the top of the file you want to profile,
* include watcher.php and follow the include
* with the statement:
* declare(ticks=1);
*
* At the end of the script, call watch_function_output()
* to see the results. The value returned in
* the output for each function call is the
* number of system ticks spent in each given
* function/method. These ticks relate to
* the number of lower-level operations the
* PHP engine must perform. For more information,
* check the php.net documentation under
* declare() or contact me:
*  Michael Bailey "jinxidoru@byu.net"
*
* You are welcome to use this code to any end,
* be it for good or evil. Though, if it be for
* evil, I'd be interested in knowing what you're
* doing for the sake of curiousity.
*/
// tick function for watching functions
// if $ret_funcs is true, a tick is not registered,
// but instead, the function list is returned.
function watch_functions_tick($ret_funcs=false) {
 static $funcs = array();
 // do tick
 if (!$ret_funcs) {
  // get the current function from debug_backtrace
  $trace = debug_backtrace();
  $trace = $trace[1];
  $func = $trace['function'];
  if ($trace['class']) $func = $trace['class'].$trace['type'].$func;
  // as long as the given function is a valid one, add it to the array
  if ($func !== 'watch_functions_tick' && $func != 'unknown' && $func) {
   $funcs[$func]++;
   $funcs['__TICK_COUNT__']++;
  }
 // return the function array
 } else {
  return $funcs;
 }
}
// output the function list with the number of ticks each
// function received as well as the percentage of the total
// ticks it represents
function watch_functions_output() {
 // retrieve the function list
 $funcs = watch_functions_tick(true);
 $tick_count = $funcs['__TICK_COUNT__'];
 arsort($funcs);
 // add the percentage onto each function entry
 foreach ($funcs AS $k => $v) {
  $funcs[$k] = sprintf('%d (%.2f%%)', $funcs[$k], $funcs[$k]*100/$tick_count);
 }
 // display the function list
 print "<pre>";
 print_r($funcs);
 print "</pre>";
}
// start the ticking
register_tick_function('watch_functions_tick');
?>
```

