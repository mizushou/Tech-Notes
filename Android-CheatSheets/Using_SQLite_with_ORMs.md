- やること
    - ORM
    - schema定義
    - SQLiteでDB・Table作成(create)
    - recordを作成・更新(insert)
    - recordを参照(select)
- 重要なこと
    - DB(SQLite)を使用するということは...
        1. modelの管理がListのインスタンス変数からDBになる
            - メモリにmodelを保存しない
            - 例では、```CrimeLab```のインスタンス変数```mCrimes```を削除
        2. したがって、modelの追加、参照はDBがリソースとなる
        3. 大きな変更点
            - 今まではCrimeLabがデータの元本であるmCrime@CrimeLab直接保持していた。
                - mCrims(元本) -> crimeインスタンス
            - 今度からはDBが元本。mCrime@CrimeAdapterはある時点でのスナップショットでしかない。
            - そのため、DBが更新された場合はスナップショットの同期が必要
                - DB(元本) -> List<Crime>(スナップショット) -> crimeインスタンス
- 実装前の前提知識
    - 例で作成するDB・sheme
        - DB名
            - ```crimeBase.db```
        - Table名
            - ```crimes```
        - column名 ：
            1. ```uuid```
            2. ```title```
            3. ```data```
            4. ```solved```
    - 使うクラス・メソッド
        - SQLiteDatabase
            1. openCreateDatabase()
            2. databaseList()
            3. public long insert (String table, String nullColumnHack, ContentValues values)
                - 引数
                    1. table : テーブル名
                    2. nullColumnHack : nullableにするColumn名。基本はnull
                    3. values : insertするデータ（modelからContentValueに変換したもの）
                - 戻り値
                    - 作成したrecordの_id。失敗した場合は-1
            4. public int update (String table, ContentValues values, String whereClause, String[] whereArgs)
                 - 引数
                    1. table : テーブル名
                    2. values : insertするデータ（modelからContentValueに変換したもの）
                    3. whereClause : where句. ?はplaceholder.次の引数の値に置き換わる
                        ```
                        CrimeTable.Cols.UUID + " = ?"
                        ```
                    4. whereArgs :
                        - where句で指定するcolumn名。String配列で指定
                 - 戻り値
                    - 作成したrecordの_id。失敗した場合は-1
                 - ?を使ってベタでsql書かないのはSQL injection対策らしい
            5. public Cursor query (String table, String[] columns, String selection, String[] selectionArgs, String groupBy, String having, String orderBy, String limit)
                - まさにselect文を実行するメソッド
                - 引数
                    1. table : テーブル名
                    2. values : insertするデータ（modelからContentValueに変換したもの）
                    3. whereClause : where句. ?はplaceholder.次の引数の値に置き換わる
                        ```
                        CrimeTable.Cols.UUID + " = ?"
                        ```
                    4. whereArgs :
                        - where句で指定するcolumn名。String配列で指定
                - 戻り値
                    - jccc
        - **SQLiteOpenHelper**★
            - **DB作成・更新** をこの抽象クラスを継承して実現する（以下のDBを作成するときの流れをやってくれる）
            - 使うメソッド
                1. onCreate
                    - この中にsqlのcreateを書く
                2. onUpgrade
                3. getWritableDatabase
                    - 実際に使うのはこのメソッド
                    - 適宜、onCreate, onUpgradeを内部で呼んでくれる
                        1. .dbファイルがなければonCreateでDB・Table作成
                        2. .dbがあるのであれば、DBのversionとSQLiteOpenHelperのサブクラスのversion変数を比較。DBのversionが低ければonUpgradeでshchema更新
        - ContentValues
            > This class is used to store a set of values that the ContentResolver can process.

            - KV-Store
            - HashMapみたいなもの
            - KeyがColumns, Valueが値
            - **model -> ContentValueに変換してinsertを実行**★
        - Cursorインタフェース
            - record取得では```Cursor```クラスを活用する
            - Cursorクラスはselect文の返却を表すクラス（dataset）
            - **SQLiteDatabaseのqueryメソッドでは実際はSQLiteCursorが入っている**
                - SQLiteCursor
                    - moveToFirst()
                    - isAfterLast()
                    - close()
    - **SQLiteDatabase（DB）をインスタンス変数として持つのは、modelを保持、操作するクラス（Singleton）**
        - 例では、```CrimeLab```
        - ```SQLiteOpenHelper```の実装クラスのメソッドを呼び出してDBをopen（作成）する。
    - DBを作成するときの一般的な流れ
        1. DBがすでに存在するかチェック
        2. DBがなければ、DB, Tableを作成。必要であればデータを入れる
        3. DBがあるならばSchemaのバージョン確認
        4. 古ければupdate
