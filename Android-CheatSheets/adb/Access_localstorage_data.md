- やること
    - AVD（エミュレータ）のローカルストレージにアクセス
- 手順
    1. adbを使う方法(直接avdにログイン)
        1. emulatorを起動
            - usbデバッグを一度、off -> onにしないとdebugポート認識されないかも
        1. 以下のコマンドでログイン
            ```shell
            adb shell
            ```
        1. root昇格
            ```shell
            $ su    
            ```
        1. /data配下にローカルデータ（SQLiteのデータ）などがある
    1. Android Device monitrを使う方法
        1. emulatorを起動
        1. ADM起動
            - AS 3.X系？から除外されてるぽいので直接シェル起動
            ```shell
            cd ~/Android/Sdk/toos/
            sh ./monitor
            ```
        1.  おそらくdebugポートが使えにというメッセージが出る
            - エミュレータ側でusbデバッグを一度、off -> on
            - しばらくすると認識される
        1. DDMS perspecitveのFile Exploerタブを選択
            - ただし、/data配下はパーミッションないので、上の方法でパーミッションの変更が必要そう
