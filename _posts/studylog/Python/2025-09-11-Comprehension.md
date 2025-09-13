---
layout: post
related_posts:
    - /studylog/python
title:  "컴프리헨션 Comprehension(List, Set, Dictionary)"
date:   2025-09-11
categories:
  - studylog
  - python
description: >
  예시를 통해 
---
* toc
{:toc .large-only}

# 컴프리헨션(Comprehension)
컴프리헨션(Comprehension)은 파이썬에서 리스트, 세트, 딕셔너리 등의 컬렉션을 간단하게 생성하거나 변형하는 방법 중 하나입니다. 컴프리헨션은 반복문과 조건문을 사용하여 간결하게 컬렉션을 생성하는 기법으로, 코드를 더 간단하고 가독성 좋게 작성할 수 있도록 도와줍니다.
## 리스트 컴프리헨션
리스트 컴프리헨션은 새로운 리스트를 생성하는데 사용됩니다. 기존 리스트의 각 요소를 반복하면서 조건을 적용하여 새로운 리스트를 생성할 수 있습니다.