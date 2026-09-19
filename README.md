## Ex. No. : 5
## Develop a program to create a simple calculator using android studio.
## AIM :
To create and design an android application for a simple calculator using android studio.

## EQUIPMENTS REQUIRED :

Android Studio (Latest Version)

## ALGORITHM :

Step 1: Open Android Studio and click on File → New → New Project.

Step 2: Enter the application name as Calculator, select the required Minimum SDK, and click Finish.

Step 3: Select Empty Activity and create the Android project.

Step 4: Design the calculator interface with number and operator buttons in activity_main.xml.

Step 5: Implement addition, subtraction, multiplication, division, clear, delete, and equals operations in MainActivity.java.

Step 6: Use Implicit Intent with ACTION_SEND to share the calculated result with other applications.

Step 7: Save and run the application, perform calculations, and verify the result and sharing functionality.


## PROGRAM :

## Program to create and design an android application simple calculator using intent.

### Mainactivity.java:
```
package com.example.simplecaculator;


import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    TextView display;
    double firstNumber = 0;
    String operator = "";
    boolean newNumber = true;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        display = findViewById(R.id.display);
        int[] numberButtons = {
                R.id.btn0, R.id.btn1, R.id.btn2, R.id.btn3, R.id.btn4,
                R.id.btn5, R.id.btn6, R.id.btn7, R.id.btn8, R.id.btn9
        };
        View.OnClickListener numberListener = v -> {
            Button button = (Button) v;
            if (newNumber) {
                display.setText(button.getText().toString());
                newNumber = false;
            } else {
                display.append(button.getText().toString());
            }
        };
        for (int id : numberButtons) {
            findViewById(id).setOnClickListener(numberListener);
        }
        findViewById(R.id.btnDot).setOnClickListener(v -> {
            if (newNumber) {
                display.setText("0.");
                newNumber = false;
            } else if (!display.getText().toString().contains(".")) {
                display.append(".");
            }
        });
        findViewById(R.id.btnPlus).setOnClickListener(v -> setOperator("+"));
        findViewById(R.id.btnMinus).setOnClickListener(v -> setOperator("-"));
        findViewById(R.id.btnMultiply).setOnClickListener(v -> setOperator("×"));
        findViewById(R.id.btnDivide).setOnClickListener(v -> setOperator("÷"));
        findViewById(R.id.btnEquals).setOnClickListener(v -> calculate());
        findViewById(R.id.btnClear).setOnClickListener(v -> {
            display.setText("0");
            firstNumber = 0;
            operator = "";
            newNumber = true;
        });
        findViewById(R.id.btnDelete).setOnClickListener(v -> {
            String value = display.getText().toString();
            if (value.length() > 1) {
                display.setText(value.substring(0, value.length() - 1));
            } else {
                display.setText("0");
                newNumber = true;
            }
        });
        findViewById(R.id.btnShare).setOnClickListener(v -> {
            String result = display.getText().toString();
            Intent shareIntent = new Intent(Intent.ACTION_SEND);
            shareIntent.setType("text/plain");
            shareIntent.putExtra(
                    Intent.EXTRA_TEXT,
                    "Calculator Result: " + result
            );
            startActivity(Intent.createChooser(
                    shareIntent,
                    "Share Result"
            ));
        });
    }
    private void setOperator(String op) {
        String displayText = display.getText().toString();
        if (displayText.isEmpty()) return;

        try {
            firstNumber = Double.parseDouble(displayText);
            operator = op;
            newNumber = true;
        } catch (NumberFormatException e) {

        }
    }
    private void calculate() {
        if (operator.isEmpty()) return;
        String displayText = display.getText().toString();
        if (displayText.isEmpty()) return;

        double secondNumber;
        try {
            secondNumber = Double.parseDouble(displayText);
        } catch (NumberFormatException e) {
            return;
        }

        double result = 0;
        switch (operator) {
            case "+":
                result = firstNumber + secondNumber;
                break;
            case "-":
                result = firstNumber - secondNumber;
                break;
            case "×":
                result = firstNumber * secondNumber;
                break;
            case "÷":
                if (secondNumber == 0) {
                    display.setText(R.string.error);
                    newNumber = true;
                    operator = "";
                    return;
                }
                result = firstNumber / secondNumber;
                break;
        }

        if (result == (long) result) {
            display.setText(String.valueOf((long) result));
        } else {
            display.setText(String.valueOf(result));
        }
        operator = "";
        newNumber = true;
    }
}

```
### Activity_main.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:background="#101114">

    <TextView
        android:id="@+id/display"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:background="#1C1D21"
        android:gravity="center_vertical|end"
        android:padding="20dp" android:text="0"
        android:textColor="#FFFFFF"
        android:textSize="42sp"
        android:maxLines="1"
        android:ellipsize="start"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHeight_percent="0.28" />

    <Button
        android:id="@+id/btnDelete"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="⌫"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/display"
        app:layout_constraintStart_toEndOf="@id/btnClear"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btnDivide"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="÷"
        android:textSize="22sp"
        app:layout_constraintTop_toBottomOf="@id/display"
        app:layout_constraintStart_toEndOf="@id/btnDelete"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btnMultiply"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="×"
        android:textSize="22sp"
        app:layout_constraintTop_toBottomOf="@id/display"
        app:layout_constraintStart_toEndOf="@id/btnDivide"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn7"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="7"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btnClear"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn8"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="8"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btnClear"
        app:layout_constraintStart_toEndOf="@id/btn7"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn9"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="9"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btnClear"
        app:layout_constraintStart_toEndOf="@id/btn8"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btnMinus"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="-"
        android:textSize="22sp"
        app:layout_constraintTop_toBottomOf="@id/btnClear"
        app:layout_constraintStart_toEndOf="@id/btn9"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn4"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="4"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btn7"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn5"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="5"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btn7"
        app:layout_constraintStart_toEndOf="@id/btn4"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn6"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="6"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btn7"
        app:layout_constraintStart_toEndOf="@id/btn5"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btnPlus"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="+"
        android:textSize="22sp"
        app:layout_constraintTop_toBottomOf="@id/btn7"
        app:layout_constraintStart_toEndOf="@id/btn6"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn1"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="1"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btn4"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button android:id="@+id/btn2"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="2"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btn4"
        app:layout_constraintStart_toEndOf="@id/btn1"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn3"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="3"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btn4"
        app:layout_constraintStart_toEndOf="@id/btn2"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btnDot"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="."
        android:textSize="22sp"
        app:layout_constraintTop_toBottomOf="@id/btn4"
        app:layout_constraintStart_toEndOf="@id/btn3"
        android:layout_marginEnd="4dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btn0"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="0"
        android:textSize="20sp"
        app:layout_constraintTop_toBottomOf="@id/btn1"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintWidth_percent="0.50"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btnEquals"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="="
        android:textSize="24sp"
        android:textColor="#FFFFFF"
        android:backgroundTint="#6750A4"
        app:layout_constraintTop_toBottomOf="@id/btn1"
        app:layout_constraintStart_toEndOf="@id/btn0"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHeight_percent="0.12" />

    <Button
        android:id="@+id/btnClear"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:text="AC"
        android:textSize="20sp"
        android:textColor="#FF6B6B"
        app:layout_constraintTop_toBottomOf="@id/display"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintWidth_percent="0.25"
        app:layout_constraintHeight_percent="0.12" />
   
   <Button
        android:id="@+id/btnShare"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Share Result"
        android:textSize="16sp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        android:layout_margin="8dp" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
## OUTPUT :

<img width="1919" height="1199" alt="Screenshot 2026-08-04 141820" src="https://github.com/user-attachments/assets/6a130752-9184-4e01-8e13-6df877786afb" />

## RESULT :
Thus,A simple android application create a simple calculator using Android Studio is developed and executed successfully.
