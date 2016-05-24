---
layout: post
title: "Android BroadcastReceiver简单实例"
subtitle: "demo"
date: 2015-03-09
author: fixxz
category: bug
tags: bug
finished: true
---
## MainActivity  广播发起者

{% highlight java %}
package pic.zx.testbroadcastreceiver;  
  
import android.app.Activity;  
import android.app.AlertDialog;  
import android.content.DialogInterface;  
import android.content.Intent;  
import android.os.Bundle;  
import android.os.Handler;  
import android.os.Message;  
import android.view.View;  
import android.view.View.OnClickListener;  
import android.widget.Button;  
import android.widget.TextView;  
  
public class MainActivity extends Activity {  
  
    private Button btn_intent;  
    private static TextView text_show;  
    private Intent intent;  
    final String INTENT_ACTION = "pic.zx.action.service";  
    final String INTENT_ACTION2 = "pic.zx.action.service2";  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
        initView();  
    }  
    private void initView(){  
        btn_intent = (Button)findViewById(R.id.btn_intent);  
        text_show = (TextView)findViewById(R.id.text_show);  
        btn_intent.setOnClickListener(new OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                new AlertDialog.Builder(MainActivity.this)  
                    .setTitle("Make a choose")  
                    .setPositiveButton("无序广播", new DialogInterface.OnClickListener() {  
                        @Override  
                        public void onClick(DialogInterface dialog, int which) {  
                            // TODO Auto-generated method stub  
                            intent = new Intent();  
                            intent.putExtra("extra", "im a test from the MainActivity");  
                            intent.putExtra("if", "no");  
                            intent.setAction(INTENT_ACTION2);  
                            sendBroadcast(intent);  
                        }  
                    })  
                    .setNegativeButton("有序广播", new DialogInterface.OnClickListener() {  
                          
                        @Override  
                        public void onClick(DialogInterface dialog, int which) {  
                            // TODO Auto-generated method stub  
                            intent = new Intent();  
                            intent.putExtra("extra", "im a test from the MainActivity");  
                            intent.putExtra("if", "yes");  
                            intent.setAction(INTENT_ACTION);  
                            sendOrderedBroadcast(intent, null);  
                        }  
                    }).create().show();  
            }  
        });  
    }  
    public static class handler_tell extends Handler{  
        @Override  
        public void handleMessage(Message msg) {  
            if(msg.what == 20201){  
                text_show.setText("第一次已接收");  
            }  
            if(msg.what == 20){  
                text_show.append("     已发送!!");  
            }  
            if(msg.what == 202){  
                text_show.append("     已接收!!");  
            }  
            super.handleMessage(msg);  
        }  
    }  
}  
{% endhighlight %}

## SecondActivity   无序广播的接受者   有序广播的优先接受者(优先级为20)	

{% highlight java %}
package pic.zx.testbroadcastreceiver;  
  
import pic.zx.testbroadcastreceiver.MainActivity.handler_tell;  
import android.content.BroadcastReceiver;  
import android.content.Context;  
import android.content.Intent;  
import android.os.Bundle;  
import android.widget.Toast;  
  
public class SecondActivity extends BroadcastReceiver{  
    handler_tell h = new handler_tell();  
    @Override  
    public void onReceive(Context context, Intent intent) {  
        // TODO Auto-generated method stub  
        h.sendEmptyMessage(20201);  
        Bundle bundle = new Bundle();  
        bundle.putString("extra2", "im a text from SecondActivityBroadcastReceiver");  
        if(intent.getExtras().getString("if").contains("yes")){  
            //讲bundle放入结果中  
            setResultExtras(bundle);  
            //取消广播  
//          abortBroadcast();   
            h.sendEmptyMessage(20);  
        }  
        else   
            Toast.makeText(context, intent.getExtras().getString("extra"), 0).show();  
    }  
}  
{% endhighlight %}

## ThirdActivity   有序广播的最终接受者(优先级为0)
{% highlight java %}
package pic.zx.testbroadcastreceiver;  
  
import pic.zx.testbroadcastreceiver.MainActivity.handler_tell;  
import android.content.BroadcastReceiver;  
import android.content.Context;  
import android.content.Intent;  
import android.os.Bundle;  
import android.widget.Toast;  
  
public class ThirdActivity extends BroadcastReceiver{  
  
    handler_tell j = new handler_tell();  
    @Override  
    public void onReceive(Context context, Intent intent) {  
        j.sendEmptyMessage(202);  
		//出问题的地方 获取整个Extras  
        Bundle bundle = intent.getExtras();  
        //获取前一个BroadcastReceiver所存入的extras  
        Bundle bundle2 = getResultExtras(true);  
        Toast.makeText(context, "初始信息为: "+bundle.getString("extra")+  
                "\n后添加的为: "+ bundle2.getString("extra2"), 1).show();  
    }  
  
}  
{% endhighlight %}

## AndroidManifest.xml
{% highlight html %}
<?xml version="1.0" encoding="utf-8"?>  
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
    package="pic.zx.testbroadcastreceiver"  
    android:versionCode="1"  
    android:versionName="1.0" >  
  
    <uses-sdk  
        android:minSdkVersion="8"  
        android:targetSdkVersion="18" />  
  
    <application  
        android:allowBackup="true"  
        android:icon="@drawable/ic_launcher"  
        android:label="@string/app_name"  
        android:theme="@style/AppTheme" >  
        <activity  
            android:name="pic.zx.testbroadcastreceiver.MainActivity"  
            android:label="@string/app_name" >  
            <intent-filter>  
                <action android:name="android.intent.action.MAIN" />  
  
                <category android:name="android.intent.category.LAUNCHER" />  
            </intent-filter>  
        </activity>  
        <receiver android:name="pic.zx.testbroadcastreceiver.SecondActivity">  
            <intent-filter android:priority="20">  
                <action android:name="pic.zx.action.service"/>  
                 <action android:name="pic.zx.action.service2"/>  
            </intent-filter>  
        </receiver>  
        <receiver android:name="pic.zx.testbroadcastreceiver.ThirdActivity">  
            <intent-filter android:priority="0">  
                <action android:name="pic.zx.action.service"/>  
            </intent-filter>  
        </receiver>  
    </application>  
  
</manifest>  
{% endhighlight %}