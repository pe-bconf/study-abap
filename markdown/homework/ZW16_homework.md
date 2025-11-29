1.  버튼명 및 아이콘 수정

![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw16_homework/image002.jpg)

```ABAP
	FORM set_function_key .
	  g_function_key-icon_id   = icon_extra.
	  g_function_key-icon_text = 'STRAVELAG SAMPLE다운'.
	  g_function_key-text      = 'STRAVELAG SAMPLE다운'.
	  sscrfields-functxt_01    = g_function_key.
	
	  CLEAR g_function_key.
	ENDFORM. 
```
2.  download\_excel\_smpl 수정  
```ABAP
*sheet 명
  SET PROPERTY OF go_sheet 'Name' = 'STRAVELAG'. 
*column format
	  DEFINE _set_col_format.
	    PERFORM column_format USING &1 &2.
	  END-OF-DEFINITION.
	
	  _set_col_format:
	    1 '@',
	    2 '@',
	    3 '@',
	    4 '@',
	    5 '@',
	    6 '@',
	    7 '@',
	    8 '@',
	    9 '@',
	   10 '@',
	   11 '@',
	   12 '@',
	   13 '@'. 
*조회 대상 테이블 및 변수 타입 변경
	  DATA: tab1 TYPE TABLE OF STRAVELAG WITH HEADER LINE.	
	  SELECT * FROM STRAVELAG INTO TABLE tab1.

*클립보드 대상 내용 수정
* 헤더
	  CONCATENATE
	  'MANDT' 'AGENCYNUM' 'NAME' 'STREET' 'POSTBOX' 'POSTCODE' 'CITY' 'COUNTRY' 'REGION' 'TELEPHONE' 'URL' 'LANGU' 'CURRENCY'
	  INTO lt_clip SEPARATED BY lv_deli.
	  APPEND lt_clip.
	  CLEAR lt_clip.
	
* 복사 붙여넣기 대상
	  LOOP AT tab1.
	    rnum = sy-tabix + 3.
	    CONCATENATE TAB1-MANDT TAB1-AGENCYNUM TAB1-NAME TAB1-STREET TAB1-POSTBOX TAB1-POSTCODE TAB1-CITY TAB1-COUNTRY TAB1-REGION TAB1-TELEPHONE TAB1-URL TAB1-LANGU TAB1-CURRENCY
	    INTO lt_clip SEPARATED BY lv_deli.
	    APPEND lt_clip. " = APPEND WA INTO TB.
	    CLEAR lt_clip. " = APPEND WA.
	  ENDLOOP.

*붙여넣기 범위 수정
	  CALL METHOD OF go_application 'Cells' = e_cell1
	    EXPORTING
	      #1 = 3
	      #2 = 1.	
	
	  CALL METHOD OF go_application 'Cells' = e_cell2
	    EXPORTING
	      #1 = rnum
	      #2 = 13.	
	
	  CALL METHOD OF go_application 'Range' = e_range
	    EXPORTING
	      #1 = e_cell1
	      #2 = e_cell2.
	
	  CALL METHOD OF e_range 'Select'.	
	  CALL METHOD OF go_sheet 'Paste'.
 
*타이틀수정
    PERFORM fill_cell USING go_application 1 1 'STRAVELAG 테이블 엑셀 샘플양식'.

*엑셀 헤더 색상설정 범위 및 테두리 설정 영역 수정
	  CALL METHOD OF go_application 'CELLS' = cell1
	    EXPORTING #1 = 3 #2 = 1.
	
	  CALL METHOD OF go_application 'CELLS' = cell2
	    EXPORTING #1 = 3 #2 = 13.
	
	  CALL METHOD OF go_application 'Range' = range
	    EXPORTING #1 = cell1 #2 = cell2.
	
	  CALL METHOD OF range 'Interior' = lo_interior.
	  SET PROPERTY OF lo_interior 'Color' = 65535.
	
*그리드 데이터 입력된 영역 대상 스타일 적용을 위해 범위 수정
	  CALL METHOD OF go_application 'CELLS' = cell2
	    EXPORTING #1 = rnum #2 = 13.
	
	  CALL METHOD OF go_application 'Range' = range
	    EXPORTING #1 = cell1 #2 = cell2.
	
	  _set_border:
	   7,
	   8,
	   9,
	   10,
	   11,
	   12.
	
	  CALL METHOD OF range 'select'.
	  SET PROPERTY OF range 'HorizontalAlignment' = c_center. 

*제목 병합 영역 수정
*엑셀 좌표계로 선택
	  CALL METHOD OF go_application 'RANGE' = range
	    EXPORTING #1 = 'A1:M1'.
    CALL METHOD OF range 'Merge'.
```
3.  결과물       
![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw16_homework/image004.jpg)
