
;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
;*                                                                           *
;* Mutex lock routines - implemented as C-callable function.                 *
;*                                                                           *
;* History:                                                                  *
;*                                                                           *
;*     12Jul25  MC Walker    Initial Creation                                *
;*                                                                           *
;* Contained in the library:  proc.lib                                       *
;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

_TEXT segment  byte public 'CODE'

    assume CS:_TEXT

    public _set_lock
    public _release_lock

;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
;* set_lock()                                                                *
;*                                                                           *
;* This function blocks until a mutual exclusion lock is obtained.           *
;*                                                                           *
;* Args:                                                                     *
;*     None.                                                                 *
;*                                                                           *
;* Returns:                                                                  *
;*                                                                           *
;*     None. Blocking function.                                              *
;*                                                                           *
;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

lock_var:                             ;1 is locked, 0 unlocked
     dd      0

_set_lock proc far

set_start:

    mov eax, 1                        ;load a one
    xchg eax, [lock_var]              ;swap one with the lock var
                             
    test eax, eax                     ;test orig var with itself.
                                      ;if var was zero, the lock has been obtained

     jnz  set_start                   ;repeat if lock not obtained.

     ret                              ;lock obtained, return to caller

_set_lock endp




;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
;* release_lock()                                                            *
;*                                                                           *
;* This function releases a mutual exclusion lock.                           *
;*                                                                           *
;* Args:                                                                     *
;*     None.                                                                 *
;*                                                                           *
;* Returns:                                                                  *
;*                                                                           *
;*     None.                                                                 *
;*                                                                           *
;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

_release_lock proc far

    xor eax, eax                     ;zero out register
    xchg eax, [lock_var]             ;swap zero with the lock var

    ret                              ;lock released, return to caller

_release_lock endp

_TEXT ends

    end

;* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
;