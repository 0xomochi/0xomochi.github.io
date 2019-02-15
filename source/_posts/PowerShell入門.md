---
title: PowerShell~入門~
date: 2018-09-10 15:12:56
categories:
- PowerShell
- beginner
tags:
- PowerShell
---
## PowerShellのインストール(for mac)
Windows, Linuxその他へのインストールについては他を参照されたし
```
// Homebrew-Caskのインストール
$ brew tap caskroom/cask

// Homebrew-CaskからPowerShellをインストール
$ brew cask install powershell

// PowerShellのバージョン確認
$ pwsh -V
PowerShell v6.0.4

// PowerShellのアプデートの時は
$ brew update
$ brew cask upgrade powershell
```

## PowerShellの起動
terminalでこれを打ち込むだけ
```
$ pwsh
PowerShell v6.0.4
Copyright (c) Microsoft Corporation. All rights reserved.

https://aka.ms/pscore6-docs
Type 'help' to get help.

PS >
```

## PowerShellを触ってみる
PowerShellは大文字小文字を区別しないので`$PSV` + tab でも`$psv` + tab でも`$PSVersionTable`と補完される
```
PS > $PSVersionTable

Name                           Value
----                           -----
PSVersion                      6.0.4
PSEdition                      Core
GitCommitId                    v6.0.4
OS                             Darwin 17.7.0 Darwin Kernel Version 17.7.0: Thu Jun 21 22:53:14 PDT...
Platform                       Unix
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```
出力内容を一時保存しておく
```
PS > $ver = $PSVersionTable
PS > $ver

Name                           Value
----                           -----
PSVersion                      6.0.4
PSEdition                      Core
GitCommitId                    v6.0.4
OS                             Darwin 17.7.0 Darwin Kernel Version 17.7.0: Thu Jun 21 22:53:14 PDT...
Platform                       Unix
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```
PowerShellのexit
```
PS > exit
// あるいはCtr+D
```

### Cmdlet(コマンドレット)
lsコマンドと同じようなもの(PowerShellではCmdletと呼んでいるだけ)
Cmdletは`[Verb]-[Nown]`の形になっている
例: `Get-Content`, `Set-Location`
```
// Get-ChildItemはPowerShellを開いているディレクトリにあるファイルとディレクトリの一覧を表示するCmdlet(= ls)
PS > Get-ChildItem


    Directory: /Users/omochi


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2018/08/24      1:08                Applications
d-----       2018/09/10     15:11                Desktop
d-----       2018/09/05     10:36                Documents
d-----       2018/09/09     21:03                Downloads
d-----       2018/09/09     18:10                Library
d-----       2018/08/24      0:07                Movies
d-----       2018/08/24      0:07                Music
d-----       2018/08/24      0:07                Pictures
d-----       2018/08/24      0:07                Public
```
### シンタックスハイライト
PowerShellコンソール上では変数、コマンド、数字がデフォルトで色付けされていて可愛い
