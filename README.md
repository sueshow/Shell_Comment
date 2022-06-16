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
    > if[ condition && condition ]; then <br>
    >   echo "XXX" <br>
    > elif[ condition && condition ]; then <br>
    >   echo "XXX" <br>
    > else <br>
    >   echo "XXX" <br>
    >   exit 0 <br>
    > fi 
  * while：迴圈中 commands 會一直被重複執行，直到 condition 的值為偽為止
    > while condition
    > do
    > commands
    > done
  * 數學函數：test n1 -op n2 or [n1 -op n2]
    > -eq，=：等於 <br>
    > -ne，!=：不等於 <br>
    > -lt，\<：小於 <br>
    > -le，\<=：小於或等於 <br>
    > -gt，\>：大於 <br>
    > -ge，\>=：大於或等於
  * 四則運算：
    * 四則運算必須使用 expr 這個指令來輔助
    * 如果要將結果指定給變數，必須用 '`' 包起來
    * 在 + - * / %(求餘數) 的二邊都有空白，如果沒有空白將產生錯誤
      > expr 5 -2 <br>
      > 結果：3 <br>
      > sum=`expr 5 + 10` <br>
      > echo $sum <br>
      > 結果：3 <br>
      > count=`expr 5 \* 3` <br>
      > echo $count <br>
      > 結果：15 <br>
      > echo `expr $count % 3` <br>
      > 結果：0 
    * 可以使用 $(()) 將運算式放在括號中，即可達到運算的功能
      > a=10 <br>
      > b=5 <br>
      > c=$((${a}+${b})) <br>
      > echo $c <br>
      > 結果：15 <br>
      > c=$((${a}*${b})) <br>
      > echo $c <br>
      > 結果：50 
  * 檔名：test -options file_name or [-option file_name]
    > -r：file exists and readable <br>
    > -w：file exists and writeable <br>
    > -x：file exists and executable <br>
   
    > -f：file exists and is a regular file <br>
      > ex：-f /proc_data/MAC_20220601.xls <br>

    > -d：file exists and is a directory <br>
    > -u：file exists and is setuid <br>
    > -s：file exists and is greater than zero in size 
  * 字串：test -option string or [-option string] 
    > -z：string length is zero <br>
    > -n：string length is non-zero <br>
    > string1 = string2：string1 is identical to string2 <br>
    > string1 != string2：string1 is not identical to string2 
<br>

## 程式會自動定義的變數
<table border="1" width="30%">
    <tr>
        <th width="5%">變數名稱</a>
        <th width="25%">說明</a>
    </tr>
    <tr>
        <td> $? </td>
        <td> 表示上一個指令的離開狀況，一般指令正常離開會傳回 0。不正常離開則會傳回 1、2 等數值 </td>
    </tr>
    <tr>
        <td> $$ </td>
        <td> 這一個 shell 的 process ID number </td>
    </tr>
    <tr>
        <td> $! </td>
        <td> 最後一個在背景執行的程式的 process number </td>
    </tr>
    <tr>
        <td> $- </td>
        <td> 這個參數包含了傳遞給 shell 旗標 (flag) </td>
    </tr>
    <tr>
        <td> $1 </td>
        <td> 代表第一個參數，$2 則為第二個參數，依此類推。而 $0 為這個 shell script 的檔名 </td>
    </tr>
    <tr>
        <td> $# </td>
        <td> 執行時，給這個 Shell Script 參數的個數 </td>
    </tr>
    <tr>
        <td> $* </td>
        <td> 包含所有輸入的參數，$@ 即代表 $1, $2,....直到所有參數結束。$* 將所有參數無間隔的連在一起，存成一個單一的參數。也就是說 $* 代表了 "$1 $2 $3..." </td>
    </tr>
    <tr>
        <td> $@ </td>
        <td> 包含所有輸入的參數，$@ 即代表 $1, $2,....直到所有參數結束。$@ 用將所有參數以空白為間隔，存在 $@ 中。也就是說 $@ 代表了 "$1" "$2" "$3".... </td>
    </tr>
</table> 
<br>

## 參考資訊
* [Shell 和 Shell Script](https://www.cyut.edu.tw/~ywfan/1109linux/201109chapter11shell%20script.htm)
<br>
