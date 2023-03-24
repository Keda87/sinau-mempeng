# DB Indexing

### Composite Index
- composite index istilah yang dipakai untuk multi column index.
    - cocok dipake kalo ada kondisi filter dengan beberapa kondisi.
    - urutan kolom sangat berpengaruh, jadi index(a, b, c) berbeda dengan index(c, b, a).
    - contoh praktis penggunaan composite index, dari table berikut.

        |   id    |    author_id    |    class_id    |   rank   |
        |---------|-----------------|----------------|----------|
        |   1     |        6        |      4         |     A    |
        |   2     |        3        |      4         |     C    |
        |   3     |        1        |      1         |     A    |
        |   4     |        8        |      2         |     B    |

        Dengan komposit index: `index(author_id, class_id, rank)`

    - query yang mendapatkan benefit dari composit index dengan kondisi berikut:
        - `WHERE author_id = ... AND class_id = ... AND rank = ...`
        - `WHERE author_id = ... AND class_id = ...`
        - `WHERE author_id = ... AND rank = ...`
        - `WHERE author_id = ...`

    - query yang gak berdampak meskipun sudah ada composite index:
        - `WHERE rank = ...`
        - `WHERE class_id = ... AND rank = ...`
        - `WHERE class_id = ...`
        - `WHERE rank = ... AND class_id = ... AND author_id = ... // (dibalik)`

    - dalam men-define composite index, selain urutan kolom sangat penting, kardinalitas juga sangat berpengaruh.
        - kardinalitas itu keunikan data dalam table.
        - urutan kolom dimulai dengan kolom dengan kardinalitas paling tinggi dilanjutkan sampe ke yang rendah.
        - dari contoh table diatas, anggap aja:
            - class     : 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
            - rank      : A, B, C, D, E
            - author    : 1, 2 ... terus bertambah seiring waktu

        - dari kolom diatas, kardinalitas paling tinggi ada di kolom author dan kardinalitas terendah ada di rank (karena cuman 5), sehingga urutan composite indexnya (author_id, class_id, rank).
        
# Referensi
- Belum dibaca:
    - https://dataschool.com/sql-optimization/how-indexing-works/
