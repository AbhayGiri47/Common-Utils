# Common-Utils
Utils such as isNetworkConnected  ,EmailpatternMatcher ,hide keyboard, getcalenderDate



open class CommonUtils {

    companion object {

        fun validEmail(email: String): Boolean {
            return android.util.Patterns.EMAIL_ADDRESS.matcher(email).matches()
        }

        fun isNetworkConnected(context: Context): Boolean {
            val cm = context.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager
            val activeNetwork = cm.activeNetworkInfo
            return activeNetwork != null && activeNetwork.isConnectedOrConnecting
        }

        fun hideSoftKeyBoard(context: Activity) {
            val view = context.currentFocus
            if (view != null) {
                val imm =
                    context.applicationContext.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
                imm.hideSoftInputFromWindow(view.getWindowToken(), 0)
            }
        }

        fun hideKeyboardFrom(context: Context, view: View) {
            val imm = context.getSystemService(Activity.INPUT_METHOD_SERVICE) as InputMethodManager
            imm.hideSoftInputFromWindow(view.windowToken, 0)
        }

        fun getCalenderDate(context: Context, listener: DateChooseListener) {

            val calender = Calendar.getInstance(TimeZone.getDefault())

            val dialog = DatePickerDialog(context, DatePickerDialog.OnDateSetListener { view, year, month, dayOfMonth ->
                val monthOfYear: Int = month + 1

                var updatedDate: String = ""
                var updatedMonth: String = ""

                if (monthOfYear in 0..9)
                    updatedMonth = "0$monthOfYear"
                else
                    updatedMonth = monthOfYear.toString()

                if (dayOfMonth in 0..9)
                    updatedDate = "0$dayOfMonth"
                else
                    updatedDate = dayOfMonth.toString()

                val selectedDate = "$updatedMonth/$updatedDate/$year"
                listener.onDatePick(selectedDate)

            }, calender.get(Calendar.YEAR), calender.get(Calendar.MONTH), calender.get(Calendar.DAY_OF_MONTH))
            //condition to disable past date
            //dialog.datePicker.minDate = System.currentTimeMillis() - 1000
            dialog.show()
        }


        fun getCalenderDateMonthRange(context: Context, listener: DateChooseListener,minMonth :Int =-1,maxMonth : Int =-1) {

            val calender = Calendar.getInstance(TimeZone.getDefault())

            val dialog = DatePickerDialog(context, DatePickerDialog.OnDateSetListener { view, year, month, dayOfMonth ->
                val monthOfYear: Int = month + 1

                var updatedDate: String = ""
                var updatedMonth: String = ""

                if (monthOfYear in 0..9)
                    updatedMonth = "0$monthOfYear"
                else
                    updatedMonth = monthOfYear.toString()

                if (dayOfMonth in 0..9)
                    updatedDate = "0$dayOfMonth"
                else
                    updatedDate = dayOfMonth.toString()

                val selectedDate = "$updatedMonth/$updatedDate/$year"
                listener.onDatePick(selectedDate)

            }, calender.get(Calendar.YEAR), calender.get(Calendar.MONTH), calender.get(Calendar.DAY_OF_MONTH))
            //condition to disable past date
            //dialog.datePicker.minDate = System.currentTimeMillis() - 1000
            if (minMonth==0){
                dialog.datePicker.minDate =  Calendar.getInstance().timeInMillis
            }else if (minMonth!=0 && minMonth!=-1){
                val cal =Calendar.getInstance()
                cal.add(Calendar.MONTH,minMonth)
                dialog.datePicker.minDate =  cal.timeInMillis
            }

            if (maxMonth==0){
                dialog.datePicker.maxDate =  Calendar.getInstance().timeInMillis
            }else if (maxMonth!=0 && maxMonth!=-1){
                val cal =Calendar.getInstance()
                cal.add(Calendar.MONTH,maxMonth)
                dialog.datePicker.maxDate =  cal.timeInMillis
            }

            dialog.show()
        }


        fun getStartTime(context: Activity, listner: TimePickerDialog.OnTimeSetListener) {
            var time = ""
            val mcurrentTime: Calendar = Calendar.getInstance()
            val hour: Int = mcurrentTime.get(Calendar.HOUR_OF_DAY)
            val minuteCal: Int = mcurrentTime.get(Calendar.MINUTE)


            val timePickerDialog = TimePickerDialog(
                context, TimePickerDialog.OnTimeSetListener { view, hourOfDay, minute ->
                    val period: String
                    var hour1 = 0

                    var hourUpdate = ""
                    var minuteUpdate = ""

                    if (minute in 0..9)
                        minuteUpdate = "0$minute"
                    else
                        minuteUpdate = minute.toString()
                    if (hourOfDay in 0..11)
                        period = "AM"
                    else
                        period = "PM"
                    if (period == "AM") {
                        if (hourOfDay == 0)
                            hour1 = 12
                        else if (hourOfDay in 1..11)
                            hour1 = hourOfDay
                    } else if (period == "PM") {
                        if (hourOfDay == 12)
                            hour1 = 12
                        else if (hourOfDay in 13..23)
                            hour1 = hourOfDay - 12
                    }

                    if (hour1 in 0..9)
                        hourUpdate = "0$hour1"
                    else
                        hourUpdate = hour1.toString()

                    time = "$hourUpdate:$minuteUpdate $period"

                    listner.onTimeSet(view, hour1, minute)
                }, hour, minuteCal, false
            )
            timePickerDialog.show()
        }

        fun getCurrentdate(format : String="dd/MM/yyyy"): String {
            val c = Calendar.getInstance().time
//            println("Current time => $c")
            val df = SimpleDateFormat(format)
            return df.format(c)
        }

        fun getCurrentdate1(format : String="dd-MM-yyyy"): String {
            val c = Calendar.getInstance().time
//            println("Current time => $c")
            val df = SimpleDateFormat(format)
            return df.format(c)
        }

        fun convertBytesToKB(size: Int): String {
            return if (size > 0) {
                val localSize:Double = size.toDouble() / 1024
                DecimalFormat("#######.##").format(localSize) + " KB"
            } else {
                "0 KB"
            }
        }

        fun convertDecimalTo2decimal(value: Double): String {
            return DecimalFormat("#######.##").format(value)
        }
    }


    interface DateChooseListener {
        fun onDatePick(date: String)
    }
}
