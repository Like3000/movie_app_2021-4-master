<!-- @format -->

# 강태훈 201840104

## [12월 8일]

#### 조건부 렌더링

    React에서 조건부 렌더링은 JavaScript에서의 조건 처리와 같이 동작함.

###### 엘리멘트 변수

    엘리먼트를 저장하기 위해 변수를 사용할 수 있다.
    출력의 다른 부분은 변하지 않은 채로 컴포넌트의 일부를 조건부로 렌더링 할 수 있다.
    ex) 로그인 / 로그아웃 버튼
    function LoginButton(props) {
        return (
            <button onClick={props.onClick}>
            Login
            </button>
        );
    }

    function LogoutButton(props) {
        return (
            <button onClick={props.onClick}>
            Logout
            </button>
        );
    }

###### 논리 && 연산자로 If를 인라인으로 표현

    1. JSX 안에는 중괄호를 이용해서 표현식을 포함 할 수 있다.
    2. JavaScript의 논리 연산자 &&를 사용하면 쉽게 엘리먼트를 조건부로 넣을 수 있다.
    3. JavaScript에서 true && expression은 항상 expression으로 평가되고 false && expression은 항상 false로 평가됨.
    4. && 뒤의 엘리먼트는 조건이 true일때 출력이 됨
    5. 조건이 false라면 React는 무시하고 건너뜀
    6. falsy 표현식을 반환하면 여전히 && 뒤에 있는 표현식은 건너뛰지만 falsy 표현식이 반환됨
    ex) <div>0</div>이 render 메서드에서 반환됨.
    render() {
        const count = 0;
        return (
            <div>
            { count && <h1>Messages: {count}</h1>}
            </div>
        );
    }

###### 조건부 연산자로 If-Else구문 인라인으로 표현하기

    엘리먼트를 조건부로 렌더링하는 다른 방법은 조건부 연산자인 condition ? true: false를 사용
    가독성이 다소 떨어지지만 큰 표션식을 넣을 수도 있음

###### 컴포넌트가 렌더링하는 것을 막기

    가끔 다른 컴포넌트에 의해 렌더링될 때 컴포넌트 자체를 숨기고 싶을 때 렌더링 결과를 출력하는 대신 null을 반환하면 해결

#### 리스트와 Key

    열의 값을 반환할 때는 map()함수를 사용 / 변환하여 반환하는 것도 가능
    ex)
     const number =[1,2,3,4,5];
    const doubled = numbers.map((number) => number *2 );
    console.log(doubled);

###### 여러개의 컴포넌트 렌더링 하기

    엘리먼트 모음을 만들고 중괄호 {}를 이용하여 JSX에 포함 가능
    JavaScript map() 함수를 사용하여 numbers 배열을 반복 실행
    각 항목에 대해 <li> 엘리먼트를 반환하고 엘리먼트 배열의 결과를 listItems에 저장

###### 기본 리스트 컴포넌트

    일반적으로 컴포넌트 안에서 리스트를 렌더링
    아래 예시를 numbers 배열을 받아서 순서 없는 엘리먼트 리스트를 출력하는 컴포넌트로 리팩토링
    ex)
    function NumberList(props) {
        const numbers = props.numbers;
        const listItems = numbers.map((number) =>
            <li>{number}</li>
        );
        return (
            <ul>{listItems}</ul>
        );
    }

    const numbers = [1, 2, 3, 4, 5];
    ReactDOM.render(
        <NumberList numbers={numbers} />,
        document.getElementById('root')
    );

    위의 코드를 실행하면 리스트의 각 항목에 key를 넣어야 한다는 경고가 표시
    key는 엘리먼트 리스트를 만들 때 포함해야 하는 특수한 문자열 어트리뷰트

###### key

    Key는 React가 어떤 항목을 변경, 추가 또는 삭제할지 식별하는 것을 도움
    key는 엘리먼트에 안정적인 고유성을 부여하기 위해 배열 내부의 엘리먼트에 지정
    Key를 선택하는 가장 좋은 방법은 리스트의 다른 항목들 사이에서 해당 항목을 고유하게 식별할 수 있는 문자열을 사용(대부분의 경우 데이터의 ID를 key로 사용)
    렌더링 한 항목에 대한 안정적인 ID가 없다면 최후의 수단으로 항목의 인덱스를 key로 사용

###### Key로 컴포넌트 추출하기

    ListItem 안에 있는 li 엘리먼트가 아니라 분해하는 곳의 ListItem엘리먼트가 key를 가져야 함

###### Key는 형제 사이에서만 고유한 값이어야 한다.

    두 개의 다른 배열을 만들 때 동일한 key를 사용 가능
    ex)
    function Blog(props) {
        const sidebar = (
            <ul>
            {props.posts.map((post) =>
                <li key={post.id}>
                {post.title}
                </li>
            )}
            </ul>
        );
        const content = props.posts.map((post) =>
            <div key={post.id}>
            <h3>{post.title}</h3>
            <p>{post.content}</p>
            </div>
        );
        return (
            <div>
            {sidebar}
            <hr />
            {content}
            </div>
        );
    }

    const posts = [
        {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
        {id: 2, title: 'Installation', content: 'You can install React from npm.'}
    ];
    ReactDOM.render(
        <Blog posts={posts} />,
        document.getElementById('root')
    );

###### JSX에 map() 포함시키기

    JSX를 사용하면 중괄호 안에 모든 표현식을 포함 시킬 수 있으므로 map() 함수의 결과를 인라인으로 처리 가능

#### 폼

    폼은 사용자가 폼을 제출하면 새로운 페이지로 이동하는 기본 HTML 폼 동작을 수행
    React에서 동일한 동작을 원한다면 그대로 사용
    JavaScript 함수로 폼의 제출을 처리하고 사용자가 폼에 입력한 데이터에 접근하도록 하는 것이 편리
    이를 위한 표준 방식은 “제어 컴포넌트 (controlled components)“라고 불리는 기술을 이용

###### 제어 컴포넌트 (Controlled Component)

    HTML에서 <input>, <textarea>, <select>와 같은 폼 엘리먼트는 일반적으로 사용자의 입력을 기반으로 자신의 state를 관리하고 업데이트 함
    React에서는 변경할 수 있는 state가 일반적으로 컴포넌트의 state 속성에 유지되며 setState()에 의해 업데이트 됨
    React state를 “신뢰 가능한 단일 출처 (single source of truth)“로 만들어 두 요소를 결합할 수 있음
    폼을 렌더링하는 React 컴포넌트는 폼에 발생하는 사용자 입력값을 제어
    React에 의해 값이 제어되는 입력 폼 엘리먼트를 제어 컴포넌트 라고 함

###### textarea 태그

    HTML에서 <textarea> 엘리먼트는 텍스트를 자식으로 정의
    ex)
    <textarea>
    Hello there, this is some text in a text area
    </textarea>

    React에서 <textarea>는 value 어트리뷰트를 대신 사용
    <textarea>를 사용하는 폼은 한 줄 입력을 사용하는 폼과 비슷하게 작성 가능

