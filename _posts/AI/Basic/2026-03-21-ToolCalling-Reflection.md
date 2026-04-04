---
layout: post
related_posts:
  - /ai/basic
title:  "Tool Calling & Reflection - 에이전트 실전 패턴"
date:   2026-03-19
categories:
  - ai
description: >
    외부 도구를 직접 호출하는 Tool Calling Agent부터, 스스로 결과를 평가·개선하는 Reflection/Reflexion 패턴까지 코드 중심으로 학습
---
* toc
{:toc .large-only}

# Tool Calling Agent란?

자신의 지식만 사용하는 것이 아니라, **외부 도구(API, DB 코드 실행기 등)를 직접 호출** 해 문제를 해결하는 에이전트이다.

