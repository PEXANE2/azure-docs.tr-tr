---
title: Özel bir Linux kapsayıcısı yapılandırma
description: Azure App Service bir özel Linux kapsayıcısını yapılandırmayı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: 9a27abe5457cf8adf2963db545c629134ae53709
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89566985"
---
# <a name="configure-a-custom-linux-container-for-azure-app-service"></a>Azure App Service için özel bir Linux kapsayıcısı yapılandırma

Bu makalede, Azure App Service üzerinde çalışacak özel bir Linux kapsayıcısının nasıl yapılandırılacağı gösterilir.

Bu kılavuzda, App Service Linux uygulamalarının kapsayıcılama için temel kavramlar ve yönergeler sağlanmaktadır. Azure App Service hiç kullanmadıysanız, önce [özel kapsayıcı hızlı başlangıç](quickstart-custom-container.md?pivots=container-linux) ve [öğreticiyi](tutorial-custom-container.md?pivots=container-linux) izleyin. Ayrıca [çok kapsayıcılı bir uygulama hızlı](quickstart-multi-container.md) başlangıcı ve [öğretici](tutorial-multi-container-app.md)de mevcuttur.

## <a name="configure-port-number"></a>Bağlantı noktası numarasını Yapılandır

Varsayılan olarak App Service, özel kapsayıcının 80 numaralı bağlantı noktasında dinlediğini varsayar. Özel görüntinizdeki Web sunucusu 80 dışında bir bağlantı noktası kullanabilir. Azure 'a özel kapsayıcının kullandığı bağlantı noktasını uygulama ayarını kullanarak söylemiş olursunuz `WEBSITES_PORT` . [Bu öğreticideki Python örneği](https://github.com/Azure-Samples/docker-django-webapp-linux) için GitHub sayfası, `WEBSITES_PORT` olarak _8000_ ayarlamanız gerektiğini gösterir. Bu ayarı, [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) Cloud Shell komutunu çalıştırarak ayarlayabilirsiniz. Örneğin:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_PORT=8000
```

## <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Özel Kapsayıcınız, dışarıdan sağlanması gereken ortam değişkenlerini kullanabilir. Cloud Shell komutu çalıştırarak içine geçirebilirsiniz [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) . Örneğin:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WORDPRESS_DB_HOST="myownserver.mysql.database.azure.com"
```

Bu yöntem, hem tek Kapsayıcılı uygulamalar için hem de ortam değişkenlerinin *Docker-Compose. yıml* dosyasında belirtildiği çok Kapsayıcılı uygulamalar için geçerlidir.

## <a name="use-persistent-shared-storage"></a>Kalıcı paylaşılan depolama kullan

Dosyaları yeniden başlatmalar arasında kalıcı hale getirmek ve örnekleri arasında paylaşmak için uygulamanızın dosya sistemindeki */Home* dizinini kullanabilirsiniz. `/home`Uygulamanızda, kapsayıcı uygulamanızın kalıcı depolamaya erişmesini sağlamak için verilmiştir.

Kalıcı depolama devre dışı bırakıldığında, dizine yazma işlemleri, `/home` uygulama yeniden başlatmaları veya birden çok örnek arasında kalıcı olmaz. Tek özel durum `/home/LogFiles` , Docker ve kapsayıcı günlüklerini depolamak için kullanılan dizindir. Kalıcı depolama etkinleştirildiğinde, dizine yapılan tüm yazmaları `/home` kalıcı hale getirilir ve genişleme uygulamasının tüm örnekleri tarafından erişilebilir.

Varsayılan olarak, kalıcı depolama *etkindir* ve ayar uygulama ayarları 'nda gösterilmez. Devre dışı bırakmak için `WEBSITES_ENABLE_APP_SERVICE_STORAGE` Cloud Shell komut çalıştırarak uygulama ayarını ayarlayın [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) . Örneğin:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=false
```

> [!NOTE]
> Ayrıca, [kendi kalıcı depolama alanınızı da yapılandırabilirsiniz](configure-connect-to-azure-storage.md?pivots=platform-linux).

## <a name="enable-ssh"></a>SSH 'yi etkinleştirme

SSH, kapsayıcı ile istemci arasında güvenli iletişime olanak tanır. Özel bir kapsayıcının SSH 'yi desteklemesi için onu Dockerfile öğesine eklemeniz gerekir.

> [!TIP]
> Tüm yerleşik Linux kapsayıcıları, görüntü depolarında SSH yönergelerini ekledi. Nasıl etkinleştirildiğini görmek için [Node.js 10,14 deposu](https://github.com/Azure-App-Service/node/blob/master/10.14) ile aşağıdaki yönergeleri izleyebilirsiniz.

- SSH sunucusunu yüklemek ve kök hesabın parolasını olarak ayarlamak için [Çalıştır](https://docs.docker.com/engine/reference/builder/#run) yönergesini kullanın `"Docker!"` . Örneğin, [alp Linux](https://hub.docker.com/_/alpine)tabanlı bir görüntü için aşağıdaki komutlara ihtiyacınız vardır:

    ```Dockerfile
    RUN apk add openssh \
         && echo "root:Docker!" | chpasswd 
    ```

    Bu yapılandırma kapsayıcıya dış bağlantılara izin vermiyor. SSH yalnızca `https://<app-name>.scm.azurewebsites.net` Yayımlama kimlik bilgileriyle kullanılabilir ve kimlik doğrulaması yapılabilir.

