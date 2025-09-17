---
layout: post
related_posts:
  - /programmers/lv2
title:  "JadenCase 문자열 만들기"
date:   2025-09-13
categories:
  - programmers
  - lv2
description: >
  java, python으로 풀이
---
* toc
{:toc .large-only}

## Java
```java
class Solution {
    public String solution(String s) {
        s = s.toLowerCase();
        char[] ch = s.toCharArray();
        
        for(int i = 0; i < ch.length; i++) {
            if(i == 0 || ch[i - 1] == ' ') {
                ch[i] = Character.toUpperCase(ch[i]);
            }
        }
            
        return new String(ch);
    }
}
```