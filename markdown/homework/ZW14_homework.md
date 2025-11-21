### 과제 - 기존 프로그램 Import 절차

1.  신규 프로그램 생성

    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image002.jpg)

2.  메인프로그램 붙여넣기

    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image004.jpg)

3.  Include 프로그램 생성
    > 각 프로그램 별로 에디터에 수신한 텍스트 내용 붙여넣기 - 총 6개 파일



    1.  에디터 화면에서 각각 새 프로그램에 맞게 이름 변경 후 진행

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image006.jpg)

    2.  대상 더블 클릭하여 프로그램 생성

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image008.jpg)

5.  Screen 생성  
    > 메인프로그램의 SELECTION 블록 뒤에 표현될 스크린 호출 영역의 스크린 번호

    1.  스크린 호출 정의 CALL SCREEN 뒤의 스크린 번호 더블클릭

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image010.jpg)

    2.  스크린 번호 더블 클릭 이후

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image012.jpg)

    3.  short description에 간단한 정보 입력 - 예시: 100 (참고: selection screen은 1000의 스크린번호를 가짐)

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image014.jpg)

    4.  상단 Layout 버튼으로 UI 제작화면으로 이동

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image016.jpg)

    5.  화면에 Custom Control 등록

        1.  ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image018.jpg) 클릭

        2.  Custom Control 영역 설정

            ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image020.jpg)

        3.   이름 지정 - CON01 - \*\_O01 파일에 있는 OUTPUT 정의 내용 중 CREATE OBJECT\_INSTANCE subroutine 정의를 통해 확인

            ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image022.jpg)

        4.  Element List에서 OK\_CODE 등록

            ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image024.jpg)

5.  Screen Flow Logic 등록

    1.  좌측 Object List 중 스크린 디렉터리 하위의 스크린 번호 선택하여 Flow Logic 에디터로 이동

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image026.jpg)

    2.  에디터 내 내용 등록

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image028.jpg)

    3.  버튼 Toolbar 등록

        1.  Flow Logic 내용 내 status\_0100 OUTPUT 내용의PF-STATUS '100' 더블클릭으로 툴바 등록 화면 진입

            ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image030.jpg)

        2.  Short description 입력 - 100

            ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image032.jpg)

        3.  SAVE, BACK, EXIT, CANC 버튼 코드 등록

            ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image034.jpg)

        4.  타이틀바 설정\- TITLEBAR 뒤 번호 더블클릭

            ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image036.jpg)

            \- 변경 윈도우에서 &1 입력 - &1을 통해 1개의 변수 내용을 매핑함을 정의한 상태, 여러 개 필요시 개수만큼 &n 추가

            ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image038.jpg)

6.  등록된 소스 Activatve

    1.  활성화 버튼 클릭 or Ctrl+F3

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image040.jpg)

    2.  대상 선택 - include 6개, screen 1개, GUI 상태 정의 1개, 메인프로그램 1개

        ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image042.jpg)

    3.  미완성 텍스트 작성 - \*\_TOP 소스 내 대부분의 변수 선언이 존재하기 때문에 Activate 이후 설정 가능
       
        \- Text Symbol
    
           ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image044.jpg)

        \- Selection Texts
    
           ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image046.jpg)

        \- Text Elements Activate 실행       
        
           ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image048.jpg)

7.  프로그램 실행

    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw14_homework/image050.jpg)
