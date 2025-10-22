# SSL + Mkcert no NGinx
Instalação do MkCert para a geração de certificados em ambiente local. 
## Instalação e configuração

Instalar o pacote de ferramentas de NSS( Network Security Services ):

```bash
sudo apt update
sudo apt install libnss3-tools
```

### Instalação do MkCert:

```bash
sudo apt install mkcert
```

#### Criar a pasta ssl:

```bash
sudo mkdir /etc/nginx/ssl
```

#### Gerar certificado:
navegue até a pasta ssl

```
cd /etc/nginx/ssl
```

criação do certificado

```bash
mkcert -install
mkcert gcsi.local localhost 127.0.0.1 ::1
```

### Configuração do nginx
#### editar o arquivo default:
```
sudo nano /etc/nginx/sites-available/default
```

edite as linhas de listen, deixe elas iguais a essas:
```
listen 443 ssl default_server;
listen [::]:443 ssl default_server;
```
adicione essas duas linhas abaixo da linha "`server_name localhost;`"
```
ssl_certificate /etc/nginx/ssl/gcsi.local+3.pem;
ssl_certificate_key /etc/nginx/ssl/gcsi.local+3-key.pem;
```
#### exemplo de como deve ficar o default:
```
server {
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    server_name localhost;

    # Certificados gerados com mkcert
    ssl_certificate     /etc/nginx/ssl/gcsi.local+3.pem;
    ssl_certificate_key /etc/nginx/ssl/gcsi.local+3-key.pem;

    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

#### Reinicie o nginx:
```
sudo systemctl stop nginx
sudo systemctl start nginx
```
## Pronto! 🎉🎉🎉💥🎈🎈
agora verifique no seu navegador acessando:
`https://localhost` ou `https://127.0.0.1`
