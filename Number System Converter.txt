INCLUDE Irvine32.inc

.data
	Heading byte "                                         NUMBER SYSTEM CONVERTER",0dh,0ah,0
	main_heading byte "				       NUMBER SYSTEM CONVERTER",0dh,0ah,0
	line byte         "				       _______________________",0ah,0dh,0
	line1 byte    "                                         -----------------------",0ah,0
	line2 byte   "                ______________________________________",0dh,0ah,0
	newline byte "--------------------------------------------------------------------------",0dh,0ah,0
	
	Group_Members byte "					*** GROUP MEMBERS ***",0dh,0ah
				  byte " ",0dh,0ah
				  byte "				      1.Hamza Jafri (20k-1669) ",0dh,0ah
				  byte "				      2.Umer Vohra  (20k-1677) ",0dh,0ah
				  byte "				      3.Bilal Mamji (20k-1702) ",0dh,0ah,0

	OPTIONS byte "Select the type of conversion you want to perform from the following 4 ",0dh,0ah
				   byte " ",0dh,0ah
				   byte "1. Convert Binary to Hexadecimal and Decimal ",0dh,0ah
				   byte "2. Convert Decimal to Hexadecimal and Binary ",0dh,0ah
				   byte "3. Convert Hexadecimal to Binary and Decimal ",0dh,0ah
				   byte "4. Convert Octal to Hexadecimal and Decimal ",0dh,0ah
				   byte "5. Exit ",0dh,0ah,0

	SELECT_OPTION byte "Please enter the conversion you want to perform: ",0
	OPTION_TEMP dword ?
	
	THANK_YOU_MESSAGE byte "		| THANK YOU FOR USING OUR CALCULATOR |",0dh,0ah,0
	
	ERROR_OPTIONS byte "               | Please select from the given options |",0dh,0ah,0
	
	DECIMAL_NUMBER_CONVERTED BYTE "	 --> DECIMAL Number : ",0
	HEXADECIMAL_NUMBER_CONVERTED BYTE "	 --> HEXADEC Number : ",0
	BINARY_NUMBER_CONVERTED BYTE "	--> BINARY Number : ",0
	
	;-------------------------------*****------------------------------
	;---------------------------BINARY DATA----------------------------
	BINARY_INPUT byte "Enter the BINARY number you want to convert: ",0
	
	;Binaryinputs proto, BINARY_LENGTH:DWORD, BASE:DWORD, DECIMAL_NUMBER:DWORD, COUNT:DWORD ///////// PROTO

	ERROR_NOT_BINARY_NUMBER BYTE "              | Invalid Binary can only contain 0 and 1 |",0ah,0dh,0
	BINARY_NUMBER_ARRAY BYTE 33 DUP(?) 
	BINARY_LENGTH DWORD 0
	BASE DWORD 2    
	DECIMAL_NUMBER DWORD ?
	COUNT DWORD 0
	
	;-------------------------------*****------------------------------
	;---------------------------DECIMAL DATA----------------------------
	DECIMAL_INPUT byte "Enter the DECIMAL number you want to convert: ",0
	
	DECIMAL_TEMP dword ?
	
	;-------------------------------*****------------------------------
	;---------------------------HEXADECIMAL DATA----------------------------
	HEXADECIMAL_INPUT byte "Enter the HEXADECIMAL number you want to convert: ",0
	
	HEXADECIMAL_TEMP dword ?
	
	;-------------------------------*****------------------------------
	;---------------------------OCTAL DATA----------------------------
	OCTAL_INPUT byte "Enter the OCTAL number you want to convert: ",0  
	
	ERROR_NOT_OCTAL_NUMBER BYTE "              | Invalid OCTAL Number can contain 0 - 7 |",0ah,0dh,0
	OCTAL_NUMBER BYTE 33 DUP(?)  
	OCTAL_LENGTH DWORD 0
	BASE_OCTAL DWORD 8           
	DECIMAL_NUMBER_OCTAL DWORD ?
	COUNT_OCTAL DWORD 0

