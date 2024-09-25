In **Kore.ai**, you can define backend APIs using **Service Nodes** in your dialog tasks to connect your chatbot to external systems and fetch or send data. Kore.ai allows you to define and integrate APIs (such as REST APIs) that can be triggered during conversations. Here’s a detailed guide on how to define backend APIs in Kore.ai:

### Steps to Define Backend API in Kore.ai

1. **Access the Dialog Task Builder**
2. **Add a Service Node**
3. **Define the API Request**
4. **Handle API Responses**
5. **Test and Validate**

---

### 1. **Access the Dialog Task Builder**

To define a backend API in Kore.ai, you first need to create or modify a **Dialog Task**.

- **Log in to your Kore.ai Bot Builder account** and navigate to the bot for which you want to add API integration.
- Go to the **Bot Tasks** section and either create a new **Dialog Task** or edit an existing one.
    - If creating a new task, select **Dialog Task** and give it a name. This will open the **Task Editor** where you can define the conversational flow.

---

### 2. **Add a Service Node**

A **Service Node** is where the API call happens. You’ll be able to define the backend API you want the bot to connect to.

- In the **Task Editor**, drag and drop a **Service Node** from the left panel into the conversation flow.
    - **Service Nodes** are used to perform backend API calls, such as retrieving data from external systems or submitting data from the user.
- Once you’ve added the **Service Node**, configure it by clicking on it. This will bring up the configuration options for the API request.

---

### 3. **Define the API Request**

#### a. **Set Request Type (GET, POST, etc.)**

Define the API request type based on what you need. Kore.ai supports **GET, POST, PUT, DELETE** requests, allowing you to retrieve or send data to your backend system.

- In the **Service Node Configuration**, select the **Request Type**. For example:
    - **GET**: To fetch data (e.g., user investment details).
    - **POST**: To send data (e.g., user login credentials).

#### b. **Specify API URL**

Provide the complete URL of your backend API.

- **URL**: Input the API endpoint that you want to call. You can use dynamic variables (e.g., user input) if necessary.

Example:

plaintext