###### select 태그

    HTML에서 <select>는 드롭 다운 목록 생성
    selected 옵션이 있으므로 Coconut 옵션이 초기값이 되는 점을 주의
    React에서는 selected 어트리뷰트를 사용하는 대신 최상단 select태그에 value 어트리뷰트를 사용
    한 곳에서 업데이트만 하면되기 때문에 제어 컴포넌트에서 사용하기 더 편함

    주의)
    select 태그에 multiple 옵션을 허용한다면 value 어트리뷰트에 배열을 전달 가능
    ex)
    <select multiple={true} value={['B', 'C']}>

###### file input 태그

    HTML에서 <input type="file">는 사용자가 하나 이상의 파일을 자신의 장치 저장소에서 서버로 업로드하거나 File API를 통해 JavaScript로 조작 가능
    <input type="file" />
    값이 읽기 전용이기 때문에 React에서는 비제어 컴포넌트

###### 다중 입력 제어하기

    여러 input 엘리먼트를 제어해야할 때, 각 엘리먼트에 name 어트리뷰트를 추가하고 event.target.name 값을 통해 핸들러가 어떤 작업을 할 지 선택 가능
    setState()는 자동적으로 현재 state에 일부 state를 병합하기 때문에 바뀐 부분에 대해서만 호출하면 됨

###### 제어되는 Input Null 값

    제어 컴포넌트에 value prop을 지정하면 의도하지 않는 한 사용자가 변경할 수 없음
    value를 설정했는데 여전히 수정할 수 있다면 실수로 value를 undefined나 null로 설정 가능

#### 합성 (Composition) vs 상속 (Inheritance)

    React는 강력한 합성 모델을 갖고 있으며 상속 대신 합성을 사용하여 컴포넌트 간에 코드를 재사용하는 것이 좋다.

###### 컴포넌트에서 다른 컴포넌트를 담기

    어떤 컴포넌트들은 어떤 자식 엘리먼트가 들어올 지 미리 예상할 수 없는 경우가 있다.
    범용적인 ‘박스’ 역할을 하는 Sidebar 혹은 Dialog와 같은 컴포넌트에서 특히 자주 볼 수 있다.
    이러한 컴포넌트에서는 특수한 children prop을 사용하여 자식 엘리먼트를 출력에 그대로 전달하는 것이 좋다.
    ex)
    function FancyBorder(props) {
        return (
            <div className={'FancyBorder FancyBorder-' + props.color}>
            {props.children}
            </div>
        );
    }

###### 특수화

    때로는 어떤 컴포넌트의 “특수한 경우”인 컴포넌트를 고려해야 하는 경우가 있다.
    WelcomeDialog는 Dialog의 특수한 경우

#### React로 사고하기

    React는 JavaScript로 규모가 크고 빠른 웹 애플리케이션을 만드는 가장 좋은 방법
    React의 가장 멋진 점 중 하나는 앱을 설계하는 방식
    React는 Facebook과 Instagram을 통해 확장성을 입증

###### 1단계 : UI를 컴포넌트 계층 구조로 나누기

    1. 하위 컴포넌트를 포함한 모든 컴포넌트의 주변에 박스를 그리고 그 각각에 이름붙이기
    2. 새로운 함수나 객체를 만들 때처럼 생성
    3. 한 가지 테크닉은 단일 책임 원칙

###### 2단계 : React로 정적인 버전 만들기

    1. 가장 쉬운 방법은 데이터 모델을 가지고 UI를 렌더링은 되지만 아무 동작도 없느 버전을 만들어보는 것이다.
    2. 과정을 나누는 것이 좋은데, 정적 버전을 만드는 것은 생각은 적게 필요하지만 타이핑은 많이 필요로 하고, 상호작용을 만드는 것은 생각은 많이 해야 하지만 티이핑은 적게 필요로 하기 때문
    3. 데이터 모델을 렌더링하는 앱의 정적 버전을 만들기 위해 다른컴포넌트를 재사용하는 컴포넌트를 만들고 props를 히용해 데이터를 전달
    4. props는 부모가 자식에게 데이터를 넘겨줄 때 사용할 수 있는 방법
    5. state는 오직 상호작용을 위해, 즉 시간이 지남에 따라 데이터가 바뀌는 것에 사용

###### 3단계 : UI state에 대한 최소한의 표현 찾아내기

    1. UI를 상호작용하게 만들려면 기반 데이터 모델을 변경할 수 있는 방법 필요
    2. React에서는 이를 state를 통해 변경

###### 4단계 : State 가 어디에 있어야 할 지 찾기

    1. 최소한으로 필요한 State가 뭔지 찾아야 함
    2. 어떤 컴포넌트가 state를 변경하거나 소유할지 찾아야 함
    3. 애플리케이션이 가지는 각각의 state
    - state를 기반으로 렌더링하는 모든 컴포넌트
    - 공통 소유 컴포넌트
    - 공통 혹은 더 상위에 있는 컴포넌트가 state
    - state를 소유할 적절한 컴포넌트를 찾지 못했다면, state를 소유하는 컴포넌트를 하나 만들어서 공통 오너 컴포넌트의 상위 계층에 추가

###### 5단계 : 역방향 데이터 흐름 추가

    1. 지금까지 계층 구조 아래로 흐르는 props와 state로 앱 구현
    2. 다른 방향의 데이터 흐름을 만들어볼 차례
    3. 계층 구조의 하단에 있는 폼 컴포넌트에서 FilterableProductTable의 state를 업데이트할 수 있어야 함
    4. React는 전통적인 양방향 데이터바인딩과 비교하면, 더 많은 타이핑을 필요로 하지만 데이터 흐름을 명시적으로 보이게 만들어서 프로그램이 어떻게 동작하는지 파악할 수 있게 도와 줌

#### Hook

    Hook은 React 버전 16.8부터 React 요소로 새로 추가
    기존의 Class로 코들르 작성할 필요 없이 Hook을 이용하여 State와 여러가지 React의 기능을 사요할 수 있음

