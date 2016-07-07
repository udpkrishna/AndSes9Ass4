# AndSes9Ass4

MainActivity.java
package me.rk.andses9ass4;

import android.support.v4.app.FragmentManager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.ListView;
import android.widget.Toast;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity implements DialogCallback{
    ListView mListView;
    private CustomAdaptor mCustomAdaptor;
    public static ArrayList<DataModle> arrayList=new ArrayList();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mListView=(ListView)findViewById(R.id.listView);


        mCustomAdaptor=new CustomAdaptor(this,arrayList);
        mListView.setAdapter(mCustomAdaptor);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.app_menu, menu);
        return super.onCreateOptionsMenu(menu);
    }

    final FragmentManager fragmentManager=getSupportFragmentManager();
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        Toast.makeText(MainActivity.this, "Add is selected", Toast.LENGTH_SHORT).show();
        AppDialogFragment appDialogFragment=new AppDialogFragment(this);
        appDialogFragment.show(fragmentManager,"Entry Dialog");


        return super.onOptionsItemSelected(item);
    }


    @Override
    public void getDataFromDialog() {

    mCustomAdaptor.notifyDataSetChanged();
    }
}

AppDialogFragment.java
package me.rk.andses9ass4;

import android.annotation.SuppressLint;
import android.app.Dialog;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.app.DialogFragment;
import android.text.style.TtsSpan;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.Toast;

/**
 * Created by airodyra on 7/3/2016.
 */
public class AppDialogFragment extends DialogFragment{

//    ListView listView;
    DialogCallback mDialogCallback;

    public AppDialogFragment(){

    }
    @SuppressLint("ValidFragment")
    public AppDialogFragment(DialogCallback dialogCallback){
//        listView=nlistview;
        mDialogCallback=dialogCallback;
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View dialogView=inflater.inflate(R.layout.dialog_fragment, container,false);
        final Dialog dialog=getDialog();
        dialog.setTitle("Entry Details");
        dialog.setCancelable(false);

        final EditText nameEditText=(EditText)dialogView.findViewById(R.id.name);
        final EditText phonenumberEditText=(EditText)dialogView.findViewById(R.id.phonenumber);
        final EditText dobEditText=(EditText)dialogView.findViewById(R.id.dob);
       // listView=(ListView)dialogView.findViewById(R.id.listView);

        Button saveButton=(Button)dialogView.findViewById(R.id.saveButton);
        saveButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String name=nameEditText.getText().toString();
                String phone=phonenumberEditText.getText().toString();
                String dob=dobEditText.getText().toString();


                if (nameEditText.length()==0||phonenumberEditText.length()==0||dobEditText.length()==0) {
                    Toast.makeText(getActivity(), "Fields cannot be empty", Toast.LENGTH_SHORT).show();
                }else{
                   // final String[] nameArray={name};
                    //final String[] phoneArray={phone};
                    //final String[] dobArray={dob};

                    DataModle dataModle=new DataModle();
                    dataModle.setName(name);
                    dataModle.setPhoneNo(phone);
                    dataModle.setDob(dob);

                    MainActivity.arrayList.add(dataModle);

                    Log.i("Name",name );
                    Log.i("Phone",phone);
                    Log.i("DOB", dob);
                    Log.i("Activity", String.valueOf(getActivity()));

                    mDialogCallback.getDataFromDialog();
                }
            }
        });

        Button cancelButton=(Button)dialogView.findViewById(R.id.cancelButton);
        cancelButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                dialog.dismiss();
            }
        });

        return dialogView;
    }
}

DataModle.java
package me.rk.andses9ass4;

/**
 * Created by airodyra on 7/6/2016.
 */
public class DataModle {

    String name;
    String dob;
    String phoneNo;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDob() {
        return dob;
    }

    public void setDob(String dob) {
        this.dob = dob;
    }

    public String getPhoneNo() {
        return phoneNo;
    }

    public void setPhoneNo(String phoneNo) {
        this.phoneNo = phoneNo;
    }
}

DialogCallback.java
package me.rk.andses9ass4;

/**
 * Created by airodyra on 7/6/2016.
 */
public interface DialogCallback {

    void getDataFromDialog();
}

activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="me.rk.andses9ass4.MainActivity">

    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"></ListView>
</RelativeLayout>

custom_list_view.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="@android:color/holo_blue_light"
    android:padding="10dp">

    <TextView
        android:id="@+id/name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Name"
        android:textSize="20dp"/>

    <TextView
        android:id="@+id/phonenumber"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Phone Number"
        android:textSize="20dp"/>

    <TextView
        android:id="@+id/dob"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Date of Birth"
        android:textSize="20dp"/>
</LinearLayout>

dialog_fragment.xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/heading"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Enter the Details"
        android:padding="10dp"
        android:textSize="25dp"
        android:textColor="@android:color/holo_blue_bright"/>

    <EditText
        android:id="@+id/name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/heading"
        android:hint="Name"
        android:padding="10dp"/>

    <EditText
        android:id="@+id/phonenumber"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/name"
        android:hint="Phone Number"
        android:padding="10dp"/>

    <EditText
        android:id="@+id/dob"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/phonenumber"
        android:hint="Date of Birth"
        android:padding="10dp"/>

    <Button
        android:id="@+id/saveButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Save"
        android:layout_below="@+id/dob"
        android:layout_toStartOf="@+id/cancelButton" />

    <Button
        android:id="@+id/cancelButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Cancel"
        android:layout_below="@+id/dob"
        android:layout_alignParentEnd="true"
        android:layout_marginEnd="98dp" />

    <ListView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/saveButton"></ListView>

</RelativeLayout>
