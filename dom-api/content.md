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


# DOM을 꺠우치다 - Cody Lindley
## 개요
* 문서 개체 모델(Document Object Model)은 자바스크립트 Node 개체의 계층화된 트리다.
    * HTML 문서를 작성할 때에는 HTML 콘텐츠를 다른 HTML 콘텐츠 내에 캡슐화하게 되는데, 이를 통해 트리로 표현 가능한 계층 구조가 만들어진다.
    * 대개 이러한 계층 구조나 캡슐화 시스템은 HTML 문서 내에서 들여쓰기 표시를 통해 시각적으로 표시된다.
    * 브라우저는 HTML 문서를 로딩 시 이 계층 구조를 해석해서 마크업이 어떻게 캡슐화되었는지를 보여주는 노드 개체 트리를 생성한다.
    * 브라우저는 HTML 코드를 해석해서 트리 형태로 구조화된 노드들을 가지고 있는 문서(DOM)를 생성한다.
    * HTML 문서가 브라우저에 의해 해석되어 실제 문서를 나타내는 노드 개체들의 트리 구조로 변환된다는 것이다.
    * DOM의 목적은 Javascript를 사용해서 이 문서에 대한 스크립팅(삭제, 추가, 바꾸기, 이벤트 처리, 수정 ... ) 작성을 위한 프로그래밍 인터페이스를 제공하는 것이다.
    * DOM은 원래 XML 문서를 위한 애플리케이션 프로그래밍 인터페이스였지만, HTML 문서에서도 사용되도록 확장되었다.
* 노드 개체 유형
    * HTML 문서를 다룰 때 마주치게 되는 가장 일반적인 노드 유형은 다음과 같다.
        * DOCUMET_NODE (ex. window.document)
        * ELEMENT_NODE (ex. <body>, <a>, <div>, <script>, <style>, ...)
        * ATTRIBUTE_NODE (ex. class='name')
        * TEXT_NODE (ex. 줄바꿈과 공백을 포함한 HTML 문서 내의 텍스트 문자)
        * DOCUMENT_FRAGMENT_NODE (ex. document.createDocumentFragment())
        * DOCUMENT_TYPE_NODE (ex. <!DOCTYPE html>)
    * 이 목록에 있는 노드 유형 형식은 javascript 브라우저 환경에서 Node 개체의 속성으로 기록되는 상수 값의 속성과 동일하다.
    * node의 이 속성들은 상수 값이며, node 개체의 특정 유형에 매핑되는 숫자 코드 값을 저장하는데 사용된다.
    ```
    console.log(Node.ELEMENT_NODE) // 1
    ```
    ```
    for (var key in Node) {
	    console.log(key, ' = ' + Node[key]); // 모든 노드의 유형과 그 값을 출력한다.
    }
    // ELEMENT_NODE = 1
    // ATTRIBUTE_NODE = 2
    ...
    ```
    * DOM 사양에서는 의미상 Node, Element, Text, Attr, HTMLAnchorElement를 인터페이스로 분류하고 있지만, 노드를 생성하는 javascript 생성자 함수에 부여된 이름이기도 하다는 점을 명심해야 한다.
    * 인터페이스이지만, 이 책에서는 개체나 생성자 함수로 언급한다.
    * ATTRIBUTE_NODE는 실제로는 트리의 일부가 아니지만, 역사적인 이유로 목록에 포함되었다.
    * attribute 노드를 실제 DOM 트리 구조에 참여하지 않는 element 노드의 하위 노드인 것으로 ㄱ나주하고 있다.
    * 이 책에서는 COMMENT_NODE 에 대해서 상세한 내용을 포함하고 있지 않지만, HTML 문서 내의 주석은 Comment 노드이며 사실상 Text 노드와 유사하다.
    * 이 책 전반에 걸쳐 노드들에 대해 다루겠지만, 특정 노드를 nodeType 이름을 사용해서 언급하는 경우는 별로 없을 것이다.
* Node 개체로부터 상속받은 하위 노드 개체
    * 통상적인 DOM 트리의 각 노드 개체는 Node로부터 속성과 메서드를 상속받는다.
    * 문서내의 노드 유형에 따라 Node 개체를 확장한 하위 노드 개체/인터페이스가 추가로 존재한다.
    * 다음 목록은 가장 일반적인 노드 인터페이스에 대해 브라우저에서 구현된 상속 모델을 나열하고 있다.
        * Object < Node < Element < HTMLElement
        * Object < Node < Attr (DOM4 에서 사용이 금지됨)
        * Object < Node < CharacterData < Text
        * Object < Node < Document < HTMLDocument
        * Object < Node < DocumentFragment
    * 모든 노드 유형이 Node로부터 상속받는다는 것뿐만 아니라 상속 체인이 길어질 수도 있다.
    * 예를 들어 모든 HTMLAnchorElement 노드는 HTMLElement, Element, Node, Object 개체로부터 속성 및 메서드를 상속 받는다.
    * Node는 javascript 생성자 함수에 불과하다.
        * 따라서 논리적으로 볼 때 Node는 javascript의 다른 개체들처럼 Object.prototype으로부터 상속을 받는다.
    * 모든 노드 유형이 Node 개체로부터 속성 및 메서드를 상속받는다는 것을 검증하기 위해, Element 노드 개체에 대해 루프를 돌면서 속성 및 메서드를 조사해보자.
    ```
    // element 노드 개체에 대한 참조를 얻음
    var nodeAnchor = document.querySelector('a');

    // element 노드 개체에 대한 속성 키를 저장하기 위한 props 배열을 생성
    var props = [];

    // element 노드 개체에 대해 루프를 돌면서 모든 속성 및 메서드를 얻어냄
    for (var key in nodeAnchor) {
        props.push(key);
    }

    // 속성 및 메서드를 알파벳순으로 기록
    console.log(props.sort());
    ```
    * 이 목록에는 Node 개체로부터 상속받은 속성 및 메서드 뿐만 아니라 Element, HTMLElement, HTMLAnchorElement, Node, Object 개체로부터 상속 받은 수많은 속성 및 메서드가 존재한다.
    * 여기서 요점은 이 모든 속성과 메서드를 살펴보는 것이 아니라, 모든 노드가 prototype 체인의 속성뿐만 아니라 생성자로부터 일련의 기본 속성 및 메서드를 상속받는다는 점을 언급하기 위함이다.
* 노드를 다루기 위한 속성 및 메서드
    * 앞에서 논의했듯이, 모든 노드 개체는 속성과 메서드를 1차적으로 Node 개체로부터 상속받는다.
    * 이 속성 및 메서드는 DOM을 조작, 조사, 탐색하는 기준이 되는 값과 함수다.
    * 노드 인터페이스에서 제공되는 속성 및 메서드 외에, document, HTMLElement, HTML*Element 인터페이스와 같은 하위 노드 인터페이스에서도 수많은 관련 속성 및 메서드가 제공된다.
    * 다음은 모든 노드 개체에서 상속되는 것 중 가장 흔히 사용되는 Node 속성 및 메서드인데, 하위 노드 인터페이스에 속한 노드들을 다루기 위한 관련 속성들도 포함되어 있다.
    * Node 속성
        * childNodes
        * firstChild
        * lastChild
        * nextSibling
        * nodeName
        * nodeType
        * nodeValue
        * parentNode
        * previousSibling
    * Node 메서드
        * appendChild()
        * cloneNode()
        * compareDocumentPosition()
        * contains()
        * hasChildNodes()
        * insertBefore()
        * isEqualNode()
        * removeChild()
        * replaceChild()
    * Document 메서드
        * document.createElement()
        * document.createTextNode()
    * HTML*Element 속성
        * innerHTML
        * outerHTML
        * textContent
        * innerText
        * outerText
        * firstElementChild
        * lastElementChild
        * nextElementChild
        * previousElementChild
        * children
    * HTML element 메서드
        * insertAdjacentHTML()

* 노드의 유형과 이름 식별하기
    * 모든 노드는 Node로부터 상속받은 nodeType 및 nodeName 속성을 가진다.
    * 예를 들어, Text 노드의 nodeType은 3이며, nodeName 값은 #text 이다.
    * 이전에 언급했듯이, 숫자 값 3은 노드가 나타내는 내장 개체의 형식을 나타내는 숫자 코드다.
    * 다음은 이 책에서 논의된 노드 개체들의 nodeType 및 nodeName에서 반환되는 값이다.
    ```
    const log = (temp) => {
        console.log(temp.nodeName, temp.nodeType);
    };

    log(document.doctype); // html 10
    log(document); // #document 9
    log(document.createDocumentFragment()); // #document-fragment 11
    log(document.querySelector('a')); // A 1(element 노드여서)
    log(document.querySelector('a').firstChild); // #text 3
    ```
    * 명확하지 않은 경우에 노드의 유형을 판별하는 가장 빠른 방법은 nodeType 속성을 확인해 보는것이다.
    * 노드 유형을 판별하는 것은 해당 노드에서 사용 가능한 속성과 메서드를 알 수 있게 해주므로, 스크립트 작성시에 매우 유용하다.

* 노드 값 가져오기
    * nodeValue 속성은 Text와 Comment를 제외한 대부분 노드 유형에서는 null 값을 반환한다.
    * 이 속성의 용도는 Text와 Commnet 노드에서 실제 텍스트 문자열을 추출하는데 초점을 맞추고 있다.
    * 다음 코드에서는 이 책에서 논의된 노드들에 대해 이 속성을 사용하는 예를 보여준다.
    ```
    const log = (temp) => console.log(temp);
    log(document.doctype.nodeValue); // null
    log(document.nodeValue); // null
    log(document.createDocumentFragment().nodeValue); // null
    log(document.querySelector('a').nodeValue); // null
    log(document.querySelector('a').firstChild.nodeValue); // Hi
    ```
    * Text나 Comment 노드 값은 nodeValue 속성에 새로운 문자열 값을 부여해서 설정할 수 있다.
        * document.querySelector('a').firstChild.nodeValue = 'hi'
* javascript 메서드를 사용해서 element 및 text 노드를 생성하기
    * 브라우저가 HTML 문서를 해석할 때 html 파일 내용을 기반으로 해서 노드와 트리를 구성하게 된다.
    * 브라우저는 html 문서를 초기 로딩할 때 노드 생성을 처리한다.
    * 하지만 javascript 를 사용해서 직접 노드를 생성하는 것도 가능하다.
    * 다음 두 메서드는 javascript를 사용해서 element 및 text 노드를 프로그래밍적으로 생성할 수 있게 해준다.
        * createElement()
        * createTextNode()
    * 다른 메서드들도 존재하지만, 흔히 사용되지 않는다.
    ```
    var elementNode = document.createElement('div');
    console.log(elementNode, elementNode.nodeType); // <div></div> 1

    var textNode = document.createTextNode('Hi');
    console.log(textNode, textNode.nodeType); // 'HI' 3
    ```
    * createElement() 메서드는 생성될 element를 지정하는 문자열을 매개변수로 받는다.
    * 이 문자열은 element 개체의 tagName 속성에서 반환되는 문자열과 동일하다.
    * createAttribute() 메서드는 사용이 금지되었다.
    * attribute 노드를 만드는데 사용되어서는 안된다.
    * 대신에, 통상적으로 개발자들은 getAttribute(), setAttribute(), removeAttribute() 메서드를 사용한다.

