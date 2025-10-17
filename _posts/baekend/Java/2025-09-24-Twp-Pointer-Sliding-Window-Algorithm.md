---
layout: post
related_posts:
    - /studylog/java
title:  "투 포인터 & 슬라이딩 윈도우 알고리즘(Two Poiter & Sliding Window Algorithm)"
date:   2025-09-24
categories:
  - studylog
  - java
description: >
  
---
* toc
{:toc .large-only}

![TwoPointerVsSlidingWindow](https://velog.velcdn.com/images/dongwookang/post/80f23520-9022-4443-9a1e-c907a5dc3369/image.png)

# Two Pointer(투 포인터)
* 포인터 두 개를 잡는다.(start, end)
* 단, start는 항상 end보다 작거나 같다.

![TwoPointerImg1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fdp7u3i%2Fbtrf5h6owjq%2FAAAAAAAAAAAAAAAAAAAAAB3kEBNs8ON2C6N-T9UH5zcpTITlbuDCWr1L2nh19GyW%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3D6xXi1MNNK%252FZqXngwVKCGbDUWkQg%253D)

### start < n 보다 작을 때, sum > M이면
![TwoPointerImg2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FOtvZe%2FbtrfXztwrsR%2FAAAAAAAAAAAAAAAAAAAAAFSTeH_lEx1JRKn_5AI7jBqNRvLjJIOe0gm40mGjesMX%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3DJH%252FwQXDek8XdrcyEcvPuZfePk5Q%253D)

### sum에서 start의 위치 값을 빼주고(sum -= start), start를 한 칸 앞으로 이동한다.(start++)
![TwoPointerImg3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fdrekok%2FbtrfU3CmVBP%2FAAAAAAAAAAAAAAAAAAAAAKkbIFLPdqaN2uxuKzgyeoAxKD3wTIKDqdCciCu1_1AY%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3DG%252FmZiM%252BhS2lCiEYE2YfkhuCvVYc%253D)

### end가 n에 도달하면 while문을 빠져나온다.
![TwoPointerImg4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbCCbB8%2FbtrfXHxRueK%2FAAAAAAAAAAAAAAAAAAAAAPcAo2INwYsfUvtAC4N6PNpa1B08oGOlDly_xPl9JpoG%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3D3ZK9GgiDKBSht5kJMdGpI5DW3YM%253D)

### sum < M이면
![TwoPointerImg5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Ftdaaf%2FbtrfWbGCKTJ%2FAAAAAAAAAAAAAAAAAAAAAJjsrK_XTsuym5bg1VHSCB8K9IvFDp8df2cOQ3RGkyWv%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3D%252FpznaVMKtQzRFhpC9VKIPDtEyHY%253D)

### sum에 현재 end 위치 값을 더해주고(sum += end), end를 한 칸 앞으로 이동한다.(end++)
![TwoPointerImg6](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FdkCOsF%2FbtrfU6ej9WF%2FAAAAAAAAAAAAAAAAAAAAAMQF7awRKHgfMuHEM736nnFINsBmLxFXgowQTg92kCgw%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3D6oihImgTn16EzXqUTE2NU8kFNZ0%253D)

### sum == m이면, 카운트 한다.(cnt++)
![TwoPointerImg7](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FcWw1Pw%2FbtrfWJ3MITW%2FAAAAAAAAAAAAAAAAAAAAAFm1jFYap79F49i5G7hKynXqn7nAGKXmjLuk8JheoPCo%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1759244399%26allow_ip%3D%26allow_referer%3D%26signature%3DJ9JJxH3lHdEQD6qRM3Vv0am6%252BSM%253D)

# Sliding Window(슬라이딩 윈도우)
![SlidingWindowImg1](https://velog.velcdn.com/images/wlwl99/post/e0ebddc2-e075-41e5-bec6-ac424364d1da/image.png)
