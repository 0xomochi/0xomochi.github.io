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
Windows, Linuxその他へのインストールについては他を参照されたし。
```bash
# Homebrew-Caskのインストール
$ brew tap caskroom/cask

# Homebrew-CaskからPowerShellをインストール
$ brew cask install powershell

# PowerShellのバージョン確認
$ pwsh -V
PowerShell v6.0.4

# PowerShellのアプデートの時は
$ brew update
$ brew cask upgrade powershell
```

## PowerShellの起動
terminalでこれを打ち込むだけ。
```bash
$ pwsh
PowerShell v6.0.4
Copyright (c) Microsoft Corporation. All rights reserved.

https://aka.ms/pscore6-docs
Type 'help' to get help.

PS >
```

## PowerShellを触ってみる
PowerShellは大文字小文字を区別しないので、`$PSV` + tab でも`$psv` + tab でも`$PSVersionTable`と補完される。

```powershell
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

出力内容を一時保存しておく。
```powershell
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
PowerShellのexit方法は以下の通り。
```powershell
PS > exit
# あるいはCtr+D
```

### Cmdlet(コマンドレット)
lsコマンドと同じようなもの(PowerShellではCmdletと呼んでいるだけ)。  
Cmdletは`[Verb]-[Nown]`の形になっている。  
例: `Get-Content`, `Set-Location`
```powershell
# Get-ChildItemはPowerShellを開いているディレクトリにあるファイルとディレクトリの一覧を表示するCmdlet(= ls)
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
PowerShellコンソール上では変数、コマンド、数字がデフォルトで色付けされていて可愛い。

## 実際に使っていく時に便利な開発環境
`Visual Studio Code`を使って開発を行うと便利。  
使いやすいので書いておく。

まず、以下から.zipファイルをダウンロードしてVSCodeをインストールする。  
[Visual Studio Code Download link](https://code.visualstudio.com/docs/?dv=osx)

↓

VSCodeにPowerShellの拡張機能をインストールする。  
Marketplaceで`PowerShell`と検索すると`PowerShell`という名前の拡張機能がヒットするのでそれをinstallする。

↓

以下のような環境でPowerShellを扱えるようになる。

<img src="https://github.com/0xomochi/images/blob/master/vscode_powershell.png?raw=true"  title="PowerShell VSCode">



## ファンクションキーについて
・F5: デバッグ実行  
・F9: ブレークポイントの設定(ブレークポイント解除の際はもう一度F9を押せば良い)  
・F8: 部分実行  


## PowerShellでWebページのStatusCheckをしてみる

statusCodeを取得するCmdlet
```powershell
$statusCode = (Invoke-WebRequest -Uri https://google.com).StatusCode
```

statusCodeが200であることを確認するCmdlet  
(200の場合は"True"を返す)
```powershell
$statusCode -eq 200
```

statusCodeが200でないことを確認する
```powershell
# -eq(equal演算子)を-notで否定する回りくどい書き方
-not($statusCode -eq 200)
```

`StatusCheck.ps1`は以下。
```powershell
# 0xomochi
# StatusCheck.ps1

$url = "https://google.com"     # configure "$url"
# statusCodeを取得する
$statusCode = try {
    # Invoke-WebRequestはstatusCode404ではなく例外を出力するためstatusCodeが200かどうかを確認するときに支障が出る
    # 例外構文を使って例外が出た時も必ずstatusCodeを取得するようにする
    (Invoke-WebRequest -Uri $url).StatusCode
}
catch {
    $Error[0].Exception.GetBaseException().Response.StatusCode.Value__
}

# statusCodeが200であるかどうかを確認する
# $validStatusCode = 200
$validStatusCode = 404
if ($statusCode -ne $validStatusCode){

    # ./StatusCheck.logというファイルが存在するかを確認する
    if (-not(Test-Path ./StatusCheck.log)) {
        # StatusCheck.logが存在しなければStatusCheck.logファイルを新しく作る
        New-Item -Path ./StatusCheck.log
    }

    # date, statusCode, urlをjsonで保存
    $json = [ordered]@{
        Date = (Get-Date).ToString("F")
        Url = $url
        StatusCode = $statusCode
     }
     $json | ConvertTo-Json -Compress
}
```

jsonファイルに`Out-File`Cmdletで書き込みを行う。
```powershell
$json | Out-File -LiteralPath ./StatusCheck.log
```

この時点での出力結果`StatusCheck.log`は以下のようになる。  
何回書き込みをしてもlogが上書きされてしまい、追記されない。
```
Name                           Value                                                                                                               
----                           -----                                                                                                               
Url                            https://google.com                                                                                                  
StatusCode                     200                                                                                                                 
Date                           2019/02/18 13:03:13 
```



先ほどと同様にjsonファイルに"Out-File"Cmdletを使って書き込みを行う。  
`-Append`は`./StatusCheck.log`に書き込む度に追記モードにするためのスイッチである。  
```powershell
$json | Out-File -LiteralPath ./StatusCheck.log -Append
```

追記モードに変更した後の出力結果`StatusCheck.log`は以下のようになる。
```
Name                           Value                                                                                                               
----                           -----                                                                                                               
Url                            https://google.com                                                                                                  
StatusCode                     200                                                                                                                 
Date                           2019/02/18 13:03:13 

Name                           Value                                                                                                               
----                           -----                                                                                                               
Url                            https://google.com                                                                                                  
StatusCode                     200                                                                                                                 
Date                           2019/02/18 13:03:13 
```



(to be updated)