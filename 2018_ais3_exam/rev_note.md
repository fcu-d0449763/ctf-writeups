AIS3逆向筆記
=====
# AIS3逆向筆記
##### Exercise
###### Variable
- 0
```c 
// 0.c
int main(){
    return 0;
}
```
```
// 0.bin
080483ed: <main>
 80483ed: push ebp     ; push os call's ebp
 80483ee: mov ebp, esp ;  main's stack frame.
 80483f0: mov eax, 0x0 ; return value
 80483f5: pop ebp
 80483f6: ret
```

- 1.int


```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x4
 80483f3:	mov    DWORD PTR [ebp-0x4],0xa ; 4-byte value on stack, (int)10
 80483fa:	mov    eax,0x0
 80483ff:	leave  
 8048400:	ret
```

- 2 char
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x4
 80483f3:	mov    BYTE PTR [ebp-0x1],0x41 //a 1-byte value on stack , (char)'A'
 80483f7:	mov    eax,0x0
 80483fc:	leave  
 80483fd:	ret 
```

- 3 Short integer
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x4
 80483f3:	mov    WORD PTR [ebp-0x2],0xa ; 2-byte value on stack, (short)10
 80483f9:	mov    eax,0x0
 80483fe:	leave  
 80483ff:	ret
```

- 4 Pointer
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    DWORD PTR [ebp-0x8],0xa  ; int a = 10
 80483fa:	lea    eax,[ebp-0x8]            ; store address into eax
 80483fd:	mov    DWORD PTR [ebp-0x4],eax  ; int *b = &a
 8048400:	mov    eax,0x0
 8048405:	leave  
 8048406:	ret 
```

- 5 Global Variable / Static Variable
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x4
 80483f3:	mov    DWORD PTR [ebp-0x4],0x0
 80483fa:	mov    eax,ds:0x804a01c         ; data segments global variable
 80483ff:	mov    DWORD PTR [ebp-0x4],eax
 8048402:	mov    eax,0x0
 8048407:	leave  
 8048408:	ret 
```

###### Control Flow Statements
- 6
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x4
 80483f3:	mov    DWORD PTR [ebp-0x4],0xa  ; int A = 10
 80483fa:	cmp    DWORD PTR [ebp-0x4],0x63 ; cmpare A with 0x63
 80483fe:	jg     8048407 <main+0x1a>      ; JG(JNLE) jump to 8048407 when A bigger than 0x63
 8048400:	mov    eax,0x0
 8048405:	jmp    804840c <main+0x1f> ; jump to 804840c
 8048407:	mov    eax,0x0
 804840c:	leave  
 804840d:	ret
```
```c
int main(){
  int a = 10;
  if(a > 0x63) { return 0; }
  return 0;
}
```


- 7 Nested if statement
```
080483ed <main>:
80483ed:       push   ebp
80483ee:       mov    ebp,esp
80483f0:       sub    esp,0x4
80483f3:       mov    DWORD PTR [ebp-0x4],0xa  ; int A = 0xa
80483fa:       cmp    DWORD PTR [ebp-0x4],0x63 ; comapre A with 0x63
80483fe:       jg     804840d <main+0x20>      ; jump to 804840d when A bigger than 0x63
8048400:       cmp    DWORD PTR [ebp-0x4],0x32 ; compare A with 0x32
8048404:       jle    804840d <main+0x20>      ; jump to 804840d when smaller or equal than 0x32
8048406:       mov    eax,0x0
804840b:       jmp    8048412 <main+0x25>      ; jump to 8048412
804840d:       mov    eax,0x0
8048412:       leave  
8048413:       ret
```

- 8 if else statement
```
080483ed <main>:
80483ed:       push   ebp
80483ee:       mov    ebp,esp
80483f0:       sub    esp,0x8
80483f3:       mov    DWORD PTR [ebp-0x4],0xa  ; int A = 0xa
80483fa:       cmp    DWORD PTR [ebp-0x4],0x63 ; compare A with 0x63
80483fe:       jg     8048409 <main+0x1c>      ; jump to 8048409 when A bigger than 0x63
8048400:       mov    DWORD PTR [ebp-0x8],0x1  ; int B = 0x1
8048407:       jmp    8048410 <main+0x23>      ; jump to 8048410
8048409:       mov    DWORD PTR [ebp-0x8],0x0  ; int B = 0
8048410:       mov    eax,0x0
8048415:       leave  
8048416:       ret  
```
```c
int main() {
    int A = 0xa, B;
    if(A > 0x63) {
        B = 0;
    } else {
        B = 1;
    }
    return 0;
}
```

- 9 if-(else if)-else / switch case statment
```
080483ed <main>:
 80483ed:       push   ebp
 80483ee:       mov    ebp,esp
 80483f0:       sub    esp,0x8
 80483f3:       mov    DWORD PTR [ebp-0x4],0x1 ; int A = 1
 80483fa:       mov    eax,DWORD PTR [ebp-0x4] ; eax = A
 80483fd:       cmp    eax,0x2                 ; compare A with 0x2
 8048400:       je     8048412 <main+0x25>     ; jump to 8048412 when A == 2
 8048402:       cmp    eax,0x3                 ; compare A with 0x3
 8048405:       je     8048418 <main+0x2b>     ; jump to 8048418 when A == 3
 8048407:       cmp    eax,0x1                 ; compare A with 0x1
 804840a:       jne    804841e <main+0x31>     ; jump to 804841e if A != 1
 804840c:       mov    BYTE PTR [ebp-0x5],0x41 ; char B = 0x41
 8048410:       jmp    8048423 <main+0x36>     ; jump to 8048423
 8048412:       mov    BYTE PTR [ebp-0x5],0x42 ; char B = 0x42
 8048416:       jmp    8048423 <main+0x36>     ; jump to 8048423
 8048418:       mov    BYTE PTR [ebp-0x5],0x43 ; char B = 0x43
 804841c:       jmp    8048423 <main+0x36>     ; jump to 8048423
 804841e:       mov    BYTE PTR [ebp-0x5],0x44 ; char B = 0x44
 8048422:       nop
 8048423:       mov    eax,0x0
 8048428:       leave  
 8048429:       ret 
