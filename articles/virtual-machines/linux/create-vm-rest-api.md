---
title: REST API ile bir Linux VM oluşturma
description: Azure'da Yönetilen Diskler ve Azure REST API ile SSH kimlik doğrulamasını kullanan bir Linux sanal makinesini nasıl oluşturabilirsiniz öğrenin.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 06/05/2018
ms.author: cynthn
ms.openlocfilehash: 1594c030839cccdd48c4b032c6ad92f746f78e26
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970266"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>REST API ile SSH kimlik doğrulamasını kullanan bir Linux sanal makinesi oluşturun

Azure'daki Bir Linux sanal makinesi (VM), diskler ve ağ arabirimleri gibi çeşitli kaynaklardan oluşur ve konum, boyut ve işletim sistemi görüntüsü ve kimlik doğrulama ayarları gibi parametreleri tanımlar.

Azure portalı, Azure CLI 2.0, çok sayıda Azure SDK'sı, Azure Kaynak Yöneticisi şablonu ve Ansible veya Terraform gibi birçok üçüncü taraf aracı aracılığıyla bir Linux VM oluşturabilirsiniz. Tüm bu araçlar sonuçta Linux VM oluşturmak için REST API kullanın.

Bu makalede, yönetilen diskler ve SSH kimlik doğrulaması ile Ubuntu 18.04-LTS çalıştıran bir Linux VM oluşturmak için REST API nasıl kullanılacağını gösterir.

## <a name="before-you-start"></a>Başlamadan önce

İsteği oluşturmadan ve göndermeden önce şunları yapmanız gerekir:

* Aboneliğiniz `{subscription-id}` için
  * Birden çok aboneliğiniz [varsa,](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest) bkz.
* Önceden `{resourceGroupName}` yarattığınız bir
* Aynı kaynak grubunda [sanal ağ arabirimi](../../virtual-network/virtual-network-network-interface.md)
* Bir SSH anahtar çifti (yoksa yeni bir anahtar [çifti oluşturabilirsiniz)](mac-create-ssh-keys.md)

## <a name="request-basics"></a>Temel leri isteme

Sanal bir makine oluşturmak veya güncelleştirmek için aşağıdaki *PUT* işlemini kullanın:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

`{subscription-id}` Ve `{resourceGroupName}` parametrelere ek olarak, `{vmName}` (isteğe`api-version` bağlı, ancak bu makale ile `api-version=2017-12-01`test edildi) belirtmeniz gerekir

Aşağıdaki üstbilgiler gereklidir:

| İstek üst bilgisi   | Açıklama |
|------------------|-----------------|
| *İçerik Türü:*  | Gereklidir. `application/json` olarak ayarlayın. |
| *Yetkilendirme:* | Gereklidir. Geçerli bir `Bearer` [erişim belirtecine](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients) ayarlayın. |

REST API istekleriyle çalışma hakkında genel bilgi için, [REST API isteğinin/yanıtının bileşenlerine](/rest/api/azure/#components-of-a-rest-api-requestresponse)bakın.

## <a name="create-the-request-body"></a>İstek gövdesini oluşturma

Bir istek gövdesi oluşturmak için aşağıdaki yaygın tanımlar kullanılır:

| Adı                       | Gerekli | Tür                                                                                | Açıklama  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | True     | string                                                                              | Kaynak konumu. |
| ad                       |          | string                                                                              | Sanal makinenin adı. |
| properties.hardwareProfil |          | [Donanım Profili](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Sanal makinenin donanım ayarlarını belirtir. |
| properties.storageProfil  |          | [Depolama Profili](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Sanal makine disklerinin depolama ayarlarını belirtir. |
| properties.osProfil       |          | [OSProfil](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Sanal makinenin işletim sistemi ayarlarını belirtir. |
| properties.networkProfil  |          | [Ağ Profili](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Sanal makinenin ağ arabirimlerini belirtir. |

Örnek bir istek gövdesi aşağıdadır. `{computerName}` VM adını ve `{name}` parametrelerini, oluşturduğunuz ağ arabiriminin `networkInterfaces`adını, içinde kullanıcı `adminUsername` adınız `path`ve , SSH anahtar çiftinizin *ortak* bölümünü (örneğin, `~/.ssh/id_rsa.pub` `keyData`) . Değiştirmek isteyebileceğin diğer `location` parametreler `vmSize`şunlardır ve .  

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

İstek gövdesindeki kullanılabilir tanımların tam listesi için, [Sanal makinelerin istek gövdesi tanımlarını oluşturması veya güncellmesini](/rest/api/compute/virtualmachines/createorupdate#definitions)görün.

## <a name="sending-the-request"></a>İsteğin gönderilmesi

Bu HTTP isteğini göndermek için tercih ettiğiniz istemciyi kullanabilirsiniz. **Ayrıca, Try it** düğmesini tıklatarak [tarayıcı içi](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate) bir araç da kullanabilirsiniz.

### <a name="responses"></a>Yanıtlar

Sanal bir makine oluşturmak veya güncelleştirmek için işlem için iki başarılı yanıt vardır:

| Adı        | Tür                                                                              | Açıklama |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 TAMAM      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Tamam          |
| 201 Oluşturuldu | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Oluşturulan     |

Bir yoğunlaştırılmış *201* Bir VM oluşturur önceki örnek istek gövdesinden oluşturulan yanıt bir *vmId* atanmış olduğunu gösterir ve *provisioningState* *Oluşturma:*

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

REST API yanıtları hakkında daha fazla bilgi için [yanıt iletisini işleme](/rest/api/azure/#process-the-response-message)ye bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure REST API'leri veya Azure CLI veya Azure PowerShell gibi diğer yönetim araçları hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Azure İşlem sağlayıcısı REST API](/rest/api/compute/)
- [Azure REST API’yi kullanmaya başlayın](/rest/api/azure/)
- [Azure CLI](/cli/azure/)
- [Azure PowerShell modülü](/powershell/azure/overview)
