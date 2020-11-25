---
title:  "[error] TypeError: must be real number, not tuple 오류"
date: 2020-7-12
categories: ['Debug']
tags: ['파이썬', '파일 출력']
---

- 에러 내용 <br>
![](https://lh6.googleusercontent.com/dLIjGTWTW9NQFyMNOu67hecxD-_o9OmNoogu4nFr-gK6BL-vjLbSn9BFe8dD8tC0ED2X-U6YtzarUi3SxAwtJY8gBS19BAXpJYu99gXcr_YqKrQGDn_ILA58TUSRWWqxZwsRH8tz)

- 문제 코드  <br>
![](https://lh4.googleusercontent.com/q-HdcpTxRHClGlq037vP2ww6Y5NAHI88zUmAxz5YAB4CLL7hPq7sl87pDoN768JmaayS53xk8O7hLlbHSMY9I9MN6Iem_fNU3dsMs-e5hvQtp7FH9KJkd3zJ9_1yKq_RRkDUS2r-)

- 문제 원인 <br>
저장할 path(str타입)를 output 변수에 저장해서 처리하면 <br>
VideoWriter에서 output 사용할 때 str으로의 참조값이 들어감<br>
<br>

- 해결 <br>
변수를 사용하지 말고 대신 저장할 “경로” 직접 삽입
```python
writer = cv2.VideoWriter('/videos/result.avi',fourcc,30,(frame.shape[1],frame.shape[0],True)
```