### レイアウト
- Androidにおいてレイアウトとは？
  - ユーザーインタフェースの視覚的構造が定義されているもの（**UIの設計図**）
- レイアウトの宣言方法は？
  - 2種類ある
    1. XMLでUI要素を宣言する：
      - Android では、ウィジェットやレイアウトなどの View クラスとサブクラスに対応する単純な XML ボキャブラリが提供されます。
      - この方法の優れている所（メリット）
        - 動作を制御するコードとアプリケーションの表示をうまく切り離すことができる点
    1. 実行時にレイアウト要素のインスタンスを作成する：
      - アプリケーションで View オブジェクトと ViewGroup オブジェクトを作成できます。また、プログラムを使ってプロパティを操作することもできます。
      - この方法で優れている所（メリット）
        - j

### ViewとViewGroup(静的画面)
[JavaDrive ビューとビューグループ](https://www.javadrive.jp/android/activity/index4.html)

1. View
  - Viewとはなにか？
    - ボタンやテキストなどのUI部品
    - Viewクラスで定義されている
      - [Viewクラス](https://developer.android.com/reference/android/view/View.html)
  - どうやって使う？
    - setContentViewメソッドを使って、アクティビティにview(ボタンなどのUI単体部品)を配置するイメージ
    - どのタイミングで配置するの？
      - アクティビティが開始された直後に呼び出されるonCreateメソッドの中で、setContentViewメソッドを呼び出して配置する
  - **Widget**
    - 画面に表示されるViewのことをWidgetと呼ぶことがある
1. ViewGroup
  - ViewGroupとはなにか？
    - 一般的に画面には複数のUI部品が配置されるが、setContentViewでは１つのViewしか配置できない。
    - そのため複数のviewをまとめる入れ物であるViewGroupを利用する
  - ViewGroupクラスで定義されている
    - [ViewGroupクラス](https://developer.android.com/reference/android/widget/LinearLayout.html)
  - どうやって使う？
    - ViewGroupクラスはView（UI部品）の配置（レイアウト）方法ごとにサブクラスを持っている。
    - 種類
      1. LinearLayout
        - 縦or横に並べる
      1. RelativeLayout
        - View同士の相対的な位置関係で並べる
      1. WebView
        - Web表示する

### Adapter（動的画面）
- Adapterとはなにか？
  - レイアウトのコンテンツが動的または事前設定されていないとき、実行時にレイアウトを設定できる
- どうやって使う？
- 関係図
  - [データソース] ⇔ [Adapter] ⇔ [AdapterView]
    - [データソース]：DBなどのViewとして表示するデータ
    - [Adapter]：データへのアクセス手段を提供し、取得したデータをViewへ変換する
  - レイアウトの種類は？
    1. ListView
      - スクロール可能な1列のリスト
    2. GridView
      - スクロール可能な列と行のリスト


### XMLでレイアウトを記述する場合
- AndroidのUI要素を宣言するXMLボキャブラリ
  - 原則以下のルール
    - (要素名)⇔(クラス名)
    - (属性名)⇔(メソッド名)
- XMLの記述ルール
  - 各レイアウトファイルには、必ず1つのルート要素が含まれていて、そのルート要素は **View** または **ViewGroup** オブジェクトでなくてはなりません。
  - シンタックス
```xml
<?xml version="1.0" encoding="utf-8"?>
<ViewGroup
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@[+][package:]id/resource_name"
    android:layout_height=["dimension" | "match_parent" | "wrap_content"]
    android:layout_width=["dimension" | "match_parent" | "wrap_content"]
    [ViewGroup-specific attributes] >
    <View
        android:id="@[+][package:]id/resource_name"
        android:layout_height=["dimension" | "match_parent" | "wrap_content"]
        android:layout_width=["dimension" | "match_parent" | "wrap_content"]
        [View-specific attributes] >
        <requestFocus/>
    </View>
    <ViewGroup >
        <View />
    </ViewGroup>
    <include layout="@layout/layout_resource"/>
</ViewGroup>
```
- レイアウトリソース
  - [レイアウトリソース](https://developer.android.com/guide/topics/resources/layout-resource.html)

### Fragment
- [Android はじめてのFragment](https://qiita.com/Reyurnible/items/dffd70144da213e1208b)
- [Fragment](https://developer.android.com/guide/components/fragments.html)
- [複数画面をサポートする](https://developer.android.com/guide/practices/screens_support)

- Fragmentとはどんなものか？
  - **ライフサイクル** を持ったView
  - ViewとActivityの中間のイメージ
  - Activityに配置できる部品としてのActivityのイメージ
  - 静的なActivityに対して、動的なFragmentなイメージ
- Fragmentを使うメリット？
  - Fragmentは複数の画面で使いまわすことができる
  - FragmentはActivityと違って**親子関係**を持てる
  - Fragmentに関する処理は１つの**Transaction**として扱うことができる（Activityのバックスタックが利用される）
- デザインの指針
  - アクティビティのレイアウトをフラグメントに分割する -> 各フラグメント上で動的にUIを変化させることができる
- AndroidDevelperよりFragmentに関する記述抜粋
  - フラグメントはアクティビティの実行中に追加したり削除したりできます（別のアクティビティで再利用できる「サブ アクティビティ」のようなものです）。
  - **フラグメントは常にアクティビティに埋め込まれている必要がある**
  - フラグメントのライフサイクルはホストの**アクティビティのライフサイクルの影響を直接受ける**。
  - フラグメントのトランザクションを実行するとき、アクティビティで管理されているバックスタックにそれを追加することもできます。
  - バックスタックでは、「戻る」を選択するとフラグメントのトランザクションを戻す（前に戻る）ことができます
  - アクティビティのレイアウトの一部としてフラグメントを追加すると、**フラグメントはアクティビティのビュー階層の ViewGroup に置かれ、フラグメントが自身のビュー レイアウトを定義します。**
  - フラグメントをアクティビティのレイアウトに挿入するには、
    1. アクティビティのレイアウト ファイルでフラグメントを <fragment> 要素として定義する
    1. アプリのコードで既存の ViewGroup に追加します。

### Fragmentのライフサイクル
- [Fragmentのライフサイクル](https://developer.android.com/guide/components/fragments#Lifecycle)
- AndroidDevelperよりFragmentのライフサイクルに関する記述抜粋
    - アクティビティとフラグメントのライフサイクルの**最も重要な違いは、バックスタックでのそれぞれの保存方法**です。
    - これ以外については、**フラグメントのライフサイクルの管理は、アクティビティのライフサイクルの管理とほとんど同じです。** そのため、アクティビティのライフサイクルの管理におけるベスト プラクティスがフラグメントにも適用されます。
    - もう 1 つ理解しておくべき事項は、**アクティビティの寿命がどのようにフラグメントの寿命に影響を与えるか**という点
    - 警告: Fragment 内に **Context オブジェクトが必要な場合は、getActivity()** を呼び出すことができます。ただし、getActivity() はフラグメントがアクティビティにアタッチされている場合にのみ呼び出すようにしてください。 フラグメントがまだアタッチされていない場合や、ライフサイクルの終了時にデタッチされた場合は、getActivity() は null を返します。
- 
