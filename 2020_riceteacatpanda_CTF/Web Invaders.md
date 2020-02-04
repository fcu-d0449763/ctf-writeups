# Web Invaders

Assign: 飛飛 林
Status: Web

一個 iframe [https://jef1056.github.io/](https://jef1056.github.io/)

![Web%20Invaders/Untitled.png](Web%20Invaders/Untitled.png)

用 F12 看網頁

![Web%20Invaders/Untitled%201.png](Web%20Invaders/Untitled%201.png)

看參數 `Module`很奇怪

![Web%20Invaders/Untitled%202.png](Web%20Invaders/Untitled%202.png)

`_filesToPreload` 這個參數怪怪的 每一個都印出來看看，查了一下 Unit8Array to text 

`var string2 = new TextDecoder("utf-8").decode(Module._filesToPreload[2].data);`

![Web%20Invaders/Untitled%203.png](Web%20Invaders/Untitled%203.png)

找看看有沒有 FLAG 字串 在 js 裡面字串用 .search 去找會回傳位置

`string2.search('rtcp{')
>9706`

![Web%20Invaders/Untitled%204.png](Web%20Invaders/Untitled%204.png)

切字串到 9706 附近，前後字串看看切到 9740，js 用 substring

`string2.substring(9706,9740)`

![Web%20Invaders/Untitled%205.png](Web%20Invaders/Untitled%205.png)

`rtcp{web_h^ck3r_0004212}`