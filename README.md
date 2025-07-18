# RushTranslate App

## Project Deliverables
- **Full Source Code:** This repository contains the complete source code for both the client (React Native/Expo) and server (Node.js/Express) applications. No compiled binaries are included.
- **Repository Address:**
  - https://github.com/Liviuu1/RushTranslateApp
  - The repository contains all code under the `client/` and `server/` directories.

## Application Compilation, Installation, and Launch Steps

### Prerequisites
- **Node.js** (v18 or later recommended)
- **yarn** (v1.22 or later)
- **Expo CLI** (for client): Install globally with `yarn global add expo-cli`

---

### 1. Clone the Repository
```sh
git clone [your-repository-address]
cd RushTranslate
```

---

### 2. Server Setup
```sh
cd server
yarn install
```

#### Launch the Server
```sh
yarn start
```
- The server will start on the default port (check `server.js` for the port, e.g., 5000).

---

### 3. Client Setup
```sh
cd ../client
yarn install
```

#### Configure Environment Variables
- Create a `.env` file in the `client/` directory with the following content:
```
EXPO_PUBLIC_API_BASE_URL=http://10.0.2.2:5000/api/translations
```
- Replace `http://10.0.2.2:5000` with your server's address if running on a different host or port.

#### Launch the Client (Expo)
```sh
expo start
```
- This will open the Expo Dev Tools in your browser. You can run the app on an emulator, simulator, or physical device using the Expo Go app.

---

### 4. Additional Notes
- Make sure the server is running before using the client app for translation features.
- No compiled binaries are included; all code is provided in source form.
- For any issues, check the respective `package.json` files for available scripts and dependencies.

---

## Wireframe: [wireframe](https://www.figma.com/design/JWIU6D9crToZwDgviTKRd2/RushTranslate?node-id=0-1&node-type=canvas&t=aoCEVEw9KvQHlGIw-0)

## Features

### Camera Translation
The camera translation feature provides real-time text detection and translation:

#### Core Features
- Real-time camera preview with text detection
- Support for both front and back cameras
- High-quality photo capture with gallery integration
- Smart permission handling with user feedback
- Text recognition with visual overlay
- Instant translation of detected text

#### Camera Controls
- Intuitive camera flip (front/back)
- Smart capture button with loading state
- Gallery access for existing photos
- Preview mode with retake option
- Flash control for low-light conditions

#### Text Recognition & Translation
- Real-time text detection with visual feedback
- Accurate bounding boxes for detected text
- Green highlighting for active text regions
- Multi-region text detection support
- Instant translation of detected text
- Support for multiple language pairs

### Voice Translation
The voice translation feature enables real-time speech translation:

#### Core Features
- Real-time voice recognition
- Support for multiple languages
- High-quality audio recording
- Clear audio playback of translations
- Offline voice recognition support

#### Voice Controls
- One-tap recording start/stop
- Audio level visualization
- Language pair quick-switch
- Playback controls for translations
- History of recent translations

### Common Features

#### User Interface
- Modern, minimalist Tamagui design
- Intuitive navigation and controls
- Loading indicators with visual feedback
- Consistent design language
- Responsive layout for all devices
- Dark/light theme support

#### Error Handling
- Graceful permission management
- Comprehensive error feedback
- Offline mode handling
- Process state management
- Detailed error logging

#### Accessibility
- Full i18n support
- High contrast UI elements
- Clear, readable text
- Proper touch target sizing
- Voice feedback support
- RTL language support

#### Technical Architecture
- TypeScript for type safety
- Modern React hooks architecture
- Efficient state management
- Clean component structure
- Proper resource management
- Modular service architecture

## Project Structure

```plaintext
RushTranslate/
├── client/
│   ├── App.tsx                    # Main entry point
│   ├── assets/                    # Static assets
│   ├── src/
│   │   ├── screens/
│   │   │   ├── camera/           # Camera translation features
│   │   │   ├── voice/            # Voice translation features
│   │   │   ├── home/             # Main dashboard
│   │   │   ├── settings/         # App settings
│   │   │   ├── language_selection/# Language selection
│   │   │   ├── package_details/  # Translation package details
│   │   │   ├── saved/            # Saved translations
│   │   │   └── welcome/          # Welcome/onboarding
│   │   ├── components/           # Shared UI components
│   │   ├── services/             # Business logic services
│   │   └── utils/                # Utility functions
│   ├── routes/                   # Navigation setup
│   └── types/                    # TypeScript definitions
│
└── server/                       # Backend implementation
    ├── controllers/              # Business logic
    ├── models/                   # Data models
    ├── routes/                   # API routes
    └── services/                 # Backend services
```

## Architecture

# **Presentation Layer (UI/UX)**

This layer will be responsible for user interaction.  
Expo React Native will be used alongside Tamagui for UI styling and components

