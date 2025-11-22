
1. 버튼 추가
   
    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw15_homework/image001.png?raw=true)
```ABAP
* 수정위치1 - *_F01 #SET_FUNCTION_KEY
...
  G_FUNCTION_KEY-ICON_ID   = ICON_EXTRA.
  G_FUNCTION_KEY-ICON_TEXT = 'SPFLI SAMPLE다운'.
  G_FUNCTION_KEY-TEXT      = 'SPFLI SAMPLE다운'.
  SSCRFIELDS-FUNCTXT_03    = G_FUNCTION_KEY.

  CLEAR G_FUNCTION_KEY.
...

* 수정위치2 - *_SEL FUNCTION KEY
...
SELECTION-SCREEN:
  FUNCTION KEY 1,
  FUNCTION KEY 3.
...

```


2. 버튼 이벤트 동작 정의 및 함수 수정
```ABAP
* 수정위치1 - *_F01 #ACT_FUNCTION_KEY
...
    CASE SSCRFIELDS-UCOMM.
        WHEN 'FC01'.
            PERFORM EXCEL_DOWN_SMPL USING ''.
        WHEN 'FC03'.
            PERFORM EXCEL_DOWN_SMPL USING 'SPFLI'.
    ENDCASE. 
...
	
* 수정위치2 - *_F01 #EXCEL_DOWN_SMPL
...
    FORM excel_down_smpl USING p_tb_name.
* 다운로드 양식 선택
    IF p_tb_name = 'SPFLI'.
	    LS_KEY-OBJID = 'ZTEST15_SPFLI_EXCEL01'.
	    LS_KEY-RELID = 'MI'.
    ELSE.
	    LS_KEY-OBJID = 'ZTEST14_EXCEL01'.
	    LS_KEY-RELID = 'MI'.
    ENDIF.
...	
* 엑셀 다운
	  PERFORM DOWNLOAD_EXCEL_SMPL USING p_tb_name LS_KEY-OBJID.
...
	
* 수정위치3 - *_F01 #EXCEL_DOWN_SMPL
...
    FORM download_excel_smpl  USING    
	                            p_tb_name
	                            p_ls_key_objid . 
...
	* 데이터 입력
	IF p_tb_name = 'SPFLI'.
	  SET PROPERTY OF GO_SHEET 'Name' = 'SPFLI'.
	  PERFORM FILL_CELL USING GO_APPLICATION 01: 01 'MANDT',
	                                               02 'CARRID',
	                                               03 'CONNID',
	                                               04 'COUNTRYFR',
	                                               05 'CITYFROM',
	                                               06 'AIRPFROM',
	                                               07 'COUNTRYTO',
	                                               08 'CITYTO',
	                                               09 'AIRPTO',
	                                               10 'FLTIME',
	                                               11 'DEPTIME',
	                                               12 'ARRTIME',
	                                               13 'DISTANCE',
	                                               14 'DISTID',
	                                               15 'FLTYPE',
	                                               16 'PERIOD'.
	
	ELSE.
	  SET PROPERTY OF GO_SHEET 'Name' = 'ZSCARR'.
	  PERFORM FILL_CELL USING GO_APPLICATION 01: 01 'MANDT',
	                                             02 'CARRID',
	                                             03 'CARRNAME',
	                                             04 'CURRCODE',
	                                             05 'URL'.
	ENDIF. 
	
...
```

3. Debug  - FILL_CELL 호출

    ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw15_homework/image003.png?raw=true)

4. 결과물

   ![](https://github.com/pe-bconf/study-abap/blob/main/attach/zw15_homework/image005.png?raw=true)

