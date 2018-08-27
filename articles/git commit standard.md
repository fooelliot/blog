# git 提交规范

### **前言**

无规矩不成方圆，编程也一样。

如果你有一个项目，从始至终都是自己写，那么你想怎么写都可以，没有人可以干预你。可是如果在团队协作中，大家都张扬个性，那么代码将会是一团糟，好好的项目就被糟践了。不管是开发还是日后维护，都将是灾难。

### **提交规则**

一个commit只做一件事情，若一个commit做了多件事情需要拆分成多个commit
严格遵循commit message格式
每次只允许提交一个commit，若本地有多个commit等待提交，必须等前面的commit合并进入主版本库并在本地合并完成后才可提交后面的commit

### **内容要求**

在commit log中，要写清楚以下几点内容：

这个commit解决了哪个问题，或者实现了哪个功能，需要有对应的bug url或者功能列表url；
如果是个bug fix，请详述之前存在什么问题，这个commit是如何解决的；
如果这个commit只是一个大的过程中的某一步，请注明，并且简要列出这个commit之后还需要做的事情；
如果有需要提醒审核者注意的问题，比如一个known bug，或者还有某些已知问题，或者需要未来重构，请用“NOTICE：”另起一行标注并说明。

### **提交格式**

Git Commit 规范可能并没有那么夸张，但如果你在版本回退的时候看到一大段糟心的 Commit，恐怕会懊恼不已吧。所以，严格遵守规范，利人利己，建议大家养成良好的开发习惯。
每次提交，Commit message 都包括三个部分：header，body 和 footer。

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

其中，header 是必需的，body 和 footer 可以省略。
不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。

**Header**

Header部分只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）。

**type**

用于说明 commit 的类别，只允许使用下面7个标识。

* feat：新功能（feature）
* fix：修补bug
* docs：文档（documentation）
* style： 格式（不影响代码运行的变动）
* refactor：重构（即不是新增功能，也不是修改bug的代码变动）
* test：增加测试
* chore：构建过程或辅助工具的变动

如果type为feat和fix，则该 commit 将肯定出现在 Change log 之中。其他情况（docs、chore、style、refactor、test）由你决定，要不要放入 Change log，建议是不要。

**scope**

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

如果你的修改影响了不止一个scope，你可以使用*代替。

**subject**

subject是 commit 目的的简短描述，不超过50个字符。

**其他注意事项：**

* 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
* 第一个字母小写
* 结尾不加句号（.）

**Body**

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent

**有两个注意点:**

* 使用第一人称现在时，比如使用change而不是changed或changes。
* 永远别忘了第2行是空行
* 应该说明代码变动的动机，以及与以前行为的对比。

**Footer**

Footer 部分只用于以下两种情况：

* 不兼容变动
* 如果当前代码与上一个版本不兼容，则 Footer 部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法。


create procedure product()
begin
 declare i int;
 set i=0;
 while i<5 do
  insert into t1(filed) values(i);
  set i=i+1;
 end while;
end;


delimiter $                                                           

create procedure hello(in n int)                               
begin                                                                 
declare num001 , num002 int;                                

set num001 = 1 , num002 = 10;                             

while n - num001 >= 0 do                                     
/*当num001小于等于n时执行下面的操作*/

insert into test(test_name,test_num)

values(concat("zhangsan" , num001) , num002);        
/*在test表中插入数据test_name=zhangsan+num001,test_num=num002，contat意为将张三和num001连起来*/

set num001=num001+1,num002=num002*2;             
/*设置num001=num001+1,num002=num002*2*/

end while;                                                          
/*当num>100时，不再执行插入操作循环结束*/

end $                                                                

delimiter ;  