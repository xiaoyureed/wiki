---
title: Autohotkey 脚本集
tags:
  - autohotkey
categories: tool
date: 2018-04-02 10:42:48
---

<div align="center">
autohotkey note & 我的脚本集
https://greyli.com/autohotkey-automate-your-life/
开机启动文件夹 # r ---> shell:startup
</div>

<!--more-->

# quickstart

*   热键， run(相应的有RunWait xx表示程序xx关闭后执行命令); eg: `#space::Run www.baidu.com`同时按下win+space会调用默认browser访问指定地址(命令和变量的名称是不区分大小写的)

    *   保存为demo.ahk, 双击执行; 
    *   #表示win键, space表示空格键, 双冒号 :: 表示每次按下此热键时会执行其后续命令, run表示运行后面的语句,  文档, URL 或快捷方式. 这里有一些常见示例
        ```
        Run Notepad
        Run C:\My Documents\Address List.doc // 如果程序没有包含在env变量中, 需要指定完整的路径, 如Run %A_ProgramFiles%\Winamp\Winamp.exe;
        Run C:\My Documents\My Shortcut.lnk
        Run www.yahoo.com
        Run mailto:someone@somedomain.com
        ```
    *   其他一些例子:
        `#n::Run Notepad`win+n唤起记事本程序
        `^!c::Run calc.exe`ctrl+alt+c唤起指定程序
    *   多行热键: 执行多行命令, 请把首行放在热键定义的下面，且在最后行命令的下一行添加 return
        ```
        #n::
        Run http://www.google.com
        Run Notepad.exe
        return
        ```
*   热字串
    `::btw::by the way`输入“Btw”跟着空格或逗号时，会把它替换为“By the way”：

*   发送键击&鼠标点击, Send 命令可以发送键击到活动的 (最前面的) 窗口
    Control+Alt+S被设置为输入签名的热键
    ```
    ^!s::
    Send Sincerely,{Enter}John Smith ;;;发送字符串Sincerely,{Enter}John Smith, {enter}表示换行键
    Send ^c!{tab}pasted:^v ;;;发送ctrl+c跟着atl+tab键, 跟着字符串 "pasted:" 和 Control+V
    return
    ```

# demo

```ahk
; 热键, 见 https://wyagd001.github.io/zh-cn/docs/Hotkeys.htm
^u::
  Send, Hahahah first demo
Return

; 热字串, 空格即出, 注意末尾的空格也有效
::btw::By the Way, 

```

# my scripts

```ahk
#NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
; #Warn  ; Enable warnings to assist with detecting common errors.
SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.

; 交换win和ctrl和alt
;LWin::LControl
;LControl::LWin
; LAlt::LControl

;-----------------------------------方向键映射 start--------------------------------------------------------------
CapsLock & i::
    Send, {up}
return 

CapsLock & k::
    Send, {down}
return

CapsLock & j::
    Send, {left}
return


CapsLock & l::
    Send, {right}
return

;-----------------------------------方向键映射 end-------

;-----------------------------------方向键选择 start-----
; o, p 单个字符左右选择, nm整个单词左右选择

CapsLock & o::
    Send, +{left}
return

CapsLock & p::
    Send, +{right}
return

CapsLock & n::
    Send, ^+{left}
return

CapsLock & m::
    Send, ^+{right}
return



;----------------------------------- &选择 end---------

; --------------------右边alt, win改为home和end. start--------------
RAlt::Home
RWin::End

; ------------大写锁定 & 退格=删除一个单词--------------
CapsLock & BS::
    Send, ^+{Left}{BS}
return

;---------------------------code上下移动(alt+i/k)--------------

!i::
    Send, !{up}
return
!k::
    Send, !{down}
return

;;;;;;;;;;;;;;;;;;;;; alt + m, 向下复制一行 ;;;;;;;;;;;;
!m::
    Send, ^!{down}
return

;;;;;;;;;;;;;;;;;;;;; 标签页切换 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

!w::
    Send, ^{Tab}
return

!q::
    Send, ^+{Tab}
return

; D:\sync\我的坚果云\green\AutoHotkeyHelp_CN\scripts\autohotkey.ahk
; ctrl & shift & c ---> 复制文件路径
^+c::
; null= 
Send ^c
sleep,200
clipboard=%clipboard% ;%null%
tooltip,%clipboard%
sleep,500
tooltip,
return

; win & c ---> 复制16进制颜色 23B167
#c::
MouseGetPos, mouseX, mouseY
; 获得鼠标所在坐标，把鼠标的 X 坐标赋值给变量 mouseX ，同理 mouseY
PixelGetColor, color, %mouseX%, %mouseY%, RGB
; 调用 PixelGetColor 函数，获得鼠标所在坐标的 RGB 值，并赋值给 color
StringRight color,color,6
; 截取 color（第二个 color）右边的6个字符，因为获得的值是这样的：#RRGGBB，一般我们只需要 RRGGBB 部分。把截取到的值再赋给 color（第一个 color）。
clipboard = %color%
; 把 color 的值发送到剪贴板
return

; Chrome没打开状态时候 –> 打开；打开没激活状态时候 –> 激活；打开处在激活状态时候 —> 隐藏
#CapsLock::
IfWinNotExist ahk_class Chrome_WidgetWin_1
{
    Run "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe"
    WinActivate
}
Else IfWinNotActive ahk_class Chrome_WidgetWin_1
{
    WinActivate
}
Else
{
    WinMinimize
}
Return

; -----------------------------------------------------

; Ctrl+Shift+T 在当前目录打开cmder / git bash
CapsLock & g::
Process,Exist,explorer.exe
if (%ErrorLevel% != 0){
    if WinActive("ahk_class CabinetWClass"){
        path := getExplorerPath()   
        ; MsgBox "%path%"
        Run, git bash "%path%"
        return
    } ; 如果在桌面
    if WinActive("ahk_class WorkerW"){
        Run ,git bash %A_Desktop%
        return
    }
}
; 获取当前路径的函数
getExplorerPath(){
    IfWinExist, ahk_class CabinetWClass
    {
        ControlGetText,address,ToolbarWindow323,ahk_class CabinetWClass
        StringLen, length, address
        StringRight, path, address, length-4
        return path
    }
    return
}

; -----------------------------------------------------------------------

; :*:string:: 输入完后直接执行，无需空格，回车，tab键 

; MouseGetPos, mouseX, mouseY
; if (%mouseY% > 1000) {
; WheelUp::Send {Volume_Up}     ; 在任务栏上滚动滚轮:增加/减小音量.
; WheelDown::Send {Volume_Down} ;
; }
```

