---
title: 電腦內部運作
---
現代的電腦無論其運算速度如何，只要是單一處理器 (CPU, Central Processing Unit)，都可以用下面的圖形來說明其基本架構，至於多處理器電腦系統本文不予討論。

![](https://raw.githubusercontent.com/NCNU-CALab/c.programming.im/master/images/machine.jpg)

各部分的功能如下所述：

- Memory(記憶體)：可分成很多個小格子，每個格子都可存放一筆資料。有時也可稱為 Main Memory(主記憶體)
- I/O interface(輸出入介面)：是電腦和外界的窗口，可以從外界輸入 (Input) 資料，或將資料輸出 (Output) 到外界。常見的輸入裝置如鍵盤，滑鼠; 常見的輸出裝置如螢幕，印表機。
- Control Unit(控制單元)：負責指令的擷取和解釋
- ALU(Arithmetic and Logic Unit，算數與邏輯單元)：負責算數與邏輯指令的實際執行工作，裡面有一些記憶體供計算所需，特別稱為 Register(暫存器)。暫存器的數量隨硬體而不同，此處說明用的虛擬電腦只有一個暫存器。一般常見的暫存器則有
	- 累加器 (Accumulator)：儲存執行運算的資料
	- 旗標暫存器 (Flag Register)：儲存運算處理後的 CPU 狀態
	- **程式計數器 (Program Counter)**：儲存目前執行指令所在的地址
	- 基底暫存器 (Base Register)：表達某塊記憶體空間的開始位置
	- 索引暫存器 (Index Register)：儲存基底暫存器的相對位置
	- 通用暫存器 (General Purpose Register)：儲存一般資料
	- 指令暫存器 (Instruction Register)：儲存指令。僅供 CPU 內部使用

在上圖的機器中共有 100 個記憶體可用，每個記憶體可以存放 3 個數字。Control Unit, ALU, Program Counter 在現代的硬體設計裡多做在一起，稱為 CPU(Central Processing Unit)。Pentium 4 就是 Intel 這家公司的 CPU 產品。

電腦執行一個機器指令的步驟為：

1. Fetch(擷取指令): 由 Control Unit 讀取 Program Counter 的內容，根據 Program Counter 的數值去相對應的 Memory 抓指令
2. Decode(解碼): 抓到的指令經 Control Unit 判讀，決定要如何執行該指令
3. Execute(執行): 在 ALU(Alrithmetic and Logic Unit) 裡執行該指令
4. Write(寫回): 更改 Program Counter 的內容

## 虛擬機器的指令集

不同的 CPU 其設計的指令集也不同，最常見的 x86 系列 CPU(Pentium 4 即屬於該系列)，其指令集包括幾百個指令。一般而言，電腦指令可以分類為下面幾類：

- 資料複製: 將暫存器的複製到主記憶體 (STORE)，將主記憶體複製到暫存器 (LOAD)
- 算數運算: 如加減乘除
- 邏輯運算: 判斷條件是否成立，如大於，小於，等於
- 更改 Program Counter
- 輸出入

為說明起見，我們定義以下的虛擬機器指令集：

虛擬機器的指令的格式：由 3 個數字組成，第一個數字代表指令，後面兩個數字代表記憶體的地址。機器指令執行時會改變某些記憶體 (含 Program Counter 以及暫存器等 CPU 內部的記憶體，以及 Main Memory) 的內容，而這些記憶體就決定了指令執行的結果。換句話說，電腦不記得過去，也不知道未來，他只能透過記憶體的內容知道現在的情況。

- **指令 0--LOAD XX**
	- 將 XX 記憶體的內容複製到暫存器內，然後將 Program Counter 加 1
- **指令 1--STORE XX**
	- 將暫存器的內容複製到 XX 記憶體，然後將 Program Counter 加 1
- **指令 2--ADD XX**
	- 將記憶體 XX 的內容與暫存器內的數值相加，結果放在暫存器上，然後 Program Counter 加 1
- **指令 3--SUB XX**
	- 將暫存器內的數值減掉記憶體 XX 的內容，結果放在暫存器上，然後 Program Counter 加 1　
- **指令 4--JUMP XX**
	- 將 Program Counter 的內容改為 XX
- **指令 5--SKIP**
	- 若暫存器的內容 >=0，則把 Program Counter+1，否則把 Program Counter+2
- **指令 6--INPUT**
	- 由輸入裝置讀入資料放到暫存器上，然後將 Program Counter+1　
- **指令 7--OUTPUT**
	- 由暫存器將資料送到輸出裝置，然後 Program Counter +1　
- **指令 8--CALL XX**
	- 將 (Program Counter+1) 放到堆疊上，然後 Program Counter 的內容改成 XX　
- **指令 9--RETURN**
	- 由堆疊上取出一個數值，然後將 Program Counter 的內容改成此數值　
- **指令 10--HALT**
	- 程式停止執行

**附註：**

所謂堆疊 (Stack) 是一個上方只有一個開口的容器，當要把資料放入堆疊時，是放在堆疊的最上面，當要從堆疊拿資料時，是從最上面拿。放入的動作稱為 push，拿出來的動作稱為 pop。 由於只有一個開口，你可以觀察到先放進去的資料會最後拿出來 (Firt In Last Out，FILO)。例如依序 push 1，2，3，4 四個數字，然後再 pop 四次，你會發現拿出來的順序是 4，3，2，1 。只要符合 FILO 特性的容器，就可稱為堆疊。電腦內部的 Stack 是用 Main Memory 來模擬的。

### 以下是以虛擬機器語言所寫的兩數字相加程式，-- 後面的部分是註解，是給人看的，電腦不予處理：

- INPUT -- 輸入數字到暫存器，program counter+1
- STORE  99  -- 將暫存器的內容複製到記憶體 99 的地方，program counter+1
- INPUT  -- 輸入數字到暫存器，program counter+1
- ADD  99 -- 將記憶體 99 的內容加到暫存器，program counter+1
- OUTPUT -- 將暫存器的內容輸出，program counter+1
- HALT -- 程式停止執行

### 計算 abs(x-y)

- INPUT -- 輸入數字到暫存器，program counter+1
- STORE 98 -- 將暫存器的內容複製到記憶體 98 的地方，program counter+1
- INPUT -- 輸入數字到暫存器，program counter+1
- STORE 99 -- 將暫存器的內容複製到記憶體 99 的地方，program counter+1
- SUB 98 -- 將暫存器的內容減掉記憶體 98 的內容，program counter+1
- SKIP -- 如果暫存器的內容 >= 0 則 program counter+1; 否則 program coounter+2
- JUMP 9 -- 將 program counter 設為 9，也就是跑到第 9 行的 OUT 指令
- LOAD 98 -- 將記憶體 98 的內容複製到暫存器，program counter+1
- SUB 99 -- 將暫存器的內容減掉記憶體 99 的內容 ，program counter+1
- OUTPUT -- 將暫存器的內容輸出, program counter + 1
- HALT -- 程式停止執行

### 呼叫函數將兩個數字相加的範例

- INPUT -- 輸入數字到暫存器，program counter+1
- STORE 99 -- 將暫存器的內容複製到記憶體 99，program counter+1
- INPUT -- 輸入數字到暫存器，program counter+1
- STORE 98 -- 將暫存器的內容複製到記憶體 98，program counter+1
- CALL 07 -- 將 program counter+1(也就是 5) 放到堆疊上 ，program counter 改為 07
- OUTPUT -- 將暫存器的內容輸出, program counter + 1
- HALT -- 結束程式
- LOAD 99 -- 將記憶體 99 的內容複製到暫存器上，program counter+1
- ADD 98 -- 將暫存器的內容加上記憶體 98 的內容，program counter+1
- RETURN -- 由堆疊取出一個數值 (目前最上面的為 5)，並將 program counter 設為該數值

## Software development process

撰寫程式的流程如下圖，所需要的工具包括 Text Editor, Preprocessor, Compiler, Linker 等四種

![](https://raw.githubusercontent.com/NCNU-CALab/c.programming.im/master/images/procedure.jpg)

**Text Editor**：程式設計師利用此工具編輯 (Edit)Source code file(Text Format), 也就是說產生. c 檔的工具。

**Preprocessor**：結合 Source code file(.c 檔) 和零到多個 Header files(.h 檔)，經過編輯 (Edit) 後成為另一個修改過的 Source code file。

**Compiler**：將 source code 編譯 (compile) 產生 Object code file(.o 檔, Binary Format)，同一個作業平台的 Object code 格式相同，也就是說 Object code file 可以由不同程式語言的 Compiler 產生。

**Linker**：將幾個 Object code files 連結 (link) 產生一個可執行檔 (.exe 檔)。

如果你的開發環境是在 Windows 上，下面有關 vi 和 gcc 的說明可以跳過。

在 Unix 作業系統內，最常見的 Text Editor 是 vi。

```
$ gcc hello.c
```

若 `hello.c` 完全沒有錯誤，則以上命令會產生一個執行檔 `a.out`，在命令列下打 `a.out` 即可觀察你所寫程式的執行結果。有時候若你設定的環境變數不對，則可能出現 Command Not Found 的錯誤，此時你可輸入 `./a.out`。若不想讓 `gcc` 編譯出來的執行檔叫做 `a.out`，則可下達以下命令：

```
$ gcc -o exename hello.c
```

撰寫大型程式時，很可能會有多個 .c 的原始程式檔，假設他們的名稱是 `source1.c`,  `source2.c`, `source3.c`... 如果要個別編譯這些 source code，則下達如下命令：

```
$ gcc -c source1.c
```

若編譯成功，會產生 `source1.o` 的檔案。要將數個 Object Code 連結成為一個可執行檔，則下達以下命令：

```
$ gcc source1.o source2.o source3.o
```

至於 Windows 作業系統上的程式編寫環境，最常使用的是 Microsoft Visual Studio，裡面就包含了 Visual C++ 的開發環境。因為 C++ 可視為 C 語言的父集合，因此你也可以用 Visual C++ 來開發 C 程式。這類視覺化的開發工具 (Dev C++ 也是其中一種)，已經把 Text Editor, Preprocessor, Compiler, Linker 整合在一起， 並包括 Project Management(專案管理) 等功能，因此這類工具又稱為 Integrated Development Environment(IDE，整合開發環境)。
