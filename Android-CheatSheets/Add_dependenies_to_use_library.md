- 目的
    - supportライブラリなどのインポート
- 確認方法
    - **app/build.gradle** のdependeciesを確認する
        - build.gradleは2種類ある。
            1. for the project as a whole
            2. app module ★こっちに追加される
- いつインポートされる？
    - appがcompileされるとき
        - Gradleがdependeniesを見つけて、ダウンロード、インポートする
- dependenciesを追加する方法
    1. Open the **project structure**
        - File -> Project Structure
    2. Select the **Dependencies tab** on the app module
    3. Add the dependencies
        - press [+] button -> choose Libray dependency
            - ex) appcompan-v7
    4. (Support Libraryなどの場合のみ) minSdkVersionを設定する
        - app/build.gradleでminSdkVersion の設定を設定
        - gradleファイルを更新後はsyncする
- 参照（gradleファイルを直接更新する場合）
    - [Support Library のセットアップ](https://developer.android.com/topic/libraries/support-library/setup)
