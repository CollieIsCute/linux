.. SPDX-License-Identifier: GPL-2.0

.. include:: ../disclaimer-zh_TW.rst

:Original: Documentation/admin-guide/README.rst

:譯者:

 吳想成 Wu XiangCheng <bobwxc@email.cn>
 胡皓文 Hu Haowen <src.res@email.cn>

Linux核心5.x版本 <http://kernel.org/>
=========================================

以下是Linux版本5的發行小記。仔細閱讀它們，
它們會告訴你關於它的一切，解釋如何安裝核心，以及遇到問題時該如何做。

什麼是 Linux？
---------------

  Linux 是 Unix 作業系統的複製版本，由 Linus Torvalds 在一個鬆散的網絡駭客
  （Hacker，無貶義）團隊的幫助下從頭開始編寫。它旨在實現兼容POSIX和
  單一UNIX規範。

  它具有在現代成熟的Unix中應當具有的所有功能，包括真正的多工、虛擬記憶體、
  共享庫、按需載入、共享的寫入時複製（COW）執行檔、恰當的記憶體管理以及包括
  IPv4和IPv6在內的複合堆疊網絡。

  Linux在GNU通用公共許可證，版本2（GNU GPLv2）下發布，詳見隨附的COPYING文件。

它能在什麼樣的硬體上運行？
-----------------------------

  雖然Linux一開始是爲32位元的x86 PC機（386或更高版本）開發的，但今天它也能運行在
  （至少）Compaq Alpha AXP、Sun SPARC與UltraSPARC、Motorola 68000、PowerPC、
  PowerPC64、ARM、Hitachi SuperH、Cell、IBM S/390、MIPS、HP PA-RISC、Intel 
  IA-64、DEC VAX、AMD x86-64 Xtensa和ARC架構上。

  Linux很容易移植到大多數通用的32位元或64位元架構，只要它們有一個分頁記憶體管理
  單元（PMMU）和一個移植的GNU C編譯器（gcc；GNU Compiler Collection，GCC的一
  部分）。Linux也被移植到許多沒有PMMU的架構中，雖然功能明顯地受到某些限制。
  Linux也被移植到了其自己上。現在可以將核心作爲使用者空間應用程式運行——這被
  稱爲使用者模式Linux（UML）。

文件
-----
網際網路上和書籍上都有大量的電子文件，既有Linux專屬文件，也有與一般UNIX問題相關
的文件。我建議在任何Linux FTP網站上尋找LDP（Linux文件項目）書籍的文件子目錄。
本README文件並不是關於系統的文件：有更好的可用資源。

 - 網際網路上和書上都有大量的（電子）文件，既有Linux專屬文件，也有與普通
   UNIX問題相關的文件。我建議在任何有LDP（Linux文件項目）書籍的Linux FTP
   網站上尋找文件子目錄。本 README 文件並不是關於系統的說明文件：上述方式找得到更好的資源。

 - 文件/子目錄中有各種README文件：例如，這些文件通常包含一些特定驅動程式的
   核心安裝說明。請閱讀
   :ref:`Documentation/process/changes.rst <changes>` 文件，它包含了升級核心
   可能會導致的問題的相關資訊。

安裝核心原始碼
---------------

 - 如果您要安裝完整的原始碼，請把核心tar檔案包放在您有權限的目錄中（例如您
   的主目錄）並將其解包::

     xz -cd linux-5.x.tar.xz | tar xvf -

   將「X」替換成核心最新版。

   【不要】使用 /usr/src/linux 目錄！這裡有一組函式庫的標頭檔使用的核心標頭檔
   （通常是不完整的）。它們應該與函式庫配對，而不是被核心的變化搞得一團糟。

 - 您還可以通過打補丁在5.x版本之間升級。補丁以xz格式發行。要通過打補丁進行
   安裝，請取得所有較新的補丁文件，進入核心原始碼（linux-5.x）的目錄並
   執行::

     xz -cd ../patch-5.x.xz | patch -p1

   請【按順序】替換所有大於當前原始碼樹版本的「x」，這樣就可以了。您可能想要
   刪除備份文件（文件名類似xxx~ 或 xxx.orig)，並確保沒有失敗的補丁（文件名
   類似xxx# 或 xxx.rej）。如果有，不是你就是我犯了錯誤。

   與5.x核心的補丁不同，5.x.y 核心（也稱爲穩定版核心）的補丁並非往上遞增，而是
   直接套用基本的5.x核心。例如，如果您的基本核心是5.0，並且希望套用5.0.3
   補丁，則必不可用到5.0.1和5.0.2的補丁。同理，如果您運行的是5.0.2核心，
   並且希望跳轉到5.0.3，那麼在應用5.0.3補丁之前，必須首先撤銷5.0.2補丁
   （即patch -R）。更多關於這方面的內容，請閱讀
   :ref:`Documentation/process/applying-patches.rst <applying_patches>` 。

   另有一替換方案，腳本 patch-kernel 可以用來自動化這個過程。它能確定當前核心版本並
   應用找到的所有補丁::

     linux/scripts/patch-kernel linux

   上面命令中的第一個參數是核心原始碼的位置。補丁需在當前目錄套用，但
   若補丁在他處，也可將另一個目錄指定爲第二個參數。

 - 確保沒有過時的 .o 文件和依賴關係::

     cd linux
     make mrproper

   現在您應該已經正確安裝了原始碼。

