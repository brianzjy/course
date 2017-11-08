# codewars问题解答
## 目录
1. [字符串合并并按英文字母排序](#jump)

---
#### <span id = "jump">字符串合并并按英文字母排序</span>
##### 描述
2个字符串拼接、去重并按英文字母排序。s1="aedsf"  s2="edcsf"  返回出 str="acdefs" 。
##### 代码方法
```
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
