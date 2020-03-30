---
title: Sidecar konteyner ile SSL etkinleştirin
description: Nginx'i bir araç kapsayıcısında çalıştırarak Azure Kapsayıcı Örnekleri'nde çalışan bir kapsayıcı grubu için SSL veya TLS bitiş noktası oluşturma
ms.topic: article
ms.date: 02/14/2020
ms.openlocfilehash: 43b39c7c13d6d5e52aae2ce1706e4880ab27d225
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294952"
---
# <a name="enable-an-ssl-endpoint-in-a-sidecar-container"></a>Kenar araç konteynerinde SSL bitiş noktası nı etkinleştirme

Bu makalede, bir uygulama kapsayıcısı ve bir SSL sağlayıcısı çalıştıran bir kenar araç kapsayıcısı olan bir [kapsayıcı grubunun](container-instances-container-groups.md) nasıl oluşturulacagın bir şekilde oluşturulması gösterilmektedir. Ayrı bir SSL bitiş noktası olan bir kapsayıcı grubu ayarlayarak, uygulama kodunuzu değiştirmeden uygulamanız için SSL bağlantılarını etkinleştirebilirsiniz.

İki kapsayıcıdan oluşan örnek bir kapsayıcı grubu ayarlarsınız:
* Genel Microsoft [aci-helloworld](https://hub.docker.com/_/microsoft-azuredocs-aci-helloworld) görüntü kullanarak basit bir web uygulaması çalıştıran bir uygulama kapsayıcısı. 
* Kamu [Nginx](https://hub.docker.com/_/nginx) görüntü çalışan bir sidecar konteyner, SSL kullanmak için yapılandırılmıştır. 

Bu örnekte, kapsayıcı grubu yalnızca Genel IP adresi ile Nginx için port 443 ortaya çıkarır. Nginx, HTTPS isteklerini bağlantı noktası 80'de dahili olarak dinleyen eşlik eden web uygulamasına yönlendirir. Örneği, diğer bağlantı noktalarında dinleyen kapsayıcı uygulamalar için uyarlayabilirsiniz. 

Bkz. Bir kapsayıcı grubunda SSL'yi etkinleştirmek için diğer yaklaşımlar için [sonraki adımlar.](#next-steps)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu makaleyi tamamlamak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.0.55 veya daha sonra önerilir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Nginx'i Bir SSL sağlayıcısı olarak ayarlamak için bir SSL sertifikasına ihtiyacınız vardır. Bu makalede, kendi imzalı bir SSL sertifikasının nasıl oluşturulup oluşturulup oluşturulutamamlandığı gösterilmektedir. Üretim senaryoları için, bir sertifika yetkilisinden sertifika almanız gerekir.

Kendi imzalanmış bir SSL sertifikası oluşturmak için Azure Cloud Shell'de ve birçok Linux dağıtımında bulunan [OpenSSL](https://www.openssl.org/) aracını kullanın veya işletim sisteminizde karşılaştırılabilir bir istemci aracı kullanın.

Önce yerel çalışma dizininde bir sertifika isteği (.csr dosyası) oluşturun:

```console
openssl req -new -newkey rsa:2048 -nodes -keyout ssl.key -out ssl.csr
```

Kimlik bilgilerini eklemek için istemleri izleyin. Ortak Ad için, sertifikayla ilişkili ana bilgisayar adını girin. Parola istendiğinde, parola eklemeyi atlamak için yazmadan Enter tuşuna basın.

Sertifika isteğinden kendi imzalı sertifikayı (.crt dosyası) oluşturmak için aşağıdaki komutu çalıştırın. Örnek:

```console
openssl x509 -req -days 365 -in ssl.csr -signkey ssl.key -out ssl.crt
```

Şimdi dizinde üç dosya görmeniz gerekir: sertifika`ssl.csr`isteği (`ssl.key`), özel anahtar (`ssl.crt`), ve kendi imzalı sertifika ( ). Daha `ssl.key` sonraki `ssl.crt` adımları kullanırsınız.

## <a name="configure-nginx-to-use-ssl"></a>Nginx'i SSL kullanacak şekilde yapılandırın

### <a name="create-nginx-configuration-file"></a>Nginx yapılandırma dosyası oluşturma

Bu bölümde, Nginx'in SSL kullanması için bir yapılandırma dosyası oluşturursunuz. Aşağıdaki metni yeni bir dosyaya `nginx.conf`kopyalayarak başlayın. Azure Bulut BulutU'da, çalışma dizininizde dosyayı oluşturmak için Visual Studio Code'u kullanabilirsiniz:

```console
code nginx.conf
```

In `location`, uygulamanız `proxy_pass` için doğru bağlantı noktası ile ayarlamak için emin olun. Bu örnekte, kapsayıcı için bağlantı `aci-helloworld` noktası 80'i ayarladık.

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
        ssl_protocols              TLSv1.2;

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

### <a name="base64-encode-secrets-and-configuration-file"></a>Base64-kodlama sırları ve yapılandırma dosyası

Base64-Nginx yapılandırma dosyasını, SSL sertifikasını ve SSL anahtarını kodlayın. Sonraki bölümde, kodlanmış içeriği kapsayıcı grubunu dağıtmak için kullanılan bir YAML dosyasına girersiniz.

```console
cat nginx.conf | base64 > base64-nginx.conf
cat ssl.crt | base64 > base64-ssl.crt
cat ssl.key | base64 > base64-ssl.key
```

## <a name="deploy-container-group"></a>Konteyner grubunu dağıtma

Şimdi bir [YAML dosyasında](container-instances-multi-container-yaml.md)kapsayıcı yapılandırmaları belirterek kapsayıcı grubu dağıtmak.

### <a name="create-yaml-file"></a>YAML dosyaoluşturma

Aşağıdaki YAML'yi yeni bir `deploy-aci.yaml`dosyaya kopyala . Azure Bulut BulutU'da, çalışma dizininizde dosyayı oluşturmak için Visual Studio Code'u kullanabilirsiniz:

```console
code deploy-aci.yaml
```

Altında `secret`belirtilen temel64 kodlanmış dosyaların içeriğini girin. Örneğin, `cat` içeriğini görmek için base64 kodlanmış dosyaların her biri. Dağıtım sırasında, bu dosyalar kapsayıcı grubunda gizli bir [birim](container-instances-volume-secret.md) eklenir. Bu örnekte, gizli birim Nginx kapsayıcısına monte edilmiştir.

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

az grubu oluşturma komutu ile bir kaynak grubu [oluşturun:](/cli/azure/group#az-group-create)

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

[Az kapsayıcı oluşturma](/cli/azure/container#az-container-create) komutu ile kapsayıcı grubunu dağıtın ve YAML dosyasını bağımsız değişken olarak geçirin.

```azurecli
az container create --resource-group <myResourceGroup> --file deploy-aci.yaml
```

### <a name="view-deployment-state"></a>Dağıtım durumunu görüntüleme

Dağıtım durumunu görüntülemek için aşağıdaki [az kapsayıcı göster](/cli/azure/container#az-container-show) komutunu kullanın:

```azurecli
az container show --resource-group <myResourceGroup> --name app-with-ssl --output table
```

Başarılı bir dağıtım için çıktı aşağıdakilere benzer:

```console
Name          ResourceGroup    Status    Image                                                    IP:ports             Network    CPU/Memory       OsType    Location
------------  ---------------  --------  -------------------------------------------------------  -------------------  ---------  ---------------  --------  ----------
app-with-ssl  myresourcegroup  Running   nginx, mcr.microsoft.com/azuredocs/aci-helloworld        52.157.22.76:443     Public     1.0 core/1.5 gb  Linux     westus
```

## <a name="verify-ssl-connection"></a>SSL bağlantısını doğrula

Kapsayıcı grubunun genel IP adresine gitmek için tarayıcınızı kullanın. Bu örnekte gösterilen IP `52.157.22.76`adresi , **https://52.157.22.76**yani URL . Nginx sunucu yapılandırması nedeniyle çalışan uygulamayı görmek için HTTPS'yi kullanmanız gerekir. HTTP üzerinden bağlanma girişimleri başarısız oldu.

![Bir Azure kapsayıcı örneğinde çalışan uygulamayı gösteren tarayıcı ekran görüntüsü](./media/container-instances-container-group-ssl/aci-app-ssl-browser.png)

> [!NOTE]
> Bu örnek, sertifika yetkilisinden değil, kendi imzalı bir sertifika kullandığından, tarayıcı siteye HTTPS üzerinden bağlanırken bir güvenlik uyarısı görüntüler. Sayfaya devam etmek için uyarıyı kabul etmeniz veya tarayıcı veya sertifika ayarlarını ayarlamanız gerekebilir. Bu beklenen bir davranıştır.

>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, konteyner grubunda çalışan bir web uygulamasına SSL bağlantılarını etkinleştirmek için bir Nginx kapsayıcısının nasıl ayarlanabileceğiniz gösterin. Bu örneği, bağlantı noktası 80 dışındaki bağlantı noktalarında dinleyen uygulamalar alabilirsiniz. Ayrıca, Https'yi kullanmak için 80 (HTTP) bağlantı noktasındaki sunucu bağlantılarını otomatik olarak yeniden yönlendirmek için Nginx yapılandırma dosyasını güncelleştirebilirsiniz.

Bu makalede sidecar Nginx kullanırken, [Caddy](https://caddyserver.com/)gibi başka bir SSL sağlayıcısı kullanabilirsiniz.

Kapsayıcı grubunuzu bir [Azure sanal ağında](container-instances-vnet.md)dağıtıyorsanız, arka uç kapsayıcı örneği için bir SSL bitiş noktasını etkinleştirmek için şunları dahil olmak üzere diğer seçenekleri düşünebilirsiniz:

* [Azure İşlevler Proxies](../azure-functions/functions-proxies.md)
* [Azure API Management](../api-management/api-management-key-concepts.md)
* [Azure Uygulama Ağ Geçidi](../application-gateway/overview.md) - örnek [dağıtım şablonuna](https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress-vnet)bakın.
