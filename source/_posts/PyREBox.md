---
title: PyREBoxについて
date: 2019-02-12 15:22:57
categories:
- tool
- PyREBox
tags: 
- PyREBox
- malware analysis
---

# PyREBox
### [PyREBox](https://github.com/Cisco-Talos/pyrebox)(=Python Reverse Engineering Sandbox)
Cisco-TalosがリリースしたPythonスクリプトで操作できるQEMUベースのリバースエンジニアリング用サンドボックス(オープンソース)

### PyREBox概要
>PyREBox is a Python scriptable Reverse Engineering sandbox. It is based on QEMU, and its goal is to aid reverse engineering by providing dynamic analysis and debugging capabilities from a different perspective. PyREBox allows to inspect a running QEMU VM, modify its memory or registers, and to instrument its execution, by creating simple scripts in python to automate any kind of analysis. QEMU (when working as a whole-system-emulator) emulates a complete system (CPU, memory, devices...). By using VMI techniques, it does not require to perform any modification into the guest operating system, as it transparently retrieves information from its memory at run-time.
>
>Several academic projects such as DECAF, PANDA, S2E, or AVATAR, have previously leveraged QEMU based instrumentation to overcome reverse engineering tasks. These projects allow to write plugins in C/C++, and implement several advanced features such as dynamic taint analysis, symbolic execution, or even record and replay of execution traces. With PyREBox, we aim to apply this technology focusing on keeping the design simple, and on the usability of the system for threat analysts.
>
>PyREBox won the Volatility Plugin Contest in 2017!

PyREBoxはVolatility Frameworkのプラグインコンテストで優勝している。
Volatility Frameworkは無料配布されているメモリフォレンジックツールである。
詳しくは[Volatility Foundation](https://www.volatilityfoundation.org/)を参照。

>>[Results from the (5th Annual) 2017 Volatility Plugin Contest are in!](https://volatility-labs.blogspot.com/2017/11/results-from-5th-annual-2017-volatility.html)
>>1st: Xabier Ugarte-Pedrero (Cisco Talos): PyREBox
>>PyREBox provides an extensible reverse engineering sandbox that combines debugging capabilities with introspection. The analyst can interact with the whole system emulator, QEMU, guest either manually, using IPython, or by creating Python scripts. Unlike previous reverse engineering platforms, PyREBox, is explicitly designed for modern threat analysts and the tasks they commonly perform. PyREBox also leverages Volatility to help bridge the semantic gap challenges typically associated with virtual machine introspection.
>
>This tool was presented at HITB Amsterdam 2018. You can see the slides, or watch the presentation. It was also presented at the third edition of EuskalHack Security Congress (slides available).

</br>

PyREBoxの詳細なフレームワークと機能については以下を参照。
参考1:[PyREBox, a Python Scriptable Reverse Engineering Sandbox (MONDAY, JULY 17, 2017)](https://blog.talosintelligence.com/2017/07/pyrebox.html)
参考2:[talosintelligence.com/prebox ](https://talosintelligence.com/pyrebox)

</br>
---

### Malware Monitor
Pythonで書かれたPyREBox用のスクリプト。
マルウェア解析に有用な情報を自動的に収集することでマルウェア解析の初期段階において解析者を助ける。
主な機能はAPIトレース、ダンプ、実行コードのパスの可視化、メモリモニタ。

>`The api tracer module` allows to trace Windows API function calls, and to automatically extract the input and output parameters. An IDA Python script allows to import and visualize this information in IDA.
>
>`The dumper module` allows to dump the memory of a process during its execution. This module is configurable by the user, who can choose the best moment to trigger the memory dump.
>
>`The coverage module` collects an execution trace that can be used to colorize basic blocks in IDA. This features provides the user information about which code paths get executed, and which do not.
>
>Finally, `the memory monitor module` (refered to as interproc in the scripts), monitors different memory-related operations and events, and also allows to monitor process interaction events, like new processes created, memory injection to existing processes, and so on. This last module is orthogonal to the other three. Since it monitors process creation and opening, it allows to monitor not only the initial process, but all those related to it. For example, if api tracer is turned on, and the memory monitor detects that the first process creates a second process, api tracer will start monitoring this new process and will generate an API call trace for it as well.

GitHub: [Malware Monitor (pyrebox/mw_monitor/)](https://github.com/Cisco-Talos/pyrebox/tree/master/mw_monitor)


### 関連
- [Malware monitor：PyREBox を活用したマルウェア分析(Talos JAPAN 2018/04/24)](https://gblogs.cisco.com/jp/2018/04/talos-malware-monitor-pyrebox-for-analysis/)