```
```c
// 共筆社群版
int main(){
  int A = 0;
  char B;
  if(A == 2){
    B = 'B';
  } else if(A == 3) {
    B = 'C';
  } else if(A != 1){
    B = 'D';
  } else {
    B = 'A';
  }
  return 0;
}
```
> 阿對啦 把不等於的分支翻過來就好了
```c
// Asuka 官方正解
int main(){
  int A = 0;
  char B;
  switch(A){
    case 1:
      B = 'A';
      break;
    case 2:
      B = 'B';
      break;
    case 3:
      B = 'C';
      break;
    default:
      B = 'D';
  }
  return 0;
}
```


- 10 for statement
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    DWORD PTR [ebp-0x8],0x0 ; int A = 0
 80483fa:	mov    DWORD PTR [ebp-0x4],0x0 ; int B = 0
 8048401:	jmp    804840d <main+0x20>
 8048403:	mov    eax,DWORD PTR [ebp-0x4] ; eax = B
 8048406:	add    DWORD PTR [ebp-0x8],eax ; A += eax
 8048409:	add    DWORD PTR [ebp-0x4],0x1 ; B += 1
 804840d:	cmp    DWORD PTR [ebp-0x4],0x9 ; comapre B and 0x9
 8048411:	jle    8048403 <main+0x16>     ; jump to 8048403 if B is less or equal than 0x9
 8048413:	mov    eax,0x0
 8048418:	leave  
 8048419:	ret 
```
```c
int main(){
    int a = 0;
    for(int b = 0;b <= 9 /*>*/; b++ ){
        a += b;
    }
    return 0;
}
```
> hackmd 的 highlight 怪怪的 ...
> 正常發揮(x
> 聽說還有10~15
> 我們需要更多的人肉decompiler QQQQQQ
> 總之先打好吧qwq
> 再找時間compiler xd

- 11 while statement
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    DWORD PTR [ebp-0x8],0x0
 80483fa:	mov    DWORD PTR [ebp-0x4],0x0
 8048401:	jmp    804840d <main+0x20>
 8048403:	mov    eax,DWORD PTR [ebp-0x4]
 8048406:	add    DWORD PTR [ebp-0x8],eax
 8048409:	add    DWORD PTR [ebp-0x4],0x1
 804840d:	cmp    DWORD PTR [ebp-0x4],0x9
 8048411:	jle    8048403 <main+0x16>
 8048413:	mov    eax,0x0
 8048418:	leave  
 8048419:	ret 
```
Exactly as for statement 跟for一樣的assembly code
```c=

```

- 12 do while statement
```
080483ed <main>:
 80483ed:       push   ebp
 80483ee:       mov    ebp,esp
 80483f0:       sub    esp,0x4
 80483f3:       mov    DWORD PTR [ebp-0x4],0x0  ; int A = 0
 80483fa:       add    DWORD PTR [ebp-0x4],0x1  ; A += 1
 80483fe:       cmp    DWORD PTR [ebp-0x4],0x63 ; comapre A with 0x63
 8048402:       jle    80483fa <main+0xd>       ; A smaller then 100
 8048404:       mov    eax,0x0
 8048409:       leave  
 804840a:       ret 
```
```c=
int main(){
    int n = 0;
    do {
        n++;
    } while(n<100) /*>*/
    return 0;
}
```


- 13 Nested for statement
```
080483ed <main>:
 80483ed:       push   ebp
 80483ee:       mov    ebp,esp
 80483f0:       sub    esp,0xc
 80483f3:       mov    DWORD PTR [ebp-0xc],0x0 ; c
 80483fa:       mov    DWORD PTR [ebp-0x4],0x0 ; a
 8048401:       jmp    8048424 <main+0x37>
 8048403:       mov    DWORD PTR [ebp-0x8],0x0 ; b
 804840a:       jmp    8048410 <main+0x23>
 804840c:       add    DWORD PTR [ebp-0x8],0x1 ; b++
 8048410:       cmp    DWORD PTR [ebp-0x8],0x9 ; cmp b with 9
 8048414:       jle    804840c <main+0x1f>
 8048416:       mov    eax,DWORD PTR [ebp-0x4] ; eax = a
 8048419:       imul   eax,DWORD PTR [ebp-0x8] ; eax *= b
 804841d:       mov    DWORD PTR [ebp-0xc],eax ; c = eax
 8048420:       add    DWORD PTR [ebp-0x4],0x1 ; a++
 8048424:       cmp    DWORD PTR [ebp-0x4],0x9 ; cmp a with 9
 8048428:       jle    8048403 <main+0x16>
 804842a:       mov    eax,0x0
 804842f:       leave  
 8048430:       ret 
```
for(){for(){}}
```c
int main(){
  for(int i = 0; i <= 9; i++ ){
    for(int j = 0; j <= 9; j++){
      c = i*j;
    }
  }
  return 0;
}
```
> 99 乘法 !?


- 14 Goto statement
```
080483ed <main>:
 80483ed:       push   ebp
 80483ee:       mov    ebp,esp
 80483f0:       sub    esp,0x4
 80483f3:       mov    DWORD PTR [ebp-0x4],0xa ; int A = 0xa
 80483fa:       cmp    DWORD PTR [ebp-0x4],0x1 ; compare A with 0x1
 80483fe:       jne    8048402 <main+0x15>     ; jump to 8048402 if A != 0x1
 8048400:       jmp    8048417 <main+0x2a>     ; goto 8048417
 8048402:       cmp    DWORD PTR [ebp-0x4],0x2 ; compare A with 0x2
 8048406:       jne    8048417 <main+0x2a>     ; jump to 8048417 if A != 0x2
 8048408:       nop
 8048409:       mov    DWORD PTR [ebp-0x4],0x0 ; A = 0
 8048410:       mov    eax,0x0
 8048415:       jmp    804841c <main+0x2f>     ; return 0
 8048417:       mov    eax,0x1                 ; return 1
 804841c:       leave  
 804841d:       ret 
```

```c
int main(){
  int a = 10;
  if(a == 1){
    goto label;
  } else if(a == 2){
    a = 0;
    return 0
  }
  label:
    return 1;
}
```


- 15 Continue statement in While Loop
```
080483ed <main>:
80483ed:       push   ebp
80483ee:       mov    ebp,esp
80483f0:       sub    esp,0x4
80483f3:       mov    DWORD PTR [ebp-0x4],0x0  ; int A = 0
80483fa:       jmp    8048408 <main+0x1b>
80483fc:       cmp    DWORD PTR [ebp-0x4],0x64 ; comapre A with 0x64
8048400:       jle    8048404 <main+0x17>      ; jump to 8048404 if A lower or equal 0x64
8048402:       jmp    8048408 <main+0x1b>      ; jump to 8048408
8048404:       add    DWORD PTR [ebp-0x4],0x1
8048408:       cmp    DWORD PTR [ebp-0x4],0x63
804840c:       jle    80483fc <main+0xf>
804840e:       mov    eax,0x0
8048413:       leave  
8048414:       ret   
```

###### Arithmetic operation
- 16 加法
```
080483ed <main>:
 80483ed:push   ebp
 80483ee:mov    ebp,esp
 80483f0:sub    esp,0xc
 80483f3:mov    DWORD PTR [ebp-0x4],0x5 ; int A = 5
 80483fa:mov    DWORD PTR [ebp-0x8],0x5 ; int B = 5
 8048401:mov    DWORD PTR [ebp-0xc],0x0 ; int C = 0
 8048408:mov    eax,DWORD PTR [ebp-0x8] ; eax = B
 804840b:mov    edx,DWORD PTR [ebp-0x4] ; eax = A
 804840e:add    eax,edx                 ; eax = A + B
 8048410:mov    DWORD PTR [ebp-0xc],eax ; C = eax (A + B)
 8048413:mov    eax,0x0
 8048418:leave  
 8048419:ret 
```

```clike=
int main() {
    int A = 5;
    int B = 5;
    int C = 0;
    C = A + B;
    
    return 0;
}
```
- 17 減法
```
080483ed <main>:
  80483ed:push   ebp
  80483ee:mov    ebp,esp
  80483f0:sub    esp,0xc
  80483f3:mov    DWORD PTR [ebp-0x4],0x5 ; int A = 5
  80483fa:mov    DWORD PTR [ebp-0x8],0x5 ; int B = 5
  8048401:mov    DWORD PTR [ebp-0xc],0x0 ; int C = 0
  8048408:mov    eax,DWORD PTR [ebp-0x8] ; eax = B
  804840b:mov    edx,DWORD PTR [ebp-0x4] ; edx = A
  804840e:sub    edx,eax                 ; edx -= eax
  8048410:mov    eax,edx                 ; eax = edx
  8048412:mov    DWORD PTR [ebp-0xc],eax ; C = eax
  8048415:mov    eax,0x0
  804841a:leave  
  804841b:ret 
```
```clike=
int main (){
    int A = 5;
    int B = 5;
    int C = 0;
    C = A - B;
    
    return 0;
}
```

- 18 乘法
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0xc
 80483f3:	mov    DWORD PTR [ebp-0x4],0x5 ; int A = 5
 80483fa:	mov    DWORD PTR [ebp-0x8],0x5 ; int B = 5
 8048401:	mov    DWORD PTR [ebp-0xc],0x0 ; int C = 0
 8048408:	mov    eax,DWORD PTR [ebp-0x4] ; eax = A
 804840b:	imul   eax,DWORD PTR [ebp-0x8] ; eax * 5
 804840f:	mov    DWORD PTR [ebp-0xc],eax ; C = eax
 8048412:	mov    eax,0x0
 8048417:	leave  
 8048418:	ret 
```
```clike=
int main (){
    int A = 5;
    int B = 5;
    int C = 0;
    C = A * B;
    
    return 0;
}
```

- 19 除法
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0xc
 80483f3:	mov    DWORD PTR [ebp-0x4],0x5 ; int a= 5
 80483fa:	mov    DWORD PTR [ebp-0x8],0x5 ; int b = 5
 8048401:	mov    DWORD PTR [ebp-0xc],0x0 ; int c = 0
 8048408:	mov    eax,DWORD PTR [ebp-0x4] ; eax = a
 804840b:	cdq    
 804840c:	idiv   DWORD PTR [ebp-0x8]     ; edx = eax % b; eax /= b;
 804840f:	mov    DWORD PTR [ebp-0xc],eax ; c = eax
 8048412:	mov    eax,0x0
 8048417:	leave  
 8048418:	ret 
```
```c
int main() {
    int a = 5;
    int b = 5;
    int c = 0;
    c = a / b;
    return 0;
}
```

- 20 取餘
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0xc
 80483f3:	mov    DWORD PTR [ebp-0x4],0x5
 80483fa:	mov    DWORD PTR [ebp-0x8],0x5
 8048401:	mov    DWORD PTR [ebp-0xc],0x0
 8048408:	mov    eax,DWORD PTR [ebp-0x4]
 804840b:	cdq    
 804840c:	idiv   DWORD PTR [ebp-0x8]
 804840f:	mov    DWORD PTR [ebp-0xc],edx
 8048412:	mov    eax,0x0
 8048417:	leave  
 8048418:	ret 
```
- idiv 後
    - eax 存 商
    - edx 存 餘
```c
int main(){
  int a=5,b=5,c=0;
  c = a % b;
  return 0;
}
```

##### Bitwise operation
- 21 and
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    DWORD PTR [ebp-0x4],0x4 ; int A = 4
 80483fa:	mov    DWORD PTR [ebp-0x8],0x0 ; int B = 0
 8048401:	mov    eax,DWORD PTR [ebp-0x4] ; eax = A
 8048404:	and    eax,0x1                 ; eax = eax & 0x1
 8048407:	mov    DWORD PTR [ebp-0x8],eax ; B = eax
 804840a:	mov    eax,0x0
 804840f:	leave  
 8048410:	ret 
```
```c
int main(){
  int a=4, b=0;
  b = a & 1;
  return 0;
}
```

- 22 or
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    DWORD PTR [ebp-0x4],0x4 ; int A = 4
 80483fa:	mov    DWORD PTR [ebp-0x8],0x0 ; int B = 0
 8048401:	mov    eax,DWORD PTR [ebp-0x4] ; eax = A
 8048404:	or     eax,0x1                 ; eax = eax | 0x1
 8048407:	mov    DWORD PTR [ebp-0x8],eax ; B = eax
 804840a:	mov    eax,0x0
 804840f:	leave  
 8048410:	ret
```
```c
int main(){
  int a=4, b=0;
  b = a | 1;
  return 0;
}
```
- 23 xor 
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    DWORD PTR [ebp-0x4],0x4 ; int A = 4
 80483fa:	mov    DWORD PTR [ebp-0x8],0x0 ; int B = 0
 8048401:	mov    eax,DWORD PTR [ebp-0x4] ; eax = A
 8048404:	xor    al,0xff                 ; al = al ^ 0xff
 8048406:	mov    DWORD PTR [ebp-0x8],eax ; B = eax
 8048409:	mov    eax,0x0
 804840e:	leave  
 804840f:	ret 
```
```c
int main(){
  int a=4, b=0;
  b = (char)a ^ 0xff;
  return 0;
}
```
> 求解為什麼是 (char)
> al 只有最低的 8-bit
> 不過他放是把EAX整個放回去B耶
> 對啊
> 所以還4intㄅ QQQQ？
> 所以我只有強轉 a
> 雖然這大概是 gcc 幹得好事吧我猜?

- 24 not
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    DWORD PTR [ebp-0x4],0x4 ; int A = 4
 80483fa:	mov    DWORD PTR [ebp-0x8],0x0 ; int B = 0
 8048401:	mov    eax,DWORD PTR [ebp-0x4] ; eax = A
 8048404:	not    eax                     ; eax = !eax
 8048406:	mov    DWORD PTR [ebp-0x8],eax ; B = eax
 8048409:	mov    eax,0x0
 804840e:	leave  
 804840f:	ret 
```
```c
int main(){
  int a=4, b=0;
  b = ~a;
  return 0;
}
```

- 25 shift right 算術右移
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x4
 80483f3:	mov    DWORD PTR [ebp-0x4],0xa
 80483fa:	sar    DWORD PTR [ebp-0x4],1
 80483fd:	mov    eax,0x0
 8048402:	leave  
 8048403:	ret 
```
```c
int main(){
  int a = 10;
  a >>= 1;
}
```

- 26 shift left 邏輯左移
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x4
 80483f3:	mov    DWORD PTR [ebp-0x4],0xa
 80483fa:	shl    DWORD PTR [ebp-0x4],1
 80483fd:	mov    eax,0x0
 8048402:	leave  
 8048403:	ret 
```
```c
int main(){
  int a = 10;
  a <<= 1; /* >> */
}
```

###### Function Call
- 27
```
0x80484e0 = "Year = %d"

0804841d <main>:
804841d:push   ebp
804841e:mov    ebp,esp
8048420:sub    esp,0xc
8048423:mov    DWORD PTR [ebp-0x4],0x7e0
804842a:mov    eax,DWORD PTR [ebp-0x4]
804842d:mov    DWORD PTR [esp+0x4],eax
8048431:mov    DWORD PTR [esp],0x80484e0
8048438:call   80482f0 <printf@plt>
804843d:mov    eax,0x0
8048442:leave  
8048443:ret    
```

```clike=
int  main()
{
  int year = 2016;
  printf("Year = %d", year);
  return 0;
}
```

- 28
```
080483ed <myfunc>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	mov    eax,DWORD PTR [ebp+0xc]
 80483f3:	mov    edx,DWORD PTR [ebp+0x8]
 80483f6:	add    eax,edx
 80483f8:	pop    ebp
 80483f9:	ret    
080483fa <main>:
 80483fa:	push   ebp
 80483fb:	mov    ebp,esp
 80483fd:	sub    esp,0xc
 8048400:	mov    DWORD PTR [esp+0x4],0x5
 8048408:	mov    DWORD PTR [esp],0x5
 804840f:	call   80483ed <myfunc>
 8048414:	mov    DWORD PTR [ebp-0x4],eax
 8048417:	mov    eax,0x0
 804841c:	leave  
 804841d:	ret 
```
```c=
int myfunc( int a, int b){
  return b + a; // ooops 感謝修正
}
int main(){
  int a;
  a = myfunc(5,5);
  return 0;
}

```

- 29
```
080483ed <myfunc>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	mov    eax,DWORD PTR [ebp+0xc]
 80483f3:	mov    edx,DWORD PTR [ebp+0x8]
 80483f6:	add    eax,edx
 80483f8:	pop    ebp
 80483f9:	ret    
080483fa <main>:
 80483fa:	push   ebp
 80483fb:	mov    ebp,esp
 80483fd:	sub    esp,0x10
 8048400:	mov    DWORD PTR [ebp-0x4],0x80483ed
 8048407:	mov    DWORD PTR [esp+0x4],0x5
 804840f:	mov    DWORD PTR [esp],0x5
 8048416:	mov    eax,DWORD PTR [ebp-0x4]
 8048419:	call   eax
 804841b:	mov    DWORD PTR [ebp-0x8],eax
 804841e:	mov    eax,0x0
 8048423:	leave  
 8048424:	ret 
```

```clike=
int myfunc(int a, int b) {
    return a + b;
}

int main(){
    int a = 0x80483ed, b;
    b = *(((int*)(int,int))a)(5,5); /*可以不要這樣嗎qwq 系統打亂記憶體會爆炸嗎w*/ /* 請去找 Asuka 姐接裡論 */
    /* 怕 */ /*不要編 PIE 應該都還好*/

    return 0;
}
```
```clike=

int myfunc(int a, int b) {
    return a + b;
}

int main()
{
    int (*f)(int, int) = myfunc;
    int sum = f(5, 5);
}

```

###### Type Conversion
- 30
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    WORD PTR [ebp-0x2],0x1
 80483f9:	movsx  eax,WORD PTR [ebp-0x2]
 80483fd:	mov    DWORD PTR [ebp-0x8],eax
 8048400:	mov    eax,0x0
 8048405:	leave  
 8048406:	ret 
```
```clike=
int main(){
    short a = 1;
    int b = (int)a;
    return 0;
}
```





- 31
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x8
 80483f3:	mov    DWORD PTR [ebp-0x4],0xa
 80483fa:	mov    eax,DWORD PTR [ebp-0x4]
 80483fd:	mov    WORD PTR [ebp-0x6],ax  /*有人看得到這裡嗎ＱＱ為啥-0x6 short 是2*/ /* WORD PTR 解出來的就是 2-byte 的記憶體 */
 8048401:	mov    eax,0x0
 8048406:	leave  
 8048407:	ret 
```

```c
int main(){
  int a = 10;
  short b = (short)a;
}
```

- 32 浮點數存取
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	and    esp,0xfffffff8
 80483f3:	sub    esp,0x8
 80483f6:	fld    QWORD PTR ds:0x80484a0
 80483fc:	fstp   QWORD PTR [esp]
 80483ff:	mov    eax,0x0
 8048404:	leave  
 8048405:	ret 
```
```c=
// 0x80484a0 → 0x4024333333333333
int main(){
  double a= 10.1; // 應該是double吧？ // 喔對耶 沒注意到是 64bit

}
// 忘了浮點數塞不進指令裡
```

> 心已累 (效能意義上)
> 電腦CPU嗎ww 大量hackmd  wwww
> 快 lag 到不能打了 QQ
> F12把上面刪掉會怎樣Ww
> 快試有
> 蒸發掉不要怪我
> 有版本控制LA
> 世界毀滅
> 他十分鐘會備份一次
> 我們10分鐘可以產很多字
> 我們484放棄
> 腦子真的不行了
> 加油各位
> F5重開就比較順嘞
> 腦子不能F5....
> 重整加只開編輯版面勉強順一點點...

- 33 array
```
080483ed <main>:
 80483ed:	push   ebp
 80483ee:	mov    ebp,esp
 80483f0:	sub    esp,0x2c
 80483f3:	mov    DWORD PTR [ebp-0x4],0x0        ; int i = 0
 80483fa:	jmp    804840a <main+0x1d>
 80483fc:	mov    eax,DWORD PTR [ebp-0x4]
 80483ff:	mov    edx,DWORD PTR [ebp-0x4]
 8048402:	mov    DWORD PTR [ebp+eax*4-0x2c],edx ; arr[i] = i
 8048406:	add    DWORD PTR [ebp-0x4],0x1        ; i++
 804840a:	cmp    DWORD PTR [ebp-0x4],0x9        ; i <= /*> 9
 804840e:	jle    80483fc <main+0xf>
 8048410:	mov    eax,0x0
 8048415:	leave  
 8048416:	ret 
```

```c
int main(){
  int a[10];
  for(int i = 0; i < 10; i++){
    a[i] = i;
  }
  return 0;
}
```

- 34 2d array
```
080483ed <main>:
80483ed:push   ebp
80483ee:mov    ebp,esp
80483f0:sub    esp,0x198
80483f6:mov    DWORD PTR [ebp-0x4],0x0
80483fd:jmp    8048432 <main+0x45>
80483ff:mov    DWORD PTR [ebp-0x8],0x0
8048406:jmp    8048428 <main+0x3b>
8048408:mov    edx,DWORD PTR [ebp-0x4]
804840b:mov    eax,edx
804840d:shl    eax,0x2
8048410:add    eax,edx
8048412:add    eax,eax
8048414:mov    edx,DWORD PTR [ebp-0x8]
8048417:add    eax,edx
8048419:mov    DWORD PTR [ebp+eax*4-0x198],0xa
8048424:add    DWORD PTR [ebp-0x8],0x1
8048428:cmp    DWORD PTR [ebp-0x8],0x9
804842c:jle    8048408 <main+0x1b>
804842e:add    DWORD PTR [ebp-0x4],0x1
8048432:cmp    DWORD PTR [ebp-0x4],0x9
8048436:jle    80483ff <main+0x12>
8048438:mov    eax,0x0
804843d:leave  
804843e:ret   
```

```clike=
int main(){
    int array[10][10]; 
    int i ,j;
    for (i=0;i<10;++i){
        for(j=0;j<10;++j){
            array[i][j] =10;//[j+10*i]
        }

    }
    return 0 ;
}

```

- 35 多重指標
```
080483ed <main>:
80483ed:push   ebp
80483ee:mov    ebp,esp
80483f0:sub    esp,0xc
80483f3:mov    DWORD PTR [ebp-0x8],0xa
80483fa:lea    eax,[ebp-0x8]
80483fd:mov    DWORD PTR [ebp-0xc],eax
8048400:lea    eax,[ebp-0xc]
8048403:mov    DWORD PTR [ebp-0x4],eax
8048406:mov    eax,0x0
804840b:leave  
804840c:ret   
```

```clike=
int main(){
  int a = 10;
  int *b = &a;
  int **c = &b;
  return 0;
}
```

- 36 Structure( same as local variable)
```
0804841d <myfunc>:
 804841d:       push   ebp
 804841e:       mov    ebp,esp
 8048420:       sub    esp,0x8
 8048423:       mov    eax,DWORD PTR [ebp+0xc]    ; eax = argument 2
 8048426:       mov    DWORD PTR [esp+0x4],eax
 804842a:       mov    DWORD PTR [esp],0x8048500
 8048431:       call   80482f0 <printf@plt>
 8048436:       leave  
 8048437:       ret

08048438 <main>:
8048438:push   ebp
8048439:mov    ebp,esp
804843b:sub    esp,0x10
804843e:mov    DWORD PTR [ebp-0x8],0x7e0
8048445:mov    DWORD PTR [ebp-0x4],0x1
804844c:mov    eax,DWORD PTR [ebp-0x8]
804844f:mov    edx,DWORD PTR [ebp-0x4]
8048452:mov    DWORD PTR [esp],eax
8048455:mov    DWORD PTR [esp+0x4],edx
8048459:call   804841d <myfunc>
804845e:mov    eax,0x0
8048463:leave  
8048464:ret  
```

```clike=
struct student{
 int year;
 int id;
}

void my func(struct student n){
    print("id=%d\n",n.id);
}

int main{
    struct student data;
    data.year =2016;
    data.id =1;
    myfunc(data);
    return 0;
}
```


- 37 struct with bitfield
```
080483ed <main>:
80483ed:push   ebp
80483ee:mov    ebp,esp
80483f0:sub    esp,0x4
80483f3:movzx  eax,BYTE PTR [ebp-0x4] ; eax = (char)a
80483f7:and    eax,0xfffffffe         ; eax &= -2
80483fa:mov    BYTE PTR [ebp-0x4],al  ; *((char*)(&a)) = eax
80483fd:movzx  eax,BYTE PTR [ebp-0x4]
8048401:or     eax,0x2                ; eax |= 2
8048404:mov    BYTE PTR [ebp-0x4],al  ; *((char*)(&a)) = eax
8048407:mov    eax,0x0
804840c:leave  
804840d:ret 
```

```clike=
typedef union {
  unsigned int up:1;
  unsigned int down:1;
} eh;

int main(){
  eh a;
  a.up = 1
  a.down =0;
}

```
> 媽的是 bitfield 喔 !!??
> 所以怎麼看R
> 為什麼看得出來
> 0xe = 0b11111110
> 跟這個 and 會把低 bit 設為 0
> 0x2 = 0b00000010
> 跟這個 or 會把低位數起第二 bit 設為 1
> 至於為啥那麼大是因為用的是 unsigned int

- 38 union
```
080483ed <main>:
80483ed:push   ebp
80483ee:mov    ebp,esp
80483f0:sub    esp,0x4
80483f3:mov    DWORD PTR [ebp-0x4],0x7e0
80483fa:mov    DWORD PTR [ebp-0x4],0x1
8048401:mov    eax,0x0
8048406:leave  
8048407:ret  
```

```clike=
typedef union {
  int val;
  int var2;
} useless;

int main(){
    useless a;
    a.val = 0x7e0;
    a.var2 = 1;
    return 0;
}
```


- 39 enum
```
080483ed <main>:
 80483ed:       push   ebp
 80483ee:       mov    ebp,esp
 80483f0:       sub    esp,0xc
 80483f3:       mov    DWORD PTR [ebp-0xc],0x0
 80483fa:       mov    DWORD PTR [ebp-0x8],0x1
 8048401:       mov    DWORD PTR [ebp-0x4],0x2
 8048408:       mov    eax,0x0
 804840d:       leave  
 804840e:       ret  
```
```clike=
enum Alpha {
  A,
  B,
  C
};

int main() {
    int a =A;
    int b =B;
    int c =C;
    return 0;
}
```
> 吐嘈： int a = 0, b = 1, c = 2; ????????????????????????????????????
> 好像對(O)
> 沒錯啊 XD
> C語言重新學enum

###### Signed or unsigned
> 組合語言沒有 signed / unsigned 差別吧
> 頂多 if  出來的不一樣
> Slide這樣寫XDDDD... 就只好這樣抄囉 ㄎㄎㄎ
> 交給其他大神

- 40 signed
```
080483ed <main>:
 80483ed:       push   ebp
 80483ee:       mov    ebp,esp
 80483f0:       sub    esp,0x4
 80483f3:       mov    DWORD PTR [ebp-0x4],0xffffffff
 80483fa:       mov    eax,0x0
 80483ff:       leave  
 8048400:       ret   
```

```clike=
int main(){
    int A = -1;  // 對齁 ㄇㄉ補數
    // signed int A = -1;
    // unsigned int A = 4294967295; // 完全硬要 可以.... //XDDD
    return 0;
}
```


- 41
```
080483ed <main>:
 80483ed:       push   ebp
 80483ee:       mov    ebp,esp
 80483f0:       sub    esp,0x8
 80483f3:       mov    WORD PTR [ebp-0x8],0x1
 80483f9:       movzx  eax,WORD PTR [ebp-0x8]
 80483fd:       mov    DWORD PTR [ebp-0x4],eax
 8048400:       movzx  eax,WORD PTR [ebp-0x8]
 8048404:       mov    edx,0x0
 8048409:       div    WORD PTR [ebp-0x8]
 804840d:       mov    WORD PTR [ebp-0x6],ax
 8048411:       cmp    WORD PTR [ebp-0x8],0x64
 8048416:       jbe    804841f <main+0x32>  ; below or equal
 8048418:       mov    eax,0x0
 804841d:       jmp    8048424 <main+0x37>
 804841f:       mov    eax,0x0
 8048424:       leave  
 8048425:       ret    
```
```clike
int main(){
  unsigned short int n1 =1; // 0x8
  unsigned short int result; // 0x6
  //unsigned int a; // 0x4
  //unsigned short int b, c; // 0x8 0x6
  /*
  b = 1;
  a = b;
  c = b/b;
  */
  
  int n2; // 0x4
  result = n1/n1;
  if (n1 >100){
      return 0;
  }
  return 0;
  /*
  if(b <= 0x64){
    return 0;
  } else {
    return 0;
  }
  */
}

