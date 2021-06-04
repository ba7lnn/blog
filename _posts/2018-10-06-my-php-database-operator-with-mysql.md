---
layout: post
title: My php database operator with mysql
tags: php
---

<style>
	.highlight-left{margin: 0px;}
</style>


# this is is my use php operater data access code

{% highlight php %}

date_default_timezone_set('Asia/Shanghai');
define('DEBUG', $_SERVER['REMOTE_ADDR']=='127.0.0.1'?true:false);
define('DB_AUTO_FREE', true);
define('TABLE_PREFIX', 'my_');

// Mysql &  LOG_MSG
if (DEBUG) {	
	define('CONNECTION_STR', 'mysql://db_user:db_password@localhost:3306/db_development_name');
	define('LOG_MSG', 'msg.txt');
}else{
	define('CONNECTION_STR', 'mysql://db_user:db_password@localhost:3306/db_product_name');
}


function logger($msg) {
	if (defined('LOG_MSG') && LOG_MSG) {
		//if ($file=fopen(LOG_MSG, 'a')) fwrite($file, $msg."\n"); fclose($file);
		file_put_contents(LOG_MSG, $msg."\n", FILE_APPEND | LOCK_EX);
	}
}

$tables = array('attachs', 'metas', 'nodes', 'nodes_text', 'taged', 'tags');

foreach($tables as $name) {
	define("table_".$name, TABLE_PREFIX.$name);
	${"table_".$name} = TABLE_PREFIX.$name;
}

function db_connect() {
	static $connect_id;
	if (!is_resource($connect_id)) {
		if (!defined('CONNECTION_STR')) trigger_error('Connect() - CONNECTION_STR is undefined', E_USER_ERROR);
		$dsn = parse_url(CONNECTION_STR) or trigger_error("Connect() - could not parse connection string:".CONNECTION_STR, E_USER_ERROR);
		$connect_id = mysql_connect($dsn['host'].':'.$dsn['port'], $dsn['user'], $dsn['pass']) or trigger_error('Connection - could not connect to database Server:'.$dsn['host'], E_USER_ERROR);
		mysql_select_db(str_replace('/', '', $dsn['path']), $connect_id) or trigger_error('Connect() - could not select Database: '. $dsn['path'], E_USER_ERROR);
		mysql_query('SET NAMES UTF8', $connect_id);
	}
	return $connect_id;		
}

//$sql Model::Prepare("SELECT * FROM person WHERE last = '%s' AND age=%d", $_GET['lastname'], $_GET['age']);
// FindBySql('Person', $sql);
function db_prepare() {		
	$args =  func_get_args();
	$rawsql = array_shift($args);
	if (get_magic_quotes_gpc()) $args = array_map('stripslashes', $args);
	$sql = vsprintf($rawsql, $args);
	return $sql;
}

function db_query($sql) {
	//log query
	if (defined('LOG_SQL') && LOG_SQL) {
		if ($file=fopen(LOG_SQL, 'a')) fwrite($file, $sql."\n");
	}
	if ($result = mysql_query($sql, db_connect())) {
		return $result;
	} else {
		trigger_error('Query - query failed:'.$sql.' with error:'.mysql_error(db_connect()), E_USER_WARNING);
		return false;
	}
	
}

function db_row($sql, $limit=true){
	if($limit) $sql.=' LIMIT 1';
	$query_id = db_query($sql);
	//if(!$queryId) trigger_error('Query - query failed:'.$sql.' with error:'.mysql_error(Connect()), E_USER_ERROR);
	$rs = mysql_fetch_array($query_id, MYSQL_ASSOC);
	if(!$rs) return false;
	if(defined(DB_AUTO_FREE) && DB_AUTO_FREE) mysql_free_result($query_id);
	return (count($rs)==1)?current($rs):$rs;
}

function db_create($table, $values){
	foreach($values as $key=>$value){
		$keys[] = $key;
		$insertvalues[]='\''.$value.'\'';
	}
	$keys = implode(',',$keys);
	$insertvalues = implode(',',$insertvalues);
	$sql = "INSERT INTO $table ($keys) VALUES ($insertvalues);";	
	db_query($sql);
	return mysql_insert_id(db_connect());
}
function db_update($table, $update_values, $filter='') {
	foreach($update_values as $key=>$value)$sets[]=$key.'=\''.$value.'\'';
	$sets = implode(',',$sets);
	if ($filter) $where_condition = " WHERE ".$filter;
	$query = "UPDATE $table SET $sets $where_condition";
	db_query($query);
	return db_affected();
}