軟體要求
---------

   編譯和運行5.x核心需要各種軟體包的最新版本。請參考
   :ref:`Documentation/process/changes.rst <changes>`
   來了解最低版本要求以及如何升級軟體包。請注意，使用過舊版本的這些包可能會
   導致很難追蹤的間接錯誤，因此，不要假設在建立或者操作出現明顯問題時，可以
   直接升級軟體包。

爲核心建立目錄
---------------

   編譯核心時，預設情況下所有輸出文件都將與核心原始碼放在一起。使用
   ``make O=output/dir`` 選項可以爲輸出文件（包括 .config）指定替換位置。
   例如::

     kernel source code: /usr/src/linux-5.x
     build directory:    /home/name/build/kernel

   要配置和編譯核心，請使用::

     cd /usr/src/linux-5.x
     make O=/home/name/build/kernel menuconfig
     make O=/home/name/build/kernel
     sudo make O=/home/name/build/kernel modules_install install

   請注意：如果使用了 ``O=output/dir`` 選項，那麼它必須用於make的所有調用。

配置核心
---------

   即使只升級一個小版本，也不要跳過此步驟。每個版本中都會添加新的配置選項，
   如果配置文件沒有按預定設置，就會出現奇怪的問題。如果您想以最少的工作量
   將現有配置升級到新版本，請使用 ``makeoldconfig`` ，它只會詢問您新配置
   選項的答案。

 - 其他配置命令包括::

     "make config"      純文字界面。

     "make menuconfig"  基於文字的色彩選單、選項列表和對話框。

     "make nconfig"     增強的基於文字的色彩選單。

     "make xconfig"     基於Qt的配置工具。

     "make gconfig"     基於GTK+的配置工具。

     "make oldconfig"   基於現有的 ./.config 文件選擇所有選項，並詢問
                        新配置選項。

     "make olddefconfig"
                        類似上一個，但不詢問直接將新選項設置爲預設值。

     "make defconfig"   根據架構，使用arch/$arch/defconfig或
                        arch/$arch/configs/${PLATFORM}_defconfig中的
                        預設選項值創建./.config文件。

     "make ${PLATFORM}_defconfig"
                        使用arch/$arch/configs/${PLATFORM}_defconfig中
                        的預設選項值創建一個./.config文件。
                        用「makehelp」來取得您架構中所有可用平台的列表。

     "make allyesconfig"
                        通過儘可能將選項值設置爲「y」，創建一個
                        ./.config文件。

     "make allmodconfig"
                        通過儘可能將選項值設置爲「m」，創建一個
                        ./.config文件。

     "make allnoconfig" 通過儘可能將選項值設置爲「n」，創建一個
                        ./.config文件。

     "make randconfig"  通過隨機設置選項值來創建./.config文件。

     "make localmodconfig" 基於當前配置和載入的模組（lsmod）創建配置。禁用
                           已載入的模組不需要的任何模組選項。

                           要爲另一台機器創建localmodconfig，請將該機器
                           的lsmod存儲到一個文件中，並將其作爲lsmod參數傳入。

                           此外，通過在參數LMC_KEEP中指定模組的路徑，可以將
                           模組保留在某些文件夾或kconfig文件中。

                   target$ lsmod > /tmp/mylsmod
                   target$ scp /tmp/mylsmod host:/tmp

                   host$ make LSMOD=/tmp/mylsmod \
                           LMC_KEEP="drivers/usb:drivers/gpu:fs" \
                           localmodconfig

                           上述方法在交叉編譯時也適用。

     "make localyesconfig" 與localmodconfig類似，只是它會將所有模組選項轉換
                           爲內置（=y）。你可以同時通過LMC_KEEP保留模組。

     "make kvm_guest.config"   爲支援kvm客體核心啓用額外選項。

     "make xen.config"   爲支援xen dom0客體核心啓用額外選項。

     "make tinyconfig"  儘可能配置小的核心。

   更多關於使用Linux核心配置工具的資訊，見文件
   Documentation/kbuild/kconfig.rst。

 - ``make config`` 注意事項:

    - 包含不必要的驅動程式會使核心變大，並且在某些情況下會導致問題：
      探測不存在的控制器卡可能會混淆其他控制器。

    - 如果存在協處理器，則編譯了模擬數學的核心仍將使用協處理器：在
      這種情況下，數學模擬永遠不會被使用。核心會稍微大一點，但不管
      是否有數學協處理器，都可以在不同的機器上工作。

    - 「kernel hacking」配置細節通常會導致更大或更慢的核心（或兩者
      兼而有之），甚至可以通過配置一些 routines 來主動嘗試破壞劣質程式碼以發現
      核心問題，從而降低核心的穩定性（kmalloc()）。因此，您可能應該在
      「開發」、「實驗」或「除錯」特性相關問題時將此選項設定為 'n'。

