waifu2x-caffe (for Windows)
----------

 制作者 : lltcggie

本ソフトは、画像変換ソフトウェア「[waifu2x](https://github.com/nagadomi/waifu2x)」の変換機能のみを、
[Caffe](http://caffe.berkeleyvision.org/)を用いて書き直し、Windows向けにビルドしたソフトです。
CPUで変換することも出来ますが、CUDA(あるいはcuDNN)を使うとCPUより高速に変換することが出来ます。

GUI supports English and Japanese and Simplified Chinese and Korean.


 要求環境
----------

このソフトを動作させるには最低でも以下の環境が必要です。

 * OS : Windows Vista以降 64bit (32bit用exeはありません)
 * メモリ : 空きメモリ1GB以上 (ただし、変換する画像サイズによる)
 * GPU : CUDAが動くNVIDIA製GPU(CPUで変換する場合は不要)
 * Visual C++ 2013 再頒布可能パッケージがインストールされていること(必須)
    - 上記パッケージは[こちら](https://www.microsoft.com/ja-jp/download/details.aspx?id=40784)
    - `ダウンロード` ボタンを押した後、`vcredist_x64.exe`を選択し、ダウンロード・インストールを行って下さい。
    - 見つからない場合は、「Visual C++ 2013 再頒布可能パッケージ」で検索してみて下さい。

cuDNNで変換する場合はさらに

 * GPU : Compute Capability 3.0 以上のNVIDIA製GPU

自分のGPUのCompute Capabilityが知りたい場合は[こちらのページ](https://developer.nvidia.com/cuda-gpus)で調べて下さい。


 cuDNNについて
--------

cuDNNはNVIDIA製GPUでのみつかえる高速な機械学習向けのライブラリです。
cuDNNを使わなくてもCUDAで変換出来ますが、cuDNNを使うと以下のような利点があります。

 * GPUと分割サイズの設定によっては大きな画像を少し高速に変換することが出来る(GPUによっては(最新に近いGPUほど？)CUDAと全く差が無い)
 * VRAMの使用量を減らすことが出来る(CUDAのおよそ9分の1)

このような利点があるcuDNNですが、ライセンスの関係上動作に必要なファイルを配布することが出来ません。
なので、cuDNNを使いたい人は[こちらのページ](https://developer.nvidia.com/cuDNN)でWindows向けバイナリ(v3以降)をダウンロードし、
「cudnn64_4.dll」をwaifu2x-caffeのフォルダに入れて下さい。
なお、ソフトを起動している最中にdllを入れた場合はソフトを起動しなおしてください。
(cuDNNをダウンロードするにはNVIDIA Developerへの登録とCUDA Registered Developersへの登録が必要です。
 CUDA Registered Developersはおそらく(簡単な)審査があるっぽいので登録してもすぐにcuDNNをダウンロード出来るわけではありません。)

作者の環境での処理速度、VRAM使用量の計測結果は以下の通りです。
(処理時間は1回しか計っていないのでおまけです。VRAM使用量の方にご注目下さい)

GPU : GTX 660
VRAM : 2GB
処理内容 : 600*600の画像でノイズ除去と拡大、JPEGノイズ除去レベル1、拡大率2.00

CUDA

| 分割サイズ |   処理時間   | VRAM使用量(MB) |
|:-----------|:-------------|:---------------|
| 100        | 00:00:02.590 | 373            |
| 120        | 00:00:02.500 | 529            |
| 150        | 00:00:02.733 | 816            |
| 200        | 00:00:05.110 | 1229           |

cuDNN

| 分割サイズ |   処理時間   | VRAM使用量(MB) |
|:-----------|:-------------|:---------------|
| 100        | 00:00:02.532 | 42             |
| 120        | 00:00:02.423 | 58             |
| 150        | 00:00:02.343 | 87             |
| 200        | 00:00:02.331 | 149            |


 使い方(GUI版)
--------

「waifu2x-caffe.exe」はGUIソフトです。ダブルクリックで起動します。

「入力パス」欄に画像かフォルダをドラッグ&ドロップで放り込むと「出力パス」欄が自動で設定されます。
出力先を変えたい場合は「出力パス」欄を変更して下さい。

好みに合わせて変換設定を変更することが出来ます。


##入出力設定
     ファイルの入出力に関する設定項目群です。

###「入力パス」
     変換したいファイルのパスを指定します。
     フォルダを指定した場合は、サブフォルダ内も含めた「フォルダ内の変換する拡張子」が付くファイルを変換対象とします。

###「出力パス」
     変換後の画像を保存するパスを指定します。
    「入力パス」で フォルダを指定した場合は、指定したフォルダの中に変換したファイルを(フォルダ構造をそのままで)保存します。指定したフォルダがない場合は自動で作成します。

###「フォルダ内の変換する拡張子」
     「入力パス」がフォルダの場合の、フォルダ内の変換する画像の拡張子を指定します。
     デフォルト値は`png:jpg:jpeg:tif:tiff:bmp:tga`です。
     また、区切り文字は`:`です。
     大文字小文字は区別しません。
     例. png:jpg:jpeg:tif:tiff:bmp:tga

###「出力拡張子」
     変換後の画像の形式を指定します。
    「出力画質設定」と「出力深度ビット数」に設定できる値はここで指定する形式により異なります。

###「出力画質設定」
     変換後の画像の画質を指定します。
     設定できる値は整数です。
     指定できる値の範囲と意味は「出力拡張子」で設定した形式により異なります。

      * .jpg : 値の範囲(0～100) 数字が高いほど高画質
      * .webp : 値の範囲(1～100) 数字が高いほど高画質 100だと可逆圧縮
      * .tga : 値の範囲(0～1) 0なら圧縮なし、1ならRLE圧縮

###「出力深度ビット数」
     変換後の画像の1チャンネルあたりのビット数を指定します
     指定できる値は「出力拡張子」で設定した形式により異なります。

##変換画質・処理設定
     ファイル変換の処理方法、画質に関する設定項目群です。

###「変換モード」
     変換モードを指定します。
      * ノイズ除去と拡大 : ノイズ除去と拡大を行います 
      * 拡大 : 拡大を行います
      * ノイズ除去 : ノイズ除去を行います
      * ノイズ除去(自動判別)と拡大 : 拡大を行います。入力がJPEG画像の場合のみノイズ除去も行います

###「JPEGノイズ除去レベル」
    ノイズ除去レベルを指定します。レベルの高いほうがより強力にノイズを除去しますが、のっぺりとした絵になる可能性もあります。

###「拡大率」
    拡大率を指定します。
    2倍を超える拡大率の場合、(ノイズを除去する場合は最初の1回だけ行い)指定された拡大率を超えるまで2倍ずつ拡大し、2の累乗倍でない拡大率の場合は最後に縮小するという処理を行います。そのため変換結果がのっぺりとした絵になる可能性があります。

###「モデル」
    使用するモデルを指定します。
    変換対象の画像によって最適なモデルは異なるので、様々なモデルを試してみることをおすすめします。
      * 2次元イラスト(RGBモデル) : 画像のRGBすべてを変換する2次元イラスト用モデル
      * 写真・アニメ(Photoモデル) : 写真・アニメ用のモデル
      * 2次元イラスト(Yモデル) : 画像の輝度のみを変換する2次元イラスト用モデル

###「TTAモードを使う」
    TTA(Test-Time Augmentation)モードを使うかどうかを指定します。
    TTAモードを使うと変換が8倍遅くなる代わりに、PSNR(画像の評価指数の一つ)が0.15くらい上がるそうです。

##処理速度設定
     画像変換の処理速度に影響する設定項目群です。

###「プロセッサー」
    変換を行うプロセッサーを指定します。
      * CUDA(使えたらcuDNN) : CUDA(GPU)を使って変換を行います(cuDNNが使えるならcuDNNが使われます)
      * CPU : CPUのみを使って変換を行います

###「分割サイズ」
    内部で分割して処理を行う際の幅（ピクセル単位）を指定します。
    最適な(処理が最速で終わる)数値の決め方は「分割サイズ」の項で説明します。
    「-------」で区切られている上の方は入力された画像の縦横サイズの約数、
    下の方は「crop_size_list.txt」から読み出した汎用的な分割サイズです。
    分割サイズが大きすぎる場合、要求されるメモリの量(GPUを使う場合はVRAMの量)がPCで使用できるメモリを超えてソフトが強制終了するので気をつけてください。
    処理速度にそれなりに影響するので、同じ画像サイズの画像をフォルダ指定で大量に変換するときは、最適な分割サイズを調べてから変換することをおすすめします。

##その他
     画像変換の処理速度に影響する設定項目群です。

###「UIの言語」
    UIの言語を設定します。
    初回起動時はPCの言語設定と同じ言語が選ばれます。(存在しない場合は英語になります)

###「cuDNNチェック」
    「cuDNNチェック」ボタンを押すとcuDNNが使えるか調べることが出来ます。
    cuDNNが使えない場合は理由が表示されます。

「実行」ボタンを押すと変換が始まります。
途中でキャンセルしたい場合は「キャンセル」ボタンを押します。
ただし、実際に停止するまでタイムラグがあります。
プログレスバーは複数枚の画像を変更した際の進行度合いを示しています。


 使い方(CUI版)
--------

「waifu2x-caffe-cui.exe」はコマンドラインツールです。
`コマンドプロンプト` を立ち上げ、次のようにコマンドを打ち込み、使用して下さい。


以下のコマンドは、使い方を画面に出力します。
```
waifu2x-caffe-cui.exe --help
```

以下のコマンドは、画像変換を実行するコマンドの例です。
```
waifu2x-caffe-cui.exe -i mywaifu.png -m noise_scale --scale_ratio 1.6 --noise_level 2
```
以上を実行すると、`mywaifu(noise_scale)(Level2)(x1.600000).png`に変換結果が保存されます。


本ソフトでは、以下のオプションを指定することが出来ます。

###-i <文字列>,  --input_file <文字列>
     (必須)  変換する画像へのパス
     フォルダを指定した場合、そのフォルダ以下の画像ファイルを全て変換してoutput_fileで指定したフォルダへ出力します。

###-o <string>,  --output_file <string>
     変換された画像を保存するファイルへのパス
     (input_fileがフォルダの場合)変換された画像を保存するフォルダへのパス
     (input_fileが画像ファイルの場合)拡張子(最後の.pngなど)は必ず入力するようにして下さい。
     指定しなかった場合は自動でファイル名を決定し、そのファイルに保存します。
     ファイル名の決定ルールは、
     `[元の画像ファイル名]``(モード名)``(ノイズ除去レベル(ノイズ除去モードの場合))``(拡大率(拡大モードの場合))``.出力拡張子`
     のようになっています。
     保存される場所は、基本的には入力画像と同じディレクトリになります。

###-l <文字列>,  --input_extention_list <文字列>
     input_fileがフォルダの場合の、フォルダ内の変換する画像の拡張子を指定します。
     デフォルト値は`png:jpg:jpeg:tif:tiff:bmp:tga`です。
     また、区切り文字は`:`です。
     例. png:jpg:jpeg:tif:tiff:bmp:tga

###-e <文字列>,  --output_extention <文字列>
     input_fileがフォルダの場合の、出力画像の拡張子を指定します。
     デフォルト値は`png`です。

###-m <noise|scale|noise_scale>,  --mode <noise|scale|noise_scale>
     変換モードを指定します。指定しなかった場合は`noise_scale`が選択されます。
      * noise : ノイズ除去を行います (正確には、ノイズ除去用のモデルを用いて画像変換を行います)
      * scale : 拡大を行います (正確には、既存アルゴリズムで拡大した後に、拡大画像補完用のモデルを用いて画像変換を行います)
      * noise_scale : ノイズ除去と拡大を行います (ノイズ除去を行った後に、引き続き拡大処理を行います)
      * auto_scale : 拡大を行います。入力がJPEG画像の場合のみノイズ除去も行います

###-s <小数点付き数値>, --scale_ratio <小数点付き数値>
     何倍に拡大するかを指定します。デフォルト値は`2.0`ですが、2.0倍以外も指定できます。
     2.0以外の数値を指定すると、次のような処理を行います。
      * まず、指定された倍率を必要十分にカバーするように、2倍拡大を繰り返し行います。
      * 2の累乗以外の数値がしてされている場合は、指定倍率になるように拡大した画像を線形フィルタで縮小します。

###-n <1|2>, --noise_level <1|2>
     ノイズ除去レベルを指定します。ノイズ除去用のモデルはレベル1とレベル2のみ用意されているので、
      1 もしくは 2 を指定して下さい。
     デフォルト値は`1`です。

###--model_dir <文字列>
     モデルが格納されているディレクトリへのパスを指定します。デフォルト値は`models/anime_style_art_rgb`です。
     標準では以下のモデルが付属しています。
      * `models/anime_style_art_rgb` : RGBすべてを変換する2次元画像用モデル
      * `models/anime_style_art` : 輝度のみを変換する2次元画像用モデル
      * `models/ukbench` : 写真用モデル(拡大するモデルのみ付属しています。ノイズ除去は出来ません)
     基本的には指定しなくても大丈夫です。デフォルト以外のモデルや自作のモデルを使用する時などに指定して下さい。

###-p <cpu|gpu|cudnn>, --process <cpu|gpu|cudnn>
     処理に使うプロセッサーを指定します。デフォルト値は`gpu`です。
      * cpu : CPUを使って変換を行います。
      * gpu : CUDA(GPU)を使って変換を行います。Windows版でのみ、cuDNNが使えるならcuDNNを使います。
      * cudnn : cuDNNを使って変換を行います。

###-c <整数>, --crop_size <整数>
     分割サイズを指定します。デフォルト値は`128`です。

###-q <整数>, --output_quality <整数>
     変換後の画像の画質を設定します。デフォルト値は`-1`です
     指定できる値と意味は「出力拡張子」で設定した形式により異なります。
     -1の場合は、各画像形式のデフォルト値が使われます。

###-d <整数>, --output_depth <整数>
     変換後の画像の1チャンネルあたりのビット数を指定します。デフォルト値は`8`です。
     指定できる値は「出力拡張子」で設定した形式により異なります。

###-b <整数>, --batch_size <整数>
     mini-batchサイズを指定します。デフォルト値は`1`です。
     mini-batchサイズは画像を「分割サイズ」で分割したブロックを同時に処理する数のことです。例えば`2`を指定した場合、2ブロックずつ変換していきます。
     mini-batchサイズを大きくすると分割サイズを大きくするとの同様にGPUの使用率が高くなりますが、計測した感じだと分割サイズを大きくした方が効果が高いです。
     (例えば分割サイズを`64`、mini-batchサイズを`4`にするより、分割サイズを`128`、mini-batchサイズを`1`にした方が処理が速く終わる)

###-t <0|1>, --tta <0|1>
     `1`を指定するとTTAモードを使用します。デフォルト値は`0`です。

###--,  --ignore_rest
     このオプションが指定された後の全てのオプションを無視します。
     スクリプト・バッチファイル用です。

###--version
     バージョン情報を出力し、終了します。

###-h,  --help
     使い方を表示し、終了します。
     手軽に使い方を確認したい時などにどうぞ。


 分割サイズ
--------

waifu2x-caffe(waifu2xもですが)は画像を変換する時、
画像を一定のサイズ毎に分割して一つずつ変換を行い、最後に結合して一枚の画像に戻す、という処理をしています。
分割サイズ(crop_size)とは、この画像を分割する際の幅（ピクセル単位）の事です。

CUDAで変換中でもGPUを使い切れていない（GPUの使用率が100%近くまでいっていない）場合、
この数値を大きくすることで（GPUを使い切ることが出来る様になるため）処理が早く終わる可能性があります。
[GPU-Z](http://www.techpowerup.com/gpuz/)などでGPU Load(GPU使用率)とMemory Used(VRAM使用率)を見ながら調節してみて下さい。
また、以下の様な特性があるので参考にして下さい。

 * 必ずしも数値が大きければ大きいほど速くなるわけでは無い
 * 分割サイズが画像の縦横サイズの約数（あるいは割ったときに余りが少ない数）だと、無駄に演算する量が減って速くなる。 (この条件にあまり当てはまらない数値が最速になるケースもあるらしい？)
 * 数値を2倍にした場合、使用するメモリ量は4倍になる(実際は3～4倍といったところ)のでソフトが落ちないように注意。特にCUDAはcuDNNに比べてメモリの消費量がとても多いので気をつけること


 アルファチャンネル付き画像について
--------

本ソフトではアルファチャンネル付き画像の拡大も対応しています。
しかし、アルファチャンネルを単体で拡大する処理になっているため、アルファチャンネル付き画像の拡大は無い場合と比べておよそ2倍の時間がかかるので注意してください。


 The format of language files
--------

Language files format is JSON.
If you create new language file, add language setting to 'lang/LangList.txt'.
'lang/LangList.txt' format is TSV(Tab-Separated Values).

  * LangName : Language name
  * LangID : Primary language [See MSDN](https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693.aspx)
  * SubLangID : Sublanguage [See MSDN](https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693.aspx)
  * FileName : Language file name

ex.

  * Japanese  LangID : 0x11(LANG_JAPANESE), SubLangID : 0x01(SUBLANG_JAPANESE_JAPAN)
  * English(US) LangID : 0x09(LANG_ENGLISH), SubLangID : 0x01(SUBLANG_ENGLISH_US)
  * English(UK) LangID : 0x09(LANG_ENGLISH), SubLangID : 0x02(SUBLANG_ENGLISH_UK)


おことわり
------------

本ソフトは無保証です。
利用者の判断の下に使用して下さい。
制作者はいかなる義務も負わないものとします。


謝辞
------
オリジナルの[waifu2x](https://github.com/nagadomi/waifu2x)、及びモデルの制作を行い、MITライセンスの下で公開して下さった [ultraist](https://twitter.com/ultraistter)さん、
オリジナルのwaifu2xを元に[waifu2x-converter](https://github.com/WL-Amigo/waifu2x-converter-cpp)を作成して下さった [アミーゴ](https://twitter.com/WL_Amigo)さん(READMEやLICENSE.txtの書き方、OpenCVの使い方等かなり参考にさせていただきました)
に、感謝します。
また、メッセージを英訳してくださった @paul70078 さん、メッセージを中国語(簡体字)に翻訳してくださった @yoonhakcher さん、中国語(簡体字)訳のプルリクエストを下さった @mzhboy さん、メッセージを韓国語に翻訳してくださった @kenin0726 さんに感謝します。
