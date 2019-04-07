## keyword
* DOM
* document
* window
* element
* nodeList
* attribute
* ParentNode
* Node
* EventTarget


* 자주 쓰이는
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


* document 노드
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

* element 노드
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

* element 노드 선택
    * querySelector()
    * getElementById()
    * querySelectorAll()
    * getElementsByTagName()
    * getElementsByClassName()

* Element 노드 지오메트리와 스크롤링 지오메트리
    * offsetLeft
    * offsetTop
    * offsetParent
    * offsetHeigh
    * offsetWidth
    * getBoundingClinetRect()
    * clientWidth
    * clientHeight
    * elementFromPoint(50, 50)
    * scrollTop
    * scrollLeft
    * scrollIntoView()
    
* Element 노드 인라인 스타일
    * style
    * style.setProperty()
    * style.getProperty()
    * style.removeProperty()
    * setAttribute()
    * getAttribute()
    * removeAttribute()
    * getComputedStyle()
    * classList
    * classList.add()
    * classList.remove()


* Text 노드
    * textContent
    * innerText
    * splitText()
    * appendData()
    * deleteData()
    * insertData()
    * replaceData()
    * subStringData()
    * normalize()
    * data
    * document.createTextNode() 

* DocumentFragment
    * createDocumentFragment()

* DOM event
    * 사용자 인터페이스 이벤트
        * load
        * unload
        * abort
        * error
        * resize
        * scroll
        * contextmenu
    * focus 이벤트
        * blur
        * focus
        * focusin
        * focusout
    * form 이벤트
        * change
        * reset
        * submit
        * select
    * mouse 이벤트
        * click
        * dblclick
        * mousedown
        * mouseenter
        * mouseleave
        * mousemove
        * mouseout
        * mouseup
        * mouseover
    * wheel 이벤트
        * wheel
    * keyboard 이벤트
        * keydown
        * keypress
        * keyup
    * touch 이벤트
        * touchstart
        * touchend
        * touchmove
        * touchenter
        * touchlave
        * touchcancel
    * window, body, frame 관련 이벤트
        * afterprint
        * beforeprint
        * beforeload
        * hashchange
        * message
        * offline
        * online
        * pagehide
        * pageshow
    * document 관련 이벤트
        * readystatechange
        * DOMContentLoaded
    * drag 이벤트
        * drag
        * dragstart
        * dragend
        * dragenter
        * dragleave
        * dragover
        * drop

* event
    * e.target
    * e.currentTarget
    * addEventListener()
    * removeEventListener()
    * stopPropagation()
    * stopImmediatePropagation()
    * preventDefault()