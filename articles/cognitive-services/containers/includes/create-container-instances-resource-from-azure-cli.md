---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Azure CLı 'dan Azure Container Instance kaynağı oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/06/2020
ms.author: dapine
ms.openlocfilehash: 700a04b58c13a9c7fd5301875226ca234cabeb96
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75689436"
---
## <a name="create-an-azure-container-instance-resource-from-the-azure-cli"></a>Azure CLı 'dan Azure Container Instance kaynağı oluşturma

Aşağıdaki YAML, Azure Container Instance kaynağını tanımlar. İçeriği kopyalayıp `my-aci.yaml` adlı yeni bir dosyaya yapıştırın ve açıklama değerlerini kendi değerlerinizle değiştirin. Geçerli YAML için [şablon biçimine][template-format] bakın. Kullanılabilir görüntü adları ve bunlara karşılık gelen depo için [kapsayıcı depoları ve görüntüleri][repositories-and-images] inceleyin. Kapsayıcı örnekleri için YAML başvurusu hakkında daha fazla bilgi için bkz. [YAML başvurusu: Azure Container Instances][aci-yaml-ref].

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
> Tüm konumlar aynı CPU ve bellek kullanılabilirliğine sahip değildir. Konum ve işletim sistemi başına kapsayıcılar için kullanılabilir kaynakların listesinin [konumu ve kaynakları][location-to-resource] tablosuna bakın.

[`az container create`][azure-container-create] komutu için oluşturduğumuz YAML dosyasına güveneceğiz. Azure CLı 'dan, `<resource-group>` `az container create` komutunu kendi kendinize koyarak yürütün. Ek olarak, bir YAML dağıtımı içindeki değerlerin güvenliğini sağlamak için [güvenli değerler][secure-values]bölümüne bakın.

```azurecli
az container create -g <resource-group> -f my-aci.yaml
```

Komutun çıkışı geçerli ise `Running...`, çıkış sonrasında yeni oluşturulan ACI kaynağını temsil eden bir JSON dizesine değişir. Kapsayıcı görüntüsü büyük olasılıkla bir süre için kullanılamayabilir, ancak kaynak artık dağıtılır.

> [!TIP]
> YAML 'nin konumla eşleşecek şekilde ayarlanması gerektiği için, genel önizleme Azure bilişsel hizmet tekliflerinin konumlarına yakın bir şekilde dikkat edin.

[azure-container-create]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-create
[template-format]: https://docs.microsoft.com/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups#template-format
[aci-yaml-ref]: ../../../container-instances/container-instances-reference-yaml.md
[repositories-and-images]: ../../cognitive-services-container-support.md#container-repositories-and-images
[location-to-resource]: ../../../container-instances/container-instances-region-availability.md#availability---general
[secure-values]: ../../../container-instances/container-instances-environment-variables.md#secure-values
