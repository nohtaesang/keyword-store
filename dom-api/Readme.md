* DOM
    * The Document Object Model
    * HTML, XML 문서의 프로그래밍 Interface 이다.
    * 문서의 구조화된 표현을 제공한다.
    * 프로그래밍언어가 DOM 구조에 접근할 수 있는 방법을 제공한다.
    * 그로인해 문서 구조, 스타일, 내용 등을 변경할 수 있게 돕는다.
    * 이 문서는 웹 브라우저를 통해 그 내용이 해석되어 웹 브라우저 화면에 나타나거나 HTML 소스 자체로 나타난다.
    * 자바스크립트는 document(문서)와 element(요소)에 접근하기 위해 DOM을 사용한다.
    * DOM은 프로그래밍 언어는 아니다.
    * 하지만 DOM이 없다면 자바스크립트 언어는 문서들에 대한 정보를 갖지 못한다.
    * 문서의 모든 element는 문서를 위한 document object model의 한 부분이다
    * 어떻게 DOM에 접근할 수 있는가?
        * 각각의 브라우저는 자신만의 방법으로 DOM을 구현했다.
        * 스크립트를 작성할 때, 문서 자체를 조작하거나 문서의 children을 얻기 위해 document 또는 window elements를 위한 API를 즉시 사용할 수 있다.
    * 중요한 데이터 타입들
        * document
            * 이 object는 root document object 자체이다.
        * element
            * DOM API의 member에 의해 return 된 element 또는 element type의 node를 의미한다.
        * nodeList
            * elements의 배열
                * list.item(1)
                * list[1]
        * attribute
            * attribute에 대한 특별한 인터페이스를 노출하는 object reference이다.
            * DOM에서 elements와 같은 nodes이다.
    * DOM interfaces
        * 많은 objects가 여러 개의 interfaces와 연관되어 있다.
        * HTML element는 DOM 이 연관되어 있는 한 nodes 트리에서 하나의 node이다.
        * document
            * root document 자체라고 할 수 있다.
        * window
            * window object는 브라우저와 같다고 할 수 있다.
        * generic Node interface 로부터 상속받은 element와 node, element interface가 협력하여 각각의 elements에서 상요할 수 있는 수많은 methods와 properties를 제공한다.




* document.getElementById('str')
    * 문서에서 str과 일치하는 id를 가진 속성을 찾는다.
    * 이를 나타내는 Element 객체를 반환한다.
    * ID는 문서에서 유일해야 하기 때문에 특정 요소를 빠르게 찾을 때 유용하다.
    * Document.querySelector() 나 Document.querySelectorAll() 과는 달리 전역 document에서만 사용할 수 있다.
    * id에 맞는 요소가 없을 땐 null을 반환한다.
    * Node.insertBefore() 와 같은 메서드로 문서 트리에 삽입하지 않는다면 찾을 수 없다.

* Document
    * 브라우저가 불러온 웹 페이지를 나타낸다.
    * 페이지 콘텐츠(DOM 트리)의 진입점 역할을 수행한다.
    * HTML, XML, SVG 등에 따라서 다양한 API가 존재한다.
        * ex) "text/html" 콘텐츠 유형으로 제공되는 HTML 문서는 HTMLDocument 인터페이스를 구현한다.
        * ex) XML과 SVG 문서는 XMLDocument 인터페이스를 구현한다.
    * Document 인터페이스는 ParentNode 인터페이스를 확장한다.
    * Method
        * createElement()
            * 지정한 tagName의 HTML요소를 만들어 반환한다.
            * tagName을 인식할 수 없으면 HTMLUnknownElement를 대신 반환한다.
            
        * getElementsByClassName()
            * 주어진 클래스 이름을 갖는 요소의 목록을 반환한다.
        * getElementsByTagName()
            * 주어진 태그명을 갖는 요소의 목록을 반환한다.
        * getElementById()
            * 주어진 ID를 가진 요소의 참조를 반환한다.
        * querySelector()
            * 문서 내에서 주어진 선택자를 만족하는 첫 번째 Element를 반환한다.
        * querySelectorAll()
            * 주어진 선택자를 만족하는 모든 요소의 NodeList를 반환한다.
        * hasFocus()
            * 포커스가 지정한 문서 내 어느 곳이든 위치한 경우 true를 반환한다.
        

* ParentNode
    * 자식을 가질 수 있는 모든 종류의 Node 객체가 공통으로 가지는 메서드와 속성을 가진다.
    * Element, Document, DocumentFragment 객체가 구현한다.
    * Property
        * childElementCount
            * 자식의 수를 반환한다.
        * children
            * ParentNode가 가진 모든 자식 중 요소만 모은 HTMLCollection을 반환한다.
                * HTMLCollection.item() 
                    * 0부터 시작하는 index의 특정 노드를 리스트에 반환한다.
                    * 인덱스의 범위에서 벗어나면 null을 반환한다.
    * Method
        * append()
            * ParentNode의 마지막 자식 다음에 주어진 Node나 DOMString 객체를 삽입한다.
            * DOMString객체는 동등한 Text 처럼 취급한다.
        * prepend()
            * 첫 번째 자식 이전에 객체를 삽입한다.
        * querySelector()
            * 하위 요소 중 주어진 선택자를 만족하는 첫 번째 Element를 반환한다.
        * querySelectorAll()
            * 하위 요소 중 주어진 선택자를 만족하는 모든 요소의 NodeList를 반환한다.

* NodeList
    * 일반적으로 element.childNodes, document.querySelectorAll 과 같은 메서드에 의해 반환되는 노드의 콜렉션이다.
    * NodeList는 Array는 아니지만, forEach() 를 사용하여 반복할 수 있다.
    * 또한 Array.from()을 사용하여 Array로 변환할 수 있다.
    * 위의 기능이 구현되지 않은 오래된 브라우저에서는 Array.prototype.forEach()를 사용하면 된다.
    * chlidNodes 는 실시간(참조), querySelectorAll은 정적(복사) NodeList를 반환한다.
    * Method
        * item()
            * 리스트 내 항목의 인덱스를 반환하고, 인덱스가 범위 외의 경우일 땐 null을 반환한다.
            * nodeList[idx] 의 대안으로 사용할 수 있다. (i가 범위를 벗어나면 undefined를 반환하므로.)
            * nodeItem = nodeList.item(index)
        * entries()
            * iterator 를 반환한다.
            * 코드가 콜렉션에 포함된 모든 키/값(노드) 쌍을 순회할 수 있도록 한다.

* Node
    * 여러 가지 DOM 타입들이 상속하는 인터페이스이다.
    * 그 다양한 타입들을 비슷하게 처리할 수 있게 있다.
    * 부모인 EventTarget 으로부터 메소드를 상속한다.

* EventTarget
    * EventTarget은 이벤트를 받고 그 이벤트 listener를 가질 수 있는 객체에 의해 구현된 인터페이스이다.
    * Element, documet, window는 가장 흔한 이벤트 대상이다.
    * Method
        * addEventListener()
            * EventTarget에 특정 이벤트 유형의 이벤트 처리기를 등록
        * removeEventListener()
            * EventTarget에서 이벤트 수신기를 제거한다.

* DOMString
    * DOMString은 UTF-16 문자열이다.
    * JavaScript의 String도 UTF-16 문자열이다
    * DOMString을 받는 매개변수에 null을 전달하면, 보통 문자열로 변환해 'null'이 된다.
* Element

* Document.querySelect()
* Node.insertBefore