* javascript 문자열을 사용하여 DOM에 Element 및 Text 노드를 생성 및 추가하기
    * innerHTML, outerHTML, textContext, insertAdjacentHTML() 속성 및 메서드는 javascript 문자열을 사용하여 DOM에 노드를 추가하는 기능을 제공한다.
    * 다음 코드에서는 innerHTML, outerHTML, textContext 속성을 사용하여 javascript 문자열로부터 노드를 생성한 다음, 바로 DOM에 추가한ㄷ.
    ```
    // strong element와 text 노드를 생성해서 DOM에 추가
    document.getElementById('A').innerHTML = '<strong>Hi</strong>';

    // div element와 text 노드를 생성해서 <div id='B'></div>를 바꾼다(div#B가 교체된다.)
    document.getElementById('B').outerHTML = '<div id="B" class="new">Whats Shaking</div>';

    // Text 노드를 생성해서 span#C 를 갱신한다.
    document.getElementById('C').textContent = 'dude';

    // 다음은 비표준 확장이다.

    // Text 노드를 생성해서 div#D를 갱신한다
    document.getElementById('D').innerText = 'Keep it';

    // Text 노드를 생성해서 div#E를 Text 노드로 바꾼다.
    document.getElementById('E').outerText = 'real!';
    console.log(document.body.innerHTML);
    ```
    * Element 노드에서만 동작하는 insertAdjacentHTML() 메서드는 이전에 언급된 메서드들에 비해 보다 세밀하게 다룰 수 있다.
    * 이 메서드를 사용하면 시작 태그 앞, 시작 태그의 뒤, 종료 태그 앞, 종료 태그 뒤에 노드를 삽입하는 것이 가능하다.
    ```
    var elm = document.getElementById('elm');
    elm.insertAdjacentHTML('beforebegin', '<span>Hey-</span>');
    elm.insertAdjacentHTML('afterbegin', '<span>dude-</span>');
    elm.insertAdjacentHTML('beforeend', '<span>-are</span>');
    elm.insertAdjacentHTML('afterend', '<span>-you?</span>');
    console.log(document.body.innerHTML);
    // <span>Hey-</span><i id="elm"><span>dude-</span>how<span>-are</span></i><span>-you?</span>
    ```
    * innerHTML 속성은 문자열 내에서 발견될 HTML 요소를 실제 DOM 노드로 변환하는 반면, textContent는 텍스트 노드를 생성하는 데만 사용 가능하다.
    * HTML 요소를 포함하고 있는 문자열을 textContent에 전달하면, 단순히 텍스트로만 출력된다.
    * document.write() 역시 DOM에 일제히 노드를 생성해서 추가하는 데 사용될 수 있다.
        * 하지만 이 메서드는 서드파티 스크립트 작업을 수행하는 데 필요하지 않는 한, 통상적으로는 잘 사용되지 않는다.
        * 기본적으로 write() 메서드는 전달된 값을 페이지가 로딩 및 해석하는 동안 페이지에 출력한다.
        * write() 메서드를 사용하면 로딩된 HTML 문서가 해석되는 것을 지연/차단시키게 된다는 것에 유의해야 한다.
    * innertHTML 이 무겁고 비싼 대가를 치르는 HTML 파서를 호출하는 데 비해, text node 생성은 간단하게 처리되므로, innerHTML 계열의 사용을 삼가야 한다.
    * insertAdjacentHTML의 beforebegin 및 afterend 옵션은 노드가 DOM 트리 내에 존재하고 부모 요소를 가진 경우에만 동작한다.
    * textContent는 script 및 style요소를 비롯한 모든 요소의 내용을 가져올 수 있지만, innerText는 그렇지 않다.
    * innerText는 스타일에 대해서는 알고 있지만, textContent와는 달리 숨겨진 요소들의 텍스트는 반환하지 않는다.
* DOM 트리의 일부를 javascript 문자열로 추출하기
    * DOM에 노드를 생성하고 추가하는 데 사용하는 것과 동일한 속성(innertHTML, outerHTML, textContent)들이 DOM의 일부를 javascript 문자열로 추출하는데 사용된다.
    ```
    console.log(document.getElementById('A').innerHTML); // <i>Hi</i>
    console.log(document.getElementById('A').outerHTML); // <div id="A"><i>Hi</i></div>
    console.log(document.getElementById('A').textContent); // Hi
    console.log(document.getElementById('B').innerHTML); // Dude<strong> !</strong>
    console.log(document.getElementById('B').outerHTML); // <div id="B">Dude<strong> !</strong></div>
    console.log(document.getElementById('B').textContent); // Dude !
    ```
    * textContent, innerText, outerText 속성은 선택된 노드 내에 포함된 모든 텍스트 노드들을 반환한다.
    * 예를 들어 document.body.textContent는 첫 번째 텍스트 노드뿐만 아니라 body 요소 내에 포함된 모든 텍스트 노드들을 가져온다.

* appendChild() 및 insertBefore()를 사용하여 노드 개체를 DOM에 추가하기
    * appendChild() 및 insertBefore() 노드 메서드는 javascript 노드 개체를 DOM 트리에 삽입할 수 있게 해준다.
    * appendChild() 메서드는 하나의 노드를 메서드가 호출된 노드의 자식 노드 끝에 삽입할 수 있게 해준다.
    * 자식 노드가 존재하지 않으면, 해당 노드가 첫 번째 자식으로 추가된다.
    ```
    var elementNode = document.createElement('strong');
    var textNode = document.createTextNode(' Dude');

    document.querySelector('p').appendChild(elementNode);
    document.querySelector('strong').appendChild(textNode);
    
    // <p>Hi<strong> Dude</strong></p> 
    ```
    * 자식 노드 목록 끝에 노드를 추가하는 것 외에 삽입 위치를 조정하는 것이 필요하면, insertBefore()를 사용하면 된다.
    ```
    var text1 = document.createTextNode('1');
    var li = document.createElement('li');
    li.appendChild(text1);

    var ul = document.querySelector('ul');

    ul.insertBefore(li, ul.firstChild);
    console.log(document.body.innerHTML);
    {/* 
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>; 
    */}
    ```
    * insertBefore() 메서드는 2개의 매개변수를 필요로한다.
    * 삽입될 노드와 해당 노드를 삽입하고자 하는 문서 내의 참조 노드다.
    * insertBefore() 메서드에 두번째 매개변수를 전달하지 않으면, 이 메서드는 appendChild() 처럼 작동한다.
* removeChild() 및 replaceChild()를 사용하여 노드를 제거하거나 바꾸기
    * DOM에서 노드를 제거하는 것은 여러 단계의 과정으로 이루어진다.
    * 먼저 삭제하고자 하는 노드를 선택해야 한다.
    * 다음으로 부모 노드에 대한 접근을 얻어야 하는데, 보통 parentNode 속성을 사용하게 된다.
    * 부모 노드에서 삭제할 노드에 대한 참조를 전달하여 removeChild() 메서드를 호출한다.
    ```
    var divA = document.getElementById('A');
    divA.parentNode.removeChild(divA);
    var divB = document.getElementById('B');
    divB.parentNode.removeChild(divB);
    console.log(document.body.innerHTML);
    ```
    * element 노드나 텍스트 노드를 바꾸는 것은 제거하는 것과 별반 다르지 않다.
    ```
    var divA = document.getElementById('A');
    var newSpan = document.createElement('span');
    newSpan.textContent = 'kikiki';
    divA.parentNode.replaceChild(newSpan, divA);
    console.log(document.body.innerHTML);
    ```
    * 제거하거나 바꾸는 대상이 무엇인지에 따라 innerHTML, outerHTML, textContent 속성에 빈 문자열을 주는것이 더 쉽고 빠를 수도 있다.
    * 하지만 브라우저의 메모리 누수가 발생할 수 있으므로 조심해야 한다.
    * replaceChild() 및 removeChild()는 각각 교체되거나 제거된 노드를 반환한다.
    * 기본적으로 해당 노드는 바꾸거나 제거하는 것이므로 사라지지 않았다.
    * 이 동작은 해당 노드가 현재 문서의 범위를 벗어나게 만든다.
    * 해당 노드에 대한 메모리상의 참조는 여전히 가지게 된다.
* cloneNode()를 사용하여 노드를 복제하기
    * 다음 코드에서는 ul(ex. HTMLUListElement) 만 복제하고, 복제한 후에는 일반적인 노드 참조처럼 다루고 있다.
    ```
    var cloneUL = document.querySelector('ul').cloneNode();
    console.log(cloneUL); // <ul></ul>
    console.log(cloneUL.constructor);
    ```
    * 노드와 그 자식 노드를 모두 복제하려면, cloneNode()메서드의 매개변수로 true를 전달한다.
    ```
    var cloneUL = document.querySelector('ul').cloneNode(true);
    console.log(cloneUL);
    console.log(cloneUL.constructor);
    console.log(cloneUL.innerHTML);
    // <li>Hi</li>
    // <li>TaeSang</li>
    ```
    * Element 노드를 복제할 때, 모든 특성 및 값도 복제된다. (인라인 이벤트 포함)
    * addEventListener()나 node.onclick으로 추가된 것은 복제되지 않는다.
    * cloneNode(true)를 사용해서 노드와 그 자식을 복제하면 NodeList가 반환될 것이라 생각할 수 있지만, 실제로는 그렇지 않다.
    * cloneNode() 때문에 문서 내에서 요소 ID가 중복될 수 있다.
* 노드 컬렉션에 대한 이해(NodeList, HTMLCollection)
    * 트리에서 노드 그룹을 선택하거나 사전에 정의된 노드 집합에 접근하려면, 해당 노드들이 NodeList나 (ex. document.querySelectorAll(*)), HTMLCollection (ex. document.scripts) 내에 있어야 한다.
    * 배열과 유사한 이 개체 컬렉션들은 다음 과 같은 특징을 가진다.
        * 컬렉션은 라이브 상태 혹은 정적일 수 있다.
            * 이는 컬렉션 내에 포함된 노드들이 현재 문서 혹은 현재 문서에 대한 스냅샷의 일부라는 것을 의미한다.
        * 기본적으로 노드는 트리 순서에 따라 컬렉션 내에서 정렬된다.
            * 이 순서는 트리 루트로부터 분기점 까지의 선형 경로와 일치한다.
        * 컬렉션은 리스트 내의 요소 개수를 나타내는 length 속성을 가진다.
* 직계 자식 노드 전부에 댛나 리스트/컬렉션 얻기
    * childNodes 속성을 사용하면 직계 자식 노드에 대한 배열 형태의 리스트(ex. NodeList)가 나온다.
    ```
    var ulElementChildNodes = document.querySelector('ul').childNodes;
    console.log('ulElementChildNodes: ', ulElementChildNodes);
    Array.prototype.forEach.call(ulElementChildNodes, function(item) {
        console.log(item);
    });
    // 여기서 나오는 #text는 계행이다
    ```
    * childNodes에서 반환되는 NodeList는 직계 자식 노드만을 가진다.
    * childNoddes가 Element 노드 뿐만 아니라 다른 노드 유형도 포함한다는 점에 유의한다. (ex. Text 및 Comment 노드)

* NodeList나 HTMLCollection을 javascript 배열로 변환
    * NodeList나 HTMLCollection은 배열 형태이지만, array의 메서드를 상속하는 진정한 javascript 배열은 아니다.
    ```
    console.log(Array.isArray(document.links)); // false
    console.log(Array.isArray(document.querySelectorAll('a'))); // false
    ```
    * NodeList를 진정한 javascript 배열로 변환하는 것은 몇 가지 이점을 가져다준다.
    * 먼저 nodeList가 라이브 리스트인데 비해, 현재 DOM에 국한되지 않은 리스트 스냅샨을 만들 수 있게 해준다.
    * 다음으로, 리스트를 javascript 배열로 변환하면 Array 개체가 제공하는 메서드들에 접근할 수 있게 된다.
    * 유사 배열 리스트를 진정한 javascript 배열로 변환하기 위해, call() 혹은 apply()에 유사 배열 리스트를 전달하면, call() 혹은 apply()는 진짜 javascript 배열을 반환하는 메서드를 호출한다.
    ```
    console.log(Array.isArray(Array.prototype.slice.call(document.links))); // true
    console.log(Array.isArray(Array.prototype.slice.call(document.querySelectorAll('a')))); // true
    ```
    * ES6에 Array.from이 추가되었다.
* DOM 내의 노드 탐색
    * 노드 참조 (ex. document.querySelector(ul))에 다음과 같은 속성들을 사용하여 DOM을 탐색함으로써 다른 노드에 대한 참조를 얻을 수 있다.
        * parentNode
        * firstChild
        * lastChild
        * nextSibling
        * previousSibling
    ```
    <body><ul><!-- kikiki --> 
    <li id='A'></li>
    <li id='B'></li>
    <!-- zzz -->
    </ul>
    </body>

    //

    var ul = document.querySelector('ul');
    console.log(ul.parentNode.nodeName); // body
    console.log(ul.firstChild.nodeName); // #comment
    console.log(ul.lastChild.nodeName); // #text
    console.log(ul.querySelector('#A').nextSibling.nodeName); // #text (계행)
    console.log(ul.querySelector('#B').previousSibling.nodeName); // #text
    ```
    * DOM에 익숙하다면, DOM을 탐색하는 것에는 element 노드뿐만 아니라 text와 comment 노드도 포함된다는 것이 별로 놀랍지 않은데, 사실 이는 이상적이지 않다.
    * 다음 속성들을 사용하면, text와 comment노드를 무시하고 DOM을 탐색하는 것이 가능하다.
        * firstElementChild
        * lastElementChild
        * nextElementSibling
        * previousElementSibling
        * children
        * parentElement
        * childElementCount
    ```
    var ul = document.querySelector('ul');

    console.log(ul.firstElementChild.nodeName); // li
    console.log(ul.lastElementChild.nodeName); // li
    console.log(ul.querySelector('#A').nextElementSibling.nodeName); // li
    console.log(ul.querySelector('#Bzz').previousElementSibling.nodeName); // li
    console.log(ul.children); // HTMLCollection이다. 모든 자식 노드는 text 노드를 가진다.
    console.log(ul.firstElementChild.parentElement); // ul
    ```
