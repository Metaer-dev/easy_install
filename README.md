# EASY INSTALL [中文](README-ZH.md)

The Easy Install script should get you going with a Frappe/ERPNext setup with minimal manual intervention and effort.

This script uses Docker with the Frappe/ERPNext Docker Repository and can be used for both Development setup and Production setup.

#### Setup

clone the repo and execute it:

```sh
$ python3 easy-install.py deploy --email=user@domain.tld --sitename=subdomain.domain.tld --app=erpnext
```

This script will install docker on your system and will fetch the required containers, setup bench and apps, default app is ERPNext instance.

The script will generate MySQL root password and an Administrator password for the Frappe/ERPNext instance, which will then be saved under `$HOME/passwords.txt` of the user used to setup the instance.
It will also generate a new compose file under `$HOME/<project-name>-compose.yml`.

When the setup is complete, you will be able to access the system at `http://<your-server-ip>`, wherein you can use the Administrator password to login.

#### Arguments

Here are the arguments for the easy-install script

**Build custom images**

```txt
usage: easy-install.py build [-h] [-n PROJECT] [-i IMAGE] [-q] [-m HTTP_PORT] [-v VERSION] [-a APPS] [-s SITES] [-e EMAIL]
                             [-p] [-r FRAPPE_PATH] [-b FRAPPE_BRANCH] [-j APPS_JSON] [-t TAGS] [-c CONTAINERFILE]
                             [-y PYTHON_VERSION] [-d NODE_VERSION] [-x] [-u]

options:
  -h, --help            show this help message and exit
  -n PROJECT, --project PROJECT
                        Project Name
  -i IMAGE, --image IMAGE
                        Full Image Name
  -q, --no-ssl          No https
  -m HTTP_PORT, --http-port HTTP_PORT
                        Http port in case of no-ssl
  -v VERSION, --version VERSION
                        ERPNext version to install, defaults to latest stable
  -a APPS, --app APPS   list of app(s) to be installed
  -s SITES, --sitename SITES
                        Site Name(s) for your production bench
  -e EMAIL, --email EMAIL
                        Add email for the SSL.
  -p, --push            Push the built image to registry
  -r FRAPPE_PATH, --frappe-path FRAPPE_PATH
                        Frappe Repository to use, default: https://github.com/frappe/frappe
  -b FRAPPE_BRANCH, --frappe-branch FRAPPE_BRANCH
                        Frappe branch to use, default: version-15
  -j APPS_JSON, --apps-json APPS_JSON
                        Path to apps json, default: frappe_docker/development/apps-example.json
  -t TAGS, --tag TAGS   Full Image Name(s), default: custom-apps:latest
  -c CONTAINERFILE, --containerfile CONTAINERFILE
                        Path to Containerfile: images/layered/Containerfile
  -y PYTHON_VERSION, --python-version PYTHON_VERSION
                        Python Version, default: 3.11.6
  -d NODE_VERSION, --node-version NODE_VERSION
                        NodeJS Version, default: 18.18.2
  -x, --deploy          Deploy after build
  -u, --upgrade         Upgrade after build
```

**Deploy using compose**

```txt
usage: easy-install.py deploy [-h] [-n PROJECT] [-i IMAGE] [-q] [-m HTTP_PORT] [-v VERSION] [-a APPS] [-s SITES] [-e EMAIL]

options:
  -h, --help            show this help message and exit
  -n PROJECT, --project PROJECT
                        Project Name
  -i IMAGE, --image IMAGE
                        Full Image Name
  -q, --no-ssl          No https
  -m HTTP_PORT, --http-port HTTP_PORT
                        Http port in case of no-ssl
  -v VERSION, --version VERSION
                        ERPNext version to install, defaults to latest stable
  -a APPS, --app APPS   list of app(s) to be installed
  -s SITES, --sitename SITES
                        Site Name(s) for your production bench
  -e EMAIL, --email EMAIL
                        Add email for the SSL.
```

**Upgrade existing project**

```txt
usage: easy-install.py upgrade [-h] [-n PROJECT] [-i IMAGE] [-q] [-m HTTP_PORT] [-v VERSION]

options:
  -h, --help            show this help message and exit
  -n PROJECT, --project PROJECT
                        Project Name
  -i IMAGE, --image IMAGE
                        Full Image Name
  -q, --no-ssl          No https
  -m HTTP_PORT, --http-port HTTP_PORT
                        Http port in case of no-ssl
  -v VERSION, --version VERSION
                        ERPNext or image version to install, defaults to latest stable
```

**Development setup using compose**

```txt
usage: easy-install.py develop [-h] [-n PROJECT]

options:
  -h, --help            show this help message and exit
  -n PROJECT, --project PROJECT
                        Compose project name
```

**Exec into existing project**

```txt
usage: easy-install.py exec [-h] [-n PROJECT]

options:
  -h, --help            show this help message and exit
  -n PROJECT, --project PROJECT
                        Project Name
```

To use custom apps, you need to create a json file with list of apps and pass it to build command.

Example apps.json

```json
[
  {
    "url": "https://github.com/frappe/wiki.git",
    "branch": "master"
  }
]
```

Execute following command to build and deploy above apps:

```sh
$ python3 easy-install.py build \
	--tag=ghcr.io/org/repo/custom-apps:latest \
	--push \
	--image=ghcr.io/org/repo/custom-apps \
	--version=latest \
	--deploy \
	--project=actions_test \
	--email=test@frappe.io \
	--apps-json=apps.json \
	--app=wiki
```

Note:

- `--tag`, tag to set for built image, can be multiple.
- `--push`, push the built image.
- `--image`, the image to use when starting docker compose project.
- `--version`, the version to use when starting docker compose project.
- `--app`, app to install on site creation, can be multiple.
- `--deploy`, flag to deploy after build/push is complete
- `--project=actions_test`, name of the project, compose file with project name will be stored in user home directory.
- `--email=test@frappe.io`, valid email for letsencrypt certificate expiry notification.
- `--apps-json`, path to json file with list of apps to be added to bench.

#### Troubleshooting

In case the setup fails, the log file is saved under `$HOME/easy-install.log`. You may then

- Create an Issue in this repository with the log file attached.