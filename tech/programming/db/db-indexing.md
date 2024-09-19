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

### Index Type (PostgreSQL)
- B-Tree (Balanced Tree)
    - Cocok digunakan untuk kolom yang bisa dilakukan untuk operasi `<   <=   =   >=   >`
    - sebisa mungkin B-Tree dipake untuk kolom yang tidak random, semisal (random string, uuid v4). karena semakin susah untuk diurutkan maka database akan sibuk melakukan rebalancing index B-Tree nya.
    - secara default, index di PostgreSQL itu pake B-Tree.
- Hash
    - Cocok digunakan untuk kolom yang hanya perlu operasi `=`
    - DDL hash index `CREATE INDEX idx_name ON table USING HASH (column);`
- GIN (General Inverted Index)
    - Cocok digunakan untuk tipe data yang isinya multiple (JSONB, Array, Hstore)
    - Bisa juga untuk full text search, tapi perlu baca-baca lagi terkait `tsvector` dan `tsquery`
    - Index size GIN ini lebih besar dibanding dengan B-Tree.
    - Update cost pada index juga lebih besar dari index yang lain, jadi kalo datanya sangat sering berubah, operasi updatenya bisa lebih besar dibanding benefit yang didapatkan.
    - DDL gin index `CREATE INDEX idx_name ON table USING GIN (column);`
        
# Referensi
- https://www.postgresql.org/docs/current/indexes-types.html
- Belum dibaca:
    - https://dataschool.com/sql-optimization/how-indexing-works/
