---
title: C++字符串操作
date: 2018-06-20 10:39:07
tags: [c++, 字符串, string]
categories: [c++]
---
C++中常见的字符串操作， 直接看代码， 注释比较详细
<!--more-->
```cpp
#include <iostream>
using namespace std;
int main()
{
    //初始化字符 
    string newString;
    string message("Aloha World!");
    cout << message << endl;
    char arr[] = {'H', 'e', 'l', 'l', 'o', '\0'};
    string st1(arr); //用字符数组初始化字符串 
    cout << st1 << endl;
    
    //向字符后追加内容 
    string s1("Welcome");
    s1.append(" to C++"); //向s1后边追加 " to C++"  
    cout << s1 << endl; //输出结果为 "Welcome to C++" 
    string s2("Welcome");
    s2.append(" to C and C++", 3, 2); //向s2后边追加字符串从第3个位置开始的2个字符，即" C" 
    cout << s2 << endl; //输出 "Welcome C"
    string s3("Welcome");
    s3.append(" to C and C++",5); //向s3后边追加从头开始的5个字符，即" to C"
    cout << s3 << endl; //输出 "Welcome to C"
    string s4("Welcome");
    s4.append(4,'G'); //向s4后边追加4个'G'
    cout << s4 << endl; //输出 "WelcomeGGGG"
    
    //给字符赋新值 
    string s5("Welcome");
    s5.assign("Dallas"); //将"Dallas"赋给s5，覆盖原值
    cout << s5 << endl; //输出"Dallas"
    string s6("Welcome");
    s6.assign("Dallas,Texas",1,3); //将字符串从1号位置开始的3个字符赋给s6，即 "all"
    cout << s6 << endl; //输出"all"
    string s7("Welcome");
    s7.assign("Dallas,Texas",6); //将字符串从头开始的6个字符赋值给s7，即"Dallas"
    cout << s7 << endl; //输出"Dallas"
    string s8("Welcome");
    s8.assign(4,'G'); //将字符G复制4份赋值给s8，即"GGGG"
    cout << s8 << endl; //输出 "GGGG"
    
    //清空字符串
    string s9("Welcome");
    cout << s9.at(3) << endl; //打印位于字符串s9的3号位置的字符 即 'c'
    cout << s9.erase(2,3) << endl; //擦除字符串s9从2号位置开始的3个字符 即"lco"，现在s9的值为"Weme"
    s9.clear(); //清空字符串s9
    cout << s9.empty() << endl; //判断s9是否为空，是则打印1，否则打印0.此处打印1 
    
    //比较字符串 compare();
    string s10("Welcome");
    string s11("Welcomg");
    cout << s10.compare(s11) << endl; //用s10和s11的ASCII码相减，打印 -2 (s10 - s11) 
    cout << s11.compare(s10) << endl; //用s11和s10的ASCII码相减，打印 2 (s11 - s10)
    cout << s10.compare("Welcome") << endl; //打印 0
    
    //获取子字符串
    string s12("Welcome");
    cout << s12.substr(0,1) << endl; //获取s12从0号位置开始的1个字符 即 'W' 
    cout << s12.substr(3) << endl; //获取s12从3号位置开始到结尾的字符串 即 "come"
    cout << s12.substr(3,3) << endl; //获取s12从3号位置开始的3个字符 即 "com"
    
    //搜索字符串
    string s13("Welcome to HTML");
    cout << s13.find("co") << endl; //在s13中搜索 "co"，返回其第一次出现的位置 即 3
    cout << s13.find("co",6) << endl; //在s13中从6号位置向后搜索"co",并返回第一次出现的位置，如果没有返回 -1  此处为 -1
    cout << s13.find('o') << endl; //在s13中搜索字符 o 的位置，并返回第一次出现的位置 此处为 4
    cout << s13.find('o',6) << endl; //在s13中从6号位置搜索字符 o 的位置，并返回第一次出现的位置，如果没有返回 -1 此处为 9 
    
    //插入和替换字符串
    string s14("Welcome to HTML");
    s14.insert(11,"C++ and "); //从s14的11号位置插入 "C++ and ",插入后s14的值为 "Welcome to C++ and HTML"
    cout << s14 << endl;
    string s15("AA");
    s15.insert(1,4,'B'); //从s15的1号位置插入4个字符B，插入后s15的值为 "ABBBBA"
    cout << s15 << endl;
    string s16("Welcome to HTML");
    s16.replace(11,4,"C++"); //将s16从11号位置开始的4个字符替换为 "C++",注意字符串结尾的 '\0' 
    cout << s16 << endl;
    
    return 0;
} 
```