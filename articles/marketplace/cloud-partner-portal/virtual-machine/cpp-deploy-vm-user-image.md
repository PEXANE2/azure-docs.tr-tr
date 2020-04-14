---
title: Bir kullanıcı VHD'sinden Azure VM dağıtma | Azure Marketi
description: Azure VM örneği oluşturmak için kullanıcı VHD görüntüsünün nasıl dağıtılangerektiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 79754b4ce7c3dfe2a5c549f4a39ef3160be423d8
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273898"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>Bir kullanıcı VHD'sinden Azure VM dağıtma

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure VM görüntü sertifikasındaki](https://aks.ms/CertifyVMimage) yönergeleri izleyin.

Bu makalede, sağlanan Azure Kaynak Yöneticisi şablonu ve Azure PowerShell komut dosyası kullanılarak yeni bir Azure VM kaynağı oluşturmak için genelleştirilmiş bir VHD görüntüsünasıl dağıtılanınca açıklanmaktadır.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vhd-deployment-template"></a>VHD dağıtım şablonu

[VHD dağıtımı](cpp-deploy-json-template.md) için Azure Kaynak Yöneticisi şablonu `VHDtoImage.json`adlı yerel bir dosyaya kopyalayın.  Aşağıdaki parametreler için değerler sağlamak için bu dosyayı edin. 

|  **Parametre**             |   **Açıklama**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | Varolan Azure kaynak grubu adı.  Genellikle anahtar kasanızla ilişkili aynı RG'yi kullanın  |
| Templatefile               | Dosyaya tam yol adı`VHDtoImage.json`                                    |
| userStorageAccountName     | Depolama hesabının adı                                                    |
| sNameForPublicIP           | Genel IP için DNS adı. Küçük olmalı                                  |
| subscriptionId             | Azure abonelik tanımlayıcısı                                                  |
| Konum                   | Kaynak grubunun standart Azure coğrafi konumu                       |
| vmName                     | Sanal makinenin adı                                                    |
| vaultName                  | Anahtar kasasının adı                                                          |
| vaultResourceGroup         | Anahtar kasasının kaynak grubu
| sertifikaUrl             | Anahtar kasasında depolanan sürüm de dahil olmak üzere sertifikanın url'si:`https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
| vhdUrl                     | Sanal sabit diskin URL'si                                                   |
| vmSize                     | Sanal makine örneğinin boyutu                                           |
| publicIPAddressName        | Genel IP adresinin adı                                                  |
| virtualNetworkName         | Sanal ağın adı                                                    |
| nicName                    | Sanal ağ için ağ arabirim kartının adı                     |
| adminUserName              | Yönetici hesabının kullanıcı adı                                          |
| adminPassword              | Yönetici parolası                                                          |
|  |  |


## <a name="powershell-script"></a>Powershell komut dosyası

Aşağıdaki komut dosyasını kopyalayın ve `$storageaccount` değişkenler için değerler sağlamak için edin. `$vhdUrl`  Varolan genelleştirilmiş VHD'nizden bir Azure VM kaynağı oluşturmak için çalıştırın.

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

VM'niz dağıtıldıktan [sonra, VM görüntünüzü onaylamaya](./cpp-certify-vm.md)hazırsınız.