.code
main PROC
;invoke binaryinputs, 33 DUP(?),0,2,0,0 //////////// INVOKE

	INTRO:
	mov eax,0
	call Box
    call SetTextColor
	mov ecx,4
    FR:
    call crlf
    LOOP FR
	mov edx,offset main_heading
	call writestring
	mov edx,offset line
	call writestring
	mov ecx,4
    ER:
    call crlf
    LOOP ER
	mov edx,offset Group_Members
	call writestring
	mov ecx,8
    CR:
    call crlf
    LOOP CR
    call waitmsg
	

	MAIN_LABEL_FOR_CONVERTER:
		call clrscr
		mov edx,offset Heading
		call writestring
		mov edx,offset line1
		call writestring
		;mov edx,offset newline
		;call writestring
		call crlf
		mov edx,offset OPTIONS
		call writestring
		call crlf
		mov edx,offset SELECT_OPTION
		call writestring
		call readdec
		call crlf
		mov OPTION_TEMP,eax

		COMPARE_1_LABEL:
			mov eax,OPTION_TEMP
			cmp eax,1
			JE OPTION_1_LABEL
			JL ERROR_LABEL

		COMPARE_2_LABEL:
			mov eax,OPTION_TEMP
			cmp eax,2
			JE OPTION_2_LABEL

		COMPARE_3_LABEL:
			mov eax,OPTION_TEMP
			cmp eax,3
			JE OPTION_3_LABEL
		
		COMPARE_4_LABEL:
			mov eax,OPTION_TEMP
			cmp eax,4
			JE OPTION_4_LABEL
		
		COMPARE_5_LABEL:
			mov eax,OPTION_TEMP
			cmp eax,5
			JE QUIT_LABEL_OPTION_5
			JGE ERROR_LABEL

		OPTION_1_LABEL:
			mov eax,0
			mov edx, OFFSET BINARY_INPUT
			call WriteString
			mov edx,OFFSET BINARY_NUMBER_ARRAY
			mov ecx,SIZEOF BINARY_NUMBER_ARRAY 
			call ReadString
			mov BINARY_LENGTH,eax  
			mov ecx, BINARY_LENGTH    
			mov eax,0
			mov esi,0                                  
			call BINARY_TO_DECIMAL_CONVERT_PROCEDURE
			jmp MAIN_LABEL_FOR_CONVERTER
		
		OPTION_2_LABEL:
			mov edx,offset DECIMAL_INPUT
			call writestring
			call readdec
			mov DECIMAL_TEMP,eax
			call DISPLAY_DECIMAL_TO_BINARY_AND_HEXADECIMAL
			jmp MAIN_LABEL_FOR_CONVERTER
		
		OPTION_3_LABEL:
			mov edx,offset HEXADECIMAL_INPUT
			call writestring
			call readhex
			mov HEXADECIMAL_TEMP,eax
			call DISPLAY_HEXADECIMAL_TO_BINARY_AND_DECIMAL
			jmp MAIN_LABEL_FOR_CONVERTER
		
		OPTION_4_LABEL:
			mov edx, OFFSET OCTAL_INPUT
			call WriteString
			mov edx,OFFSET OCTAL_NUMBER
			mov ecx,SIZEOF OCTAL_NUMBER 
			call ReadString             
			mov OCTAL_LENGTH,eax  
			mov eax,0
			mov esi,0                             
			mov ecx, OCTAL_LENGTH       
			call OCTAL_TO_DECIMAL_CONVERT_PROCEDURE
			jmp MAIN_LABEL_FOR_CONVERTER
		
		ERROR_LABEL:
			mov edx,offset line2
			call writestring
			call crlf
			mov edx,offset ERROR_OPTIONS
			call writestring
			mov edx,offset line2
			call writestring
			mov ecx,11
			BR:
			call crlf
			LOOP BR
			call waitmsg
			jmp MAIN_LABEL_FOR_CONVERTER

QUIT_LABEL_OPTION_5:
call crlf
call crlf
mov edx,offset line2
call writestring
call crlf
mov edx,offset THANK_YOU_MESSAGE
call writestring
mov edx,offset line2
call writestring
mov ecx,7
DR:
call crlf
LOOP DR
EXIT
main ENDP

;BINARY_TO_DECIMAL_CONVERT_PROCEDURE PROC, BINARY BINARY_LENGTH:DWORD, BASE:DWORD, DECIMAL_NUMBER:DWORD, COUNT:DWORD ///////// PROC

BINARY_TO_DECIMAL_CONVERT_PROCEDURE PROC
	;LOCAL BASE:DWORD          ////////// Local Directive
	;mov BASE,2

	OUTER_CONVERSION_LABEL:
		cmp ecx,0										      
		je DISPLAY_BINARY_TO_DECIMAL_AND_HEXADECIMAL         
		mov COUNT,ecx         

		CONDITION_1:				
			cmp BINARY_NUMBER_ARRAY[esi],'0'          
			je INCRMENT_LABEL                  

		CONDITION_2:
			cmp BINARY_NUMBER_ARRAY[esi],'1'         
			jne NOT_BINARY_ERROR                      

			mov ecx, BINARY_LENGTH     
			sub ecx,esi   
			dec ecx		  

			mov eax,1      
			;.while(ecx >= 0)

			top:
				cmp ecx,0
				jge L1
			
				jmp L2

				L1:
					cmp ecx,0   
					je stop     
										
					mov ebx,BASE			
					mul ebx					
					dec ecx	
					jmp top
			;.endw

			L2:

			stop:
				add DECIMAL_NUMBER,eax
				jmp INCRMENT_LABEL

		NOT_BINARY_ERROR:
			call crlf
			mov edx,offset line2
			call writestring
			call crlf
			mov edx, OFFSET ERROR_NOT_BINARY_NUMBER  
			call WriteString
			mov edx,offset line2
			call writestring
			mov ecx,9
			AR:
			call crlf
			LOOP AR
			call waitmsg
			ret
			;exit

	INCRMENT_LABEL:
		inc esi 
		mov ecx,COUNT  
		dec ecx  
		jmp OUTER_CONVERSION_LABEL

	call DISPLAY_BINARY_TO_DECIMAL_AND_HEXADECIMAL
	ret
