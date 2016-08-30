More Joel on Software
===================


[toc]


#[关于Joel文章的网站](http://www.joelonsoftware.com/)

#乔尔测试
---------


##乔尔测试内容包括

> 1. 你们使用版本控制系统吗?
> 2. 有一键部署吗?
> 3. 有每日构建吗?
> 4. 有Bug数据库吗?
> 5. 你们是否在写新代码前保证Bug都修复完了? ([无限缺陷方法论](#无限缺陷方法论))
> 6. 你们的进度表是最新的吗? ([我需要进一步读这个](#无痛软件时间表))
> 7. 有需求文档吗?
> 8. 程序员们有安静的工作环境吗?
> 9. 你们使用能买到的最好的工具吗?
> 10. 有测试团队吗?
> 11. 应聘者在面试时写代码吗?
> 12. 你们进行目标用户随机可用性测试吗([走廊可用性测试](#走廊可用性测试))?

[**我们是如何做的**](#我们是如何做的)


##无限缺陷方法论
>在Bug被修正之前等待的时间越长，以后要修正时的代价越大


##[无痛软件时间表](http://www.joelonsoftware.com/articles/fog0000000245.html)
(Painless Software Schedules)

##走廊可用性测试
在走廊随便拉一个人使用写好的程序, 重复进行可暴露大量问题


##我们是如何做的
1. 你们使用版本控制系统吗?
使用perforce, git做为版本管理工具

2. 有一键部署吗?
使用PBS, RBS平台, gradle, shell脚本控制版本同步, 编译和打包

3. 有每日构建吗?
不同项目构建周期不同, spay项目为一周两次

4. 有Bug数据库吗?
PLM系统, JIRA系统

5. 你们是否在写新代码前保证Bug都修复完了?
- bug修改与新工能开发同时进行.
- bug修改由QA及VOC驱动
- QA包括自测, SEL, CMT

6. 你们的进度表是最新的吗?
开发计划由developer给出建议, 由PM与BD协调后制定

7. 有需求文档吗?
需求文档，UX及GUI设计文档由PM提供

8. 程序员们有安静的工作环境吗?
没有, 会被不同的业务打断

9. 你们使用能买到的最好的工具吗?
是的, 性能足够的PC，多显示器, 市场版本开发样机, 正版软件

10. 有测试团队吗?
- spay项目组一般分配2-3名SEL测试人员做专题测试
- 项目release会分配2-3名SEL测试人员做版本测试
- 平均每developer配有0.13名测试人员

11. 应聘者在面试时写代码吗?
不写
参考: [The Guerrilla Guide to Interviewing <<面试简易指南>>](http://www.joelonsoftware.com/articles/fog0000000073.html)

12. 你们进行目标用户随机可用性测试吗?
spay项目是这样


##[这篇文章的原地址](http://www.joelonsoftware.com/articles/fog0000000043.html)




#语言选择和代码优化
-------------------

##施勒梅尔算法
~~Schlaemmer?~~

```c++
void strcat(char* dest, char* src) {
   while (*dest) dest++;
   while (*dest++ = *src++)
}
```

每次都是重头开始
**代码的效率**


##有关字符串的问题

###Pascal字符串
>- 字符串长度在255以内
>- 第一个字节存储字符串长度

e.g.
```c++
char* str = "\006hello!";
```


###String, StringBuffer和StringBuilder
1. String 内容不可变. 每次变更是生成了新的String. 大量String直接操作后会产生多个无用引用而
引起jvm开始gc, 速度会变慢.
2. StringBuffer**线程安全**, 维护缓冲区, 支持insert, append等操作, 对内容进行修改但不产生新对象.
3. StringBuilder**线程不安全**, 提供与StringBuffer兼容的API, 但不保证同步. 单个线程中比StringBuffer快