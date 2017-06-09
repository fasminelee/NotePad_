# NotePad
This is an AndroidStudio rebuild of google SDK sample NotePad

一: 时间戳

View 层 修改 noteslist_item.xml

```
<TextView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/creatTime"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_margin="5dp"
    android:gravity="right"
    android:textAlignment="textEnd"
    android:textStyle="italic"/>
```

 Controller 层 修改 NotePadProvider.java

![Image](https://github.com/fasminelee/Android_/blob/master/000-Preview/NotePadProvider.java.png)

二: 搜索框🔍 核心代码

```java
String where = NotePad.Notes.COLUMN_NAME_TITLE
  +" LIKE '%"+newText+"%' ";

cursor = managedQuery(
        getIntent().getData(),            // Use the default content URI for the provider.
        PROJECTION,                       // Return the note ID and title for each note.
        where,         					// No where clause, return all records.
        null,                             // No where clause, therefore no where column values.
        NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
);

adapter.changeCursor(cursor);

Toast.makeText(this,"查询-"+keyWord.getText().toString()+"-完成",Toast.LENGTH_SHORT).show();
```






