---
title: Spring表达式语言
date: 2018-01-07 20:08:48
tags: Spring
categories: 技术
---
# Spring表达式语言:SpEL(一段简单的spel入门代码)

## 代码
``` java 代码
package com.panjianguo.service;

import org.springframework.expression.EvaluationContext;
import org.springframework.expression.Expression;
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;
import org.springframework.expression.spel.support.StandardEvaluationContext;

import java.lang.reflect.Method;

/**
 * 一句话功能简述
 */
public class SpelTest {

    public static void main(String[] args) throws NoSuchMethodException {
        //创建解析器
        ExpressionParser parser = new SpelExpressionParser();
        //解析表达式
        Expression expression =
                parser.parseExpression("#decorate('Hello '+#person.name+#end)");
        //构造上下文
        EvaluationContext context = new StandardEvaluationContext();
        //为end参数值来赋值
        context.setVariable("end", "!");
        
        // 为对象赋值
        Person person = new Person("小明", "男", 18);
        context.setVariable("person", person);
        
        // 为方法赋值
        Method decorate = SpelTest.class.getDeclaredMethod("decorate", String.class);
        context.setVariable("decorate", decorate);
        //打印expression表达式的值
        System.out.println(expression.getValue(context));
    }

    // 人员
    static class Person {
        private String name;
        private String gender;
        private Integer age;

        Person(String name, String gender, Integer age) {
            this.name = name;
            this.gender = gender;
            this.age = age;
        }

        public String getName() {
            return name;
        }
    }

    // 修饰方法
    static String decorate(String content) {
        return "***"+content+"***";
    }


}

```

<!--more-->

## 执行输出值
```
/Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/bin/java ...
***Hello 小明!***

Process finished with exit code 0
```
## 主要步骤 
1) 创建解析器
2) 给解析器设置解析表达式
3) 创建构造上下文
4) 为构造上下文设置转化参数
5) 解析器通过构造上下文将解析表达式转化成想要的内容（在SpEL表达式中，默认情况下，表达式前缀为 ' # ' ，而后缀为 ' } ' 。如果表达式中没有前缀和后缀，那么表达式字符串就被当作纯文本。）
