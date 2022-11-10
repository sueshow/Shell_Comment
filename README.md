# Shell

## 認識 SHELL
* 所有的電腦是由硬體和軟體架構而成的，而負責主要運算的部分是作業系統的核心(kernel)，kernel 須接受來自鍵盤的輸入，交由 CPU 進行處理，最後將執行結果輸出到螢幕上
* 說明：輸入 pwd 命令，我們知道是 print working directory 的意思，但 kernel 並不知道，這時候，shell 就會將 pwd 翻譯為 kernel 能理解的程式碼，所以在使用電腦的時候，基本上是和 shell 打交道，而不是直接和 kernel 溝通
* Linux 的 kernel 只有一個，但 kernel 之外的 shell 卻有許多種，例如 bourne Shell、C Shell、Korn Shell、Zsh Shell等等，但最常接觸到有 BASH (Bourne Again SHell)，為 GNU 所加強的一個 Bourne shell 版本，也是大多數 Linux 套件的預設 shell
<br>


## Shell Script
* 要點
  * 在 [ ] 當中，只能有一個判別式
  * 在 [ ] 與 [ ] 當中，可以使用 && 或 || 來組織判別式
  * 每一個獨立的元件之間『都需要有空白鍵來隔開』
  * 定義變數，指定變數的等號(=)前後不空格
* 設定變數：變數會在同一個 process 中持續存在，直到此 process 結束
  > var = value <br>
  
  > var_h=`date +%H` <br>
  > 結果：取執行時間的小時 <br>
  > var_m=`date +%M` <br>
  > 結果：取執行時間的分鐘
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
    > 結果：arthur 就是 $arthur <br>
  
    > 會將「arthur 就是 我的小名」放入檔案：XXXXXX.txt <br>
    > echo "arthur就是 $arthur" >> XXXXXX.txt <br>
    
    * In Software：將參數往後面的程序帶
      > echo "@@JCS_STEP_VAR var_h $\var_h"
      > echo "@@JCS_GLOBAL_VAR %varname %varvalue"
* 宣告：路徑變數、環境變數
  > 設定名稱變數 <br>
  > declare -x path="/proc_data/MAC" <br>
  > echo ${path} <br>
  
  > 設定日期變數 <br>
  > declare -i txdt="20220801" 
* 目錄
  * 指定路徑
    > cd "/proc_data/MAC"
  * 建立目錄
    > mkdir "XXXXX"
  * 刪除目錄
    > rm -r "/proc_data/MAC"
  * 設定權限
    > 可讀可寫可執行
    > chmod 777 "/proc_data/MAC"   
* 解壓縮/壓縮檔案
  * 壓縮
    * 說明：7-Zip 預設會壓縮該目錄與所有子目錄的所有檔案
    * 壓縮 dir1\ 資料夾下所有檔案，且壓縮檔案中會看到 dir1 這個資料夾
      > 7z a "dir1.zip" "dir1\" 
    * 壓縮 dir1\ 資料夾下所有檔案，壓縮檔案不會看到 dir1 這個資料夾，只會看到裡面的檔案與子資料夾
      > 7z a "dir1.zip" "dir1\*"
    * 壓縮整個資料夾，並且保留現有檔案的完整路徑
      > CD /D G:\ <br>
      > 7z a "dir2.zip" "sub1\dir1\dir2"
    * 壓縮限定特定類型的檔案，可先加上 -r 參數，再加不同的檔名樣式 (File Patterns) 就可找出檔案並加入壓縮檔
      > 7z a "dir1.zip" "dir1\" -r "dir1\*.aspx" <br>
      > 7z a "dir1.zip" "dir1\" -r "dir1\*.aspx" "dir1\*.dll"
    * 排除特定類型的檔案不要進壓縮檔
      > 7z a "dir1.zip" "dir1\" "-xr!*.pdb" "-xr!web.config"
    * 將目前資料夾下的所有檔案壓縮到上一層目錄：不加上任何參數，預設就是把當前目錄全部都壓縮起來，但請記得壓縮檔不要放在當前目錄下
      > 7z a "..\dir1.zip"
  * 解壓縮
    * 解壓縮到當前目錄：解出來的東西會跟壓縮檔放在一起
      > 7z x "dir1.zip"
    * 解壓縮到指定輸出目錄：在 -o 與 Path 中間不能有任何空白字元
      > 7z x "dir1.zip" -o"dir1" <br>
      > 7z x "dir1.zip" -o"C:\Program Files\"
    * 只解壓縮特定檔案類型到指定輸出目錄
      > 7z x "dir1.zip" -o"dir1" -r "*.dll"
    * 擷取特定檔案類型到指定輸出目錄
      * 取出所有 *.js 檔案到指定輸出目錄
        > 7z e "dir1.zip" -o"dir1" -r "*.js"
      * 若出現檔名衝突問你要不要覆蓋的提示。如果不想提示直接覆蓋，可以加上 -y 參數
        > 7z e "dir1.zip" -o"dir1" -r "*.dll" -y
  * 其他進階用法
    * 解壓縮/壓縮檔案時包含解壓縮密碼
      * 設定一個密碼(-p)
        > 7z a "dir1.zip" "dir1/" -p"1q2w3e4r"
      * 設定一個密碼(-p)，並且將壓縮檔的 Header 資訊一併加密，也就是連檔名都一起加密，開啟壓縮檔的時候就要先輸入密碼才知道內容
        > 7z a "dir1.7z" "dir1/" -p"1q2w3e4r"  -mhe
      * 解壓縮一個加密過的壓縮檔到指定輸出目錄
        > 7z x "dir1.7z" -o"dir1/" -p"1q2w3e4r"
    * 列出所有檔案清單
      > 7z l "dir1.zip"
    * 顯示壓縮檔中完整的技術資訊
      > 7z l "dir1.zip" -slt
