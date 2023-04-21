## 1. 相对布局

![相对布局](../MDImg/Android Studio/1.1 相对布局.png)

![相对布局](../MDImg/Android Studio/1.12 相对布局-父控件.png)

## 2. 下拉选项spinner

```java
package com.example.myapplication;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Spinner;
import android.widget.Toast;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivitySpinner extends AppCompatActivity{
    Spinner spinner;
    String[] major;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.active_spinner);
        spinner = findViewById(R.id.spinner);

        //准备数据
        major = new String[]{"软件技术", "通信技术", "计算机应用技术", "大数据技术"};
        /**
         * 创建适配器
         * @Param context:上下文对象 MainActivity对象
         * @Param :布局
         */
        ArrayAdapter adapter = new ArrayAdapter(MainActivitySpinner.this, android.R.layout.simple_expandable_list_item_1,major);
        //设置到Spinner
        spinner.setAdapter(adapter);

        // Spinner的事件处理
        spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            //参数position是选项的位置，与数组中的位置是一致的
            @Override
            public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
                Toast.makeText(MainActivitySpinner.this, "你选中的是"+ major[position], Toast.LENGTH_LONG).show();
            }

            @Override
            public void onNothingSelected(AdapterView<?> parent) {
            }
        });
    }
}
```

## 3. 跳转页面

### 3.1 startActivityForResult跳转页面

第一个页面

```java
Intent intent1=new Intent();
intent1.setAction("android.intent.action.MessageStatus");
intent1.putExtra("text1", text1);
//TODO Ⅰ startActivityForResult跳转页面
//TODO Ⅰ.1 startActivityForResult跳转页面
/**
  * @Parent Intent intent
  * @Parent RequestCode 请求码 用于区分到底是哪一个页面带回的请求数据
*/
startActivityForResult(intent1,1);//跳转到页面
```

第二个页面
```java
 @Override
    public void onClick(View view) {
        Intent intent1 = new Intent();
        Bundle bundle = new Bundle();
        switch (view.getId()) {
            case R.id.tv_status1:
                bundle.putInt("image", R.drawable.freetime);
                bundle.putString("text", "空闲中");
                break;
            case R.id.tv_status2:
                bundle.putInt("image", R.drawable.ic_fishing);
                bundle.putString("text", "摸鱼中");
                break;
        }
        intent1.putExtras(bundle);
        //TODO Ⅰ.2 setResult回去上一个页面，返回数据
        /**
         * @Param ResultCode: 结果码
         * @Param Intent: 带数据回去
         */
        setResult(RESULT_OK, intent1);//回去上一个页面
        finish();//让当前页面关闭，此页面出栈进入销毁状态
    }
```

## 4. 生命周期方法及调用

![生命周期](../MDImg/Android Studio/4.1生命周期.png)

## 5. ListView控件及数据初始化

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
<!--    listview应用-->
    <ListView
        android:id="@+id/listview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
    />
</LinearLayout>
```

```java
package com.example.studypower;

import android.os.Bundle;
import android.view.View;
import android.widget.AbsListView;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.SimpleAdapter;
import android.widget.Toast;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import java.util.ArrayList;
import java.util.HashMap;

public class MainActivityScore extends AppCompatActivity {
    ListView listView;
    String[] major;
    private ArrayList datas;
    int[] image;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_score);
        init();
    }
    private  void init() {
        //文本数据
        major = new String[]{"软件技术", "计算机应用技术", "通信技术", "大数据技术"};
        //图片数据
        image = new int[]{R.drawable.diqiu,R.drawable.huoxing,R.drawable.jinxing,R.drawable.shuixing};
        
        listView = findViewById(R.id.listview);

        //准备数据
        datas = new ArrayList();

        for (int i = 0; i < major.length; i++) {
            //一个列表项添加一个map
		   //	map内有两个数据 图片和字符串
            HashMap data = new HashMap();
            data.put("image", image[i]);
            data.put("text", major[i]);
            //list集合内放map列表项
            datas.add(data);
        }
        /**
         * 创建适配器
         * 适配器的作用：
         *      它把数据放到列表项布局的相应控件上
         * @Param context:上下文对象 MainActivityScore对象
         * @Param resource:只能是TestView
         */
//        ArrayAdapter adapter = new ArrayAdapter(MainActivityScore.this, android.R.layout.simple_expandable_list_item_1,major);

        /**
         * 简单适配器
         * @Param context:上下文对象 MainActivityScore对象
         * @Param data: 列表项数据
         *        List<? extends Map<String, ?>> data:
         *           list集合内放map<String, ?>对象,
         *           Map集合放每一个列表项里的数据
         *           eg:
         *              map <第一项是一个图片,第二项是一个字符串>
         *              map <key,value>
         *               有多少个列表项就有多少个键值对
         * @Param resource: 列表项布局资源id
         * @Param from：把哪个数据放到哪个控件上
         *        String[] from：这个数组里放map的键
         * @Param to: 布局中的控件id
         *        int[] to:这个数组放控件id
         * >注：第四个参数中的元素要跟第五个参数数组中的元素一一对应
         * 源码
         *     public SimpleAdapter(Context context, List<? extends Map<String, ?>> data,
         *                              int resource, String[] from, int[] to) {
         *         throw new RuntimeException("Stub!");
         *     }
         */
        SimpleAdapter adapter1 = new SimpleAdapter(MainActivityScore.this, datas,
                R.layout.item_list, new String[]{"image","text"}, new int[]{R.id.iv_icon,R.id.tv_name});
        //设置到ListView
        listView.setAdapter(adapter1);
    }
}
```

