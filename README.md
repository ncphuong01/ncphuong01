vẽ gd
*******chuyển tab
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {
    private Button button_addR;
    private Button button_addP;
    private Button button_userRP;
    private Button button_statRP;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_main);
        button_addR = findViewById(R.id.button_addR);
        button_addR.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                        Intent intent = new Intent(MainActivity.this, NhapThuActivity.class);
                        startActivity(intent);

            }
        });
      }
    }



**************************************88
    Tạo pách kít model user và sqlite

  user:

  package com.example.myapplication123.model;

public class User {
    private String username;
    private String password;
    private String name;
    private String sex;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }
}


sqlite:

package com.example.myapplication123.model;

import android.content.ContentValues;
import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteDatabase.CursorFactory;
import android.database.sqlite.SQLiteOpenHelper;

public class SQLite extends SQLiteOpenHelper{
    private static final String DATABASE_NAME = "usernamemanage";
    private static final int DATABASE_VERSION = 1;
    private static final String TABLE_NAME = "user";

    private static final String KEY_USERNAME = "username";
    private static final String KEY_PASSWORD = "password";
    private static final String KEY_NAME = "name";
    private static final String KEY_SEX = "sex";

    public SQLite(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String create_user_table = String.format("CREATE TABLE %s(%s TEXT, %s TEXT, %s TEXT, %s TEXT)", TABLE_NAME, KEY_USERNAME, KEY_PASSWORD, KEY_NAME, KEY_SEX);
        db.execSQL(create_user_table);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        String drop_user_table = String.format("DROP TABLE IF EXISTS %s", TABLE_NAME);
        db.execSQL(drop_user_table);

        onCreate(db);
    }
    public void adduser(User user) {
        SQLiteDatabase db = this.getWritableDatabase();

        ContentValues values = new ContentValues();
        values.put(KEY_USERNAME, user.getUsername());
        values.put(KEY_PASSWORD, user.getPassword());
        values.put(KEY_NAME, user.getName());
        values.put(KEY_SEX, user.getSex());

        db.insert(TABLE_NAME, null, values);
        db.close();
    }

}



****************************8
Thêm dl bằng sql tại chỗ thêm dl


import androidx.appcompat.app.AppCompatActivity;

import android.content.Context;
import android.content.Intent;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.example.myapplication123.model.SQLite;
import com.example.myapplication123.model.User;

public class SignUpActivity extends AppCompatActivity {
    private Button SU_buttonback;
    private Button SU_buttonsu;
    private EditText SU_username,SU_password,SU_name,SU_sex;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        SQLite sql = new SQLite(this);
        setContentView(R.layout.activity_sign_up);

        SU_buttonback = findViewById(R.id.SU_buttonback);
        SU_buttonback.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(SignUpActivity.this, MainActivity.class);
                startActivity(intent);
            }
        });
        SU_username = findViewById(R.id.SU_username);
        SU_password = findViewById(R.id.SU_password);
        SU_name = findViewById(R.id.SU_name);
        SU_sex = findViewById(R.id.SU_sex);
        SU_buttonsu = findViewById(R.id.SU_buttonsu);
        SU_buttonsu.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                User newuser = new User();

                newuser.setName(SU_name.getText().toString());
                newuser.setPassword(SU_password.getText().toString());
                newuser.setUsername(SU_username.getText().toString());
                newuser.setSex(SU_sex.getText().toString());
                sql.adduser(newuser);
                Intent intent = new Intent(SignUpActivity.this, HomeActivity.class);
                startActivity(intent);
            }
        });

    }
}

