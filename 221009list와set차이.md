# List와 Set차이

## List

- 리스트는 중복을 허용한다.
- 데이터들의 순서가 있다. (인덱스)



## Set

- set은 데이터들의 중복을 허용하지 않는다.
- 데이터들의 순서를 보장하지 않는다.
- 그 이유는 set은 보통 hash table로 구현되기 때문이다.
  - hash 테이블에서는 key가 중복되지 않습니다.
  - hash 테이블에서 key를 검색하는 것은 O(1)로 매우 효율적입니다.
  - 따라서 데이터를 검색할 때 list보다 Set에서 하는 것이 훨씬 빠르고 효율적입니다.



---

출처: https://www.youtube.com/watch?v=CMgpTGs_N_w