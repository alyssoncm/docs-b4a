# How To Build a Backend for ReactJS Using Back4App

## Introduction

In this tutorial, you will build a complete backend for a ReactJS application using Back4App. You will integrate various Back4App features—such as database management, Cloud Code Functions, user authentication, and real-time queries (Live Queries)—to create a secure, scalable, and robust backend that can seamlessly interact with your ReactJS frontend. Along the way, you will explore how easy it is to set up and configure your backend in Back4App’s environment, saving considerable time and effort compared to manually configuring servers and databases.

Using Back4App for your ReactJS backend can streamline your development workflow. Back4App provides a rich set of services, including a Parse-powered database, real-time updates, and file storage, all of which integrate smoothly with ReactJS. By the end of this tutorial, you will have a fully functional backend that supports data storage, user authentication, and real-time collaboration. You will also gain valuable experience in leveraging Back4App’s security features, scheduling tasks with Cloud Jobs, setting up webhooks for external integrations, and more. After completing this tutorial, you will be well-equipped to expand this foundation into a production-ready application or incorporate custom logic and third-party APIs as needed.

---

## Prerequisites

To complete this tutorial, you will need:

- **A Back4App account and a new Back4App project**  
  [Getting Started with Back4App](https://www.back4app.com/docs/get-started)  
  If you do not have an account, you can create one for free. Follow the guide above to get your project ready.

- **Basic ReactJS development environment**  
  You can set this up using [Create React App](https://create-react-app.dev/docs/getting-started/) or a similar tool. Ensure you have Node.js installed on your machine.

- **Node.js (version 14 or above) installed**  
  [Installing Node.js](https://nodejs.org/en/download/)  
  You will need Node.js for installing npm packages and running local development servers.

- **Familiarity with JavaScript and basic ReactJS concepts**  
  [ReactJS Official Documentation](https://reactjs.org/docs/getting-started.html)  
  If you’re new to React, review the official docs or a beginner’s tutorial before starting.

- **(Optional) Familiarity with REST APIs, GraphQL, or the Parse SDK**  
  While not strictly necessary, it helps to understand how to work with these technologies for a smoother integration.

Make sure you have all of these prerequisites in place before you begin. Having your Back4App project set up and your local ReactJS environment ready will help you follow along more easily.

---

## Step 1 – Setting Up Back4App Project

### Create a New Project

The first step in building your ReactJS backend on Back4App is creating a new project. If you have not already created one, follow these steps:

1. **Log in to your Back4App account**.  
2. **Click the “New App” button** in your Back4App dashboard.  
3. **Give your app a name** (e.g., “ReactJS-Backend-Tutorial”).  
4. **Select your app stack or region** if prompted, then finalize the creation.

> [SCREENSHOT: Indicate where you can show the “New App” button and naming your app in the Back4App dashboard.]

Once the project is created, you will see it listed in your Back4App dashboard. This project will be the foundation for all backend configurations discussed in this tutorial.

### Connect the Parse SDK

Back4App relies on the Parse Platform to manage your data, provide real-time features, handle user authentication, and more. Connecting your ReactJS application to Back4App involves installing the `parse` npm package and initializing it with the credentials from your Back4App dashboard.

1. **Retrieve your Parse Keys**:  
   In your Back4App dashboard, navigate to your app’s “App Settings” or “Security & Keys” section to find your **Application ID** and **JavaScript Key**. You will also find the **Parse Server URL** (often in the format `https://parseapi.back4app.com`).  
   > [SCREENSHOT: Indicate where the user can see the Application ID, JavaScript Key, and the Server URL in the Back4App dashboard.]

2. **Install the Parse SDK** in your ReactJS project:
   ```bash
   npm install parse
   ```
   If you’re using Yarn, you can install it with:
   ```bash
   yarn add parse
   ```

3. **Initialize Parse** in your ReactJS application. Typically, you would create a file (e.g., `parseConfig.js`) in your `src` directory:
   ```javascript
   // src/parseConfig.js
   import Parse from 'parse';

   // Replace the placeholders with your Back4App credentials
   Parse.initialize('YOUR_APPLICATION_ID', 'YOUR_JAVASCRIPT_KEY');
   Parse.serverURL = 'https://parseapi.back4app.com';

   export default Parse;
   ```
   This file ensures that whenever you import Parse elsewhere in your React app, it is pre-configured to connect to your specific Back4App instance.

By completing this step, you have established a secure connection between your ReactJS frontend and the Back4App backend. All requests and data transactions are securely routed through this SDK, reducing the complexity of manual REST or GraphQL calls (although you can still use them when needed).

---

## Step 2 – Setting Up the Database

### Saving and Querying Data

With your Back4App project set up and the Parse SDK integrated into your React app, you can now start saving and retrieving data. The simplest way to create a record is to use the `Parse.Object` class:

```javascript
// Example: Create a Todo item

import Parse from './parseConfig';

async function createTodoItem(title, isCompleted) {
  const Todo = Parse.Object.extend('Todo');
  const todo = new Todo();
  todo.set('title', title);
  todo.set('isCompleted', isCompleted);

  try {
    const savedTodo = await todo.save();
    console.log('Todo saved successfully:', savedTodo);
    return savedTodo;
  } catch (error) {
    console.error('Error saving Todo:', error);
  }
}

// Example: Query all Todo items

async function fetchTodos() {
  const Todo = Parse.Object.extend('Todo');
  const query = new Parse.Query(Todo);

  try {
    const results = await query.find();
    console.log('Fetched Todo items:', results);
    return results;
  } catch (error) {
    console.error('Error fetching Todos:', error);
  }
}
```

#### Using the REST API

Alternatively, you can use Back4App’s REST API endpoints:

```bash
curl -X POST \
  -H "X-Parse-Application-Id: YOUR_APPLICATION_ID" \
  -H "X-Parse-REST-API-Key: YOUR_REST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"title": "Buy groceries", "isCompleted": false}' \
  https://parseapi.back4app.com/classes/Todo
```

#### Using the GraphQL API

Back4App also provides a GraphQL interface:

```graphql
mutation {
  createTodo(input: {
    fields: {
      title: "Clean the house"
      isCompleted: false
    }
  }) {
    todo {
      objectId
      title
      isCompleted
    }
  }
}
```

These diverse options let you integrate data operations in the way that best suits your development process—whether that’s through the Parse SDK, REST, or GraphQL.

### Schema Design and Data Types

By default, Parse allows **schema creation on the fly**, but you can also define your classes and data types in the Back4App dashboard for more control.

1. **Navigate to the “Database” section** in your Back4App dashboard.  
2. **Create a new class** (e.g., “Todo”) and add relevant columns, such as `title` (String) and `isCompleted` (Boolean).  
   > [SCREENSHOT: Indicate where you click “Create a class” and how to configure new columns.]

Back4App also supports various data types: **String**, **Number**, **Boolean**, **Date**, **File**, **Relation**, **GeoPoint**, etc. You can choose the appropriate type for each field. If you prefer, you can also let Parse automatically create these columns when you first save an object from your React app.

#### Creating a Data Model with the Back4App AI Agent

Back4App offers an AI Agent that can help you design your data model:

1. **Open the AI Agent** from your project dashboard or Data Browser view.  
2. **Describe your data model** in simple language (e.g., “A Todo class with a title, a description, and a completion flag”).  
3. **Review and apply** the generated schema.  
   > [SCREENSHOT: Indicate where the AI Agent can be accessed, and show an example of the generated schema interface.]

Using the AI Agent can save you time when setting up your data architecture and ensure consistency across your application.

### Relational Data

If you have relational data—say, a `User` object that points to multiple `Todo` items—you can use **Pointers** or **Relations** in Parse. For example, adding a pointer to user:

```javascript
// Linking a Todo to a user with a pointer
async function createTodoForUser(userObjectId, title) {
  const Todo = Parse.Object.extend('Todo');
  const todo = new Todo();

  // Construct a pointer to the user
  const userPointer = new Parse.User();
  userPointer.id = userObjectId;

  // Set fields
  todo.set('title', title);
  todo.set('owner', userPointer);

  try {
    return await todo.save();
  } catch (err) {
    console.error('Error creating Todo with user relationship:', err);
  }
}
```

When you query, you can also include pointer data:

```javascript
const query = new Parse.Query('Todo');
query.include('owner');
const todosWithOwner = await query.find();
```

This `include('owner')` call fetches user details alongside each Todo, making your relational data seamlessly accessible.

### Live Queries

For real-time updates, Back4App provides **Live Queries**. In your ReactJS app, you can subscribe to changes on a specific class:

1. **Enable Live Queries** in your Back4App dashboard under your app’s **Server Settings**. Make sure “Live Queries” is turned on.  
2. **Initialize a Live Query Subscription** in your code:

```javascript
import Parse from './parseConfig';

async function subscribeToTodos(callback) {
  const query = new Parse.Query('Todo');
  const subscription = await query.subscribe();

  subscription.on('create', (newTodo) => {
    console.log('New Todo created:', newTodo);
    callback('create', newTodo);
  });

  subscription.on('update', (updatedTodo) => {
    console.log('Todo updated:', updatedTodo);
    callback('update', updatedTodo);
  });

  subscription.on('delete', (deletedTodo) => {
    console.log('Todo deleted:', deletedTodo);
    callback('delete', deletedTodo);
  });

  return subscription;
}
```

By subscribing, you receive real-time notifications whenever a new Todo is created, updated, or deleted. This feature is particularly valuable for collaborative or dynamic apps where multiple users need to see the latest data without refreshing the page.

---

## Step 3 – Applying Security with ACLs and CLPs

### Back4app Security Mechanism

Back4App takes security seriously by providing **Access Control Lists (ACLs)** and **Class-Level Permissions (CLPs)**. These features let you restrict who can read or write data on a per-object or per-class basis, ensuring only authorized users can modify your data.

### Access Control Lists (ACLs)

An **ACL** is applied to individual objects to determine which users, roles, or the public can perform read/write operations. For example:

```javascript
async function createPrivateTodo(title, ownerUser) {
  const Todo = Parse.Object.extend('Todo');
  const todo = new Todo();
  todo.set('title', title);

  // Create an ACL granting read/write access only to the owner
  const acl = new Parse.ACL(ownerUser);
  acl.setPublicReadAccess(false);
  acl.setPublicWriteAccess(false);

  todo.setACL(acl);

  try {
    return await todo.save();
  } catch (err) {
    console.error('Error saving private todo:', err);
  }
}
```

When you save the object, it has an ACL that prevents anyone but the specified user from reading or modifying it.

> [SCREENSHOT: Indicate how ACLs appear in the data browser if you want to demonstrate the ACL column in the Back4App dashboard.]

### Class-Level Permissions (CLPs)

**CLPs** govern an entire class’s default permissions, such as whether the class is publicly readable or writable, or if only certain roles can access it.

1. **Go to your Back4App Dashboard**, select your app, and open the **Database** section.  
2. **Select a class** (e.g., “Todo”).  
3. **Open the Class-Level Permissions** tab.  
   > [SCREENSHOT: Indicate where to find CLPs in the dashboard.]  
4. Configure your defaults, such as “Requires Authentication” for read or write, or “No Access” for the public.  

These permissions set the baseline, while ACLs fine-tune permissions for individual objects. A robust security model typically combines both CLPs (broad restrictions) and ACLs (fine-grained per-object restrictions).

---

## Step 4 – Writing and Deploying Cloud Functions

### Cloud Code Functions Architecture

**Cloud Code** allows you to run server-side logic in response to triggers (e.g., before a save or after a delete) or as custom functions that you can call from your frontend. With Cloud Code:

- You can **install and import NPM modules** for advanced functionality.  
- You can implement **triggers** (e.g., `beforeSave`, `afterSave`) to validate or transform data before it’s stored.  
- You can create **custom endpoints** to run specialized logic not covered by default operations.

In Back4App, Cloud Code runs inside the Parse Server environment. You typically have a `main.js` (or `index.js`) file where you define these functions.

### Deploy Your Function

Below is a simple Cloud Code function that calculates the length of a text string sent from the client:

```javascript
// main.js

Parse.Cloud.define('calculateTextLength', async (request) => {
  const { text } = request.params;
  if (!text) {
    throw new Error('No text provided');
  }
  return { length: text.length };
});
```

**Deploying via the Back4App CLI**:  
1. Install the CLI:  
   ```bash
   npm install -g back4app-cli
   ```
2. Login and initialize your project:  
   ```bash
   back4app login
   back4app init
   ```
3. Deploy your cloud code:  
   ```bash
   back4app deploy
   ```
   > [SCREENSHOT: Indicate how the CLI looks in terminal or show the Cloud Code directory structure in your editor.]

**Deploying through the Dashboard**:  
1. In your app’s dashboard, go to **Cloud Code** > **Functions**.  
2. Copy/paste the function into the **main.js** editor.  
3. Click **Save** or **Deploy**.

### Calling Your Function

From ReactJS using the Parse SDK:

```javascript
import Parse from './parseConfig';

async function getTextLength(text) {
  try {
    const result = await Parse.Cloud.run('calculateTextLength', { text });
    console.log('Text length:', result.length);
  } catch (err) {
    console.error('Error calling cloud function:', err);
  }
}
```

You can also call it via REST:

```bash
curl -X POST \
  -H "X-Parse-Application-Id: YOUR_APP_ID" \
  -H "X-Parse-REST-API-Key: YOUR_REST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"text": "Hello Back4App"}' \
  https://parseapi.back4app.com/functions/calculateTextLength
```

Or via GraphQL:

```graphql
mutation {
  calculateTextLength(input: {
    text: "Hello GraphQL"
  }) {
    result
  }
}
```

This flexibility enables you to integrate your custom logic into your ReactJS frontend or any other client that supports REST or GraphQL.

---

## Step 5 – Configuring User Authentication

### User Authentication in Back4App

Back4App leverages the **Parse User** class as the foundation for authentication. By default, Parse handles password hashing, session tokens, and secure storage. This means you don’t have to set up complex security flows manually.

### Setting Up User Authentication

In a React application, you can create a new user with:

```javascript
import Parse from './parseConfig';

async function signUpUser(username, password, email) {
  const user = new Parse.User();
  user.set('username', username);
  user.set('password', password);
  user.set('email', email);

  try {
    await user.signUp();
    console.log('User signed up successfully!');
  } catch (error) {
    console.error('Error signing up user:', error);
  }
}
```

Log in an existing user:

```javascript
async function logInUser(username, password) {
  try {
    const user = await Parse.User.logIn(username, password);
    console.log('User logged in:', user);
  } catch (error) {
    console.error('Error logging in user:', error);
  }
}
```

Via REST, a login might look like:

```bash
curl -X GET \
  -H "X-Parse-Application-Id: YOUR_APP_ID" \
  -H "X-Parse-REST-API-Key: YOUR_REST_API_KEY" \
  -G \
  --data-urlencode 'username=alice' \
  --data-urlencode 'password=secret123' \
  https://parseapi.back4app.com/login
```

### Session Management

After a successful login, Parse creates a **session token** that is stored in the user object. In your React app, you can access the currently logged-in user:

```javascript
const currentUser = Parse.User.current();
if (currentUser) {
  console.log('Currently logged-in user:', currentUser.getUsername());
} else {
  console.log('No user is logged in');
}
```

Parse automatically handles token-based sessions in the background, but you can also manually manage or revoke them. This is useful when you need to log out:

```javascript
await Parse.User.logOut();
```

### Social Login Integration

Back4App and Parse can integrate with popular OAuth providers, such as **Google** or **Facebook**, by installing additional packages or using existing adapters. For example, you can set up Facebook login by configuring your **Facebook App ID** and using `Parse.FacebookUtils.logIn()`. Detailed instructions vary, so refer to the [Parse Social Login Docs](https://docs.parseplatform.org/js/guide/#oauth-logins) or relevant Back4App guides.

### Email Verification and Password Reset

To enable email verification and password reset:

1. **Navigate to the Email Settings** in your Back4App dashboard.  
2. **Enable email verification** to ensure new users verify ownership of their email addresses.  
3. **Configure the From address**, email templates, and your custom domain if desired.  
4. For password reset:
   ```javascript
   await Parse.User.requestPasswordReset(email);
   ```
   This triggers an email with a password reset link from Back4App.

These features improve account security and user experience by validating user ownership of emails and providing a secure password-recovery method.

---

## Step 6 – Handling File Storage

### Uploading and Retrieving Files

Parse includes the `Parse.File` class for handling file uploads, which Back4App stores securely:

```javascript
import Parse from './parseConfig';

async function uploadImage(file) {
  // file is typically from an <input type="file" ... /> in React
  const name = file.name;
  const parseFile = new Parse.File(name, file);

  try {
    const savedFile = await parseFile.save();
    console.log('File saved:', savedFile.url());
    return savedFile.url();
  } catch (err) {
    console.error('Error uploading file:', err);
  }
}
```

To attach the file to an object in the database, you can do:

```javascript
async function createPhotoObject(file) {
  const Photo = Parse.Object.extend('Photo');
  const photo = new Photo();
  const parseFile = new Parse.File(file.name, file);
  photo.set('imageFile', parseFile);

  return await photo.save();
}
```

Retrieving the file URL is straightforward:

```javascript
const imageFile = photo.get('imageFile');
const imageUrl = imageFile.url();
```

You can display this `imageUrl` in your React components by setting it as the `src` of an `<img>` tag.

### File Security

By default, files are publicly accessible via their URL. You can add access control by setting up **CLPs** and **ACLs** on the underlying `File` objects if you prefer more restricted or authenticated-only access. This ensures only your app’s authorized users can retrieve certain files.

---

## Step 7 – Scheduling Tasks with Cloud Jobs

### Cloud Jobs

**Cloud Jobs** in Back4App let you schedule and run routine tasks on your backend—like cleaning up old data or sending a daily summary email. A typical Cloud Job might look like this:

```javascript
// main.js

Parse.Cloud.job('cleanupOldTodos', async (request) => {
  // This runs in the background, not triggered by a direct user request
  const Todo = Parse.Object.extend('Todo');
  const query = new Parse.Query(Todo);

  // For example, remove todos older than 30 days
  const now = new Date();
  const THIRTY_DAYS = 30 * 24 * 60 * 60 * 1000;
  const cutoff = new Date(now - THIRTY_DAYS);

  query.lessThan('createdAt', cutoff);

  try {
    const oldTodos = await query.find({ useMasterKey: true });
    await Parse.Object.destroyAll(oldTodos, { useMasterKey: true });
    return `Deleted ${oldTodos.length} old Todos.`;
  } catch (err) {
    throw new Error('Error during cleanup:' + err.message);
  }
});
```

1. **Deploy your Cloud Code** with the new job (via CLI or the dashboard).  
2. **Go to the Back4App Dashboard** > **Cloud Jobs**.  
   > [SCREENSHOT: Indicate how you can schedule the job in the dashboard, specifying frequency or on-demand execution.]  
3. **Schedule** the job to run daily or at whatever interval suits your needs.

Cloud Jobs enable you to automate background maintenance or other periodic processes—without requiring manual intervention.

---

## Step 8 – Integrating Webhooks

### Webhooks

**Webhooks** allow your Back4App app to send HTTP requests to an external service whenever certain events occur. This is powerful for integrating with third-party systems like payment gateways (e.g., Stripe), email marketing tools, or analytics platforms.

1. **Navigate to the Webhooks configuration** in your Back4App dashboard.  
2. **Set up an endpoint** (e.g., `https://your-external-service.com/webhook-endpoint`).  
3. **Configure triggers** to specify which events in your Back4App classes or Cloud Code functions will fire the webhook.

For instance, if you want to notify a Slack channel whenever a new `Todo` is created:

- Create a Slack App that accepts incoming webhooks.  
- Copy the Slack webhook URL.  
- In your Back4App dashboard, set the endpoint to that Slack URL for the event “New record in the Todo class.”  
- You can also add custom HTTP headers or payloads if needed.

In some advanced scenarios, you can define Webhooks in Cloud Code by making custom HTTP requests in triggers:

```javascript
Parse.Cloud.afterSave('Todo', async (request) => {
  // If it's a new Todo...
  if (!request.original) {
    await Parse.Cloud.httpRequest({
      method: 'POST',
      url: 'https://your-external-service.com/webhook-endpoint',
      headers: {
        'Content-Type': 'application/json'
      },
      body: {
        todoId: request.object.id,
        title: request.object.get('title'),
      }
    });
  }
});
```

---

## Step 9 – Exploring the Back4App Admin Panel

### Admin Panel

Back4App provides an **Admin App** (Parse Dashboard-based) that gives you a powerful UI to manage your app beyond the standard data browser:

- **View and edit classes**: Create new classes, edit existing data, manage indexes.
- **Manage users and roles**: Reset passwords, view email confirmations, or ban accounts if necessary.
- **Configure app settings**: Adjust your security keys, environment variables, or application environment.

[Back4App Admin App Documentation](https://www.back4app.com/docs/parse-dashboard/admin-app)

> [SCREENSHOT: Indicate how to access the Admin Panel from your project’s settings or an external URL provided by Back4App.]

Using the Admin Panel streamlines administrative tasks that might otherwise require manual code updates or database manipulation. You can keep an eye on logs, server performance, and schedule additional tasks as needed.

---

## Conclusion

By following this comprehensive tutorial, you have:

- **Created a secure backend** for a ReactJS app on Back4App.  
- **Configured a database** with class schemas, data types, and relationships.  
- **Integrated real-time queries** (Live Queries) for immediate data updates.  
- **Applied security measures** using ACLs and CLPs to protect and manage data access.  
- **Implemented Cloud Code** functions to run custom business logic on the server side.  
- **Set up user authentication** with support for email verification and password resets.  
- **Managed file uploads** and retrieval, with optional file security controls.  
- **Scheduled Cloud Jobs** for automated background tasks.  
- **Used Webhooks** to integrate with external services.  
- **Explored the Back4App Admin Panel** for advanced app management.

### Next Steps

- **Build a production-ready ReactJS app** by extending this backend to handle more complex data models, caching strategies, and performance optimizations.  
- **Integrate advanced features** such as specialized authentication flows, role-based access control, or external APIs (like payment gateways).  
- **Check out Back4App’s official documentation** for deeper dives into advanced security, performance tuning, and logs analysis.  
- **Explore other tutorials** on real-time chat applications, IoT dashboards, or location-based services. You can combine the techniques learned here with third-party APIs to create complex, real-world applications.

With a solid ReactJS frontend and a robust Back4App backend, you are now well-equipped to develop feature-rich, scalable, and secure applications. Continue exploring more advanced functionalities, integrate your own business logic, and harness the power of Back4App to save you countless hours in server and database administration. Happy coding!
