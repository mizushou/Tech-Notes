
create a UI
manage a UI
interact with the model object

CrimeActivity -> CrimeFragment - 1 crime(model)

- ここでは、
    - UIを持つFragmentの実装方法を記載する。（UI Fragmentと呼ぶ）
    - UIを持たないFragmentは、アクティビティのバックグラウンド動作を提供する
- Hosting a UI Fragmentをするために必要なこと
    1. define a spot in its layout for the fragment's view
    2. manage the lifecycle of the fragment instance
- 実装の方針
    - 2種類ある
        1. ActivityのlayoutとしてFragmentを追加する（静的）
            - メリット
                - 直感的（わかりやすい）
            - デメリット
                - inflextible（ActivityのViewに密結合）（動的）
        2. Activityのcode内にFragmentを追加する
            - メリット
                - complex
            - デメリット
                - flexible
    - 参照
        - [フラグメントをアクティビティに追加する](https://developer.android.com/guide/components/fragments#Adding)
- 実装前の注意事項
    - Fragmentクラスは2種類ある
        1.  android.app
            - Fragmentが導入されたAPI 11のnativeのFragment
        2. android.support.v4.app
            - support libraly版のFragment. support libralyは実行環境のAndroidのバージョンより新しいバージョンのAPIを使えるように設計されている。
            - **基本はこっちを使用する**
    -  ここで作成するUI Fragmentは **controller**.
        - interacts with model and virew object.
    - FragmentとActivityではviewのインスタンスを取得するまでの流れ若干違う
        - 何が違う？
            - inflateする場所（タイミング）
                - Activity
                    - **onCreateメソッド** のsetContentView(R.layout.activity_quiz)
                - Flagment
                    - **onCreateViewメソッド** のLayoutInflater.inflate(R.layout.fragment_crime, container, false)
            - inflateを明示的にするかどうか？
                - Activity
                    - **暗黙的** （setContentView内で行われる）
                - Fragment
                    - **明示的** （inflater.inflatemメソッドで明示的に行う）
            - viewインスタンス
                - Activity
                    - Activityインスタンスのインスタンス変数にセットされる？？（たぶん）
                        - なので、findViewById(R.id.true_button);と直接メソッドから取得できる（getter感覚）
                        - 裏では結局View.findViewById(R.id.crime_title)を読んでる便利メソッドらしい
                - Fragment
                    -  inflateメソッドが返すviewインスタンスをローカル変数に代入
                        - なので、View.findViewById(R.id.crime_title)でViewのローカル変数経由で取得
- 実装の流れ
    1. Fragmentと紐づくModelクラスを作成する
        - packageの上で右クリック -> New -> Java Class
    2. 親Activityのレイアウトを定義する（必要なこと#1）
        - Activity hierarchyの中にFragment viewのspotを作成しておく。（ここに子FragmentのViewが表示される）
        - 親Activityはここでは単なるcontainer
    3. UI Fragmentを作成する（Activityの時と同じ流れ）
        1. Fragmentのレイアウトを定義する
        2. Fragmentのクラスを作成し、レイアウトで定義したviewをセットする
            1. Javaクラスを作成し、Fragmentクラスのサブクラスにする
                - packageの上で右クリック -> New -> Java Class
                - **suppport libraryのFragmentをimportする★**
            2.  modelのメンバー変数を用意する
            3. callbackメソッドを実装する
                1. onCreate
                    - Activityの時は違って、**setContentViewは呼ばない**。（つまりここではviewをinflateさせない）
                    ```java
                    @Override
                    public void onCreate(@Nullable Bundle savedInstanceState) {
                        super.onCreate(savedInstanceState);
                        mCrime = new Crime();
                    }
                    ```
                2. onCreateView
                    - **Activityにはないcallback**
                    - Viewインスタンスを返却する（voidじゃない）
                    - ここでLayoutInflater.inflateメソッドを使ってviewを明示的にinflateする。
                    ```java
                    @Nullable
                    @Override
                    public View onCreateView(
                        @NonNull LayoutInflater inflater,
                        @Nullable ViewGroup container,
                        @Nullable Bundle savedInstanceState) {
                        //1st parameter : layout resource ID
                        //2nd parameter : view's parent
                        //3rd parameter : 2nd parameterのparentにこのviewをaddするかどうか
                        View v = inflater.inflate(R.layout.fragment_crime, container, false);
                    ```
                3. onCreateView内でviewインスタンスに接続する
                    - 例えばボタン
                    ```java
                    public View onCreateView(
                            @NonNull LayoutInflater inflater,
                            @Nullable ViewGroup container,
                            @Nullable Bundle savedInstanceState) {
                            View v = inflater.inflate(R.layout.fragment_crime, container, false);

                            mDateButton = v.findViewById(R.id.crime_date);
                            mDateButton.setText(mCrime.getDate().toString());
                            mDateButton.setEnabled(false);

                            return v;
                        }
                    ```
        3. hhhh
