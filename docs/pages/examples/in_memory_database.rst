.. _example-connect-sqlite-db-mem:

Make an in-memory database
--------------------------
:py:func:`~simplesqlite.connect_memdb` function can create a SQLite database in memory.
This function return |SimpleSQLite| instance,
the instance can execute methods as well as a |SimpleSQLite| instance opened with write mode.

:Sample Code:
    .. code-block:: python
        :caption: Make an in-memory database and create a table in the database

        import json

        import simplesqlite


        table_name = "sample_table"
        con = simplesqlite.connect_memdb()

        # create table -----
        data_matrix = [
            [1, 1.1, "aaa", 1,   1],
            [2, 2.2, "bbb", 2.2, 2.2],
            [3, 3.3, "ccc", 3,   "ccc"],
        ]
        con.create_table_from_data_matrix(
            table_name,
            attr_name_list=["attr_a", "attr_b", "attr_c", "attr_d", "attr_e"],
            data_matrix=data_matrix)

        # display values in the table -----
        print(con.fetch_attr_name_list(table_name))
        result = con.select(select="*", table_name=table_name)
        for record in result.fetchall():
            print(record)

        # display data type for each column in the table -----
        print(json.dumps(con.fetch_attr_type(table_name), indent=4))


:Output:
    .. code-block:: none

        ['attr_a', 'attr_b', 'attr_c', 'attr_d', 'attr_e']
        (1, 1.1, u'aaa', 1.0, u'1')
        (2, 2.2, u'bbb', 2.2, u'2.2')
        (3, 3.3, u'ccc', 3.0, u'ccc')
        {
            "attr_b": " REAL",
            "attr_c": " TEXT",
            "attr_a": " INTEGER",
            "attr_d": " REAL",
            "attr_e": " TEXT"
        }