* 彙整
  <table border="1" width="30%">
    <tr>
        <th width="2%">命令</a>
        <th width="2%">代表意義</a>
        <th width="6%">命令說明</a>
        <th width="10%">指令</a>
    </tr>
    <tr>
        <td> a </td>
        <td> Add </td>
        <td> 將檔名列中的檔案加入壓縮檔 </td>
        <td> -i (Include)         <br>
             -m (Method)          <br>
             -p (Set Password)    <br>
             -r (Recurse)         <br>
             -sfx (create SFX)    <br>
             -si (use StdIn)      <br>
             -so (use StdOut)     <br>
             -t (Type of archive) <br>
             -u (Update)          <br>
             -v (Volumes)         <br>
             -w (Working Dir)     <br>
             -x (Exclude)         </td>
    </tr>
    <tr>
        <td> d </td>
        <td> Delete </td>
        <td> 將指定檔名由壓縮檔內移除 </td>
        <td> -i (Include)         <br>
             -m (Method)          <br>
             -p (Set Password)    <br>
             -r (Recurse)         <br>
             -u (Update)          <br>
             -w (Working Dir)     <br>
             -x (Exclude)         </td>
    </tr>
    <tr>
        <td> e </td>
        <td> Extract </td>
        <td> 將指定檔名由壓縮檔中擷取出來 </td>
        <td> -ai (Include archives)                 <br>
             -an (Disable parsing of archive_name)  <br>
             -ao (Overwrite mode)                   <br>
             -ai (Exclude archives)                 <br>
             -i (Include)                           <br>
             -o (Set Output Directory)              <br>
             -p (Set Password)                      <br>
             -r (Recurse)                           <br>
             -so (use StdOut)                       <br>
             -x (Exclude)                           <br>
             -y (Assume Yes on all queries)         </td>
    </tr>
    <tr>
        <td> l </td>
        <td> List </td>
        <td> 顯示壓縮檔案內的檔案資訊 </td>
        <td> -ai (Include archives)                 <br>
             -an (Disable parsing of archive_name)  <br>
             -ai (Exclude archives)                 <br>
             -i (Include)                           <br>
             -p (Set Password)                      <br>
             -r (Recurse)                           <br>
             -x (Exclude)                           </td>
    </tr>
    <tr>
        <td> t </td>
        <td> Test </td>
        <td> 測試壓縮檔的完整性 </td>
        <td> -ai (Include archives)                 <br>
             -an (Disable parsing of archive_name)  <br>
             -ai (Exclude archives)                 <br>
             -i (Include)                           <br>
             -p (Set Password)                      <br>
             -r (Recurse)                           <br>
             -x (Exclude)                           </td>
    </tr>
    <tr>
        <td> u </td>
        <td> Update </td>
        <td> 用較新的同名檔案更新壓縮檔內較舊的檔案 </td>
        <td> -i (Include)         <br>
             -m (Method)          <br>
             -p (Set Password)    <br>
             -r (Recurse)         <br>
             -sfx (create SFX)    <br>
             -si (use StdIn)      <br>
             -so (use StdOut)     <br>
             -t (Type of archive) <br>
             -u (Update)          <br>
             -w (Working Dir)     <br>
             -x (Exclude)         </td>
    </tr>
    <tr>
        <td> x </td>
        <td> eXtract with full paths </td>
        <td> 以完整路徑的格式解出檔案 </td>
        <td> -ai (Include archives)                 <br>
             -an (Disable parsing of archive_name)  <br>
             -ao (Overwrite mode)                   <br>
             -ai (Exclude archives)                 <br>
             -i (Include)                           <br>
             -o (Set Output Directory)              <br>
             -p (Set Password)                      <br>
             -r (Recurse)                           <br>
             -so (use StdOut)                       <br>
             -x (Exclude)                           <br>
             -y (Assume Yes on all queries)         </td>
    </tr>
  </table>
