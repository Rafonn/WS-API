## Key Features

* **Resource X:** API used for DB x FrontEnd integration with a real-time chat.
* **Real-time Communication:** Uses WebSockets.
* **Data Persistence:** Connects to a SQL Server database for User x AI conversation information.
* **Containerization:** Ready to be executed in a Docker environment.

---

## Technologies Used

* **Language:** C#
* **Framework:** ASP.NET Core 8
* **Database:** SQL Server
* **WebSockets:** Native ASP.NET Core WebSockets
* **Containerization:** Docker
* **Other important dependencies:**

---

## Prerequisites

Before you begin, you will need to have the following tools installed on your machine:

* [Git](https://git-scm.com)
* [.NET SDK ([Version])](https://dotnet.microsoft.com/download)
* [Docker Desktop](https://www.docker.com/products/docker-desktop)
* [Your preferred DBMS, e.g., PostgreSQL, SQL Server Management Studio] (or a Docker instance of the database)
* (Optional) An IDE like [Visual Studio Code](https://code.visualstudio.com/) or [Visual Studio](https://visualstudio.microsoft.com/)

---

## Environment Setup

1.  **Database Configuration:**
    * Create a database named `[YourDatabaseName]`.
    * Update the connection string in the `appsettings.json` file (or `appsettings.Development.json`):
        ```json
        {
          "ConnectionStrings": {
            "DefaultConnection": "YourConnectionStringHere"
          },
          // ... other configurations
        }
        ```
    * (If using Entity Framework Core Migrations) Apply migrations:
        ```bash
        dotnet ef database update
        ```

---

## Running the Application (Local Development)

### Without Docker

1.  **Restore dependencies:**
    ```bash
    dotnet restore
    ```

2.  **Run the application:**
    ```bash
    dotnet run
    ```
    The API will be available at `http://localhost:[PORT]` or `https://localhost:[HTTPS_PORT]`. The port is usually defined in `Properties/launchSettings.json`.

### With Docker

1.  **Build the Docker image:**
    ```bash
    docker build -t your-api-name .
    ```

2.  **Run the Docker container:**
    * **Make sure your database is accessible by the container.** If the database is running locally (outside a container), you might need to use `host.docker.internal` in the connection string within the Dockerfile or docker-compose.yml.
    * **Basic example:**
        ```bash
        docker run -d -p 8080:80 -e ASPNETCORE_ENVIRONMENT=Development -e ConnectionStrings__DefaultConnection="YourConnectionStringForDocker" --name my-api-container your-api-name
        ```
        In this example:
        * `-d`: Runs in detached mode.
        * `-p 8080:80`: Maps port 8080 of the host to port 80 of the container (where the ASP.NET Core application usually listens for HTTP). Adjust as needed.
        * `-e ASPNETCORE_ENVIRONMENT=Development`: Sets the environment.
        * `-e ConnectionStrings__DefaultConnection="YourConnectionStringForDocker"`: Overrides the connection string for the Docker environment. **Important:** Use `__` (double underscore) to nest JSON keys in environment variables for ASP.NET Core.
        * `--name my-api-container`: Names the container.

    The API will be available at `http://localhost:8080`.

### Using Docker Compose (Recommended for development environment with database)

If you have a `docker-compose.yml` file to manage the API and database together:

1.  **Bring up the services:**
    ```bash
    docker-compose up -d --build
    ```
    This will build the images (if necessary) and start the containers defined in `docker-compose.yml`.

2.  **To stop the services:**
    ```bash
    docker-compose down
    ```

---

## API Endpoints

### Resource A

* **GET /api/resourceA**
    * Description: Returns a list of all items from Resource A.
    * Response: `200 OK`
        ```json
        [
          { "id": 1, "name": "Item 1" },
          { "id": 2, "name": "Item 2" }
        ]
        ```

* **GET /api/resourceA/{id}**
    * Description: Returns a specific item from Resource A.
    * Parameters:
        * `id` (int): Item identifier.
    * Response: `200 OK` or `404 Not Found`
        ```json
        { "id": 1, "name": "Item 1" }
        ```

* **POST /api/resourceA**
    * Description: Creates a new item for Resource A.
    * Request Body:
        ```json
        { "name": "New Item" }
        ```
    * Response: `201 Created`
        ```json
        { "id": 3, "name": "New Item" }
        ```

* **(Other methods: PUT, DELETE etc.)**

## Tests

* **Unit Tests:**
    ```bash
    dotnet test /path/to/your/unitTestsProject.csproj
    ```

* **Integration Tests:**
    ```bash
    dotnet test /path/to/your/integrationTestsProject.csproj
    ```
    *It may be necessary to ensure that the test database is configured and running.*
