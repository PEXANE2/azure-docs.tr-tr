---
title: Bir kapsayıcı grubunda SSL 'yi etkinleştirme
description: Azure Container Instances çalıştıran bir kapsayıcı grubu için SSL veya TLS uç noktası oluşturma
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 541d53a9a9530f7ac80227dbae598b3da2691301
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773069"
---
# <a name="enable-an-ssl-endpoint-in-a-container-group"></a>Bir kapsayıcı grubunda SSL uç noktasını etkinleştirme

Bu makalede, bir uygulama kapsayıcısına ve bir SSL sağlayıcısı çalıştıran bir sepet kapsayıcısına sahip bir [kapsayıcı grubu](container-instances-container-groups.md) oluşturma gösterilmektedir. Ayrı bir SSL uç noktasıyla bir kapsayıcı grubu ayarlayarak, uygulama kodunuzu değiştirmeden uygulamanız için SSL bağlantılarını etkinleştirirsiniz.

İki kapsayıcıdan oluşan örnek bir kapsayıcı grubu ayarlarsınız:
* Genel Microsoft [aci-HelloWorld](https://hub.docker.com/_/microsoft-azuredocs-aci-helloworld) görüntüsünü kullanarak basit bir Web uygulaması çalıştıran bir uygulama kapsayıcısı. 
* SSL kullanmak üzere yapılandırılmış genel [NGINX](https://hub.docker.com/_/nginx) görüntüsünü çalıştıran bir sepet kapsayıcısı. 

Bu örnekte, kapsayıcı grubu yalnızca NGINX için 443 numaralı bağlantı noktasını genel IP adresiyle kullanıma sunar. NGINX, HTTPS isteklerini 80 numaralı bağlantı noktasında dinleme yaptığı yardımcı Web uygulamasına yönlendirir. Diğer bağlantı noktalarını dinleyen kapsayıcı uygulamalarına yönelik örneği uyarlayabilirsiniz. Bir kapsayıcı grubunda SSL 'yi etkinleştirmeye yönelik diğer yaklaşımlara yönelik [sonraki adımlara](#next-steps) bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu makaleyi tamamlayabilmeniz için Azure Cloud Shell veya yerel bir Azure CLı yüklemesi kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.0.55 veya üzeri önerilir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Bir SSL sağlayıcısı olarak NGINX ayarlamak için bir SSL sertifikasına sahip olmanız gerekir. Bu makalede, otomatik olarak imzalanan bir SSL sertifikası oluşturma ve ayarlama işlemlerinin nasıl yapılacağı gösterilir. Üretim senaryolarında, sertifika yetkilisinden bir sertifika edinmeniz gerekir.

Otomatik olarak imzalanan bir SSL sertifikası oluşturmak için Azure Cloud Shell ve birçok Linux dağıtımlarında bulunan [OpenSSL](https://www.openssl.org/) aracını kullanın veya işletim sisteminizde karşılaştırılabilir bir istemci aracı kullanın.

Önce yerel çalışma dizininde bir sertifika isteği (. csr dosyası) oluşturun:

```console
openssl req -new -newkey rsa:2048 -nodes -keyout ssl.key -out ssl.csr
```

Kimlik bilgilerini eklemek için istemleri izleyin. Ortak ad için sertifikayla ilişkili ana bilgisayar adını girin. Parola istendiğinde, parola eklemeyi atlamak için yazmadan ENTER tuşuna basın.

Sertifika isteğinden otomatik olarak imzalanan sertifika (. CRT dosyası) oluşturmak için aşağıdaki komutu çalıştırın. Örneğin:

```console
openssl x509 -req -days 365 -in ssl.csr -signkey ssl.key -out ssl.crt
```

Şu anda dizinde üç dosya görmeniz gerekir: sertifika isteği (`ssl.csr`), özel anahtar (`ssl.key`) ve otomatik olarak imzalanan sertifika (`ssl.crt`). Sonraki adımlarda `ssl.key` ve `ssl.crt` kullanırsınız.

## <a name="configure-nginx-to-use-ssl"></a>NGINX 'i SSL kullanacak şekilde yapılandırma

### <a name="create-nginx-configuration-file"></a>NGINX yapılandırma dosyası oluştur

Bu bölümde, NGINX 'in SSL kullanması için bir yapılandırma dosyası oluşturacaksınız. Aşağıdaki metni`nginx.conf`adlı yeni bir dosyaya kopyalayarak başlayın. Azure Cloud Shell, çalışma dizininizde dosyayı oluşturmak için Visual Studio Code kullanabilirsiniz:

```console
code nginx.conf
```

`location`, `proxy_pass` uygulama için doğru bağlantı noktasıyla ayarladığınızdan emin olun. Bu örnekte, `aci-helloworld` kapsayıcısı için 80 numaralı bağlantı noktasını ayarlayacağız.

```console
# nginx Configuration File
# https://wiki.nginx.org/Configuration

# Run as a less privileged user for security reasons.
user nginx;

worker_processes auto;

events {
  worker_connections 1024;
}

pid        /var/run/nginx.pid;

http {

    #Redirect to https, using 307 instead of 301 to preserve post data

    server {
        listen [::]:443 ssl;
        listen 443 ssl;

        server_name localhost;

        # Protect against the BEAST attack by not using SSLv3 at all. If you need to support older browsers (IE6) you may need to add
        # SSLv3 to the list of protocols below.
        ssl_protocols              TLSv1 TLSv1.1 TLSv1.2;

        # Ciphers set to best allow protection from Beast, while providing forwarding secrecy, as defined by Mozilla - https://wiki.mozilla.org/Security/Server_Side_TLS#Nginx
        ssl_ciphers                ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:ECDHE-RSA-RC4-SHA:ECDHE-ECDSA-RC4-SHA:AES128:AES256:RC4-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!3DES:!MD5:!PSK;
        ssl_prefer_server_ciphers  on;

        # Optimize SSL by caching session parameters for 10 minutes. This cuts down on the number of expensive SSL handshakes.
        # The handshake is the most CPU-intensive operation, and by default it is re-negotiated on every new/parallel connection.
        # By enabling a cache (of type "shared between all Nginx workers"), we tell the client to re-use the already negotiated state.
        # Further optimization can be achieved by raising keepalive_timeout, but that shouldn't be done unless you serve primarily HTTPS.
        ssl_session_cache    shared:SSL:10m; # a 1mb cache can hold about 4000 sessions, so we can hold 40000 sessions
        ssl_session_timeout  24h;


        # Use a higher keepalive timeout to reduce the need for repeated handshakes
        keepalive_timeout 300; # up from 75 secs default

        # remember the certificate for a year and automatically connect to HTTPS
        add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains';

        ssl_certificate      /etc/nginx/ssl.crt;
        ssl_certificate_key  /etc/nginx/ssl.key;

        location / {
            proxy_pass http://localhost:80; # TODO: replace port if app listens on port other than 80
            
            proxy_set_header Connection "";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $remote_addr;
        }
    }
}
```

### <a name="base64-encode-secrets-and-configuration-file"></a>Base64 kodlama gizli dizileri ve yapılandırma dosyası

Base64-NGINX yapılandırma dosyasını, SSL sertifikasını ve SSL anahtarını kodlayın. Sonraki bölümde, kodlanmış içeriği kapsayıcı grubunu dağıtmak için kullanılan bir YAML dosyasına girersiniz.

```console
cat nginx.conf | base64 -w 0 > base64-nginx.conf
cat ssl.crt | base64 -w 0 > base64-ssl.crt
cat ssl.key | base64 -w 0 > base64-ssl.key
```

## <a name="deploy-container-group"></a>Kapsayıcı grubunu dağıt

Artık bir [YAML dosyasında](container-instances-multi-container-yaml.md)kapsayıcı yapılandırmasını belirterek kapsayıcı grubunu dağıtın.

### <a name="create-yaml-file"></a>YAML dosyası oluştur

Aşağıdaki YAML 'yi `deploy-aci.yaml`adlı yeni bir dosyaya kopyalayın. Azure Cloud Shell, çalışma dizininizde dosyayı oluşturmak için Visual Studio Code kullanabilirsiniz:

```console
code deploy-aci.yaml
```

`secret`altında belirtilen base64 kodlu dosyaların içeriğini girin. Örneğin, Base64 kodlamalı her bir dosyanın içeriğini görmek için `cat`. Dağıtım sırasında, bu dosyalar kapsayıcı grubundaki gizli bir [birime](container-instances-volume-secret.md) eklenir. Bu örnekte, gizli birim NGINX kapsayıcısına bağlanır.

```YAML
api-version: 2018-10-01
location: westus
name: app-with-ssl
properties:
  containers:
  - name: nginx-with-ssl
    properties:
      image: nginx
      ports:
      - port: 443
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - name: nginx-config
        mountPath: /etc/nginx
  - name: my-app
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  volumes:
  - secret:
      ssl.crt: <Enter contents of base64-ssl.crt here>
      ssl.key: <Enter contents of base64-ssl.key here>
      nginx.conf: <Enter contents of base64-nginx.conf here>
    name: nginx-config
  ipAddress:
    ports:
    - port: 443
      protocol: TCP
    type: Public
  osType: Linux
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

### <a name="deploy-the-container-group"></a>Kapsayıcı grubunu dağıtma

[Az Group Create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bir bağımsız değişken olarak YAML dosyasını geçirerek, [az Container Create](/cli/azure/container#az-container-create) komutuyla kapsayıcı grubunu dağıtın.

```azurecli
az container create --resource-group <myResourceGroup> --file deploy-aci.yaml
```

### <a name="view-deployment-state"></a>Dağıtım durumunu görüntüle

Dağıtımın durumunu görüntülemek için, aşağıdaki [az Container Show](/cli/azure/container#az-container-show) komutunu kullanın:

```azurecli
az container show --resource-group <myResourceGroup> --name app-with-ssl --output table
```

Başarılı bir dağıtım için çıkış aşağıdakine benzer:

```console
Name          ResourceGroup    Status    Image                                                    IP:ports             Network    CPU/Memory       OsType    Location
------------  ---------------  --------  -------------------------------------------------------  -------------------  ---------  ---------------  --------  ----------
app-with-ssl  myresourcegroup  Running   mcr.microsoft.com/azuredocs/nginx, aci-helloworld        52.157.22.76:443     Public     1.0 core/1.5 gb  Linux     westus
```

## <a name="verify-ssl-connection"></a>SSL bağlantısını doğrula

Çalışan uygulamayı görüntülemek için tarayıcınızda IP adresine gidin. Örneğin, bu örnekte gösterilen IP adresi `52.157.22.76`. NGINX sunucu yapılandırması nedeniyle, çalışan uygulamayı görmek için `https://<IP-ADDRESS>` kullanmanız gerekir. `http://<IP-ADDRESS>` bağlanma girişimleri başarısız olur.

![Bir Azure kapsayıcı örneğinde çalışan uygulamayı gösteren tarayıcı ekran görüntüsü](./media/container-instances-container-group-ssl/aci-app-ssl-browser.png)

> [!NOTE]
> Bu örnek, bir sertifika yetkilisinden değil, otomatik olarak imzalanan bir sertifika kullandığından, tarayıcıda HTTPS üzerinden bağlantı kurulurken bir güvenlik uyarısı görüntülenir. Bu beklenen bir davranıştır.
>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kapsayıcı grubunda çalışan bir Web uygulamasına SSL bağlantılarını etkinleştirmek için bir NGINX kapsayıcısının nasıl ayarlanacağı gösterilmektedir. Bu örneği, 80 numaralı bağlantı noktasında dinleme yapan uygulamalar için uyarlayabilirsiniz. Ayrıca, NGINX yapılandırma dosyasını, bağlantı noktası 80 (HTTP) üzerindeki sunucu bağlantılarını HTTPS kullanacak şekilde otomatik olarak yeniden yönlendirmek üzere güncelleştirebilirsiniz.

Bu makalede dışarıdan yükleme sırasında NGINX kullanılıyorsa, [Caddy](https://caddyserver.com/)gibi başka bir SSL sağlayıcısı da kullanabilirsiniz.

Kapsayıcı grubunuzu bir [Azure sanal ağında](container-instances-vnet.md)dağıtırsanız, aşağıdakiler de dahil olmak üzere arka uç kapsayıcı ÖRNEĞI için SSL uç noktası etkinleştirmek üzere diğer seçenekleri göz önünde bulundurun:

* [Azure İşlev Proxy'leri](../azure-functions/functions-proxies.md)
* [Azure API Management](../api-management/api-management-key-concepts.md)
* [Azure Application Gateway](../application-gateway/overview.md)

Uygulama Ağ Geçidi kullanmak için bkz. örnek [dağıtım şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress-vnet).
