============================================================
NGROK + JENKINS + GITHUB WEBHOOK SETUP (FULL PROCEDURE)
============================================================

====================
1. INSTALL & SETUP NGROK
====================

1. Regenerate your authtoken from:
   https://dashboard.ngrok.com/tunnels/authtokens

2. Copy your NEW authtoken.

3. Add token using full path (because ngrok is installed in WindowsApps):

"C:\Users\navee\AppData\Local\Microsoft\WindowsApps\ngrok.exe" config add-authtoken YOUR_NEW_TOKEN


============================
2. START NGROK TUNNEL FOR JENKINS
============================

Jenkins runs on: http://localhost:8080

Start ngrok:

"C:\Users\navee\AppData\Local\Microsoft\WindowsApps\ngrok.exe" http 8080

You will see:

Forwarding  https://xxxxx.ngrok-free.app -> http://localhost:8080

Copy the HTTPS URL.


=================================
3. CONFIGURE GITHUB WEBHOOK
=================================

Go to:
GitHub Repository → Settings → Webhooks → Add Webhook

Payload URL:
https://xxxxx.ngrok-free.app/github-webhook/

Content type:
application/json

Events:
Just the push event

Click "Add Webhook".
GitHub should show a green tick (✓ Delivered).


=================================
4. CONFIGURE JENKINS FREESTYLE JOB
=================================

Job → Configure → Build Triggers

Enable:
GitHub hook trigger for GITScm polling

Save.


=================================
5. CONFIGURE JENKINS PIPELINE JOB
=================================

Use this in Jenkinsfile:

pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('Build') {
            steps {
                echo "Webhook triggered"
            }
        }
    }
}


=========================
6. TEST THE WEBHOOK
=========================

GitHub:
Settings → Webhooks → Recent Deliveries → Redeliver

Expected results:
- GitHub: HTTP 200 OK
- Jenkins: Build starts


=========================
7. IMPORTANT NOTES
=========================

1. Ngrok URL changes every time (unless paid plan).
2. You must keep the ngrok terminal window open.
3. If ngrok closes, webhook stops.
4. Each time URL changes, update GitHub webhook.


============================================================
END OF README
============================================================
