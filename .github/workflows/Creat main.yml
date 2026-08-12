implementation platform('com.google.firebase:firebase-bom:32.7.0')
implementation 'com.google.firebase:firebase-firestore-ktx'
package com.example.serialapp

import android.os.Bundle
import android.widget.*
import androidx.appcompat.app.AppCompatActivity
import com.google.firebase.firestore.FirebaseFirestore

class MainActivity : AppCompatActivity() {

    private lateinit.db: FirebaseFirestore
    private lateinit var contentLayout: FrameLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        db = FirebaseFirestore.getInstance()
        contentLayout = findViewById(R.id.contentLayout)

        // ডিফল্টভাবে Total Serial পেজ লোড হবে
        loadTotalSerialFragment()

        // নিচের চারটি ন্যাভিগেশন আইকনের কাজ
        findViewById<LinearLayout>(R.id.navTotalSerial).setOnClickListener {
            loadTotalSerialFragment()
        }
        findViewById<LinearLayout>(R.id.navAddSerial).setOnClickListener {
            loadAddSerialFragment()
        }
        findViewById<LinearLayout>(R.id.navAddDoctor).setOnClickListener {
            loadAddDoctorFragment()
        }
        findViewById<LinearLayout>(R.id.navAddCareOf).setOnClickListener {
            loadAddCareOfFragment()
        }
    }

    private fun loadTotalSerialFragment() {
        contentLayout.removeAllViews()
        val view = layoutInflater.inflate(R.layout.fragment_total_serial, contentLayout, false)
        
        val tvTotal = view.findViewById<TextView>(R.id.tvTotalCount)
        val listView = view.findViewById<ListView>(R.id.listViewSerials)

        // Firestore থেকে সিরিয়াল ফেচ করা
        db.collection("serials").addSnapshotListener { snapshots, e ->
            if (e != null) return@addSnapshotListener
            val serials = mutableListOf<String>()
            for (doc in snapshots!!) {
                val patient = doc.getString("patientName") ?: ""
                val doctor = doc.getString("doctorName") ?: ""
                val status = doc.getString("status") ?: "অপেক্ষমাণ"
                serials.add("রোগী: $patient | ডাক্তার: $doctor [$status]")
            }
            tvTotal.text = "টোটাল সিরিয়াল: ${serials.size}"
            val adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, serials)
            listView.adapter = adapter
        }
        contentLayout.addView(view)
    }

    private fun loadAddSerialFragment() {
        contentLayout.removeAllViews()
        val view = layoutInflater.inflate(R.layout.fragment_add_serial, contentLayout, false)

        val etPatient = view.findViewById<EditText>(R.id.etPatientName)
        val etCareOf = view.findViewById<EditText>(R.id.etCareOfName)
        val etDoctor = view.findViewById<EditText>(R.id.etDoctorName)
        val btnSave = view.findViewById<Button>(R.id.btnSaveSerial)

        btnSave.setOnClickListener {
            val patient = etPatient.text.toString().trim()
            val careOf = etCareOf.text.toString().trim()
            val doctor = etDoctor.text.toString().trim()

            if (patient.isNotEmpty() && doctor.isNotEmpty()) {
                val serialMap = hashMapOf(
                    "patientName" to patient,
                    "careOfName" to careOf,
                    "doctorName" to doctor,
                    "status" to "অপেক্ষমাণ",
                    "timestamp" to System.currentTimeMillis()
                )

                db.collection("serials").add(serialMap)
                    .addOnSuccessListener {
                        Toast.makeText(this, "সিরিয়াল সফলভাবে যোগ করা হয়েছে!", Toast.LENGTH_SHORT).show()
                        etPatient.text.clear()
                        etCareOf.text.clear()
                        etDoctor.text.clear()
                    }
                    .addOnFailureListener {
                        Toast.makeText(this, "ত্রুটি হয়েছে!", Toast.LENGTH_SHORT).show()
                    }
            } else {
                Toast.makeText(this, "দয়া করে পেশেন্ট এবং ডাক্তারের নাম লিখুন", Toast.LENGTH_SHORT).show()
            }
        }
        contentLayout.addView(view)
    }

    private fun loadAddDoctorFragment() {
        contentLayout.removeAllViews()
        val view = layoutInflater.inflate(R.layout.fragment_add_doctor, contentLayout, false)
        val etDoctor = view.findViewById<EditText>(R.id.etDoctorInput)
        val btnAdd = view.findViewById<Button>(R.id.btnAddDoc)

        btnAdd.setOnClickListener {
            val docName = etDoctor.text.toString().trim()
            if (docName.isNotEmpty()) {
                db.collection("doctors").add(hashMapOf("name" to docName))
                    .addOnSuccessListener {
                        Toast.makeText(this, "ডাক্তার যোগ করা হয়েছে", Toast.LENGTH_SHORT).show()
                        etDoctor.text.clear()
                    }
            }
        }
        contentLayout.addView(view)
    }

    private fun loadAddCareOfFragment() {
        contentLayout.removeAllViews()
        val view = layoutInflater.inflate(R.layout.fragment_add_careof, contentLayout, false)
        val etCareOf = view.findViewById<EditText>(R.id.etCareOfInput)
        val btnAdd = view.findViewById<Button>(R.id.btnAddCare)

        btnAdd.setOnClickListener {
            val careName = etCareOf.text.toString().trim()
            if (careName.isNotEmpty()) {
                db.collection("careofs").add(hashMapOf("name" to careName))
                    .addOnSuccessListener {
                        Toast.makeText(this, "কেয়ার অফ যোগ করা হয়েছে", Toast.LENGTH_SHORT).show()
                        etCareOf.text.clear()
                    }
            }
        }
        contentLayout.addView(view)
    }
}
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- ডাইনামিক কন্টেন্ট এরিয়া -->
    <FrameLayout
        android:id="@+id/contentLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@id/bottomNav" />

    <!-- নিচের চারটি আইকন আকৃতির অপশন -->
    <LinearLayout
        android:id="@+id/bottomNav"
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:layout_alignParentBottom="true"
        android:background="#EFEFEF"
        android:orientation="horizontal"
        android:weightSum="4">

        <LinearLayout
            android:id="@+id/navTotalSerial"
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">
            <TextView android:text="📋" android:textSize="18sp" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
            <TextView android:text="Total" android:textSize="12sp" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
        </LinearLayout>

        <LinearLayout
            android:id="@+id/navAddSerial"
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">
            <TextView android:text="➕" android:textSize="18sp" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
            <TextView android:text="Add Serial" android:textSize="12sp" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
        </LinearLayout>

        <LinearLayout
            android:id="@+id/navAddDoctor"
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">
            <TextView android:text="👨‍⚕️" android:textSize="18sp" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
            <TextView android:text="Add Doctor" android:textSize="12sp" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
        </LinearLayout>

        <LinearLayout
            android:id="@+id/navAddCareOf"
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:gravity="center"
            android:orientation="vertical">
            <TextView android:text="👤" android:textSize="18sp" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
            <TextView android:text="Care Of" android:textSize="12sp" android:layout_width="wrap_content" android:layout_height="wrap_content"/>
        </LinearLayout>

    </LinearLayout>
</RelativeLayout>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp"
    android:gravity="center">

    <EditText
        android:id="@+id/etPatientName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Patient Name (পেশেন্ট নেম)" />

    <EditText
        android:id="@+id/etCareOfName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Care Of Name (কেয়ার অফ নেম)" />

    <EditText
        android:id="@+id/etDoctorName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Doctor Name (ডাক্তার নেম)" />

    <Button
        android:id="@+id/btnSaveSerial"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Save Serial"
        android:layout_marginTop="20dp"/>
</LinearLayout>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvTotalCount"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="টোটাল সিরিয়াল: 0"
        android:textSize="18sp"
        android:textStyle="bold"
        android:paddingBottom="10dp"/>

    <ListView
        android:id="@+id/listViewSerials"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true; // টেস্ট বা ছোট প্রজেক্টের জন্য উন্মুক্ত রাখা হয়েছে
    }
  }
}
