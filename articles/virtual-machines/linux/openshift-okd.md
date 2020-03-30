---
title: Azure'da OKD dağıtma
description: OKD'yi Azure'da dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/15/2019
ms.author: haroldw
ms.openlocfilehash: 6ecae09b0efbc0e35e591b31dbb54fb49ce101b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74035395"
---
# <a name="deploy-okd-in-azure"></a>Azure'da OKD dağıtma

Azure'da OKD 'yi (eski adıyla OpenShift Origin) dağıtmanın iki yolunu kullanabilirsiniz:

- Gerekli tüm Azure altyapı bileşenlerini el ile dağıtabilir ve [okd belgelerini](https://docs.okd.io)takip edebilirsiniz.
- OKD kümesinin dağıtımını kolaylaştıran varolan bir [Kaynak Yöneticisi şablonu](https://github.com/Microsoft/openshift-origin) da kullanabilirsiniz.

## <a name="deploy-using-the-okd-template"></a>OKD şablonundan kullanarak dağıtma

Kaynak Yöneticisi şablonu kullanarak dağıtmak için, giriş parametrelerini sağlamak için bir parametre dosyası kullanırsınız. Dağıtımı daha da özelleştirmek için GitHub repo'yu çatalla ve uygun öğeleri değiştirin.

Bazı yaygın özelleştirme seçenekleri şunlardır, ancak bunlarla sınırlı değildir:

- Burç VM boyutu (azuredeploy.json değişkeni)
- Adlandırma kuralları (azuredeploy.json'daki değişkenler)
- OpenShift küme özellikleri, ana bilgisayar dosyası (deployOpenShift.sh) üzerinden değiştirildi

[OKD şablonunun OKD'nin](https://github.com/Microsoft/openshift-origin) farklı sürümleri için birden çok dal vardır.  İhtiyaçlarınıza bağlı olarak, doğrudan repo'dan dağıtabilir veya dağıtımından önce repo'yu çatallayabilir ve özel değişiklikler yapabilirsiniz.

Parametre `appId` için daha önce oluşturduğunuz hizmet `aadClientId` ilkesinden gelen değeri kullanın.

Aşağıda, gerekli tüm girdileri içeren azuredeploy.parameters.json adlı bir parametre dosyası örneği verilmiştir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "storageKind": {
            "value": "managed"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "false"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "enableAzure": {
            "value": "true"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

Parametreleri özel bilgilerinizle değiştirin.

Farklı sürümler farklı parametrelere sahip olabilir, bu nedenle lütfen kullandığınız dal için gerekli parametreleri doğrulayın.

### <a name="deploy-using-azure-cli"></a>Azure CLI’yi kullanarak dağıtma


> [!NOTE] 
> Aşağıdaki komut Azure CLI 2.0.8 veya daha sonra gerektirir. CLI sürümünü `az --version` komutla doğrulayabilirsiniz. CLI sürümünü güncelleştirmek için [Azure CLI'yi yükle'ye](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)bakın.

Aşağıdaki örnek, OKD kümesini ve ilgili tüm kaynakları openshiftrg adlı bir kaynak grubuna, myOpenShiftCluster dağıtım adı ile dağıtır. Şablon, azuredeploy.parameters.json adlı yerel bir parametre dosyasını kullanırken doğrudan GitHub repo'sundan başvurulur.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtımın tamamlanması, dağıtılan toplam düğüm sayısına bağlı olarak en az 30 dakika sürer. OpenShift konsolunun URL'si ve OpenShift ana sının DNS adı dağıtım bittiğinde terminale yazdırılır. Alternatif olarak, Azure portalından dağıtımın çıktıları bölümünü görüntüleyebilirsiniz.

```json
{
  "OpenShift Console Url": "http://openshiftlb.cloudapp.azure.com/console",
  "OpenShift Master SSH": "ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com"
}
```

Dağıtımın tamamlanmasını bekleyen komut satırını bağlamak istemiyorsanız, grup `--no-wait` dağıtımı seçeneklerinden birini ekleyin. Dağıtımdan elde edilen çıktı, kaynak grubunun dağıtım bölümündeki Azure portalından alınabilir.

## <a name="connect-to-the-okd-cluster"></a>OKD kümesine bağlanma

Dağıtım bittiğinde, openshift konsoluna tarayıcınızla `OpenShift Console Url`bağlanın. Alternatif olarak, OKD yöneticisine SSH yapabilirsiniz. Dağıtımçıktısını kullanan bir örnek aşağıda verilmiştir:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu, OpenShift kümesini ve ilgili tüm kaynakları artık ihtiyaç duyulmadığında kaldırmak için [az grubu silme](/cli/azure/group) komutunu kullanın.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-container-platform-3x-post-deployment.md)
- [OpenShift dağıtımını giderme](./openshift-container-platform-3x-troubleshooting.md)
- [OKD ile başlarken](https://docs.okd.io)
