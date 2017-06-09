# NotePad
This is an AndroidStudio rebuild of google SDK sample NotePad

### 一: 时间戳

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

### 二: 模糊查询🔍 核心代码

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



### 三. 排序

前面那个SharedPreferences个里已经定义了sort类型了，就可以直接set进去。 !-- 排序菜单列表-->

添加属性并在OnCreate加载配置：//排序类型 private int sortType; ...

```
//获取排序值
value = params.get("sort");
sortType = Integer.parseInt(value);

```

排序类型先在NotePad中写个契约类吧：

```java
public static final class SortType implements BaseColumns { 
/** 
  * 排序类型契约： 
  * SORT_DEFALUT 默认排序，即修改时间降序 
  * SORT_TITLE_ASC 标题升序 
  * SORT_TITLE_DESC 标题降序 
  * SORT_CREATEDATE_ASC 创建时间升序 
  * SORT_CREATEDATE_DESC 创建时间降序 */ 
	public static final int SORT_DEFALUT=0; 
	public static final int SORT_TITLE_ASC=1; 
	public static final int SORT_TITLE_DESC=2; 
	public static final int SORT_CREATEDATE_ASC=3; 
	public static final int SORT_CREATEDATE_DESC=4; 
} 
```

```java
写个函数返回排序字符串：
  /** 
   * 获取排序类型 
   * @param type 排序类型 
   * @return 返回数据库排序字符串 
   */ 
	private String getSortType(int type) { 
  		if (type == NotePad.SortType.SORT_TITLE_ASC) // 标题升序 
          return NotePad.Notes.COLUMN_NAME_TITLE + " ASC"; 
  		if (type == NotePad.SortType.SORT_TITLE_DESC) // 标题降序 
    		return NotePad.Notes.COLUMN_NAME_TITLE + " DESC"; 
  		if (type == NotePad.SortType.SORT_CREATEDATE_ASC) // 创建时间升序 
          return NotePad.Notes.COLUMN_NAME_CREATE_DATE + " ASC"; 
  		if (type == NotePad.SortType.SORT_CREATEDATE_DESC) // 创建时间降序 
          	return NotePad.Notes.COLUMN_NAME_CREATE_DATE + " DESC"; 
  		else // 默认 
          	return NotePad.Notes.DEFAULT_SORT_ORDER; 
	}
```

