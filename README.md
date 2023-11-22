#   INTENT UMUM

#   BEKER
*   Membuat Alarm

        Catatan :
        Hanya jam, menit, dan pesan tambahan yang tersedia di Android 2.3 (API level 9) dan yang lebih rendah. Tambahan lainnya ditambahkan di versi lebih baru platform ini.

    1. Intent :

        Java
  
            public void createAlarm(String message, int hour, int minutes) {
                Intent intent = new Intent(AlarmClock.ACTION_SET_ALARM)
                    .putExtra(AlarmClock.EXTRA_MESSAGE, message)
                    .putExtra(AlarmClock.EXTRA_HOUR, hour)
                    .putExtra(AlarmClock.EXTRA_MINUTES, minutes);
                if (intent.resolveActivity(getPackageManager()) != null) {
                    startActivity(intent);
                }
            }

        Kotlin
  
            fun createAlarm(message: String, hour: Int, minutes: Int) {
                val intent = Intent(AlarmClock.ACTION_SET_ALARM).apply {
                    putExtra(AlarmClock.EXTRA_MESSAGE, message)
                    putExtra(AlarmClock.EXTRA_HOUR, hour)
                    putExtra(AlarmClock.EXTRA_MINUTES, minutes)
                }
                if (intent.resolveActivity(packageManager) != null) {
                    startActivity(intent)
                }
            }
  
        Catatan :

                Untuk memanggil intent ACTION_SET_ALARM, aplikasi Anda harus memiliki izin SET_ALARM:

        Hasilnya :

    ![](Hasil%20Dari%20Alarm%20Intent%20Java%20&%20Kotlin.jpg)

    2.  Filter Intent :
   
            <activity ...>
                <intent-filter>
                  <action android:name="android.intent.action.SET_ALARM" />
                   <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>

*   Membuat Timer

        Catatan : Intent ini ditambahkan dalam Android 4.4 (API level 19).

    1.  Intent
   
        Java

            public void startTimer(String message, int seconds) {
                Intent intent = new Intent(AlarmClock.ACTION_SET_TIMER)
                    .putExtra(AlarmClock.EXTRA_MESSAGE, message)
                    .putExtra(AlarmClock.EXTRA_LENGTH, seconds)
                    .putExtra(AlarmClock.EXTRA_SKIP_UI, true);
                if (intent.resolveActivity(getPackageManager()) != null) {
                    startActivity(intent);
                }
            }

        Kotlin

            fun startTimer(message: String, seconds: Int) {
                val intent = Intent(AlarmClock.ACTION_SET_TIMER).apply {
                    putExtra(AlarmClock.EXTRA_MESSAGE, message)
                    putExtra(AlarmClock.EXTRA_LENGTH, seconds)
                    putExtra(AlarmClock.EXTRA_SKIP_UI, true)
                }
                if (intent.resolveActivity(packageManager) != null) {
                    startActivity(intent)
                }
            }

        Catatan :
            
            Untuk memanggil intent ACTION_SET_TIMER, aplikasi Anda harus memiliki izin SET_ALARM:

        Hasilnya :

    ![](Hasil%20Dari%20Timer%20Intent%20Java%20&%20Kotlin.jpg)

    2.  Filter Intent:
    
            <activity ...>
                <intent-filter>
                    <action android:name="android.intent.action.SET_TIMER" />
                    <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>

*   Menampilkan Semua Alarm

        Catatan : Intent ini ditambahkan dalam Android 4.4 (API level 19).

    Filter Maksud :

            <activity ...>
                <intent-filter>
                    <action android:name="android.intent.action.SHOW_ALARMS" />
                    <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>

#   KALENDER
*   Menambahkan Kejadian Kalender
    1.  Intent :
   
        Kotlin

            fun addEvent(title: String, location: String, begin: Long, end: Long) {
                val intent = Intent(Intent.ACTION_INSERT).apply {
                    data = Events.CONTENT_URI
                    putExtra(Events.TITLE, title)
                    putExtra(Events.EVENT_LOCATION, location)
                    putExtra(CalendarContract.EXTRA_EVENT_BEGIN_TIME, begin)
                    putExtra(CalendarContract.EXTRA_EVENT_END_TIME, end)
                }
                if (intent.resolveActivity(packageManager) != null) {
                    startActivity(intent)
                }
            }
    
        Java

            public void addEvent(String title, String location, long begin, long end) {
                Intent intent = new Intent(Intent.ACTION_INSERT)
                    .setData(Events.CONTENT_URI)
                    .putExtra(Events.TITLE, title)
                    .putExtra(Events.EVENT_LOCATION, location)
                    .putExtra(CalendarContract.EXTRA_EVENT_BEGIN_TIME, begin)
                    .putExtra(CalendarContract.EXTRA_EVENT_END_TIME, end);
                if (intent.resolveActivity(getPackageManager()) != null) {
                    startActivity(intent);
                }
            }   

    2.  Filter Intent:

            <activity ...>
                <intent-filter>
                    <action android:name="android.intent.action.INSERT" />
                    <data android:mimeType="vnd.android.cursor.dir/event" />
                    <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>

