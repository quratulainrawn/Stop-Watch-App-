# Stop-Watch-App-
Technologies Used Programming Language: Java Development Environment: Android Studio UI Design: XML
This app is a project from my App Development Internship at KaiRiz Cyber Technologies.

# Stopwatch App Report

## Project Overview

The Stopwatch App is a simple Android application designed to demonstrate the implementation of a basic stopwatch with start, stop, and reset functionalities. The app is developed using Java and XML in Android Studio. This project aims to enhance understanding of Android development concepts, such as user interface design, event handling, and threading.

## Objectives

- **Develop a user-friendly stopwatch application** with start, stop, and reset functionalities.
- **Implement accurate time measurement** using Java's `Handler` and `Runnable` classes.
- **Enhance skills in Android UI design** using XML layouts.
- **Improve knowledge of event handling** and threading in Android.

## Technologies Used

- **Programming Language:** Java
- **Development Environment:** Android Studio
- **UI Design:** XML

## Features

1. **Start Button:** Initiates the stopwatch.
2. **Stop Button:** Pauses the stopwatch without resetting the time.
3. **Reset Button:** Resets the stopwatch to zero.

## Implementation Details

### 1. Layout Design (XML)

The app's user interface is designed using XML. It consists of a `TextView` to display the elapsed time and three `Button` elements for controlling the stopwatch. Below is the XML layout for the app.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvTimer"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:textSize="48sp"
        android:text="00:00:00"
        android:layout_marginBottom="24dp"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/tvTimer"
        android:orientation="horizontal"
        android:gravity="center">

        <Button
            android:id="@+id/btnStart"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Start"
            android:layout_marginEnd="16dp" />

        <Button
            android:id="@+id/btnStop"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Stop"
            android:layout_marginEnd="16dp" />

        <Button
            android:id="@+id/btnReset"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Reset" />

    </LinearLayout>
</RelativeLayout>
```

### 2. Main Activity (Java)

The Java code manages the stopwatch's functionality, including time calculations and button event handling. Here is the implementation of the `MainActivity` class.

```java
package com.example.stopwatchapp;

import android.os.Bundle;
import android.os.Handler;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private TextView tvTimer;
    private Button btnStart, btnStop, btnReset;
    private long startTime = 0L;
    private Handler handler = new Handler();
    private long elapsedTime = 0L;
    private boolean running = false;

    private Runnable updateTimer = new Runnable() {
        @Override
        public void run() {
            if (running) {
                long millis = System.currentTimeMillis() - startTime;
                int seconds = (int) (millis / 1000);
                int minutes = seconds / 60;
                int hours = minutes / 60;
                seconds = seconds % 60;
                minutes = minutes % 60;

                tvTimer.setText(String.format("%02d:%02d:%02d", hours, minutes, seconds));
                handler.postDelayed(this, 500);
            }
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvTimer = findViewById(R.id.tvTimer);
        btnStart = findViewById(R.id.btnStart);
        btnStop = findViewById(R.id.btnStop);
        btnReset = findViewById(R.id.btnReset);

        btnStart.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (!running) {
                    startTime = System.currentTimeMillis() - elapsedTime;
                    handler.post(updateTimer);
                    running = true;
                }
            }
        });

        btnStop.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (running) {
                    elapsedTime = System.currentTimeMillis() - startTime;
                    handler.removeCallbacks(updateTimer);
                    running = false;
                }
            }
        });

        btnReset.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                startTime = 0L;
                elapsedTime = 0L;
                tvTimer.setText("00:00:00");
                handler.removeCallbacks(updateTimer);
                running = false;
            }
        });
    }
}
```

### 3. Key Functionalities

- **Start Functionality:**
  - When the start button is clicked, the app records the current time as the starting point and begins updating the `TextView` every 500 milliseconds to reflect the elapsed time.
  
- **Stop Functionality:**
  - The stop button pauses the stopwatch, saving the elapsed time so it can be resumed later.
  
- **Reset Functionality:**
  - The reset button sets the elapsed time back to zero, updating the `TextView` to display "00:00:00".

### 4. Event Handling

- **Button Click Events:**
  - The app utilizes `OnClickListener` interfaces to handle button click events, ensuring smooth and responsive interactions.

- **Handler and Runnable:**
  - The `Handler` and `Runnable` classes are used to manage time updates in a separate thread, maintaining UI responsiveness.

## Challenges and Solutions

### Challenge 1: Managing UI Updates

**Solution:**  
Using the `Handler` and `Runnable` classes allowed the app to update the UI without freezing the main thread, ensuring a smooth user experience.

### Challenge 2: Accurate Time Tracking

**Solution:**  
By recording the start time and calculating the elapsed time using `System.currentTimeMillis()`, the app maintains accurate time tracking, even when paused and resumed.

## Key Takeaways

- **Thread Management:** Learned how to use handlers and runnables to perform background tasks in Android applications.
- **UI Design with XML:** Gained experience in designing intuitive and responsive user interfaces using XML layouts.
- **Event Handling:** Enhanced skills in implementing event listeners to manage user interactions effectively.
- **Time Calculation:** Improved understanding of time measurement and calculations in Java.

## Future Enhancements

- **Lap Functionality:** Implement a lap feature to record intermediate times without stopping the stopwatch.
- **Custom Themes:** Allow users to customize the app's appearance with different themes and color schemes.
- **Notification Support:** Add notifications to alert users when a specific time has been reached.

