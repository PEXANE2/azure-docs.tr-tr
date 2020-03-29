---
title: Azure CLI ile DevTest Labs'da sanal makineler oluşturun ve yönetin
description: Azure CLI ile sanal makineler oluşturmak ve yönetmek için Azure DevTest Laboratuvarlarını nasıl kullanacağınızı öğrenin
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
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: d3cd104e36cb407e9b1b833335869cac2c69d0ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76167065"
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Azure CLI'yi kullanarak DevTest Labs ile sanal makineler oluşturun ve yönetin
Bu hızlı başlangıç, laboratuarınızda bir geliştirme makinesi oluşturma, başlatma, bağlama, güncelleme ve temizleme konusunda size yol gösterecektir. 

Başlamadan önce:

* Bir laboratuvar oluşturulmamadıysa, yönergeler [burada](devtest-lab-create-lab.md)bulunabilir.

* [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli) Başlamak için Azure ile bağlantı oluşturmak için az oturum açın. 

## <a name="create-and-verify-the-virtual-machine"></a>Sanal makineoluşturma ve doğrulama 
DevTest Labs ile ilgili komutları yürütmeden önce, `az account set` komutu kullanarak uygun Azure bağlamını ayarlayın:

```azurecli
az account set --subscription 11111111-1111-1111-1111-111111111111
```

Sanal bir makine oluşturmak için `az lab vm create`komutu: . Laboratuvar, laboratuvar adı ve sanal makine adı için kaynak grubu gereklidir. Bağımsız değişkenlerin geri kalanı sanal makinenin türüne bağlı olarak değişir.

Aşağıdaki komut Azure Market Yeri'nden Windows tabanlı bir görüntü oluşturur. Görüntünün adı, Azure portalını kullanarak sanal bir makine oluştururken göreceğiniz le aynıdır. 

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "Visual Studio Community 2017 on Windows Server 2016 (x64)" --image-type gallery --size 'Standard_D2s_v3' --admin-username 'AdminUser' --admin-password 'Password1!'
```

Aşağıdaki komut, laboratuvarda bulunan özel bir görüntüyü temel alan sanal bir makine oluşturur:

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "My Custom Image" --image-type custom --size 'Standard_D2s_v3' --admin-username 'AdminUser' --admin-password 'Password1!'
```

**Görüntü türü** bağımsız **değişkeni galeriden** **özele**değiştirildi. Görüntünün adı, Azure portalında sanal makine yi oluştursanız da gördüğünüzle eşleşir.

Aşağıdaki komut, ssh kimlik doğrulaması ile bir pazar görüntüsünden bir VM oluşturur:

```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```

Ayrıca, **resim türü** parametresini **formüle**ayarlayarak formüllere dayalı sanal makineler de oluşturabilirsiniz. Sanal makineniz için belirli bir sanal ağ seçmeniz gerekiyorsa, **vnet adı** ve **alt ağ** parametrelerini kullanın. Daha fazla bilgi için az [lab vm oluştur'a](/cli/azure/lab/vm#az-lab-vm-create)bakın.

## <a name="verify-that-the-vm-is-available"></a>VM'nin kullanılabilir olduğundan doğrulayın.
VM'nin `az lab vm show` başlamadan ve bağlanmadan önce kullanılabilir olduğunu doğrulamak için komutu kullanın. 

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

## <a name="start-and-connect-to-the-virtual-machine"></a>Sanal makineyi başlatın ve bağlanın
Aşağıdaki örnek komut bir VM başlatır:

```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```

VM'ye bağlanın: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) veya [Uzak Masaüstü.](../virtual-machines/windows/connect-logon.md)
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>Sanal makineyi güncelleştirin
Aşağıdaki örnek komut, bir VM'ye eserler uygular:

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

### <a name="list-artifacts-available-in-the-lab"></a>Laboratuvarda bulunan yapıları listele

Bir laboratuvarda VM'de bulunan yapıları listelemek için aşağıdaki komutları çalıştırın.

**Cloud Shell - PowerShell**: $expand '$expand)\`içinde $ önce backtick ( ) kullanımı dikkat edin:

```azurecli-interactive
az lab vm show --resource-group <resourcegroupname> --lab-name <labname> --name <vmname> --expand "properties(`$expand=artifacts)" --query "artifacts[].{artifactId: artifactId, status: status}"
```

**Cloud Shell - Bash**: komutu\\$ önünde eğik çizgi ( ) karakterinin kullanımı dikkat edin. 

```azurecli-interactive
az lab vm show --resource-group <resourcegroupname> --lab-name <labname> --name <vmname> --expand "properties(\$expand=artifacts)" --query "artifacts[].{artifactId: artifactId, status: status}"
```

Örnek çıktı: 

```json
[
  {
    "artifactId": "/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.DevTestLab/labs/<lab name>/artifactSources/public repo/artifacts/windows-7zip",
    "status": "Succeeded"
  }
]
```

## <a name="stop-and-delete-the-virtual-machine"></a>Sanal makineyi durdurup silme    
Aşağıdaki örnek komut bir VM'yi durdurur.

```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Bir VM silin.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki içeriğe bakın: [Azure DevTest Labs için Azure CLI belgeleri.](/cli/azure/lab?view=azure-cli-latest) 
