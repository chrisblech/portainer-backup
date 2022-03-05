[![Docker](https://img.shields.io/docker/v/savagesoftware/portainer-backup/latest?color=darkgreen&logo=docker&label=DockerHub%20Latest%20Image)](https://hub.docker.com/repository/docker/savagesoftware/portainer-backup/)
[![NPM](https://img.shields.io/npm/v/portainer-backup?color=darkgreen&logo=npm&label=NPM%20Registry)](https://hub.docker.com/r/savagesoftware/portainer-backup)

# Portainer Backup

(Developed with ♥ by SavageSoftware, LLC.)

---

## Overview

A utility for scripting or scheduling Portainer backups.  This utility can backup the entire Portainer database, optionally protect the archive file with a password and can additionally backup the `docker-compose` files for stacks created in the Portainer web interface.

![SCREENSHOT](https://github.com/SavageSoftware/portainer-backup/raw/master/assets/screenshot.jpg)

[![NPM](https://nodei.co/npm/portainer-backup.png?downloads=true&downloadRank=false&stars=false)](https://www.npmjs.com/package/portainer-backup)

---

## TL;DR

**NodeJS & NPM**

Command to install `portainer-backup` using node's **NPM** command:

```shell
npm install --global portainer-backup   
```

Command to launch `portainer-backup` after installing with NPM to perform a `backup` of your portainer server:

```shell
portainer-backup \
  backup \
  --url "http://portainer:9000" \
  --token "PORTAINER_ACCESS_TOKEN" \
  --directory $PWD/backup  
```

**NPX**

Command to install & launch `portainer-backup` using node's [NPX](https://nodejs.dev/learn/the-npx-nodejs-package-runner) command to perform a `backup` of your portainer server:

```shell
npx portainer-backup \
  backup \
  --url "http://portainer:9000" \
  --token "PORTAINER_ACCESS_TOKEN" \
  --directory $PWD/backup  
```

**DOCKER**

Command to launch `portainer-backup` using a Docker container to perform a `backup` of your portainer server:

```shell
docker run -it --rm \
  --name portainer-backup \
  --volume $PWD/backup:/backup \
  --env PORTAINER_BACKUP_URL="http://portainer:9000" \
  --env PORTAINER_BACKUP_TOKEN="YOUR_ACCESS_TOKEN" \
  savagesoftware/portainer-backup:latest \
  backup
```

---

## Prerequisites

This utility has only been tested on Portainer **v2.11.0** and later.

> **NOTE:** If attempting to use with an older version of Portainer this utility will exit with an error message.  While it is untested, you can use the `--ignore-version` option to bypass the version validation/enforcement.

You will need to obtain a [Portiner Access Token](https://docs.portainer.io/v/ce-2.11/api/access) from your Portainer server from an adminstrative user account.

---

## Supported Commands & Operations

This utility requires a single command to execute one of the built in operations.

| Command    | Description |
| ---------- | ----------- |
| `backup`   | Backup portainer data archive    |
| `schedule` | Run scheduled portainer backups  |
| `stacks`   | Backup portainer stacks          |
| `test`     | Test backup (no files are saved) |
| `info`     | Get portainer server info        |
| `restore`  | Restore portainer data           |

> **NOTE:** The `restore` command is not currently implemented due to issues with the Portainer API.

### Backup

The `backup` operation will perform a single backup of the Portainer data from the specified server.  This backup file will be TAR.GZ archive and can optionally be protected with a password (`--password`).  The process will terminate immedately after the `backup` operation is complete.
 
The following NPX command will perform a `backup` of the Portainer data.
```shell
npx portainer-backup \
  backup \
  --url "http://portainer:9000" \
  --token "PORTAINER_ACCESS_TOKEN" \
  --directory $PWD/backup \
  --overwrite
``` 

The following docker command will perform a `backup` of the Portainer data.
```shell
docker run -it --rm \
  --name portainer-backup \
  --volume $PWD/backup:/backup \
  --env TZ="America/New_York" \
  --env PORTAINER_BACKUP_URL="http://portainer:9000" \
  --env PORTAINER_BACKUP_TOKEN="PORTAINER_ACCESS_TOKEN" \
  --env PORTAINER_BACKUP_PASSWORD=""  \
  --env PORTAINER_BACKUP_OVERWRITE=true  \
  --env PORTAINER_BACKUP_DIRECTORY=/backup \
  savagesoftware/portainer-backup:latest \
  backup
``` 

### Test

The `test` operation will perform a single backup of the Portainer data from the specified server.  With the `test` operation, no data will be saved on the filesystem.  The `test` operation is the same as using the `--dryrun` option.  The process will terminate immedately after the `test` operation is complete.
 
The following NPX command will perform a `test` of the Portainer data.
```shell
npx portainer-backup \
  test \
  --url "http://portainer:9000" \
  --token "PORTAINER_ACCESS_TOKEN" \
  --directory $PWD/backup
``` 

The following docker command will perform a `test` of the Portainer data.
```shell
docker run -it --rm \
  --name portainer-backup \
  --volume $PWD/backup:/backup \
  --env TZ="America/New_York" \
  --env PORTAINER_BACKUP_URL="http://portainer:9000" \
  --env PORTAINER_BACKUP_TOKEN="PORTAINER_ACCESS_TOKEN" \
  --env PORTAINER_BACKUP_DIRECTORY=/backup \
  savagesoftware/portainer-backup:latest \
  test
``` 


### Schedule

The `schedule` operation will perform continious scheduled backups of the Portainer data from the specified server.  The `--schedule` option or `PORTAINER_BACKUP_SCHEDULE` environment variable takes a cron-like string expression to define the backup schedule.  The process will run continiously unless a validation step fails immediately after startup.
 
The following NPX command will perform a `test` of the Portainer data.
```shell
npx portainer-backup \
  schedule \
  --url "http://portainer:9000" \
  --token "PORTAINER_ACCESS_TOKEN" \
  --directory $PWD/backup \
  --overwrite \
  --schedule "0 0 0 * * *"
``` 

The following docker command will perform a `schedule` of the Portainer data.
```shell
docker run -it --rm \
  --name portainer-backup \
  --volume $PWD/backup:/backup \
  --env TZ="America/New_York" \
  --env PORTAINER_BACKUP_URL="http://portainer:9000" \
  --env PORTAINER_BACKUP_TOKEN="PORTAINER_ACCESS_TOKEN" \
  --env PORTAINER_BACKUP_PASSWORD=""  \
  --env PORTAINER_BACKUP_OVERWRITE=true  \
  --env PORTAINER_BACKUP_DIRECTORY=/backup \
  --env PORTAINER_BACKUP_SCHEDULE="0 0 0 * * *" \
  savagesoftware/portainer-backup:latest \
  schedule
``` 

### Info

The `info` operation will perform an information request to the specified Portainer server.  The process will terminate immedately after the `info` operation is complete.
 
The following NPX command will perform a `info` from the Portainer server.
```shell
npx portainer-backup info --url "http://portainer:9000"
``` 

The following docker command will perform a `info` request from the Portainer data.
```shell
docker run -it --rm \
  --name portainer-backup \
  --env PORTAINER_BACKUP_URL="http://portainer:9000" \
  savagesoftware/portainer-backup:latest \
  info
``` 

---

## Return Value

This utility will return a numeric value after the process exits. 

| Value | Description |
| ----- | ----------- |
| 0     | Utility executed command successfully   |
| 1     | Utility encountered an error and failed |

---

## Supported Command Line Options & Environment Variables

This utility supports both command line arguments and environment variables for all configuration options.

| Option      | Environment Variable | Type | Description |
| ----------- | -------------------- | ---- | ----------- |
| `-t`, `--token`                     | `PORTAINER_BACKUP_TOKEN`          | string      | Portainer access token |
| `-u`, `--url`                       | `PORTAINER_BACKUP_URL`            | string      | Portainer base url |
| `-Z`, `--ignore-version`            | `PORTAINER_BACKUP_IGNORE_VERSION` | true\|false | Bypass portainer version check/enforcement |
| `-d`, `--directory`, `--dir`        | `PORTAINER_BACKUP_DIRECTORY`      | string      | Backup directory/path |
| `-f`, `--filename`                  | `PORTAINER_BACKUP_FILENAME`       | string      | Backup filename |
| `-p`, `--password`, `--pw`          | `PORTAINER_BACKUP_PASSWORD`       | string      | Backup archive password |
| `-o`, `--overwrite`                 | `PORTAINER_BACKUP_OVERWRITE`      | true\|false | Overwrite existing files |
| `-s`, `--schedule`, `--sch`         | `PORTAINER_BACKUP_SCHEDULE`       | string      | Cron expression for scheduled backups |
| `-i`, `--include-stacks`, `--stacks`| `PORTAINER_BACKUP_STACKS`         | true\|false | Include stack files in backup |
| `-q`, `--quiet`                     | `PORTAINER_BACKUP_QUIET`          | true\|false | Do not display any console output |
| `-D`, `--dryrun`                    | `PORTAINER_BACKUP_DRYRUN`         | true\|false | Execute command task without persisting any data |
| `-X`, `--debug`                     | `PORTAINER_BACKUP_DEBUG`          | true\|false | Print stack trace for any errors encountered|
| `-J`, `--json`                      | `PORTAINER_BACKUP_JSON`           | true\|false | Print formatted/strucutred JSON data |
| `-c`, `--concise`                   | `PORTAINER_BACKUP_CONCISE`        | true\|false | Print concise/limited output |
| `-v`, `--version`                   |  _(N/A)_                          |             | Show utility version number |
| `-h`, `--help`                      |  _(N/A)_                          |             | Show help |

> **NOTE:** If both an environment variable and a command line option are configured for the same option, the command line option will take priority.

---

## Schedule Expression

This utility accepts a cron-like expression via the `--schedule` option or `PORTAINER_BACKUP_SCHEDULE` environment variable 

> **NOTE:** Additional details on the supported cron syntax can be found here: https://github.com/node-cron/node-cron/blob/master/README.md#cron-syntax


```
Syntax Format:

    ┌──────────────────────── second (optional)
    │   ┌──────────────────── minute
    │   │   ┌──────────────── hour
    │   │   │   ┌──────────── day of month
    │   │   │   │   ┌──────── month
    │   │   │   │   │   ┌──── day of week
    │   │   │   │   │   │
    │   │   │   │   │   │
    *   *   *   *   *   *

Examples:

    0   0   0   *   *   *   Daily at 12:00am
    0   0   5   1   *   *   1st day of month @ 5:00am
    0 */15  0   *   *   *   Every 15 minutes
```

### Allowed field values

|     field    |        value        |
|--------------|---------------------|
|    second    |         0-59        |
|    minute    |         0-59        |
|     hour     |         0-23        |
| day of month |         1-31        |
|     month    |     1-12 (or names) |
|  day of week |     0-7 (or names, 0 or 7 are sunday)  |

#### Using multiples values

| Expression | Description |
| ---------- | ----------- |
| `0  0  4,8,12  *  *  *` | Runs at 4p, 8p and 12p |

#### Using ranges

| Expression | Description |
| ---------- | ----------- |
| `0  0  1-5  *  *  *` | Runs hourly from 1 to 5 |

#### Using step values

Step values can be used in conjunction with ranges, following a range with '/' and a number. e.g: `1-10/2` that is the same as `2,4,6,8,10`. Steps are also permitted after an asterisk, so if you want to say “every two minutes”, just use `*/2`.

| Expression | Description |
| ---------- | ----------- |
| `0  0  */2  *  *  *` | Runs every 2 hours |

#### Using names

For month and week day you also may use names or short names. e.g:

| Expression | Description |
| ---------- | ----------- |
| `* * * * January,September Sunday` | Runs on Sundays of January and September |
| `* * * * Jan,Sep Sun` | Runs on Sundays of January and September |

---

## Command Line Help

Use the `help` command or `--help` option to see a listing of command line options directly via the CLI.

```
  ___         _        _                ___          _             
 | _ \___ _ _| |_ __ _(_)_ _  ___ _ _  | _ ) __ _ __| |___  _ _ __ 
 |  _/ _ \ '_|  _/ _` | | ' \/ -_) '_| | _ \/ _` / _| / / || | '_ \
 |_| \___/_|  \__\__,_|_|_||_\___|_|   |___/\__,_\__|_\_\\_,_| .__/
                                                             |_|   
┌──────────────────────────────────────────────────────────────────┐
│   Made with ♥ by SavageSoftware, LLC © 2022    (Version 0.0.1)   │
└──────────────────────────────────────────────────────────────────┘

Usage: <command> [(options...)]

Commands:
  portainer-backup backup              Backup portainer data
  portainer-backup schedule            Run scheduled portainer backups
  portainer-backup stacks              Backup portainer stacks
  portainer-backup info                Get portainer server info
  portainer-backup test                Test backup data & stacks (backup --dryru
                                       n --stacks)
  portainer-backup restore <filename>  Restore portainer data
  portainer-backup help                Show help
  portainer-backup version             Show version

Portainer Options:
  -t, --token           Portainer access token
          [string]                                                 [default: ""]
  -u, --url             Portainer base url
                                     [string] [default: "http://portainer:9000"]
  -Z, --ignore-version  Bypass portainer version check/enforcement
                                                      [boolean] [default: false]

Backup Options:  (applies only to 'backup' command)
  -d, --directory, --dir          Backup directory/path
                                                   [string] [default: "/backup"]
  -f, --filename                  Backup filename
                                   [string] [default: "portainer-backup.tar.gz"]
  -p, --password, --pwd           Backup archive password [string] [default: ""]
  -o, --overwrite                 Overwrite existing files
                                                      [boolean] [default: false]
  -s, --schedule, --sch           Cron expression for scheduled backups
                                             [string] [default: "0 0 0 * * * *"]
  -i, --include-stacks, --stacks  Include stack files in backup
                                                      [boolean] [default: false]

Stacks Options:  (applies only to 'stacks' command)
  -d, --directory, --dir  Backup directory/path    [string] [default: "/backup"]
  -o, --overwrite         Overwrite existing files    [boolean] [default: false]

Restore Options:  (applies only to 'restore' command)
  -p, --password, --pwd  Backup archive password          [string] [default: ""]

Options:
  -h, --help     Show help                                             [boolean]
  -q, --quiet    Do not display any console output    [boolean] [default: false]
  -D, --dryrun   Execute command task without persisting any data.
                                                      [boolean] [default: false]
  -X, --debug    Print details stack trace for any errors encountered
                                                      [boolean] [default: false]
  -J, --json     Print formatted/strucutred JSON data [boolean] [default: false]
  -c, --concise  Print concise/limited output         [boolean] [default: false]
  -v, --version  Show version number                                   [boolean]

Command Examples:
  info     --url http://192.168.1.100:9000

  backup   --url http://192.168.1.100:9000
           --token XXXXXXXXXXXXXXXX
           --overwrite
           --stacks

  stacks   --url http://192.168.1.100:9000
           --token XXXXXXXXXXXXXXXX
           --overwrite

  restore  --url http://192.168.1.100:9000
           --token XXXXXXXXXXXXXXXX
           ./file-to-restore.tar.gz

  schedule --url http://192.168.1.100:9000
           --token XXXXXXXXXXXXXXXX
           --schedule "0 0 0 * * *"

Schedule Expression Examples: (cron syntax)

    ┌──────────────────────── second (optional)
    │   ┌──────────────────── minute
    │   │   ┌──────────────── hour
    │   │   │   ┌──────────── day of month
    │   │   │   │   ┌──────── month
    │   │   │   │   │   ┌──── day of week
    │   │   │   │   │   │
    │   │   │   │   │   │
    *   *   *   *   *   *

    0   0   0   *   *   *   Daily at 12:00am
    0   0   5   1   *   *   1st day of month @ 5:00am
    0 */15  0   *   *   *   Every 15 minutes

    Additional Examples @ https://github.com/node-cron/node-cron#cron-syntax
```

---

## Docker Compose

Alternatively you can use a `docker-compose.yml` file to launch your portainer-backup container.  Below is a sample `docker-compose.yml` file you can use to get started:

```yaml
version: '3.8'
services:
  portainer-backup:
    container_name: portainer-backup
    image: savagesoftware/portainer-backup:latest
    hostname: portainer-backup
    restart: unless-stopped
    command: schedule
    environment:
      TZ: America/New_York
      PORTAINER_BACKUP_URL: "http://portainer:9000"
      PORTAINER_BACKUP_TOKEN: "PORTAINER_ACCESS_TOKEN"
      PORTAINER_BACKUP_PASSWORD: ""
      PORTAINER_BACKUP_OVERWRITE: 1
      PORTAINER_BACKUP_SCHEDULE: "0 0 0 * * *"
      PORTAINER_BACKUP_STACKS: 1
      PORTAINER_BACKUP_DRYRUN: 0
      PORTAINER_BACKUP_CONCISE: 1
      PORTAINER_BACKUP_DIRECTORY: "/backup"
      PORTAINER_BACKUP_FILENAME: "portainer-backup.tar.gz"
    volumes:
      - /var/backup:/backup
```

Just run the `docker-compose up -d` command in the same directory as your `docker-compose.yml` file to launch the container instance.

---
