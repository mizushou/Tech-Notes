- やること
    - ORM
    - schema定義
    - SQLiteでDB・Table作成(create)
    - recordを作成・更新(insert)
    - recordを参照(select)
- 重要なこと
    - DB(SQLite)を使用するということは...
        1. modelをListのインスタンス変数での管理からDB管理になる
            - メモリにmodelを保存しない
            - 例では、```CrimeLab```のインスタンス変数```mCrimes```を削除
        2. したがって、modelの追加、参照はDBがリソースとなる
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
        - record取得では```Cursole```クラスを活用する
    - **SQLiteDatabase（DB）をインスタンス変数として持つのは、modelを保持、操作するクラス（Singleton）*
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
    3. recordを作成
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
    4. record更新
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
    5. record参照
- その他
