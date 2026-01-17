
- 디버깅 중 값 변조
	- 현재 상태의 필드 값 직접 할당
    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image001.png?raw=true)
	
		1. 값 변조 테스트
			a. GS_ZSC-CARRID  필드를 조회 변수에 등록
			a. ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image003.png?raw=true) 버튼으로 입력창 활성
			a. 예외 발생 조건문에 true 상태로 값 변경
				1) 위 케이스는 값을 초기화 직후 값 상태로 변경(빈값)
AA -> (빈값)
			a. 예외 발견 분기 흐름 진행 확인
			    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image005.png?raw=true)

		1. 결과
			a. 변조한 첫 행의 인식이 디버깅에서 변경되어 반환된 내용
			![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image007.png?raw=true)
			

- PERFORM 
	- 기본적으로 Call by Reference로 perform에 전달되는 값은 주소가 전달되므로 내부에서 값이 바뀌면 주소가 가리키는 대상의 실제 값을 바꾸는 것이므로 변경된 값이 할당되며, 참조형 변수도 동일하게 동작
		- form using
			- 대상 변수  
			![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image009.png?raw=true)
			
			- 대상 form 선언
			![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image011.png?raw=true)
			
			- debugging 
				- 값 변경 전
				![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image013.png?raw=true)
				- 값 변경 후
				![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image015.png?raw=true)
				
		- form changing
			- 대상 변수  
			![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image009.png?raw=true)
			- 대상 form 선언
			![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image017.png?raw=true)
			- debugging
				- 값 변경 전  
				![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image019.png?raw=true)
				- 값 변경 후
				![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image021.png?raw=true)
	
	- USING
		- 해당 subroutine은 내부에서 변동이 없을 것이란 표현
		- 표현 뒤에 변수를 선언한다고 해당 변수의 값만 전달되진 않으며, 값만 전달되게 하려면 별도 선언 방식 사용
			- USING VALUE(p_val) - 외부로 부터 전달되는 변수의 값을 그대로 복사해서 내부에서 사용하므로 외부값은 변하지 않음
				- 예
					- 대상 변수  
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image023.png?raw=true)
					- 대상 form
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image025.png?raw=true)
					- debugging
						- 변경 전  
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image027.png?raw=true) 
						- 변경 후  
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image029.png?raw=true)  
						- 종료 후  
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image031.png?raw=true) 
			- 단 전달되는 변수가 참조형인 경우, 해당 주소의 멤버필드가 변경되는 건 저장 대상에 포함되지 않음
				- 예
					- 대상 변수  
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image033.png?raw=true) 
					- 대상 form  
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image035.png?raw=true) 
					- debugging
						- 변경 전
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image036.png?raw=true) 
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image038.png?raw=true) 
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image040.png?raw=true) 
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image042.png?raw=true) 
						- 변경 후
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image044.png?raw=true)  
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image046.png?raw=true)  
						- 종료 후
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image048.png?raw=true)   
	
	
	- CHANGING
		- 해당 subroutine은 내부에서 변동이 발생할 수 있음을 표현
		- 표현 뒤에 변수 대상으로 별도 처리를 수행하게 하려면 별도 선언 방식 사용
			- CHANGING VALUE(p_val) - 외부로 부터 전달되는 변수의 값을 함수내에 복사해서 사용하고, 정상 종료 시 해당 변수에 변경된 값 전달
				- 해당 변수 값 변경 발생 이후 함수 내부에서 예외가 발생하게 되면 외부에서 전달된 값 유지
				- 참조형 변수가 외부로 부터 전달됐을 때도 마찬가지로 외부에서 전달받은 참조형 변수가 가리키는 주소를 가진 내부용 변수에 이를 복사하여 사용하고 정상 종료 시에만 변경된 주소값을 외부에서 전달해준 변수에 할당
				- 예
					- 대상 변수  
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image050.png?raw=true)  						 
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image052.png?raw=true)   
					- 대상 form  
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image054.png?raw=true)   
					- debugging
						- 변경 전
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image056.png?raw=true)   
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image058.png?raw=true)    
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image060.png?raw=true)    
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image062.png?raw=true)    
						- 변경 후(일반변수)
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image064.png?raw=true)    
						- 변경 후(참조형변수)
						   - 새로운 구조체 생성 후 함수로 전달된 변수에 값 할당  
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image066.png?raw=true)    
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image068.png?raw=true)    													
						  - 함수로 전달된 변수의 주소 유지 확인
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image070.png?raw=true)
						- 함수종료 후
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image072.png?raw=true) 
						![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image074.png?raw=true)
						
