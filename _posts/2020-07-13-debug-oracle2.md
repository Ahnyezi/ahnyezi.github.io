---
title:  "[error] 파이썬 오라클 연동 'like', '%' 사용할 때 주의할 점"
date: 2020-7-13
categories: ['Debug']
tags: ['파이썬', '오라클']
---

[참고 블로그](https://brownbears.tistory.com/421)

- 파이썬에서 '%'는 외부에서 값을 받아올 때 타입을 지정해주기 위해 사용(c언어 방식과 동일) 
```python
test = 'Hello %s' %'Bob'
print(test)
# Hello Bob
```

- 따라서 like를 사용하는 sql문에서 %를 함께 쓸 경우에
-  sql2 처럼 sql 문 내에 직접 %를 사용하지 않고
-  sql1의 형태를 사용한 뒤에, :1 값을 지정해줄때 사용하는 변수d에 직접 %를 더한 str문을 삽입한다.
- 코드: sql1(o), sql2(x)
![](https://lh6.googleusercontent.com/mIFzfn8LPxn35Z-J_5zfPUcTZPU7ygE-HfyhGIdC41iEJ7FPnjSVpqqusVQoYrl2xCNlh1aqAXeyrUZN__PXS7mzPILtWGTVp3rj-qagbfrMRi-o-iw9x2-DPXPnfAL0xaYLbvuJ)


