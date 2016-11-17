---
layout: post
title: "How to Stream Events Live on YouTube (using nothing but an iPad)"
date:   2015-02-27 11:40:00 -0500
categories: video streaming youtube
---
Over the past year, I’ve been broadcasting youth hockey games live through YouTube using nothing but a tripod and an ipad (with tripod mount). The process was not exceptionally simple to set up but it’s effective and lightweight. Note that there is an iPad app that costs $20 / month to stream videos longer than 30 minutes so there is a cost. It’s been well worth it.  

Here’s how I did it:

### Step 1: Configure Youtube Account

- Go to YouTube.com
- Click “Sign In”
- Either log into a Google account or click “Create Account”
- Once logged in, click the profile icon in the upper right of the screen, then click “Creator Studio”

![Creator Studio](/assets/youtube-creator-studio.png)

You’ll arrive at a Dashboard screen for Creator Studio.

- Click to open “Video Manager” and then click “Live Events”

![Video Manager](/assets/youtube-video-manager.png)

If this is your first time doing a Live Event, you’ll need to click to enable Live Events. (There’s a big blue button in the center of the screen).

The YouTube system may go through a verification process using either a voice phone call or text message. Complete it and you’re ready for step 2.

### Step 2: Add an Event

- From the Live Events page, click “Create live event.”
- Enter the title, the date and time of the event. I usually change “Public” to “Unlisted” for the video because we embed the games in Mylights (http://mylights.io) so the moments that matter can be tagged and shared. Make sure to change Type from “Quick” to “Custom.” Then click to save.

![Create Live Video](/assets/youtube-create-live-video.png)

Next, we’ll configure the Ingestion Settings. You’ll automatically be placed on that screen once you initially save to create the event.

- Select “Custom Ingestion” under “Choose maximum sustained bitrate of your encoder”
- Then, select “Create new stream” from the “Select a stream” drop-down. We’re going to create a saved stream for your iPhone or iPad.

![Ingestion Settings](/assets/youtube-ingestion-settings.png)

- On the pop-up “Create new stream” screen, enter a name for the stream, e.g. “My iPad” and select a Maximum sustained bitrate of “500 Kbps - 2000 Kbps (480p)” from the drop-down. Save changes.

![Create stream](/assets/youtube-create-stream.png)

- Now select “Other encoders” from the “Select your encoder” drop-down list. The system will provide settings for your encoder (the iPad/iPhone). I would recommend printing this page so you have something to reference when we configure the iPad app.

![Encoder Settings](/assets/youtube-encoder-settings.png)

- Save changes and we’re ready for Step 3.

### Step 3: Install and Configure iOS App

I’ve tried a number of apps for streaming live including the ones like UStream (um, if you’re going to insert a commercial into my broadcast, at least have the decency to pause the video so viewers don’t miss anything), LiveStream, etc. They all require their proprietary video hosting service which seemed to run into trouble. The app I’ve used with success is [Studio Switcher Basic](https://itunes.apple.com/us/app/switcher-studio-basic/id783007942?mt=8). Here’s how to install and configure it for broadcasting in YouTube.

- Obviously, install the app. Once you have it installed, open it. In the bottom right corner, there are five buttons. Tap on the one that looks like a satellite dish.
- Under Live Broadcasting Service (CDN), tap “RTMP” to select it and then tap the “i” button.
- Tap the plus (+) sign on the right.

- From the RTMP Parameters window, enter the following information:

  - Channel Name: (Whatever you want, e.g. “My YouTube Channel”)
  - Server URL: (That’s on the page we printed in the previous step as “Primary Server URL”, e.g. - rtmp://a.rtmp.youtube.com/live2
  - Stream: (That’s also on the page we printed as “Stream Name”)
  - Video Format: 960 x 540
  - Video Bitrate: 700 kbps (This one turned out to be important)
  - Audio Format: Default value
  - Audio Bitrate: Default value
  - Tap “OK”

Make sure the channel you created is now selected. (It should be by default). We’re ready to broadcast.

One note…once you do a test broadcast and you’re happy with the approach, you’ll definitely want to “Upgrade” within the app which will allow you to broadcast more than 30 minutes. It’s just an iTunes subscription transaction which is very simple through the app.

### Step 4: Broadcast!

- If you’re not already there, go to your Live Events page from “Creator Studio” in YouTube. Click “Live Control Room” for your Live Event.

![Live Control Room Button](/assets/youtube-live-control-room-button.png)

The system will have a big red bar across the top of the screen saying that it’s not receiving anything. No kidding, we haven’t told the encoder (iPad app) to start broadcasting.

![Live Control Room](/assets/youtube-live-control-room.png)

- Back to your iPad, open the Studio Switch Basic app.
- Assuming our settings are in place from our configuration time, tap the Record + Broadcast button in the upper right of the screen.
- Then wait…it will take a up to a minute for the broadcast to show up in your Live Control Room screen.

When it does, going live is a two step process.
- First, you’ll “Preview” (required by YouTube) and then you’ll “Start Streaming.”
- When data is received from the encoder, the “Preview” button will be enabled in the Live Control Room screen.

![Preview](/assets/youtube-preview.png)

- Click Preview and confirm.
- Then wait 30-60 seconds for the “Start Streaming” button to be enabled. Also note that the stream status is “GOOD”.
- Once the “Start Streaming” button is enabled, click it and confirm.

**You’re now live!**

![Start Streaming](/assets/youtube-start-streaming.png)

When you’re done broadcasting, tap the Rec + Broadcast button to turn it off. Wait a full 60 seconds and then click “Stop Streaming” from YouTube on the computer. The broadcast is 30-60 seconds behind what you’re seeing on your iPad so if you click too early, people will miss the final bit.

### Couple of Additional Notes

- The broadcast can go from GOOD to OK to NO DATA depending on the consistency of the Internet connection.
- I’ve found that the most consistent broadcast Internet is actually through my cellular data when it gets an LTE data connection. Wireless networks in rinks are just not great for the most part. (Kettler Capitals Iceplex is an exception but it has better amenities than most rinks in North America).
- Occassionally the iPad App will either lock up or crash. Don’t panic…double-tap the home button, close the app. Then open it again and tap “Rec + Broadcast” - the broadcast will pick back up
- The audio is actually pretty good coming out of the iPad. Don’t assume you have to use a microphone or headset. I broadcast for most of the past year just talking into the ipad as I moved the camera. (Some would argue the audio was better than with a headset).
- Here’s a link to the tripod mount I’ve used for my [iPad Mini](https://www.amazon.com/gp/product/B00B0GSADW)
