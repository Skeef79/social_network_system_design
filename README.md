# TravelHub - System Design
Homework for [System Design course](https://balun.courses/courses/system_design). TravelHub is a social network for travelers, where they can connect and share their experience. 

## Functional requirements
### Posts
* create post with a possibility to add from 1 to 6 photos. Post has a description, timestamp and geolocation
* Archive posts with a possibility to recover
* Watch feed (list of posts)
* Watch and add comments to posts
* Like posts
* Search for most popular places to travel - top countries/cities by geo-location
* Watch posts from most populare places

### Profile
* Authorization
* Edit profile
* Subscribe for other users
* Stats in profile: number of posts, subcribers and subscriptions

### Messages
* Direct messages support
* Notification about new message
* Message status - read or unread
* Watch a list of incoming messages from user

## Non-functional requirements
* 10 000 000 DAU after one year, linear growth
* availability 99.9%
* read/write stats
    - each user make 1 post per day 
    - each user reads 40 posts per day
    - each user reads 100 messages per day
    - each user writes 50 messages per day
    - each user writes 1 comment per day
* response time
    - messages: 500 ms
    - posts: 10s

* geo-distributed: CIS

* data policy
  - posts are saved forever
  - maximum of 6 photos per post
  - maximum of 3 photos per message
  - maximum of 2000 characters per post description
  - maximum of 2000 characters per message
  - maximum of 500 characters per comment

## Load/Storage calculations
Calculations for 1 year

* Picture size 1280 x 720 with 16-bit depth (1.75 Mb)
* 3 bytes per character

### Posts
* Incoming RPS
```
DAU = 10 000 000
read rps = 10 000 000/86400 * 40 = 4629
write rps for post = 10 000 000/86400 * 1 = 115
```

* Incoming traffic for text
```
DAU = 10 000 000
text size = 3000 * 3 B = 6000b = 6KB
read traffic = 4629 * 6 KB = 27 MB/s
write traffic = 115 * 6 KB = 0.7 MB/s
```

* Incoming traffic for images
```
post size = 6 * 1.75 MB = 10.5 MB
read traffic = 10.5 MB * 4629 = 47 GB/s
write traffic = 1.1 GB/s
```

* Storage size for images (one year)
```
Storage size = 1.1 GB/s * 86400 * 365 = 33 PB
```

* Storage size for text (one year)
```
Storage size = 0.7 MB/s * 86400 * 365 = 21 TB
```
