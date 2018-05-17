### R.idが参照できない
- どんな問題？
  - 新規に追加した、レイアウトのidを参照できなかった
  ```
  ViewGroup layout = (ViewGroup) findViewById(R.id.activity_display_message);
  ```
- 原因は？
  - 単純にidがない。
- 解決策は？
  - idを付与する。ビルド後にidがR.javaに追加される。
  ```
  android:id="@+id/activity_display_message"
  ```
- 検討事項
  - レイアウトが追加されると本来は動的にidが割り振られるはずなのか？少なくとも自分の環境ではidは自動では割り振られない。
  - 過去のtutorialでは上記のようにレイアウトのidを取得しているが、現在のtutorialでは変わっている。あまり推奨される方法ではないのか？
- 参照記事
  - [android error on tutorial cannot find symbol variable activity_display_message](https://stackoverflow.com/questions/38758391/android-error-on-tutorial-cannot-find-symbol-variable-activity-display-message)
