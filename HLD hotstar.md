**Functional requirement**

1) upload the video 
2) streaming video like movies , shows.
3) live streaming like cricket match
4) resume from where we left
5) skip add , skip introduction
6) audio language and subtitle
7) mutiple resolution/formats of videos
8) auto video quality
9) video speed

non functional requirements

here we need high available system for good user expericence and also need low latency.
in live streaming there should be no buffering.

BOE calculation

Storage and network bandwidth

average daily users = 1 billion
% os user upload the video= 1 % = 10 M
average per video size= 1 GB
so total storage daily= 1*10 M= 10000000 GB=10000 TB= 10 PB/day
in five years= 5*400*10 =20000 PB in five years

network bandwidth
average daily users = 1 billion
average user watching daily=15 mins
size of video per min=50 MB
total data transfer per day=15*50 *10^6 Bytes *10^9(users)=1000*10^6 gb=10^9 GB day= 10^9/10^5 gb/sec=10000 gb/sec=10 TB/second

API

1)  POST /api/v1/upload{file,metadata}
   request {
  "file": "video.mp4",
  "title": "My New Movie",
  "description": "An exciting new movie",
  "language": "English",
  "genre": ["Action", "Thriller"],
  "thumbnail": "thumbnail.jpg",
  "release_date": "2025-03-10",
  "tags": ["Hollywood", "New Release"],
  "visibility": "public"
}

response-
{
  "status": "success",
  "message": "Video uploaded successfully",
  "video_id": "abc123xyz",
  "title": "My New Movie",
  "thumbnail_url": "https://cdn.ott-platform.com/thumbnails/abc123xyz.jpg",
  "video_url": "https://cdn.ott-platform.com/videos/abc123xyz.m3u8",
  "encoding_status": "processing",
  "resolution_available": ["360p", "720p", "1080p"],
  "upload_time": "2025-03-04T12:30:00Z",
  "visibility": "public",
  "cdn_provider": "Akamai"
}
2) GET /api/v1/video/play/{video_id}
3) continue_watching(video_id,user_id)
4) skipvideo(video_id,timestamp)
5) skipCurrentScene(video_id,timestamp)


**Deep Dive**

upload video

server sent the pre signed url afer storing the info of metadata and after that user upload the video through presinged url.
so it saves the badnwidth at two places and it uses ony to uplaod the video in storage.

post processing steps after video upload
1) copyright checks
2) save in differnt reslutions
3) chunking of video
4) change the video status to post processing to success

video streaming

FE sends the request to server to get the metadata so we get the streaming url of cdn and failover cdn url after that request goes to cdn 
url and it downloads few chunks and before finishing the 1 chunk it download few more chunks 

live streaming
The camera captures a live match and sends the feed to Hotstar’s encoding server.
The server transcodes the video in real-time to multiple resolutions (360p, 720p, 1080p).
The encoded chunks are sent to the CDN dynamically.
The player keeps refreshing the .m3u8 file every few seconds to get new chunks.
there is little bit lwo latency 3-10 seconds.


Most OTTs do exactly the above by using Adaptive bitrate streaming (ABS). ABS works by
detecting the user's device and internet connection speed and adjusting the video quality and bit
rate accordingly.

CDN Fetch & Cache: 
CloudFront (or Akamai) fetches chunks from S3 and caches them at edge locations.
5️⃣ User Playback: The player downloads the .m3u8 file (metadata) from the CDN and fetches video chunks.

✅ S3 is the origin storage, but CDN is used for fast playback.
