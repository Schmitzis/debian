# EteSync

Configures [EteSync server](https://github.com/etesync/server) on the target host.


## Requirements

- Ansible ≥2.10
- Python ≥3.7


## Role Variables

| Variable            | Description                               | Default                                    |
|---------------------|-------------------------------------------|--------------------------------------------|
| esc_admin           | Name of the EteSync administrator user.   | random word                                |
| esc_admin_email     | E-mail of the administrator user.         | ''                                         |
| esc_admin_password  | Admin user password, plaintext.           | ''                                         |
| esc_cert_email      | E-mail to use for LetsEncrypt deployment. | ''                                         |
| esc_debug           | Deloy in debug mode?                      | False                                      |
| esc_hosts           | List of domains/IPs to listen on.         | ['127.0.0.1']                              |
| esc_http_auth_pass  | Password for the http authentication      | ''                                         |
| esc_http_auth_user  | Username for http auth. (/admin, etc)     | ''                                         |
| esc_path            | Installation path                         | '/var/www/esc_server'                      |
| esc_provision_https | Provision HTTPs?                          | True                                       |
| esc_repository      | GIT repository to clone from.             | 'https://github.com/etesync/server'        |
| esc_rm_existing     | Replace existing EteSync installation?    | False                                      |
| esc_rootdir         | Path to the user data                     | '/var/www/html/esc_server'                 |
| esc_user            | User used to run the server               | 'www-uvicorn'                              |
| esc_version         | SHA1 hash of the commit to checkout.      | '43d5af32d72d5f59de7f31698076becf2430ccaa' |

### Remarks

- It is recommended you do not define `esc_admin` and allow playbook to choose a random word for it.

- If you _do_ define `esc_admin`, then make it is at least 4 characters long.

- Role will not create another superuser if there is already one in the DB.

- UFW/nftables are not covered in this role, use the [base role](../base/):

  ```yaml
  ufw_service:
  - { comment: 'Allow http', rule: 'allow', service: 'Nginx HTTP' }
  - { comment: 'Allow https', rule: 'allow', service: 'Nginx HTTPS' }
  ```

## Dependencies

Technically none, but tested on a remote that is already provisioned with the [base role](../base/).


## License

Apache-2.0


## Author Information

Andrew Savchenko\
https://savchenko.net
