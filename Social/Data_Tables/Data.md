# Data sample
**Users Table**
| user_id 	| email                  	| password       	| name           	| location     	| join_date           	| role   	| profile_pic             	| is_open_to_work 	| phone_number 	|
|---------	|------------------------	|----------------	|----------------	|--------------	|---------------------	|--------	|-------------------------	|-----------------	|--------------	|
| 1       	| mentor.one@gmail.com   	| hashedpassword 	| John Smith     	| Los Angeles  	| 2024-06-12 08:00:00 	| Mentor 	| profile_pic_mentor1.jpg 	| 1               	| +12135551212 	|
| 2       	| mentor.two@gmail.com   	| hashedpassword 	| Jane Doe       	| Chicago      	| 2024-06-13 09:00:00 	| Mentor 	| profile_pic_mentor2.jpg 	| 0               	| +13125550987 	|
| 3       	| mentor.three@gmail.com 	| hashedpassword 	| Alice Lee      	| Houston      	| 2024-06-14 10:00:00 	| Mentor 	| profile_pic_mentor3.jpg 	| 1               	| +17135554567 	|
| 4       	| mentor.four@gmail.com  	| hashedpassword 	| David Kim      	| Philadelphia 	| 2024-06-15 11:00:00 	| Mentor 	| profile_pic_mentor4.jpg 	| 0               	| +12675553210 	|
| 5       	| mentee.one@gmail.com   	| hashedpassword 	| Sarah Jones    	| Phoenix      	| 2024-06-16 12:00:00 	| Mentee 	| profile_pic_mentee1.jpg 	| 1               	| +16025557890 	|
| 6       	| mentee.two@gmail.com   	| hashedpassword 	| William Miller 	| San Antonio  	| 2024-06-17 13:00:00 	| Mentee 	| profile_pic_mentee2.jpg 	| 0               	| +12105556543 	|
| 7       	| mentee.three@gmail.com 	| hashedpassword 	| Olivia Brown   	| San Diego    	| 2024-06-18 14:00:00 	| Mentee 	| profile_pic_mentee3.jpg 	| 1               	| +16195552345 	|
| 8       	| mentee.four@gmail.com  	| hashedpassword 	| Charles Garcia 	| Dallas       	| 2024-06-19 15:00:00 	| Mentee 	| profile_pic_mentee4.jpg 	| 0               	| +14695559876 	|
| 9       	| mentee.five@gmail.com  	| hashedpassword 	| Emily Martinez 	| Phoenix      	| 2024-06-20 16:00:00 	| Mentee 	| profile_pic_mentee5.jpg 	| 1               	| +16235551234 	|
| 10      	| mentee.six@gmail.com   	| hashedpassword 	| David Smith    	| Philadelphia 	| 2024-06-21 17:00:00 	| Mentee 	| profile_pic_mentee6.jpg 	| 0               	| +13525557689 	|

**Profiles Table**
| profile_id 	| user_id 	| current_position          	| skills                         	| experience                  	| education                  	| Industry        	|
|------------	|---------	|---------------------------	|--------------------------------	|-----------------------------	|----------------------------	|-----------------	|
| 1          	| 1       	| Senior Software Developer 	| JavaScript, Python             	| 10 years at TechCorp        	| BSc Computer Science       	| Technology      	|
| 2          	| 2       	| HR Manager                	| Recruiting, Employee Relations 	| 5 years at HR Inc.          	| MA Human Resources         	| Human Resources 	|
| 3          	| 3       	| Senior Data Scientist     	| Python, R, SQL                 	| 8 years at DataCorp         	| MSc Data Science           	| Data Science    	|
| 4          	| 4       	| Senior HR Manager         	| Recruiting, Employee Relations 	| 7 years at HR Solutions     	| MA Human Resources         	| Human Resources 	|
| 5          	| 5       	| Junior Software Developer 	| JavaScript                     	| 1 year at StartUp           	| BSc Computer Science       	| Technology      	|
| 6          	| 6       	| HR Associate              	| Recruiting                     	| 2 years at HR Inc.          	| BA Human Resources         	| Human Resources 	|
| 7          	| 7       	| Junior Data Analyst       	| Python, SQL                    	| 1 year at DataStart         	| BSc Data Science           	| Data Science    	|
| 8          	| 8       	| Office Assistant          	| Organizational Skills          	| 2 years at Office Solutions 	| BA Business Administration 	| Administration  	|
| 9          	| 9       	| Marketing Director        	| Advertising, Brand Management  	| 12 years at MarketGurus     	| MBA Marketing              	| Marketing       	|
| 10         	| 10      	| Civil Engineer            	| Project Management, CAD        	| 6 years at BuildRight       	| MEng Civil Engineering     	| Construction    	|