- [Bu sshd_config dosyasını](https://github.com/Azure-App-Service/node/blob/master/10.14/sshd_config) görüntü deponuza ekleyin ve [Copy](https://docs.docker.com/engine/reference/builder/#copy) yönergesini kullanarak dosyayı */etc/ssh/* dizinine kopyalayın. *Sshd_config* dosyaları hakkında daha fazla bilgi için bkz. [OpenBSD belgeleri](https://man.openbsd.org/sshd_config).

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > *Sshd_config* dosyası şu öğeleri içermelidir:
    > - `Ciphers`, bu listedeki en az bir öğeyi içermelidir: `aes128-cbc,3des-cbc,aes256-cbc`.
    > - `MACs`, bu listedeki en az bir öğeyi içermelidir: `hmac-sha1,hmac-sha1-96`.

- Kapsayıcıda 2222 numaralı bağlantı noktasını açmak için [sergileme](https://docs.docker.com/engine/reference/builder/#expose) yönergesini kullanın. Kök parola bilinse de, 2222 numaralı bağlantı noktasına internet 'ten erişilemez. Yalnızca özel bir sanal ağın köprü ağı içindeki kapsayıcılar tarafından erişilebilir.

    ```Dockerfile
    EXPOSE 80 2222
    ```

- Kapsayıcının başlangıç komut dosyasında SSH sunucusunu başlatın.

    ```bash
    /usr/sbin/sshd
    ```

    Örneğin, varsayılan [Node.js 10,14 KAPSAYıCıSıNıN](https://github.com/Azure-App-Service/node/blob/master/10.14/startup/init_container.sh) SSH sunucusunu nasıl başlatadığına bakın.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

## <a name="change-the-docker-image-of-a-custom-container"></a>Özel kapsayıcının Docker görüntüsünü değiştirme

Mevcut bir özel kapsayıcı uygulamasını geçerli Docker görüntüsünden yeni bir görüntüye değiştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name <docker-hub-repo>/<image>
```

## <a name="use-an-image-from-a-private-registry"></a>Özel bir kayıt defterinden görüntü kullanma

Azure Container Registry gibi özel bir kayıt defterinden görüntü kullanmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name <image-name> --docker-registry-server-url <private-repo-url> --docker-registry-server-user <username> --docker-registry-server-password <password>
```

*\<username>* Ve için *\<password>* , özel kayıt hesabınız için oturum açma kimlik bilgilerini sağlayın.

## <a name="configure-multi-container-apps"></a>Çok Kapsayıcılı uygulamaları yapılandırma

- [Docker Compose 'de kalıcı depolamayı kullanma](#use-persistent-storage-in-docker-compose)
- [Önizleme sınırlamaları](#preview-limitations)
- [Docker Compose seçenekleri](#docker-compose-options)

### <a name="use-persistent-storage-in-docker-compose"></a>Docker Compose 'de kalıcı depolamayı kullanma

WordPress gibi çok Kapsayıcılı uygulamalarda kalıcı depolamanın düzgün çalışması gerekir. Bunu etkinleştirmek için, Docker Compose yapılandırmanızın kapsayıcının *dışında* bir depolama konumunu göstermesi gerekir. Kapsayıcının içindeki depolama konumları değişiklikleri uygulamanın yeniden başlatmalarıyla kalıcı tutmaz.

`WEBSITES_ENABLE_APP_SERVICE_STORAGE`Cloud Shell ' deki [az WebApp config appSettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak uygulama ayarını ayarlayarak kalıcı depolamayı etkinleştirin.

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

*Docker-Compose. yıml* dosyanızda, `volumes` seçeneğini ile eşleyin `${WEBAPP_STORAGE_HOME}` . 

`WEBAPP_STORAGE_HOME`, App Service içinde bulunan ve uygulamanız için kalıcı depolamayla eşlenmiş olan bir ortam değişkenidir. Örneğin:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
  - ${WEBAPP_STORAGE_HOME}/phpmyadmin:/var/www/phpmyadmin
  - ${WEBAPP_STORAGE_HOME}/LogFiles:/var/log
```

### <a name="preview-limitations"></a>Önizleme sınırlamaları

Çok Kapsayıcılı Şu anda önizleme aşamasındadır. Aşağıdaki App Service platform özellikleri desteklenmez:

- Kimlik doğrulama/yetkilendirme
- Yönetilen kimlikler
- CORS

### <a name="docker-compose-options"></a>Docker Compose seçenekleri

Aşağıdaki listede desteklenen ve desteklenmeyen Docker Compose yapılandırma seçenekleri gösterilmektedir:

#### <a name="supported-options"></a>Desteklenen seçenekler

- command
- entrypoint
- ortam
- image
- ports
- restart
- services
- volumes

#### <a name="unsupported-options"></a>Desteklenmeyen seçenekler

- build (izin verilmez)
- depends_on (yoksayılır)
- networks (yoksayılır)
- secrets (yoksayılır)
- 80 ve 8080 dışındaki bağlantı noktaları (yoksayıldı)

> [!NOTE]
> Açıkça çağrılmayan diğer seçenekler genel önizlemede yok sayılır.

[!INCLUDE [robots933456](../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: özel kapsayıcı deposundan dağıtma](tutorial-custom-container.md?pivots=container-linux)

> [!div class="nextstepaction"]
> [Öğretici: çok Kapsayıcılı WordPress uygulaması](tutorial-multi-container-app.md)