# 优秀脚本收集

```js
#NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
; #Warn  ; Enable warnings to assist with detecting common errors.
SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.


;url编码解码     
uriEncode(str) {       f = %A_FormatInteger%       SetFormat, Integer, Hex       If RegExMatch(str, "^\w+:/{0,2}", pr)          StringTrimLeft, str, str, StrLen(pr)       StringReplace, str, str, `%, `%25, All       Loop          If RegExMatch(str, "i)[^\w\.~%]", char)             StringReplace, str, str, %char%, % "%" . Asc(char), All          Else Break       SetFormat, Integer, %f%       Return, pr . str    }    uriDecode(str) {       Loop          If RegExMatch(str, "i)(?<=%)[\da-f]{1,2}", hex)             StringReplace, str, str, `%%hex%, % Chr("0x" . hex), All          Else Break       Return, str    }     
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;快速命令;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;    
:://g::    Run http://www.google.com     return    
:://b::    Run http://www.baidu.com    return     
:://s::    Run C:\Program Files\Everything\Everything.exe    return     
:://qq::    Run C:\Program Files\Tencent\QQ\Bin\QQ.exe    return     
:://cmd::    Run cmd    return     
:://n::    Run notepad    return     
:://d::    Run C:\Program Files\http://dict.cn\DianDian.exe    return     
:://e::    Run explorer    return     
;打开任务管理器    
:://t::    if WinExist Windows 任务管理器    WinActivate    else    Run taskmgr.exe    return     
;一些经常输入的字符串    
:://m::    Send babyking1949@gmail.com    return     
;打开系统属性    
:://sys::    Run control sysdm.cpl    return     
;打开autohotkey 配置文件    
:://ahk::    Run D:\GreenSoft\AutoHotKey 中文版\AutoHotKey.ini    return    
;;;;;;;;;;快速打开程序(快捷键);;;;;;;;;;;;;;;;;;;;;;;;;     
;用google搜索    
!g::    Send ^c    Run http://www.google.com/search?q=%clipboard%    return    
;用百度搜索    !b::    Send ^c    Run http://www.baidu.com/s?wd=%clipboard%    return     
!t::    Send ^c     Run http://s.taobao.com/search?q=%clipboard%    return     
;快速打开记事本    #n::    If WinExist Untitled - Notepad    WinActivate    else    Run Notepad    return     

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;通用键的映射;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;     
;对windows下的一些常用键进行映射,与苹果下的一些习惯一样(苹果下的快捷键有些非常合理:)    
!f::Send ^f  ;查找    
!q:: !F4    ;退出    
!w::Send ^w ;关闭网页窗口    
!r::Send #r ;调出cmd    
!s::Send ^s ;保存    
!n::Send ^n ;新建    
!z::Send ^z    
;选择文字    
!,::Send ^+{Left}    
!.::Send ^+{Right}    return    
!y:: Send+{Home}  ;选择当前位置到行首的文字    
!p:: Send +{End}   ;选择当前位置到行末的文字     
;选择一行    
!a::    Send {Home}    Send +{End}    return     
;鼠标的左右键实现任务切换,对thinkpad trackpoint 特别有用    
~LButton & RButton::AltTab    
~LButton & MButton::MsgBox,hello     
;<短时间双击alt键切换capslock键>    
;~ 设置一个时钟，比如400 毫秒，    
;~ 设置一个计数器，Alt_presses，按击次数，每次响应时钟把计数器清0复位    
#Persistent    
$Alt::
    if Alt_presses > 0 ; SetTimer 已经启动，所以我们记录按键。
    {        Alt_presses += 1        return    }
    ;否则，这是新一系列按键的首次按键。将计数设为 1 并启动定时器：
    Alt_presses = 1
    SetTimer, KeyAlt, 300 ;在 300 毫秒内等待更多的按键。
    return
    
    KeyAlt:
    SetTimer, KeyAlt, off
    if Alt_presses = 1 ;该键已按过一次。
    {        Gosub singleClick    }    else if Alt_presses = 2 ;该键已按过两次。
    {    Gosub doubleClick    }     ;不论上面哪个动作被触发，将计数复位以备下一系列的按键：
    Alt_presses = 0    return
    
    singleClick:
    send {alt}    return
    doubleClick:
    if GetKeyState("Capslock", "T")
    SetCapsLockState,off    else    SetCapsLockState,on    return     
    ;</短时间双击alt键切换capslock键>     
    ;copy cut paste 的快捷键
    !c::Send ^c    !x::Send ^x    !v::Send ^v
    ;上页翻页键映射
    !h::Send {PgUp}    !;::Send {PgDn}    
    ;HOME END键映射
    !u:: Send {Home}      ;    !o:: Send {End}   ;    ;Alt + jkli 实现对方向键的映射,写代码的时候灰常有用    !j:: Send {left}    !l:: Send {right}    !i:: Send {up}    !k:: Send {down}     ;Delete Backspace的映射    ;!f::Send {Backspace}    !d::Send {Delete}    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;通用键的映射;(结束);;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;     ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;实用功能;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;    ;win键 + PrintScreen键关闭屏幕    #PrintScreen::    KeyWait PrintScreen    KeyWait LWin ;释放左Win键才激活下面的命令    SendMessage,0x112,0xF170,2,,Program Manager ;关闭显示器。0x112:WM_SYSCOMMAND，0xF170:SC_MONITORPOWER。2：关闭，-1：开启显示器    Return     /*    ;双击鼠标右键在窗口最大化与正常状态之间切换    WinStatus:=0    RButton::        KeyWait, RButton    ;松开鼠标右键后才继续执行下面的代码        keyWait, RButton, D T0.15  ;在 100 毫秒内等待再次按下鼠标右键，可以设置一个自己觉得适合的等待时间。     If ErrorLevel            Click, Right        Else        {             if WinStatus=0    {         WinMaximize , A         WinStatus:=1    }    else    {         WinRestore ,A         WinStatus:=0    }        }    Return    !m::    if WinStatus=0    {         WinMaximize , A         WinStatus:=1    }    else    {         WinRestore ,A         WinStatus:=0    }    return     ;命令行cmd里可以ctrl v    #IfWinActive ahk_class ConsoleWindowClass    ^v::    MouseClick, Right, %A_CaretX%, %A_CaretY%,,0    send p    return     */    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;实用功能(结束);;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;     ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;<Chrome>;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;    #IfWinActive ahk_class Chrome_WidgetWin_0    !,::Send ^+{Tab}    !.::Send ^{Tab}    !1::Send ^+{Tab}    !2::Send ^{Tab}    !n::Send ^t    !/::Send ^w    !z::Send ^+t    !-::Send ^-    !=::Send ^=     ;选择当前位置到页尾的文字,适用于浏览器    F2::    Send ^+{End}     ;Send ^c    return     ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;<资源管理器>;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;    #IfWinActive ahk_class CabinetWClass    !f::    Run C:\Program Files\Everything\Everything.exe    return     ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;<Notepad>;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;    #IfWinActive ahk_class Notepad    ;!u::Send ^{Home}    ;!o::Send ^{End}    return     ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;<Eclipse>;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;    #IfWinActive,Eclipse    !.::Send ^{F8}    return     ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;<VisualStudio 2008>;;;;;;;;;;;;;;;;;;;;;;;    #IfWinActive ahk_class wndclass_desked_gsk    !/::Send ^{Tab}    !.::Send ^+{Tab}    !m::Send !+{Enter}    return     ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;<ADB>;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;     ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;<Onenote>;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;    #IfWinActive ahk_class OfficeTooltip    !u::Send ^{Home}    !o::Send ^{End}    return
```