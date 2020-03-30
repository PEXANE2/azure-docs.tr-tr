---
title: 'Öğretici: Özel bir resim oluşturun ve çalıştırın'
description: Azure Uygulama Hizmeti'nde çalıştırılabilen, Azure Konteyner Kayıt Şirketlerine dağıtabilen ve Uygulama Hizmeti'nde çalıştırabilen özel bir Linux görüntüsünü nasıl oluşturabileceğinizi öğrenin.
keywords: azure app service, web uygulaması, linux, docker, kapsayıcı
author: msangapu-msft
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: a6c9eb354bce09a5f652895f4af34df1f6750bec
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80045754"
---
# <a name="tutorial-build-a-custom-image-and-run-in-app-service-from-a-private-registry"></a>Öğretici: Özel bir resim oluşturun ve özel bir kayıt defterinden Uygulama Hizmeti'nde çalıştırın

[App Service,](app-service-linux-intro.md) PHP 7.3 ve Node.js 10.14 gibi belirli sürümler için linux'ta yerleşik Docker görüntüleri sağlar. App Service, hem yerleşik görüntüleri hem de özel görüntüleri bir hizmet olarak platform olarak barındırmak için Docker konteyner teknolojisini kullanır. Bu öğreticide, özel bir resim oluşturmayı ve Uygulama Hizmeti'nde çalıştırmayı öğrenirsiniz. Bu desen, yerleşik görüntülerin sizin tercih ettiğiniz dili içermediği veya uygulamanızın yerleşik görüntülerde sağlanmayan belirli bir yapılandırma gerektirdiği durumlarda yararlı olur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özel bir kapsayıcı kayıt defterine özel bir görüntü dağıtma
> * Özel görüntüyü Uygulama Hizmeti'nde çalıştırma
> * Ortam değişkenlerini yapılandırma
> * Görüntüyü güncelleştirin ve yeniden dağıtın
> * Tanılama günlüklerine erişim
> * SSH kullanarak kapsayıcıya bağlanma

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Git](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/get-started/#setup)

## <a name="download-the-sample"></a>Örneği indirme

Terminal penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak ve örnek kodu içeren dizine gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Docker dosyasından görüntüyü oluşturma

Git deposunda _Dockerfile_ dosyasına göz atın. Bu dosya, uygulamanızı çalıştırmak için gereken Python ortamını açıklar. Görüntü ayrıca, kapsayıcı ile konak arasında güvenli iletişim için bir [SSH](https://www.ssh.com/ssh/protocol/) sunucusu ayarlar. _Dockerfile'daki_son satır, `ENTRYPOINT ["init.sh"]`SSH hizmetini ve Python sunucusunu başlatmak için çağrıda bulununur. `init.sh`

```Dockerfile
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222

#service SSH start
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

`docker build` Docker görüntüsünü komutla oluşturun.

```bash
docker build --tag mydockerimage .
```

Docker kapsayıcısını çalıştırarak derlemenin çalışmasını test edin. Komutu [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) ver ve görüntünün adını ve etiketini ona geçirin. `-p` bağımsız değişkenini kullanarak bağlantı noktasını belirttiğinizden emin olun.

```bash
docker run -p 8000:8000 mydockerimage
```

`http://localhost:8000` konumuna göz atarak web uygulamasının ve kapsayıcının düzgün çalıştığını doğrulayın.

![Web uygulamasını yerel olarak test etme](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Oluşturduğunuz resmi kullanan bir uygulama oluşturmak için, kaynak grubu oluşturan, görüntüyü iten ve ardından çalıştırmak için App Service planı web uygulamasını oluşturan Azure CLI komutlarını çalıştırırsınız.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Bulut Kabuğu'nda, [`az acr create`](/cli/azure/acr?view=azure-cli-latest#az-acr-create) bir Azure Kapsayıcı Kayıt Defteri oluşturmak için komutu kullanın.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

### <a name="sign-in-to-azure-container-registry"></a>Azure Konteyner Kayıt Defterinde Oturum Aç

Bir resmi kayıt defterine itmek için, özel kayıt defteriyle birlikte kimlik doğrulamanız gerekir. Bulut Kabuğu'nda, [`az acr show`](/cli/azure/acr?view=azure-cli-latest#az-acr-show) oluşturduğunuz kayıt defterinden kimlik bilgilerini almak için komutu kullanın.

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

Çıktı, kullanıcı adı ile birlikte iki parola yı gösterir.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<registry-username>"
}
```

Yerel terminal pencerenizden, aşağıdaki örnekte gösterildiği gibi `docker login` komutu kullanarak Azure Konteyner Kayıt Defteri'nde oturum açın. * \<Azure-container-registry-name>* ve * \<kayıt defteri kullanıcı adı>* kayıt defterinizin değerleriyle değiştirin. İstendiğinde, önceki adımdaki parolalardan birini yazın.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username>
```

Girişin başarılı olduğunu doğrulayın.

### <a name="push-image-to-azure-container-registry"></a>Azure Container Registry’ye görüntü gönderme

Azure Kapsayıcı Kayıt Defteri için yerel resminizi etiketleyin. Örnek:
```bash
docker tag mydockerimage <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

`docker push` komutunu kullanarak görüntüyü gönderin. Görüntüyü kayıt defterinin adıyla etiketleyin ve arkasına görüntü adınızı ve etiketinizi ekleyin.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Bulut Kabuğu'nda, itme başarılı olduğundan doğrulayın.

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Aşağıdaki çıktıyı almalısınız.

```json
[
  "mydockerimage"
]
```

### <a name="create-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-web-app"></a>Web uygulaması oluşturma

Bulut Kabuğu'nda, [web app](app-service-linux-intro.md) `myAppServicePlan` [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) Uygulama Hizmeti planında komutla birlikte bir web uygulaması oluşturun. _ \<Uygulama adı>_ benzersiz bir uygulama adı yla, _ \<azure-konteyner-kayıt defteri adı>'yi_ de kayıt defteri adınız ile değiştirin.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --deployment-container-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-registry-credentials-in-web-app"></a>Web uygulamasında kayıt defteri kimlik bilgilerini yapılandırma

App Service'in özel görüntüyü çekebilmesi için kayıt defteriniz ve resminiz hakkında bilgi gerekir. Bulut Kabuğu'nda, komutu [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) sağlayın. * \<Uygulama adı>, * * \<azure-konteyner-kayıt defteri-adı>, * _ \<kayıt defteri-kullanıcı adı>_ ve _ \<parola>_ değiştirin.

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group myResourceGroup --docker-custom-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0 --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> Docker Hub dışındaki bir kayıt `--docker-registry-server-url` defteri kullanırken, `https://` kayıt defterinin tam nitelikli alan adı tarafından izlenecek şekilde biçimlendirilmelidir.
>

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Docker görüntülerinin çoğu, 80'den farklı bir bağlantı noktası gibi özel ortam değişkenleri kullanır. Uygulama Hizmeti'ne uygulama ayarını kullanarak resminizin kullandığı bağlantı noktası hakkında `WEBSITES_PORT` bilgi verirsiniz. [Bu öğreticideki Python örneği](https://github.com/Azure-Samples/docker-django-webapp-linux) için GitHub sayfası, `WEBSITES_PORT` olarak _8000_ ayarlamanız gerektiğini gösterir.

Uygulama ayarlarını ayarlamak [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) için Bulut Kabuğu'ndaki komutu kullanın. Uygulama ayarları büyük/küçük harfe duyarlıdır ve boşlukla ayrılmıştır.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app-name> --settings WEBSITES_PORT=8000
```

### <a name="test-the-web-app"></a>Web uygulamasını test etme

Web uygulamasına göz atarak uygulamanın çalıştığını doğrulayın (`http://<app-name>.azurewebsites.net`).

> [!NOTE]
> Uygulamaya ilk kez erişinizde, Uygulama Hizmeti'nin tüm görüntüyü çekmesi gerektiğinden, bu uygulama biraz zaman alabilir. Tarayıcı zaman ları doluyorsa, sayfayı yenilemeniz.

![Web uygulaması bağlantı noktası yapılandırmasını test etme](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Web uygulamasını değiştirme ve yeniden dağıtma

Git deponuzda app/templates/app/index.html dosyasını açın. İlk HTML öğesini bulun ve şöyle değiştirin.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>
      </div>
    </div>
  </nav>
```

Python dosyasını değiştirdikten ve kaydettikten sonra, yeni Docker görüntüsünü yeniden oluşturup göndermeniz gerekir. Ardından değişikliklerin geçerlilik kazanması için web uygulamasını yeniden başlatın. Bu öğreticide daha önce kullandığınız komutları kullanın. [Docker dosyasından görüntü oluştur'a](#build-the-image-from-the-docker-file) başvurabilir ve [görüntüyü Azure Kapsayıcı Kayıt Defteri'ne itebilirsiniz.](#push-image-to-azure-container-registry) [Web uygulamasını test etme](#test-the-web-app) başlığı altındaki yönergeleri izleyerek web uygulamasını test edin.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="enable-ssh-connections"></a>SSH bağlantılarını etkinleştirme

SSH, kapsayıcı ile istemci arasında güvenli iletişime olanak tanır. Kapsayıcınıza SSH bağlantısını etkinleştirmek için özel görüntünüzbunun için yapılandırılmalıdır. Zaten gerekli yapılandırmaya sahip örnek deposuna bir göz atalım.

* [Dockerfile'da,](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile)aşağıdaki kod SSH sunucusunu yükler ve oturum açma kimlik bilgilerini de ayarlar.

    ```Dockerfile
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Bu yapılandırma kapsayıcıya dış bağlantılar kurulmasına izin vermez. SSH yalnızca Kudu/SCM Sitesi aracılığıyla kullanılabilir. Kudu/SCM sitesi Azure hesabınızla birlikte kimlik doğrulanır.

* [Dockerfile,](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L18) [sshd_config](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/sshd_config) dosyasını */etc/ssh/directory* adresindeki depoda kopyalar.

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

* [Dockerfile,](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L22) konteynerdeki 2222 bağlantı noktasını ortaya çıkarır. Bu, yalnızca özel sanal ağın köprü ağı içinde yer alan kapsayıcıların erişebildiği dahili bir bağlantı noktasıdır. 

    ```Dockerfile
    EXPOSE 8000 2222
    ```

* [Giriş komut dosyası](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/init.sh#L5) SSH sunucusunu başlatır.

    ```bash
    #!/bin/bash
    service ssh start
    ```

### <a name="open-ssh-connection-to-container"></a>Kapsayıcıya SSH bağlantısı açma

SSH bağlantısı na sadece Kudu sitesinden ulaşılabilir. `https://<app-name>.scm.azurewebsites.net`

Bağlanmak için, `https://<app-name>.scm.azurewebsites.net/webssh/host` adresine göz atın ve Azure hesabınızla oturum açın.

Ardından etkileşimli konsolu görüntüleyen bir sayfaya yeniden yönlendirilirsiniz.

Bazı uygulamaların kapsayıcı içinde çalıştırıldığını doğrulamak isteyebilirsiniz. Kapsayıcıyı incelemek ve çalıştırma işlemlerini doğrulamak için, komut isteminde `top` komutunu verin.

```bash
top
```

`top` komutu kapsayıcıda çalıştırılan tüm işlemleri ortaya çıkarır.

```
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Tebrikler! App Service'te özel bir Linux kapsayıcısı yapılandırıldınız.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Özel bir kapsayıcı kayıt defterine özel bir görüntü dağıtma
> * Özel görüntüyü Uygulama Hizmeti'nde çalıştırma
> * Ortam değişkenlerini yapılandırma
> * Görüntüyü güncelleştirin ve yeniden dağıtın
> * Tanılama günlüklerine erişim
> * SSH kullanarak kapsayıcıya bağlanma

Özel bir DNS adının uygulamanızla nasıl eşleştinolduğunu öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: Uygulamanıza özel DNS adını haritalandırın](../app-service-web-tutorial-custom-domain.md)

Veya diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Özel kapsayıcı yapılandırma](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Öğretici: Multi-konteyner WordPress uygulaması](tutorial-multi-container-app.md)
