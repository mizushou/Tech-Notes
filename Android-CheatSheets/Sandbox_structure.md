- Appのsandboxのパス
```shell
/data/data/com.example.shouhei.criminaintent
```
    - sandbox内
    ```shell
    # ls -al
    total 28
    drwx------   6 u0_a83 u0_a83       4096 2018-06-06 19:00 .
    drwxrwx--x 105 system system       4096 2018-06-06 18:09 ..
    drwxrws--x   2 u0_a83 u0_a83_cache 4096 2018-06-06 18:09 cache
    drwxrws--x   2 u0_a83 u0_a83_cache 4096 2018-06-06 18:09 code_cache
    drwxrwx--x   2 u0_a83 u0_a83       4096 2018-06-06 18:09 databases  ★db
    drwx------   3 u0_a83 u0_a83       4096 2018-06-06 19:00 files ★File
    ```
        - databases内
        ```shell
        # ls -al
        total 32
        drwxrwx--x 2 u0_a83 u0_a83  4096 2018-06-06 18:09 .
        drwx------ 6 u0_a83 u0_a83  4096 2018-06-06 19:00 ..
        -rw-rw---- 1 u0_a83 u0_a83 20480 2018-06-08 13:59 crimeBase.db  ★SQLiteのdb
        -rw-rw---- 1 u0_a83 u0_a83     0 2018-06-08 13:59 crimeBase.db-journal
        ```
        - files内
        ```shell
        # ls -al
        total 212
        drwx------ 3 u0_a83 u0_a83   4096 2018-06-08 14:56 .
        drwx------ 6 u0_a83 u0_a83   4096 2018-06-06 19:00 ..
        -rwx------ 1 u0_a83 u0_a83 200517 2018-06-08 14:56 IMG_f53a3b0c-0cfa-41d5-9ce4-8702942eba3f.jpg ★camera appが保存した画像
        drwx------ 3 u0_a83 u0_a83   4096 2018-06-06 19:00 instant-run
        ```
