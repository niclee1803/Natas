# Natas 11
Source code:
```php
<html>
<head>
<!-- This stuff in the header has nothing to do with the level -->
<link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
<script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
<script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
<script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
<script>var wechallinfo = { "level": "natas11", "pass": "<censored>" };</script></head>
<?

$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}

$data = loadData($defaultdata);

if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}

saveData($data);



?>

<h1>natas11</h1>
<div id="content">
<body style="background: <?=$data['bgcolor']?>;">
Cookies are protected with XOR encryption<br/><br/>

<?
if($data["showpassword"] == "yes") {
    print "The password for natas12 is <censored><br>";
}

?>

<form>
Background color: <input name=bgcolor value="<?=$data['bgcolor']?>">
<input type=submit value="Set color">
</form>

<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
```



From the code, we can see that ```cookie -> base64 decode -> xor_encrypt -> json decode -> array data``` and the xor_encrypt key is censored.

First, we need to find the xor_encrypt key.   
We first take the cookie given to us from our developer tools nad we base64_decode it.   
We take the default array data given, ```$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");``` and we json_encode it.   
XOR the results together to get the key.   
   
Why does this work? According to the rules of XOR, it does!    
A XOR B = C    
A XOR C = B    

After we get the key, we can reverse engineer the desired array to get the cookie we want.    
Paste the cookie into developer tools and refresh the browser     
We can see that the app gives the password for next level only when it decodes an array with "showpassword" set to "yes"     

## Key takeaways
### XOR is reversible, it can be vulnerable if
* The plaintext is known
* The ciphertext is leaked
* The key is reused (as in this challenge)   
   
### Static and hardcoded keys are not safe
### Cookies can be tampered with if encryption is predictable and reversible


Full code to get the result:
```php
<?php
$cookie = "HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D";
$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

// $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);

// cookie -> base64 decode -> xor_encrypt -> json decode -> data

$encodeddata = json_encode($defaultdata);
$decodedcookie = base64_decode($cookie);


$key = $encodeddata ^ $decodedcookie;
//echo $key;

// key is eDWoe

function xor_encrypt($in) {
    $key = 'eDWo';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

$data = array( "showpassword"=>"yes", "bgcolor"=>"#ffffff");
$ans = base64_encode(xor_encrypt(json_encode($data)));
echo $ans;
```
