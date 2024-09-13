# PupPals - Find Your Pup Some Pals 🐶


https://github.com/user-attachments/assets/4f7385ae-a98e-4924-8b73-2763ea26391f


## Table of Contents
- [Description](#description)
- [Demo](#demo)
- [Team Members](#team-members)
- [Technologies Used](#technologies-used)
- [Getting Started](#getting-started)
- [Planning](#planning)
- [Features](#features)
- [My Contributions](#my-contributions)
- [Challenges](#challenges)
- [Wins](#wins)
- [Key Takeaways](#key-takeaways)
- [Future Improvements](#future-improvements)

## Description
PupPals is a full-stack application designed to connect dog owners with other local dog owners. It was my third project during the Software Engineering Immersive Course with General Assembly, completed as part of a three-person team over two weeks. The unique aspect of PupPals is that matching is based solely on the dogs themselves, not the owners' appearances, promoting genuine connections between pets and their humans.

## Demo
The application is deployed on Heroku. You can explore PupPals [here](https://puppals-f25422820259.herokuapp.com/).

## Team Members
- Filomena Murgo (myself)
- Cassie Lee
- Mollie Gregson

## Technologies Used
### Backend
- Node.js
- Express.js
- MongoDB
- Mongoose

### Frontend
- JavaScript
- React
- SCSS/SASS

### Additional Packages
- Axios
- Bootstrap
- Cloudinary
- React Card Flip

### Development Tools
- Git
- GitHub
- Insomnia

## Getting Started
To run this project locally:

1. Fork and clone the repository
2. Open the project file in your preferred code editor
3. From the project root folder, open an integrated terminal:
   ```
   npm install
   npm run start  # Run after ALL packages are installed
   ```
4. In a second terminal, navigate to the client folder:
   ```
   npm install
   npm run dev  # Run after ALL packages are installed
   ```
5. Navigate to http://localhost:4000/ in your browser

## Planning
Our planning phase was crucial to the project's success, especially given our two-week timeframe. We employed a variety of tools and techniques to ensure we had a solid foundation for our development process:

![paw-pals_project-3](https://github.com/user-attachments/assets/476dc345-73e0-4fa0-a06c-24c4cb209886)


1. **Mood Board:** We created a mood board to establish the visual direction for our app, helping us align on the look and feel we wanted to achieve.

![Untitled-2024-03-26-1608](https://github.com/user-attachments/assets/1e1bead0-fce3-4a64-9e45-1917c248039e)

2. **Wireframing:** We sketched out detailed wireframes for each page of our application. This helped us visualise the user interface and identify the components we'd need to build. We planned for five main pages: Home Page, Register Page, Login Page, User Profile Page, and Browsing Pups Page.
![68747470733a2f2f7265732e636c6f7564696e6172792e636f6d2f647634796d697373732f696d6167652f75706c6f61642f76313732303435343435372f526561644d652f70757070616c732d666c6f7763686172745f6e6d7671356d2e706e67](https://github.com/user-attachments/assets/e2bae4a3-918c-48ca-859a-36332076cf2d)
![68747470733a2f2f7265732e636c6f7564696e6172792e636f6d2f647634796d697373732f696d6167652f75706c6f61642f76313732303435343435382f526561644d652f70757070616c732d6d6f64656c5f7468706f39742e706e67](https://github.com/user-attachments/assets/f5e805f8-3489-422d-abf5-396d1e92d22c)

3. **Data Models and Relationships:** We spent considerable time mapping out our data models and their relationships. This included designing schemas for Users, Pups, and Chats, and determining how they would interact within our application.

<img width="1258" alt="Screenshot 2024-09-13 at 15 50 41" src="https://github.com/user-attachments/assets/9f76bd74-d647-4f89-b7cf-7336e74fb662">

4. **Task Allocation:** We used a Trello board to break down our project into manageable tasks. This allowed us to assign responsibilities, track progress, and ensure we were all aligned on our goals and deadlines.

5. **Technology Choices:** We collectively decided on our tech stack, opting for a MERN (MongoDB, Express.js, React, Node.js) setup. We also chose additional libraries like Axios for HTTP requests and React Card Flip for some UI elements.

6. **Timeline:** We created a rough timeline for our two-week sprint, allocating time for planning, development, testing, and deployment.

This thorough planning process gave us a clear direction and helped us anticipate challenges before we encountered them in the development phase.

## Features
- User authentication (register/login)
- User profile creation and editing
- Dog profile creation and editing
- Browse other dogs' profiles
- Match with other dogs by "throwing a bone"
- Chat functionality between matched users
- Image upload using Cloudinary

## My Contributions

### Home Page
I took the lead on developing the Home Page, which serves as the first point of contact for our users. It features a hero image that my team mate Cassie Lee designed, setting the tone for the application's friendly and inviting atmosphere. The page includes a navigation bar with links to the Registration and Login pages, making it easy for new users to join or existing users to access their accounts.

Towards the bottom of the page, I added two key sections:
1. An informative container explaining what PupPals is about, helping new visitors understand the app's purpose and benefits.
2. A showcase of pups currently looking for friends, which gives potential users a glimpse of the community they could join.

### Frontend-Backend Connection
One of my primary responsibilities was establishing and maintaining the connection between our frontend and backend. This involved:

- Setting up Axios for making HTTP requests from our React frontend to our Express backend.
- Implementing proper error handling to ensure a smooth user experience even when things go wrong.
- Creating and managing API endpoints for various functionalities, ensuring they were correctly consumed by the frontend.

### Pup Schema
I played a key role in designing and implementing the Pup Schema, which is central to our application's functionality. The schema includes fields such as:

```javascript
const pupSchema = new mongoose.Schema({
  name: { type: String, required: true },
  breed: { type: String, required: true },
  age: { type: Number, required: true },
  size: { type: String, required: true, enum: ['Small', 'Medium', 'Large'] },
  temperament: { type: String, required: true },
  image: { type: String, required: true },
  owner: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true }
})
```

This schema allows us to store comprehensive information about each dog, facilitating better matches between pups.

### "Throwing the Bone" Matching Feature
I developed the core functionality for our matching system, which we playfully call "throwing the bone". This feature allows users to express interest in another pup, potentially leading to a match. Here's a simplified version of the code I wrote for this feature:

```javascript
const throwBones = async (req, res) => {
  try {
    const userId = req.params.userId;
    const targetProfile = await User.findById(userId)
    
    // Check if current user has already thrown a bone to this profile
    const matchedId = targetProfile.bonesThrownBy.find(id => id.equals(req.currentUser._id))
    if (!matchedId) {
      targetProfile.bonesThrownBy.push(req.currentUser._id)
      await targetProfile.save();
    }
    
    // Check for mutual match
    const firstMatch = targetProfile.bonesThrownBy.find(id => id.equals(req.currentUser._id));
    const secondMatch = req.currentUser.bonesThrownBy.find(id => id.equals(userId));

    if (!firstMatch || !secondMatch) {
      return res.json({ Message: 'No match yet' })
    }

    // If mutual match, create a new chat
    const chatData = {
      messages: [],
      users: [firstMatch, secondMatch],
      pups: [currentUserPup._id, targetUserPup._id]
    };

    const newChat = await Chat.create(chatData);

    return res.status(200).json({ targetProfile, newChat });
  } catch (error) {
    sendError(error, res)
  }
}
```

This function checks for mutual interest between users and creates a new chat if a match is found, enabling users to start communicating.

### Generic Error Handling
To ensure a robust and user-friendly application, I implemented a generic error handling system. This system catches and processes errors throughout the application, providing meaningful feedback to users and logging issues for our team to address. Here's a snippet of the error handling middleware I created:

```javascript
const errorHandler = (err, req, res, next) => {
  console.error(err.stack)
  
  if (err.name === 'ValidationError') {
    return res.status(400).json({ message: err.message })
  }
  
  if (err.name === 'CastError') {
    return res.status(400).json({ message: 'Invalid ID' })
  }
  
  res.status(500).json({ message: 'Something went wrong' })
}

app.use(errorHandler)
```

This middleware catches different types of errors and returns appropriate status codes and messages, improving the overall reliability of our application.

## Challenges
One of the main challenges I faced was implementing the matching system. Determining whether to handle the matching logic in the frontend or backend required careful consideration. We ultimately decided to implement it in the backend for better security and data integrity.

Another challenge was effectively managing state in React, particularly when updating user profiles or chat messages. Ensuring that the UI reflected the most current data required a solid understanding of React's state management and lifecycle methods.

## Wins
Successfully implementing the "throwing the bone" feature was a significant win. Seeing users match and start conversations based on their dogs' profiles was incredibly satisfying.

The home page design received positive feedback from our instructors and peers, which was very encouraging. It effectively conveyed the purpose and feel of our application.

Our team's communication and collaboration were excellent throughout the project. We used Slack for ongoing communication and Trello for task management, which helped us stay organised and avoid major conflicts in our code.

## Key Takeaways
- The importance of planning cannot be overstated. Our detailed planning phase made the development process much smoother.
- Effective error handling is crucial for creating a robust application and providing a good user experience.
- Regular communication within a team is vital for the success of a project, especially when working on interconnected features.

## Future Improvements
- Implement a map feature using a map API to show dog-friendly locations and parks.
- Add location-based filtering for finding nearby pups.
- Create a review system for the application, allowing potential users to see feedback from current users.
- Enhance the responsive design to ensure a seamless experience on mobile devices.
- Implement real-time notifications for new matches and messages.

This project was an incredible learning experience, allowing me to apply and expand my full-stack development skills while creating a fun and useful application for dog owners.
