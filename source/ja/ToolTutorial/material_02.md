# 02. マテリアルでトゥーン風の炎を表現する。

## 概要

本章ではマテリアルを使用してトゥーン風の炎を表現します。マテリアルなしでは難しかった複雑な表現を実現できます。

<div align="center">
<iframe src='../../Effects/viewer_ja.html#Tutorials/Mat_02/Fire.efkefc'></iframe>
<p>本章で作成するエフェクト</p>
</div>

## 作成

ただ、本章では様々な画像や3Dモデルを使用します。
ここに関しては慣れているソフトで似たような素材データを作成しましょう。

それらのデータを作成するのは手間がかかるため、既に作成した素材データをこちらに用意しました。
作成方法も少し解説しますが、素材の作成ができなかったり、作成するのが面倒な方はこれらを使用してください。

<a href="../../Effects/Tutorials/Mat_02_01.zip">ダウンロード</a>

既にモデルとマテリアルは設定してあります。
マテリアルの新規作成や基本的な使い方は前章を参照してください。

まずは最初にスクロールする雲模様のグレー画像を表示します。

``` 画像参照ノード ``` を追加して、画像には ``` Textures/Noise1.png ``` を選択します。
このままだと、ただ画像が表示されるだけです。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Grad_Ja.png
   :align: center
```

動かすために ``` 移動UVノード ``` を追加します。
そして、``` 画像参照ノード ```に接続します。
``` 移動UVノード ``` の速度には ``` (0.0, 0.4) ``` を入力します。

上下方向に画像が流れるようになりました。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Noise_Moving_Ja.png
   :align: center
```

ただ、このままでは雲模様が粗すぎるので細かくします。
``` 画像参照ノード ``` のUVに ``` 掛け算ノード ``` を接続します。
掛け算の値には``` 移動UVノード ```の出力と ``` (4.0, 1.0) ``` を指定します。

そうすると、画像のUV座標の1が4になります。
ということは、今まで1枚表示されていた範囲に4枚表示されます。
今回は横方向にたくさん表示されるようになります。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Noise_Enlarge_Ja.png
   :align: center
```

このままでは全く炎に見えません。
動きを複雑にするために歪みを加えます。

``` 画像参照ノード ``` を追加して、歪み画像 ``` Textures/Normal1.png ```を選択します。

``` 要素抽出ノード ``` を追加し、RGを抽出します。
そして ``` 引き算ノード ``` を追加し、0.5を引きます。
歪み画像の赤色と緑色は0.5を0として、上下左右に歪ませるようになっています。
そのため、中央の0.5を引きます。

さらに、歪みの強さを調節するために0.5を掛けます。

そして、歪みの値をUVに足します。

そうすると、画像が歪むようになりました。
なんとなく炎に見えるかもしれません。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Noise_Distort_Ja.png
   :align: center
```

炎は上のほうが暗く、下のほうが明るくなります。
それを実現するために、グラデーション画像を足したり掛けたりします。

``` 画像参照ノード ``` を追加し ``` Textures/Gradation1.png ``` を設定します。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Grad_Ja.png
   :align: center
```

そして、歪んだ画像と追加された画像を ``` 足し算ノード ``` で足します。

下のほうが光る画像になりました。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Grad_Add_Ja.png
   :align: center
```

次に上のほうを暗くするために、先ほどの画像と追加された画像を ``` 掛け算ノード ``` で掛けます。

そうすると上のほうが暗くなりました。
なんとなく炎のようになってきました。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Grad_Mul_Ja.png
   :align: center
```

最後に、着色します。

着色には色のついたグラデーションの画像を使用します。
``` 画像参照ノード ``` を追加し ``` Textures/Gradation2.png ``` を設定します。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Grad_Color_Ja.png
   :align: center
```

そして、要素抽出ノードで先ほどの流れる画像からRGを抽出します。
それをUVに入力します。


そうすると、グラデーションの画像に沿って色が変わるようになりました。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Grad_Color_Distort_Ja.png
   :align: center
```

入力された流れる画像の色に従って、グラデーション画像の参照する位置が変わっています。
そのため、グラデーションに従って色が変わるようになります。

黒色は0なので、画像の上を指し、白色は1なので画像の下を指します。
それを利用して、グレーの画像からカラーの画像にしています。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/LookUp.png
   :align: center
```

上下しか参照する位置を変えないのにRGの2つを入力していいのか、という問題があります。
それに関しては、今回は左右方向は色が同じのため、どんな値を入力しても変わりません。

グラデーションの画像次第ではRの値は固定して、Gのみを変えるようにしましょう。

同様に、透明度も変更します。
画像参照ノードで画像 ``` Textures/Gradation3.png ``` を追加します。
同様に流れる画像を接続します。白い部分が不透明、黒い部分が透明になります。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Grad_OpacityMask_Distort_Ja.png
   :align: center
```

画像参照ノードの出力をEmmisiveに接続します。

画像参照ノードの出力をOpacityMaskに接続します。

炎の模様が表示されました。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Output_Ja.png
   :align: center
```

ですが、細かい部分の色がおかしくなっています。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/PreResult.png
   :align: center
```

その原因は画像参照ノードで画像を参照するときに、リピートが選択されていることです。

画像にはクランプとリピートというものがあります。
これは、端をどう扱うか、というパラメーターです。

UV座標が1.0を超えた時、端を端の色として扱うか、端を繰り返すかを指定できます。
端の方を参照するとき、リピートに設定していると反対側を参照します。

左がクランプで右がリピートです。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Clamp_Repeat.png
   :align: center
```

色の値が1.0(255)を超えたり、0を下回ったりして、反対側を参照した結果、色がおかしくなります。

計算の誤差で1.0を超えることもありますし、今回に関しては画像を加算しているため、1.0を超えます。

そのため、グラデーションの画像参照ノードのリピートをクランプに変更します。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Sampler_Ja.png
   :align: center
```

これで完成です。

<div align="center">
<iframe src='../../Effects/viewer_ja.html#Tutorials/Mat_02/Fire.efkefc'></iframe>
</div>

最後に、本章で作成されたエフェクトをダウンロードできるようにしてみました。

<a href="../../Effects/Tutorials/Mat_02_02.zip">ダウンロード</a>

### 雲画像の作成方法

今回はPhotoShopを使用しています。
解像度1024の画像を新規作成し、PhotoShopで雲1を選択します。
コントラストを高め、白と黒が強く出るようにします。

そして、解像度を512に小さくして保存します。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Image_Cloud.png
   :align: center
```

### 歪み画像の作成方法

今回はPhotoShopを使用しています。
解像度1024の画像を新規作成し、PhotoShopで雲1を選択します。

次に3072x3072の画像を作成します。
先ほどの雲模様を9個コピーします。
これは、ループ画像を簡単に作れるようにするためです。
そして、法線を作成します。

最後に、中央の1024x1024の画像を切り取り、512に縮小して保存します。

```eval_rst
.. image:: ../../img/Tutorial/Mat_02/Image_Normal.png
   :align: center
```

### グラデーション画像の作成方法

PhotoShopのグラデーションで作成しています。
トゥーンなので、色が急に変化するようにしています。

## まとめ

本章では炎を作成しました。
わかりやすくするために画像を複数分けていますが、実際は軽くするために画像を結合することもあります。

ただ、このままでは固定された連続の炎が流れるのみです。
次章では、これを様々なものに応用できるようにします。
