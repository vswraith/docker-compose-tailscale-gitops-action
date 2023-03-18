# Docker Compose Gitops
A [GitHub Action](https://github.com/marketplace/actions/docker-compose-gitops) making GitOps with the simplicity of docker-compose possible, using SSH or optionally Tailscale SSH, with support for docker swarm, uploading directory for bind mounts and other features!

The Action is adapted from work by [TapTap21](https://github.com/TapTap21/docker-remote-deployment-action) and [wshihadeh](https://github.com/marketplace/actions/docker-deployment)


## Example

Here is an example of how to use the action

```yaml
- name: Tailscale
  uses: tailscale/github-action@ce41a99162202a647a4b24c30c558a567b926709
  with:
    authkey: ${{ secrets.TAILSCALE_AUTHKEY }}
    hostname: Github-actions

- name: Start Deployment
  uses: FarisZR/docker-compose-gitops-action@v1
  with:
    remote_docker_host: root@100.107.201.124
    tailscale_ssh: true # no need for manual private and public keys
    compose_file_path: postgres/docker-compose.yml
    upload_directory: true # upload docker directory
    docker_compose_directory: postgres # directory to upload
    docker_login_password: ${{ secrets.DOCKER_REPO_PASSWORD }}
    docker_login_user: ${{ secrets.DOCKER_REPO_USERNAME }}
    docker_login_registry : ${{ steps.login-ecr.outputs.registry }}
    args: -p postgres up -d
```

## Action Inputs

- `args` - Docker compose/stack command arguments. Example: `-p app_stack_name -d up` required
- `remote_docker_host` Specify Remote Docker host. The input value must be in the following format (user@host) required
- `tailscale_ssh` Enables Tailscale ssh mode, which uses managed ssh keys from Tailscale, and skips the private and public keys. default: false
- `ssh_public_key` Remote Docker SSH public key. Required when Tailscale ssh isn't enabled
- `ssh_private_key` SSH private key used in PEM format to connect to the docker host. Required when Tailscale ssh isn't enabled
- `ssh_port` The SSH port to be used. Default is 22.
- `compose_file_path` Docker compose file path. Default is `docker-compose.yml`(repo root), sub-directory Example: `caddy/docker-compose.yml`
- `upload_directory` Uploads docker compose directory, useful for extra files like Configs. Optional
- `docker_compose_directory` Specifies which directory in the repository to upload, needed for upload_directory
- `post_upload_command` Optional input to execute a command post upload, when `upload_directory` is enabled. Useful for things like changing permissions before starting containers.
- `docker_swarm` Uses docker swarm instead of compose by using the docker stack command, default: false
- `docker_login_user` The username for the container repository user. (DockerHub, ECR, etc.). Optional.
- `docker_login_password` The password for the container repository user. 
- `docker_login_registry` The docker container registry to authenticate against Optional

## License

This project is licensed under the MIT license. See the [LICENSE](LICENSE) file for details.
