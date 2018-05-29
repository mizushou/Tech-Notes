### Android Support library

[SupportLibrary](https://developer.android.com/topic/libraries/support-library/index.html)


- Android Support libraryパッケージとは何か？
  - Androidフレームワーク自体には含めるのは適切ではないが、アプリデベロッパーにとって有益な機能をまとめたライブラリ。
- 使うメリットは？
  - 例えば以下のような機能を利用することができる。
    1. アプリの下位互換性を保ってくれる（appcompatライブラリ [appcompat:アプリ互換性の意]）
      - アプリ自体はシステム（Android）のバージョンの確認と必要に応じた動作をする必要がない。それらを全てSupport libraryに任せることができる。
    1. 推奨されるAndroidレイアウトパターンを利用できる
      - レイアウトパターンを利用することで一般的なUIを既存のコードを利用することで実装できる
    1. TVやウェラブルなどの異なるデバイス向けのライブラリを利用できる
    1. その他のユーティリティ
- 使い方は？
  - [Support Library のセットアップ](https://developer.android.com/topic/libraries/support-library/setup.html)
- 種類
  - v4 Support Libraries
  - v7 Support Libraries