###### Hook의 특징

    선택적 사용 기존의 코드를 다시 작성할 필요 없이 일부의 컴포넌트들 안에서 Hook을 사용 가능
    100% 이전버전과의 호환성 Hook은 호환성을 깨뜨리는 변화 없음
    현재 사용 가능 Hook은 배포 v16.8.0 에서 사용 가능

## [12월 01일]

### 주요개념

#### State and Lifecyle

    1. Clock 컴포넌트를 완전히 재사용하고 캡슐화하는 방법
    2. 첫 번째 예제와 두 번째 예제는 컴포넌트의 분리만 있을 뿐 차이가 없음
    3. 중요한 것은 clock이 스스로 업데이트하도록 만들기 위해서는 clock에 state가 있어야 함
    4.State는 props와 유사하지만, 비공개이며 컴포넌트에 의해 완전히 제어
    5. 함수에서 클래스로 변환
    - React.Component를 확장하는 동일한 이름의 ES6 class를 생성
    - render() 라고 불리는 빈 메서드를 추가
    - 함수의 내용을 redner()메서드 안으로 옮김
    - render() 내용 안에 있는 props를 this.props로 변경
    - 남아있는 빈 함수 선언을 삭제

    6. 클래스에 로컬 state 추가
    ex)
    1. render() 메서드 안에 있는 this.props.date를 this.state.date로 변경
         class Clock extends React.Component {
            render() {
                return (
                <div>
                    <h1>Hello, world!</h1>
                    <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
                </div>
                );
            }
        }

    2. 초기 this.state를 지정하는 class constructor를 추가(props를 기본 constructor에 전달하는지 유의)
         class Clock extends React.Component {
            constructor(props) {
                super(props);
                this.state = {date: new Date()};
            }
            render() {
                return (
                <div>
                    <h1>Hello, world!</h1>
                    <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
                </div>
                );
            }
        }

    3. <Clock/> 요소에서 date prop을 삭제
    ReactDOM.render(
        <Clock />,
    document.getElementById('root')
    );

    *결과
    tick()메서드가 들어가지 않아 현재시간만 표시되고 시간에 따른 변화가 없음

    7. 생명주기 메서드를 클래스에 추가
    - Clock이 처음 DOM에 렌더링 될 때마다 타이머를 설정하려고 하는 것을 React에서 '마운팅'이라 함
    - Clock에 의해 생섣된 DOM이 삭제될 때마다 타이머를 해제하려고 하는 것을 React에서 '언마운팅'이라 함
    - componentDidMount() 메서드
        componentDidMount() {
            this.timerID = setInterval(
                () => this.tick(),
                1000
            );
        }
    - componentWillunmount() 메서드
        componentWillUnmount() {
            clearInterval(this.timerID);
        }

    - Clok 컴포넌트가 매초 작동할 수 있도록 tick()메서드를 구현하여 위 예제에 적용

    8. State 올바르게 사용법
    - 직접 State를 수정하지 않음
    - State 업데이트는 비동기적일 수도 있음
    - State 업데이트는 병합됨
    9. 데이터는 아래로 흐름
    - 부모 컴포넌트나 자식 컴포넌트 모두 특정 컴포넌트가 stateful인지 또는 stateless 인지 알 수 없어 state는 종종 로컬 또는 캡슐화 라고 함
    - 컴포넌트는 자신의 state를 자식 컴포넌트에 props로 전달 가능
    - 일반적으로 이것을 하향식 또는 단방향식 데이터 흐름 이라 함
    - React 앱에서 컴포넌트가 stateful 또는 stateless에 대한 것은 시간이 지남에 따라 변경될 수 있는 구현 세부 사항으로 간주
    - stateful 컴포넌트 안에서 stateless 컴포넌트를 사용할 수 있으며, 그 반대 경우도 마찬가지로 사용 가능

#### 이벤트 처리하기

    1. React 엘리먼트에서 이벤트를 처리하는 방식은 DOM 엘리먼트에서 이벤트를 처리하느 방식과 매우 유사함
    - React의 이벤트는 소문자 대신 캐멀 케이스(camelCase)를 사용
    - JSX를 사용하여 문자열이 아닌 함수로 이벤트 핸들러를 전달
    ex) HTML
    <button onclick="activateLasers()">
        Activate Lasers
    </button>

    ex) react
    <button onClick={activateLasers}>
        Activate Lasers
    </button>
    2. 이벤트 핸들러에 인자 전달
    - 루프 내부에서는 이벤트 핸들러에 추가적인 매개변수를 전달하는 것이 일반적

    ex) id가 행의 ID일 경우 실행되는 예제
    <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
    <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>

    - 위 두 줄은 동등하며 각각 화살표 함수와 Function.prototype.bind를 사용
    - 두 경우 모두 React 이벤트를 나타내는 e 인자가 ID 뒤에 두 번째 인자로 전달됩
    - 화살표 함수를 사용하면 명시적으로 인자를 전달해야 하지만 bind를 사용할 경우 추가 인자가 자동으로 전달

## [11월 24일]

#### create-react-app으로 [ Remarkable ] 사용하기

    1.npm create-react-app으로 markdown-editor 프로젝트를 생성함
    2.프로젝트가 정상 동작하는지 확인함
    3.App.js에 있는 필요없는 코드를 삭제한 후 문서의 코드를 복사해 넣음
    4.component의 이름을 App으로 수정함
    5.rendering은 index.js에 위임함
    6.App.js에서 React와 Remarkable을 import함
    7.프로젝트가 동작이 되는지 확인함

#### React 배우기

    1. React는 처음부터 점진적으로 적용할 수 있도록 서례되었으며 필요한 만큼 React를 사용할 수 있음
    2. 온라인 코드 편집기를 사용하여 간편하게 리액트를 경험할 수 있음
    3. CodeSandbox는 create-react-app으로 생성된 프로젝트와 동일한 환경에서 테스트가 가능함
    4. CDN방식으로 간편하게 테스트를 할 수 있도록 HTML코드를 제공하고 있음
    5. React 문서가 어렵게 느껴진다면, Tania Rascia가 쓴 React개요를 먼저 학습하는 것이 도움이 됨
    6. 개발을 통해 React를 학습하고 싶다면 React홈페이지에 자습서를 통해 공부함

