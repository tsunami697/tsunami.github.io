---
layout:     post   				    # 使用的布局（不需要改）
title:      Kernel: xxx_initcall	# 标题 
subtitle:   内核学习 				# 副标题
date:       2022-07-27			  	# 时间
author:     Qi						# 作者
header-img: img/post-bg-2015.jpg 	# 这篇文章标题背景图片
catalog: 	true 					# 是否归档
tags:								# 标签
    - kernel
    - gnu c
---

# xxx_initcall
> 内核使用gnu c编程， 所以很多不常见的语法都是gnu c的语法
## 一、宏定义

### 解析

```c
// kernel/include/linux/init.h
// 定义
	...
typedef int (*initcall_t)(void); 	//定义函数指针，常见
	...        
#define ___define_initcall(fn, id, __sec) \	//这行不多说，常见
        
     /* 1. static initcall_t: 声明一个函数指针变量:变量叫做__initcall_##fn##id; 
        2. 特殊符号：“##”：
        	“##”和“#”是宏定义中专门使用的，“##”意思是连接前后两个内容，拼接作用。“#”意思是把后面内容当做字符串
        	举个例子：__define_initcall(func, 4, sec)展开后为：
        	static initcall_t __initcall_func4 ...;
        3. gun c使用__used、__attribute__设置变量的属性，在kernel/include/linux/compiler_types.h(内核头文件)。
        	used: 其作用是告诉编译器避免被链接器因为未用过而被优化掉。
        	attribute((section(“name”)))：将作用的函数或数据放入指定名为"section_name"对应的段中 
      */
     static initcall_t __initcall_##fn##id __used \	
         __attribute__((__section__(#__sec ".init"))) = fn;    //有个空格，从lds文件看 会被忽略
	...
#define __define_initcall(fn, id) ___define_initcall(fn, id, .initcall##id) 
	...
#define subsys_initcall(fn)     __define_initcall(fn, 4)

// kernel/drimver/input/input.c
subsys_initcall(input_init);
展开后：
static initcall_t __initcall_input_init4 __uesd __attribute__((__section__(.initcall4.init))) = input_init;
该句作用是：声明一个函数指针变量并初始化为input_init
    ...
```

### 使用



###  section编译

## 二、lds、map

kernel/System.map

kernel/arch/arm64/kernel/vmlinux.lds
