### 3-1 Document your inventory and base commands

- ansible_user: specifies the SSH user used to connect to the remote server (admin).
- ansible_ssh_private_key_file: indicates the path to the private SSH key stored.
- prod group: contains the production host.

### 3-2 Document your playbook

All Docker installation tasks were moved into roles/docker/tasks/main.yml.
The main playbook (playbook.yml) now only defines the target hosts and calls the docker role.

### 3-3 Document your docker_container tasks configuration.

db: runs stiivelee/tp-devops-database:latest, uses env vars from /opt/app.env, attached to app_network.

app: runs stiivelee/tp-devops-simple-api:latest, exposes port 8080, uses /opt/app.env, connected to app_network and proxy_network.

proxy: runs stiivelee/tp-devops-httpd:latest, exposes port 80, connected to proxy_network to forward requests to backend.

### 3.4 Is it really safe to deploy automatically every new image on the hub ? explain. What can I do to make it more secure?

Not completely, automatically deploying every image can be risky if a broken or compromised image is pushed.
To make it more secure : add tests in CI before deployment (build - test - deploy), sign Docker images to ensure authenticity.



