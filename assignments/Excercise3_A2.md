**Concept design**

<br>
<br>

**Concept** CourseFiltering [Course]

**Purpose**\
Enables users to efficiently locate courses relevant to their academic goals by narrowing down a large collection into a manageable set based on categories.

**Principle**\
Each class begins with a set of system assigned tags, and the user begins with a list of all available classes. The user selects one or more tags, and the filter produces a list of courses that only includes classes matching all selected tags. The user can then add or remove tags and get a new list of courses that only includes classes matching all selected tags. The user can then clear the filters to remove all active tags.

**State**
  - A set of courses with
    - A set of tags
- A set of tags with
  - An Id String
  - A category String
- A set of Active tags
- A set of Filtered courses

**Actions**
  - AddTag (t: tag)
    - **effects** adds tag to the active set and updates to ensure classes matching all selected tags are in set of filtered courses

  - RemoveTag (t: tag)
    - **effects** removes tag from the active set of tags and updates to ensure classes matching all selected tags are in set of filtered courses
      
  - ClearTags()
    - **effects** resets filter and clears the tags in the Active set of tags and clears the courses in the set of filtered courses

<br>
<br>

**Concept** CourseScheduling [User]

**Purpose**
Enables the user to create multiple schedules and add classes to them to compare schedules.

**Principle**
The user begins by creating a schedule. The user selects courses from a set of system created courses and adds them to the schedule. The user can create their own course and then remove a course from the schedule. The user can delete previous schedules and create a new empty schedule to add and remove courses from.

**State**
  - A set of schedules with
    - A set of courses
    - An owner
  - A set of courses with
    - A day
    - A start time
    - An end time
      
**Actions**
  - createCourse (d : day, start : time, end : time) : (c : course)
    - **requires** valid day and start and end times
    - **effects** course does not already exist
- addCourse (c : course, u: user, s : schedule)
  - **requires** course is valid, course does not already exist in schedule and user is the owner of the schedule
  - **effects** course is added to the schedule
- removeCourse (c : course, u : user, s : schedule)
  - **requires** course is valid, course exists on the schedule, and that user is the owner of the schedule
  - **effects** course is removed from the schedule
- createSchedule (u : user) : (s : schedule)
  - **effects** creates empty schedule with user as the owner
- deleteSchedule (u: user)
  - **requires** user is the owner of the schedule
  - **effects** deletes schedule

<br>
<br>

**Concept** UserAuthentication

**Purpose** 
limit access to known users

**Principle** 
after a user registers with a username and a password, they can authenticate with that same username and password and be treated each time as the same user

**State**
  - a set of Users with
    - an email address String
    - a username String
    - a password String
    - a confirmed flag
    - a token String
      
**Actions**
  - register (username: String, password: String, email: String): (user: User) (token: String)
    - **requires** a unique username
    - **effects** creates a User with the associated username and password. Creates a new token to email the user via a synchronous state and verify their email address. Confirmed is set to false
  - confirm (username: String, token: String)
    - **effects** if the token matches the usernames’ associated User, the flag confirmed changes to true else confirmed stays false
  - authenticate (username: String, password: String)
    - **requires** User’s confirmed flag must be true and username and password must match internal record for user
    - **effects** if the user is authenticated, they will be treated each time as the same user associated with that username and password. If they are not, they will not proceed Questions

<br>
<br>

**Concept** UserSession

**Purpose**
Enables the user to stay logged in during an interaction period so they can perform actions without authenticating each time

**Principle**
The user is successfully authenticated, and a session is created and associated with the user. The user can extend their session or explicitly end it or allow it to expire after a fixed duration.

**State**
  - A set of sessions with
    - A userID
    - An expiry time

**Actions**
  - startSession ( u : userID) : ( s : session)
    - **requires** a valid authenticated user
    - **effects** creates a session associated with the user and an expiry time
  - endSession ( s : session)
    - **requires** session is valid
    - **effects** deletes session
  - useSession (s : session)
    - **requires** session is not expired
    - **effects** allows user to proceed as authenticated user
  - extendSession (s : session)
    - **requires** valid session
    - **effects** deletes session and creates a new session for that user
  - System expire ( s : session)
    - **requires** session has passed expiry time
    - **effects** deletes session

<br>
<br>

**Sync** deleteSchedule
  + **When** userSession.useSession ()
  + **Then** courseScheduling.deleteSchedule(user : userID)

**Sycn** createSchedule
  + **When** userSession.useSession ()
  + **Then** courseScheduling.createSchedule(user : userID)
  
**Sync** addToSchedule
  + **When** userSession.useSession ()
  + **Then** courseScheduling.addCourse()

**Sycn** removeFromtSchedule
  + **When** userSession.useSession ()
  + **Then** courseScheduling.removeCourse()

**Sync** beginSession
  + **When** userauthentication.authenticate () : (user)
  + **Then** userSession.startSession (userID : user)

**Sync** selectCourse
  + **When**\
  courseScheduling.addCourse(course, schedule)\
  Course must exist in courseFiltering.FilteredCourses or courseScheduling.createCourse () : (course)
  + **Then** course is added to schedule

<br>
<br>
<br>

In Academica, the app’s functionality is structured around four key concepts that work together to provide a seamless scheduling experience for students. User Authentication ensures that only known users can access the system; users register, confirm their email, and authenticate with a username and password. Once authenticated, the User Session concept manages their session, allowing students to perform actions without re-authenticating for each interaction. Syncs such as beginSession and useSession ensure that the user’s session status controls access to other concepts.

The Course Filtering concept allows users to narrow down courses by tags with categories such as department, level, or graduation requirements and id Strings such as History or Chemistry. Its state maintains the active filters and the resulting set of courses, which ensures that users can only select relevant courses. The Course Scheduling concept builds on this, enabling users to create multiple schedules, add or remove courses, and compare them. Syncs such as selectCourse enforce that courses added to schedules come either from the filtered set or from valid user-created courses, maintaining consistency and preventing invalid selections.
Additionally, the type courses in Course Scheduling is bound to course objects from Course Filtering, ensuring that scheduling actions only reference valid course data.

<br>
<br>


[Back to Assignment 2](Assignment2.md)
