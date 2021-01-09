```kotlin
package com.paigesoftware.timer

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.os.CountDownTimer
import kotlinx.android.synthetic.main.activity_main.*
import java.util.*

class MainActivity : AppCompatActivity() {

    companion object {
        const val START_TIME_IN_MILLIS: Long = 600000
    }

    private var gTimerIsRunning = false
    private var gTimeIncrementInMillis: Long = 0
    private var gTimeDecrementInMillis: Long = START_TIME_IN_MILLIS

    private lateinit var gCountDownTimer: CountDownTimer

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        button_start.setOnClickListener {
            if(!gTimerIsRunning) {
                startTimer()
            }
        }

        button_pause.setOnClickListener {
            if(gTimerIsRunning) {
                pauseTimer()
            }
        }

        button_reset.setOnClickListener {
            resetTimer()
        }

        updateCountDownText(gTimeDecrementInMillis, "decrement")
        updateCountDownText(gTimeIncrementInMillis, "increment")

    }

    private fun startTimer() {
        gCountDownTimer = object: CountDownTimer(gTimeDecrementInMillis, 1000) {
            override fun onTick(millisUntilFinished: Long) {
                //millisUntilFinished returns decremental value, but I intentionally `-` operation on this for cleaner code
                gTimeDecrementInMillis -= 1000
                updateCountDownText(gTimeDecrementInMillis, "decrement")
                gTimeIncrementInMillis += 1000
                updateCountDownText(gTimeIncrementInMillis, "increment")
            }

            override fun onFinish() {
                gTimerIsRunning = false
            }
        }.start()

        gTimerIsRunning = true

    }

    private fun pauseTimer() {
        gCountDownTimer.cancel()
        gTimerIsRunning = false
    }

    private fun resetTimer() {
        gCountDownTimer.cancel()
        gTimerIsRunning = false
        gTimeDecrementInMillis = START_TIME_IN_MILLIS
        gTimeIncrementInMillis = 0
        updateCountDownText(gTimeDecrementInMillis, "decrement")
        updateCountDownText(gTimeIncrementInMillis, "increment")
    }

    private fun updateCountDownText(timeInMillis: Long, type: String) {
        val minutes = (timeInMillis / 1000) / 60
        val seconds = (timeInMillis / 1000) % 60
        val formattedString = String.format(Locale.getDefault(), "%02d:%02d", minutes, seconds)
        if(type == "increment") {
            increment_timer_text_view.text = formattedString
        }
        if(type == "decrement")
        {
            decrement_timer_text_view.text = formattedString
        }
    }

}
```