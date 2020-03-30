---
title: Öğretici - Çoklu kapsayıcı grubunu dağıt - şablon
description: Bu eğitimde, Azure CLI ile bir Azure Kaynak Yöneticisi şablonu kullanarak Azure Kapsayıcı Örnekleri'nde birden çok kapsayıcıiçeren bir kapsayıcı grubunu nasıl dağıtabileceğinizi öğrenirsiniz.
ms.topic: article
ms.date: 04/03/2019
ms.custom: mvc
ms.openlocfilehash: d2b4e20520cad28c5d62118f6c9d10fcc43ac89e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533635"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-resource-manager-template"></a>Öğretici: Kaynak Yöneticisi şablonu kullanarak çok kapsayıcılı bir grup dağıtma

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [Kaynak Yöneticisi](container-instances-multi-container-group.md)

Azure Kapsayıcı Örnekleri, bir [kapsayıcı grubu](container-instances-container-groups.md)kullanarak birden çok kapsayıcının tek bir ana bilgisayara dağıtımını destekler. Bir kapsayıcı grubu, bir hizmetin ikinci bir bağlı işleme ihtiyaç duyduğu günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama kenar arabası inşa ederken yararlıdır.

Bu öğreticide, Azure CLI'yi kullanarak bir Azure Kaynak Yöneticisi şablonu dağıtarak basit bir iki kapsayıcı kenar araba yapılandırması çalıştırma adımlarını izlersiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Çok kapsayıcılı grup şablonu yapılandırma
> * Kapsayıcı grubunu dağıtma
> * Kapsayıcıların günlüklerini görüntüleme

Kaynak Yöneticisi şablonu, kapsayıcı grubuyla ek Azure hizmet kaynakları (örneğin, Azure Dosyaları paylaşımı veya sanal ağ) dağıtmanız gerektiğinde senaryolar için kolayca uyarlanabilir. 

> [!NOTE]
> Çoklu kapsayıcı grupları şu anda Linux kapsayıcıları ile sınırlıdır. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-template"></a>Şablonu yapılandırma

Aşağıdaki JSON'u yeni bir dosyaya `azuredeploy.json`kopyalayarak başlayın. Azure Bulut BulutU'da, çalışma dizininizde dosyayı oluşturmak için Visual Studio Code'u kullanabilirsiniz:

```
code azuredeploy.json
```

Bu Kaynak Yöneticisi şablonu, iki kapsayıcı, genel BIR IP adresi ve iki açık bağlantı noktası içeren bir kapsayıcı grubu tanımlar. Gruptaki ilk kapsayıcı internete bakan bir web uygulaması çalıştırAr. İkinci konteyner, sidecar, grubun yerel ağ üzerinden ana web uygulaması için bir HTTP isteği yapar.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

Özel bir kapsayıcı resim kayıt defteri kullanmak için, JSON belgesine aşağıdaki biçimde bir nesne ekleyin. Bu yapılandırmanın örnek bir uygulaması [için, ACI Kaynak Yöneticisi şablon başvuru][template-reference] belgelerine bakın.

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[Az grubu dağıtım oluşturma][az-group-deployment-create] komutu ile şablonu dağıtın.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
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

```bash
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

```bash
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

Bu öğreticide, Azure Kapsayıcı Örnekleri'nde çok kapsayıcılı bir grup dağıtmak için bir Azure Kaynak Yöneticisi şablonu kullandınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Çok kapsayıcılı grup şablonu yapılandırma
> * Kapsayıcı grubunu dağıtma
> * Kapsayıcıların günlüklerini görüntüleme

Ek şablon örnekleri için Azure [Kapsayıcı Örnekleri için Azure Kaynak Yöneticisi şablonlarına](container-instances-samples-rm.md)bakın.

Ayrıca, [yaml dosyakullanarak](container-instances-multi-container-yaml.md)bir çok kapsayıcı grubu belirtebilirsiniz. YAML biçiminin daha özlü yapısı nedeniyle, dağıtımınız yalnızca kapsayıcı örnekleri içeriyorsa, YAML dosyasıyla dağıtım iyi bir seçimdir.


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
