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
* 4 bytes per character

### Messages
* Incoming RPS
```
DAU = 10 000 000
read rps = 10 000 000 / 86400 * 100 = 11574
write rps = 10 000 000 / 86400 * 50 = 5787
```
* Incoming traffic

```
message size = 2000 * 4b + 3 * 1.75Mb = 5.25 Mb
read traffic = 5.25Mb * 5785 rps = 59 Gb/s
write traffic = 5.25Mb * 11574 rps = 29.6 Gb/s
```

* Storage Size for one year
```
Storage size = 29.6 Gb/s * 86400 * 365 = 911587.5 Tb = 890 Pb
```

### Posts
* Incoming RPS
```
DAU = 10 000 000
read rps = 10 000 000/86400 * 40 = 4629
write rps for post = 10 000 000/86400 * 1 = 115
write rps for comment = 10 000 000/86400 * 1 = 115
```

* Incoming traffic
```
post size = 2000 * 4b + 6 * 1.75Mb = 10.5Mb
read traffic = 10.5Mb * 4629 = 47 Gb/s
write traffic = 1.1 Gb/s
```

* Storage size for one year
```
Storage size = 1.1Gb/s * 86400 * 365 = 33 Pb
```