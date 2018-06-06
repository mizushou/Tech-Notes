 ### R.javaとリソースID
  - https://www.javadrive.jp/android/xml_layout/index2.html
    - R.java
      - なにもの?
        - リソースとリソースIDの紐付け管理しているファイル
      - いつ作られる？
        - プロジェクトを始めてビルドする時に作成される。その後ビルドが行われるたびに自動更新される。
      - これを使うメリットは?
        - 開発者はリソースを追加するだけで裏で自動でIDの割り振りおよび管理をしてくれる。そのため開発者はリソース毎のIDを意識してなくていい。
        - 意識するのはリソースIDを格納する変数のみ。（シンボリックリンクのイメージ）
  - リソースID
    - なにもの?
      - R.javaで定義されているリソース毎に割り振られた整数値
        - R.javaで以下のように定義（紐付け）がされている
        - 実際にリソースIDを使う際は整数を直接使用することはなく、リソースIDである整数値を格納しているint型の変数を使用する（またはドキュメントなどでリソースIDと出てくる時この変数で説明していることがほどんど）
          ```java
          public static final class color {
            public static final int colorPrimary=0x7f040027;
          }
          ```
  - ポイント
    - **Androidの部品には全てID（整数値）が割り振られている**
    - そのIDを用いればプログラム（Java）から全ての部品を参照できる(たぶん)
      - 注意点：リソースIDを取得し、そのIDから実際のリソース（値）に変換する必要がある
---

### スタブメソッド
  - スタブメソッド
    - なにもの？
      - 中身のないメソッド
      - stubは「切り株」の意味。幹（実装）はないものの、いずれ幹（実装）ができるものとして用意したメソッドをスタブメソッドと呼ぶ。
    - いつ作られる？
      - アプリケーションの大枠の流れを作るためにひとまずコンパイルを通す時など
    - これを使うメリットは？
      - 大まかなアプリケーションの流れを作成できる
    - ポイント
      - コンパイルを通すだけなので
        1. アクセス修飾子はpublicにしておく
        2. 呼び出し元を考慮し、適切な戻り値にしておく
        3. 呼び出し元を考慮し、適切な引数にしておく

---

### アクティビティ
[アクティビティ](https://developer.android.com/guide/components/activities.html)
- なにもの？
  - 画面を提供するアプリケーションコンポーネント（**アクティビティはあくまでもただの画面**）
  - ユーザーインタフェース（ボタンなど）をViewを配置することで実装する
    - ルートのViewGroupをsetContentView()に渡して実装する
- どうやって作るの？
  1. Activityのサブクラスを作成する
    - **アクティビティのライフサイクルの状態の切り替え時（アクティビティの作成、停止、再開、破棄など）にシステムが呼び出すコールバックメソッド** を実装する必要がある
      - caller（呼ぶ人）:システム（Android OS)
      - callbackメソッド（呼ばれる人）:
        - callbckメソッドの種類
          1. onCreate()
            - **実装必須**。さらにこのメソッドの中でsetContentView()を定義して、ユーザーインターフェースを実装することも必須。
  1. ユーザインターフェースを実装する
  1. マニフェストでアクティビティを宣言する
    - <activity> 要素を <application> 要素の子要素として追加します。
      - 必須属性
        - android:name
```
<manifest ... >
  <application ... >
      <activity android:name=".ExampleActivity" />
      ...
  </application ... >
  ...
</manifest >
```
- いつ作られる？
  - アプリケーションは画面によって構成される。すなわち、複数のアクティビティによってアプリケーションは構成される。
  -


---

### インテント
[Intent](https://developer.android.com/reference/android/content/Intent.html)

- インテント
  - なにもの？
    - 別のアクティビティを開始する際に情報（データ）を付与して送るオブジェクト。
    ```
    異なるコンポーネント（2 つの activity など）の間に実行時バインディングを
    付与するオブジェクトです。Intent はアプリの「これを行うという意図」を表します。
    intent は幅広いタスクで用いることができる。
    ```
      - 現在のアクティビティから次のアクティビティへの**伝言メモ**のイメージ
      - データは「キー:値」の形式で格納される
    - 構造は？
      - action
      - data
      - category
      - type
      - components
      - extra
        - bundle
  - いつ作られる？
    - 現在のアクティビティから別のアクティビティを開始する時。（インテントを利用することはアクティビィティを開始するための必要条件）
  - これを使うメリットは？
    - 現在のアクティビティから情報を次のアクティビティに引き継げる
    - そのため、アクティビティ間のつながりができる。
  - 関連する記事
    - [インテントとインテントフィルタ](https://developer.android.com/guide/components/intents-filters.html)
  - ポイント
    -　

### バンドル
[バンドル](https://qiita.com/kojionilk/items/138eea19dadb14997136)


### Tool bar
- 一般名称はアプリバー（またはアクションバー）と呼ばれる。
- ToolbarはAppCompatライブラリのToolbarウィジェットのこと。現在ではこれをアプリバーとして使用するのが一般的。
- tool barはAndroid 5.0(Lollipop)以降。それ以前はaction bar。
> A Toolbar is a generalization of action bars for use within application layouts.
>
> a Toolbar may be placed at any arbitrary level of nesting within a view hierarchy.

[アプリバー](https://developer.android.com/training/appbar/)

### Support library
[Support Library](https://developer.android.com/topic/libraries/support-library/)