* contains()와 compareDocumentPosition() 으로 DOM 트리 내의 Node 위치를 확인하기
    * 노드의 contains() 메서드를 사용하면 특정 노드가 다른 노드 내에 포함되었는지를 알 수 있다.
    * DOM 트리 내에서 주변 노드와 연관된 노드 위치에 대해 보다 확실한 정보를 얻고 싶을 경우, 노드의 compareDocumentPosition() 메서드를 사용하면 된다.
    * 기본적으로 이 메서드는 전달된 노드에 상대적으로 선택된 노드에 대한 정보를 요청할 수 있게 해준다.
    * 반환되는 정보는 숫자다
        * 0: 동일한 Element
        * 1: 선택된 노드와 전달된 노드가 동일한 문서에 존재하지 않음
        * 2: 전달된 노드가 선택된 노드 앞에 있음
        * 4: 전달된 노드가 선택된 노드 뒤에 있음
        * 8: 전달된 노드가 선택된 노드의 조상임
        * 16, 10: 전달된 노드가 선택된 노드의 자손임
    * contains()는 선택된 노드와 전달된 노드가 동일한 경우 true를 반환한다.
    * compareDocumentPosition()은 특정 노드가 다른 노드와 하나 이상의 고나계를 가질 수 있기 때문에, 다소 혼동될 수 있다.
        * 예를 들어, 노드가 포함 관계(16)이자 앞에 있는 경우(4), compareDocumentPostion()은 20을 반환혼다.
* 두 노드가 동일한지 판단하기
    * 두 노드는 다음 조건들이 만족되는 경우에만 동일하다.
        * 두 노드가 동일한 형식이다.
        * nodeName, localName, namespaceURI, prefix, nodeValue 문자열 특성이 동일하다.
            * 즉 둘다 null 이거나, 동일한 길이와 동일한 문자를 가져야 한다.
        * NamedNodeMaps 특성이 동일하다.
            * 즉 둘다 null 이거나 길이가 동일해야 하며, 하나의 맵 내에 존재하는 각 노드들과 다른 맵에 존재하는 노드가 동일해야 하되 인덱스가 동일할 필요는 없다.
        * childNodes NodeList가 동일하다.
            * 즉 둘다 null 이거나, 동일한 길이를 가지고 같은 인덱스의 노드가 동일해야 한다.
            * 정규화가 동일성에 영향을 미칠 수 있으므로, 이를 피하기 위해서는 비교를 수행하기 전에 노드를 정규화해야 한다.
    * DOM 내의 노드에 대해 isEqualNode() 메서드를 호풀하면, 매개변수로 전달하는 노드와 동일한지를 물어보게 된다.
    ```
    <input type='text'>
    <input type='text'>

    <textarea>foo</textarea>
    <textarea>bar</textarea>
    
    //

    var input = document.querySelectorAll('input');
    console.log(input[0].isEqualNode(input[1])); // true

    var textarea = document.querySelectorAll('textarea');
    console.log(textarea[0].isEqualNode(textarea[1])); // false
    ```
    * 두 노드가 완전히 동일한지가 아니라, 두 노드 참조가 동일한 노드를 참조하고 있는지를 알고 싶다면, === 연산자를 사용하여 간단하게 확인해볼 수 있다.
    * 이 연산자는 동일하지는 않아도 참조가 일치하는지를 알려준다.


## Document 노드
* 개요
    * HTMLDocument 생성자(document로부터 상속됨)는 DOM 내에 DOCUMENT_NODE(ex. window.document) 를 생성한다.
    * 이를 확인하려면, document 노드 개체 생성에 사용되는 생성자가 무엇인지를 물어보면 된다.
    ```
    console.log(window);
    console.log(window.document);
    console.log(window.document.constructor); // HTMLDocument(){ [native code]}
    console.log(window.document.nodeType); // DOCUMNET_NODE에 대한 숫자 키 매핑인 9가 출력된다.
    ```
    * 이 코드를 통해 HTMLDocumnet 생성자 함수가 window.document 노드 개체를 생성하며, 이 노드가 DOCUMENT_NODE 개체라는 결론이 내려진다.
    * Document 및 HTMLDocument 생성자는 보통 HTML 문서를 로드 시 브라우저에 의해 인스턴스가 만들어진다.
    * 하지만 document.implementation ,createHTMLDocumnet()를 사용하면, 브라우저 내에 현재 로드된 문서 외부에 직접 HTML 문서를 생성할 수 있다.
    * createHTMLDocument() 외에, createDocument() 를 사용하여 HTML 문서로 설정된 document 개체를 생성할 수 있다.
    * 이 메서드들을 사용하는 예로, 프로그래밍적인 방법으로 iframe에 HTML 문서를 제공하는 것을 들 수 있다.
* HTMLDocument의 속성 및 메서드
    * HTMLDocument 노드에 존재하는 속성 및 메서드와 관련된 정확한 정보를 얻어내려면, 사앙서는 무시하고 브라우저에 직접 물어보는 것이 가장 좋다.
    ```
    var documentPropertiesIncludeInherited = [];
    for (var p in document) {
        documentPropertiesIncludeInherited.push(p);
    }
    console.log('documentPropertiesIncludeInherited: ', documentPropertiesIncludeInherited);

    var documentPropertiesOnlyInherited = [];
    for (var p in document) {
        if (!document.hasOwnProperty(p)) {
            documentPropertiesOnlyInherited.push(p);
        }
    }
    console.log('documentPropertiesOnlyInherited: ', documentPropertiesOnlyInherited);


    ```
    * 수많은 속성이 존재하기에, 상속된 속성은 고려되지도 않았다.
    * 주목해야 할 속성과 메서드
        * doctype
        * documentElement
        * implementation.*
        * activeElement
        * body
        * head
        * title
        * lastModified
        * referrer
        * URL
        * defaultview
        * compatMode
        * ownerDocument
        * hasFoucs()
    * HTMLDocument 노드 개체는 DOM을 다룰 때 사용 가능한 수많은 메서드와 속성에 접근하는데 사용된다.

* 일반적인 HTML 문서 정보 얻기 (제목, url, referrer, 최종 수정일 ,호환 모드)
    * documnet 개체에 로드된 HTML 문서/DOM에 대한 일반적인 정보에 접근할 수 있게 해준다.
    ```
    var d = document;
    console.log('d.title: ', d.title);
    console.log('d.URL: ', d.URL);
    console.log('d.referrer: ', d.referrer);
    console.log('d.lastModified: ', d.lastModified);
    console.log('d.compatMode: ', d.compatMode);
    ```

* document 자식 노드
    * document 노드는 DocumentType 노드 개체 하나와 Element 노드 개체 하나를 가질 수 있다.
    * 통상적으로 HTML 문서는 하나의 doctype(ex. !DOCTYPE html)과 하나의 element(ex. html lang='en') 만을 가지므로, 별로 놀라울 것은 없다.
    * 따라서 document 개체의 자식에 대해 물어보면, 최소한 문서의 doctype/DTD와 html lang='en'이 포함된 배열을 얻게 될 것이다.
    ```
    console.log(document.childNodes);

    // doctype/DTD
    console.log(document.childNodes[0].nodeType); // DOCUMENT_TYPE_NODE를 의미하는 숫자 키 10이 출력된다.

    // <html> element
    console.log(document.childNodes[1].nodeType); // ELEMENT_TYPE_NODE를 의미하는 숫자 키 1 이 출력된다.
    ```
    * HTMLDocument 생성자에서 생성되는 window.document 개체와 Document 개체를 혼동해서는 안된다.
    * window.document가 DOM 인터페이스의 시작점이라는 것만 기억하자.
    * Document.childNodes가 자식 노드를 가지고 있는 이유가 바로 이때문이다. ????
    * comment 노드가 html lang='en' 외부에 있을 경우, window.document의 자식 노드가 된다.
    * 하지만 html element 외부에 comment 노드를 두는 것은 IE에서 잘못된 결과가 야기될 수 있으며, DOM 사양을 위반하는 것이다.

* document는 !DOCTYPE, html lang='en, haed, body에 대한 바로가기를 제공한다.
    * 다음 속성들을 사용하면, 각 노드에 대한 바로가기 참조를 얻을 수 있다.
        * documnet.doctype // !DOCTYPE
        * document.documentElement // html lang='en'
        * document.head // head
        * document.body // body
    * doctype은 DocumentType()으로 생성되고, window.document는 HTMLDocument()로 부터 생성되므로 혼동하지 말자.
* document.implementation.hasFeature()를 사용하여 DOM사양/기능 탐지하기
    * document.implementation.hasFeature()를 사용하면 현재 문서에 대해 브라우저가 구현/지원하는 기능 및 수준에 대해 물어볼 수 있다.
    ```
    console.log('document.implementation.hasFeature("Core","2.0"): ', document.implementation.hasFeature('Core', '2.0'));
    console.log('document.implementation.hasFeature("Core","3.0"): ',   document.implementation.hasFeature('Core', '3.0'));
    ``` 
    * 브라우저가 core DOM level 3 사양을 구현했는지 물어볼 수 있다.
    * hasFeature() 만을 신뢰하지 말고, 그 외에 탐지 기능을 함께 사용해야 한다.
    * isSupported 메서드를 사용하면, 특정/선택된 노드에 대한 구현 정보를 수집할 수 있다.

* 문서 내에서 포커스를 가지고 있거나 활성 상태인 노드에 대한 참조를 얻기
    * document.activeElement를 사용하면, 문서 내에서 포커스를 가지고 있거나 활성 상태인 노드에 대한 참조를 바로 얻을 수 있다.
    ```
    document.querySelector('textarea').focus(); // textarea에 focus 설정
    console.log(document.activeElement); // 문서 내에서 포커스를 가지고 있거나 활성 상태인 element에 대한 참조를 얻음
    ```
    * 포커스를 가지고 있거나 활성 상태인 element는 포커스를 받을 수 있는 element를 반환한다.
    * 브라우저는 웹 페이지를 방문해서 탭 키를 눌러보면, 페이지 내에서 포커스를 받을 수 있는 element들로 포커스가 전환되는 것을 볼 수 있다.
    * 노드를 선택하는 것, 키스트로크, 스페이스바, 마우스로 무엇인가를 입력하기 위해 포커스를 받은 element를 혼동하지 말자

* 문서 혹은 문서 내의 특정 노드가 포커스를 가지고 있는지 판별하기
    * document.hasFocus() 메서드를 사용하면, 사용자가 현재 해당 HTML 문서가 로드된 창에 포커스를 두고 있는지 알 수 있다.
    ```
    setTimeout(function() {
        console.log(document.hasFocus());
    }, 2000);
    ``` 
* document.defaultView는 최상위/전역 개체에 대한 바로가기다
    * defaultView 속성은 javascript 최상위 개체, 혹은 전역 개체라고 불리는 것에 대한 바로가기다.
    * 웹 브라우저에게 최상위 개체는 window 개체이므로, javascript 브라우저 환경에서 defaultView는 이 개체를 가리킨다.
    ```
    console.log(document.defaultView); // window 
    ```
    * 최상위 개채가 없는 DOM 이나 웹 브라우저 내에서 실행되지 않는 javascript 환경의 경우, 이 속성은 최상위 개체 영역에 접근할 수 있게 해준다.

* Element에서 ownerDocumnet를 사용하여 Document에 대한 참조 얻기
    * 노드에서 ownerDocument 속성을 호출하면, 노드가 포함된 documnet에 대한 참조를 반환한다.

## Element 노드
* HTMLElement 개체 개요
    * HTML 문서 내의 각 element들은 고유한 성질을 가진다.
    * 각자 element를 DOM 트리 내의 노드 개체로 인스턴스화하는 고유한 javascript 생성자를 가진다.
    * 예를 들어 a element는 HTMLAnchorElement() 생성자를 통해 DOM 노드로 만들어진다.
    ```
    // DOM에서 a element 노드를 가져와 생성하는 데 사용된 생성자의 이름을 물어본다
    console.log(document.querySelector('a').constructor);
    // function HTMLAnchorElement(){[native code]}
    ```
    * 이 코드의 요점은 DOM 에서 각 element가 고유한 javascript 인터페이스/생성자를 통해 만들어진다는 것이다. (HTMLHtmlElement, HTMLHeadElement, HTMLDivElement, ...)

* HTML*Element 개체의 속성 및 메서드
    * HTML*Element 노드에 존재하는 속성 및 메서드와 관련된 정확한 정보를 얻으려면 브라우저에게 물어보는 것이 제일 좋다.
    ```
    var anchor = document.querySelector('a');
    for (let p in anchor) {
        console.log(p);
    }
    ```
    * 주요 속성 및 메서드들의 목록
        * createElement()
        * tagName
        * children
        * getAttribute()
        * setAttribute()
        * hasAttribute()
        * removeAttribute()
        * classList()
        * dataset
        * attributes
* Element 생성
    * Element 노드는 브라우저가 HTML 문서를 해석해서 문서 콘텐츠를 기반으로 대응되는 DOM이 만들어질 때 인스턴스화 된다.
    * 이것 외에 createElement()를 사용하여 프로그래밍적으로 Element 노드를 생성할 수도 있다.
    ```
    var elementNode = document.createElement('textarea');
    // HTMLTextAreaElement()는 <textarea>를 생성한다.
    document.body.appendChild(elementNode);
    ```
    * createElement() 메서드에 전달되는 값은 생성할 Element의 형식을 지정하는 문자열이다.
    * createElement에 전달되는 값은 element가 생성되기 전에 소문자 문자열로 변경된다.

