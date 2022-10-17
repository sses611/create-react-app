## Frontend Clean Code
참고 : https://youtu.be/edWbHp_k_9Y

## 목차
### 실무에서 클린코드의 의의
  * 유지보수 시간의 단축, (시간 = 돈 = 자원)
     * 코드 파악의 용이
     * 버그 발생시 디버깅시간의 단축
     * 리뷰
     
### 안일한 코드 추가의 함정
  <details>
  <summary>As-is</summary>  
  <div markdown="1">

  ```JavaScript
  function QuestingPage(){
    async function handleQuesetionSubmit(){
      const 약관동의 = await 약관동의_받아오기();
      if(!약관동의){
        await = 약관동의_팝업열기();
      }
      await 질문전송(questionValue);
      alert("질문이 등록되었어요.");
    }
    return(
      <main>
        <form>
          <textarea placeholder="어떤 내용이 궁금한거요?"/>
          <Button onClick={handleQustionSubmit}질문하기</Button>
        </form>
      </main>
    )
  }
  ```
  </div>
  </details>
  
  <details>
  <div markdown="1">
    1. 하나의 목적인 코드가 떨어져 있어서 이후 기능탐색시 미로찾기를 해야한다. =응집도<br/>
    2. handleQuestionSubmit 함수가 3가지 일을 하고 있다. =단일책임<br/>
    3. 함수의 세부 구현단계가 제각각이다. =추상화
  </div>
 
  <summary>As-is(Add)</summary>  
  <div markdown="1">

  ```JavaScript
  function QuestingPage(){
    const [popupOpened, setPopupOpened] = useState(false);
    async function handleQuesetionSubmit(){
      const 약관동의 = await 약관동의_받아오기();
      if(연결전문가 !== null){
          setPopupOpened(true);
      }else
        const 약관동의 = await 약관동의_받아오기();
        if(!약관동의){
          await 약괸동의_팝업열기();
        }
      await 질문전송(questionValue);
      alert("질문이 등록되었어요.");
    }
    return(
      <main>
        <form>
          <textarea placeholder="어떤 내용이 궁금한거요?"/>
          <Button onClick={handleQustionSubmit}질문하기</Button>
        </form>
        {popupOpened && {
          <연결전문가 팝업 onSubmit ={handleMyExpertQuestionSubmit} />
        }}
      </main>
    );
  ```
  </div>
  </details>
  
  <details>
  <summary>To-be</summary>  
  <div markdown="1">
    
  ```JavaScript
  function QuestingPage(){
    const 연결전문가 = useFetch(연결전문가_받아오기);
    
    async function handleQuesetionSubmit(){
      await 질문전송(questionValue);
      alert("질문이 등록되었어요.");
     
    async function handleMyExpertQuestionSubmit(){
      await 연결전문가_질문전송(questionValue, 연결전문가.id);
      alert(`${연결전문가.name}에게 질문이 등록되었어요.`);
    } 
     return(
      <main>
        <form>
          <textarea placeholder="어떤 내용이 궁금한거요?"/>
          {연결전문가.connected ? (
            <PopupTriggerButton
              popup={(
                <연결전문가가 팝업
                  onButtonSubmit={handleExpertQuestionSubmit}/>
              )}
            질문하기 </PopupTriggerButton>
           
           <Button onClick ={async () => {
            await openPopupToNotAgreedUsers();
            await handleMyExpertQuestionSubmit();
           }}
           </Button>
          </form>
      </main>
    );
  }
  async functon openPopupToNotAgreedUser(){
    const 약관동의 = await 약관동의_받아오기();
    if(!약관동의){
      await 약관동의_팝업열기();
    }
  }
  ```
  </div>
  </details>
  
### 로직을 빠르게 찾을 수 있는 코드 
*  클린코드 != 짧은코드가 아니다. / 찾고싶은 코드를 빠르게 찾을 수 있는 것
   * 뭉치면 쾌적 = 당장몰라도 되는 디테일
   * 뭉치면 답답 = 코드파악에 필수적인 핵심정보
