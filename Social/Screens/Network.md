
# Questions 
**1. How to retrieve a list of people you may know based on your recent activities?**

````sql
SELECT 
  users.user_id, 
  users.email, 
  users.name, 
  users.location, 
  users.role, 
  users.profile_pic, 
  users.phone_number
FROM 
  users
WHERE 
  users.user_id IN (
    SELECT 
      posts.user_id
    FROM 
      posts
    JOIN 
      activities ON posts.post_id = activities.post_id
    WHERE 
      activities.user_id = 1 AND 
      activities.activity_name = 'Like'
    ORDER BY 
      activities.activity_date DESC
  );
````

#### Answer:

| user_id | email               | name         | location     | role   | profile_pic             | phone_number |
| ------- | ------------------- | ------------ | ------------ | ------ | ----------------------- | ------------ |
| 1       | mentor1@example.com | Mentor One   | Los Angeles  | Mentor | profile_pic_mentor1.jpg | +12135551212 |
| 2       | mentor2@example.com | Mentor Two   | Chicago      | Mentor | profile_pic_mentor2.jpg | +13125550987 |
| 3       | mentor3@example.com | Mentor Three | Houston      | Mentor | profile_pic_mentor3.jpg | +17135554567 |
| 4       | mentor4@example.com | Mentor Four  | Philadelphia | Mentor | profile_pic_mentor4.jpg | +12675553210 |

**2. List of following/follower**
**Following**
````sql
SELECT 
  users.user_id, 
  users.email, 
  users.name, 
  users.role, 
  users.profile_pic, 
  users.phone_number
FROM 
  users
JOIN 
  follows ON users.user_id = follows.followee_id
WHERE 
  follows.follower_id = 1;
  
````
**Followers**
````sql
SELECT 
   users.user_id, 
  users.email, 
  users.name, 
  users.role, 
  users.profile_pic, 
  users.phone_number
FROM 
  users
JOIN 
  follows ON users.user_id = follows.follower_id
WHERE 
  follows.followee_id = 1;
````
#### Answer:
**Following data table**

| user_id | email                  | name      | role   | profile_pic             | phone_number |
| ------- | ---------------------- | --------- | ------ | ----------------------- | ------------ |
| 2       | mentor.two@gmail.com   | Jane Doe  | Mentor | profile_pic_mentor2.jpg | +13125550987 |
| 3       | mentor.three@gmail.com | Alice Lee | Mentor | profile_pic_mentor3.jpg | +17135554567 |

---
**Follower data table**

| user_id | email                  | name      | role   | profile_pic             | phone_number |
| ------- | ---------------------- | --------- | ------ | ----------------------- | ------------ |
| 2       | mentor.two@gmail.com   | Jane Doe  | Mentor | profile_pic_mentor2.jpg | +13125550987 |
| 3       | mentor.three@gmail.com | Alice Lee | Mentor | profile_pic_mentor3.jpg | +17135554567 |
| 4       | mentor.four@gmail.com  | David Kim | Mentor | profile_pic_mentor4.jpg | +12675553210 |

**3. List of connection**
````sql
SELECT u.*
FROM users u
JOIN Connection c ON u.user_id = c.User1ID OR u.user_id = c.User2ID
WHERE c.User1ID = 1 OR c.User2ID = 1;
````

#### Answer:

| user_id 	| email                  	| password       	| name       	| location    	| join_date           	| role   	| profile_pic             	| is_open_to_work 	| phone_number 	|
|---------	|------------------------	|----------------	|------------	|-------------	|---------------------	|--------	|-------------------------	|-----------------	|--------------	|
| 1       	| mentor.one@gmail.com   	| hashedpassword 	| John Smith 	| Los Angeles 	| 2024-06-12 08:00:00 	| Mentor 	| profile_pic_mentor1.jpg 	| 1               	| +12135551212 	|
| 2       	| mentor.two@gmail.com   	| hashedpassword 	| Jane Doe   	| Chicago     	| 2024-06-13 09:00:00 	| Mentor 	| profile_pic_mentor2.jpg 	| 0               	| +13125550987 	|
| 1       	| mentor.one@gmail.com   	| hashedpassword 	| John Smith 	| Los Angeles 	| 2024-06-12 08:00:00 	| Mentor 	| profile_pic_mentor1.jpg 	| 1               	| +12135551212 	|
| 3       	| mentor.three@gmail.com 	| hashedpassword 	| Alice Lee  	| Houston     	| 2024-06-14 10:00:00 	| Mentor 	| profile_pic_mentor3.jpg 	| 1               	| +17135554567 	|

