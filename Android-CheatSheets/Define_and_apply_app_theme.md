- やること
    - appのメインテーマスタイルを定義(既存のthemeを拡張)し適用する
- 実装する前の前提知識   
    - A theme is a type of style that's **applied to an entire app, activity, or view hierarchy, not just an individual view.** When you apply your style as a theme, every view in the app or activity applies each style attribute that it supports.
    - styleはview一個のデザインを定義するもの
    - themeはアプリ全体に適用されるデザイン。
    - style ∈ theme
    - **parent 属性は親style。これを指定することで既存styleを拡張できる。**
    - support libraryのthemeについて（今回拡張するtheme）
        - 種類
            1. Theme.AppCompa
                - アプリのバックグラウンドが黒、文字が白
            2. Theme.AppCompat.Light
                - アプリのバックグラウンが白、文字が黒
            3. Theme.AppCompat.Light.DarkActionBar
                - アプリのバックグラウンが白 + 色系のアクションバー向けに文字が白抜きになる
        - 今回overrideするattribute
            1. colorPrimary
                - app barの色
            2. colorPrimaryDark
                - ステータスバーの色
            3. colorAccent
                - text fieldとかcheckboxの色など
- 実装の流れ
    1. style.xmlでthemeの定義をする
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
    2. color.xmlでthemeで使用する色を定義
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <resources>
            <color name="colorPrimary">#008577</color>
            <color name="colorPrimaryDark">#00574B</color>
            <color name="colorAccent">#D81B60</color>
        </resources>
    ```
    3. Androidmanifestで指定。（Viewと違ってlayoutでは指定しない）
    ```xml
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"
        ...
    ```
- その他
    - [color tool](https://material.io/tools/color/#!/?view.left=0&view.right=0)
