在开始解剖ES6之前，我们首先需要知道：

任何一次版本的迭代，必然是弥补以前的不足，或者为了更好发展的铺垫。而对于ES6的第一位成员for-of来说，是为了弥补什么，又或者在铺垫什么呢？

首先我们来看以下代码:

需求：我们需要比较当前循环的item与下一项item的大小
```
var arr1 = [1,2,3,4,5];
for(var index in arr1){
        console.info(arr1[index] < arr1[index+1]);
        if(index === 3){
                break;
        }
}
```
对于这段最简单的js代码，我们先来目测一下，比较结果貌似肯定是后面的值大于前面，输出结果自然也就是```true```了，所以，理论上的输出结果是这样的：
```
true
true
true
true
```
然而，实际上却是这样的：
```
false
false
false
false
false
```
什么鬼？
我们可以很明显的看到，这儿不止是输出结果是```false```，而且很明显，```break```也没有起到作用，竟然输出了5次，问题出在什么地方？

为了找出问题，我们不得不在循环内添加这个代码来判断```arr1[index+1]```究竟是什么：
```
console.info("index:"+arr1[index] +" -- index2"+ arr1[index+1]);
```
输出结果是这样的：
```
index:1 -- index2：undefined
index:2 -- index2：undefined
index:3 -- index2：undefined
index:4 -- index2：undefined
index:5 -- index2：undefined
```
What? ```arr1[index+1]```是```undefined```? 1+1=undefined?
显然不可能，那么问题肯定是出在了```index+1```上了，接下来我们看看这个```index+1```究竟是个什么玩意：
添加以下代码：
```
console.info(index+1);
```
结果如下：
```
01
11
21
31
41
```
很显然，和我们预计中的结果不一样，而且非常明显，```index+1```并没有进行加法运算，而是进行了字符串拼接？难道```index```是```string```类型？
别着急，我们来验证一下，添加如下代码：
```
console.info(typeof(index));
```
结果如下：
```
string
string
string
string
string
```
这个时候，答案已经呼之欲出了，没错的，```index```的值是```string```类型的，哦买噶的...

>没错，使用```for-in```进行迭代的时候，我们得到的索引值并非是```int```类型，而是```string```类型

>所以，使用```for-in```的小伙伴们以后可要注意喽，别掉进这个坑里面了。

>至于如何解决这个问题，我们会在后面进行讲解。

那么为什么循环了5次，而不是4次呢？这是因为在```===```这样严格相等的模式下，js解析器是会判断变量类型的，当然，如果使用```==```这样的非严格模式，就不会出现这样的问题了，但是并不建议这样使用。