```


- 42
```
080483ed <main>:
 80483ed:       push   ebp
 80483ee:       mov    ebp,esp
 80483f0:       sub    esp,0x8
 80483f3:       mov    WORD PTR [ebp-0x8],0x1
 80483f9:       movsx  eax,WORD PTR [ebp-0x8]
 80483fd:       mov    DWORD PTR [ebp-0x4],eax
 8048400:       movsx  eax,WORD PTR [ebp-0x8]
 8048404:       movsx  ecx,WORD PTR [ebp-0x8]
 8048408:       cdq    
 8048409:       idiv   ecx
 804840b:       mov    WORD PTR [ebp-0x6],ax
 804840f:       cmp    WORD PTR [ebp-0x8],0x64
 8048414:       jle    804841d <main+0x30>
 8048416:       mov    eax,0x0
 804841b:       jmp    8048422 <main+0x35>
 804841d:       mov    eax,0x0
 8048422:       leave  
 8048423:       ret 
```

```c
int main(){

}

```

jle /jbe -> signed or unsigned

###### String operation

- 43
    - gcc -O0 -mpreferred-stack-boundary=2 -fno-stack-protector
```
0804841d <main>:
 804841d:       push   ebp
 804841e:       mov    ebp,esp
 8048420:       sub    esp,0x10
 8048423:       mov    DWORD PTR [ebp-0x6],0x63696c41
 804842a:       mov    WORD PTR [ebp-0x2],0x65
 8048430:       lea    eax,[ebp-0x6]
 8048433:       mov    DWORD PTR [esp+0x4],eax
 8048437:       mov    DWORD PTR [esp],0x80484e0 ; "name=%s\n"
 804843e:       call   80482f0 <printf@plt>
 8048443:       mov    eax,0x0
 8048448:       leave  
 8048449:       ret