#   Kamera
*   Memotret atau merekam video dan mengembalikannya

        Catatan : Bila Anda menggunakan ACTION_IMAGE_CAPTURE untuk menjepret foto, kamera juga mungkin mengembalikan salinan foto yang diperkecil (gambar kecil) dalam hasil Intent, yang disimpan sebagai Bitmap dan kolom ekstra bernama "data".

    1.  Intent
   
        Java

            static final int REQUEST_IMAGE_CAPTURE = 1;
            static final Uri locationForPhotos;

            public void capturePhoto(String targetFilename) {
                Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
                intent.putExtra(MediaStore.EXTRA_OUTPUT,
                    Uri.withAppendedPath(locationForPhotos, targetFilename));
                if (intent.resolveActivity(getPackageManager()) != null) {
                    startActivityForResult(intent, REQUEST_IMAGE_CAPTURE);
                }
            }

            @Override
            protected void onActivityResult(int requestCode, int resultCode, Intent data) {
                if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == RESULT_OK) {
                    Bitmap thumbnail = data.getParcelableExtra("data");
                    // Do other work with full size photo saved in locationForPhotos
                    ...
                }
            }

        Kotlin

            const val REQUEST_IMAGE_CAPTURE = 1
            val locationForPhotos: Uri = ...

            fun capturePhoto(targetFilename: String) {
                val intent = Intent(MediaStore.ACTION_IMAGE_CAPTURE).apply {
                    putExtra(MediaStore.EXTRA_OUTPUT, Uri.withAppendedPath(locationForPhotos, targetFilename))
                }
                if (intent.resolveActivity(packageManager) != null) {
                    startActivityForResult(intent, REQUEST_IMAGE_CAPTURE)
                }
            }

            override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent) {
                if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == Activity.RESULT_OK) {
                    val thumbnail: Bitmap = data.getParcelableExtra("data")
                    // Do other work with full size photo saved in locationForPhotos
                    ...
                }
            }

    2.  Filter Intent

            <activity ...>
                <intent-filter>
                    <action android:name="android.media.action.IMAGE_CAPTURE" />
                    <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>

*   Memulai aplikasi kamera dalam mode gambar diam
    1.  Intent
   
        Java

            public void capturePhoto() {
                Intent intent = new Intent(MediaStore.INTENT_ACTION_STILL_IMAGE_CAMERA);
                if (intent.resolveActivity(getPackageManager()) != null) {
                    startActivityForResult(intent, REQUEST_IMAGE_CAPTURE);
                }
            }

        Kotlin

            fun capturePhoto() {
                val intent = Intent(MediaStore.INTENT_ACTION_STILL_IMAGE_CAMERA)
                if (intent.resolveActivity(packageManager) != null) {
                    startActivityForResult(intent, REQUEST_IMAGE_CAPTURE)
                }
            }

    2.  Filter Intent

            <activity ...>
                <intent-filter>
                    <action android:name="android.media.action.STILL_IMAGE_CAMERA" />
                    <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>

*   Memulai aplikasi kamera dalam mode video
    1.  Intent

        Java

            public void capturePhoto() {
                Intent intent = new Intent(MediaStore.INTENT_ACTION_VIDEO_CAMERA);
                if (intent.resolveActivity(getPackageManager()) != null) {
                    startActivityForResult(intent, REQUEST_IMAGE_CAPTURE);
                }
            }

        Kotlin

            fun capturePhoto() {
                val intent = Intent(MediaStore.INTENT_ACTION_VIDEO_CAMERA)
                if (intent.resolveActivity(packageManager) != null) {
                    startActivityForResult(intent, REQUEST_IMAGE_CAPTURE)
                }
            }

    2.  Filter Intent

            <activity ...>
                <intent-filter>
                    <action android:name="android.media.action.VIDEO_CAMERA" />
                    <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>

#   SELAMAT BELAJAR SALAM DARI KOMUNITAS IT
#   MIDUN HAKIKI (312210583)