* 핵심데이터와 숨겨도 될 세부구현을 나눈다.
* 선언적 프로그래밍, "무엇"을 하는 함수인지 빠른 이해 가능
* 함수이름을 중요포인트로 기능
* 리팩토링, 한가지 일만 하는 기능성 컴포넌트
      <details>
      <summary>예시1</summary>  
      <div markdown="1">

      ```JavaScript
      <button onClick={async () => {  /*버튼 클릭 함수에 로그찍는 함수와 API 콜이 석여 있다.*/
        log('제출 버튼 클릭')
        await openConfirm();
      }} />  


      <LogClick message="제출 버튼 클릭">  /*로그는 버튼을 감싼 컴포넌트에서 찍고, 버튼 클릭함수에서는 API콜만 신경쓴다*/
        <button onClick={openConfirm}/>
      <LogClick>
      ```
      </div>
      </details> 
        
      <details>
      <summary>예시2</summary>  
      <div markdown="1">

      ```JavaScript
      const targetRef = useRef(null);  /*Impresstion 옵저버를 다는 코드 세부 구현과 API 콜을 하는 코드가 섞여있다.*/
      useEffect(() => {
        const observer = new IntersectionObserver(
          ([{isIntersecting}]) => {
            if(isIntersection) {
              fetchCats(nextPage_;
            }
          }
        );
        return () => {
          observer.unobserver(targetRef.current);
        }
      };
        
      <InersectionArea onImpresstion={() => fetchCats(nextPage)} /> /*Impression 옵저버 세부 구현은 감싼 컴포넌트에 숨겨두고, 사용하는 입장에서는 Impression시 API 콜만 신경쓴다. */
      <div>더보기</div>

      ```
      </div>
      </details> 
        
      <details>
      <summary>예시3 -한글변수명 사용</summary>  
      <div markdown="1">

      ```JavaScript
        const 패널티풀림 = reasons.indexOF('PENALTY') > -1;
        const 평정4점이상 =review.rate >= 80;
        
        if(패널티풀림){
          return
        }
        if(평점 4점이상){
        }
        
        const 설계사정보팝업_노출 = 12345;
        const 설계사정보팝업_확인 = 54321;
        
        const handleMatchPlanner = async () => {
          log(설계사정보팝업_노출);
          const confirmed = await openConfirm();
          if(confirmed){
            log(설계사정보팝업_확인);
            goToChat(plannerId);
          }
        }

      ```
      </div>
      </details>
        
### 추상화
      <details>
      <summary>컴포넌트 추상화 예시</summary>  
      <div markdown="1">
      
       ```JavaScript
         <div style={팝업스타일}>
          <button onClick={asyc () => {
            const res = await 회원가입();
             if(res.success){
                프로필로이동();
           }
         }}>전송</button>

         <Popup onSubmit={회원가입} onSuccess={프로필로이동} />
      ```
      </div>
      </details> 
 
 
      <details>
      <summary>함수 추상화 예시</summary>  
      <div markdown="1">

      ```JavaScript
        const planner = 
          await fetchPlanner(plannerId)
        const label = planner.new ? '새로운 상담사' : '연결중인 상담사'
        
        const label = await getPlannerLabel(plannerId)  /*중요개넘을 함수 이름에 담아 추상화*/
      ```
      </div>
      </details> 

### 액션 아이템
* 담대하기 기존코드 수정하기
  * Pull Request에 File changes를 줄이고 싶다면, Mother branch를 따서 리팩토링한 PR보기
* 큰 그림 보는 연습하기
* 팀과 함꼐 공감대 형성하기
* 문서로 적어보기
  * 향후 어떤 점에서 위험할 수 있는지
  * 어떻게 개선할 수 있는지 
