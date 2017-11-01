# android-notification-with-text-and-image

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
###### Notification with Image loading from server
```
private class sendNotification extends AsyncTask<String, Void, Bitmap> {

        Context ctx;
        String message;

        public sendNotification(Context context) {
            super();
            this.ctx = context;
        }

        @Override
        protected Bitmap doInBackground(String... params) {

            InputStream in;
            message = params[0] + params[1];
            try {

                URL url = new URL(params[2]);
                HttpURLConnection connection = (HttpURLConnection) url.openConnection();
                connection.setDoInput(true);
                connection.connect();
                in = connection.getInputStream();
                Bitmap myBitmap = BitmapFactory.decodeStream(in);
                return myBitmap;
            } catch (MalformedURLException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
            return null;
        }

        @RequiresApi(api = Build.VERSION_CODES.JELLY_BEAN)
        @Override
        protected void onPostExecute(Bitmap result) {

            super.onPostExecute(result);
            try {
                NotificationManager notificationManager = (NotificationManager) ctx
                        .getSystemService(Context.NOTIFICATION_SERVICE);

                Intent intent = new Intent(ctx, MainActivity.class);
                intent.putExtra("isFromBadge", false);


                // Set Image from url as large icon for notification 
               /* Notification notification = new Notification.Builder(ctx)
                        .setContentTitle(
                                ctx.getResources().getString(R.string.app_name))
                        .setContentText(message)
                        .setSmallIcon(R.mipmap.ic_launcher)
                        .setLargeIcon(result).build();*/
                        
                // set Image from url as bigPicture in notification
                Notification notification = new NotificationCompat.Builder(ctx)
                        .setContentTitle("Title")
                        .setContentText(message)
                        .setSmallIcon(R.mipmap.ic_launcher)
                        .setStyle(new NotificationCompat.BigPictureStyle()
                                .bigPicture(result).setSummaryText(message))
                        .build();

                // hide the notification after its selected
                notification.flags |= Notification.FLAG_AUTO_CANCEL;

                notificationManager.notify(3, notification);

            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ---------------------------------------------------    
    new sendNotification(context).execute(Title, Message, userImage);
```