#### React 홈페이지

    1. 주요개념
    -개념을 단계별로 배우려면 주요개념 부터 시작하는 것을 추천
    2. 고급개념
    -강력하지만 일반적으로 많이 사용되지는 않는 React 기능을 소개
    3. API참조
    -특정 React API를 자세히 알아보고 싶을 때 유용한 문서
    4. Hook
    -16.8부터 새로 추가된 Hook에 대한 자세한 설명을 제공

#### 주요개념

###### Hello World

    1. 가장 단순한 React
    2. ReactDOM.render(<h1>Hello, world!</h1>,document.getElementById('root'));의 코드는 Hello,world! 라는 제목을표시

###### JSX 소개

    1. JSX에 표현식 포함
    2. 함수의 호출 결과를 JSX에 표현식 포함
    3. if,for문 등과 함께 사용, 변수에 할당, 인자로 받고, 함수로부터 반환할 수 있음
    4. 속성에 따옴표를 이용해 문자열 리터럴을 정의할 수 있음
    5. 속성에 중괄호를 사용하여 JacaScrtipt 표현식을 삽입할 수 있음
    6. 태그가 비어있다면 XML처럼 /> 를 이용해 바로 닫아주어야 함
    7. Bable은 JSX를 React.createElement()호출로 컴파일 함

    ex)jsx
    function getGreeting(user) {
        if (user) {
            return <h1>Hello, {formatName(user)}!</h1>;
        }
    return <h1>Hello, Stranger.</h1>;
    }

###### 엘리먼트 렌더링

    1. Element는 React앱의 가장 작은 단위
    2. React element를 root DOM 노드에 표시하려면 ReactDOM.render()로 전달하면 됨
    3. ReactDOM은 해당 엘리먼트와 그 자식 엘리먼트를 이전의 엘리먼트와 비교하고 DOM을 원하는 상태로 만드는데 필요한 경우에만  DOM을 업데이트함

###### Component와 Props

    1. 개념적으로 컴포넌트는 JavaScript 함수와 유사함
    2. React에는 함수 컴포넌트와 클래스 컴포넌트가 있음
    3. 컴포넌트의 이름은 항상 대문자로 시작함
    4. 문서'컴포넌트 렌더링'예제의 실행 과정
    5. props 라고 하는 임의의 입력을 받은 후, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환

    ex) 함수 클래스 컴포넌트
        function Welcome(props) {
            return <h1>Hello, {props.name}</h1>;
        }
        class Welcome extends React.Component {
            render() {
             return <h1>Hello, {this.props.name}</h1>;
            }
        }

## [11월 17일]

#### 영화 앱 깃허브에 배포

    1. package.json 파일을 열어 homepage 키와 키값을 browserslist 키 아래에 추가
    2. package.json 파일에 scripts 키값으로 명령어를 추가
    3. (git add./git commit -m ""/git push origin master)의 명령어를 사용하여 깃허브에 업로드
    4. (npm install gh-pages)의 명령어를 사용하여 gh-pages를 설치
    5. (git remote -v)의 명령어를 입력하여 업로드한 깃허브 저장소의 주소를 확인
    6. (npm run deploy)의 명령어를 입력하여 영화 앱을 배포
    7. URL에 'https://계정.github.io/저장소 이름'을 입력해서 확인가능

#### React의 특징

    1. 캡슐화된 컴포넌트로 개발되어 재사용이 용이
    2. DOM과는 별개로 상태를 관리가능
    3. 상호작용이 많은 UI개발에 적합
    4. 기존 코드와 별개로 개발이 가능

#### js파일 없이 리액트 앱 생성

    <script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.js"></script>

    -react cdn과 babel cdn을 사용해 html내부에서 리액트를 사용 가능

## [11월 10일]

#### 애플리케이션

    1. props와 state를 사용해서 간단한 Todo 애플리케이션을 만들 수 있음
    2. 이벤트 핸들러들이 인라인으로 각각 존재하는 것처럼 보이지만, 실제로는 이벤트 위임을 통해 하나로 구현됨

    ex)
    class TodoApp extends React.Component {
        constructor(props) {
            super(props);
            this.state = { items: [], text: '' };
            this.handleChange = this.handleChange.bind(this);
            this.handleSubmit = this.handleSubmit.bind(this);
        }

        render() {
            return (
            <div>
                <h3>TODO</h3>
                <TodoList items={this.state.items} />
                <form onSubmit={this.handleSubmit}>
                <label htmlFor="new-todo">
                    What needs to be done?
                </label>
                <input
                    id="new-todo"
                    onChange={this.handleChange}
                    value={this.state.text}
                />
                <button>
                    Add #{this.state.items.length + 1}
                </button>
                </form>
            </div>
            );
        }

        handleChange(e) {
            this.setState({ text: e.target.value });
        }

        handleSubmit(e) {
            e.preventDefault();
            if (this.state.text.length === 0) {
            return;
            }
            const newItem = {
            text: this.state.text,
            id: Date.now()
            };
            this.setState(state => ({
            items: state.items.concat(newItem),
            text: ''
            }));
        }
    }

        class TodoList extends React.Component {
        render() {
            return (
            <ul>
                {this.props.items.map(item => (
                <li key={item.id}>{item.text}</li>
                ))}
            </ul>
            );
        }
    }
    ReactDOM.render(
    <TodoApp />,
    document.getElementById('todos-example')
    );

#### 외부 플러그인을 사용하는 컴포넌트

    1. React는 유연하며 다른 라이브러리나 프레임워크를 함께 활용할 수 있음
    2. 예제에서는 외부 마크다운 라이브러리인 remarkable을 사용해 textarea의 값을 실시간으로 변환됨

    ex)
    constructor(props) {
        super(props);
        this.md = new Remarkable();
        this.handleChange = this.handleChange.bind(this);
        this.state = { value: 'Hello, **world**!' };
    }

    handleChange(e) {
        this.setState({ value: e.target.value });
    }

    getRawMarkup() {
        return { \_\_html: this.md.render(this.state.value) };
    }

    render() {
        return (
        <div className="MarkdownEditor">
        <h3>Input</h3>
        <label htmlFor="markdown-content">
        Enter some markdown
        </label>
        <textarea
                id="markdown-content"
                onChange={this.handleChange}
                defaultValue={this.state.value}
                />
        <h3>Output</h3>
        <div
                className="content"
                dangerouslySetInnerHTML={this.getRawMarkup()}
                />
        </div>
        );
    }

    ReactDOM.render(
    <MarkdownEditor />,
    document.getElementById('markdown-example')
    );

#### markdown-editor

    1. npx create-react-app markdown-editor
    2. npm i remarkable --save
    3. 필요없는 파일 삭제

