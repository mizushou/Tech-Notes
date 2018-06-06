-  やること
    1. ActivityがホストしているFragmentにtoobarを追加
    2. themeを設定
- AppCompatライブラリのtoolbarの要件
    1. dependencyにAppCompatサポートライブラリを追加
    2. AppCompatのthemesを使用する
    3. **ActivityがすべてAppCompatActivityのサブクラス**
- 実装前の注意事項
    - toolbarの追加の仕方は2種類ある
        1. Activityのアプリバーにtoolbarを直接表示する（AndroidDeveloperはこっち）
        2. ActivityがホストしているFragmentにtoolbarを表示する（教科書はこっち）
    - 単語
        - menu
            - toolbarの右上に用意された場所
            - 複数のitmeから構成される（menu itemsと呼ばれることもある）
- 実装の流れ
    1. Add the AppCompat dependency
    2. Update the theme(スタイルシートを当てる)
        - themeを決める。AppCompatライブラリでは以下の3要素からアプリの基本色を決めることができる
            1. Theme.AppCompat
            2. Theme.AppCompat.Light
            3. Theme.AppCompat.Light.DarActionBar
        - 実装の流れ
            1. AndroidManifestに```android:theme``` を追加する
            2. ```res/values/styles.xml``` でthemeを定義する

            ```xml
            <resources>
                        <!-- Base application theme. -->
                        <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
                            <!-- Customize your theme here. -->
                            <item name="colorPrimary">@color/colorPrimary</item>
                            <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
                            <item name="colorAccent">@color/colorAccent</item>
                        </style>
            </resources>
            ```
    3.  ActivityがすべてAppCompatActivityのサブクラスであることを確認
    4. menuのレイアウトファイルを定義する（xml）
        1. resディレクトリでNew -> Android resource file
        2. Resource typeをMenuに変更して、OK
        ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <menu xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto">
            <item
                android:id="@+id/new_crime"
                android:icon="@drawable/ic_menu_add"
                android:title="@string/new_crime"
                app:showAsAction="ifRoom|withText" />
            <item
                android:id="@+id/show_subtitle"
                android:title="@string/show_subtitle"
                app:showAsAction="ifRoom" />
        </menu>
        ```
    5. iconを作成する
        - デフォルトアイコンの使用を避ける
        - Asset Studioを使用
    6.  menuを作成する
        1.
- 参考
    - [Theming with AppCompat](https://medium.com/google-developers/theming-with-appcompat-1a292b754b35)
