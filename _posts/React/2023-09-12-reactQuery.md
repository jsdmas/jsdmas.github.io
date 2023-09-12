---
title: 'react-Query'
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2023-09-12
---

# React-Query

> fetching, caching, 서버 데이터와의 동기화를 지원해주는 라이브러리입니다.

# staleTime

stale의 사전적 정의는 [신선하지 않은, (만든 지) 오래된]입니다.

말 그대로 staleTime은 데이터가 신선한 상태로 남아 있는 시간을 말합니다. 리액트 쿼리에서의 기본 값은 0입니다.

리액트 쿼리에서 데이터는 fresh한 데이터일 수도, stale한 데이터일 수도 있습니다.

fresh한 데이터라면, API 호출 없이 저장된(캐싱된) 데이터가 다시 사용됩니다.

stale한 데이터라면, 윈도우에 다시 포커스되었을 때나, 컴포넌트가 다시 마운트될 때나, 네트워크가 다시 연결되었을 때 등 신선한 데이터를 다시 페치해 올 것입니다.

# cacheTime

cacheTime은 메모리에 저장된 캐싱 데이터가 유효한 시간을 말합니다. 리액트 쿼리에서의 기본 값은 300000으로, 5분입니다.

쿼리를 사용하는 모든 컴포넌트가 언마운트되었을 때, 쿼리는 비활성화(inactive 상태)됩니다. 그리고 비활성화된 데이터는 캐시에서 삭제됩니다.

데이터가 stale한 상태이면 데이터를 다시 페치해 올 텐데, API 요청을 통해 신선한 데이터를 완전히 가져오기 전까지 리액트 쿼리는 캐싱된 데이터를 사용합니다.

# 기본 설정일 경우(staleTime=0, cacheTime=5m)

기본 설정일 경우에는, 데이터의 상태는 반환되자마자 stale로 판단되므로 늘 stale한 데이터라고 할 수 있습니다.  
따라서 쿼리를 요청할 때마다 API 요청을 하게 됩니다.

# staleTime이 5분, cacheTime이 5분일 때

처음으로 쿼리가 호출되었을 때는 위와 동일합니다.  
1분 뒤, 같은 쿼리를 호출해본다 가정하면 캐시는 5분 동안 유효하므로 사용할 수 있습니다.  
staleTime도 동일하게 5분이므로, 아직 신선한 데이터를 가지고 있습니다.  
따라서 캐시된 데이터를 사용하고, API 요청은 하지 않습니다.

# staleTime 5분, cacheTime 0분일 때

처음으로 쿼리가 호출되었을 때는 동일합니다.  
1분 뒤, 같은 쿼리를 호출한다면 데이터는 신선한 상태이므로, API 요청을 하지 않고 반환된 데이터를 그대로 사용합니다.  
30분 뒤 같은 쿼리를 호출했을 땐, 데이터는 stale 상태로 변질되었습니다. 따라서 새로운 API 요청을 하게 됩니다.

# staleTime을 조정 해야할 시기

staleTime이 기본 값인 0인 이상, 데이터는 프레시하다고 간주할 수 없습니다.  
데이터의 변경이 자주 일어나지 않는다면, staleTime을 조정하여 사용하는 게 불필요한 API 호출을 더 줄일 수 있기 때문에 더 이득일 수도 있습니다.

예를 들어 변경할 수 있는 사용자가 로그인한 유저 단 한 명일 경우(개인의 투두 리스트를 관리하는 경우, 프로필 수정 등)에는 staleTime을 Infinity로 설정하고,  
POST/UPDATE/DELETE 이벤트가 발생했을 경우에만 쿼리 무효화를 통해 새로운 데이터를 갱신하여 불필요한 HTTP Request 요청을 줄이고 효과적으로 서버 데이터를 관리할 수 있습니다.

# Ref

- [react-query_staletime](https://tanstack.com/query/v4/docs/react/guides/initial-query-data#staletime-and-initialdataupdatedat)
- [react-query_cache](https://tanstack.com/query/latest/docs/react/guides/caching?from=reactQueryV3&original=https%3A%2F%2Ftanstack.com%2Fquery%2Fv3%2Fdocs%2Fguides%2Fcaching)
- [https://www.codemzy.com/blog/react-query-cachetime-staletime](https://www.codemzy.com/blog/react-query-cachetime-staletime)