```

```clike
int main(){

    char x[] = "Alice";
    printf("name = %s \n", x);
    return 0;
}
```

- 44 String compare
    - gcc -O3 -mpreferred-stack-boundry=2 -fno-stack-protector -D_FORTIFY_SOURCE=0

```
08048320 <main>:
 8048320:       push   edi
 8048321:       mov    eax,0x65 ; 'e'
 8048326:       push   esi
 8048327:       mov    edi,0x8048500 ; "Alice"
 804832c:       sub    esp,0xc
 804832f:       mov    ecx,0x6
 8048334:       mov    DWORD PTR [esp+0x6],0x63696c41; 'Alic'
 804833c:       lea    esi,[esp+0x6]
 8048340:       mov    WORD PTR [esp+0xa],ax
 8048345:       repz cmps BYTE PTR ds:[esi],BYTE PTR es:[edi]
 8048347:       jne    8048355 <main+0x35>
 8048349:       mov    DWORD PTR [esp],0x8048506 ; "Hey Alice!\n"
 8048350:       call   80482f0 <puts@plt>
 8048355:       add    esp,0xc
 8048358:       xor    eax,eax
 804835a:       pop    esi
 804835b:       pop    edi
 804835c:       ret  
```

```clike
int strcmp_func(char[] str){ //inlined function
  if(strcmp(str, "Alice") == 0){
    return 1;
  }
  return 0;
}

int main(){
  char s[] = "Alice";

  if(strcmp_func(s)){
    puts("Hey Alice!\n");
  }

}
```

