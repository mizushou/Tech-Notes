
1. Tools checkup
    - Device : 以下の要件を満たすことでFirebaseを実行できる
        1. Android Device API level9+(Android 2.3)
            1. Play Service9.0+
            - setting -> app -> GooglePlay servicesでバージョンを確認
    - Android SDK(Android Studio) : 以下の要件を満たすことでFirebase client libraryをビルド可能になる
        1. Google Plya services rev 30+
        2. Google Repository rev 26+
            1. Android Studioを開き、SDK Mangerを開く。
            2. Appearance & Behavoir -> System Settings -> Android SDKを開き、中央のSDK Toolsを開く。
            3. 以下を確認
                1. Google Plya servicesのrevが30+
                2. Google Repositoryのrevが26+
2. Firebase console
    1. [Firebase console](https://firebase.google.com/)
    1. 右上のconsoleをクリック
    2. 青いCREATE NEW PROJECTボタンをクリック
    3. Create a projectポップアップで以下を入力
        1. Project name(plat formで一つにすべき)
        2. Country/region(指定した国の通貨が使用される)
    3. Projectを作成しProjectページに遷移したら、AndroidアプリをFirebaseに追加をクリック。次の情報を入力する
        1. Androidパッケージ名
            1. build.gradle(app)のapplicationIdに記載されているもの（念の為）
        2. デバッグ用の署名証明書（SHA-1 hash of your debug key）
            - 以下の作成手順で表示されるSHA-1署名を使用
            - keytoolコマンドを使用
            ```
            keytool - 暗号化鍵、X.509証明書チェーンおよび信頼できる証明書を含むキーストア(データベース)を管理します。
            ```

            ```
            -exportcert

               {-alias alias} {-file cert_file} {-storetype storetype} {-keystore keystore}

               [-storepass storepass] {-providerName provider_name}

               {-providerClass provider_class_name {-providerArg provider_arg}}

               {-rfc} {-v} {-protected} {-Jjavaoption}
           aliasに関連付けられた証明書をキーストアから読み込み、ファイルcert_fileに格納します。ファイルが指定されていない場合は、stdoutに証明書が出力されます。
           デフォルトでは、証明書はバイナリ符号化で出力されます。-rfcオプションが指定されている場合、出力可能符号化方式の出力はインターネットRFC 1421証明書符号化規格で定義
           されます。
           aliasが、信頼できる証明書を参照している場合は、該当する証明書が出力されます。それ以外の場合、aliasは、関連付けられた証明書チェーンを持つ鍵エントリを参照しま
           す。この場合は、チェーン内の最初の証明書が返されます。この証明書は、aliasによって表されるエンティティの公開鍵を認証する証明書です。
           このコマンドは、以前のリリースでは-exportという名前でした。このリリースでは、引き続き古い名前がサポートされています。今後は、新しい名前-exportcertが優先されま
           す。
            ```
            ```
            -list
         {-alias alias} {-storetype storetype} {-keystore keystore} [-storepass storepass]
         {-providerName provider_name}
         {-providerClass provider_class_name {-providerArg provider_arg}}
         {-v | -rfc} {-protected} {-Jjavaoption}
         aliasで特定されるキーストア・エントリの内容をstdoutに出力します。aliasが指定されていない場合は、キーストア全体の内容が表示されます。
         このコマンドは、デフォルトでは証明書のSHA1フィンガープリントを表示します。 -vオプションが指定されている場合は、所有者、発行者、シリアル番号、拡張機能などの付加的
         な情報とともに、人間が読むことのできる形式で証明書が表示されます。-rfcオプションが指定されている場合は、出力可能符号化方式で証明書の内容が出力されます。出力可能
         符号化方式は、インターネットRFC 1421証明書符号化規格で定義されています。
         -vオプションと-rfcオプションを同時に指定することはできません。
            ```
            - 作成手順
                1. 既存のdebug.keystore(パスワードはandroid)から電子署名作成
                ```shell
                $ keytool -exportcert -list -v -alias androiddebugkey -keystore ~/.android/debug.keystore
                キーストアのパスワードを入力してください:android
                別名: androiddebugkey
                作成日: 2018/05/15
                エントリ・タイプ: PrivateKeyEntry
                証明書チェーンの長さ: 1
                証明書[1]:
                所有者: C=US, O=Android, CN=Android Debug
                発行者: C=US, O=Android, CN=Android Debug
                シリアル番号: 1
                有効期間の開始日: Tue May 15 14:05:38 PDT 2018 終了日: Thu May 07 14:05:38 PDT 2048
                証明書のフィンガプリント:
                	 MD5:  FC:1A:6D:25:32:16:7C:1A:2E:CD:3C:18:7F:E6:69:66
                	 SHA1: 83:42:27:8B:04:DC:D5:2B:11:38:21:D0:22:0D:32:BC:0A:CB:82:58  ★これ
                	 SHA256: 61:93:31:F0:65:ED:E2:B4:F9:D3:83:25:F0:46:8F:61:C6:21:21:72:92:26:7D:96:25:D0:E2:78:20:35:35:10
                署名アルゴリズム名: SHA1withRSA
                サブジェクト公開鍵アルゴリズム: 1024ビットRSA鍵
                バージョン: 1
                ```
    4. google-services.jsonをダウンロード
    5. google-services.jsonをアプリのappフォルダに配置する
    ```shell
    $ mv google-services.json ~/StudioProjects/MLkitDemo/app/
    ```
3. Project modification
    - Firebase client libraryをインポート・ビルドするためにgradle fileを更新
        1. build.gradle(module)
            - リポジトリの追加
            - パス : <Project>/build.gradle  (ルートにある)
            ```
            dependencies {
                ...
                classpath 'com.google.gms:google-services:4.0.0'
                ...
            }
            ```
        2. build.gradle(app)
            - pluginの追加
            - パス : <Project>/app/build.gradle (app配下にある)
                1. 最終行に追加
                    ```
                    apply plugin: 'com.google.gms.google-services'
                    ```
                2. dependenciesに使用するpluginを追加。ひとまずcore(analtics)を追加
                    ```
                    dependencies {
                        ....
                        // Firebase dependencies
                        implementation 'com.google.firebase:firebase-core:16.0.1'
                    }
                    ```
4. 確認
    - appを起動し、logcatで確認
        - auth, crashは最初ビビったが、そもそもauthもcrashもpluginを追加していないしdebugレベルだしひとまずスルー
        - ```FirebaseApp initialization successful```が出ていることを確認してひとまず完了
        ```
        2018-06-14 13:16:33.942 4655-4655/com.example.shouhei.mlkitdemo D/FirebaseApp: com.google.firebase.auth.FirebaseAuth is not linked. Skipping initialization.
        2018-06-14 13:16:33.946 4655-4655/com.example.shouhei.mlkitdemo D/FirebaseApp: com.google.firebase.crash.FirebaseCrash is not linked. Skipping initialization.
        2018-06-14 13:16:33.947 4655-4655/com.example.shouhei.mlkitdemo I/FirebaseInitProvider: FirebaseApp initialization successful
        ```