**Posts Table**
| post_id 	| user_id 	| content                                             	| post_date           	| views 	| likes 	| shares 	| comments 	|
|---------	|---------	|-----------------------------------------------------	|---------------------	|-------	|-------	|--------	|----------	|
| 1       	| 1       	| Excited to start my new project!                    	| 2024-06-10 10:00:00 	| 150   	| 25    	| 5      	| 10       	|
| 2       	| 2       	| Looking for talented software developers.           	| 2024-06-11 11:00:00 	| 200   	| 40    	| 10     	| 15       	|
| 3       	| 3       	| How do we know?                                     	| 2024-06-11 11:00:00 	| 400   	| 20    	| 20     	| 55       	|
| 4       	| 4       	| What is love?                                       	| 2024-06-11 11:00:00 	| 500   	| 30    	| 20     	| 35       	|
| 5       	| 1       	| Just published a new research paper!                	| 2024-06-20 10:00:00 	| 300   	| 60    	| 15     	| 20       	|
| 6       	| 2       	| Hiring for several new roles.                       	| 2024-06-21 11:00:00 	| 250   	| 50    	| 12     	| 18       	|
| 7       	| 3       	| Presenting at the upcoming data science conference. 	| 2024-06-22 12:00:00 	| 350   	| 70    	| 17     	| 22       	|
| 8       	| 4       	| Promoted to Senior HR Manager!                      	| 2024-06-23 13:00:00 	| 275   	| 55    	| 13     	| 19       	|
| 9       	| 5       	| Completed my first project!                         	| 2024-06-24 14:00:00 	| 100   	| 20    	| 5      	| 10       	|
| 10      	| 6       	| Attended a great recruiting workshop.               	| 2024-06-25 15:00:00 	| 125   	| 25    	| 6      	| 12       	|
| 11      	| 7       	| Learned a lot from my first data analysis project.  	| 2024-06-26 16:00:00 	| 150   	| 30    	| 7      	| 14       	|
| 12      	| 8       	| Enjoying my work as an office assistant.            	| 2024-06-27 17:00:00 	| 175   	| 35    	| 8      	| 16       	|

**PlatformRating Table**
| rating_id | user_id | rating | comment                  | rating_date         |
|-----------|---------|--------|--------------------------|---------------------|
| 1         | 1       | 5      | Great platform!          | 2024-06-10 10:00:00 |
| 2         | 2       | 4      | Good experience overall. | 2024-06-11 11:00:00 |

**Tag Table**
| tag_id | tag_name                | usage_count | last_used           |
|--------|-------------------------|-------------|---------------------|
| 1      | Software Development    | 100         | 2024-06-10 10:00:00 |
| 2      | Human Resources         | 50          | 2024-06-11 11:00:00 |
| 3      | Marketing               | 75          | 2024-06-12 14:00:00 |
| 4      | Data Science            | 120         | 2024-06-13 16:00:00 |
| 5      | Machine Learning        | 85          | 2024-06-14 15:00:00 |
| 6      | Artificial Intelligence | 90          | 2024-06-15 17:00:00 |
| 7      | Cybersecurity           | 80          | 2024-06-16 18:00:00 |
| 8      | Cloud Computing         | 95          | 2024-06-17 19:00:00 |
| 9      | Web Development         | 110         | 2024-06-18 20:00:00 |
| 10     | Mobile Development      | 105         | 2024-06-19 21:00:00 |

**Challenges Table**
| ChallengeID | UserID | Name                | Location        | Duration | StartTime           | Category    | Description                        | Phase           |
|-------------|--------|---------------------|-----------------|----------|---------------------|-------------|------------------------------------|-----------------|
| 1           | 1      | Catch Rare Pokémon  | Viridian Forest | 90min    | 2024-06-13 16:00:00 | Adventure   | Find elusive Pokémon in the forest | Exploration     |
| 2           | 2      | Win Pokémon Contest | Cerulean City   | 90min    | 2024-06-13 16:00:00 | Competition | Show off your Pokémon skills       | Performance     |
| 3           | 3      | Defeat Elite Four   | Indigo Plateau  | 90min    | 2024-06-13 16:00:00 | Challenge   | Prove your strength in battles     | Final Challenge |
| 4           | 4      | Complete Pokédex    | Pallet Town     | 90min    | 2024-06-13 16:00:00 | Collection  | Capture all Pokémon species        | Completion      |

