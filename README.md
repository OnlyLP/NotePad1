# NotePad
# 基本要求：所有都完成    附加功能完成有3个：UI，背景的修改，分组功能
#  NoteList中显示条目增加时间戳显示
#  添加笔记查询功能（根据标题查询）
## 如下图  时间在每一行的左下方  本程序用的程序功能还是即使的 ，随输入输查询
时间的代码：
   values.put(NotePad.Notes.COLUMN_NAME_TITLE, title);
            values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE,t);
            values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,t);
        } else if (title != null) {
            // In the values map, sets the value of the title
            values.put(NotePad.Notes.COLUMN_NAME_TITLE, title);

        }

        // This puts the desired notes text into the map.
        values.put(NotePad.Notes.COLUMN_NAME_NOTE, text);
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,t);
  查询的代码：
   private  TextWatcher textWatcher=new TextWatcher() {
        @Override
        public void beforeTextChanged(CharSequence s, int start, int count, int after) {

        }

        @Override
        public void onTextChanged(CharSequence s, int start, int before, int count) {

            Intent intent = getIntent();

            // If there is no data associated with the Intent, sets the data to the default URI, which
            // accesses a list of notes.
            if (intent.getData() == null) {
                intent.setData(NotePad.Notes.CONTENT_URI);
            }

        /*
         * Sets the callback for context menu activation for the ListView. The listener is set
         * to be this Activity. The effect is that context menus are enabled for items in the
         * ListView, and the context menu is handled by a method in NotesList.
         */
            listView.setOnCreateContextMenuListener(NotesList.this);

        /* Performs a managed query. The Activity handles closing and requerying the cursor
         * when needed.
         *
         * Please see the introductory note about performing provider operations on the UI thread.
         */
            cursor = managedQuery(
                    getIntent().getData(),            // Use the default content URI for the provider.
                    PROJECTION,                       // Return the note ID and title for each note.
                    NotePad.Notes.COLUMN_NAME_TITLE+"   "+"like ? and"+ NotePad.Notes.GROUP_NAME+" = ?",                             // No where clause, return all records.
                    new String[]{"%"+editText.getText().toString()+"%",spinner.getSelectedItem().toString()},                             // No where clause, therefore no where column values.
                    null // Use the default sort order.
            );
            aaa();



        }

        @Override
        public void afterTextChanged(Editable s) {

        }
    };

![](https://github.com/OnlyLP/NotePad1/blob/master/image/1.jpg)
![](https://github.com/OnlyLP/NotePad1/blob/master/image/2.jpg)

# 其他功能
## 一.UI设计：对布局，字体从新了设计，使APP看起来美观大方
UI的背景代码：
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"

    android:id="@+id/list12">
    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
       android:background="#00000000"
     >
        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="@drawable/app_notes"
            android:layout_centerVertical="true"/>
        <EditText
            android:layout_width="250dp"
            android:layout_height="50dp"
            android:id="@+id/mainedit"
            android:hint="搜索"
            android:drawableLeft="@drawable/search"
            android:textColor="#000"
            android:layout_gravity="center"
            android:background="@drawable/bg"
            android:layout_marginTop="15dp"
            android:textColorHint="#000"
           android:layout_centerVertical="true"
            android:layout_marginLeft="70dp"
         />





    </RelativeLayout>
    <Spinner
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:entries="@array/group"
        android:id="@+id/spinner1"
        />



   <RelativeLayout
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       >
    <ListView
        android:layout_width="350dp"
        android:layout_height="match_parent"
        android:id="@+id/mainlist"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="15dp">

    </ListView>
<Button
    android:layout_width="35pt"
    android:layout_height="35pt"
    android:id="@+id/addbutton"
    android:layout_alignParentBottom="true"
    android:layout_centerHorizontal="true"
    android:background="@drawable/add"
  />
   </RelativeLayout>

</LinearLayout>

![](https://github.com/OnlyLP/NotePad1/blob/master/image/1.jpg)

## 二.修改背景颜色：可以对背景颜色自定义的修改，下次进来可以保存上次修改的颜色
![](https://github.com/OnlyLP/NotePad1/blob/master/image/3.jpg)

## 三.分组功能：
数据库的代码：
    public void onCreate(SQLiteDatabase db) {
           db.execSQL("CREATE TABLE " + NotePad.Notes.TABLE_NAME + " ("
                   + NotePad.Notes._ID + " INTEGER PRIMARY KEY,"
                   + NotePad.Notes.COLUMN_NAME_TITLE + " TEXT,"
                   + NotePad.Notes.COLUMN_NAME_NOTE + " TEXT,"
                   + NotePad.Notes.COLUMN_NAME_CREATE_DATE + " INTEGER,"
                   + NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE + " INTEGER,"
                   + NotePad.Notes.GROUP_ID + " INTEGER,"
                   + NotePad.Notes.GROUP_NAME + " TEXT"
                   + ");");


       }

###  在编辑页面可以分类：
添加分组的代码：
values.put(NotePad.Notes.GROUP_ID,spinner.getSelectedItemPosition());
 values.put(NotePad.Notes.GROUP_NAME,spinner.getSelectedItem().toString());
![](https://github.com/OnlyLP/NotePad1/blob/master/image/6.jpg)
![](https://github.com/OnlyLP/NotePad1/blob/master/image/7.jpg)

### 在首页可以利用分组功能进行快速查询
代码：spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                if ("全部".equals(spinner.getSelectedItem().toString()))
                    cursor = managedQuery(
                            getIntent().getData(),            // Use the default content URI for the provider.
                            PROJECTION,                       // Return the note ID and title for each note.
                            null,                          // No where clause, return all records.
                            null,                             // No where clause, therefore no where column values.
                            null // Use the default sort order.
                    );
                else
                cursor = managedQuery(
                        getIntent().getData(),            // Use the default content URI for the provider.
                        PROJECTION,                       // Return the note ID and title for each note.
                        NotePad.Notes.GROUP_ID+" = ?",                             // No where clause, return all records.
                        new String[]{position+""},                             // No where clause, therefore no where column values.
                        null // Use the default sort order.
                );
                aaa();
            }
![](https://github.com/OnlyLP/NotePad1/blob/master/image/4.jpg)
![](https://github.com/OnlyLP/NotePad1/blob/master/image/5.jpg)

# 本APP免费

