The offline version editing function of Inkdown is almost identical to the online version and is open source. If you want to completely save data only on your own computer or use your own server to publish your documents, you can use the offline version.

## Deployment Publish Program

Inkdown provides a`Node.js` based network service program, allowing your documents to be easily shared with other readers. He will establish a mapping between the device file path and remote documents, allowing local Markdown documents to be shared to the network with just one click, and will automatically manage image dependencies and link conversions. The usage process is very simple. If you have your own `Linux` server, you can install it in 5 minutes.

### Sharing program features

- No intrusion into local documents, only synchronize relevant files to the server when you want to share local documents.
- Using `sqlite` as data storage, Linux servers generally include the sqlite program by default.
- You can share a single markdown file or the entire folder. The document links in the folder will automatically convert the links when uploaded, and internal documents can use file paths to jump to each other.
- When synchronizing file updates, the `hash` value of the file will be compared, and uploaded documents or images will not be uploaded again.
- The images that the document depends on will be automatically managed. When the document is closed for sharing, the images will be automatically deleted and there will be no redundant files on the server.
- The generated document and editor display effects are completely consistent, using `next.js` as the server-side rendering framework. The network document opens very quickly, and the document is responsive. It can also be read normally on mobile devices.
- The `device ID` and `file path` generate a unique ID on the server, and multiple devices will not affect each other. A server can also be used by multiple people simultaneously.
- The service program has the ability to automatically update. If a new service program is released, an upgrade prompt will appear in the editor. Click the upgrade button to proceed. If the service is not updated, you can try executing `pm2 restart inkdown` in the project directory.

> When renaming or moving files and folders in the Inkdown Editor, if the custom service is enabled, the remote file mapping path will be automatically changed.

### Install

Network programs rely on the latest `nodejs` features and have certain requirements for `Linux` system versions.

- `centos` 8+
- `ubuntu` 20+
- `debian` 10+

Assuming you log in to the server using the `root` account

```sh
# Need unzip decompression program or yum install unzip
apt install unzip
# Install nodejs management tool
curl -fsSL https://fnm.vercel.app/install | bash
source $HOME/.bashrc
# Installing nodejs 20
fnm install 20
# Download and install the Inkdown sharing program
curl -fsSL https://github.com/1943time/bluestone-service/releases/latest/download/install.sh | bash
cd bluestone-service
# Starting network services
pm2 start
```

### Docker

```sh
cd bluestone-server
docker build -t inkdown-image .
docker run -d --name inkdown -p 3006:80 inkdown-image
```

We will expose the Inkdown port to the `3006` port of the host, and you can change it as needed. 

### Settings

There is a `package.json` file in the bluestone service folder. By default, the sharing program uses port `80`. If you want to change the port, you can change the `scripts ->start` field in the JSON file

```json
{
	"scripts": {
		//"start": "next start -p 80",
		// Change port to 3000
		"start": "next start -p 3000"
	}
}
```

In the inkdown field, there are the following configurations that can be customized according to usage

```json
{
	"inkdown": {
	  "secret": "BLUESTONE",
	  "home-site": "",
	  "favicon": "/favicon.png"
  }
}
```

- `secret`Connection key, used in editor connection settings, recommended to be changed.
- `home-site`Configure this field to display the site icon in the upper right corner of the network document, click the link to the homepage.
- `favicon`The icon path of the site is located in the `/public` folder of the service program by default, and can be replaced with other images.

After completing the settings, you can publish the Markdown document with just one click. The local image that the Markdown document depends on will be automatically uploaded when published.

### Nginx

In most cases, the sharing program may not enable port `80`, and you may need an nginx proxy. The following nginx configurations can be used as a reference:

```nginx
server {
    listen      80;
    # listen      443 ssl;
    # ssl_certificate path.pem;
    # ssl_certificate_key path.key;
    # ssl_session_timeout 5m;
    # ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    # ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    # ssl_prefer_server_ciphers on;
    server_name  doc.yourdomain.com;
    gzip on;
    gzip_comp_level 6;
    gzip_min_length 1k;
    gzip_static on;
    gzip_types
    application/javascript
    text/javascript
    text/css
    application/json
    application/manifest+json
    image/svg+xml
    application/wasm;
    client_max_body_size 100M;
    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host  $http_host;
        proxy_set_header X-Nginx-Proxy true;
        proxy_http_version 1.1;
        proxy_pass    http://localhost:3006; # port
        if ($request_filename ~* ^.*?\.(gif|jpg|jpeg|png|css|js|json|ttf|woff|woff2|wasm)$){
           expires  max;
        }
    }
}
```