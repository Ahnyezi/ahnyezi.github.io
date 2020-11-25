---
title:  "[error] cx_Oracle.DatabaseError: ORA-01036: illegal variable name/number 오류"
date: 2020-7-1
categories: ['Debug']
tags: ['파이썬', '오라클']
---

- 에러 메세지 <br>
![](https://lh3.googleusercontent.com/vwq_ZjEWqCsXd5c7R0FmNZHlm_fIeHCmSlG1w4MnA9F69TacFtWOHV6xJ2a5XCLYEeD3JQuidtizEY6vMaFhIebkfT-Y4IPZi6vxFfmrg9Mi-kdY_jecoofpGPrtinLGC_HLrd6u)

- 문제 원인 <br>
파이썬에서 오라클로 전달하려는 sql문의 문법이 맞지 않아서 생기는 오류.
```python
# 오류 문장 id컬럼에 ':' 빠져있음
sql = 'update car_member set pwd =:1, tel=:2, car_num =:3 where id =4'
# 수정 문장
sql = 'update car_member set pwd =:1, tel=:2, car_num =:3 where id =:4'
```
