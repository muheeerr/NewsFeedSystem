News Feed System Design
Database Design
Entities
Entity (user, page, group etc): entityId, name, description, createdDatetime
Post: postId, title, text, authorId, createdDatetime
Attachment: attachmentId, postId, url, createdDatetime
Connections : followerId (entityId), followeeId (entityId), createdDatetime
Relationships
Follower-Followee: A User can follow other Users or Entities. 
Author-Post: Users and Entities can generate Posts. (authorId in post tbl as foreign key)
Post-Attachment: Each Post may have some associated Attachment/s. (1:n)
High-level Design
Workflows:
Post generation:
When User A ask for its news feed, the system will:
Get followees: retrieve the IDs of all the users/entities that User A follows
Get posts: retrieve latest Posts against followees IDs.
Rank posts: rank the posts based on relevance and time.
Cache: cache the feeds generated and return the top 20 posts to User
When User reaches the end of those first 20 posts, another request is sent to fetch the next 20 posts. (pagination)
Post publishing (Realtime updates)
Assume Ahmed follows Maaz, and Maaz sends a new post. The system will need to update Ahmed’s news feed:
Get followers: retrieve the IDs of Maaz's followers
Add posts: Add the post Maaz created to the news feed pool of those follower IDs.
Update Cache: update the ranked post into cache
Notify followers: let the follower know that there are new posts.
Components:
Web servers: maintains connections with the users. (realtime subscriptions to handle new updates and notification (assuming web), push notification services for mobile apps
Application server: executes the workflows mentioned above.
Database:
User/Entity: database table
Post: database table
Attachment (image/video/file): (uploaded on s3 bucket/azure blob,  URL stored in table)
connections :table that contains entities connections with each other




