# codewars问题解答
## 目录
1. [字符串合并并按英文字母排序](#jump)
2. [秒转化时间格式](#jump2)
3. [取负数](#jump3)
4. [字符串转化如：abc => A-Bb-Ccc](#jump4)

---
#### <span id = "jump">字符串合并并按英文字母排序</span>
##### 描述
2个字符串拼接、去重并按英文字母排序。s1="aedsf"  s2="edcsf"  返回出 str="acdefs" 。
##### 代码方法
```
个人方法：
function longest(s1, s2) {  
    var str = s1+s2;  
    var str2="";  
    for(var i=0;i<str.length;i++){  
        if(str2.search(str[i])==-1)
            str2+=str[i];
    }     
    var newStr = str2.split("").sort().join("");  
    return newStr;  
}
其他优秀方法：
function longest(s1, s2) {
  return Array.from(new Set(s1 + s2)).sort().join('');
}
方法：
function longest(s1, s2) {
  // your code
  s3 = s1 + s2;
  s4 = s3.split("");
  s4 = s4.sort().filter(function(element, index, array){
    return element !== array[index - 1];
  });
  return s4.join("");
}
```

##### 知识点
search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。
```
<script type="text/javascript">
var str="Visit W3School!"
document.write(str.search(/W3School/))
</script>

输出：6
如果输出：-1就表示没有  例子中就是使用这里-1的时候把字符串收集起来
```
split() 方法用于把一个字符串分割成字符串数组。
```
var str="How are you doing today?"

document.write(str.split(" ") + "<br />")
document.write(str.split("") + "<br />")
document.write(str.split(" ",3))

输出：
How,are,you,doing,today?
H,o,w, ,a,r,e, ,y,o,u, ,d,o,i,n,g, ,t,o,d,a,y,?
How,are,you
index:表示输出的个数
```

sort() 方法用于对数组的元素进行排序。
如果调用该方法时没有使用参数，将按字母顺序对数组中的元素进行排序，
```
var arr = new Array(6)
arr[0] = "George"
arr[1] = "John"
arr[2] = "Thomas"
arr[3] = "James"
arr[4] = "Adrew"
arr[5] = "Martin"
document.write(arr + "<br />")
document.write(arr.sort())

输出：
George,John,Thomas,James,Adrew,Martin
Adrew,George,James,John,Martin,Thomas
```

join() 方法用于把数组中的所有元素放入一个字符串。
元素是通过指定的分隔符进行分隔的。
```
var arr = new Array(3)
arr[0] = "George"
arr[1] = "John"
arr[2] = "Thomas"

document.write(arr.join("."))

输出：
George.John.Thomas
```


---
#### <span id = "jump2">秒转化时间格式</span>
##### 描述
给一个参数秒数，转化为   
HH = 时间，00-99,参数秒已经限制，不允许出现不允许出现100  
HH = 时间，00-59  
HH = 时间，00-99  
##### 代码方法
```
function humanReadable(seconds) {
  var pad = function(x) { return (x < 10) ? "0"+x : x; }
  return pad(parseInt(seconds / (60*60))) + ":" +
         pad(parseInt(seconds / 60 % 60)) + ":" +
         pad(seconds % 60)
}
alert(humanReadable(8263));
```

##### 知识点
parseInt() 函数可解析一个字符串，并返回一个整数。
```
parseInt("10");			//返回 10
parseInt("19",10);		//返回 19 (10+9)
parseInt("11",2);		//返回 3 (2+1)
parseInt("17",8);		//返回 15 (8+7)
parseInt("1f",16);		//返回 31 (16+15)
parseInt("010");		//未定：返回 10 或 8
```

**%** 取模（余数）
```
5%2;			//返回 1
```

---
#### <span id = "jump3">取负数</span>
##### 描述
给个数字返回负数
##### 代码方法
```
function opposite(number) {
  return(-number);
}
}
```

---
#### <span id = "jump4"> 字符串转化如：abc => A-Bb-Ccc </span>
##### 描述
字符串转化如：abc => A-Bb-Ccc .第一个字母大写
##### 代码方法
```
function accum(s) {
  return s.split('').map((c, i) => (c.toUpperCase() + c.toLowerCase().repeat(i))).join('-');
}
```

##### 知识点
toUpperCase(),toLowerCase() 大小写。
```
'aBc'.toUpperCase();			//返回 ABC
'aBc'.toLowerCase();		//返回 abc
```

repeat() 返回一个新的字符串对象，它的值等于重复了指定次数的原始字符串。
```
"abc".repeat(3); // Returns "abcabcabc"
"abc".repeat(0); // Returns an empty string.
```
