---
layout: post
title: mongodb 적응기 1
subtitle: 몽고db 적응기
categories: MongoDB
tags: [MONGODB]
date: 2023-04-22
---
# 잡설
게임회사로 이직했다!
nestjs와 rdbms 사용하다
express와 mongodb를 사용해야한다.!!!!
그래서 몽고디비를 좀 공부를 할 필요가 있다!

몽고디비 강의를 구매했다!!
솔직히 oop와 di, module controller service구조가 그립다... ㅋㅋㅋ
하지만 인간은 적응의 동물이지 않은가!
채팅서버나 알림톡을 사용하면서 대충 사용해 보긴 했지만
본격적으로 몽고만 쓰려니까 어색하고 힘드네

빠르게 적응해야해!! 게임서버 개발 재밌을거 같다!
그러니 그러니 쉬는 날도 열심히 공부하자!!!!!!!!!!!!!!!!!!!!!!!
당분간 화이팅이다........

## db 구조
### rbdms 와 비교!

db - db
table - collection
column - field
row - document

### index unique
몽고 디비에도 똑같이 컬럼? unique 와 index를 줄수 있다.

### objectid validate
단순 스트링인지 objectId 형태에 맞는지 검증 할땐
mongoose.isValidObectId(userId)
이런 형태로 이게 objectid 인지 검증이 가능하다.
return 은 boolean값으로 

### delete
delete 할때 hardDelete 의경우
findOneAndDelete 을 사용 할 수 있다.
이 경우는 return이 삭제된경우 삭제된 것의 객체를 리턴 
즉 삭제를 안한경우는 null을 리턴한다.
그냥 delete의 경우는 객체를 불러오지 않는

### mongoose 장점
몽고db orm으로 mongoose가 있다.
얘가 스트링 타입으로 id 를 보내도 쿼리를 만들때 objectId로 바꿔주거나
update할때 $set을 안해도 다른값들이 안바뀌게 $set옵션을 기본으로 추가해 준다!
이런 부분들이 mongoose의 장점 중 하나다!
그리고 쿼리를 createAt updateAt 관리도 해주


## node 비동기 성능 개선 팁

```
const user = await user.find()
const blog = await blog.find()
```
이런 함수 두개가 있을때 단순히 불러오는 거라면 user를 가져오고 blog를
가져와 동기적으로 처리한다.
이럴땐 

```
const [blog, user] = await Promise.all([
    user.find()
    blog.find()
])

```
해주면 프로미스 올 안에서 비동기 적으로 처리되어 성능을 개선할 수 있다.


## nesting된 것 수정할 때
```
Comment.findOneAndUpdate({_id: commentId}, {content}, {new: true})
Blog.updateOne(
    {"comment._id": commentID},
    {"comments.$.content": content}
)
```
코멘트id에 해당 id 의 컨텐츠를 수정할 수 있다