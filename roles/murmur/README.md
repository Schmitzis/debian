# murmur

Install and configure murmur-server (aka Mumble server).

## Requirements

- Ansible ≥2.10 on the controller

## Role Variables

| Variable                  | Description                   | Default                                      |
|---------------------------|-------------------------------|----------------------------------------------|
| mur_acme_path             | Path to install acme.sh       | '{{ ansible_env.HOME }}/acme'                |
| mur_acme_repository       | ACME git repository           | 'https://github.com/acmesh-official/acme.sh' |
| mur_acme_version          | Commit hash to checkout       | '8fcecd59a0fd991f1fb4248692af63889bb90d81'   |
| mur_certrequired          | Require client certificates?  | True                                         |
| mur_domain                | Domain to listen on           | 'mumble.example.com'                         |
| mur_email                 | LetsEncrypt notifications     | 'notification@example.com'                   |
| mur_from                  | Allow connections from        | 'any'                                        |
| mur_host                  | Address on which to listen    | localhost                                    |
| mur_logdays               | Days to retain logs           | 30                                           |
| mur_max_users             | Total number of users allowed | 100                                          |
| mur_max_users_per_channel | Maximum users per channel     | 10                                           |
| mur_obfuscate_ip_in_logs  | Obfuscate client IP in logs?  | False                                        |
| mur_port                  | Port to listen on (TCP/UDP)   | 64738                                        |
| mur_server_password       | Password to join the server   | ''                                           |
| mur_ssl_cert              | Path to the SSL certificate   | ''                                           |
| mur_ssl_key               | Path to the cert. private key | ''                                           |

## Dependencies

None.

## License

Apache 2.0

## Author Information

Andrew Savchenko\
https://savchenko.net