編譯核心
---------

 - 確保您至少有gcc 4.9可用。
   有關更多資訊，請參閱 :ref:`Documentation/process/changes.rst <changes>` 。

   請注意，您仍然可以使用此核心運行a.out使用者程式。

 - 執行 ``make`` 來創建核心映像壓縮檔。如果您事先安裝了 lilo 以對應核心的 makefile，
   那麼也可以進行 ``makeinstall`` ，但是您可能需要先檢查特定的lilo設置。

   實際安裝必須以root身份執行，但任何正常構建都不需要。
   無須徒然使用root身份。

 - 如果您將核心的任何部分配置爲模組，那麼還必須執行 ``make modules_install`` 。

 - 詳細的核心編譯/生成輸出：

   通常，核心構建系統在相當安靜的模式下運行（但不是完全安靜）。但是有時您或
   其他核心開發人員需要看到編譯、連結或其他命令的執行過程。爲此，可使用
   「verbose（詳細）」構建模式。
   向 ``make`` 命令傳遞 ``V=1`` 來實現，例如::

     make V=1 all

   如需令構建系統也顯示出每個目標 rebuild 的原因，請使用 ``V=2`` 。預設爲 ``V=0`` 。

 - 準備一個備份核心以防出錯。對於開發版本尤其如此，因爲每個新版本都包含
   尚未除錯的新代碼。也要確保已經備份與核心對應的模組。如果要安裝的新核心與
   與原來正在使用的核心相同版本，請在進行 ``make modules_install`` 安裝
   之前備份modules目錄。

   或者，在編譯之前，使用核心配置選項「LOCALVERSION」向常規核心版本附加
   一個唯一的後綴。LOCALVERSION可以在「General Setup」選單中設置。

 - 爲了啟動新核心，您需要將核心映像（例如編譯後的
   .../linux/arch/x86/boot/bzImage）複製到常規可引導核心的位置。

 - 不再支援在沒有LILO等啓動裝載程式幫助的情況下直接從磁片引導核心。

   如果從硬碟啟動Linux，很可能使用LILO，它使用/etc/lilo.conf文件中
   指定的核心映像檔。核心映像檔通常是/vmlinuz、/boot/vmlinuz、
   /bzImage或/boot/bzImage。使用新核心前，請保存舊映像的副本，並複製
   新映像覆蓋舊映像。然後您【必須重新運行LILO】來更新載入映射！否則，
   將無法啓動新的核心映像。

   重新安裝LILO通常需要運行/sbin/LILO。您可能希望編輯/etc/lilo.conf
   文件爲舊核心映像指定一個條目（例如/vmlinux.old)防止新的不能正常
   工作。有關更多資訊，請參閱LILO文件。

   重新安裝LILO之後，您應該就已經準備好了。關閉系統，重新啓動，盡情
   享受吧！

   如果需要更改核心映像中的預設根設備、影片模式等，請在適當的地方使用
   bootloader 的 boot 選項。無需重新編譯核心即可更改這些參數。

 - 重啟新核心並享受它吧。

