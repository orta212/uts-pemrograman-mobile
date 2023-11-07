## **Repositori ini dibuat untuk memenuhi penilaian tengah semester matakuliah pemrograman mobile**  
Nama : Orta Yamaesa  
Nim : 312210147  
Kelas : TI.22.B1  
Mata Kuliah : Pemrograman Mobile  
**Tugas : Membuat tombol yang setiap diklik dapat bertambah angkanya, namun dengan urutan angka fibonacci, lalu lengkapi dengan fitur toast**  
berikut adalah link video aplikasi yang di jalankan (dijalankan pada device menggunakan usb debugging) : [tonton video](https://www.youtube.com/shorts/Fj5d-t8HxHg){:target="_blank"}  
<br>
Source Code:  
* activity_main.Xml (dibuat dengan design pada android studio):


  <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >
    <LinearLayout
        android:id="@+id/linear"
        android:layout_width="match_parent"
        android:layout_height="502dp"
        android:background="#eeeeee"
        android:gravity="center"
        android:orientation="vertical"
        android:paddingLeft="10dp"
        android:paddingRight="10dp"
        android:rotationX="2">

        <TextView
            android:id="@+id/textNama"
            android:layout_width="200dp"
            android:layout_height="wrap_content"
            android:text="Orta Yamaesa 312210147"
            android:textAlignment="center"
            android:textSize="24sp" />

        <TextView
            android:id="@+id/textCount"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Tombol Hitung diklik sebanyak : 0"
            android:textAlignment="center"
            android:textSize="20sp" />

        <TextView
            android:id="@+id/textCountFibo"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="5dp"
            android:layout_marginBottom="5dp"
            android:marqueeRepeatLimit="marquee_forever"
            android:paddingTop="5dp"
            android:paddingBottom="5dp"
            android:text="0"
            android:textAlignment="center"
            android:textColor="#000000"
            android:textSize="150sp" />

    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="165dp"
        android:layout_below="@id/linear"
        android:orientation="vertical">

        <Button
            android:id="@+id/buttonCount"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="1dp"
            android:layout_marginLeft="1dp"
            android:layout_marginTop="3dp"
            android:text="HITUNG"
            android:textAlignment="center"
            android:textStyle="bold" />

        <Button
            android:id="@+id/buttonReset"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="1dp"
            android:layout_marginLeft="1dp"
            android:layout_marginTop="3dp"
            android:text="RESET"
            android:textStyle="bold" />

        <Button
            android:id="@+id/buttonToast"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Tampilkan Toast"
            android:textAlignment="center"
            android:textStyle="bold" />
    </LinearLayout>

</RelativeLayout>

Penjelasan :  
1. kita isi relative layout dengan 2 linear layout
2. linear layout1 diisi dengan 3 textview:
   * nama + nim
   * jumlah klik pada tombol hitung
   * angka fibonacci terakhir
3. linear layout 2 berisi :
   * button hitung
   * button reset
   * dan button toast  



* MainActivity.Java :  

``` java
package id.co.myapllication;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    public int count = 0 ;
    public int countFibo = 0 ;
    public TextView showCount;
    public TextView showCountFibo;
    public Button buttonToast;
    public Button buttonCount;
    public Button buttonReset;
    public Toast toastA;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        buttonToast=(Button)findViewById(R.id.buttonToast);
        buttonCount=(Button)findViewById(R.id.buttonCount);
        buttonReset=(Button)findViewById(R.id.buttonReset);
        showCount =(TextView)findViewById(R.id.textCount);
        showCountFibo =(TextView)findViewById(R.id.textCountFibo);


        buttonToast.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(toastA != null) { toastA.cancel(); }
                    toastA = Toast.makeText(getApplicationContext(), "Angka Fibonacci : " + countFibo, Toast.LENGTH_SHORT);
                    toastA.show();
            }
        });

        buttonCount.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) { calculate(view); }
        });

        buttonReset.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) { reset(view); }
        });


    }
    protected void calculate(View view){
        count++;
        countFibo = calculateFibo(count);
        showCount.setText("Tombol Hitung diklik sebanyak : " + Integer.toString(count));
        showCountFibo.setText(Integer.toString(countFibo));
        if(count % 5 == 0){
            if(toastA != null) toastA.cancel();
            toastA = Toast.makeText(getApplicationContext(),"Tombol hitung diklik : "+ count + " Kali", Toast.LENGTH_SHORT);
            toastA.show();
        }
    }

    protected int calculateFibo(int n){
        if(n <= 1) return n;
        int prev,current,next;
        prev = 0;
        current = 1;
        for (int i = 2; i <= n; i++) {
            next = prev + current;
            prev = current;
            current = next;
        }
        return current;
    }

    protected void reset(View view){
        count = 0;
        countFibo = 0;
        showCount.setText("Tombol Hitung diklik sebanyak : " + Integer.toString(count));
        showCountFibo.setText(Integer.toString(countFibo));
    }
}
```

penjelasan MainActivity.java :   
``` Java
    public int count = 0 ;
    public int countFibo = 0 ;
    public TextView showCount;
    public TextView showCountFibo;
    public Button buttonToast;
    public Button buttonCount;
    public Button buttonReset;
    public Toast toastA;
```  
* kita deklarasikan properti atau atribut yang akan kita gunakan dalam aplikasi ini
* untuk count dan countFibo langsung kita isi dengan nilai 0 (integer)
* masing masing diberi sesuai tipe data dan kita set visibility nya public
<br> <br> <br>
```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        buttonToast=(Button)findViewById(R.id.buttonToast);
        buttonCount=(Button)findViewById(R.id.buttonCount);
        buttonReset=(Button)findViewById(R.id.buttonReset);
        showCount =(TextView)findViewById(R.id.textCount);
        showCountFibo =(TextView)findViewById(R.id.textCountFibo);


        buttonToast.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(toastA != null) { toastA.cancel(); }
                    toastA = Toast.makeText(getApplicationContext(), "Angka Fibonacci : " + countFibo, Toast.LENGTH_SHORT);
                    toastA.show();
            }
        });

        buttonCount.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) { calculate(view); }
        });

        buttonReset.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) { reset(view); }
        });


    }
```
* disini kita akan mengisi beberapa properti yang belum diisi sebelumnya sesuai dengan tipe data
* lalu dihubungkan menggunakan tipe data dan juga id yang ada ada activity_main.xml
* kemudian kita beri event atau aksi yang akan dilakukan ketika tombol tombol diklik
* saat tombol hitung diklik maka akan menjalankan fungsi calculate
* saat tombol reset diklik maka akan menjalankan fungsi reset
* saat tombol tampilkan toast diklik maka akan menampilkan toast dengan data angka fibonacci
  <br> <br>

```java
    protected void calculate(View view){
        count++;
        countFibo = calculateFibo(count);
        showCount.setText("Tombol Hitung diklik sebanyak : " + Integer.toString(count));
        showCountFibo.setText(Integer.toString(countFibo));
        if(count % 5 == 0){
            if(toastA != null) toastA.cancel();
            toastA = Toast.makeText(getApplicationContext(),"Tombol hitung diklik : "+ count + " Kali", Toast.LENGTH_SHORT);
            toastA.show();
        }
    }
```
* saat fungsi dijalankan, maka akan menambah properti count dengan nilai 1
* kemudian count yang sudah ditambah , maka akan dijadikan nilai dalam menjalankan fungsi calculateFibo lalu akan menerima data baru yang telah diolah di fungsi tersebut dan kemusian dimasukkan ke properti countFibo
* tampilkan jumlah klik tombol hitung dan juga angka fibonacci
* kita tampilkan toast setiap tombol count diklik sebanyak 5 kali
* untuk toastA.cancel() bermaksud agar toast yang sudah ada langsung dihentikan karena toast paling cepat akan hilang dalam dua detik, jika belum dua detik dan ada toast yang aktif maka toast yang baru akan masuk dalam antrian, dan akan dijalankan berurut setelah dua detik dengan cancel ini maka kita hentikan toast yang ada
* lalu jalankan toast.show() agar langsung kita tampilkan toast yang baru
<br> <br> <br>

```java
    protected int calculateFibo(int n){
        if(n <= 1) return n;
        int prev,current,next;
        prev = 0;
        current = 1;
        for (int i = 2; i <= n; i++) {
            next = prev + current;
            prev = current;
            current = next;
        }
        return current;
    }
```
* di fungsi calculateFibo angka count yang dikirimkan dari fungsi calculate akan diolah sehingga akan me return angka fibonacci
<br> <br> <br>

```java
protected void reset(View view){
        count = 0;
        countFibo = 0;
        showCount.setText("Tombol Hitung diklik sebanyak : " + Integer.toString(count));
        showCountFibo.setText(Integer.toString(countFibo));
    }
```
* disini semua properti angka dan textview akan direset