#### 영화 앱 깃허브에 배포

    1. package.json 파일을 열어 homepage 키와 키값을 browserslist 키 아래에 추가
    2. package.json 파일에 scripts 키값으로 명령어를 추가
    3. (git add./git commit -m ""/git push origin master)의 명령어를 사용하여 깃허브에 업로드
    4. (npm install gh-pages)의 명령어를 사용하여 gh-pages를 설치
    5. (git remote -v)의 명령어를 입력하여 업로드한 깃허브 저장소의 주소를 확인
    6. (npm run deploy)의 명령어를 입력하여 영화 앱을 배포
    7. URL에 'https://계정.github.io/저장소 이름'을 입력해서 확인가능

#### React의 특징

    1. 캡슐화된 컴포넌트로 개발되어 재사용이 용이
    2. DOM과는 별개로 상태를 관리가능
    3. 상호작용이 많은 UI개발에 적합
    4. 기존 코드와 별개로 개발이 가능

#### js파일 없이 리액트 앱 생성

    <script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.js"></script>

    -react cdn과 babel cdn을 사용해 html내부에서 리액트를 사용 가능

## [11월 03일]

#### 컴포넌트 설치 오류 등 원인 규명이 되지 안은 오류가 있을 경우

    $ npm cache clean --force
    $ npm rebuild
    $ rm -rf node_modules
    $ npm install

    1. rm명령이 실행되지 않으면 shell을 관리자 권한으로 실행한 후 다시 시도
    2. 안될경우 탐색기에서 삭제(시간이 조검 걸릴수 있음)
    3. cache clean과 rebuild를 통해 많은 부분 해결 가능

#### package.json과 package-lock.json 차이

    1. package.json은 패기지 의존성 관리 파일
    2. 동일한 개발환경을 구성해야 할때 사용하는 것이 package.json
    3. 오류가 있거나 재설정 해야되는경우도 유용하게 사용가능
    4. package-lock.json파일이 있으면 npm install명령은 package.json 대신 package-lock.json 사용하여 모듈생성

#### 영화 상세 정보 기능 만들어보기

    1. console.log를 통해 About으로 어떤 props가 넘어오는지 확인
    2. route props 살펴보기
    - function About(props) {
        console.log(props);
    3. route props에 데이터 담아 전송
    - <Link to={{pathname:'/about', state:{frameElement:true}}}>About</Link>
    4. Navigation컴포넌트를 정리
    - <Link to="/about">About</Link>
    5. Movie 컴포넌트에 Link 컴포넌트를 추가
    - <Link
      to={{
        pathname:'/movie-detail',
        state:{year,title,summary,poster,genres},
        }}
      >
    6. Detail 컴포넌트를 작성
    - import React from "react";
      function Detail(props){
         console.log(props);
         return <span>hello</span>;
      }
    7. App.js를 연 다음 Detail 컴포넌트를 임포트하고 Route 컴포넌트에 Detail컴포넌트를 교재와같이 작성
    - import React from "react";
        function Detail(props){
        console.log(props);
        return <span>hello</span>;
        }
      export default Detail;
    8. 영화 카드를 클릭해서 /movie-detail로 이동한 후 화면에는 Detail 컴포넌트가 출력하고 있는 hello라는 문장을 확인
       그리고 console탭에서 Movie 컴포넌트에서 Link를 통해 보내준 데이터가 들어있는지 확인
    9. URL에 /movie-detail로 바로 이동한 후 console탭에 영화 데이터가 들어있는지 확인
       (영화의 데이터가 없어 state가 undefined으로 나옴)

## [10월 27일]

#### react-router-dom 설치 및 프로젝트 폴더 정리

    1. 터미널에 npm install react-router-dom 입력 설치
    2. components 폴더에 Movie 컴포넌트 옮기기
    - src/components 폴더 만들고 Movie컴포넌트 이동
    3. routes 폴더에 라우터가 보여줄 화면 구현
    - src/routes 폴더 만들고 Home.js와 About.js 파일 생성
    4. Home.js 수정
    - App.js내용을 Home.js로 복사하고 컴포넌트 이름을 Home으로 수정
    - Home.css를 생성하고 Home.js에 import
    5. Home.css 구현
    6. App.js 수정

#### 라우터 구현

    1. 라우터는 사용자가 입력한 URL을 통해 특정 컴포넌트를 불러줌
    ex) loclahost:3000/about
    2. App.js에 HashRouter와 Route 컴포넌트 import하고 적용
    - HashRouter컴포넌트가 Route컴포넌트를 감싸 반환하도록 App.js를 수정
    3. Route 컴포넌트에 path, component props 추가
    - About 컴포넌트를 import
    - import About from './routes/About';
      <Route path="/about" component={About} />
    4. About.js 수정
    - import React from "react";
      function About(){
        return <span> abcdefg </span>
      }
      export default About;
    5. 라우터 테스트
    - localhost:3000/#에 path props로 전달했던 값 /about을 붙여 다시 접속
    -  작성했던 내용이 출력확인
    6. Home 컴포넌트를 위한 Route컴포넌트 추가
    - App 컴포넌트에 Home 컴포넌트를 import하고, 또 다른 Route 컴포넌트를 추가
    - import Home from './routes/Home';
      <Route path="/" component={Home} />
    7. 라우터 테스트 후 App.js 원래대로 복구
    - <Route path = "/" component={Home} />
      <Route path = "/about" component={About} />
    8. Route 컴포넌트에 exact props 추가
    - exact={true}
    9. About.css 작성

#### 네비게이션 구현

    1. Navigation 컴포넌트 만들기
    - import React from "react";
      function Navigation(){
        return (
            <div>
                <a href="/">Home</a>
                <a href="/about">About</a>
            </div>
        );
      }
    2. Navigation 컴포넌트 App 컴포넌트에 포함
    - App컴포넌트에 Navigation을 import시키고, <HashRoute>에서 불러옴
    - import Navigation from "./components/Navigation"; <Navigation />
    3. Home 링크 눌러보기
    - react-router-dom의 Link 컴포넌트를 사용
    4. a 태그 Link 컴포넌트로 변경
    - Navigation 컴포넌트에 Link 컴포넌트를 import하고 a 태그를 Link 컴포넌트로 수정
    - href속성은 to로 수정
    - import {Link} from 'react-router-dom'
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
    5. Navigation 컴포넌트 위치 확인 후 컴포넌트 스타일링
    - • Navigation 컴포넌트에 css를 import하고 이를 적용

## [10월 13일]

