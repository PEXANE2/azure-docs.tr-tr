---
title: Öğretici - Çoklu konteyner grubunu dağıtın - YAML
description: Bu eğitimde, Azure CLI ile bir YAML dosyasını kullanarak Azure Kapsayıcı Örnekleri'nde birden çok kapsayıcıiçeren bir kapsayıcı grubunu nasıl dağıtabileceğinizi öğrenirsiniz.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: cce98ec56ee1d84c087150ba486b9482515b46f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533600"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-yaml-file"></a>Öğretici: YAML dosyalarını kullanarak çok kapsayıcılı bir grubu dağıtma

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [Kaynak Yöneticisi](container-instances-multi-container-group.md)
>

Azure Kapsayıcı Örnekleri, bir [kapsayıcı grubu](container-instances-container-groups.md)kullanarak birden çok kapsayıcının tek bir ana bilgisayara dağıtımını destekler. Bir kapsayıcı grubu, bir hizmetin ikinci bir bağlı işleme ihtiyaç duyduğu günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama kenar arabası inşa ederken yararlıdır.

Bu öğreticide, Azure CLI'yi kullanarak bir [YAML dosyası](container-instances-reference-yaml.md) dağıtarak basit bir iki kapsayıcı kenar araba yapılandırmasını çalıştırmak için adımları izlersiniz. YAML dosyası, örnek ayarlarını belirtmek için kısa bir biçim sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * YAML dosyalarını yapılandırma
> * Kapsayıcı grubunu dağıtma
> * Kapsayıcıların günlüklerini görüntüleme

> [!NOTE]
> Çoklu kapsayıcı grupları şu anda Linux kapsayıcıları ile sınırlıdır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-yaml-file"></a>YAML dosyalarını yapılandırma

Azure CLI'de az [kapsayıcı oluşturma][az-container-create] komutu içeren bir çok kapsayıcı grubu dağıtmak için, bir YAML dosyasında kapsayıcı grubu yapılandırmasını belirtmeniz gerekir. Ardından YAML dosyasını bir parametre olarak komuta geçirin.

Aşağıdaki YAML'yi **deploy-aci.yaml**adlı yeni bir dosyaya kopyalayarak başlayın. Azure Bulut BulutU'da, çalışma dizininizde dosyayı oluşturmak için Visual Studio Code'u kullanabilirsiniz:

```
code deploy-aci.yaml
```

Bu YAML dosyası, iki kapsayıcı, bir genel IP adresi ve iki açık bağlantı noktası içeren "myContainerGroup" adlı bir kapsayıcı grubunu tanımlar. Kapsayıcılar genel Microsoft görüntülerinden dağıtılır. Gruptaki ilk kapsayıcı internete bakan bir web uygulaması çalıştırAr. İkinci konteyner, kenar araba, periyodik olarak konteyner grubunun yerel ağ üzerinden ilk kapsayıcıda çalışan web uygulamasına HTTP istekleriyapar.

```YAML
apiVersion: 2018-10-01
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
      - port: 8080
  - name: aci-tutorial-sidecar
    properties:
      image: mcr.microsoft.com/azuredocs/aci-tutorial-sidecar
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
    - protocol: tcp
      port: '8080'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Özel bir kapsayıcı resim kayıt defteri `imageRegistryCredentials` kullanmak için, ortamınız için değiştirilen değerlerle özelliği kapsayıcı grubuna ekleyin:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

## <a name="deploy-the-container-group"></a>Kapsayıcı grubunu dağıtma

az grubu oluşturma komutu ile bir kaynak grubu [oluşturun:][az-group-create]

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[Az kapsayıcı oluşturma][az-container-create] komutu ile kapsayıcı grubunu dağıtın ve YAML dosyasını bağımsız değişken olarak geçirin:

```azurecli-interactive
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

Birkaç saniye içinde Azure’dan bir ilk yanıt almanız gerekir.

## <a name="view-deployment-state"></a>Dağıtım durumunu görüntüleme

Dağıtım durumunu görüntülemek için aşağıdaki [az kapsayıcı göster][az-container-show] komutunu kullanın:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Çalışan uygulamayı görüntülemek istiyorsanız, tarayıcınızdaki IP adresine gidin. Örneğin, IP bu `52.168.26.124` örnek çıktı:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-container-logs"></a>Kapsayıcı günlüklerini görüntüleme

[Az kapsayıcı günlükleri][az-container-logs] komutunu kullanarak bir kapsayıcının günlük çıktısını görüntüleyin. Bağımsız `--container-name` değişken, günlükleri çekilecek kapsayıcıyı belirtir. Bu örnekte, `aci-tutorial-app` kapsayıcı belirtilir.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Çıktı:

```console
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Kenar araç kapsayıcısının günlüklerini görmek için, kapsayıcıyı `aci-tutorial-sidecar` belirten benzer bir komut çalıştırın.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Çıktı:

```console
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Gördüğünüz gibi, kenar araba düzenli olarak ana web uygulamasına grubun yerel ağı üzerinden bir HTTP isteği nde bulunarak çalıştığını garanti eder. Bu sidecar örneği, 'den `200 OK`başka bir HTTP yanıt kodu aldıysa bir uyarıyı tetiklemek için genişletilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Kapsayıcı Örnekleri'nde çok kapsayıcılı bir grup dağıtmak için bir YAML dosyası kullandınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Çok kapsayıcılı bir grup için BIR YAML dosyayı yapılandırma
> * Kapsayıcı grubunu dağıtma
> * Kapsayıcıların günlüklerini görüntüleme

[Kaynak Yöneticisi şablonu](container-instances-multi-container-group.md)kullanarak çok kapsayıcılı bir grup da belirtebilirsiniz. Kaynak Yöneticisi şablonu, kapsayıcı grubuyla ek Azure hizmet kaynakları dağıtmanız gerektiğinde senaryolar için kolayca uyarlanabilir.

<!-- LINKS - External -->


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
