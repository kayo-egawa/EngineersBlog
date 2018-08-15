---
title: FFRI Dataset 2018 の紹介 1/2
date: 2018-07-30 15:51:02
tags:
- マルウェア研究用データセット
- MWS
author: 中川 恒
---

## はじめに
FFRI リサーチエンジニアの中川です。弊社エンジニアブログの記念すべき第1回目の記事を執筆することになりました。よろしくお願いします。

記事の内容に入る前に、新しく始めたこのエンジニアブログについて簡単に紹介したいと思います。
弊社ではすでに、[FFRI 公式ブログ](https://www.ffri.jp/blog/index.htm)を公開しており、弊社製品の防御実績の紹介、主点情報などを掲載しています。本エンジニアブログは技術者向けに次のような情報を発信することを意図して開設されたブログとなります。

- 注目するべき脆弱性の情報、PoCの紹介。
- 弊社エンジニアが業務において利用しているツールの紹介。
- 弊社が外部向けに公開したツールの紹介。
- サイバーセキュリティ関連のカンファレンスのサーベイレポート。

第1回目はMWS 2018プレミーティングで公開したFFRI Dataset 2018の紹介をしていきたいと思います。

## FFRI Datasetについて
弊社では2013年から毎年、情報処理学会コンピュータセキュリティ研究会マルウェア対策人材育成ワークショップ (MWS) において、研究者向けにデータ・セットを配布しています。このデータ・セットには弊社が独自に収集したマルウェアの解析ログが含まれており、主に機械学習での利用を意図して、こうしたデータ・セットの配布を行っております。

主に次のような理由からこうしたデータ・セットを毎年配布しています。
- データ・セットを利用した共同研究を行うため。
- 弊社の知名度の向上のため。

## FFRI Dataset 2017まで

2013年から2017年まで、[Cuckoo Sandbox](https://cuckoosandbox.org/)で実行したマルウェアの動的解析ログを提供していました。下にデータセットの一部を示します。

```
        {
            "families": [],
            "description": "Allocates read-write-execute memory (usually to unpack itself)",
            "name": "allocates_rwx",
            "markcount": 1,
            "references": [],
            "marks": [
                {
                    "pid": 3568,
                    "cid": 251,
                    "call": {
                        "category": "process",
                        "status": 1,
                        "stacktrace": [],
                        "api": "NtProtectVirtualMemory",
                        "return_value": 0,
                        "arguments": {
                            "base_address": "0x6c161000",
                            "length": 4096,
                            "protection": 64,
                            "process_identifier": 3568,
                            "process_handle": "0xffffffff"
                        },
                        "time": 1494420463.232368,
                        "tid": 3572,
                        "flags": {
                            "protection": "PAGE_EXECUTE_READWRITE"
                        }
                    },
                    "type": "call"
                }
            ],
            "severity": 2
        },
```
FFRI Dataset 2017のデータの一部。

動的解析のログとして、実行時のAPIのコール、ファイルハンドル、プロセスidの情報などがJSONファイルとして出力されています。
毎年3000検体、多いときで8000検体ほどの動的解析ログを提供していましたが、次のような問題点がありました。

- 提供できる検体数の限界
動的解析のログを取得するのに1検体で180秒程度かかり、提供する検体数を増やすことに限界がありました。例えば50万検体の動的解析ログ情報を取得しようとした場合、1台のマシンで動的解析ログを収集すると約1000日かかることになります。

- 提供される情報の可読性
静的情報と比較して動的情報は可読性が低く、利用に際しての障壁がありました。

- マルウェアの解析ログのみの提供
マルウェアの解析ログのみの提供となっており、良性ファイルの情報を提供していませんでした。

公開当初は動的解析ログを用い、亜種を判別するのに利用される事例などもありましたが、上に挙げた問題点もあり、年々利用事例が減少しておりました。
こうした問題点を踏まえ、本年度新たにリリースしたのがFFRI Dataset 2018です。

## FFRI Dataset 2018について
FFRI Dataset 2018は2017までと違い、次のような特徴を持ったデータセットとなっています。

- 動的解析ログではなく、ファイルの表層情報(各種ファイルハッシュ値、ヘッダーダンプなど)を提供。
- 十分なデータ数(マルウェア: *290000*件、良性ファイル: *210000*件)を提供。
- マルウェアだけでなく、良性ファイルも提供。
- データセット作成に用いたマルウェアとしては、弊社がVirusTotalなどから独自に入手したものを利用。
- データセット作成に用いた良性ファイルとしては、弊社がVectorで配布されているフリーウェアなどを利用。

具体的にどういった内容なのか、見たほうがわかりやすいと思います。以下にデータセットの内容の一部を示します。

```
"date","md5","sha1","sha256","ssdeep","imphash","impfuzzy","Totalhash","AnyMaster","AnyMaster_v1.0.1","EndGame","Crits","peHashNG","Platform","GUI Program","Console Program","DLL","Packed","Anti-Debug","mutex","contains base64","AntiDebugMethod","PEiD","TrID"
"20171211","9324c53e63ab2a964a0acfce58aea380","1bc7ac68993ac2c631de114f8143427ab576f733","eed327c8c212a6770cca4ceddc76f4553bfd5a737960bad4780b1d1925812727","12288:dKcd8fWzsUIz7GpvhjbA3Ny/aYrnj9jEsX2utrxAG7ENYLU+fIT:dKcCfWzqz7Gp5MdQBtrxzY4fIT","d48847f05c5fd88a678b7af8d736f338","12:oZG0Oov81VjLz01BEU/m4jv3oA+SQSLCnQ6iA5kXtAhS:YlOovk1Lz0vEU/1T3F+SQSLCnQnAKdAE","39e824f389c0c799338dae982b5f5cddc70d6d4a","d568373b72e49cc695d6c889a2484676962c7b3b","47b87614d3bba15f990c6ba7616dd198c2d5cae8","80909742a51a0eb3c23250932d6d6f6e","0f3a3d35f76bd0ef3edfd0ff9babefd96fe447f5","98682512ab52900b1e3be694040982d183ca0eef48544b0e83207efed1ca2e2e","32 bit","yes","no (yes)","no","yes","yes","no","yes","","MASMTASM|TASM_MASM|TASM_MASM_additional|PE_Diminisher_v01_additional","37.5% (.EXE) InstallShield setup (43053/19/16)\n27.2% (.EXE) Win32 Executable MS Visual C++ (generic) (31206/45/13)\n24.0% (.EXE) Win64 Executable (generic) (27625/18/4)\n3.9% (.EXE) Win32 Executable (generic) (4508/7/1)\n1.9% (.EXE) Win32 Executable MS Visual FoxPro 7 (2249/79)"
```
FFRI Dataset 2018のデータの一部。

データセットはCSVファイルファイルとして提供しています。データフィールドとして、マルウェアのクラスタリングに有用とされている各種ハッシュ値、GUI Programであるのか、アンチデバッグ技法が使われているのかなどの情報を掲載しております。以下にデータフィールドの内容と取得に用いたツールについて簡単にまとめています。

各データフィールド:
- date: ファイルの入手時期 (マルウェアにのみ記載)
- md5, sha1, sha256: 暗号学的ハッシュ関数によるファイルのハッシュ値
- ssdeep: ファジーハッシュ関数によるファイルのハッシュ値
- imphash, impfuzzy: IATの情報に基づいて計算されるハッシュ値
- Totalhash, AnyMaster, AnyMaster_v1.0.1, EndGame, Crits, peHashNG: PEのセクション情報、エントロピーの情報などに基づいて計算されるハッシュ値
- Platform: アーキテクチャ (32bitか64bitか)
- GUI Program: GUIプログラムか否か
- Console Program: コンソールプログラムか否か
- DLL: DLLか否か
- Packed: パッカーがしようされているか否か
- Anti-Debug: アンチデバッグ技法が利用されているか否か
- mutex: ミューテックスが利用されているか否か
- contains base64: base64が含まれているか否か
- AntiDebugMethod: アンチデバッグ技法の名称
- PEiD: [PEiD](https://github.com/K-atc/PEiD)のシグニチャ名
- TrID: [TriD](http://mark0.net/soft-trid-e.html)によるファイル形式に関する情報

取得に用いられたツール:
- ssdeep: https://github.com/ssdeep-project/ssdeep
- imphash: https://github.com/erocarrera/pefile
- impfuzzy: https://github.com/JPCERTCC/impfuzzy
- Totalhash, AnyMaster, AnyMaster_v1.0.1, EndGame, Crits, peHashNG: https://github.com/knowmalware/pehash
- Platform, GUI Program, Console Program, DLL, Packed, Anti-Debug, mutex, contains base64, AntiDebugMethod: https://github.com/K-atc/PEiD

また、各種ハッシュ値の情報に加え、[pefile](https://github.com/erocarrera/pefile)による各ファイルのPEヘッダーダンプの情報も提供しています。

```
----------Parsing Warnings----------

Suspicious flags set for section 0. Both IMAGE_SCN_MEM_WRITE and IMAGE_SCN_MEM_EXECUTE are set. This might indicate a packed executable.

Suspicious flags set for section 1. Both IMAGE_SCN_MEM_WRITE and IMAGE_SCN_MEM_EXECUTE are set. This might indicate a packed executable.

Imported symbols contain entries typical of packed executables.

----------DOS_HEADER----------

[IMAGE_DOS_HEADER]
0x0        0x0   e_magic:                       0x5A4D
0x2        0x2   e_cblp:                        0x50
0x4        0x4   e_cp:                          0x2
0x6        0x6   e_crlc:                        0x0
0x8        0x8   e_cparhdr:                     0x4
0xA        0xA   e_minalloc:                    0xF
0xC        0xC   e_maxalloc:                    0xFFFF
0xE        0xE   e_ss:                          0x0

...
```

現状ではヘッダーダンプをテキスト形式で配布しており、少しパースが難しくなっています。JSONやXML形式での配布を現在検討中です。

## データ・セットの利用方法
ここまで読んで、

「実際にFFRI Dataset 2018を使って、機械学習モデルとか作れないの？モデルを作って評価していないの？」

と疑問に思われた方は多いと思います。答えは「できます！」です。
実は先日の弊社新人研修にて、FFRI Dataset 2018を用い、マルウェアを分類する機械学習モデルを作成するという課題に(私も含め)新人が知恵を絞りました。最終的に、アンサンブル学習を用いTPR 75% FPR 0.03%を達成するモデルが作成され、表層情報を特徴量としてうまく機械学習を適用することができることがわかりました！！
次回は、その機械学習モデルの詳細について記事を公開する予定です。


## 共同研究募集
弊社ではFFRI Dataset 2018を利用した共同研究を絶賛募集中です。興味のある方はお問い合わせください。
