

### Chapter 1 Your First Android Application
---
##### String resources
- デフォルトはstrings.xmlという名前だが、res/valuesディレクトリ内にあるのであれば他の名前でもいい。また複数のstringsファイルを作成することもできる。
- res/valuesはresoursesのroot

##### From layout file to View objects
-

##### Resources and resource IDs
- resource IDsはR.javaに記載されている。（見つからない。。。）

- LayoutInflaterクラスでxmlから対応するViewオブジェクトを作成する
- 直接このクラスを使うことはなく、ContextクラスのgetSystemServiceメソッドなどを通して呼び出すみたい
- LayoutInflater
  - https://developer.android.com/reference/android/view/LayoutInflater

### Chaper 4 Your Second Activity
---
#####   Setting Up a Second Activity

- toos, tools:text
  - This namespace allows you to override any attributo on awidget for the purpose of displaying it differently in the Andrdoid Studio preview. Because TextView has a text attribute, you can provide a literal dummy value for it to help you know what it will look like a runtime.
  - プレビューで値がどのように表示するかを確認できる
- activity_cheet.xmlは今回は状態を保存しないので、landscape alternativeを作成しない。

##### Declaring activities in the manifest

- The manifest is an XML file containing metadata that describes your application to the Andrdoird OS.
- AndroidManifest is configration file.
-

### Chapter

- intentのExtrasについて
  - Extras are arbitrary data that the calling activitiy can include with an intent. You can think of them like constructor arguments, even though you cannnot use a custom constructor with an activity subclass.