#### Movie 컴포넌트 만들기

    1. src폴더에 Movie.js 파일을 새로 생성
    2. 컴포넌트의 기본 골격을 작성
    3. Movie 컴포넌트는 state가 필요하지 않으므로 클래스형 컴포넌트가 아닌, 함수형 컴포넌트로 작성
    4. Movie에 넘어와야 하는 영화 데이터를 정의하고, 관리하기 위해 prop-types를 사용
    5. Movie.propTypes 작성
    - 먼저 id를 Movie.propTypes를 추가
    - id의 자료형은 Number이고, 반드시 있어야 함으로 PropType.number.isRequired로 작성
    - year, title, summary, poster를 각각 Movie.propTypes에 추가(poster props는 영화 포스터 이미지 주소를 저장하기 위한 것)
    6. 노마드 코더 영화 API 정렬 기능 사용
    - yts.lt/api#list_movies에 접속한 다음 Endpoint Parameters에 sort_by라는 Parameter 사용/ Examples를 참고
    - yts-proxy.now.sh/list_movies.json?sort_by=rating 접속 후 평점을 기준으로 내림차순으로 영화 데이터를 정렬 확인가능
    7. axios.get() 수정
    - axios.get()에 yts-proxy.now.sh/list_movies.json?sort_by=rating을 전달
    8. Movie 컴포넌트에서 props를 추가, 출력
    - Movie 컴포넌트에서 id, title, year, summary, poster props를 받아 출력할 수 있도록 수정
    - App 컴포넌트에서 Movie컴포넌트를 그릴 때 title만 우선 출력
    - 음식 앱을 만들 때 컴포넌트를 map() 함수로 출력했던 것과 같이 코딩
    ex) funtion Movie ({id,title,year,summary,poster}){
        return <h4>{title}</h4>;
        }
    9. App 컴포넌트에서 Movie컴포넌트 그리기
    - We are ready를 출력하고 있는 자리, 즉 로딩이 완료 되면 실행되는 자리에 movies.map()을 사용
    - map() 함수의 첫 번째 인자로 컴포넌트를 반환하는 함수를 전달
    ex) render(){
        const { isLoading,Movies } = this.state;
        return <div>{isLoading ? 'Loading...' :movies.map()</div>};
        }
    10. map() 함수에 컴포넌트를 반환하는 함수 전달
    11. Movie 컴포넌트를 반환하도록 movies.map() 수정
    - App.js에 Movie 컴포넌트를 import한 다음, movies.map()에 전달한 함수가 <Movie />를 반환
    ex) import moive from './Movie'
    12. Movie컴포넌트에 props 전달
    - Movie컴포넌트에 props 전달
    - 단, poster props의 경우 키 이름이 medium_cover_image이므로 movies.poster가 아니라 movies.medium_cover_image라고 작성
    - App을 실행해 보면 영화 제목이 나오는 것을 확인가능
    ex)<Movie
        id={movie.id}
        year={movie.year}
        title={movie.title}
        summary={movie.summary}
        poster={moive.medium_cover_image}
        />
    13. console탭에서 영화 데이터 확인
    14. key props 추가
    - key props는 유일해야 함으로 API에서 넘겨주는 id값을 사용
    - console.log()는 사용하지 않음으로 삭제
    ex) movie.map((movie)=>{
        ~~console.log(movie);~~
        return(
            <Movie
        id={movie.id}
        year={movie.year}
        title={movie.title}
        summary={movie.summary}
        poster={moive.poster}
        />
        )
    })

#### 영화 앱 전체 모습 수정

    1. App.css 내용 모두 지우고 없다면 새로 생성
    2. Movie 컴포넌트에 genres props 넘겨주기
    - 런타임(runtime) 아래 있는 장르(genres)를 추가
    3. Movie 컴포넌트 수정
    - App컴포넌트에서 Movie컴포넌트에 genres props를 넘김
    4. App 컴포넌트 수정
    - genres props가 Movie 컴포넌트에 undefined로 넘어 왔다는 부분 부터 수정
    5. class 속성 이름 className으로 바꾸기
    - HTML의 class와 자바스크립트의 class라는 이름이 겹치면 리액트가 혼란스러울 수 있기 때문
    - abel문의 for element 과 유사
    - for=“name”이 아니라 htmlFor=“name”으로 작성
    - https://www.w3schools.com/tags/tag_label.asp 참조
    6. 영화 장르 출력
    - Movie컴포넌트에서 장르를 출력하도록 코드를 수정
    - genres props가 배열이므로 map()함수를 사용
    - genres props를 ul, li 태그로 감싸서 출력
    - console을 확인하면 kye props가 없다는 메시지가 나옴
    7.  li tag에 key props 추가
    - genre에는 key값으로 사용하기에 적당한 id값이 없으므로 2번째 매개변수를 지정할 경우 배열의 index값을 반환해 주는 기능을 사용

## [10월 06일]

#### componentDidMount() 함수

    componentDidMount()함수를 선언 후, 함수 안에 console.log() 함수를 작성하여 실행되는 시점 확인
    console을 통해 확인해 보면 render() 함수 실행 직후인 것을 확인가능

#### componentDidUpdate() 함수

    componentDidUpdate()함수의 실행 순서를 확인
    console에 출력되지 않음. 화면이 업데이트 되어야 함
    버튼을 클릭해서 화면을 업데이트 하면서, console을 확인
    버튼을 클릭하면 setState()함수가 실행되고, render()함수로 화면이 업데이트된 직후 componentDidUpdate()함수가 실행

#### componentWillUnmount() 함수

    위의 componentDidUpdate() 함수와 같이 테스트 하지만 실행여부 확인불가
    이 함수는 컴포넌트가 화면에서 떠날 때 실행
    보통 컴포넌트에 적용한 이벤트 리스너를 제거할 때 많이 사용

#### 영화 앱 만들기 워밍업

    1. App 컴포넌트 비우기
    2. 영화 데이터 로딩 상태 표시
    - state를 선언하고, isLoading키 생성 후 키값을 true로 설정
    - 데이터가 없기 때문에 확인을 위해 true로 세팅
    3. 구조분해할당 + 삼항연산자 = 로딩상태 출력
    - 구조분해할당으로 this.state에 있는 isLoading을 상수로 선언하여, 앞으로 this.state를 쓰지 않아도 사용가능하도록 만듬
    - 삼항연산자를 사용해서 isLoading이 true면 'Loading...'을 출력하고 false면 'We are ready'를 출력
    4. false면 'We are ready'를 출력
    - setTimeout()함수의 첫 번째 인자는 실행할 함수이고, 두 번째 인자로 전달한 값은 지연시간임
      ㄴ두 번째 인자 시간만큼 지난 후 첫번째 인자의 함수를 실행(시간의 단위는 msec)
    - life cycle함수인 componentDidMount()함수를 사용하여 먼저 Render() 함수가 수행된 후 setTimeout()함수가 실행
    5. 영화 데이터를 어디에 저장할지 찾기
    6. movies state를 만들기 교재와 같이 배열로 코드 작성

