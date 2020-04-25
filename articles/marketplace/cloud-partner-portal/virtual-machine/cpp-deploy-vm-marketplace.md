---
title: Azure Marketi 'nden bir VM dağıtma
description: Azure Marketi önceden yapılandırılmış bir sanal makineden bir sanal makinenin nasıl dağıtılacağını açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 2fb04e2e953e2bcbe479f7685b8042fdc7ea1767
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82147003"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>Azure Marketi 'nden bir sanal makine dağıtma

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure VM görüntü sertifikadaki](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-image-certification) yönergeleri izleyin.

Bu makalede, belirtilen Azure PowerShell betiği kullanılarak Azure Marketi 'nden önceden yapılandırılmış bir sanal makinenin (VM) nasıl dağıtılacağı açıklanmaktadır.  Bu betik Ayrıca sanal makinede WinRM HTTP ve HTTPS uç noktalarını da kullanıma sunar.  Betik, [Azure Key Vault için sertifika oluşturma](./cpp-create-key-vault-cert.md)bölümünde açıklandığı gibi Azure Key Vault için karşıya yüklenmiş bir sertifikanız olmasını gerektirir. 

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vm-deployment-template"></a>VM dağıtım şablonu

Hızlı başlangıç Azure VM dağıtım şablonu, [azuredeploy. JSON](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json)çevrimiçi dosyası olarak kullanılabilir.  Aşağıdaki parametreleri içerir:

|  **Parametre**        |   **Açıklama**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName    | Depolama hesabının adı                       |
| Dnsnameforpublicıp    | Genel IP için DNS adı. Küçük harfle yazılmalıdır.    |
| adminUserName            | Yöneticinin Kullanıcı adı                          |
| adminPassword            | Yönetici parolası                          |
| ımagepublisher        | Görüntü yayımcısı                                   |
| ımageteklif            | Görüntü teklifi                                       |
| Imate ku                | Görüntü SKU 'SU                                         |
| vmSize                | VM boyutu                                    |
| vmName                | VM adı                                    |
| vaultName                | Anahtar kasasının adı                             |
| vaultResourceGroup    | Anahtar kasasının kaynak grubu                   |
| Sertifika URL 'si        | Anahtar Kasası sürümü de dahil olmak üzere sertifika URL 'SI (örneğin,)`https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
|  |  |


## <a name="deployment-script"></a>Dağıtım betiği

Aşağıdaki Azure PowerShell betiğini düzenleyin ve belirtilen Azure Marketi VM 'sini dağıtmak için yürütün.

```powershell

New-AzResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış bir VM 'yi dağıttıktan sonra, içerdiği çözümleri ve Hizmetleri yapılandırıp buna erişebilir veya daha fazla geliştirme için kullanabilirsiniz. 
