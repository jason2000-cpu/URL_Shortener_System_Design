## URL Shortener
## 1 Purpose
Our plan is to build a url shortener service that shortens lengthy urls and simplifies resource sharing. It’s a potential marketing tool to help brands and individuals engage with their customers and audience. Although there are many implementations of url shortener’s our’s is customized to the african market needs.

## 2 Design Goals
We chose 5 metrics to gauge our design outcome, as follows:

1. Usability -  80% of users should be able to copy and paste lengthy urls and get a shorter link.
2. Customer Experience - Make lengthy urls shareable and easy to read. People dislike reading lengthy material.
3. Visual Appeal - Create an aesthetic and authentic look that evokes emotion. Colors, Fonts and Overall visual balance of the design should stay consistent across the entire application
4. Engagement - Track link success through the analysis of the user click rate. Shorter urls tend to increase click through rates across social media channels.
5. Cost - Make the service affordable to any potential user of our application through free subscription and effective price plans.

## 3. Definitions, Acronyms and Abbreviations
Url - Uniform Resource Locator. Describes a path to a file stored somewhere on the server.

## 4. Audience
The intended users for our url shortening service are :

1. Content Creators.
2. Marketers.
3. Authors and Bloggers
4. Brands and Businesses.
5. Internet users.


## 5. Case Study: Bitly Overview Summary (URL Shortening services)

We took upon the challenge of building a web application that enables users to shorten lengthy urls. Existing services i.e Bitly, Tiny Url and Goo.gl are effective but are doubtful of whether they’ve put African business needs into perspective. Our solution aims to tackle this and solely focus on the African market.

The application design will include an entire process of pasting urls, custom branding urls, analysing urls, enabling fast redirects and generation of QR codes. In addition to making our service scalable, all our design goals should be met.

## 6 User Types

1. Individual users are people in need of url shortening services. Registered users have a user account and a dashboard. Non registered users can still shorten their urls without logging in the system via the landing page.
2. Businesses represent a single established entity using the service for brand related and engagement purposes. Businesses subscribed to the service can customise their links to fit their brands theme and purpose. I.e https://safaricom/mpesa


## 7 Product Functions
The main product functions are as follows:

1. **Authentication Service** - enables user authentication and authorisation. Each user is assigned a token while their data is persisted in the database.


2. __API Gateway__ - routes traffic according to the given path.

3. **Shortening Service** - responsible for shortening the lengthy urls, redirecting to the original URL and keeping the count of urls shortened by the user. Generates a shortened url by the help of the hash service which maps a hash to its original url.

4. **Hash Service** - manages a collection of pre-created and used hashes.

5. **Scheduler Service** - generates new hashes when the number of existing hashes in the database is low. In charge of recycling new hashes on the expiration date.

## 8. Design and Implementation Considarations
### 8.1 System Design

1. **Microservice Architecture** - The backend is split into different services which function independently thus enabling modularity. It also allows for the separation of logic and scaling of each service according to traffic needs.
2. **Hashes** - Generate a unique hash tokens for each url to enable user customization. 
3. **Token generation** - Use of the Hashid library to generate unique tokens. Hashids create short, unique ids from numbers 0-9 and custom alphabets.

E.g


  | **HashId input**      | Token Generated | Test Text     |
| :---        |    :----:   |          ---: |
| 1      | ​l6o3k1n       |    https://twitter.com/​​l6o3k1n  |
| 


4. Encoding the short URL Algorithms - Base 36 or Base 62 encoding. In base 62 encoding, we use a-z,A-Z and 0-9 in the characters.
Storage Estimates -  400 million new urls per month * 12 * 4 years = 19.2 billion urls. 
5. Length of Token and the character it contains - Small characters a-z and numbers in the range of 0 and 9 are considered safe, which results in a character set of 36 in total. 

The table below describes the number of tokens that can be generated using tokens of various lengths.

A typical short url takes this format: https://ts.bv/qp37hi. Ts.bv -represents an  application, service or company. The last part is the token, a unique random identifier.


  | Token Length      | Unique Token range combo | Range    |
| :---        |    :----:   |          ---: |
| 6     | 2,176,782,336      |    >2 billion  |
|  7 | 78,364, 164,096 | > 78 billion|
|8 | 2,821,109,907,456 | > 2 trillion|
|9 | 101,559,956,668,416 | 100 trillion|


### 8.2 Tech Stack

1. Angular - Front end development of the application.
2. Spring Boot - Development of the micro-services.
3. Spring cloud gateway - For effective api routing, security and monitoring.
4. Spring cloud openfeign - Enables cross-service communication.
5. Spring Security - Secure the api service.
6. Spring cloud kubernetes - Deploy the backend application on the cloud.


