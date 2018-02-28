# Configure Push Notifications with [Firebase](https://firebase.google.com/) and [Pusher](https://pusher.com/)

## Android app

- Create or open your Google Firebase Project

  https://console.firebase.google.com/

- Go to `Project settings` and `Add Firebase to your Android app`

Follow on-screen instructions specifying the package name of your android app

- Download `google-services.json` in project's root folder

- Copy `Server key` from `Settings` > `Cloud Messaging`

- Open Pusher Dashboard

  https://dash.pusher.com/

- Add push notifications to your Android app, pasting `Server key` copied before

- Go to `Keys` and using `Instance Id` and `Secret Key` you can send a test notification

```bash
curl -H "Content-Type: application/json" \
     -H "Authorization: Bearer <Secret Key>" \
     -X POST "https://<Instance Id>.pushnotifications.pusher.com/publish_api/v1/instances/<Instance Id>/publishes" \
     -d '{
      "interests": ["hello"],
      "fcm": {
        "notification": {
          "title": "Hello",
          "body": "Hello, world!"
        },
        "data": {
          "title": "Hello data",
          "body": "Hello, data world!",
          "action": "test 123"
        }
      }
     }'
```

or you can send a notification directly through Firebase

```bash
curl -X POST --header "Authorization: key=<Server key>" \
    --Header "Content-Type: application/json" \
    https://fcm.googleapis.com/fcm/send \
    -d '{
      "to": "/topics/ciao001",
      "notification": {
        "title": "Hello",
        "body": "Hello, world!"
      },
      "data": {
        "title": "Hello data",
        "body": "Hello, data world!",
        "action": "test 123"
      }
    }'

# or with data-only

curl -X POST --header "Authorization: key=<Server key>" \
    --Header "Content-Type: application/json" \
    https://fcm.googleapis.com/fcm/send \
    -d '{
      "to": "/topics/ciao001",
      "data": {
        "title": "Hello data",
        "body": "Hello, data world!",
        "action": "test 123"
      }
    }'
```