- 実装の流れ
    1. schema定義体作成
        1. schema定義（これはただの設計としての定義）
        2. 1.で決めたschema定義に対して、schema情報を保持するクラスを作成する
            1. ```database``` packageを作成する。
            2. そのpackageの中にschema情報を保持するクラスを作成する
                - 保持するschema情報
                    1. table名
                    2. column名
                - 例では```CrimeDbSchema```
                - inner classで持つことで、ドット記法が使える（Java-safe way）
                    - ```CrimeTable.Cols.TITLE```
    2. DB・Table作成
        1. ```SQLiteOpenHelper``` のサブクラスを作成する
            - 例では```CrimeBaseHelper```
            - constructor用意
            - onCreate, onUpgradeをoverrideする
                1. onCreate
                    1. sqlのcreate文をexeSQL()にパラメータとして渡す
                2. onUpgrade
        2. modelを保持するクラスに、インスタンス変数でSQLiteDatabase（DB）を用意
            - 例では```CrimeLab```
        3. getWritableDatabase()でSQLiteDatabase（DB）インスタンスを取得
    3. record作成(create)
        1. modelをContentValuesへ変換するメソッドを用意する
            - _idは自動採番なのでセットする必要はない
            ```java
            private static ContentValues getContentValues(Crime crime) {
                ContentValues values = new ContentValues();
                values.put(CrimeTable.Cols.UUID, crime.getId().toString());
                values.put(CrimeTable.Cols.TITLE, crime.getTitle());
                values.put(CrimeTable.Cols.DATE, crime.getDate().toString());
                values.put(CrimeTable.Cols.SOLVED, crime.isSolved() ? 1 : 0);
                values.put(CrimeTable.Cols.SUSPECT, crime.getSuspect());

                return values;
            }
            ```
        2. insert実行
            - modelから変換したContentValuesを渡して、insert実行
            ```java
            public void addCrime(Crime c) {

                ContentValues values = getContentValues(c);

                mDatabase.insert(CrimeTable.NAME, null, values);
            }
            ```
    4. record更新(update)
        1. modelをContentValuesへ変換するメソッドを用意する(insertする際に用意していると思うが)
        2. insert実行
            - modelから変換したContentValuesとWhere句を指定して、update実行
            ```java
            public void updateCrime(Crime crime) {

              String uuidString = crime.getId().toString();
              ContentValues values = getContentValues(crime);

              mDatabase.update(
                  CrimeTable.NAME, values, CrimeTable.Cols.UUID + " = ?", new String[] {uuidString});
            }
            ```
    5. record参照（全探索） (select)
        - ここでの全探索は、DBからselectで全探索して、その各レコードをmodelインスタンスに変換すること
            1. SQLiteDatabase.queryメソッドをカスタマイズしておく(select文実行メソッド)
                - 特定のテーブル専用のメソッドを用意する
                - 実装する場所はmodelを管理するクラス
                    - 例だと、CrimeLab
                - **戻り値は、CursorのWrapperクラス（生のCrusorを毎回整形するのが面倒だから）**
                    -  例だと、CrimeCursorWrapper（次で作る）

                ```java
                private CrimeCursorWrapper queryCrimes(String whereClause, String[] whereArgs) {
                  Cursor cursor =
                      mDatabase.query(
                          CrimeTable.NAME,
                          null, // columns - null selects all columns
                          whereClause,
                          whereArgs,
                          null, // groupBy
                          null, // having
                          null // orederBy
                          );

                  return new CrimeCursorWrapper(cursor);
                }
                ```

                ```
            1. CursorWrapperクラスを拡張して、select結果（生データ）をmodelに変換するメソッドを持つクラスを作成する
                - このクラスを噛ませる理由は、CrusorをCursorWrapperに食わせてmodelに変換するという処理を毎回やるのを防ぐため
                - getCrime()がselect結果からcrimeインスタンスに変換する便利メソッド
                ```java
                public class CrimeCursorWrapper extends CursorWrapper {

                      public CrimeCursorWrapper(Cursor cursor) {

                        super(cursor);
                      }

                      public Crime getCrime() {

                        String uuidString = getString(getColumnIndex(CrimeTable.Cols.UUID));
                        String title = getString(getColumnIndex(CrimeTable.Cols.TITLE));
                        String date = getString(getColumnIndex(CrimeTable.Cols.DATE));
                        int isSolved = getInt(getColumnIndex(CrimeTable.Cols.SOLVED));
                        String suspect = getString(getColumnIndex(CrimeTable.Cols.SUSPECT));

                        Crime crime = new Crime(UUID.fromString(uuidString));
                        crime.setTitle(title);
                        crime.setDate(LocalDate.parse(date));
                        crime.setSolved(isSolved != 0);
                        crime.setSuspect(suspect);

                        return crime;
                    }
                }
                ```
            1. 実際にqueryを実行してmodelに変換するメソッドを実装する
                - queryメソッドが返すCursor（実際SQLiteCursor）は、レコード位置をポイントするポインターのようなものを持っている
                    1. moveToFirstメソッドでレコード位置を一番最初にする
                    2. レコードを一つずつmodelにしていく
                        - moveToNextメソッドで次にレコードへ移動する
                        - isAfterLastメソッドがFalseの時、ポインターがデータセットの最後まで来たことになる
                    3. 必ずCursorはcloseメソッドで閉じる
                        - そうしないと無駄なログが吐き出され続ける

                    ```java
                    public List<Crime> getCrimes() {

                          List<Crime> crimes = new ArrayList<>();
                          // Full search from crime table
                          CrimeCursorWrapper cursor = queryCrimes(null, null);

                          try {
                            cursor.moveToFirst();
                            while (!cursor.isAfterLast()) {
                              // convert from row to crime instance and add List
                              crimes.add(cursor.getCrime());
                              cursor.moveToNext();
                            }
                          } finally {
                            cursor.close();
                          }
                          return crimes;
                    }
                    ```
    5. record参照（uuid指定）  (select)
        1. 4.の全探索の1.2.と同じ
        2. 4の1.のメソッドでwhere句を指定し、queryを実行しmodelに変換
            - 今回はuuid指定なので返ってくるレコードは一つ。レコードの最初の位置だけを取得
            ```java
            public Crime getCrime(UUID id) {

                  CrimeCursorWrapper cursor =
                      queryCrimes(CrimeTable.Cols.UUID + " =?", new String[] {id.toString()});

                  try {
                    if (cursor.getCount() == 0) {
                      return null;
                    }

                    cursor.moveToFirst();
                    return cursor.getCrime();
                  } finally {
                    cursor.close();
                  }
          }
            ```
    6. sanpshotの同期
        1. mCrimes@CrimeAdapterを最新に同期する処理を追加する
        ```java
        public void updateUI() {
              CrimeLab crimeLab = CrimeLab.get(getActivity());
              // get latest data set.
              List<Crime> crimes = crimeLab.getCrimes();  ★

              if (mAdapter == null) {
                mAdapter = new CrimeAdapter(crimes);
                mCrimeRecyclerView.setAdapter(mAdapter);
              } else {
                // snapshot(mCrimes@CrimeAdapter) is synchronized with latest data set.
                mAdapter.setCrimes(crimes);  ★
                mAdapter.notifyDataSetChanged();
              }
              updateSubtitle();
        }
        ```

- その他
    - ポイント
        1. select（参照系） : AndroidではqueryメソッドはCrusorいで返される
            - Cursor -> modelに変換
        2. insert, update, delete(更新系)
            - model -> ContentValues
