---
title: Özel bir Linux kapsayıcısı yapılandırma
description: Azure Uygulama Hizmeti'nde özel bir Linux kapsayıcısını nasıl yapılandırıştırmayı öğrenin. Bu makalede, en yaygın yapılandırma görevleri gösterir.
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: 6baa1fbd4932aa83a54081ff166dcae7f258fff9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280150"
---
# <a name="configure-a-custom-linux-container-for-azure-app-service"></a>Azure Uygulama Hizmeti için özel bir Linux kapsayıcısı yapılandırma

Bu makalede, Azure Uygulama Hizmeti'nde çalışacak özel bir Linux kapsayıcısını nasıl yapılandırabileceğiniz gösterilmektedir.

Bu kılavuz, Uygulama Hizmeti'nde Linux uygulamalarının kapsayıcıhale getirilmesi için temel kavramlar ve yönergeler sağlar. Azure Uygulama Hizmeti'ni hiç kullanmadıysanız, önce [özel kapsayıcıhızlı başlat'ı](quickstart-docker-go.md) ve [öğreticiyi](tutorial-custom-docker-image.md) izleyin. Ayrıca bir [çok konteyner uygulaması quickstart](quickstart-multi-container.md) ve [öğretici](tutorial-multi-container-app.md)var.

## <a name="configure-port-number"></a>Bağlantı noktası numarasını yapılandırma

Özel resminizdeki web sunucusu 80'den başka bir bağlantı noktası kullanabilir. Azure'a özel kapsayıcınızın kullandığı bağlantı noktasını `WEBSITES_PORT` uygulama ayarını kullanarak anlatırsınız. [Bu öğreticideki Python örneği](https://github.com/Azure-Samples/docker-django-webapp-linux) için GitHub sayfası, `WEBSITES_PORT` olarak _8000_ ayarlamanız gerektiğini gösterir. Bulut Kabuğu'nda [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komut çalıştırarak ayarlayabilirsiniz. Örnek:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_PORT=8000
```

## <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Özel kapsayıcınız dışarıdan sağlanması gereken ortam değişkenlerini kullanabilir. Bulut Kabuğu'nda komut [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) çalıştırarak bunları geçirebilirsiniz. Örnek:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WORDPRESS_DB_HOST="myownserver.mysql.database.azure.com"
```

Bu yöntem, ortam değişkenlerinin *docker-compose.yml* dosyasında belirtildiği tek kapsayıcı uygulamalar veya çoklu kapsayıcı uygulamaları için çalışır.

## <a name="use-persistent-shared-storage"></a>Kalıcı paylaşılan depolama alanını kullanma

Yeniden başlatmalarda dosyaları sürdürmek ve örnekler arasında paylaşmak için uygulamanızın dosya sistemindeki */home* dizinini kullanabilirsiniz. Uygulamanızdaki uygulama, `/home` konteyner uygulamanızın kalıcı depolama alanına erişmesini sağlamak için sağlanır.

Kalıcı depolama devre dışı bırakıldığında, `/home` dizin için yazma uygulaması yeniden başlatmaveya birden çok örnek arasında kalıcı değildir. Bunun tek istisnası, Docker ve konteyner günlüklerini depolamak için kullanılan `/home/LogFiles` dizindir. Kalıcı depolama etkinleştirildiğinde, `/home` dizine yapılan tüm yazılar kalıcıdır ve ölçeklenmiş bir uygulamanın tüm örnekleri tarafından erişilebilir.

Varsayılan olarak, kalıcı depolama *etkinleştirilir* ve ayar Uygulama Ayarları'nda açıklanmaz. Devre dışı kalmak için `WEBSITES_ENABLE_APP_SERVICE_STORAGE` Bulut Kabuğu'nda komut çalıştırarak [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) uygulama ayarını ayarlayın. Örnek:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=false
```

> [!NOTE]
> [Ayrıca kendi kalıcı depolama nızı da yapılandırabilirsiniz.](how-to-serve-content-from-azure-storage.md)

## <a name="enable-ssh"></a>SSH'yi etkinleştir

SSH, kapsayıcı ile istemci arasında güvenli iletişime olanak tanır. Özel bir kapsayıcının SSH'yi desteklemesi için, bunu Dockerfile'nin kendisine eklemeniz gerekir.

> [!TIP]
> Tüm yerleşik Linux kapları, görüntü depolarına SSH yönergelerini ekledi. Orada nasıl etkinleştirildiğinizi görmek için [Node.js 10.14 deposu](https://github.com/Azure-App-Service/node/blob/master/10.14) ile aşağıdaki yönergeleri gözden geçirebilirsiniz.

- SSH [RUN](https://docs.docker.com/engine/reference/builder/#run) sunucusunu yüklemek ve kök hesabın parolasını ' `"Docker!"`olarak ayarlamak için RUN yönergesini kullanın. Örneğin, [Alpine Linux](https://hub.docker.com/_/alpine)tabanlı bir görüntü için aşağıdaki komutlara ihtiyacınız vardır:

    ```Dockerfile
    RUN apk add openssh \
         && echo "root:Docker!" | chpasswd 
    ```

    Bu yapılandırma kapsayıcıya harici bağlantılara izin vermez. SSH yalnızca yayımlama kimlik bilgileri aracılığıyla `https://<app-name>.scm.azurewebsites.net` kullanılabilir ve doğrulanır.

- [Bu sshd_config dosyayı](https://github.com/Azure-App-Service/node/blob/master/10.14/sshd_config) resim deponuza ekleyin ve */etc/ssh/directory'ye* kopyalamak için [COPY](https://docs.docker.com/engine/reference/builder/#copy) yönergesini kullanın. *sshd_config* dosyaları hakkında daha fazla bilgi için [OpenBSD belgelerine](https://man.openbsd.org/sshd_config)bakın.

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > *Sshd_config* dosyası şu öğeleri içermelidir:
    > - `Ciphers`, bu listedeki en az bir öğeyi içermelidir: `aes128-cbc,3des-cbc,aes256-cbc`.
    > - `MACs`, bu listedeki en az bir öğeyi içermelidir: `hmac-sha1,hmac-sha1-96`.

- Kapsayıcıda 2222 bağlantı noktasını açmak için [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose) yönergesini kullanın. Kök parola bilinmesine rağmen, bağlantı noktası 2222'ye internetten erişilemez. Yalnızca özel bir sanal ağın köprü ağındaki kapsayıcılar tarafından erişilebilir.

    ```Dockerfile
    EXPOSE 80 2222
    ```

- Kapsayıcınızın başlangıç komut dosyasında SSH sunucusunu başlatın.

    ```bash
    /usr/sbin/sshd
    ```

    Örneğin, varsayılan [Düğüm 10.14 kapsayıcısının](https://github.com/Azure-App-Service/node/blob/master/10.14/startup/init_container.sh) SSH sunucusunu nasıl başlattığını görün.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="configure-multi-container-apps"></a>Çok kapsayıcı uygulamaları yapılandırma

- [Docker Compose'da kalıcı depolama yı kullanma](#use-persistent-storage-in-docker-compose)
- [Önizleme sınırlamaları](#preview-limitations)
- [Docker Oluşturma seçenekleri](#docker-compose-options)

### <a name="use-persistent-storage-in-docker-compose"></a>Docker Compose'da kalıcı depolama yı kullanma

WordPress gibi çoklu kapsayıcı uygulamaların düzgün çalışması için kalıcı depolama alanına ihtiyaç duyar. Bunu etkinleştirmek için Docker Compose yapılandırmanızın kapsayıcınızın *dışındaki* bir depolama konumunu işaret etmesi gerekir. Kapsayıcınızın içindeki depolama konumları, uygulama yeniden başlatmanın ötesinde değişiklikleri devam etmez.

Cloud Shell'de `WEBSITES_ENABLE_APP_SERVICE_STORAGE` [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak uygulama ayarını ayarlayarak kalıcı depolamayı etkinleştirin.

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

*Docker-compose.yml* dosyanızda, seçeneğini `volumes` `${WEBAPP_STORAGE_HOME}`belirleyin. 

`WEBAPP_STORAGE_HOME`, App Service içinde bulunan ve uygulamanız için kalıcı depolamayla eşlenmiş olan bir ortam değişkenidir. Örnek:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
  - ${WEBAPP_STORAGE_HOME}/phpmyadmin:/var/www/phpmyadmin
  - ${WEBAPP_STORAGE_HOME}/LogFiles:/var/log
```

### <a name="preview-limitations"></a>Önizleme sınırlamaları

Çoklu kapsayıcı şu anda önizlemede. Aşağıdaki Uygulama Hizmeti platformu özellikleri desteklenmez:

- Kimlik Doğrulama / Yetkilendirme
- Yönetilen Kimlikler

### <a name="docker-compose-options"></a>Docker Oluşturma seçenekleri

Aşağıdaki listeler desteklenen ve desteklenmeyen Docker Compose yapılandırma seçeneklerini gösterir:

#### <a name="supported-options"></a>Desteklenen seçenekler

- command
- entrypoint
- environment
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
- 80 ve 8080 dışındaki bağlantı noktaları (göz ardı edilen)

> [!NOTE]
> Açıkça çağrılmayan diğer seçenekler Genel Önizleme'de yoksayılır.

## <a name="configure-vnet-integration"></a>VNet tümleştirmeyi yapılandırma

VNet tümleştirmesi ile özel bir kapsayıcı kullanmak ek kapsayıcı yapılandırması gerektirebilir. Bkz. [Uygulamanızı Azure Sanal Ağıyla Tümleştirin.](../web-sites-integrate-with-vnet.md)

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Özel konteyner deposundan dağıtma](tutorial-custom-docker-image.md)

> [!div class="nextstepaction"]
> [Öğretici: Multi-konteyner WordPress uygulaması](tutorial-multi-container-app.md)