#### 영화 API사용

    1. axios 설치
    - 터미널에 npm install axios 입력후 설치
    2. YTS영화 데이터 API
    - 브라우저에 yts.lt/api 입력후, YTS영화 데이터 API 사이트에 접속
    - 사용할 API는 'List Movies API
    - List Movies를 클릭 -> API는 특정 주소를 입력하면 그 주소에 맞는 결과를 보내 줌
    3. 영화 목록 데이터 확인
    - 브라우저에서 Endpoint의 주소 중 json으로 끝나는 주소를 입력 (https://yts.mx/api/v2/list_movies.json)
    4. JSON Viewer 확장 도구 설치
    - JSON Viewer라는 확장 도구를 설치하면 정상적으로 볼 수 있음
    - JSON Viewer라고 검색후 설치
    5. 노마드 코더 영화 API를 사용 (https://github.com/serranoarevalo/yts-proxy)
    - 다른 endpoint와 함께 노마드 코더 영화 API를 사용하려면, yts-proxy.now.sh에 endpoint를 붙이면 됨
    6. 영화 정보 더 자세히 살펴보기
    - 주소창에 yts-proxy.now.sh/movie_details.json라고 접속하면 아무 것도 출력되지 않음
    - API가 movie_id라는 조건을 요구하기 때문
    7. 영화 정보를 더 자세히 보기 위해 조건 추가
    - yts.mx/api#mivie_details에 접속하면 movie_details Endpoint는 movie_id가 필수임을 알수 있음
    - 즉 yts-proxy.now.sh/list_movies.json에 movie_id를 추가해야 함
    - Example에 있는 주소를 보면 movie_id를 어떻게 추가하는지 알 수 있음
    8. 노마드 코더 영화 API를 영화 앱에서 호출
    - API를 사용하려면 axios를 import한 다음, componentDidMount()함수에서 axios로 API를 호출
    - axios.get()함수의 인자에 URL을 전달하여 API를 호출
    9. axios의 동작 확인
    - network탭을 열고 브라우저 새로고침
    - name이라는 항목에 list_movies.json이라고 나온 부분이 바로 axios가 API를 호출해서 발생한 것
    10. getMovies()함수를 만들고, 이 함수 안에서 axios.get()이 실행
    - axios.get()의 return값은 movies에 저장
    - componentDidMount()함수가 실행되면 this.getMovie()가 실행
    11. 시간이 필요하다는 것을 알리기 위해서는 async, await 키워드가 필요
    - async를 ()앞에 붙이고, 실제 시간이 필요한 대상인 axios.get()함수 에는 await을 붙임

#### 영화 데이터 화면에 그리기

    1. console.log() 함수로 영화 데이터 출력
    - API가 보내준 데이터는 movies에 들어가 있을 것으로 console에서 출력확인
    2. 출력된 데이터를 세심히 살펴 어떻게 사용할 지를 구상
    - 특히 data키에 집중
    3. 객체에 있는 movies키에 접근
    - movies변수에 있는 movies 키의 값을 추출
    - movies.data.data.movies와 같이 수정한 후 콘솔에서 확인
    4.  movies state에 영화 데이터 저장
    - this.setState({ movies: movies })와 같이 작성해서 movies state에 영화 데이터를 저장
    - consol을 통해 확인하지 않아도 되니 이 부분은 삭제
    5. ES6에서는 객체의 키와 대입할 변수의 이름이 같다면 코드를 축약 가능
    - this.setState({ movies: movies })를 this.setState({ movies })로 수정
    6. isLoading state를 true에서 false로 업데이트
    - 앱을 실행하면 여전히 Loading만 출력되지만 “영화 데이터의 출력”를 출력하려면 isLoading state의 값을 true에서 false로 업데이트
    - 처음에는 Loading...이 화면에 나타나다가 조금 시간이 지나면 We are ready로 변하는 것을 확인
    - “영화 데이터의 출력” 대신 movies state를 출력하면 됨

## [09월 29일]

#### 음식 앱에 prop-types 도입

    - 음식 데이터에 rating 추가
    1. foodLike 배열의 각 요소에 rating을 추가
    2. 값의 자료형은 number로 함
    3. Rating props를 Food 컴포넌트에 전달하면서 이 값을 검사
    4. 검사하기 위해서 props의 자료형을 검사할 수 있도록 만들어 주는 prop-types라는 도구가 필요
    - prop-types 설치
    5. 터미널에서 npm install prop-types를 입력하여 설치
    6. 정상설치 여부확인을 위해 Package.json 파일을 열어 dependencies 키에 있는 Prop-types 등록 확인
        (Prop-types는 컴포넌트가 전달받은 props가 원하는 값인지 확인하는 역할)
    - prop-types 적용
    7. import PropTypes from ‘prop-types’; 를 App.js 파일 맨 위에 추가
    8. ration props를 Food 컴포넌트에 전달
    - Food.propTypes 작성
    9. 모든 props는 문자열이고 반드시 있어야 한다는 조건을 추가
    - Food.propTypes의 rating 키 값 확인
    10. isRequired는 필요하다는 뜻으로 rating에는 string이라는 자료형이 반드시 필요
    11. 콘솔 탭에 뜬 오류 메시지 해결 필요
    - prop-types 경고 해결
    12-1. rating: PropTypes.string.isRequired 대신 rating: PropTypes.number.isRequired 로 교체
    12-2. Console탭을 확인해 보면 prop type 경고 메시지가 사라짐
    - 다른 종류의 prop-types 경고 해결
    13-1. Picture props의 이름을 image로 교체
    13-2. 화면에 사진이 나오지않지만 console에서 오류 내용 확인
    13-3. Food 컴포넌트에 picture라는 이름의 props가 필요하지만, 그 값이 undefined
    - isRequired의 뜻
    14. rating props는 필수가 아니어도 되는 항목
    15. 다만 값이 전달되는 경우 자료형이 number여야함

### State와 클래스형 컴포넌트

#### State로 숫자 증감 기능 만들기

    props는 정적인 데이터만 다룰 수 있음
    state는 동적인 데이터를 다루기 위해 사용
    state는 class형 컴포넌트에서 사용
    기존의 App.js는 04-App.js로 이름을 바꾸고 새로운 App.js 파일을 생성

#### render() 함수

    App컴포넌트가 JSX를 반환해야 하지만 class형 컴포넌트에서는 바로 return을 사용할 수 없음
    render() 함수 내에서 return문을 사용
    함수형 컴포넌트는 return문이 JSX를 반환하지만, 클래스형 컴포넌트는 render()함수가 JSX를 반환

#### State 정의

    class안에 state = { }라고 작성하여 state를 정의
    state는 객체형태의 데이터
    state를 사용하려면 반드시 class형 컴포넌트 안에서, 소문자를 사용하여 state라고 적음
    state에 count키와 키값을 0으로 추가
    return함수로 count값을 반환 > render함수에서 this.state.count를 출력

### Life Cycle

#### constructor() 함수

    클래스 내에 constructor()(생성자)함수를 선언하고, console.log()함수를 이용해 문장을 출력
    어떤 문장이 먼저 샐행되는지 비교하기 위함
    실행 후 console에서 확인하면 constructor이 먼저 실행된 것을 확인 가능

#### 생성자

    constructor()는 Component를 생성할 때 state 값을 초기화하거나 메서드를 바인딩할 때 사용
    자바스크립트에서 super는 부모클래스 생성자의 참조한다는 의미
    생성자 내에서는 setState를 사용하지 않고, this.state를 사용하여 state의 초기값을 할당
    생성자 내에서는 외부API를 직접 호출할 수 없음
    필요 하다면 componentDidMount()를 사용

#### 생명주기함수

    컴포넌트의 생성 ->생성 직후 ->props/state의 변화 ->update의 처리 ->처리 직후 ->컴포넌트 제거

## [09월 15일]

#### props

    컴포넌트에서 컴포넌트로 전달하는 데이터
    함수의 매개변수 역할
    -props를 사용하면 컴포넌트를 효율적으로 사용가능
    ex)
    function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
    }