若遇到問題
-----------

 - 如果您發現了一些可能由於核心缺陷所導致的問題，請檢查 MAINTAINERS 
   檔案看看是否有人與令您遇到麻煩的核心部分相關。如果無人在此列出，那麼第二
   個最好的方案就是把它們發給我（torvalds@linux-foundation.org），也可能發送
   到任何其他相關的郵件列表或新聞組。

 - 在所有的缺陷報告中，【請】告訴我們您在說什麼核心，如何復現問題，以及您的
   設置是什麼的（使用您的常識）。如果問題是新的，請告訴我；如果問題是舊的，
   請嘗試告訴我您什麼時候首次注意到它。

 - 如果缺陷導致如下消息::

     unable to handle kernel paging request at address C0000010
     Oops: 0002
     EIP:   0010:XXXXXXXX
     eax: xxxxxxxx   ebx: xxxxxxxx   ecx: xxxxxxxx   edx: xxxxxxxx
     esi: xxxxxxxx   edi: xxxxxxxx   ebp: xxxxxxxx
     ds: xxxx  es: xxxx  fs: xxxx  gs: xxxx
     Pid: xx, process nr: xx
     xx xx xx xx xx xx xx xx xx xx

   或者類似的核心除錯資訊顯示在屏幕上或在系統日誌里，請【如實】複製它。
   可能對你來說轉儲（dump）看起來不可理解，但它確實包含可能有助於除錯問題的
   資訊。轉儲上方的文字也很重要：它說明了核心轉儲代碼的原因（在上面的示例中，
   是由於核心指針錯誤）。更多關於如何理解轉儲的資訊，請參見
   Documentation/admin-guide/bug-hunting.rst。

 - 如果使用 CONFIG_KALLSYMS 編譯核心，則可以按原樣發送轉儲，否則必須使用
   ``ksymoops`` 程式來理解轉儲（但通常首選使用CONFIG_KALLSYMS編譯）。
   此實用程式可從
   https://www.kernel.org/pub/linux/utils/kernel/ksymoops/ 下載。
   或者，您可以手動執行轉儲尋找：

 - 在除錯像上面這樣的轉儲時，如果您可以尋找EIP值的含義，這將非常有幫助。
   十六進位值本身對我或其他任何人都沒有太大幫助：它會取決於特定的核心設置。
   您應該做的是從EIP行取得十六進位值（忽略 ``0010:`` ），然後在核心名字列表
   中尋找它，以查看哪個核心函數包含有問題的地址。

   要找到核心函數名，您需要找到系統二進位文件，該文件可以顯示核心相關的問題。也就是
   「linux/vmlinux」文件。要提取名字列表並配對崩潰核心中的EIP，請執行::

     nm vmlinux | sort | less

   這將爲您提供一個按遞增排序的核心地址列表，從中很容易找到包含有問題的地址
   的函數。請注意，核心除錯消息提供的地址不一定與函數地址完全吻合（事實上，
   這是不可能的），因此您不能只「grep」列表：不過列表將爲您提供每個核心函數
   的起始處，作法是找到一個A地址的函數，使得A地址小於你想找的函數地址，且A地址的下一個函數
   起始地址又比你想找的地址還大。事實上，在您的問題報告中加入一些「上下文」可能是
   一個好主意，給出相關的上下幾行。

   如果您由於某些原因無法完成上述操作（如您使用預編譯的核心映像或類似的映像），
   請儘可能多地告訴我您的相關設置資訊，這會有所幫助。有關詳細資訊請閱讀
   『Documentation/admin-guide/reporting-issues.rst』。

 - 或者，您可以在正在運行的核心上使用gdb（限定唯讀；即不能更改值或設置斷點）。
   爲此，請首先使用-g選項編譯核心；適當地編輯arch/x86/Makefile，然後執行 ``make
   clean`` 。您還需要啓用CONFIG_PROC_FS（通過 ``make config`` ）。

   使用新核心重新啓動後，執行 ``gdb vmlinux /proc/kcore`` 。現在可以使用所有
   普通的gdb命令。尋找系統崩潰點的命令是 ``l *0xXXXXXXXX`` （將xxx替換爲EIP
   值）。

   用gdb無法除錯一個當前未運行的核心，因為gdb會（錯誤地）忽略了編譯核心的起始
   偏移量。