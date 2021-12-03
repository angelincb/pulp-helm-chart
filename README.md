# Pulp Deployment

**Pulp is an open source  content management software. It is used for managing rpm, container images.**

[Official Documentation](https://docs.pulpproject.org/pulpcore/index.html)


**<pulp-url> variable should be replaced with the URL you want to access Pulp

#### Command used to deploy pulp subchart:
```sh
helm install pulp . 
```
Please Refer [Pulp Architecture](https://docs.pulpproject.org/pulpcore/components.html) to understand all the components.


#### Follow below steps to Create a repository & configure it

1) Login to K8S Master Node


2) Create Remote

    ```sh
    pulp rpm remote create --name <remote_name> --url <remote_url> --policy <immediate/on_demand>
    ```


3) Create a Repository and link it to above remote

    ```sh
    pulp rpm repository create --remote <remote_name> --name <repo_name>
    ```


4) Sync the Repository

    ```sh
    pulp rpm repository sync --name <repo_name>
    ```


5) Create a publication and link it to above repository

    ```sh
    pulp rpm publication create --repository <repo_name>
    ```


6) Create a distribution and link it to above repository/publication

    ```sh
    pulp rpm distribution create --base-path <base_path> --publication <pub_href> --name <dist_name>
    ```

    Once done, the contents of the repository will be available under path as specified in the distribution. It can be viewed with below command

    ```sh
    pulp rpm distribution list
    ```
    


#### Follow below steps to Push a container image into Pulp Registry

1) Tag the image you want to push into Pulp Registry

    ```sh
    docker tag <imageid> <pulp-url>/<custom-repo-name>/<custom-repo-path>

    eg:
  
    docker tag e14416ec0ef5 <pulp-url>/pulp:v2
    ```

2) Login into Pulp Registry

    ```sh
    docker login -u <pulp-admin-user> -p <pulp-admin-password> <pulp-url>:443
    ```

3) Push the Image 

   ```sh
   docker push <pulp-url>/pulp:v2
   ```

4) Verify if your image is successfully pushed

   ```sh
   http GET https://<pulp-url>:443/v2/pulp/tags/list --auth <pulp-admin-user>:<pulp-admin-password>
   ```



Please Refer [Container Workflows](https://docs.pulpproject.org/pulp_container/workflows/index.html) for commands related to container image management
