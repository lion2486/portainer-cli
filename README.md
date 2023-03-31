# Portainer CLI
Powered by [Ilhasoft's Web Team](http://www.ilhasoft.com.br/en/).
由Ilhasoft的Web团队提供支持。（已经停止维护）

Portainer CLI is a Python software to use in command line. Use this command line interface to easy communicate with your [Portainer](https://portainer.io/) application, like in a continuous integration and continuous deployment environments.
Portainer CLI是一个用于命令行的Python软件。在连续集成和连续部署环境中使用该命令行界面，可以轻松与您的Portainer应用程序进行通信。
由Ilhasoft的Web团队提供支持

## Install
## 安装

```bash
pip install [--user] portainer-cli
```
```bash
git clone https://github.com/6bf3803cb456/portainer-cli.git

rm -rf /usr/local/bin/portainer-cli
rm -rf /usr/local/lib/python3.9/dist-packages/portainer_cli

mv portainer-cli /usr/local/bin/
mv portainer_cli/ /usr/local/lib/python3.9/dist-packages/
```

## Usage

### Global flags

| Flag | Description |
|--|--|
| `-l` or `--local` | Save and load configuration file (`.portainer-cli.json`) in current directory. |
| `-d` or `--debug` | Enable DEBUG messages in stdout |

### configure command

Configure Portainer HTTP service base url.

```bash
portainer-cli configure base_url
```

**E.g:**

```bash
portainer-cli configure http://10.0.0.1:9000/
```

### login command

Identify yourself and take action.

```bash
portainer-cli login username password
```

**E.g:**

```bash
portainer-cli login douglas d1234
```

### set_apikey command

Identify yourself using an API-Key. Generate your API-Key using the Portainer Web Interface (account -> add access token).

```bash
portainer-cli set_apikey <your-api-key>
```

**E.g:**

```bash
portainer-cli set_apikey ptr_2Igx1JRPcKAKx5Q7k4R9wAYdLQBhJ+S62g+6sh7fA/w=
```

### create_stack command

Create a stack.

```bash
portainer-cli create_stack -n stack_name -sf stack_file
```

**E.g:**

```bash
portainer-cli create_stack -n traefik -sf docker-compose.yml
```

#### Flags

| Flag | Description |
|--|--|
| `-n` or `-stack-name` | Stack name |
| `-e` or `-endpoint-id` | Endpoint id (required) |
| `-sf` or `-stack-file` |Stack file |
| `-ef` or `-env-file` | Pass env file path, usually `.env` |

### update_stack command

Update a stack.

```bash
portainer-cli update_stack -s stack_id -e endpoint_id -sf stack_file
```

**E.g:**

```bash
portainer-cli update_stack -s 18 -e 1 -sf docker-compose.yml
```

#### Environment variables arguments

```bash
portainer-cli update_stack id -s stack_id -e endpoint_id -sf stack_file --env.var=value
```

Where `var` is the environment variable name and `value` is the environment variable value.

#### Flags

| Flag | Description |
|--|--|
| `-s` or `-stack-id` | Stack id |
| `-e` or `-endpoint-id` | Endpoint id (required) |
| `-sf` or `-stack-file` |Stack file |
| `-ef` or `-env-file` | Pass env file path, usually `.env` |
| `-p` or `--prune` | Prune services |
| `-c` or `--clear-env` | Clear all environment variables |

### create_or_update_stack command

Create or update a stack based on it's name.

```bash
portainer-cli create_or_update_stack -n stack_name -e endpoint_id -sf stack_file
```

**E.g:**

```bash
portainer-cli update_stack -s 18 -e 1 -sf docker-compose.yml
```

#### Environment variables arguments

```bash
portainer-cli create_or_update_stack -n stack_name -e endpoint_id -sf stack_file --env.var=value
```

Where `var` is the environment variable name and `value` is the environment variable value.

#### Flags

| Flag | Description |
|--|--|
| `-n` or `-stack-name` | Stack name |
| `-e` or `-endpoint-id` | Endpoint id (required) |
| `-sf` or `-stack-file` |Stack file |
| `-ef` or `-env-file` | Pass env file path, usually `.env` |
| `-p` or `--prune` | Prune services |
| `-c` or `--clear-env` | Clear all environment variables |

### update_stack_acl command

Update acl associated to a stack

```bash
portainer-cli update_stack_acl -s stack_id -e endpoint_id -o ownership_type
```

Remark : you can either update by stack_id or stack_name (`-s` or `-n`)

**E.g:**

```bash
portainer-cli update_stack_acl -n stack-test -e 1 -o restricted -u user1,user2 -t team1,team2
```

#### Flags

| Flag | Description |
|--|--|
| `-s` or `-stack-id` | Stack id |
| `-n` or `-stack-name` | Stack name |
| `-e` or `-endpoint-id` | Endpoint id (required) |
| `-o` or `-ownership-type` | Ownership type (`admin`|`restricted`,`public`) (required) |
| `-u` or `-users` | Comma separated list of user names (when `restricted`) |
| `-t` or `-teams` | Comma separated list of team names (when `restricted`) |
| `-c` or `-clear` | Clear users and teams before updateing them (when `restricted`) |

### get_stack_id command

Get stack id by it's name. return -1 if the stack does not exist

```bash
portainer-cli get_stack_id -n stack_name -e endpoint_id
```

**E.g:**

```bash
portainer-cli get_stack_id -n stack-test -e 1
```

#### Flags

| Flag | Description |
|--|--|
| `-n` or `-stack-name` | Stack name |
| `-e` or `-endpoint-id` | Endpoint id (required) |

### update_registry command

Update registry.

```bash
portainer-cli update_registry id [-name] [-url]
```

**E.g:**

```bash
portainer-cli update_registry 1 -name="Some registry" -url="some.url.com/r"
```

#### Authentication

You can use authentication passing `-a` or `--authentication` flag, but you must pass the `-username` and `-password` options.

```bash
portainer-cli update_registry 1 -a -username=douglas -password=d1234
```

### request command

Make a request.

```bash
portainer-cli request path [method=GET] [data]
```

**E.g:**

```bash
portainer-cli request status
```

#### Flags

| Flag | Description |
|--|--|
| `-p` or `--printc` | Print response content in stdout. |

## Development

This project use [Pipenv](https://pipenv.readthedocs.io/en/latest/) to manager Python packages.

With Pipenv installed, run `make install` to install all development packages dependencies.

Run `make lint` to run [flake8](http://flake8.pycqa.org/en/latest/) following PEP8 rules.

Run `make` or `make sdist` to create/update `dist` directory.