**Group**
| group_id 	| group_name          	| description                                                                 	|
|----------	|---------------------	|-----------------------------------------------------------------------------	|
| 1        	| Software Developers 	| A group for software developers to share ideas and collaborate on projects. 	|
| 2        	| Data Scientists     	| A group for data scientists to discuss the latest trends in data science.   	|
| 3        	| Web Designers       	| A group for web designers to share their designs and get feedback.          	|
| 4        	| HR Professionals    	| A group for HR professionals to discuss the latest trends in HR.            	|
| 5        	| Marketing Experts   	| A group for marketing experts to share strategies and success stories.      	|
| 6        	| AI Enthusiasts      	| A group for AI enthusiasts to discuss the latest developments in AI.        	|

**User Group**
| user_id 	| group_id 	| join_date           	|
|---------	|----------	|---------------------	|
| 1       	| 1        	| 2024-06-20 10:00:00 	|
| 3       	| 2        	| 2024-06-21 11:00:00 	|
| 2       	| 2        	| 2024-06-22 12:00:00 	|
| 4       	| 2        	| 2024-06-23 13:00:00 	|
| 1       	| 2        	| 2024-06-24 14:00:00 	|
| 6       	| 2        	| 2024-06-25 15:00:00 	|
| 7       	| 2        	| 2024-06-26 16:00:00 	|
| 8       	| 3        	| 2024-06-27 17:00:00 	|

**Follows**
| follow_id 	| follower_id 	| followee_id 	| follow_date         	|
|-----------	|-------------	|-------------	|---------------------	|
| 1         	| 1           	| 2           	| 2024-06-20 10:00:00 	|
| 2         	| 1           	| 3           	| 2024-06-21 11:00:00 	|
| 3         	| 2           	| 1           	| 2024-06-22 12:00:00 	|
| 4         	| 3           	| 1           	| 2024-06-23 13:00:00 	|
| 5         	| 4           	| 1           	| 2024-06-24 14:00:00 	|

**Connection**
| ConnectionID 	| User1ID 	| User2ID 	|
|--------------	|---------	|---------	|
| 1            	| 1       	| 2       	|
| 2            	| 1       	| 3       	|
| 3            	| 2       	| 3       	|

**Job**
| user_id 	| email                  	| password       	| name           	| location     	| join_date           	| role   	| profile_pic             	| is_open_to_work 	| phone_number 	|
|---------	|------------------------	|----------------	|----------------	|--------------	|---------------------	|--------	|-------------------------	|-----------------	|--------------	|
| 1       	| mentor.one@gmail.com   	| hashedpassword 	| John Smith     	| Los Angeles  	| 2024-06-12 08:00:00 	| Mentor 	| profile_pic_mentor1.jpg 	| 1               	| +12135551212 	|
| 2       	| mentor.two@gmail.com   	| hashedpassword 	| Jane Doe       	| Chicago      	| 2024-06-13 09:00:00 	| Mentor 	| profile_pic_mentor2.jpg 	| 0               	| +13125550987 	|
| 3       	| mentor.three@gmail.com 	| hashedpassword 	| Alice Lee      	| Houston      	| 2024-06-14 10:00:00 	| Mentor 	| profile_pic_mentor3.jpg 	| 1               	| +17135554567 	|
| 4       	| mentor.four@gmail.com  	| hashedpassword 	| David Kim      	| Philadelphia 	| 2024-06-15 11:00:00 	| Mentor 	| profile_pic_mentor4.jpg 	| 0               	| +12675553210 	|
| 5       	| mentee.one@gmail.com   	| hashedpassword 	| Sarah Jones    	| Phoenix      	| 2024-06-16 12:00:00 	| Mentee 	| profile_pic_mentee1.jpg 	| 1               	| +16025557890 	|
| 6       	| mentee.two@gmail.com   	| hashedpassword 	| William Miller 	| San Antonio  	| 2024-06-17 13:00:00 	| Mentee 	| profile_pic_mentee2.jpg 	| 0               	| +12105556543 	|
| 7       	| mentee.three@gmail.com 	| hashedpassword 	| Olivia Brown   	| San Diego    	| 2024-06-18 14:00:00 	| Mentee 	| profile_pic_mentee3.jpg 	| 1               	| +16195552345 	|
| 8       	| mentee.four@gmail.com  	| hashedpassword 	| Charles Garcia 	| Dallas       	| 2024-06-19 15:00:00 	| Mentee 	| profile_pic_mentee4.jpg 	| 0               	| +14695559876 	|
| 9       	| mentee.five@gmail.com  	| hashedpassword 	| Emily Martinez 	| Phoenix      	| 2024-06-20 16:00:00 	| Mentee 	| profile_pic_mentee5.jpg 	| 1               	| +16235551234 	|
| 10      	| mentee.six@gmail.com   	| hashedpassword 	| David Smith    	| Philadelphia 	| 2024-06-21 17:00:00 	| Mentee 	| profile_pic_mentee6.jpg 	| 0               	| +13525557689 	|
