# SSLMkcert 

Guia simples para configurar **SSL local no Nginx** usando **mkcert** .

---

## 🧩 Instalação

Atualize o sistema:

```bash
sudo apt update
```

Instale o mkcert e dependências:

```bash
sudo apt install mkcert libnss3-tools -y
```

---

## 🪪 Criar a unidade certificadora (CA)

Crie a autoridade certificadora local:

```bash
mkcert -install
```

---

## 🔐 Gerar os certificados

Gere os certificados para o domínio local (exemplo: `gcsi.local`):

```bash
mkcert gcsi.local localhost
```

---

## 📂 Mover os certificados para o diretório do Nginx

```bash
sudo mv gcsi.local.pem gcsi.local-key.pem /etc/nginx/
```

---

## ⚙️ Configurar o Nginx

Edite o arquivo **default** do Nginx:

```bash
sudo nano /etc/nginx/sites-available/default
```

Substitua o conteúdo por este bloco:

```nginx
server {
    listen 443 ssl;
    server_name localhost;

    ssl_certificate /etc/nginx/gcsi.local.pem;
    ssl_certificate_key /etc/nginx/gcsi.local-key.pem;

    root /caminho/do/seu/projeto;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

> 🔧 Substitua `/caminho/do/seu/projeto` pelo diretório onde está sua aplicação.

---

## 🚀 Rodando o projeto

Verifique seu IP local:

```bash
hostname -I
```

Reinicie o Nginx:

```bash
sudo systemctl restart nginx
```

Agora abra o navegador e acesse o seu IP (exemplo: `https://192.168.0.10`) ou `https://gcsi.local` — seu projeto estará rodando com **SSL ativo**. 

---

**Observação:** este procedimento é destinado apenas para uso **local** (desenvolvimento). Para produção, utilize certificados reais como os do **Let's Encrypt**.
