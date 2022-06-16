# Shell

## 認識 SHELL
* 所有的電腦是由硬體和軟體架構而成的，而負責主要運算的部分是作業系統的核心(kernel)，kernel 須接受來自鍵盤的輸入，交由 CPU 進行處理，最後將執行結果輸出到螢幕上
* 說明：輸入 pwd 命令，我們知道是 print working directory 的意思，但 kernel 並不知道，這時候，shell 就會將 pwd 翻譯為 kernel 能理解的程式碼，所以在使用電腦的時候，基本上是和 shell 打交道，而不是直接和 kernel 溝通
* Linux 的 kernel 只有一個，但 kernel 之外的 shell 卻有許多種，例如 bourne Shell、C Shell、Korn Shell、Zsh Shell等等，但最常接觸到有 BASH (Bourne Again SHell)，為 GNU 所加強的一個 Bourne shell 版本，也是大多數 Linux 套件的預設 shell
<br>

## Shell Script
* 設定變數：變數會在同一個 process 中持續存在，直到此 process 結束
  > var = value
* 取用變數
  > $var
* 呼叫：可輸出指定資訊
  * 變數的值可含有 space 的字串，帶有一個以上 space 的變數，給定值的時候，必須以成對的雙引號(")或單引號(')涵蓋
  * 雙引號(")中若含有變數(如$var)，會先將變數轉換成其實際的值，單引號(')則會將變數當成是一個值，而不會作轉換動作
  > arthur = "我的小名" <br>
  > echo $arthur <br>

  > echo "This is a Borne Shell Script" <br>

  > echo "arthur就是 $arthur" <br>
  > 結果：arthur 就是 我的小名 <br>
  > echo 'arthur就是 $arthur' <br>
  > 結果：arthur 就是 $arthur
* 宣告：路徑變數、環境變數
  > declare -x path="/proc_data/MAC" <br>
  > echo ${path} 
* 函數
  * if...else...
    > if[ && ]; then <br>
    >   echo "" <br>
    > elif[ && ]; then <br>
    >   echo "" <br>
    > else <br>
    >   echo "" <br>
    >   exit 0 
    > fi <br>
  * 確認檔案是否存在
    > -f /proc_data/MAC_20220601.xls <br>
  * 數學函數
    > -eq：等於 <br>
    > -lt：小於 <br>
    > -le：小於或等於 <br>
    > -gt：大於 <br>
    > -ge：大於或等於
<br>

## 參考資訊
* [Shell 和 Shell Script](https://www.cyut.edu.tw/~ywfan/1109linux/201109chapter11shell%20script.htm)
<br>
