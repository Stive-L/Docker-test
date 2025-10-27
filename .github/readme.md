### 2-1 What are testcontainers?

Testcontainers are Java libraries used to run lightweight Docker containers during tests.

### 2-2 For what purpose do we need to use secured variables?

Secured variables are used to store sensitive information such as token or passwords, preventing them from being exposed publicly in the repository or in the workflow files.         

### 2-3 Why did we put needs: build-and-test-backend on this job? Maybe try without this and you will see!

We put needs: test-backend so that the Docker build and push job only runs if the backend tests pass successfully.

### 2-4 For what purpose do we need to push docker images?

We need to push Docker images to Docker Hub so that they can be stored, shared, and deployed easily on other environments