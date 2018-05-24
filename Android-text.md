

### Chapter 1 Your First Android Application
---
##### String resources
- デフォルトはstrings.xmlという名前だが、res/valuesディレクトリ内にあるのであれば他の名前でもいい。また複数のstringsファイルを作成することもできる。
- res/valuesはresoursesのroot

##### From layout file to View objects
-

#### Resources and resource IDs
- resource IDsはR.javaに記載されている。（見つからない。。。）

- LayoutInflaterクラスでxmlから対応するViewオブジェクトを作成する
- 直接このクラスを使うことはなく、ContextクラスのgetSystemServiceメソッドなどを通して呼び出すみたい
- LayoutInflater
  - https://developer.android.com/reference/android/view/LayoutInflater
