### Basic Steps to create Notification

* First create Notification Channel
    ```java
    private final String CHANNEL_ID = "my_channel_01";

    public void createChannel(){

        if(Build.VERSION.SDK_INT < Build.VERSION_CODES.O) return;
        // The user-visible name of the channel.
        CharSequence name = getString(R.string.channel_name);
        // The user-visible description of the channel.
        String description = getString(R.string.channel_description);

        int importance = NotificationManager.IMPORTANCE_LOW;

        NotificationChannel mChannel = new NotificationChannel(CHANNEL_ID, name,importance);
        // Configure the notification channel.
        mChannel.setDescription(description);

        mChannel.enableLights(true);
        // Sets the notification light color for notifications posted to this
        // channel, if the device supports this feature.
        mChannel.setLightColor(Color.RED);

        mChannel.enableVibration(true);
        mChannel.setVibrationPattern(new long[]{100, 200, 300, 400, 500, 400, 300, 200, 400});

        if(notificationManagerCompat == null) notificationManagerCompat = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);
        notificationManagerCompat.createNotificationChannel(mChannel);
    }
    ```

* Now create Notification with the channel_id
    ```java
    public void createNotification(int notificationId, String title, String body) {
        NotificationCompat.Builder mBuilder = new NotificationCompat.Builder(this, CHANNEL_ID)
                .setContentTitle(title)
                .setContentText(body)
                .setSmallIcon(android.R.drawable.sym_def_app_icon)
                .setAutoCancel(true);

        // Creates an explicit intent for an Activity in your app
        Intent resultIntent = new Intent(this, SeconderyActivity.class);
        PendingIntent resultPendingIntent = PendingIntent.getActivity(this, notificationId, resultIntent, PendingIntent.FLAG_UPDATE_CURRENT );

        mBuilder.setContentIntent(resultPendingIntent);

        // mNotificationId is a unique integer your app uses to identify the
        // notification. For example, to cancel the notification, you can pass its ID
        // number to NotificationManager.cancel().
        notificationManagerCompat.notify(notificationId, mBuilder.build());
    }
    ```
    __Remember :__ If you create second notification with same id. Only the latest one would be shown, others would be update with the latest one. 
