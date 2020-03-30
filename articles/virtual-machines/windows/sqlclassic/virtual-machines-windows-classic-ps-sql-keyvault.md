---
title: Key Vault'u klasik bir Azure SQL Server VM ile tümleştirin
description: Azure Key Vault ile kullanılmak üzere SQL Server şifreleme yapılandırmasını nasıl otomatikleştirebilirsiniz öğrenin. Bu konu, klasik dağıtım modelinde oluşturduğu SQL Server sanal makineleriyle Azure Key Vault Tümleştirmesinin nasıl kullanılacağını açıklar.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: ab8d41a7-1971-4032-ab71-eb435c455dc1
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/17/2017
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: f878c6f7a59328e2f68ffbaee066bba4a5b6c898
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978141"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-classic"></a>Azure Sanal Makinelerde SQL Server için Azure Anahtar Kasası Tümleştirmesini Yapılandırma (Klasik)
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](../sql/virtual-machines-windows-ps-sql-keyvault.md)
> * [Klasik](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Genel Bakış
[Saydam veri şifreleme (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sütun düzeyi şifreleme (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)ve [yedekleme şifreleme](https://msdn.microsoft.com/library/dn449489.aspx)gibi birden çok SQL Server şifreleme özelliği vardır. Bu şifreleme biçimleri, şifreleme için kullandığınız şifreleme anahtarlarını yönetmenizi ve depolamanızı gerektirir. Azure Key Vault (AKV) hizmeti, bu anahtarların güvenliğini ve yönetimini güvenli ve yüksek kullanılabilir bir konumda geliştirmek için tasarlanmıştır. [SQL Server Bağlayıcısı,](https://www.microsoft.com/download/details.aspx?id=45344) SQL Server'ın bu anahtarları Azure Key Vault'tan kullanmasını sağlar.

> [!IMPORTANT] 
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Sql Server'ı şirket içi makinelerle çalıştırıyorsanız, [şirket içi SQL Server makinenizden Azure Key Vault'a erişmek için izleyebileceğiniz adımlar](https://msdn.microsoft.com/library/dn198405.aspx)vardır. Ancak Azure VM'lerde SQL Server için *Azure Anahtar Kasası Tümleştirme* özelliğini kullanarak zamandan tasarruf edebilirsiniz. Bu özelliği etkinleştirmek için birkaç Azure PowerShell cmdlet ile, bir SQL VM'nin anahtar kasanıza erişebilmesi için gerekli yapılandırmayı otomatikleştirebilirsiniz.

Bu özellik etkinleştirildiğinde, SQL Server Bağlayıcısı'nı otomatik olarak yükler, EKM sağlayıcısını Azure Key Vault'a erişmek üzere yapılandırır ve kasanıza erişmenizi sağlayacak kimlik bilgilerini oluşturur. Daha önce bahsedilen şirket içi belgelerdeki adımlara bakarsanız, bu özelliğin 2 ve 3 adımlarını otomatikleştirdiğini görebilirsiniz. Hala el ile yapmanız gereken tek şey anahtar tonoz ve anahtarları oluşturmaktır. Buradan, SQL VM'nizin tüm kurulumu otomatiktir. Bu özellik bu kurulumu tamamladıktan sonra, veritabanlarınızı veya yedeklemelerinizi normalde yaptığınız gibi şifrelemeye başlamak için T-SQL deyimlerini yürütebilirsiniz.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV Entegrasyonunu Yapılandır
Azure Anahtar Kasası Tümleştirmesini yapılandırmak için PowerShell'i kullanın. Aşağıdaki bölümler, gerekli parametrelere genel bir bakış ve ardından örnek bir PowerShell komut dosyası sağlar.

### <a name="install-the-sql-server-iaas-extension"></a>SQL Server IaaS Uzantısını Yükleme
İlk olarak, [SQL Server IaaS Uzantısı yükleyin.](../classic/sql-server-agent-extension.md)

### <a name="understand-the-input-parameters"></a>Giriş parametrelerini anlama
Aşağıdaki tabloda PowerShell komut dosyasını çalıştırmak için gereken parametreler sonraki bölümde listelenmiştir.

| Parametre | Açıklama | Örnek |
| --- | --- | --- |
| **$akvURL** |**Anahtar kasa URL'si** |"https:\//contosokeyvault.vault.azure.net/" |
| **$spName** |**Hizmet Müdürü adı** |"fde2b411-33d5-4e11-af04eb07b669ccf2" |
| **$spSecret** |**Hizmet Müdürü sırrı** |"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM=" |
| **$credName** |**Kimlik bilgisi adı**: AKV Tümleştirme, VM’nin anahtar kasasına erişim sağlamasına izin vererek, SQL Server’da bir kimlik bilgisi oluşturur Bu kimlik bilgisi için bir ad seçin. |"mycred1" |
| **$vmName** |**Sanal makine adı**: Daha önce oluşturulmuş bir SQL VM'nin adı. |"myvmname" |
| **$serviceName** |**Hizmet adı**: SQL VM ile ilişkili Bulut Hizmeti adı. |"mycloudservicename" |

### <a name="enable-akv-integration-with-powershell"></a>PowerShell ile AKV Entegrasyonunu Etkinleştirin
**Yeni AzureVMSqlSqlKeyVaultCredentialConfig** cmdlet, Azure Anahtar Kasası Tümleştirme özelliği için bir yapılandırma nesnesi oluşturur. **Set-AzureVMSqlServerExtension,** bu tümleştirmeyi **KeyVaultCredentialSettings** parametresi ile yapılandırır. Aşağıdaki adımlar, bu komutların nasıl kullanılacağını gösterir.

1. Azure PowerShell'de, giriş parametrelerini önce bu konunun önceki bölümlerinde açıklandığı gibi belirli değerlerinizle yapılandırın. Aşağıdaki komut dosyası bir örnektir.
   
        $akvURL = "https:\//contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2. Akv Tümleştirmesini yapılandırmak ve etkinleştirmek için aşağıdaki komut dosyasını kullanın.
   
        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

SQL IaaS Aracı Uzantısı bu yeni yapılandırma ile SQL VM güncelleştirecektir.

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