* Element의 태그 이름 얻기
    * tagName 속성을 사용하면, element의 이름에 접근할 수 있다.
    * tagName 속성은 nodeName이 반환하는 것과 동일한 값을 반환한다.
    * 원본 HTML 문서에서의 대소문자 여부에 관계없이 둘 다 값을 대문자로 반환한다.
    ```
    console.log(document.querySelector('a').tagName); // A
    console.log(document.querySelector('a').nodeName); // A
    ```  

* Elemnet의 Attribute 및 값에 대한 리스트/컬렉션 얻기
    * attribute 속성(element 노드가 Node로 부터 상속 받음)을 사용하면, 현재 element에 정의된 Attr 노드의 컬렉션을 얻을 수 있다.
    * 반환된 리스트는 NameNodeMap이다.
    ```
    var atts = document.querySelector('a').attributes;
    console.log(atts);

    for (var i = 0; i < atts.length; i++) {
        console.log(atts[i].nodeName + '=' + atts[i].nodeValue);
    }
    ```
    * attributes 속성을 통해 반환되는 배열은 라이브 상태라는 점을 고려해야 한다.
    * 이는 내용물이 언제든지 변경될 수 있다는 것을 의미한다.
    * 반환된 배열이 상속받고 있는 NameNodeMap은 getNamedItem(), setNamedItem(), removeNamedItem()과 같이 배열을 조작하기 위한 메서드를 제공한다.
    * 이 메서드들을 사용하여 attributes를 조작하는 것보다는 getAttribute(), setAttribute(), hasAttribute(), removeAttribte()를 사용하는 것이 좋다.
    * attributes를 사용할 때의 유일한 이점은 라이브 상태의 attributes 목록을 반환한다는 것뿐이다.
    * attributes 속성은 유사 배열 컬렉션이며, 읽기 전용인 length 속성을 가진다.

* Element의 Attribute 값 획득, 설정, 제거
    * element의 attribute 값을 가져오고, 설정 및 제거하기 위한 가정 일관된 방법은 getAttribute(), setAttribute(), hasAttribute(), removeAttribte() 메서드를 사용하는 것이다.
    * setAttrubute()를 사용하여 값을 null이나 '' 로 설정하지 말고 removeAttribute()를 사용해야한다.
    * 일부 element attribute는 element 노드에서 개체 속성으로 존재한다.
        * ex. document.body.id, ...
    * 이 속성을 사용하지 말고 메서드를 사용하는 것을 권장한다.
    * setAttribute는 값이 정의되어 있는지에 여부에 관계 없이, 존재 여부에 따라 true/false를 반환한다.

* Class Attribute 값 리스트 얻기
    * element 노드에 존재하는 classList 속성을 사용하면 className 속성에서 반환되는 공백으로 구분된 문자열 값을 사용하는 것보다 훨씬 쉽게 class attribute 값 리스트에 접근할 수 있다.
    ```
    var elm = document.querySelector('div');

    console.log(elm.classList); // {0='big', 1='brown', 2='bear, length: 3, value: 'big brown bear', __proto__: DOMTokenList}
    console.log(elm.className); // big brown bear
    ```
    * classList는 유사 배열 컬렉션이며, 읽기 전용인 length 속성을 가진다.
    * classList는 읽기 전용이지만 add(), remove(), contians(), toggle() 메서드를 사용해서 변경할 수 있다.
    * IE10 부터 지원한다.

* data-* attribute를 가져오고 설정하기
    * element 노드의 dataset 속성은 element에서 data-로 시작하는 모든 attribute를 가진 개체를 제공해준다.
    * 이 개체는 javascript 개체이므로, dataset을 조작해 DOM 내의 element에 변경 내용을 반영할 수 있다.
    ```

    console.log(el.dataset) // DOMStringMap {foo='foo'}
    console.log(el.dataset.foo) // foo
    el.dataset.foo ='bar'
    ```
    * dataset은 data attribute들의 camelCase 버전을 가지고 있다.
    * 즉 data-foo-foo는 dataset DOMStringMap 개체 내에 fooFoo 라는 속성으로 나열된다.
    * 하이픈은 camelCasing으로 대체된다.
    * DOM에서 data-* attribute를 제거하려면, dataset의 속성에 대해 delete 연산자를 사용하면 되낟.
        * delete dataset.foo
    * IE10 부터 제공된다.
    