#### 구조 분해 할당

    구문은 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 JavaScript 표현식
    ES6의 문법 중 구조 분해 할당을 사용하면 점 연산자를 사용하지 않아도 됨
    데이터 개수가 많아지면 구조 분할 할당을 사용하는 것이 편리함
    ex)
    let a, b, rest;
    [a, b] = [10, 20]
    console.log(a);
    // 출력결과 : 10
    console.log(b);
    // 출력결과 : 20
    [a, b, ...rest] = [10, 20, 30, 40, 50]
    console.log(rest);
    // 출력결과 : Array [30,40,50]

#### map()

    map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환함
    f12를 통해 개발자 도구를 통해 console탭으로 이동하여 사용가능
    ex)
    const array1 = [1, 4, 9, 16];
    // map() 에 함수전달
    const map1 = array1.map(x => x * 2);
    console.log(map1);
    // 출력결과 : Array [2, 8, 18, 32]

#### import를 사용한 이미지 삽입 방법

    상대경로를 사용하여 이미지 삽입방법중 하나(require 방법도 있음)
    반드시 src폴더 내부에 이미지를 저장해야됨
    ex)
    import img01 form './images/1.jpg'

## [09월 08일]

### create-react-app

#### create-react-app 앱 구현

    명령 프롬프트를 실행한 다음 리액트 앱을 만들고 싶은 곳으로 이동 후 명령으로 movie_app_2021라는 폴더를 생성
    npx create-react-app movie_app_2021 사용

#### 프로젝트 폴더 선택

    VS code를 실행하여 파일 → 폴더열기를 누른 다음 movie_app_2021 폴더를 선택

#### README.md 파일 수정

    README.md 파일을 연 다음, 그 안에 작성되어 있던 내용을 모두 지워, 새로운 내용을 입력

#### package.json 파일 수정

    test, eject 명령어는 사용하지 않아서 삭제

#### 리액트 앱 실행

    명령 프롬프트에서 루트 폴더로 이동한 다음  npm start를 입력
    명령 프롬프트에 'Compiled Successfully'와 같은 문장이 보인 다음, 크롬 브라우저가 켜지고 다음 화면이 나타나면 완료
    리액트 종료하려면 Ctrl + c를 누르면 리액트 앱이 종료

#### 로컬 저장소 초기화

    git init 명령어를 실행하면 저장소를 초기화

#### 깃허브에 저장소 생성

    깃허브 페이지에 접속 > 깃허브와 vscode를 연결 후 커밋 > 푸쉬
    푸쉬가 완료되면 깃허브 저장소를 확인해보면 변경된 README.md 확인

#### 리액트 앱 프로젝트 폴더

    VScode의 왼쪽 화면을 이용해서 movie_app_2021폴더를 열어서 public 폴더, src 폴더 확인

#### index.js 파일 수정

    src 폴더의 index.js 파일을 열고 다음과 같이 삭제선으로 표시한 코드를 삭제하고 수정

#### App.js 파일 수정

    src 폴더의 App.js 파일을 수정 > 삭제선으로 표시한 코드를 삭제하고 수정 새로운 내용을 추가

#### 리액트 동작 원리

    리액트는 작성한 프로젝트 폴더에 있는 코드를 자바스크립트를 이용하여 해석하고 해석한 결과물을 index.html 에 삽입
    index.html 파일에 없던 <div>Hello!!!</div>가 리액트 앱을 실행하면 생성됨

#### index.js 파일로 컴포넌트의 사용

    <App />을 ReactDOM.render(<App />, document.getElementById('root'))로 변경
    APP 컴포넌트가 변환하는 것들을 화면에 그릴수 있고 App 컴포넌트가 그려질 위치는 ReactDOM.render() 함수의 두 번째 인자로 전달
    리액트는 컴포넌트와 함께 동작하고, 리액트 앱은 모두 컴포넌트로 구성됨

#### Potato 컴포넌트 생성

    1. src 폴더 안에 Potato.js라는 이름의 파일을 만들고 맨위에 importReact from'react' 입력
    2. Potato 컴포넌트의 기본플이 완성 > function Potato() {}를 입력
    3. return <h3>I love potato </h3>를 입력
    4. export defult Potato;를 입력

#### Potato 컴포넌트 사용

    1. src폴더 안에 Potato.js 파일의 내용을 복사
    2. App 파일 안에 붙혀 넣기를 하고 import 부분을 import Potato from './Potato'
