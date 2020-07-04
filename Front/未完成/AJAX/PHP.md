定义

```php
$num = 10
```

输出

```php
echo $num
```

数组

```php
$arr = array(1,3,5);
print_r($arr); #Array ( [0] => 1 [1] => 3 [2] => 5 )
echo $arr[0]; #1
```

对象

```php
$dict = array("name"=>"Inj","age"=>"33");
print_r($dict);  //Array ( [name] => Inj [age] => 33 )
echo $dict["name"]; //Inj
```

条件语句

```php
 $age = 18;
 if($age>18){
     echo "成年人";
 }else{
     echo "未成年人";
 }
 $res = $age>18?"成年人":"未成年人";
 echo $res;
```

选择语句

```php
$age = 18;
switch($age){
    case 0:
        echo "0";
    break;
    case 18:
        echo "成年人";
    break;
    default:
    echo "未成年人";
break;
```

循环语句

```php
$arr = array(1,3,5);
for($i=0;$i<count($arr);$i++){
    echo $arr[$i];
    echo "<br/>";
}

$index=0;
while($index<count($arr)){
    echo $arr[$index];
    echo "<br/>";
    $index++;
}
```

