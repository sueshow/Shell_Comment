# Shell

## 認識 SHELL
* 所有的電腦是由硬體和軟體架構而成的，而負責主要運算的部分是作業系統的核心(kernel)，kernel 須接受來自鍵盤的輸入，交由 CPU 進行處理，最後將執行結果輸出到螢幕上
* 說明：輸入 pwd 命令，我們知道是 print working directory 的意思，但 kernel 並不知道，這時候，shell 就會將 pwd 翻譯為 kernel 能理解的程式碼，所以在使用電腦的時候，基本上是和 shell 打交道，而不是直接和 kernel 溝通
* Linux 的 kernel 只有一個，但 kernel 之外的 shell 卻有許多種，例如 bourne Shell、C Shell、Korn Shell、Zsh Shell等等，但最常接觸到有 BASH (Bourne Again SHell)，為 GNU 所加強的一個 Bourne shell 版本，也是大多數 Linux 套件的預設 shell
<br>

## Shell Script
* 設定變數方式：變數會在同一個 process 中持續存在，直到此 process 結束
  > var = value
* 取用變數的方法
  > $var
* 呼叫
  > arthur = "我的小名"
  > echo $arthur
* 
<br>

## 參考資訊
* [Shell 和 Shell Script](https://www.cyut.edu.tw/~ywfan/1109linux/201109chapter11shell%20script.htm)
<br>
