---
title: Azure Marketinden Bir VM dağıtma
description: Azure Marketi önceden yapılandırılmış bir sanal makineden sanal makinenin nasıl dağıtılanbildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 6e39f1c70cd94c14b12e54817941ea9106aacfdd
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273877"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>Azure Marketi'nden sanal bir makine dağıtma

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure VM görüntü sertifikasındaki](https://aks.ms/CertifyVMimage) yönergeleri izleyin.

Bu makalede, sağlanan Azure PowerShell komut dosyasını kullanarak azure marketinden önceden yapılandırılmış bir sanal makinenin (VM) nasıl dağıtılancayakadar dağıtılanınca açıklanmıştır.  Bu komut dosyası aynı zamanda VM'deki WinRM HTTP ve HTTPS uç noktalarını da ortaya çıkarır.  Komut dosyası, [Azure Anahtar Kasası için Oluşturma sertifikalarında](./cpp-create-key-vault-cert.md)açıklandığı gibi Azure Key Vault'a zaten yüklenen bir sertifikanız olmasını gerektirir. 

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vm-deployment-template"></a>VM dağıtım şablonu

Hızlı başlatma Azure VM dağıtım şablonu, çevrimiçi dosya [azuredeployment.json](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json)olarak kullanılabilir.  Aşağıdaki parametreleri içerir:

|  **Parametre**        |   **Açıklama**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName    | Depolama hesabının adı                       |
| dnsNameForPublicIP    | Kamu IP için DNS Adı. Küçük olmalı.    |
| adminUserName            | Yöneticinin kullanıcı adı                          |
| adminPassword            | Yöneticinin şifresi                          |
| imagePublisher        | Görüntü yayıncısı                                   |
| imageTeklif            | Resim teklifi                                       |
| görüntüSKU                | Görüntü SKU                                         |
| vmSize                | VM boyutu                                    |
| vmName                | VM'nin adı                                    |
| vaultName                | Anahtar kasasının adı                             |
| vaultResourceGroup    | Anahtar kasasının kaynak grubu                   |
| sertifikaUrl        | Örneğin KeyVault sürümü de dahil olmak üzere sertifikanın URL'si`https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
|  |  |


## <a name="deployment-script"></a>Dağıtım betiği

Aşağıdaki Azure PowerShell komut dosyasını edin ve belirtilen Azure Marketi VM'sini dağıtmak için çalıştırın.

```powershell

New-AzResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış bir VM dağıttıktan sonra, içerdiği çözümleri ve hizmetleri yapılandırabilir ve erişebilir veya daha fazla geliştirme için kullanabilirsiniz. 
