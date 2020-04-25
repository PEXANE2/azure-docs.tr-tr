---
title: Bir Kullanıcı VHD 'sinden Azure VM dağıtma | Azure Marketi
description: Bir Azure VM örneği oluşturmak için bir Kullanıcı VHD görüntüsünün nasıl dağıtılacağını açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: e3bad2dc63f6a75f52c537aabfa6e85d1846ef15
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82147925"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>Bir Azure VM 'yi bir Kullanıcı VHD 'sinden dağıtma

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure VM görüntü sertifikadaki](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-image-certification) yönergeleri izleyin.

Bu makalede, sağlanan Azure Resource Manager şablonu ve Azure PowerShell betiği kullanılarak yeni bir Azure VM kaynağı oluşturmak için genelleştirilmiş bir VHD görüntüsünün nasıl dağıtılacağı açıklanır.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vhd-deployment-template"></a>VHD dağıtım şablonu

[VHD dağıtımı](cpp-deploy-json-template.md) için Azure Resource Manager şablonunu adlı `VHDtoImage.json`yerel bir dosyaya kopyalayın.  Aşağıdaki parametreler için değerler sağlamak üzere bu dosyayı düzenleyin. 

|  **Parametre**             |   **Açıklama**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | Mevcut Azure Kaynak grubu adı.  Genellikle anahtar kasanız ile ilişkili RG 'yi kullanın  |
| TemplateFile               | Dosyanın tam yol adı`VHDtoImage.json`                                    |
| userStorageAccountName     | Depolama hesabının adı                                                    |
| Snameforpublicıp           | Genel IP için DNS adı. Küçük harfle yazılmalıdır                                  |
| subscriptionId             | Azure abonelik tanımlayıcısı                                                  |
| Konum                   | Kaynak grubunun standart Azure coğrafi konumu                       |
| vmName                     | Sanal makinenin adı                                                    |
| vaultName                  | Anahtar kasasının adı                                                          |
| vaultResourceGroup         | Anahtar kasasının kaynak grubu
| Sertifika URL 'si             | Anahtar kasasında depolanan sürüm dahil olmak üzere sertifikanın URL 'si (örneğin:`https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
| vhdUrl                     | Sanal sabit diskin URL 'SI                                                   |
| vmSize                     | Sanal makine örneğinin boyutu                                           |
| Publicıpaddressname        | Genel IP adresinin adı                                                  |
| virtualNetworkName         | Sanal ağın adı                                                    |
| nicName                    | Sanal ağ için ağ arabirim kartının adı                     |
| adminUserName              | Yönetici hesabının Kullanıcı adı                                          |
| adminPassword              | Yönetici parolası                                                          |
|  |  |


## <a name="powershell-script"></a>PowerShell betiği

`$storageaccount` Ve `$vhdUrl` değişkenleri için değerler sağlamak üzere aşağıdaki betiği kopyalayın ve düzenleyin.  Mevcut Genelleştirilmiş VHD 'nizden bir Azure VM kaynağı oluşturmak için yürütün.

```powershell
# storage account of existing generalized VHD 
$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

#deploying VM with existing VHD
New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd 

```

## <a name="next-steps"></a>Sonraki adımlar

VM 'niz dağıtıldıktan sonra [VM görüntünüzü onaylamaya](./cpp-certify-vm.md)hazırsınızdır.
