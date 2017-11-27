# Project Design Document
## Authors
__Stav Rockah__ 
307900878   
__Roman Smirnov__
312914443

## Abstract 
We propose to build an interactive-book application composed of an Android client and Spring server. The application is similar to a standard a run-of-the-mill ebook, but is enhanced by enabling social interaction from within the book, and by enabling presentation of richer media (e.g GIFS, interactive graphs, 360 photographs, 3d models, etc). 

##  Meeting Requirements

 Both Spring and Android development flows allow for extensive use of many of the concepts presented in this course, including but limited to: Android, JavaBeans, XML, JPA, JDBC, etc.
 
By way of the specifications outlined below, the project is of sufficient size and complexity.

## Project Goals
There are some inherent limitations to books (both electronic and otherwise) that we would like to demonstrate solutions to via this project. These problems pertain to both writers and reading.

###  Problems 
1. No update functionality - a new revision or edition is required to add content or fix errors.
2. No statistics - no writer really knows how many people actually read their book, how much time did they read it, who are they, where did they spend most of their time, where do people usually quit, etc.
3. No built in social interaction - discussion, comments, Q&A, etc can't be conducted via the book itself. 
4. No rich media  - most books are black letters on white paper. There are no videos, sounds, moving illustrations etc.
5. No interactivity - no 3d models, 360 photographs, no interactive graphs, no memory games to help you remember, not possible to easily zoom in or rotate an image, you can't hide and answer or a section which you don't care about, etc.
6. Exercises - exercises are written in a Q&A format (if at all), there is no hint option, no roadblock to continue if you didn't get it, no reward and very little motivation to solve.
7. Unsuitable for mobile - pdf books are unsuitable for mobile reading. EPUBs are more suitable, but still retain format of physical books.

### Solutions
Our app address the outlined problems thusly:
1. Writers are able to issue fixes and add content whenever, and on the fly. 
2. Writers are able to view user statistics: when, who, where, what, how, how much, etc.
3. Writers are able to insert social interaction widgets into the book. Each section or chapter can have a discussion widget which people use share their thoughts or ask and answer questions.
4. Support for GIFs, video, audio content. 
5. Support for 3d model viewing, 360 photographs, interactive graphs, etc.
6. Gamification - exercises are interactive (e.g multiple choice or fill missing word questions) and grant points which are part of an achievement system. Also, a reader can't continue until proven sufficiently proficient in a subject. 
7. Content is decoupled from presentation - looks good on mobile, desktop, tablet, etc.

## Android Client

### Overview 
The Android Client will be built for min-sdk 21 for simplicity. 

The client will consist of several independent Android specific modules with a common core module. The modules will interconnect in a reactive fashion via a two sided coupling interface (a la clean architecture). 

1. __Core__ - contains entities, business logic, interactors. 
2. __UI__  - contains android views, activities, fragments, presenters, etc.
3. __Database__ - encapsulates data access and persistence functionality.
4. __Network__ - encapsulates cloud communication functionality.


### Core Module
Handles logic, data processing, etc 

### Network Module
Downloads books, images and other resources via a REST API. 

### Database Module
Provides persistence services. 

![database diagram](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Database_Diagram.jpg)

### UI Module
The UI application module  will be divided into feature (i.e a screen) sub-modules. 
Each component of a feature module will be encapsulated into  a fragment to enhance reusability and robustness.

The UI will consist of the following main screens:
1. Available Books
2. My Books
3. Book Detail
4. Profile
5. Settings
6. Social
7. Roadmap (List of chapters)
8. Play (learn & exercise screen)

### UI Screens
#### Available Books
Displays all books available for download.
User can see the book covers and some general information about each book. User can select a book to get a more details and download the book.

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/All_Books.jpg)

#### My Books
Displays all downloaded books. User can see all the book he/she have downloaded, can see book completion and progress metrics, and start reading it. 

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/My_Books.jpg)

#### Book Detail
Displays details about a single specific book: cover, title, author, description, etc

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Book_Detail.jpg)

#### Profile
Displays statistics, achievements, etc.


![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Profile.jpg)

#### Settings
Wireframe got corrupted - it's very standard... 

#### Roadmap
Displays book chapters in a roadmap kind-of-way.  User can see which chapters he/she completed so far and some completion metrics.

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Roadmap.jpg)

#### Learn
Displays learning content and exercises in a section of the book. 
User can read content, see through examples, view videos, etc. 

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Learn.jpg)

#### Social
Displays friends and their profiles, achievements, etc. 

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Social_Friends.jpg)

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Social_Friend_Profile.jpg)

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Social_Challenge.jpg)

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Social_Activity.jpg)

----------------------------------------------

## Spring Server

### Overview 
The back-end will be built on top of Spring 5.0 with Spring Boot 2.0. 
The Spring back-end will consist of several independent micro-service processes with a few common Kafka based messaging and logging components to allow a pub/sub fault tolerant communication model. 

#### Microservices 
1. API gateway
2. Discovery & Config
3. Logging
4. Kafka Messaging 
5. Analytics
6. User accounts & Authentication 
8. Resource repository
9. Social 

### Discovery & Config
Spring provided service to allow other services to look each other up by name and get an IP. 

### Logging
Debugging and visualisation tools. 

### Messaging
Enables decoupling services via a reactive communication model (AKA pub/sub). Offers a memory component for fault tolerance. 

