---
title: Azure CLı ile DevTest Labs 'de sanal makineler oluşturma ve yönetme | Microsoft Docs
description: Azure CLı ile sanal makineler oluşturmak ve yönetmek için Azure DevTest Labs kullanmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: 11ac4e10cbd116ed204a8a11274408f5a5a9b4d9
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70183140"
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Azure CLı kullanarak DevTest Labs ile sanal makineler oluşturma ve yönetme
Bu hızlı başlangıç, laboratuvarınızda bir geliştirme makinesi oluşturma, başlatma, bağlama, güncelleştirme ve Temizleme sürecinde size kılavuzluk eder. 

Başlamadan önce:

* Bir laboratuvar oluşturulmadıysa, yönergeler [burada](devtest-lab-create-lab.md)bulunabilir.

* [Azure CLI 'Yı yükler](/cli/azure/install-azure-cli). Başlamak için az oturum çalıştırarak Azure ile bir bağlantı oluşturun. 

## <a name="create-and-verify-the-virtual-machine"></a>Sanal makineyi oluşturma ve doğrulama 
DevTest Labs ile ilgili komutları yürütmeden önce, `az account set` komutunu kullanarak uygun Azure bağlamını ayarlayın:

```azurecli
az account set --subscription 11111111-1111-1111-1111-111111111111
```

Sanal makine oluşturma komutu: `az lab vm create`. Laboratuvar, laboratuvar adı ve sanal makine adı için kaynak grubunun tümü gereklidir. Bağımsız değişkenlerin geri kalanı, sanal makine türüne bağlı olarak değişir.

Aşağıdaki komut, Azure Pazar yerinde Windows tabanlı bir görüntü oluşturur. Görüntünün adı, Azure portal kullanarak bir sanal makine oluştururken göreceğiniz şekilde aynıdır. 

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "Visual Studio Community 2017 on Windows Server 2016 (x64)" --image-type gallery --size 'Standard_D2s_v3' --admin-username 'AdminUser' --admin-password 'Password1!'
```

Aşağıdaki komut, laboratuvarda kullanılabilen özel bir görüntüye dayalı bir sanal makine oluşturur:

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "My Custom Image" --image-type custom --size 'Standard_D2s_v3' --admin-username 'AdminUser' --admin-password 'Password1!'
```

**Image-Type** bağımsız değişkeni **Galeriden** **özel**olarak değiştirildi. Azure portal sanal makineyi oluştururken, görüntünün adı, gördüklerinize göre eşleşir.

Aşağıdaki komut, bir market görüntüsünden SSH kimlik doğrulaması ile bir VM oluşturur:

```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```

Ayrıca, **resim türü** parametresini **Formül**olarak ayarlayarak, formülleri temel alan sanal makineler de oluşturabilirsiniz. Sanal makineniz için belirli bir sanal ağ seçmeniz gerekiyorsa, **VNET-adı** ve **alt ağ** parametrelerini kullanın. Daha fazla bilgi için bkz. [az Lab VM Create](/cli/azure/lab/vm#az-lab-vm-create).

## <a name="verify-that-the-vm-is-available"></a>VM 'nin kullanılabilir olduğunu doğrulayın.
Başlamadan önce VM 'nin kullanılabilir olduğunu doğrulamak için komutunukullanın.`az lab vm show` 

```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-to-the-virtual-machine"></a>Başlat ve sanal makineyi bağla
Aşağıdaki örnek komut bir VM başlatır:

```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```

Bir VM 'ye bağlanın: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) veya [Uzak Masaüstü](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>Sanal makineyi Güncelleştir
Aşağıdaki örnek komut, yapıları bir VM 'ye uygular:

```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

Laboratuvarda bulunan yapıtları listeleyin.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a>Sanal Makineyi durdur ve Sil    
Aşağıdaki örnek komut bir VM 'yi sonlandırır.

```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

VM 'yi silme.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki içeriğe bakın: [Azure DevTest Labs Için Azure CLI belgeleri](/cli/azure/lab?view=azure-cli-latest). 
