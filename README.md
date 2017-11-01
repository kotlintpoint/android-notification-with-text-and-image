# android-notification-with-text-and-image

###### Notification with Image 
```
private void showNotification(Context context) {
        NotificationManager notificationManager = (NotificationManager)
                context.getSystemService(Context.NOTIFICATION_SERVICE);

        Intent notificationIntent = new Intent(context, MainActivity.class);

        notificationIntent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP
                | Intent.FLAG_ACTIVITY_SINGLE_TOP);

        PendingIntent intent =
                PendingIntent.getActivity(context, 0,
                        notificationIntent,PendingIntent.FLAG_UPDATE_CURRENT);

        Bitmap bitmap = BitmapFactory.decodeResource(getResources(),
                R.mipmap.ic_launcher);
        String message="Hello Notification with image";
        String title="Notification !!!";
        int icon = R.mipmap.ic_launcher;
        long when = System.currentTimeMillis();

        Notification notification = new NotificationCompat.Builder(context)
                .setContentTitle(title)
                .setContentText(message)
                .setContentIntent(intent)
                .setSmallIcon(icon)
                .setWhen(when)
                .setStyle(new NotificationCompat.BigPictureStyle()
                        .bigPicture(bitmap).setSummaryText(message))
                .build();

        notification.flags |= Notification.FLAG_AUTO_CANCEL;

// Play default notification sound
        notification.defaults |= Notification.DEFAULT_SOUND;
        notification.defaults |= Notification.DEFAULT_VIBRATE;
        notificationManager.notify(0, notification);
    }
```
###### Notification with Multiline Text BigTextStyle
```
private void showNotification(Context context) {
       Notification notification =
                new NotificationCompat.Builder(this)
                        .setSmallIcon(R.mipmap.ic_launcher)
                        .setContentTitle("APP Title")
                        .setContentText("Message")
                        .setStyle(new NotificationCompat.BigTextStyle()
                                .bigText("Text Text Text Text Text Text Text Text Text" +
                                        "Text Text Text TextText Text Text Text Text Text"+
                                        "Test Text Text TextText Text Text Text Text Text"))
                .build();
        NotificationManager notificationManager = (NotificationManager)
                getSystemService(Context.NOTIFICATION_SERVICE);
        

        notification.flags |= Notification.FLAG_AUTO_CANCEL;

// Play default notification sound
        notification.defaults |= Notification.DEFAULT_SOUND;
        notification.defaults |= Notification.DEFAULT_VIBRATE;
        notificationManager.notify(1, notification);
    }
```
