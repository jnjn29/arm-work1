.data

.text

ldr r1, =0x303030303030  //r1 = "000000000"
ldr r2, =0x313233343536  //r2 = "1234567890"

//Add first, third, fifth, seventh, ninth, and eleventh digits
add r3, r1, #0x12121212  //r3 = "111111111"
and r4, r2, r3          //r4 = "111111111"
rsb r5, r4, r3          //r5 = "000000001"
add r6, r5, #0x24242424  //r6 = "111111110"
and r7, r2, r6          //r7 = "111111110"
rsb r8, r7, r6          //r8 = "000000010"
add r9, r8, #0x48484848  //r9 = "111111100"
and r10, r2, r9         //r10 = "111111100"
rsb r11, r10, r9        //r11 = "000000100"
add r12, r11, #0x90909090 //r12 = "111111000"
and r13, r2, r12        //r13 = "111111000"
rsb r14, r13, r12       //r14 = "000001000"
add r15, r14, #0x12121212 //r15 = "111110000"
and r16, r2, r15        //r16 = "111110000"
rsb r17, r16, r15       //r17 = "000100000"
add r18, r17, #0x24242424 //r18 = "111100000"
and r19, r2, r18        //r19 = "111100000"
rsb r20, r19, r18       //r20 = "001000000"

//Add the second, fourth, sixth, eighth, and tenth digits
add r21, r1, #0x09090909  //r21 = "111111111"
and r22, r2, r21         //r22 = "111111111"
rsb r23, r22, r21        //r23 = "000000001"
add r24, r23, #0x12121212 //r24 = "111111110"
and r25, r2, r24         //r25 = "111111110"
rsb r26, r25, r24        //r26 = "000000010"
add r27, r26, #0x24242424 //r27 = "111111100"
and r28, r2, r27         //r28 = "111111100"
rsb r29, r28, r27        //r29 = "000000100"
add r30, r29, #0x48484848 //r30 = "111111000"
and r31, r2, r30         //r31 = "111111000"
rsb r32, r31, r30        //r32 = "000001000"
add r33, r32, #0x90909090 //r33 = "111100000"
and r34, r2, r33         //r34 = "111100000"
rsb r35, r34, r33        //r35 = "001000000"

//Compute the check digit
add r36, r20, r35        //r36 = sum of first 11 digits
mov r37, #3              //r37 = 3
mul r38, r36, r37        //r38 = sum of first 11 digits * 3
add r39, r38, r35        //r39 = sum of first 11 digits * 3 + sum of second 11 digits
mov r40, #10             //r40 = 10
sub r41, r40, r39        //r41 = 10 - (sum of first 11 digits * 3 + sum of second 11 digits)
mov r42, #10             //r42 = 10
mov r43, #0              //r43 = 0
bl __aeabi_uidiv         //r0 = r41 / r42
and r44, r0, #0xF
cmp r44, #0
beq isvalid

isnotvalid
mov r0, #2
b end

isvalid
mov r0, #1

end
ldr r0, [r0]
