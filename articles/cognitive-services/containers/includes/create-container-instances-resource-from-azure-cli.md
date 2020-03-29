---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Azure CLI'den bir Azure kapsayıcı örnek kaynağını nasıl oluşturabilirsiniz öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/06/2020
ms.author: dapine
ms.openlocfilehash: 700a04b58c13a9c7fd5301875226ca234cabeb96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75689436"
---
## <a name="create-an-azure-container-instance-resource-from-the-azure-cli"></a>Azure CLI'den bir Azure Kapsayıcı Örneği kaynağı oluşturma

Aşağıdaki YAML, Azure Kapsayıcı Örneği kaynağını tanımlar. İçeriği yeni bir dosyaya kopyalayın `my-aci.yaml` ve yapıştırın, adı ve yorumlanan değerleri kendi dosyanızla değiştirin. Geçerli YAML için [şablon biçimine][template-format] bakın. Kullanılabilir resim adları ve bunların karşılık gelen depoları için [kapsayıcı depolarına][repositories-and-images] ve görüntülerine bakın. Kapsayıcı örnekleri için YAML başvurusu hakkında daha fazla bilgi için [YAML başvurusuna bakın: Azure Kapsayıcı Örnekleri.][aci-yaml-ref]

```YAML
apiVersion: 2018-10-01
location: # < Valid location >
name: # < Container Group name >
imageRegistryCredentials: # This is required when pulling a non-public image
  - server: containerpreview.azurecr.io
    username: # < The username for the preview container registry >
    password: # < The password for the preview container registry >
properties:
  containers:
  - name: # < Container name >
    properties:
      image: # < Repository/Image name >
      environmentVariables: # These env vars are required
        - name: eula
          value: accept
        - name: billing
          value: # < Service specific Endpoint URL >
        - name: apikey
          value: # < Service specific API key >
      resources:
        requests:
          cpu: 4 # Always refer to recommended minimal resources
          memoryInGb: 8 # Always refer to recommended minimal resources
      ports:
        - port: 5000
  osType: Linux
  volumes: # This node, is only required for container instances that pull their model in at runtime, such as LUIS.
  - name: aci-file-share
    azureFile:
      shareName: # < File share name >
      storageAccountName: # < Storage account name>
      storageAccountKey: # < Storage account key >
  restartPolicy: OnFailure
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: 5000
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

> [!NOTE]
> Tüm konumlar aynı CPU ve Bellek kullanılabilirliğine sahip değildir. Konum ve işletim sistemi başına kapsayıcılar için kullanılabilir kaynakların listelenmesi için konum ve [kaynaklar][location-to-resource] tablosuna bakın.

Komut için oluşturduğumuz YAML dosyasına [`az container create`][azure-container-create] güveneceğiz. Azure CLI'den, `az container create` kendi komutunuzun yerine `<resource-group>` komutu uygulayın. Ayrıca, BIR YAML dağıtım içinde değerleri güvence altına almak için [güvenli değerlere][secure-values]bakın.

```azurecli
az container create -g <resource-group> -f my-aci.yaml
```

Komutun çıktısı, `Running...` bir süre sonra çıktıyeni oluşturulan ACI kaynağını temsil eden bir JSON dizesine değişikliklerden sonra geçerliyse. Kapsayıcı görüntüsü büyük olasılıkla bir süre için kullanılamıyor, ancak kaynak şimdi dağıtılır.

> [!TIP]
> YAML'nin konuma uyacak şekilde ayarlanması gerektiğinden, herkese açık önizleme Azure Bilişsel Hizmet tekliflerinin konumlarına çok dikkat edin.

[azure-container-create]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-create
[template-format]: https://docs.microsoft.com/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups#template-format
[aci-yaml-ref]: ../../../container-instances/container-instances-reference-yaml.md
[repositories-and-images]: ../../cognitive-services-container-support.md#container-repositories-and-images
[location-to-resource]: ../../../container-instances/container-instances-region-availability.md#availability---general
[secure-values]: ../../../container-instances/container-instances-environment-variables.md#secure-values
