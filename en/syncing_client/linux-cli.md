# Seafile client for a Cli server

## Installation

Since 9.0.7 version, we provide the command-line client in AppImage format, which is used in the same way as seaf-cli. After you download `Seafile-cli-x86_64-x.y.z.AppImage` (e.g. `Seafile-cli-x86_64-9.0.7.AppImage`) from [our website](https://www.seafile.com/en/download/), you can rename `Seafile-cli-x86_64-x.y.z.AppImage` to `seaf-cli` and then copy `seaf-cli` to the system path.

```
chmod +x Seafile-cli-x86_64-9.0.7.AppImage
mv Seafile-cli-x86_64-9.0.7.AppImage seaf-cli
sudo cp seaf-cli /usr/local/bin
```

`Seafile-x86_64.AppImage` require FUSE version 2 to run. If your system does not have FUSE installed, please refer to <https://github.com/AppImage/AppImageKit/wiki/FUSE> to install it.

## Basic Usage

Initialize & start the client

```sh
# choose a folder where to store the seafile client settings e.g ~/seafile-client
mkdir ~/seafile-client            # create the settings folder
seaf-cli init -d ~/seafile-client  # initialise seafile client with this folder
seaf-cli start

```

Download and sync a library from a server. 

* retrieve the library id by browsing into a library on the server. The ID is part of the URL. The format looks like `f4962ce9-ba07-47b8-a83a-73dd96c2ebfd` .
* then

```sh
seaf-cli download -l "the id of the library" -s  "the url + port of server" -d "the folder where the library folder will be downloaded" -u "username on server" [-p "password"]
seaf-cli status  # check status of ongoing downloads
# Name  Status  Progress
# Apps    downloading     9984/10367, 9216.1KB/s

```

Note: if you don't supply the password parameter in the command, it will be asked later, which is more secure.

Example: `seaf-cli download -l 0536c006-8a43-449e-8718-39f12111620d -s http://cloud.seafile.com -d /tmp -u freeplant@test.com`

The above command will create a **new folder** with the same name as the library under the specified folder.

You can also sync a library with an **existing folder** on the local computer. The existing files in the local folder will be merged with the files in the library.

```sh
seaf-cli sync -l "the id of the library" -s  "the url + port of server" -d "the folder which the library will be synced with" -u "username on server" [-p "password"]
```

After running the `download` or `sync` command, the local folder will be automatically synced with the library.

## Detailed documentation

seaf-cli is the command line interface for seafile client.

Subcommands:

```
    init                Initialize config directory
    start               Start ccnet and seafile daemon
    stop                Stop ccnet and seafile daemon
    list                List local libraries
    list-remote         List remote libraries
    status              Show syncing status
    download            Download and sync a library from seafile server
    download-by-name    Download and sync a library defined by name from seafile server
    sync                Sync a library with an existing folder
    desync              De-sync a library with seafile server
    create              Create a library
    config              Configure seafile client

```

Running `seaf-cli -h` will show the above help. For each subcommand, you can also use `-h` option to get help, e.g. `seaf-cli download -h`.

Seafile client stores all its configure information in a config dir. The default location is `~/.ccnet`. All the commands below accept an option `-c <config-dir>`.

### init

Initialize seafile client. This command initializes the config dir. It also creates sub-directories `seafile-data` and `seafile` under `parent-dir`. `seafile-data` is used to store internal data, while `seafile` is used as the default location put downloaded libraries.

```
seaf-cli init [-c <config-dir>] -d <parent-dir>

```

A file named `seafile.ini` will be created under `~/.ccnet` to record the location of `seafile-data` directory.

If you want to run multiple instances of Seafile cli client in the same machine, you can specify different `config-dir` and `parent-dir` when initializing different client instances. Then the instances can run without interfering each others. When starting the instances, just specify ccnet config directories with the `-c` option.

### start

Start seafile client. This command starts  `seaf-daemon`  , which is the file syncing engine for Seafile client.

```
seaf-cli start [-c <config-dir>]

```

### stop

Stop seafile client.

```
seaf-cli stop [-c <config-dir>]

```

### Download/Download-by-name

Download and sync a library from seafile server. It will create a **new folder** with the same name as the library under the parent folder. The local folder will be automatically synced with the library. The `download-by-name` command works similarly, but can save you from finding the library ID. It only works when the library name is unique on the server.

```
seaf-cli download -l <library-id> -s <seahub-server-url> -d <parent-directory> -u <username> [-p <password>]
```

### sync

Synchronize a library with an existing folder. The existing files in the local folder will be merged with the files in the library.

```
seaf-cli sync -l <library-id> -s <seahub-server-url> -d <existing-folder> -u <username> [-p <password>]
```

### desync

Desynchronize a library from seafile server. After running this command, the local folder will no longer be synced with the server.

```
seaf-cli desync -d <existing-folder>

```

### create

Create a new library on server

```
seaf-cli create [-h] -n library-name -t description [-e library-password] -s server -u username -p password
```

### list

List information about synced libraries. The information includes library name, library ID and local folder path for the library.

```
seaf-cli list [-c <config-dir>] [--json]

```

### list-remote

List information about accessible libraries on the server. The information includes library names and ID.

```
seaf-cli list-remote -s <seahub-server-url> -u <username> [-p <password>] [-c <config-dir>] [--json]
```

### status

List syncing status of libraries. This will return the name, syncing status and progress information about all local libraries.

```
seaf-cli status

```

The returned status and their meaning:

| status                | meaning                                                        |
| :-------------------- | :------------------------------------------------------------- |
| synchronized          | Local folder is consistent with the remote library             |
| committing            | Files in local folder are being indexed                        |
| initializing          | Getting sync information from server                           |
| downloading file list | Downloading file list from server. Progress will be displayed. |
| downloading files     | Downloading files from server. Progress will be displayed.     |
| uploading             | Uploading files to server. Progress will be displayed.         |
| error                 | Error message will be displayed in the progress column.        |

### Skip SSL certificate verify

If you're using self-signed certificate on the server, you should ask the client to skip verifying certificate.

```
seaf-cli config -k disable_verify_certificate -v true

```

### Set Transfer Speed Limit

Set upload speed limit to 1MB/s :

```sh
seaf-cli config -k upload_limit -v 1000000

```

Set download speed limit to 1MB/s :

```sh
seaf-cli config -k download_limit -v 1000000

```

### Two factor authentication

seaf-cli supports 'Two Factor Authentication'.

If you want to use the feature, you should add the argument `--tfa <token>` to any `seaf-cli` commands.  `<token>` is Google Authenticator's verification code.

For example:

```sh
seaf-cli download -l "4b11d9d4-e3b1-4394-be85-9d4a80f626fa" -s "https://demo.seafile.top" -d "testst" -u "abc@abc.com" -p "abc" --tfa 002755
```

### Authenticate with Tokens

If your server uses SSO (Single Sign-On) for login, you cannot use password to login from CLI. To enable using CLI in such cases, we provide an option to authenticate with an API token since seafile client version 8.0.4. You should be able to get your API token from profile page in the web interface. (You should run 8.0.6 server .) Use "-T token" option instead of "-p password" to authenticate in the following commands:
```sh
seaf-cli create
seaf-cli download
seaf-cli sync
seaf-cli list-remote
```

### Get argument from configuration file
Since seafile client version 9.0.6, seaf-cli supports reading argument from the configuration file for create/list-remote/download command. You can modify the `seafile.ini` and add the following lines:
```
[account]
# your seafile server address
server = https://cloud.seafile.com
# your user account
user = test@seafile.com
# your API token
token = web-api-auth-token-value-from-your-profile
```
Then you can use commands without giving server, user and token on the command-line:
```
seaf-cli list-remote -C seafile.ini
```