### **Features:**
- Displaying the automatically detected language based on the country and suggesting it as the target language.  
- Providing the option to manually set a different target language if the user desires.
- A set of packages available to download for offline translation

# **Business Logic Layer**

The core logic of the app and services will be managed here. In this layer, we will integrate functionalities for offline translations and request handling.

### **Features:**
- Detecting the location to suggest the target language.  
- Synchronizing with the backend when the user has internet access to fetch new translations (e.g., using a translation API).  
- Managing offline mode: when there is no internet connection, the app will use a local database with emergency translations.  
- Implementing logic to save new translations fetched from the server to the local database when the connection is restored.  
- Alerts for the user in case the manually set target language differs from the detected one.  

# **Data Access Layer (DAL)**

This layer will manage all database-related operations.

### **Features:**
- Storing offline translations in a local database (SQLite).  
- When the app is offline, accessing translations from this local database.  
- Synchronizing the local database with new data obtained from the backend (when the app has an internet connection).  

# **Backend Layer (Server-Side)**

This will be responsible for providing updated translations from the backend when there is an internet connection.
We will use Express.js for the backend side.

### API Documentation

API Endpoints
1. List All Packages
Endpoint: GET /api/translations/packages

Description: Retrieves a list of all available translation packages.

Response:

200 OK: Returns an array of package names.
500 Internal Server Error: If there is an error retrieving the packages.

```
[
  "business essentials",
  "travel_essentials",
  "medical care essentials"
]
```

2. Get All Translations for a Package
Endpoint: GET /api/translations/:package

Description: Retrieves all languages and their translations for a specified package.

URL Parameters:

package: The name of the package (e.g., "travel essentials").
Response:

200 OK: Returns an array of languages and translations.
404 Not Found: If the package is not found.
500 Internal Server Error: If there is an error retrieving the translations.

```
{
  "languages": [
    {
      "language": "en",
      "translations": [
        { "key": "welcome_message", "text": "Welcome to our app!" },
        { "key": "logout", "text": "Logout" }
      ]
    },
    {
      "language": "es",
      "translations": [
        { "key": "welcome_message", "text": "¡Bienvenido a nuestra aplicación!" },
        { "key": "logout", "text": "Cerrar sesión" }
      ]
    }
  ]
}
```

3. Get Translations for a Specific Language in a Package
Endpoint: GET /api/translations/:package/:language

Description: Retrieves translations for a specific language within a specified package.

URL Parameters:

package: The name of the package (e.g., "travel essentials").
language: The language code (e.g., "en" for English).
Response:

200 OK: Returns an array of translations for the specified language.
404 Not Found: If the package or language is not found.
500 Internal Server Error: If there is an error retrieving the translations.

```
{
  "translations": [
    { "key": "welcome_message", "text": "Welcome to our app!" },
    { "key": "logout", "text": "Logout" }
  ]
}
```

4. Add or Update Translations for a Package and Language
Endpoint: POST /api/translations/:package

Description: Adds or updates translations for a specific package and language.

URL Parameters:

package: The name of the package (e.g., "travel essentials").
Request Body:

language (string): The language code (e.g., "fr" for French).
translations (array): An array of translation objects, where each object contains a key and text.

Example request: 

```
{
  "language": "fr",
  "translations": [
    { "key": "welcome_message", "text": "Bienvenue dans notre application!" },
    { "key": "logout", "text": "Déconnexion" }
  ]
}
```

Response:

200 OK: Returns a success message.
500 Internal Server Error: If there is an error saving the translations.

```
{
  "message": "Translations updated successfully"
}
```

Data Model

Translation Schema

```
{
  "key": String,      // Required. The key identifier for the translation.
  "text": String      // Required. The translated text for the specified key.
}
```

Language Schema

```
{
  "language": String,         // Required. The language code (e.g., "en" for English).
  "translations": [           // Array of translation objects.
    {
      "key": String,
      "text": String
    }
  ]
}
```

Translation Package Schema

```
{
  "packageName": String,      // Required. The unique name of the translation package.
  "languages": [              // Array of language objects.
    {
      "language": String,
      "translations": [
        {
          "key": String,
          "text": String
        }
      ]
    }
  ]
}
```