### User Accounts 
Handles user data and authentication. 

### Analytics
Stores a user activity for later analysis. 

### Resource Repo
Stores various resources such as books, images, etc. 

### Social
Handles various interactions between users (e.g chat, co-op. etc)

### API Gateway
Provides a network interface via a REST API.
REST API would roughly correspond to the book format defined below. 

----------------------------------------------

## Standard Book Format
We define a standard format for interactive education books to allow offline transfer and help us decouple various parts of the implementation.

We define the format to be a zip file with several meta-data JSON files and resource folders. 

### Book.json
Holds the book title, subtitle, short description, full description, creators, initial publication date, version, last revision date,  uri of cover image/media

### Chapters.json
The chapters file holds a list of chapters. Each chapter hold the chapter number, title, description, and a list of sections within the chapter.
Each chapter has a corresponding resource folder containing the corresponding section resource sub-folders.

### Section.json
 Holds a list of content elements including names, types,  URIs, etc.
 
### Content
Each content file belongs to a specific section.
The possible content elements are:
Explanation
Definition
Example
Video
Illustration
Exercise 
Comments
Q&A# Project Proposal

## Authors
__Stav Rockah__ 
307900878
__Roman Smirnov__
312914443

## TL;DR
We propose to build an interactive-book application composed of an Android client and Spring server. 

##  Meeting Requirements

 Both Spring and Android development flows allow for extensive use of many of the concepts presented in this course, including but limited to: Android, JavaBeans, XML, JPA, JDBC, etc.
 
By way of the specifications outlined below, the project is of sufficient size and complexity.


---------------------------------------------------------

# Project Design Document

## Android Client

### Overview 
The Android Client will be built for min-sdk 21 for simplicity. 

The client will consist of several independent Android specific modules with a common core module. The modules will interconnect in a reactive fashion via a two sided coupling interface (a la clean architecture). 

1. __Core__ - contains entities, business logic, interactors. 
2. __UI__  - contains android views, activities, fragments, presenters, etc.
3. __Database__ - encapsulates data access and persistence functionality.
4. __Network__ - encapsulates cloud communication functionality.


### Core Module
Handles logic, data processing, etc 

### Network Module
Downloads books, images and other resources via a REST API. 

### Database Module
Provides persistence services. 

![database diagram](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Database_Diagram.jpg)

### UI Module
The UI application module  will be divided into feature (i.e a screen) sub-modules. 
Each component of a feature module will be encapsulated into  a fragment to enhance reusability and robustness.

The UI will consist of the following main screens:
1. Available Books
2. My Books
3. Book Detail
4. Profile
5. Settings
6. Social
7. Roadmap (List of chapters)
8. Play (learn & exercise screen)

### UI Screens

#### Available Books
Displays all books available for download

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/All_Books.jpg)

#### My Books
Displays all downloaded books

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/My_Books.jpg)

#### Book Detail
Displays details about a single specific book
![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Book_Detail.jpg)

#### Profile
Displays statistics, achievements, etc 

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Profile.jpg)

#### Settings
Wireframe got corrupted - it's very standard... 

#### Roadmap
Displays book chapters

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Roadmap.jpg)

#### Play
Displays learning content and exercises 

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Play.jpg)

#### Social
Displays friends and their profiles, enables co-op. 

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Social_Friends.jpg)

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Social_Friend_Profile.jpg)

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Social_Challenge.jpg)

![wireframe](https://github.com/roman-smirnov/advanced-java-project/raw/master/screen_images/Social_Activity.jpg)

----------------------------------------------

## Spring Server

### Overview 
The back-end will be built on top of Spring 5.0 with Spring Boot 2.0. 
The Spring back-end will consist of several independent micro-service processes with a few common Kafka based messaging and logging components to allow a pub/sub fault tolerant communication model. 

#### Microservices 
1. API gateway
2. Discovery & Config
3. Logging
4. Kafka Messaging 
5. Analytics
6. User accounts & Authentication 
8. Resource repository
9. Social 

### Discovery & Config
Spring provided service to allow other services to look each other up by name and get an IP. 

### Logging
Debugging and visualisation tools. 

### Messaging
Enables decoupling services via a reactive communication model (AKA pub/sub). Offers a memory component for fault tolerance. 

### User Accounts 
Handles user data and authentication. 

### Analytics
Stores a user activity for later analysis. 

### Resource Repo
Stores various resources such as books, images, etc. 

### Social
Handles various interactions between users (e.g chat, co-op. etc)

### API Gateway
Provides a network interface via a REST API.
REST API would roughly correspond to the book format defined below. 

----------------------------------------------

## Standard Book Format
We define a standard format for interactive education books to allow offline transfer and help us decouple various parts of the implementation.

We define the format to be a zip file with several meta-data JSON files and resource folders. 

### Book.json
Holds the book title, subtitle, short description, full description, creators, initial publication date, version, last revision date,  uri of cover image/media

### Chapters.json
The chapters file holds a list of chapters. Each chapter hold the chapter number, title, description, and a list of sections within the chapter.
Each chapter has a corresponding resource folder containing the corresponding section resource sub-folders.

### Section.json
 Holds a list of content elements including names, types,  URIs, etc.
 
### Content
Each content file belongs to a specific section.
The possible content elements are:
Explanation
Definition
Example
Video
Illustration
Exercise 
Comments
Q&A