function db_remove($table, $filter){
	$sql = "DELETE FROM $table";
	if ($filter) $sql .= " WHERE $filter";
	return db_query($sql);
}

function db_fetch($sql, $begin_row=0, $limit=0){
	$data=array();
	if($begin_row) $begin_row=$begin_row . ',';
	if($limit) $sql .= ' LIMIT '.$begin_row.$limit;
	$query_id = db_query($sql);
	//if(!$queryId) trigger_error('Query - Invalid SQL:'.$sql.' with error:'.mysql_error(Connect()), E_USER_WARNING);
	while($row = mysql_fetch_array($query_id, MYSQL_ASSOC)){
		//$array[]=$thisRow;
		array_push($data, $row);
	}
	if(defined(DB_AUTO_FREE) && DB_AUTO_FREE) mysql_free_result($query_id);
	return $data;
}

function db_affected(){mysql_affected_rows(db_connect());}
function db_close() {mysql_close(db_connect());}	


{% endhighlight php %} {: .highlight-left }

# this code copy form osCommerce

{% highlight php %}
  function tep_db_connect($server = DB_SERVER, $username = DB_SERVER_USERNAME, $password = DB_SERVER_PASSWORD, $database = DB_DATABASE, $link = 'db_link') {
    global $$link;

    if (USE_PCONNECT == 'true') {
      $server = 'p:' . $server;
    }

    $$link = mysqli_connect($server, $username, $password, $database);

    if ( !mysqli_connect_errno() ) {
      mysqli_set_charset($$link, 'utf8');
    }

    @mysqli_query($$link, 'set session sql_mode=""');

    return $$link;
  }

  function tep_db_close($link = 'db_link') {
    global $$link;

    return mysqli_close($$link);
  }

  function tep_db_error($query, $errno, $error) {
    if (defined('STORE_DB_TRANSACTIONS') && (STORE_DB_TRANSACTIONS == 'true')) {
      error_log('ERROR: [' . $errno . '] ' . $error . "\n", 3, STORE_PAGE_PARSE_TIME_LOG);
    }

    die('<font color="#000000"><strong>' . $errno . ' - ' . $error . '<br /><br />' . $query . '<br /><br /><small><font color="#ff0000">[TEP STOP]</font></small><br /><br /></strong></font>');
  }

  function tep_db_query($query, $link = 'db_link') {
    global $$link;

    if (defined('STORE_DB_TRANSACTIONS') && (STORE_DB_TRANSACTIONS == 'true')) {
      error_log('QUERY: ' . $query . "\n", 3, STORE_PAGE_PARSE_TIME_LOG);
    }

    $result = mysqli_query($$link, $query) or tep_db_error($query, mysqli_errno($$link), mysqli_error($$link));

    return $result;
  }

  function tep_db_perform($table, $data, $action = 'insert', $parameters = '', $link = 'db_link') {
    reset($data);
    if ($action == 'insert') {
      $query = 'insert into ' . $table . ' (';
      while (list($columns, ) = each($data)) {
        $query .= $columns . ', ';
      }
      $query = substr($query, 0, -2) . ') values (';
      reset($data);
      while (list(, $value) = each($data)) {
        switch ((string)$value) {
          case 'now()':
            $query .= 'now(), ';
            break;
          case 'null':
            $query .= 'null, ';
            break;
          default:
            $query .= '\'' . tep_db_input($value) . '\', ';
            break;
        }
      }
      $query = substr($query, 0, -2) . ')';
    } elseif ($action == 'update') {
      $query = 'update ' . $table . ' set ';
      while (list($columns, $value) = each($data)) {
        switch ((string)$value) {
          case 'now()':
            $query .= $columns . ' = now(), ';
            break;
          case 'null':
            $query .= $columns .= ' = null, ';
            break;
          default:
            $query .= $columns . ' = \'' . tep_db_input($value) . '\', ';
            break;
        }
      }
      $query = substr($query, 0, -2) . ' where ' . $parameters;
    }

    return tep_db_query($query, $link);
  }

  function tep_db_fetch_array($db_query) {
    return mysqli_fetch_array($db_query, MYSQLI_ASSOC);
  }

  function tep_db_num_rows($db_query) {
    return mysqli_num_rows($db_query);
  }

  function tep_db_data_seek($db_query, $row_number) {
    return mysqli_data_seek($db_query, $row_number);
  }

  function tep_db_insert_id($link = 'db_link') {
    global $$link;

    return mysqli_insert_id($$link);
  }

  function tep_db_free_result($db_query) {
    return mysqli_free_result($db_query);
  }

  function tep_db_fetch_fields($db_query) {
    return mysqli_fetch_field($db_query);
  }

  function tep_db_output($string) {
    return htmlspecialchars($string);
  }

  function tep_db_input($string, $link = 'db_link') {
    global $$link;

    return mysqli_real_escape_string($$link, $string);
  }

  function tep_db_prepare_input($string) {
    if (is_string($string)) {
      return trim(tep_sanitize_string(stripslashes($string)));
    } elseif (is_array($string)) {
      reset($string);
      while (list($key, $value) = each($string)) {
        $string[$key] = tep_db_prepare_input($value);
      }
      return $string;
    } else {
      return $string;
    }
  }

  function tep_db_affected_rows($link = 'db_link') {
    global $$link;

    return mysqli_affected_rows($$link);
  }

  function tep_db_get_server_info($link = 'db_link') {
    global $$link;

    return mysqli_get_server_info($$link);
  }

  if ( !function_exists('mysqli_connect') ) {
    define('MYSQLI_ASSOC', MYSQL_ASSOC);

    function mysqli_connect($server, $username, $password, $database) {
      if ( substr($server, 0, 2) == 'p:' ) {
        $link = mysql_pconnect(substr($server, 2), $username, $password);
      } else {
        $link = mysql_connect($server, $username, $password);
      }

      if ( $link ) {
        mysql_select_db($database, $link);
      }

      return $link;
    }

    function mysqli_connect_errno($link = null) {
      if ( is_null($link) ) {
        return mysql_errno();
      }

      return mysql_errno($link);
    }

    function mysqli_connect_error($link = null) {
      if ( is_null($link) ) {
        return mysql_error();
      }

      return mysql_error($link);
    }

    function mysqli_set_charset($link, $charset) {
      if ( function_exists('mysql_set_charset') ) {
        return mysql_set_charset($charset, $link);
      }
    }

    function mysqli_close($link) {
      return mysql_close($link);
    }

    function mysqli_query($link, $query) {
      return mysql_query($query, $link);
    }

    function mysqli_errno($link = null) {
      if ( is_null($link) ) {
        return mysql_errno();
      }

      return mysql_errno($link);
    }

    function mysqli_error($link = null) {
      if ( is_null($link) ) {
        return mysql_error();
      }

      return mysql_error($link);
    }

    function mysqli_fetch_array($query, $type) {
      return mysql_fetch_array($query, $type);
    }

    function mysqli_num_rows($query) {
      return mysql_num_rows($query);
    }

    function mysqli_data_seek($query, $offset) {
      return mysql_data_seek($query, $offset);
    }

    function mysqli_insert_id($link) {
      return mysql_insert_id($link);
    }

    function mysqli_free_result($query) {
      return mysql_free_result($query);
    }

    function mysqli_fetch_field($query) {
      return mysql_fetch_field($query);
    }

    function mysqli_real_escape_string($link, $string) {
      if ( function_exists('mysql_real_escape_string') ) {
        return mysql_real_escape_string($string, $link);
      } elseif ( function_exists('mysql_escape_string') ) {
        return mysql_escape_string($string);
      }

      return addslashes($string);
    }

    function mysqli_affected_rows($link) {
      return mysql_affected_rows($link);
    }

    function mysqli_get_server_info($link) {
      return mysql_get_server_info($link);
    }
  }
  
  function tep_sanitize_string($string) {
    $patterns = array ('/ +/','/[<>]/');
    $replace = array (' ', '_');
    return preg_replace($patterns, $replace, trim($string));
  }

{% endhighlight %}  {: .highlight-left }