- 변수
	- TYPE
		- 원시 값을 담는 타입 변수
		- 고정 길이 타입
			- 숫자
				- i(4byte), p(금액, 환율, 수량 등 정확성 요구될 시 사용, 바이트 길이, 소수점 자릿수), f(8byte 부동소수점), int8
			- 문자, 바이트
				- c(문자), n(숫자로된 문자), d(날짜), t(시간), x(바이트, 16진수)
		- 가변 길이 타입
			- string(character, text), xstring(byte, raw data / binary)
		- 선언방식
			- 즉시
			```ABAP
			DATA(v_text)  = 'test'.
			```
			- 사전선언
			```ABAP
			DATA: v_text TYPE string.
			```			
	- 참조형 변수
		- REF TYPE TO
		- Data 참조(Complex 타입)
			- 비 참조형 변수들을 하나의 객체 내에 담는 형태, 내장 함수 없음
				- structure, internal table 등
			- 동일 타입의 변수에 동일 타입의 값을 할당하면 내용이 복사되어 할당
				- 예
					- 할당 전  
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image076.png?raw=true)
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image078.png?raw=true)
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image080.png?raw=true)
					- 할당 후  
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image082.png?raw=true)
					![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image084.png?raw=true)
					
			- 참조형 변수이지만 클래스형태가 아니므로 객체지향 특성을 이용할 수 있는 대상이 아님
		- Object 참조(Class 타입)
			- 비 참조형, 참조형, 내장 함수 등을 담는 클래스
			- 동일 타입의 변수에 동일 타입의 값을 할당하면 원본 변수가 가진 주소를 할당
			- 클래스형태이므로 객체지향 특성을 활용 가능
			  > 부모 객체 타입의 변수의 자식 객체 타입의 변수 할당
		- 선언방식
			- 즉시
				- 테이블 row
				```ABAP
				SELECT * FROM SPFLI
				INTO TABLE @DATA(lt_spfli)." 테이블 조회결과 할당
				```
				- loop 요소 
				```ABAP
				LOOP lt_spflis INTO DATA(ls_spfli). "루프 요소 구조체 직접 할당
				...
				ENDLOOP.
				```
				- 클래스
				``` ABAP
				DATA(cl_sample) = new sample(). " 클래스 접근 변수 직접 할당
				```
			- 사전선언
				```ABAP
				DATA: 
				lt_spfli TYPE TABLE OF SPFLI,
				ls_spfli TYPE SPFLI,
				cl_sample TYPE REF TO sample.
				```
- FIELD-SYMBOL
	- 일반 변수와 달리 실제데이터의 메모리 주소를 담는 형태
	- PERFORM 전달 시 VALUE() 형태 사용 불가
		- 실제 값을 담은게 아닌 메모리 주소에 대한 별칭참조형 변수와 유사한 접근 방식이나 ABAP 내부에서 참조형 변수와 아에 다른 형태로 취급되어 불가능
	- 선언 방식
		- 즉시
		```ABAP
		FIELD-SYMBOL(<심볼명>) " <심볼명> 으로 선언되어 문법 사용한 라인 및 하위 라인에서 접근 가능
		```
		- 사전 선언
		```ABAP
		FIELD-SYMBOLS: <심볼명1> TYPE any. " 변수 이후에 코드에서 접근할 수 있게 선언
		```
	- debugging
	![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw19_homework/image086.png?raw=true)
		1. gt_zsc의 1개 행의 메모리 정보를 <fs_zsc> 에 할당
		1. 로우의 MANDT 를 조회 변수에 등록
		1. 값 변조
			> 001 -> 002  
		1. Field Symbol를 통해 MANDT 값 변경 시, gt_zsc의 MANDT 필드값 변경 확인