###  必要なもの
---
- Java Class file
- XML layout
- AndroidManifest

### 作り方
---
1. packageのフォルダで右クリック。 New -> Activity -> Empty Activity
    - Activityを作成することで、manifestへの登録、 対応するlayoutを用意してくれる。
2. 作成したActivityに以下を用意
    1. TAG
        ```java
        private static final String TAG = "CheatActivity";
        ```
    2. onCreate以外のcallbackメソッド（各メソッド内にLogを入れておく）
        1. onStart
        1. onResume
        1. onPause
        1. onStop
        1. onDestroy
          ```java
          @Override
          protected void onStart() {
            super.onStart();
            Log.d(TAG, "onStart() called");
          }
          @Override
          protected void onResume() {
            super.onResume();
            Log.d(TAG, "onResume() called");
          }
          @Override
          protected void onPause() {
            super.onPause();
            Log.d(TAG, "onPause() called");
          }
          @Override
          protected void onStop() {
            super.onStop();
            Log.d(TAG, "onStop() called");
          }
          @Override
          protected void onDestroy() {
            super.onDestroy();
            Log.d(TAG, "onDestroy() called");
          }
          ```
