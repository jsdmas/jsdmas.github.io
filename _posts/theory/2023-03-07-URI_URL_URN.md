---
title:  "URI, URL, URN"
excerpt: "URI, URL, URN 설명"
toc : true
toc_sticky: true
categories:
  - theory
tags:
  - theory
last_modified_at: 2023-03-07
---
URI, URL, URN의 각 뜻은 다음과 같이 정의할 수 있습니다.
- 자원의 식별자(URI)
- 위치 (URL)
- 이름(URN)

# URI (Uniform Resource Identifier)
통합 자원 식별자(Uniform Resource Identifier)는 인터넷에 있는 자원을 어디에 있는지 **자원 자체를 식별하는 방법**입니다.  
- Uniform : 리소스를 식별하는 통일된 방식
- Resource : 자원, URI로 식별할 수 있는 모든 것
  - 여기서 자원은 웹 브라우저의 파일만 뜻하는게 아니라, 실시간 교통정보 등 우리가 구분할 수 있는 것은 모든 게 리소스가 됩니다.
- Identifier : 다른 항목과 구분하는데 필요한 정보

URI의 존재는 인터넷에서 요구되는 기본조건으로서 인터넷 프로토콜에 항상 붙어 다닌다.   
URI의 하위개념으로 URL, URN 이 있습니다.   

# URL (Uniform Resource Locator)
- 파일식별자 (Uniform Resource Locator)는 네트워크 상에서 **자원이 어디 있는지 위치**를 알려주기 위한 규약입니다.
- 즉, 컴퓨터 네트워크와 검색 메커니즘에서의 위치를 지정하는, 웹 리소스에 대한 참조입니다.
- 흔히 우리는 URL을 웹 사이트 주소로만 알고 있지만, URL은 웹 사이트 주소뿐만 아니라 컴퓨터 네트워크상의 자원을 모두 나타내는 표기법입니다.
- 그리고 해당 주소에 접속하려면 URL에 맞는 프로토콜(http, sftp, smp ...등)을 알아야 하고, 그와 동일한 프로토콜로 접속해야 합니다.

# URN (Uniform Resource Name)
- 통합 자원 이름(Uniform Resource Name)은 run:scheme 을 사용하는 URI를 위한 역사적인 이름입니다.
- URL이 리소스가 있는 위치를 지정한다면, URN은 리소스에 이름을 부여하는 것입니다.
- URN은 영속적이고, 위치에 독립적인 자원을 위한 지시자로 사용하기 위해 1997년도 RFC 2141 문서에서 정의되었습니다.
- 하지만 리소스가 이름에 매핑되어 있어야 하기 떄문에 이름으로 부여하면 거의 찾기가 힘듭니다. 그래서 대부분 URL만 씁니다.


