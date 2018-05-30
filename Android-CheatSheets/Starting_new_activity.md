##### 1. Activity起動（データ受け渡しなし）
---
- どういうパターン？
    - 別のActivityを起動する。
- 実装の流れ
    1. 起動する側のActivity内でintentを作る
    1. 起動する側のActivity内でstartActivityメソッドを呼ぶ
- 起動に使うメソッド
    ```
    // OS（ActiviyManager）に起動依頼を送る
    public void statActivity(Intent intent)
    ```
- 使うintentの情報
    1. component
- 使うintentのconstructor
    - Contextは起動するActivityが属するパッケージを指定
    - Classは起動したいActivityを指定
    - この2つの引数でcomponentを特定する
    ```    
    public Intent(Context packageContext, Class<?> cls)
    ```
- explicit intent? inplicit intent?
    - explicit
- reference code
  - [QuizActivity.java](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/QuizActivity.java#L131-L142)

##### 2. Activity起動(データ送信 一方向 呼び出し側 -> 呼び出される側)
---
- どういうパターン？
    - 別のActivityを起動する。
    - その際に、起動側のActivityの任意の情報をextraを使って起動される側のActivityに渡す。
- 実装の流れ
    1. 起動される側のActivityでextraのkeyを定数として定義
    1. 起動される側のActivityでintentを作成し、extraをputする -> newIntentメソッド作成★
        - 実際にextraを要求し、使う側がintentにextraをputする
    1. 起動側のActivity内でnewIntentメソッドでintentを作る
    1. 起動側のActivity内でstartActivityメソッドを呼ぶ
    1. 起動される側で、intentからextraを取得し、メンバー変数として保存する
- 起動に 使うメソッド
    ```
    // OS（ActiviyManager）に起動依頼を送る
    public void statActivity(Intent intent)
    ```
- 使うintentの情報
    1. component
    1. extra
        - constructorの引数のようなもの
- 使うintentのconstructor
    - Contextは起動するActivityが属するパッケージを指定
    - Classは起動したいActivityを指定
    - この2つの引数で特定のcomponentを特定する
    ```
    public Intent(Context packageContext, Class<?> cls)
    ```
- extraをputするために使うのメソッド
    - 一個目のパラメーターはkey
    - 二個目のパラメーターは値
    ```
    public Intent putExtra (String name, ? value)
    ```
- Intentを取得するためのメソッド
    - このメソッドが返すintentは起動側のActivityが渡したintent。つまり、statActivity(Intent)に渡したIntent.
    ```
    Activiy.getIntent()
    ```
- extraを取得するために使うメソッド
    - 一個目の引数はkey
    - 二個目の引数はkeyがない場合の値
    ```
    public bookean getBooleanExtra(String name, boolean defalutValue)
    ```
- explicit intent? inplicit intent?
    - explicit
- reference code
    - [QuizActivity.java](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/QuizActivity.java#L131-L142)
    - [newIntent@CheatActivity.java ](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/CheatActivity.java#L21-L25)
    - [onCreate@CheatActivity.java](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/CheatActivity.java#L21-L25)


##### 3. Activity起動(データ送信 双方向 呼び出し側 <-> 呼び出される側)
---
- どういうパターン？
    - 別のActivityを起動する。
    - その際に、起動側のActivityの任意の情報をextraを使って起動される側のActivityに渡す。
    - さらに起動された側のActivityがdesroyする時にresult codeと任意の情報を起動側のActivityに渡す。
- 実装の流れ
    1. 起動される側のActivityでextraのkeyを定数として定義
    1. 起動される側のActivityでintentを作成し、extraをputする -> newIntentメソッド作成★
        - 実際にextraを要求し、使う側がintentにextraをputする
    1. 起動側のActivity内でnewIntentメソッドでintentを作る
    1. 起動側のActivityでrequestCodeを定数で定義する
    1. 起動側のActivity内でstartActivityForResultメソッドを呼ぶ
    1. 起動される側でresutlにセットする情報のkeyを定数として定義 （必須ではない）
    1. 起動される側でresult codeと渡したい情報(intent)をsetResultメソッドでセットする（必須ではない）
    1. 起動側で受け取ったextraを必要な情報を抽出する（デコード）メソッドを用意する。
    1. 起動側でonActivityResultメソッドをoverrideする。 [handling result]
        - backボタンが押された際に、ActiviyManagerはsetResultの情報（requestCode, resultCode, Intent）から起動側のonActivityResultを実行する
- 起動に使うメソッド
    - 二個目の引数のrequestCodeは **リクエストの識別子** .callbackがあった際にどのリクエストからのcallbackかを確認するために必要。
    - requestCodeはユーザーが任意で定義する整数値
    ```
    public void statActivityForResult(Intent intent, int requestCode)
    ```
- 使うintentの情報
    1. component
    1. extra
        - constructorの引数のようなもの
- 使うintentのconstructor
    - Contextは起動するActivityが属するパッケージを指定
    - Classは起動したいActivityを指定
    - この2つの引数で特定のcomponentを特定する
    ```
    public Intent(Context packageContext, Class<?> cls)
    ```
- extraをputするために使うためのメソッド
    - 一個目のパラメーターはkey
    - 二個目のパラメーターは値
    ```
    public Intent putExtra (String name, ? value)
    ```
- Intentを取得するためのメソッド
    - このメソッドが返すintentは起動側のActivityが渡したintent。つまり、statActivity(Intent)に渡したIntent.
    ```
    Activiy.getIntent()
    ```
- extraを取得するために使うメソッド
    - 一個目の引数はkey
    - 二個目の引数はkeyがない場合の値
    ```
    public bookean getBooleanExtra(String name, boolean defalutValue)
    ```
- result codeをセットするためのメソッド（注：request codeではない）
    - result codeはpredeinedされている次の２つの値を使うのが一般的。
        1. Activiy.RESULT_OK
        1. Activiy.RESULT_CANCELED
    - Activiy.RESRST_USERもある
    - result codeで起動側のcallbackメソッドの振る舞いを結果に応じて切り替えることができる
    - result codeをセットするのは必須ではない。セットしない場合は、デフォルトの値を返す。
    ```
    public final void setResult(int resultCode)
    purlic final void setResult(int resultCode, Intet data) // result codeといっしょにデータ（intent）を返す
    ```
- resultをhandlingするメソッド
    - このメソッドをoverrideする。resultを受け取った際の処理をここに書く。
    ```
    protected void onActivityResult(int requestCode, int resultCode, Intent data)
    ```
- explicit intent? inplicit intent?
    - explicit
- reference code
    - [QuizActivity.java](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/QuizActivity.java#L131-L142)
    - [newIntent@CheatActivity.java ](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/CheatActivity.java#L21-L25)
    - [onCreate@CheatActivity.java](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/CheatActivity.java#L21-L25)
    - [onActivityResult@QuizActivity.java](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/QuizActivity.java#L158-L170)
    - [setAnswerShownResult@CheatActivity.java](https://github.com/mizushou/GeoQuiz/blob/86b79e4063c5737f42432ac6353b125e25fe3b0d/app/src/main/java/com/example/shouhei/geoquiz/CheatActivity.java#L55-L59)
