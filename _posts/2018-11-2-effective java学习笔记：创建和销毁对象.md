---
title: effective java学习笔记：创建和销毁对象
key: 20181031
tags: Java
---

# 创建和销毁对象

## 考虑用静态工厂方法代替构造器

>静态工厂方法
>
>```java
>public static Boolean valueOF(boolean b){
>	return b?Boolean.TRUE?boolean.FALSE;
>}
>```

### 具有名称