### **Features:**
- Calls to a translation service (Google Translate).  
- Managing the logic for language selection based on location, using GPS/IP data to identify the user's country.  
- Providing emergency translations in case the language is not immediately available.  
- Implementing an endpoint to fetch new translations and updates for the local database.  

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [Running the App](#running-the-app)
- [Deployment](#deployment)

## Overview

RushTranslate is a mobile application that provides emergency offline translations when you don't have an internet connection. Built using [React Native](https://reactnative.dev/) with [Expo](https://expo.dev/) and [Tamagui](https://tamagui.dev/) for UI components, it ensures quick and reliable translations of critical sentences in various languages.

## Features

- **Responsive UI**: Utilizing Tamagui for a consistent and responsive design.
- **Multi-language Support**: Integrated with language providers for dynamic translations.
- **Push Notifications**: Handling push notifications with Expo.
- **Social Authentication**: Implemented authentication with email and Google sign-in.

## Project Structure

Modify here
```plaintext
RushTranslate/
├── client/
│   ├── App.tsx                    # Main entry point for the React Native app
│   ├── assets/                    # Static assets like images 
│   ├── package.json               # Project dependencies and scripts
│   ├── tsconfig.json              # TypeScript configuration
│   ├── src/
│   │   ├── components/            # Reusable UI components
│   │   ├── screens/               # Application screens
│   │   ├── routes/                # App routing setup
│   │   └── README.md              # Documentation
│   │
│   └── server/
│       ├── controllers/
│       │   └── translationController.js # Business logic for translations
│       ├── models/
│       │   └── translationPackage.js    # Database models for packages and translations
│       ├── routes/
│       │   └── translationRoutes.js     # API routes for translation handling
│       ├── package.json                 # Server dependencies and scripts
│       └── server.js                    # Express server entry point
│
└── README.md
```

## Prerequisites

Before you begin, ensure you have met the following requirements:

- **Node.js** installed on your machine. You can download it from [nodejs.org](https://nodejs.org).

- **Yarn** (package manager) installed. You can install it from [yarnpkg.com](https://yarnpkg.com).

- **Expo CLI** installed globally. You can install it via npm with:

    ```bash
    npm install -g expo-cli
    ```

## Installation

1. Clone this repository to your local machine:

    ```bash
    git clone https://github.com/liviuu1/RushTranslate.git
    ```

2. Navigate to the project directory:

    ```bash
    cd client
    ```

3. Install the dependencies:

    ```bash
    yarn install

## Server Setup

1. Navigate to the server directory:

    ```bash
    cd RushTranslate/server
    ```

2. Install dependencies:

    ```bash
    yarn install
    ```

3. Create a `.env` file in the `server` directory with the following variables:

    ```plaintext
    PORT=5000
    DATABASE_URL=your_database_url
    ```    ```

## Usage

### Running the App

To start the Expo server and run the app on a simulator or physical device, use the following command:

```bash
yarn start
yarn android
yarn ios
```

### Running the Server

Access the server at http://localhost:5000

```bash
yarn start
```

## Deployment

For deployment, you can use Expo's build service to create production-ready builds of your app. You might also deploy the web version using [Vercel](https://vercel.com/) or another hosting service if applicable. Follow these steps:

1. **Build the app**:

    To create production-ready builds for Android and iOS, use the following commands:

    ```bash
    expo build:android
    expo build:ios
    ```
    Or use EAS to build

    Follow Expo's [build documentation](https://docs.expo.dev/build/introduction/) for detailed instructions on building your app.

2. **Deploy the app**:

    - For Android and iOS, follow Expo's [build documentation](https://docs.expo.dev/build/introduction/) to manage and distribute your builds.
    

# RushTranslate

A React Native application for real-time translation using camera and voice features.

## Camera Features

### Core Functionality
- Real-time camera preview using `expo-camera`
- Support for both front and back cameras
- High-quality photo capture
- Image gallery integration
- Permission handling with proper user feedback

### Camera Controls
- Camera flip functionality (front/back)
- Capture button with loading state
- Gallery access button
- Retake option in preview mode

### Text Recognition
- Real-time text detection overlay
- Visual bounding boxes for detected text
- Green highlighting for detected text regions
- Support for multiple text regions simultaneously

### User Interface
- Modern, minimalist design
- Intuitive button layout
- Loading indicators during processing
- Clear visual feedback for user actions
- Semi-transparent overlay for better text visibility
- Responsive layout that works across device sizes

### Error Handling
- Graceful camera mount error handling
- Permission denial handling with retry option
- Process state management for stable operation
- Comprehensive error logging

### Image Processing
- High-quality image capture
- Support for image editing from gallery
- Efficient image processing pipeline
- Temporary image storage management

### Accessibility
- Internationalization support via i18n
- Clear, readable text
- High contrast UI elements
- Proper button sizing for easy interaction

### Technical Improvements
- TypeScript integration for better type safety
- Modern React hooks usage
- Efficient state management
- Clean component architecture
- Proper memory management
- Modular service architecture for text recognition

## Getting Started

1. Install dependencies:
```bash
yarn install
# or
yarn install
```

2. Start the development server:
```bash
yarn start
# or
yarn start
```

3. Run on your device or simulator:
```bash
# For iOS
yarn ios
# or
yarn ios

# For Android
yarn android
# or
yarn android
```

## Dependencies

- expo-camera: ^15.0.16
- expo-image-picker: Latest version
- react-native-vector-icons: Latest version
- i18next: Latest version
- Other dependencies as specified in package.json

## Contributing

Feel free to submit issues and enhancement requests.

## License

This project is licensed under the terms specified in the LICENSE file.
