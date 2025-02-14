# Systems Design of a "Special Social Network for Travelers (SSNT)" for a [systems design course](https://balun.courses/courses/system_design "systems design course")

## Functional requirements
- Users must be able to create and delete posts
- The post must contain at least one photo and a brief text message
- Post must contain a geo-point
- Users must be able to subscribe and unsubscribe from other users
- Users must be able to like or unlike posts and comments
- Users are allowed to leave and delete comments on the post
- Users must be able to view the feed of posts
- Posts are displayed in the feed in reverse chronological order
- Posts can be searched for by users using geo-point, rating, attendance, or words in the text message

## Non-functional requirements
- \> DAU 10 000 000
- The audience of CIS countries
- Data storage time: unlimited
- Seasonality
    - 3 seasonal peaks:
        - 1st peak: 1st of May - 1st of June
        - 2nd peak: 1st of September - 1st of October
        - 3rd peak: 1st of December - 1st of January
- Average activity per user
    - creates 20 posts per month (0.66 per day)
    - reviews own posts 3 times per day
    - searches for other posts 2 times per month (0.066 per day)
    - views the feed 3 times per day
    - leaves comments 2 per day
    - puts the like under posts 15 times per day
    - subscribes to other users 4 times per month (0.133 per day)
- Limits
    - Post
        - Photos
            - Count of photos up to 5 
        - Text message
            - up to 300 characters
            - 5 mb max size (resized to 192 Kb)
    - Comment
        - Text message up to 100 characters
    - Subscription
        - Unlimited
    - Like
        - Unlimited
    - Feed
        - Unlimited scroll
        - Preload 5 posts
    - Search
        - Unlimited scroll of results
        - Preload 5 results
- Timings
    - Post creation < 5 seconds
    - Post removal < 1 seconds
    - Comment creation < 3 seconds
    - Comment removal < 1 seconds
    - Subscribe/unsubscribe < 1 seconds
    - Search < 10 seconds
    - Get feed < 5 seconds
    - Like/unlike post or comment < 2 seconds
- Support for mobile devices and browsers

## Basic calculations
### RPS
    - Post
        - creation (write) = 10 000 000 * 0.66 / 86 400 = 76
        - put likes (write) = 10 000 000 * 15 / 86 400 = 1736
        - leave comments (write) = 10 000 000 * 2 / 86 400 = 231
        - get feed (read) = 10 000 000 * 3 * 5 / 86 400 = 1736
    - Comment
        - creation (write) = 10 000 000 * 0.133 / 86 400 = 15
        - get (read) = 10 000 000 * 0.133 / 86 400 = 15
    - Search
        - get results (read) = 10 000 000 * 2 / 86 400 = 231
    - Subscription
        - subscribe (write) = 10 000 000 * 0.133 / 86 400 = 15
    
    Total
        - write = 76 + 1736 + 15 + 15 = 1832
        - read = 1736 + 15 + 231 + 15 = 1987
### Data
    - Post
        - id = 0.1 Kb
        - created_at = 0.1 Kb
        - user_id = 0.1 Kb
        - photos = 5 * 192 Kb = 960 Kb
        - text message = 300 characters = 0.3 Kb
        - geo-point = 0.1 Kb

    Total: 0.1 + 0.1 + 0.1 + 960 + 0.3 + 0.1 = 960.6 Kb

    - Comment
        - id = 0.1 Kb
        - created_at = 0.1 Kb
        - user_id = 0.1 Kb
        - post_id = 0.1 Kb
        - likes_count = 0.1 Kb
        - text message = 100 characters = 0.1 Kb

    Total: 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 = 0.6 Kb

### Traffic
    - write = 960.6 * 1832 = 1 760 Mb/s
    - read = 960.6 * 1987 = 1 888 Mb/s

### Connections
    10 000 000 * 0.1 = 1 000 000