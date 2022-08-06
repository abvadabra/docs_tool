﻿# データの復元

唐突にあなたの作品が失われる様々な理由があります。
エディタがクラッシュするかもしれないですし、バッテリーが切れるか、単純に間違ったボタンを押して数時間分の作業のエフェクトを崩壊させるかもしれません。
Effekseerのエディタは失ったプロジェクトの復元に成功する傾向を高めるために自動保存があります。

自動保存は固定時間ごとに、またエディタを閉じるたびに動作します。
自動保存はデフォルトで有効で、エディタを閉じるまで無効にできません。

## 自動保存の復元

### Last Session

`
Menu: File > Recover > Last Session
`

通常の手順でエディタが閉じられたときにテンポラリディレクトリに保存された `efk_quit.efkbac` を開きます。
このオプションはあなたがアクシデントでエディタを閉じて変更を保存していない時にプロジェクトを復元させます。
もし、このファイルが見つからない場合、エラーが表示されます。

### Auto Save

`
Menu: File > Recover > Auto Save...
`

一時的なディレクトリから自動的に保存したファイルを選択することをします。
自動的に保存されたファイルは `efk_autosave` というプレフィクスと `efkbac` という拡張子を持ちます。

これらのファイルはオプションで設定された間隔(デフォルト2分)ごとに、自動的に保存されます。
もし、間隔が0に設定された場合、自動保存は動作しません。