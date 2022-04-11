## 브라우저의 렌더링 과정(어떻게 동작하는지, 렌더링 과정은?)

- **DOM (Document Object Model), CSSOM (CSS Object Model) 생성**

서버로부터 받은 HTML, CSS를 다운로드 받습니다. 이 것들은 단순한 텍스트 파일이니 연산과 관리가 유리하도록 Object Model로 만들게 됩니다.
<br> HTML은 DOM Tree로, CSS는 CSSOM으로 만들어집니다.

- **Render Tree 생성**

이 둘을 이용하여 Render Tree를 생성합니다. Render Tree에는 스타일 정보가 설정되어 있고, **실제 화면에 표현되는 노드들로만 구성됩니다.**

- **Render Tree 배치**

Render Tree 생성이 끝나면 배치가 시작됩니다. **각 노드가 정확한 위치에 표시되기 위해 이동합니다.**

- **Render Tree 그리기**

배치가 완료되면 이제 실제 화면을 그리게 됩니다. 그럼 브라우저 화면에서 보여지게 되는거죠!
<br> 처리해야하는 스타일이 복잡할수록 paint 단계에 소요되는 시간이 깁니다.