## Element 노드 선택
* 특정 Element 노드 선택하기
    * 단일 element 노드에 대한 참조를 얻는 데 가장 흔히 사용되는 메서드는 다음과 같다.
        * querySelector()
        * getElementById()
    * getElementById() 메서드는 querySelector() 메서드에 비해 매우 단순하다.
    * querySelector() 메서드는 CSS selctor 문법 형식의 매개변수를 허영한다.
    * 이 메서드에 CSS selctor (ex. #score>tbody>tr>td:nth-of-type(2)) 를 전달하면, DOM에서 단일 element를 선택하는 데 사용된다.
    * querySelector() 는 selector를 기반으로 문서 내에서 발견되는 첫 번째 노드 element를 반환한다.
    * querySelector() 는 element 노드에도 정의되어 있다.
        * 그 덕분에 메서드의 결과를 DOM 트리의 특정 부분에 한정할 수 있어서 상호아에 맞는 쿼리를 할 수 있게 해준다.

* Element 노드 리스트 선택 및 생성하기
    * HTML 문서 내의 node list를 선택 및 생성하는 데 가장 흔히 사용되는 메서드는 다음과 같다.
        * querySelectorAll()
        * getElementsByTagName()
        * getElementsByClassName()
    * getElementsByTagName(), getElementsByClassName() 으로 생성된 node list는 라이브 상태로 간주되며, 리스트를 생성하고 선택한 후에 문서가 자동으로 업데이트된 경우에도 문서의 상태를 항상 반영한다.
    * querySelectorAll() 메서드는 라이브 상태의 element 리스트를 반환하지 않는다.
        * querySelectorAll() 에서 반환하는 리스트는 리스트 생성 시점의 문서 스냅샷이며, 문서의 변경 내용을 반영하지 않는다는 것을 의미한다.
        * 해당 리스트는 정적이며 라이브 상태가 아니다.
    * querySelectorAll(), getElementsByTagName(), getElementsByClassName() 는 element 노드에도 정의되어 있다.
    * 인자로 문자열 '*' 을 전달하면, 문서 내의 모든 element 리스트를 ㅏㄴ환한다.
    * childNodes도 NodeList를 반환한다는 것을 명심하라
    * NodeList는 유사 배열 리스트/컬렉션이며, 읽기 전용인 length 속성을 가진다.

* 직계 자식 Element 노드를 모두 선택하기
    * element 노드에서 children 속성을 사용하면, element 노드의 직계 자식 노드 전체 리스트를 얻을 수 있다.
    * children은 직계 element 노드만을 제공하며, element가 아닌 노드는 제외된다.
    * element에 자식이 없는 경우, children은 빈 유사 배열 리스트를 반환한다.
    * HTMLCollection은 element를 문서 내의 순서대로 가진다.
        * 즉, element가 DOM에 나타나는 순서대로 배열 내에 위치한다.
    * HTMLCollection은 라이브 상태이므로, 문서가 변경되면 동적으로 컬렉션에 반영된다.

* 컨텍스트 기반 Element 선택
    * 통상적으로 document 개체를 통해 접근되는 querySelector(), querySelctorAll(), getElementsByTagName(), getElemensByClassName()은 element 노드에도 정의되어 있다.
    * 이는 해당 메서드의 결과를 DOM 트리의 특정 부분으로 제한할 수 있게 해준다.
    * 이 메서드들은 라이브 상태의 DOM에서만 동작하는 것이 아니라, 코드로 생성한 DOM 구조에서도 동작한다.
    ```
    var divEl = document.createElement('div');
    var ulEl = document.createElement('ul');
    var liEl = document.createElement('li');
    liEl.setAttribute('class', 'liClass');
    ulEl.appendChild(liEl);
    divEl.appendChild(ulEl);

    console.log(divEl.querySelector('li'));
    ```

* 사전에 구성된 Element 노드 선택/리스트
    * HTML 문서에서 element 노드를 포함하고 있는 사전 구성된 유사 배열 리스트가 몇 개 존재한다는 것을 알아두자.
        * document.all -> 문서 내의 모든 element
        * document.fomrs, images, links, scripts, styleSheets

* 선택될 Element를 검증하기 위해 matchesSelector()를 사용하기
    * matchesSelector() 메서드를 사용하면, element가 selector 문자열에 들어맞는지를 판별할 수 있다.
    * 예를 들어, li가 ul의 첫 번째 자식 element인지를 판별하고 싶다고 가정해보자.
    * ul 내의 첫 번째 li를 선택한 후, 이 element가 selector인 li:first-child에 들어맞는지를 물어본다.
    ```
    console.log(document.querySelector('li').matches('li:first-child')); // true
    ```

## Element 노드 지오메트리와 스크롤링 지오메트리
* Element 노드 크기, 오프셋, 스크롤링 개요
    * HTML 문서를 웹 브라우저에서 볼 때, DOM 노드가 해석되어 시각적인 모양으로 그려진다.
    * 노드(대부분 element노드)는 브라우저에서 볼 수 있는 시각적인 표현 형태를 가지고 있다.
    * 노드의 시각적인 형태와 지오메트리를 프로그래밍을 통해 살펴보고 조작하기 위해 일련의 API들이 존재하는데 CSSOM View Module에 정의되어 있다.
    * 이 사양서에서 찾아볼 수 있는 메서드 및 속성의 하위 집합에서는 element 노드의 지오메트리를 측정하는 것뿐만 아니라, 스크롤이 가능한 노드를 조작하기 위해 가로채고, 스크롤된 노드의 값을 가져오기 위한 API도 제공한다.
    * 이번 장에서는 이러한 메서드와 속성들을 하나씩 살펴본다.
    * CSSOM View Moduel 사양에 있는 대부분의 속성(scrollLeft와 scrollTop은 제외) 는 읽기 전용이며 접근 시마다 매번 계산된다.
        * 즉 이 값들은 라이브 상태다.
* offsetParent를 기준으로 elemnt의 offsetTop 및 offsetLeft 가져오기
    * offsetTop 및 offsetLeft 속성을 사용하면, offsetParent 로부터 element 노드의 오프셋 픽셀 값을 가져올 수 있다.
    * 이 element 노드 속성들은 element의 바깥쪽 좌상단 경계로부터 offsetParent의 안쪽 좌상단 경계까지의 거리를 픽셀로 제공해준다.
    * offsetParent의 값은 가장 가까운 부모 element 중에서 CSS 위치 값이 static이 아닌 element를 검색하여 결정된다.
    * 아무 element도 발견되지 않으면, offsetParent의 값은 body element나 document에 해당하는 것(브라우저의 viewport와는 다름)이 된다.
    * 부모를 탐색하는 동안 CSS 위치 값이 static인 td, th, table element가 발견되면, 이 element가 offsetParent 값이 된다.
    ```
    var div = document.querySelector('#red');
    console.log(div.offsetLeft); // 60
    console.log(div.offsetTop); // 60
    console.log(div.offsetParent); // <body>
    ```
    * 큰 사각형이 절대위치를 가지지 않으므로 작은 사각형은 body를 parent로 가진다.
    * 큰 사각형을 position: absoulte로 바꾸면 25, 25가 출력된다.
    * 대부분의 브라우저에서는 offsetParent가 body이고 body나 html element가 눈에 보이는 margin, padding, border 값을 가지는 경우 바깥쪽 테두리에서 안쪽 테두리까지의 측정이 제대로 되지 않는다.
    * offsetParent, offsetTop, offsetLeft는 HTMLElment 개체의 확장이다.

* getBoundingClinetRect()를 사용하여 뷰포트를 기준으로 element의 Top, Right, Bottom, Left 테두리 오프셋을 얻기
    * getBoundingClientRect() 메서드를 사용하면, Viewport의 좌상단 끝을 기준으로 elemnt가 브라우저에서 그려질 때 elemnt의 바깥쪽 테두리의 위치를 얻을 수 있다.
    * left 및 right는 elemtn의 바깥쪽 테두리로부터 뷰포트의 왼쪽 끝까지로 측정되며, top과 bottom은 element의 바깥 테두리로부터 뷰포트의 상단 끝까지로 측정된다.
    ```
    var divEdges = document.querySelector('div').getBoundingClientRect();
    console.log(divEdges); // top, right, bottom, left, height, width, x, y
    console.log(divEdges.top, divEdges.right, divEdges.bottom, divEdges.left);
    ```
   

* 뷰포트에서 element의 크기 얻기(테두리 + 패딩 + 내용)
    * divEdges.height, divEdges.width
    * offsetHeight, offsetWidth

* 뷰포트에서 테두리를 제외한 element의 크기(패딩 + 내용)
    * clientWidth, clientHeight

* elementFromPoint()를 사용하여 뷰포트의 특정 지점에서 최상단 element 얻기
    * HTML 문서의 특정 지점에서 최상단 element에 대한 참조를 얻을 수 있다.
    ```
    console.log(document.elementFromPoint(50, 50)); // <div id='top'></div> 가 출력됨
    ```

* scrollHeight와 scrollWidth를 사용하여 스크롤될 element의 크기를 얻기
    * 스크롤될 노드의 높이와 너비를 제공해준다.
    * 예를 들어, 웹 브라우저에서 스크롤되는 HTML 문서를 열고 html이나 body의 속성에 접근하면, 스크롤될 HTML 문서의 전체 크기를 얻을 수 있다. 
    * CSS를 사용하여 element에 스크롤을 적용(ex. overflow: scroll)할 수 있다.
    ```
    var div = document.querySelector('div');
    console.log(div.offsetWidth); // 100
    console.log(div.scrollHeight, div.scrollWidth); // 1000 1000
    ```
    * 스크롤 가능한 영역 내에 있는 노드가 스크롤 가능한 영역의 뷰포트보다 작은 경우에 해당 노드의 높이와 너비를 알아야 한다면, scrollHeight와 scrollWidth는 뷰포트의 크기를 반환하므로 사용하지 않는 것이 좋다.
    * 스크롤될 노드가 스크롤 영역보다 작은 경우, 스크롤 가능한 영역 내에 포함된 노드의 크기를 판별하려면 clientHeight, clientWidth를 사용한다.

* scrollTop과 scrollLeft를 사용하여 top 및 left로부터 스크롤될 픽셀을 가져오거나 설정하기
    * 스크롤 때문에 현재 뷰포트에서 보이지 않는 left나 top까지의 픽셀을 반환한다.
    ```
    var div = document.querySelector('div');

    div.scrollTop = 750;
    div.scrollLeft = 750;

    console.log(div.scrollTop, div.scrollLeft);
    ```

* scrollIntoView()를 사용하여 element를 View로 스크롤하기
    * 스크롤이 가능한 노드 내에 있는 노드를 선택하면, scrollIntoView() 메서드를 사용하여 선택된 노드가 view로 스크롤되도록 할 수 있다.
    ```
    document.querySelector('content').children[4].scrollIntoView(true);
    ```
    * scrollIntoView() 메서드에 매개변수 true를 전달하면 해당 메서드로 하여금 스크롤될 대상 element의 top으로 스크롤하라는 것이다.
    * false면 element의 bottom으로 스크롤한다.


## Element 노드 인라인 스타일
* style attribute 개요
    * 모든 HTML element는 해당 element에 한정된 인러인 CSS 속성을 넣는 데 사용할 수 있는 Style attribute를 가진다.
    ```
    var divStyle = document.querySelector('div').style;
    console.log(divStyle);
    ```
    * 코드에서 style 속성이 문자열이 아닌 CSSStyleDeclaration 개체를 반환한다는 점에 유의한다.
    * 또한 CSSStyleDeclaration 개체에는 element의 안리인 스타일만이 포함된다.

* 개별 인라인 CSS 속성 가져오기, 설정, 제거
    * 인라인 CSS 스타일은 element 노드 개체에 존재하는 style 개체의 속성으로 각자 표현된다.
    * 단순히 개체의 속성 값을 설정하는 것으로 element의 개별 CSS 속성을 가져오거나 설정, 제거하는 인터페이스가 제공되는 것이다.
    * style 개체에 포함된 속성명에는 CSS 속성명에서 사용되는 일반적인 하이픈이 포함되지 않는다.
    * 변환 규칙은 매우 간단한데, 하이픈을 제거하고 카멜 케이스를 사용하면 된다.
    * 측정 단위가 필요한 속성의 경우, 적절한 단위를 포함 시켜야 한다.
    * style 개체는 CSSStyleDeclaration 개체로 개별 CSS 속성에 대한 접근뿐만 아니라 element 노드의 개별 CSS 속성을 조작하는 데 사용되는 setPropertyValue(propertyName, value), getPropertyValue(propertyName), removeProperty(propertyName) 메서드에 대한 접근도 제공해준다.
    ```
    var div = document.querySelector('div');
    var divStyle = div.style;
    divStyle.setProperty('width', '100px');
    divStyle.setProperty('height', '100px');
    divStyle.removeProperty('border');
    ```
    * setProperty()와 getPropertyValue() 메서드에 전달되는 속성명은 하이픈이 포함된 CSS 속성명을 사용한다.

* 모든 인라인 CSS 속성 가져오기, 설정, 제거
    * CSSStyleDeclaration 개체의 cssText 속성과 getAttribute() 및 setAttribute() 메서드를 사용하면, javascript 문자열을 사용하여 style attribute의 전체 값을 가져오고, 설정 및 제거할 수 있다.
    ```
    var div = document.querySelector('div');
    var divStyle = div.style;

    divStyle.cssText = 'background-color:red; border:1px solid black; height:100px; width:100px';
    console.log(divStyle.cssText);
    divStyle.cssText = '';

    div.setAttribute('style', 'background-color:red; border:1px solid black; height:100px; width:100px');
    console.log(div.getAttribute('style'));
    div.removeAttribute('style');
    ```
    * style attribute 값을 새로운 문자열로 바꾸는 것은 element의 style에 여러 변경을 수행하는 가장 빠른 방법이다.

* getComputedStyle()을 사용하여 element의 계산된 스타일 가져오기
    * style 속성은 style attribute를 통해 정의된 CSS만을 가지고 있다.
    * element의 계층화된 CSS (즉 인라인 스타일시트, 외부 스타일시트, 브라우저 스타일시트가 계층화된 것)를 가져오려면, getComputedStyle()을 사용한다.
    * 이 메서드는 style과 유사한 읽기 전용의 CSSStyleDeclaration 개체를 제공한다.
    ```
    var div = document.querySelector('div');
    console.log(window.getComputedStyle(div).backgroundColor);
    console.log(window.getComputedStyle(div).border);
    console.log(window.getComputedStyle(div).height);
    console.log(window.getComputedStyle(div).width);
    ```
    * getComputedStyle() 메서드는 CSS 특수 계층을 준수한다.
    * 예를 들어, 이 코드에서 div의 backgroundColor는 red가 아닌 green 인데, 인라인 스타일이 특수 계층의 최상위에 있기 때문이다.
    * getComputedStyle()에서 반환되는 CSSStyleDeclaration 개체는 읽기 전용이다.
    * getComputedStyle() 메서드는 색상 값을 원래 지정된 방식에 상관없이 rgb 형식으로 반환한다.
    * CSSStyleDeclaration 개체에서 단축 속성은 계산되지 않으므로, 속성 접근 시 비단축 속성명을 사용해야 한다. 
        * ex. margin이 아니라 marginTop을 사용

* class 및 id attribute를 사용하여 element의 CSS 속성을 적용 및 제거하기
    * 인라인 스타일시트나 외부 스타일시트에 정의된 스타일 규칙은 class 및 id attribute를 사용하여 element에 추가하거나 제거할 수 있다.
    * element 스타일을 조작할 때 가장 일반적인 패턴이다.
    ```
    var div = document.querySelector('div');

    div.setAttribute('id', 'bar');
    div.classList.add('foo');

    div.removeAttribute('id');
    div.classList.remove('foo');
    ```

## Text 노드
* 개요
    * HTML 문서에서 텍스트는 text 노드를 만들어내는 Text() 생성자 함수의 인스턴스로 표현된다.
    * HTML 문서가 해석될 때, HTML 페이지의 element 사이에 섞여있는 텍스트는 text 노드로 변환된다.
    ```
    var textHi = document.querySelector('p').firstChild;
    console.log(textHi.constructor);
    ```
    * 이 코드가 Text() 생성자 함수가 text 노드를 생성한다는 결론을 내려주지만, Text가 CharacterData, Node, Object로부터 상속받는다는 점을 명심해야 한다.

* Text개체 및 속성
    * 주요 속성 및 메서드
        * textContent
        * splitText()
        * appendData()
        * deleteData()
        * insertData()
        * replaceData()
        * subStringData()
        * normalize()
        * data
        * document.createTextNode() // text 노드의 속성도 상속받은 속성도 아니다.

* 공백도 text 노드를 생성한다.
    * 브라우저에 의해서나 프로그래밍 수단에 의해서 DOM이 생성될 때, 텍스트 문자뿐만 아니라 공백 역시 text 노드로 만들어진다.
    * 결국 공백도 문자이기 때문이다.
    ```
    <p id='p1'></p>
    <p id='p2'> </p>

    console.log(document.querySelector('#p1').firstChild); // null
    console.log(document.querySelector('#p2').firstChild); // " "
    ```
    * DOM에서 공백이나 텍스트 문자가 보통 text 노드로 표현된다는 것을 잊지 말라.
* text 노드 생성 및 삽입하기
    * text 노드는 브라우저가 HTML 문서를 해석하서 문서 내용을 기반으로 DOM이 구축될 때 자동적으로 생성된다.
    * 자동으로 생성된 이후에는 createTextNode()를 사용해서 프로그래밍적으로 text 노드를 생성할 수도 있다.
    ```
    var textNode = document.createTextNode('Hi');

    document.querySelector('div').appendChild(textNode);

    console.log(document.querySelector('div').innerText);
    ```
    ```
    var el = document.createElement('p');
    var textNode = document.createTextNode('Hi');

    el.appendChild(textNode);

    document.querySelector('div').appendChild(el);
    ```
* data나 nodeValue로 text 노드 값 가져오기
    * Text 노드로 표현되는 텍스트 값과 데이터는 .data나 nodeValue 속성을 사용하여 노드에서 추출할 수 있다.
    ```
    const a = document.querySelector('p').firstChild.data; // Hi,
    const b = document.querySelector('p').firstChild.nodeValue; // Hi,
    console.log(a, b);

    const c = document.querySelector('p').firstChild.innerText; // undefined
    const d = document.querySelector('p').firstChild.innerHtml; // undefined
    const e = document.querySelector('p').firstChild.textContent; // "Hi, "
    console.log(c, d, e);

    const f = document.querySelector('p').innerText; // Hi, cody
    const g = document.querySelector('p').innerHtml; // undefined
    const h = document.querySelector('p').textContent; // Hi, cody
    console.log(f, g, h);
    ```
    * p에 포함된 첫 번째 자식 노드 값만을 가져온다.
    * Text 노드에 포함된 문자의 길이를 가져오려면, 노드 자체 또는 노드의 실제 텍스트 값/데이터의 length 속성에 접근하면 된다.


* appendData(), deleteData(), insertData(), replaceData(), subStringData()로 text 조작하기
    * Text 노드가 메서드를 상속받는 CharacterData 개체는 Text 노드의 하위 값을 조작하고 추출하기 위한 메서드를 제공한다.
    ```
    var pEl = document.querySelector('p').firstChild;
    console.log(pEl);

    pEl.appendData('!');
    pEl.deleteData(7, 5);
    pEl.insertData(7, 'Blue ');
    pEl.replaceData(7, 5, 'z ');
    console.log(pEl.substringData(7, 10));
    ```

* 복수의 형제 텍스트 노드가 발생하는 경우
    * 브라우저에서 생성한 DOM 트리가 지능적으로 텍스트 노드들을 결합하기에, 통상적으로는 형제 Text 노드가 인접해서 나타나지 않는다.
    * 하지만 형제 텍스트 노드가 발생 가능한 두 가지 경우가 존재한다.
        * 텍스트 노드가 Element 노드를 포함하면 (ex. p Hi, strong cody /strong welcome! /p), 텍스트가 적절한 노드 그룹으로 분할된다.
        ```
        var pEl = document.querySelector('p');
        console.log(pEl.childNodes.length);
        console.log(pEl.firstChild.data);
        console.log(pEl.firstChild.nextSibling);
        console.log(pEl.lastChild.data);
        ```
        * 두 번째는 코드로 생성한 element에 프로그래밍적으로 text 노드를 추가할 때 발생한다.
        ```
        var p = document.createElement('p');
        var a = document.createTextNode('Hi, ');
        var b = document.createTextNode('Bye!');

        p.appendChild(a);
        p.appendChild(b);

        document.querySelector('div').appendChild(p);
        console.log(document.querySelector('div p').childNodes.length); // 2
        ```

* textContent를 사용하여 마크업이 제거된 모든 자식 텍스트 노드 반환하기
    * textContent 속성은 모든 자식 텍스트 노드를 가져오는 것뿐만 아니라, 노드의 내용을 특정 Text 노드로 설정하는데도 사용될 수 있다.
    * 노드의 텍스트 내용을 가져오기 위해 해당 속성을 사용하면, 메서드를 호출하는 데 노드에 포함된 모든 텍스트 노드의 문자열 을 합쳐서 반환된다.
    * 이 기능은 HTML 문서에서 모든 텍스트 노드를 매우 쉽게 추풀할 수 있게 해준다.
    ```
    console.log(document.body.textContent);
    ```
    * 노드 내에 포함된 텍스트를 설정하는데 textContent를 사용하면, 모든 자식 노드가 제거되고 단일 Text 노드로 바뀐다.
    ```
    document.body.textContent = '-';
    ```
    * textContent는 document나 doctype 노드에서 사용될 경우 null을 반환한다.
    * script 및 style element의 경우에는 내용이 반환된다.

* textContent와 innerText간의 차이
    * innertText에는 CSS가 반영된다.
        * 즉 숨겨진 텍스트가 있을 경우 innerText는 이 텍스트를 무시하는 반면, textContent는 무시하지 않는다.
    * innerText는 CSS의 영향을 받으므로 리플로우가 발생되는 반면, textContent는 그렇지 않다.
    * innerText는 script와 style element 내에 포함된 text 노드를 무시한다.
    * textContent와 달리 innerText는 텍스트를 정규화해서 반환한다.
        * textContent는 문서내에 있는 것을 마크업만 제거해서 그대로 반환하는 것으로 생각하면 된다.
        * 여기에는 공백, 줄바꿈, 개행 문자가 포함된다.
    * innerText는 비표준이고 브라우저에 국한된 것으로 간주되지만, textContent는 DOM 사양으로 구현되었다.

* normalize()를 사용하여 형제 텍스트 노드들을 단일 텍스트 노드로 결합하기
    * 형제 Text 노드들은 통상적으로 텍스트를 DOM에 프로그래밍적으로 추가한 경우에만 나타난다.
    * Element 노드를 포함하고 있지 않은 형제 Text 노드들을 제거하기 위해, normalize()를 사용할 수 있다.
    * 이 메서드는 DOM 내의 형제 텍스트 노드들을 단일 Text 노드로 결합한다.

* splitText()를 사용하여 텍스트 노드를 분할하기
    * 해당 텍스트 노드를 변경하고 옵셋을 기반으로 원래 텍스트에서 분할된 텍스트를 가진 새로운 Text 노드를 반환한다.
    ```
    var a = document.querySelector('p');
    console.log(a.firstChild.splitText(6).data); // world
    console.log(a.firstChild.textContent); // hello
    a.normalize();
    console.log(a.firstChild.textContent); // hello world
    ```

## DocumentFragment
* 개요
    * 라이브 DOM 트리 외부에 경량화된 문서 DOM을 만들 수 있다.
    * DocumentFragment는 마치 라이브 DOM 트리처럼 동작하되, 메모리상에서만 존재하는 빈 문서 템플릿으로 생각하면 된다.
    * DocumentFragment의 자식 노드를 메모리상에서 손쉽게 조작한 후, 라이브 DOM에 추가하는 것도 가능하다.

* createDocuementFragment()를 사용하여 DocumentFragment를 생성하기
    ```
    var docFrag = document.createDocumentFragment();
    [ 'a', 'b', 'c', 'd', 'e' ].forEach(function(e) {
        var li = document.createElement('li');
        li.textContent = e;
        docFrag.appendChild(li);
    });
    console.log(docFrag.textContent); // abcde
    ```
    * DocumentFragment를 사용하여 메모리상에서 노드 구조를 만들고 이를 라이브 노드 구조에 삽입하면 매우 효율적이다.
    * 이점
        * DocumentFragment는 어떤 종류의 노드도 가질수 있는 반면, element는 그렇지 않다.
        * DocumentFragment를 DOM에 추가하더라도 DocumentFragment 자체는 추가되지 않으며, 노드의 내용만이 추가된다.
            * element를 추가할 경우에는 element 자체도 추가 동작에 속하게 된다.
        * DocumentFragment를 DOM에 추가할 때, DocumentFragment는 추가되는 위치로 이전되며, 생성한 메모리상의 위치에 더 이상 존재하지 않는다.
            * 노드를 포함하기 위해 일시적으로 사용된 후 라이브 DOM 으로 이동되는 element 노드는 그렇지 않다.

* DocumentFragment를 라이브 DOM에 추가하기
    * appendChild()와 insertBeofre() 노드 메서드의 인수로 DocumentFragment를 전달하면, DocumentFragment의 자식 노드는 메서드가 호출되는 DOM 노드의 자식 노드로 옮겨진다.
    ```
    var docFrag = document.createDocumentFragment();
    [ 'a', 'b', 'c', 'd', 'e' ].forEach(function(e) {
        var li = document.createElement('li');
        li.textContent = e;
        docFrag.appendChild(li);
    });
    console.log(docFrag.textContent); // abcde
    document.body.appendChild(docFrag);
    console.log(docFrag.textContent); // 
    ```
    * 노드를 삽입하는 메서드에 DocumentFragment를 인수로 전달하면, 자식 노드 구조 전체가 삽입되며 DocumentFragment노드 자체는 무시된다.

* DocumentFragment에서 innerHTML 사용하기
    * 노드 메서드를 사용하여 메모리상에서 DOM 구조를 생성하는 것은 지루한데다 많은 노력을 필요로 한다.
    * 이를 피하기 위한 방법 중 하나느 DocumentFragment를 생성해서 div를 추가(DocumentFragment에서는 innerHTML이 동작하지 않음)한 다음, HTML 문자열로 갱신하기 위해 innerHTML 속성을 사용하는 것이다.
    * 그러면 결과적으로 HTML 문자열로부터 DOM 구조가 만들어진다.
    ```
    var divEl = document.createElement('div');
    var docFrag = document.createDocumentFragment();

    docFrag.appendChild(divEl);

    docFrag.querySelector('div').innerHTML = '<ul><li>foo</li><li>bar</li></ul>';
    console.log(docFrag.querySelectorAll('li').length); // 2
    ```
    * DOMParser가 추가되기를 기대하고 있다.
    * DOMParser는 문자열로 저장된 HTML을 DOM 문서로 해석할 수 있다.

* 복제를 사용하여 Fragment의 노드를 메모리상에서 유지하기
    * DocumentFragment를 추가하면, Fragment 내에 포함된 노드들을 추가하는 구조로 이동된다.
    * 노드를 추가한 이후에도 Fragment 의 내용을 메모리상에서 유지하려면, cloneNode()를 사용하여 복제하면 된다.
    ```
    var ulEl = document.querySelector('ul');
    var docFrag = document.createDocumentFragment();

    [ 'a', 'b', 'c', 'd', 'e' ].forEach(function(e) {
        var li = document.createElement('li');
        li.textContent = e;
        docFrag.appendChild(li);
    });

    ulEl.appendChild(docFrag.cloneNode(true));

    console.log(document.querySelector('ul').innerHTML);
    docFrag.removeChild(docFrag.childNodes[0]);
    console.log(docFrag.childNodes.length); // 4
    ```

## CSS 스타일시트와 CSS 규칙
* CSS 스타일시트 개요
    * 스타일시트는 외부 스타일시트를 포함하도록 HTMLLinkElement 노드를 사용하거나 인라인 스타일시트를 정의하도록 HTMLStyleElement 노드를 사용하여 HTML 문서에 추가된다.
    * 다음 HTML 문서에는 DOM 내에 이 두 가지 Element 노드가 존재하며, 해당 노드들을 생성하는 생성자를 검증한다.
    ```
    console.log(document.querySelector('#linkElement').constructor);
    console.log(document.querySelector('#styleElement').constructor);
    ```
    * 스타일시트가 HTML문서에 추가되면 CSSStylesheet 개체로 표현되며, 스타일시트 내부의 각 CSS 규칙은 CSSStyleRule 개체로 표현된다.
    ```
    console.log(document.querySelector('#linkElement').sheet);
    console.log(document.querySelector('#styleElement').sheet);
    console.log(document.querySelector('#linkElement').sheet.constructor);
    console.log(document.querySelector('#styleElement').sheet.constructor);
    ```
    * 스타일시트를 포함하고 있는 element를 선택하는 것은 스타일 시트 자체를 표현하느 ㄴ실제 개체에 접근하는 것과 동일하지 않다는 점에 유의한다.

* DOM 내의 모든 스타일시트에 접근하기
    * document.styleSheets는 HTML 문서 내에 명시적으로 연결(link) 되거나 내장(style)된 모든 스타일시트 개체 리스트에 접근할 수 있게 해준다.
    ```
    console.log(document.styleSheets);
    console.log(document.styleSheets.length);
    console.log(document.styleSheets[0]);
    console.log(document.styleSheets[1]);
    ```
    * styleSheets는 다른 노드 리스트와 마찬가지로 라이브 상태다.
    * DOM 내의 element를 선택한 후 .sheet 속성을 사용하여 CSSStylesheet 개체에 접근할 수도 있다.

* CSSStyleSheet의 속성 및 메서드
    * styleSheets 리스트나 .sheet 속성을 통해 접근 가능한 주요 속성과 메서드
        * disabled
        * href
        * media
        * ownerNode
        * parentStylesheet
        * titlt
        * tyle
        * cssRules
        * ownerRule
        * deleteRule
        * insertRule
    * href, media, ownerNode, parentStyleSheet, title, type은 읽기 전용

* CSSStyleRule 개요
    * 스타일시트에 포함된 각 CSS 규칙을 표현한다.
    * 기본적으로 CSSStyleRule은 선택자에 연결되는 CSS 속성과 값에 대한 인터페이스다.
    ```
    var styleSheet = document.querySelector('#styleElement').sheet;
    console.log(styleSheet.cssRules[0].cssText);
    ```

* CSSStyleRule의 속성 및 메서드
    * 주요
        * cssText
        * parentRule
        * parentStylesheet
        * selectorText
        * style
        * type

## DOM에서의 javascript
* 개요
    * 외부 javascript 파일을 포함시키거나 페이지 수준의 인라인 javascript를 작성하여 HTML 문서 내에 javascript를 삽입할 수 있다.

* 기본적으로 javascript는 동기 방식으로 해석됨
    * DOM이 해석될 때 script element를 만나게 되면, 문서 해석을 중지하고, 렌더링 및 다운로드를 차단한 후 javascript를 실행한다.
    * 이 동작은 블로킹을 발생시키며, DOM 해석이나 javascript 실행을 병렬적으로 수행할 수 없게 하므로, 동기 방식이라고 생각하면 된다.
    * javascript가 HTML 문서 외부에 있는 경우 블로킹이 더 심해지는데, javascript를 해석하기 전에 먼저 다운로드를 해야 하기 때문이다.
    * 로딩 단계와 관련하여 인라인 스크립트와 외부 스크립트 간의 차이점에 유의해야 한다.

* 외부 javascript의 다운로드 및 실행을 지연시키기 위해 defer를 사용하기
    * script element는 브라우저가 html 노드를 해석할 때까지 외부 javascript 파일의 블로킹, 다운로드, 실행을 지연시켜주는 defer라는 attribute를 가진다.
    * 이 attribute를 사용하면, 일반적으로 웹 브라우저가 script 노드를 만나게 될 때 발생하는 것들을 간단하게 지연시킬 수 있다.
    * defer를 사용할 경우, 지연되는 javascript 내에서는 document.write()가 사용되지 않는다고 가정한다.

* async를 사용하여 외부 javascript 다운로드 및 실행을 비동기로 수행하기
    * script element는 웹 브라우저가 DOM을 생성할 때 script element 의 순차적인 블로킹 특성을 재정의하기 위한 async라는 attribute를 가지고 있다.
    * 이 attribute를 사용함으로써 브라우저에 HTML 페이지의 생성 (즉 이미지, 스타일시트와 같은 다른 항목의 다운로드를 포함한 DOM 해석)을 차단하지 않아야 하며, 순차적 로딩 역시 하지 말라고 알려주는 것이다.
    * async attribute를 사용하면, 파일들이 병렬적으로 로드되며 다운로드가 끝난 순서대로 해석된다.
    * IE 10에서 부터 지원된다.
    * async attribute를 사용할 때의 주요 단점은 javascript 파일이 DOM에 포함된 순서와 다르게 해석된다는 것이다.
    * 이는 종속성 관리 문제를 유발한다.
    * async를 사용 시, 지연되는 javascript 내에서는 document.write()가 사용되지 않는다고 가정하낟.
    * async와 defer가 둘 다 사용된 경우에는 asynv가 우선하게 된다.

* 외부 javascript의 비동기 다운로드 및 해석을 강제화하기 위한 동적 script element의 사용하기
    * async attribute를 사용하지 않고 웹 브라우저에 javascript의 비동기 다운로드 및 해석을 강제화하려면, 프로그래밍적으로 외부 javascript 파일을 포함하는 script element를 생성해서 DOM에 삽입하는 기법이 널리 알려져 있다.
    ```
    <!-- 차단하지 않고, 다운로드를 시작한 후 완료되면 파일을 해석한다. -->
    <script>
        // code...
    </script>
    ```
    * 동적 scrpit element 사용 시의 주요 단점은 순서와 다르게 해석된다는 것이다.

* 비동기 script가 로드되는 시점을 알 수 있도록 onload 콜백을 사용하기
    
* DOM 조작 시 HTML에서 script의 위치에 주의
    * script element는 원래 동기 방식이므로, HTML 문서의 head element에 두게 되면 실행되는 javascript가 script 보다 뒤에 잇는 DOM의 요소애 의존적일 경우 타이밍 문제를 발생시킨다.
    ```
    <head>
    <script>
        console.log(document.body.innerHTML) // ERROR
    </script>
    </head>
    <body>
    ```
    * 떄문에 많은 개발자들은 모든 script element를 body element 이전에 두려고 시도한다.

* DOM 내의 script 목록 가져오기
    * document.scripts

## DOM 이벤트
* 개요
    * DOM의 이벤트는 DOM 내의 element, document 개체, window 개체와 관련되어 발생하는 사전 정의된 시점이나 사용자 정의 시점을 말한다.
    * 이 시점은 통상적으로 사전에 결정되어 있으며, 이 시점이 발생할 때 실행될 기능을 연관시킴으로써 프로그래밍적으로 알 수 있다,.
    * 이 시점은 UI의 상태(ex. 포커스, 드래그...), javascript 프로그램을 실행하는 환경의 상태(ex. 페이지 로드, ...), 프로그램 자체의 상태(ex. 페이지가 로드된 후 30초 동안 모든 마우스 클릭을 모니터링)에 의해 발생된다.
    * 이벤트를 설정하는 것은 인라인 attribute 이벤트 핸들러, 속성 이벤트 핸들러, addEventListener() 메서드를 사용하여 수행될 수 있다.
    * 인라인 attribute 이벤트 핸들러는 javascript 와 HTML을 혼합하게 되는데, 이 둘을 분리해서 유지하는 것이 가장 바람직하다.
    * 속성 이벤트 핸들러를 사용할 때의 단점은 한 번에 한 개의 값만 이벤트 속성에 할당할 수 있다는 것이다.
    * 이는 이벤트를 속성 값으로 할당할 때 하나 이상의 속성 핸들러를 DOM에 추가할 수 없다는 것을 의미한다.
    * 뿐만 아니라 속성 이벤트 핸들러를 인라인으로 사용하면, 이벤트가 호출하는 함수의 영역 체인을 활용하려는 시도는 영역 문제를 겼을 수 있다.

* DOM 이벤트 유형
    * element, document, window 개체에 연결될 수 있는 사전 정의된 이벤트 중 가장 흔히 사용되는 것들
        * 사용자 인터페이스 이벤트
            * load
                * 페에지, 이미지, css, javascript 로드될 때 발생
            * unload
                * 리소스를 제거할 때 발생
            * abort
                * 리소스가 완전히 로드 되기 전에 로드를 중지할 떄 발생
            * error
                * 리소스 로드가 실패했거나, 로드되었지만 유효하지 않은 이미지, 스키립트... 
            * resize
                * 문서 뷰의 크기가 변경되었을 때 발생
            * scroll
                * 사용자가 문서나 element를 스크롤할 때 발생
            * contextmenu
                * element를 우클릭 시 발생
        * focus 이벤트
            * blur
                * 마우스나 탭을 통해 element가 포커스를 잃었을 때 발생
            * focus
                * element가 포커스를 받았을 때 발생
            * focusin
                * 이벤트 대상이 포커스를 받으려고 하나 아직 포커스가 전환되기 이전일 때 발생
            * focusout
                * 포커스를 잃으려고 하나 아직 포커스가 전환되기 이전일 때 발생
        * form 이벤트
            * change
                * 컨트롤이 입력 포커스를 잃고 포커스를 얻은 후 값이 변경되었을 때 발생
            * reset
                * 폼이 리셋될 때 발생
            * submit
                * 폼이 전송될 때 발생
            * select
                * 사용자가 input 및 textarea를 비롯한 텍스트 필드에서 텍스트를 선택할 때 발생
        * mouse 이벤트
            * click
                * element 위에서 마우스 포인터를 클릭할 때 (혹은 사용자가 Enter 키를 눌렀을 때) 발생.
                * click은 동일한 스크린 위치에서 mousedown 및 mouseup으로 정의된다.
                * 이 이벤트들의 순서는 moudesdown > mouseup > click 이다.
                * dbclick 이벤트에 후속해서도 발생된다.
            * dblclick
                * 마우스 포인터를 element 위에서 두 번 클릭할 때 발생한다.
                * 클릭과 더블 클릭이 동시에 발생한 경우 click 이후에, 그렇지 않으면 mouseup 이후에 발생해야 한다.
            * mousedown
                * 마우스 포인터를 element에서 눌렀을 때 발생
            * mouseenter
                * 한번만
                * 마우스 포인터가 element나 하위 elemnt 중 하나의 경계 내로 들어올 떄 발생.
                * 이 이벤트 유형은 mouseover와 유사하지만, 버블링 되지 않으며 포인터 장치가 element 중 하나의 경계로 이동될 때 전파되지 않아야 한다는 차이가 있다.
            * mouseleave
            * mousemove
                * 마우스 포인터가 element 위에서 이동할 때 발생한다.
                * 포인팅 장치가 이동하는 동안 이벤트가 발생하는 빈도율은 장치/ 구현/ 플랫폼에 따라 다르다.
                * 여러개의 mosuemove 이벤트가 발생된다.
            * mouseout
                * 마우스 포인터가 element의 경계를 벗어날 때 발생된다.
                * 이 이벤트 유형은 mouseleave와 유사하지만, 버블링되며 포인터 장치가 element에서 하위 element 중 하나의 경계로 이동될 때 전파되어야 한다는 차이가 있다.
            * mouseup
                * element 위에서 마우스 포인터 버튼을 뗄 때 발생
            * mouseover
                * 계속
                * 마우스 포인터를 element 위에서 움직일 때 발생한다.
        * wheel 이벤트
            * wheel
                * 마우스 휠이 축을 따라 회전하거나 상응되는 입력 장치가 해당 동작을 흉내낼 때 발생한다.
        * keyboard 이벤트
            * keydown
                * 한번만
                * 키가 처음 눌려질 때 발생된다.
                * 키 매핑이 수행된 후에 발생되지만, 입력 수단 편집기가 keypress를 받기 이전이다.
                * 이 이벤트는 문자 코드를 발생시키지 않는 키를 포함한 모든 키에서 발생된다.
            * keypress
                * 계속
                * 문자 값을 발생시키는 키가 처음 눌려질 때 발생된다.
                * 키 매핑이 수행된 이후, 입력 수단 편집기가 keypress를 받기 이전에 발생된다.
            * keyup
                * 키를 뗼 때 발생된다.
                * 항상 관련된 keydown 및 keypress 이벤트에 후속한다
        * touch 이벤트
            * touchstart
                * 사용자가 터치 표면에 터치 포인트를 두었을 때 발생되는 이벤트
            * touchend
                * 사용자가 터치 표면에서 터치 포인트를 제거 했을 때 발생되는 이벤트
                * 화면을 드래그하는 것과 같이 터치 포인트가 터치 표면을 물리적으로 벗어난 경우도 포함
            * touchmove
                * 사용자가 터치 표면을 따라 터치 포인트를 이동할 때 발생되는 이벤트
            * touchenter
                * 터치 포인트가 DOM element에서 정의된 상호작용 영역으로 이동했을 때 발생되는 이벤트
            * touchlave
                * 터치 포인트가 DOM element에서 정의된 상호작용 영역을 벗어날 때 발생되는 이벤트
            * touchcancel
                * 동기 이벤트나 터치 취소 동작과 같이 구현에 따라 다른 수단에 의해 터치 포인트가 방해를 받았거나, 문서 창을 떠나 사용자 상호 작용을 처리할 수 있는 비문서 영역으로 터치 포인트를 옮겼을 때 발생되는 이벤트
        * window, body, frame 관련 이벤트
            * afterprint
            * beforeprint
            * beforeload
            * hashchange
                * URL에서 숫자 기호(#)에 해당하는 부분이 변경될 때 발생
            * message
            * offline
                * 브라우저가 오프라인으로 동작할 때 발생된다.
            * online
                * 브라우저가 온라인으로 동작할 떄 발생된다.
            * pagehide
            * pageshow
        * document 관련 이벤트
            * readystatechange
                * readyState가 변경될 떄 발생된다.
            * DOMContentLoaded
                * 웹 페이지의 해석은 끝났지만, 모든 리소스가 완전히 다운로드되기 전에 발생된다.
        * drag 이벤트
            * drag
                * 드래그 동작 도중 원본 개체에서 계속 발생된다.
            * dragstart
                * 사용자가 선택한 텍스트나 선택된 개체를 드래그하기 시작할 때 원본 개체에서 발생된다.
                * 사용자가 마우스를 드래그할 때는 ondragstart 이벤트가 먼저 발생된다.
            * dragend
                * 사용자가 드래그 동작을 종료하면서 마우스를 놓을 때 원본 개체에서 발생된다.
                * 대상 개체에서는 ondragleave 이벤트 다음에 ondragend 이벤트가 마지막으로 발생된다.
            * dragenter
                * 사용자가 개체를 유효한 드롭 대상으로 드래그 했을 때 대상 element에서 발생한다.
            * dragleave
                * 사용자가 드래그 동작 도중에 유효한 드롭 대상에서 벗어나도록 마우스를 옮겼을 때 대상 개체에서 발생된다.
            * dragover
                * 사용자가 개체를 유효한 드롭 대상 위로 드래그 하는 동안 element에서 계속해서 발생된다.
                * 대상 개체에서는 ondragenter 이벤트가 발생된 후 dragover 이벤트가 발생된다.
            * drop
                * 드래그-앤-드롭 동작 도중 마우스 버튼을 떼었을 때 대상 개체에서 발생된다.
                * ondrop 이벤트는 ondragleave 와 ondragend 이벤트 이전에 발생된다.


* 이벤트 흐름
    * 이벤트가 발생되면 DOM을 따라 흘러가거나 전파되면서 다른 노드와 javascript 개체들에서 동일한 이벤트를 발생시킨다.
    * 이벤트 흐름은 캡처 단계나 버블링 단계, 혹은 양쪽 모두로 발생하도록 프로그래밍할 수 있다.
    * 캡처 단계는 이를 지원하지 않는 브라우저가 있기 때문에 널리 사용되지는 않는다.
    * 통상적으로 이벤트는 버블링 단계 도중에 호출되는 것으로 가정된다.

* element, window, document 개체에 이벤트 수신기를 추가하기
    * addEventListener() 메서드는 모든 노드, 개체에 존재하며, DOM 및 브라우저 개체 모델과 관련된 javascript 개체뿐만 아니라 HTML 문서의 일부에도 이벤트 수신기를 추가할 수 있는 기능을 제공한다.
    * addEventListener() 메서드는 세 개의 인수를 받는다.
        * 수신할 이벤트 형식
        * 발생했을 떄 호출될 함수
        * 이벤트 흐름의 캡처 단계에서 발생될지, 버블링 단계에서 발생될지를 가리키는 boolean값이다.

* 이벤트 수신기 제거하기
    * 원래 수신기가 익명 함수로 추가되지 않았다면, removeEventListener() 메서드를 사용하여 이벤트 수신기를 제거할 수 있다.

* 이벤트 개체에서 이벤트 속성 얻기
    * 이벤트에서 호출되는 핸들러나 콜백 함수에는 이벤트와 관련된 모든 정보를 가지고 있는 매개변수가 전송된다.
    * event 개체는 stopPropagation(), stopImmediatePropagagtion(), preventDefault() 메서드도 제공한다.

* addEventListener 사용 시 this의 값
    * 이벤트가 연결된 노드나 개체에 대한 참조가 된다.
    * 이벤트 흐름의 일부로 이벤트가 호출되면, this 값은 이벤트 수신기가 연결된 노드나 개체의 값이 된다.
    * event.currentTarget 속성을 사용하여 this 속성이 제공하는 것과 동일하게 이벤트 수신기를 호출하는 노드나 개체에 대한 참조를 얻을 수 있다.
    * target은 이벤트 버블링의 가장 마지막에 위치한 최하위 요소를 반환한다.
    * currentTarget은 이벤트가 바인딩 된 요소를 반환한다.

* preventDefault()를 사용하여 기본 브라우저 이벤트를 취소하기
    * 브라우저는 HTML 페이지를 사용자에게 보여줄 때 사전에 구성된 여러 이벤트를 제공한다.
    * 이러한 브라우저 이벤트는 브라우저 기본 이벤트를 호출하는 노드나 개체에 연결된 이벤트 핸들러 함수 내부에서 preventDefault() 메서드를 호출해서 막을 수 있다.
    ```
    document.querySelector('a').addEventListener(
        'click',
        function(e) {
            e.preventDefault(); // url로 이동하는 a의 기본 이벤트를 중지시킴
        },
        false
    );

    document.querySelector('input').addEventListener(
        'click',
        function(e) {
            e.preventDefault();
        },
        false
    );

    document.querySelector('textarea').addEventListener(
        'keypress',
        function(e) {
            e.preventDefault();
        },
        false
    );

    // 기본 이벤트는 중지되었지만 이벤트 버블링은 중지되지 않았으므로, 이벤트가 여전히 전파된다.
    document.body.addEventListener(
        'keypress',
        function(e) {
            console.log('??');
        },
        false
    );
    ```
    * preventDefault() 메서드는 이벤트가 전파되는 것을 중지시키지 않는다.
    * 이벤트 수신기 끝부분에서 false 를 반환하면 preventDefault() 메서드를 호출하는 것과 동일한 결과를 가진다

* stopPropagation()을 사용하여 이벤트 흐름을 중지시키기
    * 캡처/버블링 이벤트 흐름 단계가 중지되지만, 노드나 개체에 직접 연결된 이벤트는 여전히 호출된다.

* stopImmediatePropagation()을 사용하여 동일한 대상의 이벤트 흐름뿐만 아니라 다른 유사 이벤트도 중지시키기
    * 이벤트 흐름 단계를 중지시키는 것 뿐만 아니라, 이후에 연결되어 호출되는 이벤트 대상의 다른 유사한 이벤트도 중지시킨다.

* 사용자 정의 이벤트
    * document.createEvent(), initCustomEvent(), dispatchEvent()와 조합해서 사용하면 사용자 정의 이벤트를 연결해서 호출할 수 있다.
    ```
    var div = document.querySelector('div');

    var a = document.createEvent('CustomEvent');

    div.addEventListener(
        'taesang',
        function(e) {
            console.log(e.detail);
            console.log(e.detail.taesang);
        },
        false
    );

    a.initCustomEvent('taesang', true, false, { taesang: 'its gone!' });

    div.dispatchEvent(a);
    ```
* 마우스 이벤트 시뮬레이션/트리거링
    * 이벤트 시뮬레이션하는 것은 사용자 정의 이벤트와 별반 다를 바 없다.
    * 마우스 이벤트를 시뮬레이션하려면, document.createEvent()를 사용하여 MouseEvent를 생성한 후 initMouseEvent()를 사용하여 발생할 마우스 이벤트를 구성한다.
    * 다음으로, 이벤트를 시뮬레이션 하고자하는 element에 전달한다.
    ```
    var div = document.querySelector('div');

    div.addEventListener(
        'click',
        function(e) {
            console.log(Object.keys(e));
        },
        false
    );

    var simulatedDivClick = document.createEvent('MouseEvent');

    simulatedDivClick.initMouseEvent(
        'click',
        true,
        true,
        document.defaultView,
        0,
        0,
        0,
        0,
        0,
        false,
        false,
        false,
        0,
        null,
        null
    );

    div.dispatchEvent(simulatedDivClick);

    ```


* 이벤트 위임
    * 이벤트 위임(delegate)은 간단히 말해 이벤트 흐름을 활용하여 단일 이벤트 수신기가 여러개의 이벤트 ㅐ상을 처리할 수 있게 하는 프로그래밍 행위를 말한다.
    * 이벤트 위임의 부수 작용은 이벤트가 생성될 때 이벤트에 응답하기 위해 이벤트 대상이 DOM 내에 있을 필요가 없다는 것이다.
    * 이는 DOM을 업데이트하는 XHR 응답을 처리할 때보다 편리해진다.
    * 이벤트 위임을 구현함으로써, javascript가 로드되어 해석된 후에 새로운 콘텐츠가 DOM에 추가되어도 즉시 이벤트에 응답할 수 있게 된다.
    * 이것이 가능한 이유는 이벤트 흐름 때문이며, 특히 그 중에서 버블링 단계임을 잊지 말아야 한다.
    * 이벤트 위임은 click, mousedown, mouseup, keydown, keyup, keypress 이벤트 형식을 처리할 때 활용하면 이상적이다.

## dom.js 만들기, 최신 브라우저용 jquery 유사 DOM 라이브러리
* 고유 범위 만들기
    * 전역 범위로부터 코드를 보호하기 위해 필요하다.
    ```
    (function(win) {
        var global = win;
        var doc = this.document;
    })(window);
    ```

* dom() 과 GetOrMakeDom() 을 생성하고 dom()과 GetOrMakeDom.prototpye을 전역에 노출시키기
    ```
    (function(win) {
        var global = win;
        var doc = this.document;

        var dom = function(params, context) {
            return Object.create(GateOrMakeDom(params, context));
        };

        var GateOrMakeDom = function(params, context) {};
    })(window);

    ```
    * IIFE에 의해 구성되는 private 범위 외부에서 dom() 함수가 접근/호출될 수 있게 하려면, 전역 범위에 해당 함수를 노출해야 한다.
    * 전역 범위에 dom이라는 속성을 생성하고, 이 속성이 로컬 dom() 함수를 가리키게 함으로써 이를 수행할 수 있다.
    * 전역 범위에서 dom 에 접근허려면, 로컬 범위를 가지는 dom() 함수를 가리키게 된다.
    ```
    (function(win) {
        var global = win;
        var doc = this.document;

        var dom = function(params, context) {
            return Object.create(GateOrMakeDom(params, context));
        };

        var GateOrMakeDom = function(params, context) {};

        global.dom = dom;
    })(window);
    ```
    * 마지막으로 해야 할 것은 GetOrMakeDom.prototype 속성을 전역 범위로 노출하는 것이다.
    ```
    (function(win) {
        var global = win;
        var doc = this.document;

        var dom = function(params, context) {
            return Object.create(GateOrMakeDom(params, context));
        };

        var GateOrMakeDom = function(params, context) {};

        global.dom = dom;

        dom.fn = GateOrMakeDom.prototype;
    })(window);
    ```
    * 이제 dom.fn에 연결되는 것은 실제로 GetOrMakeDom.prototype 개체의 속성이 되고, GetOrMakeDom 생성자 함수로부터 만들어지는 모든 개체 인스턴스에 대한 속성 찾아보기를 통해 상속된다.

* dom()에 전달되는 선택적인 Context 매개변수 생성하기
    * dom()에 호풀될 떄 GetOrMakeDom 함수도 호출되며, dom()에 전달된 매개변수가 그대로 전달된다.
    * GetOrMakeDom 생성자가 호출될 때, 가장 먼저 해야 할 것은 컨텍스트를 결정하는 것이다.
    * DOM에서 작업할 컨텍스트는 노드나 노드 자체에 대한 참조를 선택하는 데 사용되는 선택자 문자열을 전달하여 설정할 수 있다.
    * dom() 함수에 컨텍스트를 전달하면, DOM 트리의 특정 브랜치에서 element노드를 탐색하도록 제한할 수 있게 된다.
    ```
    (function(win) {
        var global = win;
        var doc = this.document;

        var dom = function(params, context) {
            return Object.create(GatOrMakeDom(params, context));
        };

        var GatOrMakeDom = function(params, context) {
            var currentContext = doc;
            if (context) {
                if (context.nodeType) {
                    // 문서 노드 혹은 element 노드 중 하나
                    currentContext = context;
                } else {
                    // 문자열 선택자인 경우, 노드를 선택하는 데 사용
                    currentContext = doc.querySelector(context);
                }
            }
        };

        global.dom = dom;

        dom.fn = GatOrMakeDom.prototype;
    })(window);
    ```
    * context 매개변수 로직이 구성되었으므로, 다음으로 실제 노드를 선택하거나 노드를 생성하는 데 사용되는 params 매개변수를 처리하는 데 필요한 로직을 추가한다.

* params를 기반으로 DOM 노드 참조를 가진 개체를 채워 반환
    * dom()에 전달된 후 getOrMakeDom()에 전달되는 params 매개변수의 형식은 다양하다.
    * 종류
        * css 선택 문자열 (ex. dom('body'))
        * html 문자열 (ex. dom(' p> hello /p>'))
        * element 노드 (ex. dom(document.body))
        * elemnt 노드의 배열 (ex. dom([document.body]))
        * nodelist (ex. dom(documnet.body.children))
        * HTMLCollection (ex. dom(document.all))
        * dom () 개체 자신 (ex. dom(dom()))
    * params를 전달하면 결과적으로 DOM 이나 document fragment 내의 노드에 대한 참조를 가진 체인화된 개체를 생성하게 된다.
    * 앞에서 언급된 각 매개변수가 노드 참조를 가진 개체를 만들어내는 데 어떻게 사용되는지 알아보자.
        * params가 undefined나 빈 문자열 혹은 공백으로 된 문자열이 아닌지를 검증하는 것부터 시작한다.
            * 이 경우, GetOrMakeDom을 호출하여 생성된 개체에 값이 0 인 length 속성을 추가해서 개체를 반환하여 함수 실행을 종료시킨다.
            * params가 false 혹은 falsy한 값이 아닌 경우 함수가 계속 실행된다.
        * 다음으로 params 값이 문자열인 경우 HTML을 포함하고 있는지 검사된다.
            * 문자열이 HTML을 포함하고 있으면, document fragment가 생성되고 DOM 구조로 변환될 수 있도록 해당 문자열은 document  fragmnet 내에 포함된 div의 innerHTML 값으로 사용된다.
            * HTML 문자열이 노드 트리로 변환되면, 최상위 노드에 접근하여 해당 구조에 대해 루프를 돌면서 노드에 대한 참조가 GetOrMakeDom이 생성하는 개체로 전달된다.
            * 문자열이 HTML을 포함하고 있지 않으면, 함수가 계속 실행된다.
        * 다음 검사는 params가 단일 노드에 대한 참조인지를 검사하고, 이 경우 노드에 대한 참조를 개체로 감싸서 반환한다.
            * 그렇지 않은 경우에는 params 값이 HTML 컬렉션, 노드 리스트, 문자열 선택자, 혹은 domI() 에서 생성된 개체라고 단정할 수 있다.
            * 문자열 선택자인 경우에는 currentContext에서 querySelectAll() 메서드를 호출하여 노드 리스트가 생성된다.
            * 문자열이 아닌 경우 HTML 컬렉션, 노드 리스트, 배열, 개체에 대해 루프를 돌면서 노드 참조를 추출하고, 해당 참조를 GetOrMakeDom을 호출 시 반환되는 개체 내에 포함되는 값으로 사용된다.
        * GetOrMakeDom() 함수 내에 이 모든 로직은 다소 과도해 보일 수도 있는데, 생성자 함수에서 노드에 대한 참조를 포함하고 있는 개체가 생성되고 이 개체가 dom()으로 반환된다고 생각하면 된다.
        ```
        (function(win) {
            var global = win;
            var doc = this.document;

            var dom = function(params, context) {
                return Object.create(GatOrMakeDom(params, context));
            };

            var regXContainsTag = /^\s*<(\w+|!)[^>]*>/;

            var GatOrMakeDom = function(params, context) {
                var currentContext = doc;
                if (context) {
                    if (context.nodeType) {
                        // 문서 노드 혹은 element 노드 중 하나
                        currentContext = context;
                    } else {
                        // 문자열 선택자인 경우, 노드를 선택하는 데 사용
                        currentContext = doc.querySelector(context);
                    }
                }

                // params가 없으면 빈 dom() 개체를 반환
                if (!prams || params === '' || (typeof params === 'string' && params.trim() === '')) {
                    this.length = 0;
                    return this;
                }

                // HTML 문자열인 경우, domfragment를 생성하고 개체를 채워 반환
                if (typeof params === 'string' && regXContainsTag(params)) {
                    var divEl = currentContext.createElement('div');
                    divEl.className = 'hippo-doc-frag-wrapper';
                    var docFrag = currentContext.createDocumentFragment();
                    docFrag.appendChild(divEl);
                    var queryDiv = docFrag.querySelector('div');
                    queryDiv.innerHTML = params;
                    var numberOfChildren = queryDiv.children.length;

                    // html 문자열이 자식들과 함께 전달될 수 있으므로 nodelist에서 루프를 돌며 개체를 채움
                    for (var z = 0; z < numberOfChildren; z++) {
                        this[z] = queryDiv.children[z];
                    }
                    this.length = numberOfChildren;
                    return this;
                }

                // 단일 노드 참조가 전달된 경우, 개체를 채워서 반환.
                if (typeof params === 'object' && params.nodeName) {
                    this.length = 1;
                    this[0] = params;
                    return this;
                }

                // 개체이지만 노드가 아닌 경우, 노드 리스트나 배열로 가정한다. 그렇지 않은 경우 문자열 선택자이므로 노드 리스트를 만든다.
                var nodes;
                if (typeof params !== 'string') {
                    nodes = params;
                } else {
                    nodes = currentContext.querySelector(params.trim());
                }

                // 위에서 생성된 배열이나 노드 리스트에 대해 루프를 돌면서 개체를 채움
                var nodeLength = nodes.length;
                for (var i = 0; i < nodeLength; i++) {
                    this[i] = nodes[i];
                }
                this.length = nodeLength;
                return this;
            };

            global.dom = dom;

            dom.fn = GatOrMakeDom.prototype;
        })(window);
        ```
        * 