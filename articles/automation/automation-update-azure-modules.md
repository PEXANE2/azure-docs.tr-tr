---
title: Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme
description: Bu makalede, Azure Automation 'da varsayılan olarak sunulan ortak Azure PowerShell modüllerini nasıl güncelleştirebileceğinizi açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 06/14/2019
ms.topic: conceptual
ms.openlocfilehash: 3f2ceda6e87ce16e910f3d215c1fabe81d522f32
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82855551"
---
# <a name="update-azure-powershell-modules-in-azure-automation"></a>Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme

Otomasyon hesabınızda Azure modüllerini güncelleştirmek için açık kaynak olarak kullanılabilen [Azure modüllerini Güncelleştir runbook 'unu](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update)kullanmanız gerekir. Azure modüllerinizi güncelleştirmek için **Update-AutomationAzureModulesForAccount** runbook 'unu kullanmaya başlamak için GitHub 'daki [Azure modüllerini Güncelleştir runbook deposundan](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) indirin. Daha sonra Otomasyon hesabınıza aktarabilir veya bir betik olarak çalıştırabilirsiniz. Otomasyon hesabınızda bir runbook 'un nasıl içeri aktarılacağını öğrenmek için bkz. [runbook 'U Içeri aktarma](manage-runbooks.md#import-a-runbook).

En yaygın PowerShell modülleri, her Otomasyon hesabında varsayılan olarak sağlanır. Azure ekibi, Azure modüllerini düzenli olarak güncelleştirir. Bu nedenle, Otomasyon hesaplarınızdaki modülleri güncel tutmak için [Update-AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) runbook 'unu kullanmanız gerekir.

Modüller ürün grubu tarafından düzenli olarak güncelleştirildiğinden, değişiklikler dahil edilen cmdlet 'ler ile gerçekleştirilebilir. Bu değişiklikler örneğin, bir parametreyi yeniden adlandırma veya bir cmdlet 'i kullanımdan kaldırma gibi runbook 'larınızı olumsuz etkileyebilir.

Runbook 'larınızı ve otomatikleştirdikleri işlemleri etkilememek için devam etmeden önce test edin ve doğrulayın. Bu amaçla tasarlanan adanmış bir Otomasyon hesabınız yoksa, runbook 'larınızın geliştirilmesi sırasında birçok farklı senaryoyu test edebilmeniz için bir tane oluşturmayı düşünün. Bu test, PowerShell modüllerini güncelleştirme gibi yinelemeli değişiklikler içermelidir.

Betikleri yerel olarak geliştirirseniz, aynı sonuçları elde etdiğinizden emin olmak için test edilirken Otomasyon hesabınızda aynı modül sürümlerinin yerel olarak olması önerilir. Sonuçlar doğrulandıktan sonra ve gerekli değişiklikleri uyguladıysanız, değişiklikleri üretime taşıyabilirsiniz.

> [!NOTE]
> Yeni bir Otomasyon hesabı en son modülleri içermeyebilir.

> [!NOTE]
> Otomasyon 'un kullanıma hazır olarak sağladığı modüller olan genel modülleri silemezsiniz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıda, Azure modüllerinizi güncelleştirmek için bu makale kullanılırken dikkate alınması gereken bazı noktalar verilmiştir:

* Bu makalede açıklanan runbook, varsayılan olarak Azure, Azurerd ve az modules güncelleştirilmesini destekler. Bu runbook ile az. Automation modüllerini güncelleştirme hakkında daha fazla bilgi için [Azure modüllerini Güncelleştir runbook Benioku dosyasını](https://github.com/microsoft/AzureAutomation-Account-Modules-Update/blob/master/README.md) gözden geçirin. Otomasyon hesabınızda az modüller kullanırken dikkate almanız gereken ek önemli etmenler vardır. Daha fazla bilgi için bkz. [Azure Automation 'da modülleri yönetme](shared-resources/modules.md).

* Bu runbook 'u başlatmadan önce Otomasyon hesabınızda bir [Azure farklı çalıştır hesabı kimlik bilgisi](manage-runas-account.md) oluşturulduğundan emin olun.

* Bu kodu, runbook yerine normal bir PowerShell betiği olarak kullanabilirsiniz: önce yalnızca [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) cmdlet 'Ini kullanarak Azure 'da oturum açın, sonra betiğe geçiş `-Login $false` yapın.

* Bu runbook 'u bağımsız bulutlarında kullanmak için, doğru ortamı `AzEnvironment` runbook 'a geçirmek üzere parametresini kullanın.  Kabul edilebilir değerler Azurecyüksek (Azure genel bulutu), AzureChinaCloud, AzureGermanCloud ve AzureUSGovernment. Bu değerler kullanılarak `Get-AzEnvironment | select Name`alınabilir. Bu cmdlet 'e bir değer geçirmezseniz, runbook varsayılan olarak Azurecum olur.

* PowerShell Galerisi kullanılabilir en son modül yerine belirli bir Azure PowerShell modülü sürümünü kullanmak istiyorsanız, bu sürümleri **Update-AutomationAzureModulesForAccount** runbook `ModuleVersionOverrides` 'unun isteğe bağlı parametresine geçirin. Örnekler için bkz. [Update-AutomationAzureModulesForAccount. ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
) runbook. `ModuleVersionOverrides` Parametresinde bahsedilen Azure PowerShell modüller, PowerShell Galerisi en son modül sürümleriyle güncellenir. `ModuleVersionOverrides` Parametreye hiçbir şey geçirirseniz, tüm modüller PowerShell Galerisi en son modül sürümleriyle güncellenir. Bu davranış, **Azure modüllerini Güncelleştir** düğmesi ile aynıdır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için açık kaynak [güncelleştirme Azure modülleri runbook 'unu](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) ziyaret edin.