## 9. Functional Requirements
1. Service should be able to create unique shortened url/links against a long url
 a.  One shortened url should be different from another
2.  Click the short url, Redirect user to the longer url
3. Generation of short urls (in terms of length size) as possible
4.  Branded link export: Creation of custom url with a maximum of 16 characters:

    - Numbers 0-9 or alphabetical letter to a-z
5. Analytics:

    - collect metrics like the number of clicks on the short url
    - Most clicked short url

6. Original and Shortened url stays in the system for a period of 4 years
7. Shortened url expires after a period of 2 years of inactivity
8. Updation and deletion of a shortened url
9. Hash url stored in the form of key value pairs
10. Link search for the shortened links
11. Embed a shortened link in a QR code


## Non-Functional Requirements

1. Service should be up and running all the time.
2. Response time: Fast URl redirection and should not degrade at any point.
3. Random short URls
4. Unique short urls
5. Availability of shortened url: 4 years since its creation



##  11. Existing System Architecture
 


![Existing System Architecture](https://drive.google.com/uc?id=1xzZ3-9rlmY07GcmhY1pwBmN0jbmctbcI)


 - New nodes added in the cluster talk to the zookeeper service.
- Each application node has to  requests for a unique number range from the zookeeper.
- The load balancer receives the request to shorten the long url. Workload is placed on the shortening service which works on a first come first served basis, turn by turn. (round-robin scheduling)
- An entry for an existing short url that points to the long url is checked in the database.  A response status code **HTTP 200 OK** is given for an existing entry that is not expired.
- For a Url not available in the database, the shortening service uses the next number range allocated by the zookeeper service and a new token is generated with the hashid library. The generated short url with a set expiry date is saved to the database. The short URL is return to the client with the status code of **HTTP 201 CREATED.**
- When the user eventually clicks the short url, the load balancer receives the request and passes it to one of the URL shortening application nodes.
- The shortening service communicates to the database to find an entry for the long url and if it exists. A redirection to the long url occurs with a status code of **HTTP 302**
- If no token for the long url is non existent, the application responds with a status of **HTTP 404 NOT FOUND.**


## 12. System Design Diagram

![System Design Diagram](https://drive.google.com/uc?id=1VmgrqJ6HXOp3LM635us9g88gB5n6qDyz)



- ZZookeeper is a configuration management distributed system that manages different servers enrolled to it.

**Zookeeper Use Cases**
- A user may upload a long url which goes through a load balancer to determine which server to assign the payload to.
- Let’s say server 2 is free to accept a payload. It get’s a request to increment it’s counter, which ranges between 2 million and 3 million and assigns a value to the request. The request goes through the base 62 algorithm and the result is a 7 character key.
- Zookeper ensures that each server has a particular counter range, with no duplicate keys or database collisions.
- If Server 3 goes down, it’s entry is removed from the keeper service and set to unassigned. Server 4 becomes assigned to the system with a new range.This way, we are able to scale in case of down time.


##  13. High Level Design (URL Shortener Service)

![HLD (url Shortener Service)](https://drive.google.com/uc?id=19dpvigx6YsKXVnAKKC_t9Lf6AiXzGz9Y)




1. Service accepts a long URL as a HTTP post request.
2. Service checks if the long url already exists in the database. If the long url already exists, a short url is constructed using the token and sent in the response.
3. If the long url doesn’t exist a new token for the long url and saves the token and long url entry into the database.
4. Service responds with status of **201 CREATED**, else **200 OK**, if the token already exists.

## 14. High Level Design URL Shortener redirection

![HLD URL Shortener Redirection](https://drive.google.com/uc?id=1tdE7_8V0SmjtDnuizovdHswJt8RCX1_H)





1. Service receives a short URL GET request like https://meliora/as7du98
2. Service checks if the token exists in the database. If it doesn’t exist, then a **HTTP 404** not found is given as a response.
3. For an existing token the corresponding entry is given. A response status of **HTTP 498** is returned for an expired token
4. Service responds with a long URL in the LOCATION HEADER, with a **HTTP 302 redirect** status.

## 14. Backend Architecture

![Backend Architecture](https://drive.google.com/uc?id=1283PE3Y3KNoqq9Lv-bqhDGqIzcF5jtIG)




--------


## 15. Data Modelling


![Data Modelling](https://drive.google.com/uc?id=1ypFgR7amEpCoUSJ4itUcCSRAyA0nISPz)



## 16. Deployment Architecture

![Deployment Architecture](https://drive.google.com/uc?id=1caAQWyImMQ6UmFBQPHe3ehl-xZTSDHxT)