BINARY_TO_DECIMAL_CONVERT_PROCEDURE ENDP

DISPLAY_BINARY_TO_DECIMAL_AND_HEXADECIMAL PROC 
	call Box2
	call crlf
	call crlf
	mov edx, OFFSET DECIMAL_NUMBER_CONVERTED
	call WriteString
	mov eax, DECIMAL_NUMBER
	call WriteDec              
	call Crlf
	call crlf
	mov edx,offset HEXADECIMAL_NUMBER_CONVERTED
	call WriteString
	mov eax, DECIMAL_NUMBER
	call writehex  
	mov DECIMAL_NUMBER,0
	mov ecx,8
	CR:
	call crlf
	LOOP CR
	call waitmsg
	ret
DISPLAY_BINARY_TO_DECIMAL_AND_HEXADECIMAL ENDP

DISPLAY_DECIMAL_TO_BINARY_AND_HEXADECIMAL PROC
	call Box2
	call crlf
	call crlf
	mov eax,DECIMAL_TEMP
	mov edx,offset BINARY_NUMBER_CONVERTED
	call writestring
	call writebin
	call crlf
	call crlf
	mov eax,DECIMAL_TEMP
	mov edx,offset HEXADECIMAL_NUMBER_CONVERTED
	call writestring
	call writehex
	mov ecx,8
	CR:
	call crlf
	LOOP CR
	call waitmsg
	ret
DISPLAY_DECIMAL_TO_BINARY_AND_HEXADECIMAL ENDP


DISPLAY_HEXADECIMAL_TO_BINARY_AND_DECIMAL PROC
	call Box2
	call crlf
	call crlf
	mov eax,HEXADECIMAL_TEMP
	mov edx,offset BINARY_NUMBER_CONVERTED
	call writestring
	call writebin
	call crlf
	call crlf
	mov eax,HEXADECIMAL_TEMP
	mov edx,offset DECIMAL_NUMBER_CONVERTED
	call writestring
	call writedec
	mov ecx,8
	CR:
	call crlf
	LOOP CR
	call waitmsg
	ret
DISPLAY_HEXADECIMAL_TO_BINARY_AND_DECIMAL ENDP


OCTAL_TO_DECIMAL_CONVERT_PROCEDURE PROC
	OUTER_CONVERSION_LABEL:
		cmp ecx,0                      
		je DISPLAY_OCTALL_TO_HEXADECIMAL_AND_DECIMAL         
		mov COUNT_OCTAL,ecx         

		CONDITION_1:
			cmp OCTAL_NUMBER[esi],'0'  
			je INCRMENT_LABEL                  

		CONDITION_2:
			cmp OCTAL_NUMBER[esi],'1'         
			jge CONDITION_3                      
			mov ecx, OCTAL_LENGTH
			sub ecx,esi
			dec ecx
			mov eax,0       
			jmp INNER_CONVERSION_LABEL

		CONDITION_3:
			cmp OCTAL_NUMBER[esi],'2'         
			jge CONDITION_4                      
			mov ecx, OCTAL_LENGTH
			sub ecx,esi
			dec ecx
			mov eax,1       
			jmp INNER_CONVERSION_LABEL
		
		CONDITION_4:
			cmp OCTAL_NUMBER[esi],'3'         
			jge CONDITION_5                      
			mov ecx, OCTAL_LENGTH
			sub ecx,esi
			dec ecx
			mov eax,2       
			jmp INNER_CONVERSION_LABEL
		
		CONDITION_5:
			cmp OCTAL_NUMBER[esi],'4'         
			jge CONDITION_6                      
			mov ecx, OCTAL_LENGTH
			sub ecx,esi
			dec ecx
			mov eax,3       
			jmp INNER_CONVERSION_LABEL
		
		CONDITION_6:
			cmp OCTAL_NUMBER[esi],'5'         
			jge CONDITION_7                      
			mov ecx, OCTAL_LENGTH
			sub ecx,esi
			dec ecx
			mov eax,4       
			jmp INNER_CONVERSION_LABEL
		
		CONDITION_7:
			cmp OCTAL_NUMBER[esi],'6'         
			jge CONDITION_8                      
			mov ecx, OCTAL_LENGTH
			sub ecx,esi
			dec ecx
			mov eax,5       
			jmp INNER_CONVERSION_LABEL
		
		CONDITION_8:
			cmp OCTAL_NUMBER[esi],'7'         
			jge CONDITION_9                      
			mov ecx, OCTAL_LENGTH
			sub ecx,esi
			dec ecx
			mov eax,6       
			jmp INNER_CONVERSION_LABEL
		
		CONDITION_9:
			cmp OCTAL_NUMBER[esi],'8'         
			jge NOT_OCTAL_ERROR                      
			mov ecx, OCTAL_LENGTH
			sub ecx,esi
			dec ecx
			mov eax,7       
			jmp INNER_CONVERSION_LABEL
			

		INNER_CONVERSION_LABEL:
			cmp ecx,0   
			je stop     
			mov ebx,BASE_OCTAL
			mul ebx
			dec ecx
			jmp INNER_CONVERSION_LABEL

		stop:
			add DECIMAL_NUMBER_OCTAL,eax
			jmp INCRMENT_LABEL

		NOT_OCTAL_ERROR:
			call crlf
			mov edx,offset line2
			call writestring
			call crlf
			mov edx, OFFSET ERROR_NOT_OCTAL_NUMBER  
			call WriteString
			mov edx,offset line2
			call writestring
			mov ecx,9
			CR:
			call crlf
			LOOP CR
			call waitmsg    
			ret
			;exit         

	INCRMENT_LABEL:
		inc esi
		mov ecx,COUNT_OCTAL 
		dec ecx
		jmp OUTER_CONVERSION_LABEL

	call DISPLAY_OCTALL_TO_HEXADECIMAL_AND_DECIMAL
	ret
