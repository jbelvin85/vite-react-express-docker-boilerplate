# Project Learning Notes

This document captures key information and decisions made during the setup and development of this project.

## Docker Development Environment

Our development environment is orchestrated using Docker Compose to run the frontend and backend in separate, isolated containers. This provides a consistent environment and simplifies development.

### Launching the Application

1.  **Prerequisite**: Ensure Docker Desktop is installed and running.
2.  **Command**: From the project root, run the following command:
    ```bash
    docker-compose up --build
    ```
    - `up`: Starts the containers defined in `docker-compose.yml`.
    - `--build`: Forces a rebuild of the Docker images, which is necessary after changing `Dockerfile`s or application dependencies (`package.json`).

### Service Architecture & Ports

The application is split into two main services, as defined in `docker-compose.yml`:

-   **`client` Service (React Frontend)**
    -   **Container Name**: `vite-react-client`
    -   **Purpose**: Runs the Vite development server for our React application.
    -   **Access URL**: http://localhost:5173
    -   **Key Feature**: Provides Hot Module Replacement (HMR) for instant UI updates during development.

-   **`server` Service (Express Backend)**
    -   **Container Name**: `vite-react-server`
    -   **Purpose**: Runs the Express.js API server.
    -   **API Base URL**: http://localhost:3001
    -   **Debug Port**: `9229` is exposed to allow the VS Code debugger to attach to the running Node.js process.

### Hot Reloading

Hot reloading is enabled for both services. Changes you make to the code on your local machine will automatically be reflected in the running containers without needing a manual restart.

-   **How it works**: The `volumes` section in `docker-compose.yml` maps your local `./client` and `./server` directories directly into their respective containers. The server then uses `nodemon` to watch for file changes and restart automatically.

### VS Code Debugging

The `.vscode/launch.json` file is configured for a seamless full-stack debugging experience.

-   **`Launch Client & Attach to Server`**: This is a compound configuration that you can run from the "Run and Debug" panel (Ctrl+Shift+D). It launches a debuggable Chrome instance for the frontend and attaches to the backend Node.js process simultaneously. This allows you to set breakpoints in both your React components and your Express API routes.