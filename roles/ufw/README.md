# UFW

Quick configuration of the `ufw` firewall.

## Requirements

Ansible ≥2.10

## Role Variables

| Variable                  | Description                             | Default |
|---------------------------|-----------------------------------------|---------|
| ufw_enable                | Enable firewall?                        | True    |
| ufw_limit_ssh             | Apply `ufw limit ssh`?                  | False   |
| ufw_logging               | Enable logging?                         | False   |
| ufw_operator_ip_whitelist | Whitelist IP of the current controller? | False   |
| ufw_policy                | Default policy                          | 'deny'  |
| ufw_rule                  | Template for a UFW rule.                | []      |
| ufw_service               | Template for a UFW service.             | []      |

### ufw_rule

Variable accepts the following list of hashes:

| Variable  | Description           | Default | Required | Possible values                    |
|-----------|-----------------------|---------|----------|------------------------------------|
| comment   | Rule comment.         | ""      | no       | Alphanumerics                      |
| direction | Traffic direction     | "in"    | no       | "in", "out"                        |
| from      | Traffic source        | any     | no       | Network address                    |
| to        | Traffic destination   | any     | no       | Network address                    |
| port      | Port, single or range | any     | no       | integer or "int:int"               |
| proto     | Protocol              | any     | no       | "tcp", "udp", "esp", "gre", "any"  |
| rule      | Action taken          | ""      | yes      | "allow", "deny", "limit", "reject" |

Example:

```yaml
ufw_rule:
  - { comment: 'Drop incoming TCP traffic from 192.168.*.* to port 22/tcp',
      direction: 'in',
      from: '192.168.0.0/16',
      port: '22',
      proto: 'tcp',
      rule: 'deny' }
```

Keep in mind that using `proto: any` might have unintended consequences.

#### Quirks

1. If rule specifies a port range, protocol must be set explicitly to "tcp" or "udp".
2. If `to` is undefined, it will be interpreted as `to any`.

### ufw_service

Similar to the above, but provisions service using data from `/etc/ufw/applications.d` and `/etc/services`.
`direction`, `port` and `proto` are replaced with a single variable: `service`.

Example:

```yaml
ufw_service:
  - { comment: 'Reject Yahoo messenger from 192.168.67.83',
      rule: 'deny',
      from: '192.168.67.83',
      service: 'Yahoo' }
```


## Dependencies

None


## License

Apache-2.0


## Author Information

Andrew Savchenko\
https://savchenko.net