OCTAL_TO_DECIMAL_CONVERT_PROCEDURE ENDP

DISPLAY_OCTALL_TO_HEXADECIMAL_AND_DECIMAL PROC
	call Box2
	call crlf
	mov edx, OFFSET DECIMAL_NUMBER_CONVERTED
	call WriteString
	mov eax, DECIMAL_NUMBER_OCTAL
	call WriteDec              
	call Crlf
	call crlf
	mov edx, OFFSET HEXADECIMAL_NUMBER_CONVERTED
	call WriteString
	mov eax, DECIMAL_NUMBER_OCTAL
	call WriteHex
	call crlf
	call crlf
	mov edx, OFFSET BINARY_NUMBER_CONVERTED
	call WriteString
	mov eax, DECIMAL_NUMBER_OCTAL
	call WriteBin
	mov DECIMAL_NUMBER_OCTAL,0
	mov ecx,6
	CR:
	call crlf
	LOOP CR
	call waitmsg
	ret
DISPLAY_OCTALL_TO_HEXADECIMAL_AND_DECIMAL ENDP

Box PROC
    mov ecx,80
    mov dl,10               ; dl = x-axis column
                            ; dh = y-axis row
    mov dh,4                 ;row
    LOOPS:
    call gotoxy
    mov al,61             ;Ascii
    call writechar
    mov eax , 3
    call delay
    inc dl
    LOOP LOOPS

    mov ecx,20
    dec dl
    inc dh
    LOOPSS:
    call gotoxy
    mov al,124
    call writechar
    mov eax , 3
    call delay
    inc dh
    LOOP LOOPSS

    mov ecx,80
    LOOPPSS:
    call gotoxy
    mov al,61
    call writechar
    mov eax , 3
    call delay
    dec dl
    LOOP LOOPPSS


    mov ecx,20
    inc dl
    dec dh
    LOOPPSSS:
    call gotoxy
    mov al,124
    call writechar
    mov eax , 3
    call delay
    dec dh
    LOOP LOOPPSSS
    ret
Box endp

Box2 PROC
    mov ecx,65
    mov dl, 5             ; dl = x-axis column
                            ; dh = y-axis row
    mov dh,15                 ;row
    LOOPS:
    call gotoxy
    mov al,61             ;Ascii
    call writechar
    mov eax , 3
    call delay
    inc dl
    LOOP LOOPS

    mov ecx,8
    dec dl
    inc dh
    LOOPSS:
    call gotoxy
    mov al,124
    call writechar
    mov eax , 3
    call delay
    inc dh
    LOOP LOOPSS

    mov ecx,65
    LOOPPSS:
    call gotoxy
    mov al,61
    call writechar
    mov eax , 3
    call delay
    dec dl
    LOOP LOOPPSS


    mov ecx,8
    inc dl
    dec dh
    LOOPPSSS:
    call gotoxy
    mov al,124
    call writechar
    mov eax , 3
    call delay
    dec dh
    LOOP LOOPPSSS
    ret
Box2 endp

END main