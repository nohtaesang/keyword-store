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