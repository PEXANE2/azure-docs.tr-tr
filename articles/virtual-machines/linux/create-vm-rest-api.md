---
title: Azure REST API Linux sanal makinesi oluşturma | Microsoft Docs
description: Azure REST API ile yönetilen diskler ve SSH kimlik doğrulaması kullanan Azure 'da Linux sanal makinesi oluşturmayı öğrenin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2018
ms.author: cynthn
ms.openlocfilehash: 9851305bdaa2f214e0d00eda3235068cac2ea980
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70083471"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>REST API ile SSH kimlik doğrulaması kullanan bir Linux sanal makinesi oluşturma

Azure 'daki bir Linux sanal makinesi (VM), diskler ve ağ arabirimleri gibi çeşitli kaynaklardan oluşur ve konum, boyut ve işletim sistemi görüntüsü ve kimlik doğrulama ayarları gibi parametreleri tanımlar.

Azure portal, Azure CLı 2,0, birçok Azure SDK, Azure Resource Manager şablon ve anormal ya da Terırform gibi birçok üçüncü taraf araç aracılığıyla bir Linux sanal makinesi oluşturabilirsiniz. Bu araçların hepsi, Linux VM oluşturmak için REST API kullanır.

Bu makalede, yönetilen diskler ve SSH kimlik doğrulamasıyla Ubuntu 18,04-LTS çalıştıran bir Linux sanal makinesi oluşturmak için REST API nasıl kullanılacağı gösterilmektedir.

## <a name="before-you-start"></a>Başlamadan önce

İsteği oluşturmadan ve göndermeden önce şunları yapmanız gerekir:

* Aboneliğiniz `{subscription-id}` için
  * Birden çok aboneliğiniz varsa bkz. [birden çok abonelikle çalışma](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)
* Bir `{resourceGroupName}` süre önce oluşturduğunuz
* Aynı kaynak grubundaki bir [sanal ağ arabirimi](../../virtual-network/virtual-network-network-interface.md)
* Bir SSH anahtar çifti (bir tane yoksa [Yeni bir tane](mac-create-ssh-keys.md) oluşturabilirsiniz)

## <a name="request-basics"></a>İstek temelleri

Bir sanal makine oluşturmak veya güncelleştirmek için aşağıdaki *PUT* işlemini kullanın:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

`{subscription-id}` Ve `{vmName}` `api-version=2017-12-01`parametrelerine ek olarak,(`api-version` isteğe bağlı, ancak bu makale ile test edilmiştir) belirtmeniz gerekir. `{resourceGroupName}`

Aşağıdaki üstbilgiler gereklidir:

| İstek üst bilgisi   | Açıklama |
|------------------|-----------------|
| *Content-Type:*  | Gerekli. Olarak `application/json`ayarlayın. |
| *Authorization:* | Gerekli. Geçerli `Bearer` bir [erişim belirtecine](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients)ayarlayın. |

REST API isteklerle çalışma hakkında genel bilgi için, bkz. [bir REST API isteği/yanıtı bileşenleri](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>İstek gövdesini oluşturma

Aşağıdaki ortak tanımlar bir istek gövdesi oluşturmak için kullanılır:

| Name                       | Gerekli | Tür                                                                                | Açıklama  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | Doğru     | dize                                                                              | Kaynak konumu. |
| name                       |          | dize                                                                              | Sanal makinenin adı. |
| Properties. hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Sanal makine için donanım ayarlarını belirtir. |
| Properties. storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Sanal makine disklerinin depolama ayarlarını belirtir. |
| Properties. osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Sanal makine için işletim sistemi ayarlarını belirtir. |
| Properties. networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Sanal makinenin ağ arabirimlerini belirtir. |

Örnek bir istek gövdesi aşağıda verilmiştir. `{computerName}` Ve `path` `adminUsername` parametrelerinde, sanal makine adını, altında `networkInterfaces`oluşturduğunuz ağ arabiriminin adını, ve içindeki kullanıcı adınızı ve SSH 'nizin genel kısmını belirttiğinizden emin olun `{name}` KeyPair (örneğin `~/.ssh/id_rsa.pub`, içinde bulunur `keyData`). Değiştirmek isteyebileceğiniz diğer parametreler, ve `location` `vmSize`içerir.  

```json
{
  "location": "eastus",
  "name": "{vmName}",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_DS1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "18.04-LTS",
        "publisher": "Canonical",
        "version": "latest",
        "offer": "UbuntuServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "{vmName}",
      "linuxConfiguration": {
        "ssh": {
          "publicKeys": [
            {
              "path": "/home/{your-username}/.ssh/authorized_keys",
              "keyData": "ssh-rsa AAAAB3NzaC1{snip}mf69/J1"
            }
          ]
        },
        "disablePasswordAuthentication": true
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  }
}
```

İstek gövdesindeki kullanılabilir tanımlarının tüm listesi için bkz. [sanal makineler istek gövdesi tanımları oluşturma veya güncelleştirme](/rest/api/compute/virtualmachines/createorupdate#definitions).

## <a name="sending-the-request"></a>İstek gönderiliyor

Bu HTTP isteğini göndermek için tercih ettiğiniz istemciyi kullanabilirsiniz. Ayrıca, **deneyin** düğmesine tıklayarak [tarayıcı içi bir araç](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate) da kullanabilirsiniz.

### <a name="responses"></a>Responses

Bir sanal makineyi oluşturma veya güncelleştirme işlemi için iki başarılı yanıt vardır:

| Name        | Tür                                                                              | Açıklama |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 TAMAM      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Tamam          |
| 201 oluşturuldu | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Oluşturuldu     |

Bir VM oluşturan önceki örnek istek gövdesinden sıkıştırılmış *201 tarafından oluşturulan* bir yanıt, bir *VMID* atandığını ve *provisioningstate* 'in *oluşturuyor*olduğunu gösterir:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

REST API yanıtları hakkında daha fazla bilgi için bkz. [Yanıt Iletisini işleme](/rest/api/azure/#process-the-response-message).

## <a name="next-steps"></a>Sonraki adımlar

Azure REST API 'Leri veya Azure CLı veya Azure PowerShell gibi diğer yönetim araçları hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Azure Işlem sağlayıcısı REST API](/rest/api/compute/)
- [Azure REST API kullanmaya başlama](/rest/api/azure/)
- [Azure CLI](/cli/azure/)
- [Azure PowerShell modülü](/powershell/azure/overview)