* 常用指令
  * exit：離開程式，如果在 exit 之後有加上數字，表示傳回值，如：exit 0。在 UNIX 系統下，當程式正常結束，會傳回一個值 0，如果不正常結束則會傳回一個非 0 的數字
  * return [n]：離開所在函式，如果在其後有加數字的話，則傳回該數字。和 exit 一樣，這個指令可以傳回該函式的執行結果，0 表示正常結束
  * pwd：顯示目前所在目錄
* Touch 
  * 可用來更改檔案或目錄的時間戳記，除此之外，該指令也可以用來建立空檔案
  * 三種時間戳記
    * access time：檔案最後被讀取的時間
    * modify time：檔案最後被修改的時間
    * change time：檔案屬性(如權限、擁有者等)最後被修改的時間
    * 可使用 stat 指令來查看一個檔案的這三種時間戳記
      > stat test.sh <br>
      > touch test.sh  # 更改時間戳記 <br>
      > touch -a test.sh  # 更新 access time <br>
      > touch -m test.sh  # 更新 modify time 
    * 複製時間戳記
      > touch -r source target   # 可將 source 這個檔案的 access time 與 modify time 都複製到 target 這個檔案上
    * 指定時間戳記：詳如參考網址
    * 建立空檔案：自動建立一個空檔案，並將檔案的時間設定為目前的時間
      > touch empty.txt <br>
      > touch empty{1..10}.txt # 建立 10 個空檔案 <br>
      > touch -c file.txt    # 不要自動建立空檔案
* 函數
  * if...else...
    > if[ condition && condition ]; then <br>
    >   echo "XXX" <br>
    > elif[ condition || condition ]; then <br>
    >   echo "XXX" <br>
    > else <br>
    >   echo "XXX" <br>
    >   exit 0 <br>
    > fi 
  * while：迴圈中 commands 會一直被重複執行，直到 condition 的值為偽為止
    > while condition  <br>
    >    do  <br>
    >    commands  <br>
    >    done  <br>
  * for
    >  for name in word1 word2 … <br>
    >     do <br> 
    >     do-list  <br>
    >     done
  * case
    > case word in  <br>
    >     pattern1) list1 ;;  <br>
    >     pattern2) list2 ;;  <br>
    >     …  <br>
    > esac 
  * 函式的運用
    > name ( )  <br>
    > {  <br>
    >    statement  <br>
    > }  <br>

    > ERRLOG=$1  <br>
    > errexit ( )  <br>
    > {  <br>
    >    echo $1  <br>
    >    date >> $ERRLOG  <br>
    >    echo $1 >> $ERRLOG  <br>
    >    exit  <br>
    > }
  * 數學函數：test n1 -op n2 or [n1 -op n2]
    > -eq，==：等於 <br>
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
    > -r：file 可以讀(readable) <br>
    > -w：file 可以寫(writeable) <br>
    > -x：file 可以執行(executable) <br>
   
    > -f：file 是一般的檔案 <br>
      > ex：-f /proc_data/MAC_20220601.xls <br>

    > -d：file 為目錄 <br>
    > -u：file exists and is setuid <br>
    > -s：file 的檔案長度大於 0 <br>
    > -L：file 是連結檔 <br>
    > -b：file 是區塊特別檔 <br>
    > -c：file 是字元特別檔 <br>
    > -u：file 的 SUID 己設定 <br>
    > -g：file 的 SGID 己設定 <br>
    > -k：file 的 sticky bit 己設定
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
        <td> $n </td>
        <td> $1 為第一個參數，$2 為第二個參數，依此類推。而 $0 為這個 shell script 的檔名 </td>
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
* [Shell Script](https://www.twbsd.org/cht/book/ch24.htm)
* [基本指令](https://ithelp.ithome.com.tw/articles/10218257)
* [Linux 的 touch 指令用法教學與範例](https://blog.gtwang.org/linux/linux-touch-command-tutorial-examples/)
* [分享幾個常用的 7-Zip 壓縮與解壓縮命令](https://blog.miniasp.com/post/2021/07/07/Useful-7-Zip-7z-CLI-Command-Options)
* [7-Zip的命令列指令](https://felixx.pixnet.net/blog/post/36966417)
<br>
