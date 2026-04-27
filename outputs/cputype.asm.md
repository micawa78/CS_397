
;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
;*                                                                           *
;* CPU type identification routines - implemented as C-callable function.    *
;*                                                                           *
;* History:                                                                  *
;*                                                                           *
;*     11Aug24  MC Walker    Initial Creation                                *
;*                                                                           *
;* Contained in the library:  highmem.lib                                    *
;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

_TEXT segment  byte public 'CODE'

    assume CS:_TEXT

    public _cpu_type

;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
;* cpu_type()                                                                *
;*                                                                           *
;* This function returns the CPU type.                                       *
;*                                                                           *
;* Args:                                                                     *
;*     None.                                                                 *
;*                                                                           *
;* Returns:                                                                  *
;*                                                                           *
;*     A word, containing the CPU type:                                      *
;*                                                                           *
;*          86  - 8086, 8088, 80186, 80188, NEC V20, NEC V30                 *
;*         286  - 80286                                                      *
;*         386  - 80386                                                      *
;*         486  - 80486                                                      *
;*                                                                           *
;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

_cpu_type proc far

    pushf                                 ;save old flags

    mov dx, 86                            ;test for an 8086 chip

    push sp                               ;save initial SP value
    pop ax                                ;move SP into AX
    cmp sp,ax                             ;If SP decrements before a value is
    jne ret_type                          ;pushed, then it's a real-mode chip.


    mov dx, 286                           ;test for 286

    pushf                                 ;save flags
    pop ax                                ;place flags into AX
    or ax, 4000h                          ;Try to set bit 14 [Nested Task bit]
    push ax                               ;push it back on the stack
    popf                                  ;then pop it back into the flags

    pushf                                 ;now retrieve it
    pop ax                                ;into AX
    test ax, 4000h                        ;if Nested Task bit can't be set
    jz ret_type                           ;while in real mode, then it's a 286

    mov dx, 386                           ;test for 386/486

    .386                                  ;assemble 32-bit instructions

    mov ebx, esp                          ;save ESP in EBX

    and esp, 0FFFFFFFCh                   ;clear lowest 2 bits of ESP
                                          ;to avoid an AC fault on a 486

    pushfd                                ;push Eflags register
    pop eax                               ;pop Eflags into EAX
    mov ecx, eax                          ;copy Eflags into ECX

    xor eax, 40000h                       ;try to change bit 18 [AC] in Eflags

    push eax                              ;push it back on the stack
    popfd                                 ;then pop it back into Eflags

    pushfd                                ;now retrieve it
    pop eax                               ;into EAX
    and eax, 40000h                       ;isolate bit 18 in EAX
    and ecx, 40000h                       ;isolate bit 18 in ECX
    cmp eax, ecx                          ;are they the same?
    je found_386                          ;if so, it's a 386
    mov dx, 486                           ;otherwise, it's a 486

found_386:

    push ecx                              ;push the original Eflags
    popfd                                 ;now restore the Eflags
    mov esp, ebx                          ;restore ESP from EBX

    .8086                                 ;assemble 16-bit instructions

ret_type:        

    mov ax, dx                            ;put cpu type in AX
    popf                                  ;restore original flags
    ret                                   ;return to caller

_cpu_type endp

_TEXT ends

    end

;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
;

