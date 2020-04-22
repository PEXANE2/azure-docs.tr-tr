---
title: Azure Otomasyonu'nda Azure PowerShell modüllerini güncelleştirin
description: Bu makalede, Azure Otomasyonu'nda varsayılan olarak sağlanan ortak Azure PowerShell modüllerini nasıl güncelleştirebileceğiniz açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 06/14/2019
ms.topic: conceptual
ms.openlocfilehash: bb73b3d644e96f9596f887faaf62eb15f01956ab
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769663"
---
# <a name="update-azure-powershell-modules-in-azure-automation"></a>Azure Otomasyonu'nda Azure PowerShell modüllerini güncelleştirin

Otomasyon hesabınızdaki Azure modüllerini güncellemek için açık kaynak olarak kullanılabilen [Azure modüllerini güncelleştir](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update)ini kullanmanız gerekir. Azure modüllerinizi güncellemek için **Update-AutomationAzureModulesForAccount** runbook'u kullanmaya başlamak için, GitHub'daki [Azure modüllerini güncelleştir'den](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) indirin. Daha sonra Otomasyon hesabınıza aktarabilir veya komut dosyası olarak çalıştırabilirsiniz. Otomasyon hesabınızda bir runbook'u nasıl içe aktarılaceksiniz öğrenmek için [bkz.](manage-runbooks.md#importing-a-runbook)

En yaygın PowerShell modülleri varsayılan olarak her Otomasyon hesabında sağlanır. Azure ekibi Azure modüllerini düzenli olarak güncelleştirir. Bu nedenle, Otomasyon hesaplarınızdaki modülleri güncel tutmak için [Update-AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) runbook'u kullanmanız gerekir.

Modüller ürün grubu tarafından düzenli olarak güncelleştirilenden, dahil edilen cmdlets ile değişiklikler oluşabilir. Bu değişiklikler, örneğin, bir parametreyi yeniden adlandırmaveya bir cmdlet'i tamamen küçümseme, runbook'larınızı olumsuz etkileyebilir.

Runbook'larınızı ve otomatikleştirdikleri işlemleri etkilemeyi önlemek için, devam etmeden önce test edin ve doğrulayın. Bu amaca yönelik özel bir Otomasyon hesabınız yoksa, runbook'larınızın geliştirilmesi sırasında birçok farklı senaryoyu test edebilmeniz için bir hesap oluşturmayı düşünün. Bu sınama, PowerShell modüllerini güncellemek gibi yinelemeli değişiklikleri içermelidir.

Komut dosyalarınızı yerel olarak geliştirirseniz, aynı sonuçları aldığınızdan emin olmak için test yaparken Otomasyon hesabınızda bulunan modül sürümlerinin yerel olarak kullanılması önerilir. Sonuçlar doğrulandıktan ve gerekli değişiklikleri uyguladıktan sonra, değişiklikleri üretime taşıyabilirsiniz.

> [!NOTE]
> Yeni bir Otomasyon hesabı en son modülleri içermeyebilir.

> [!NOTE]
> Otomasyon'un kutudan sağladığı modüller olan küresel modülleri silemezsiniz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure Modüllerinizi güncelleştirmek için bu makaleyi kullanırken göz önünde bulundurulması gereken bazı hususlar şunlardır:

* Bu makalede açıklanan runbook varsayılan olarak Azure, AzureRM ve Az modüllerinin güncelleştirilmesini destekler. Az.Automation modüllerini bu runbook ile güncelleme hakkında daha fazla bilgi için [Azure modüllerini güncelleştir](https://github.com/microsoft/AzureAutomation-Account-Modules-Update/blob/master/README.md) ini gözden geçirin. Otomasyon hesabınızdaki Az modüllerini kullanırken göz önünde bulundurulması gereken önemli faktörler de vardır. Daha fazla bilgi için Azure [Otomasyonu'nda Modülleri Yönet'e](shared-resources/modules.md)bakın.

* Bu runbook'u başlatmadan önce, Otomasyon hesabınızın bir [Azure Run As hesap kimlik bilgisi](manage-runas-account.md) oluşturduğundan emin olun.

* Bu kodu runbook yerine normal bir PowerShell komut dosyası olarak kullanabilirsiniz: önce [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) cmdlet'i kullanarak Azure'da oturum açın, ardından komut dosyasına geçin. `-Login $false`

* Egemen bulutlar üzerinde bu runbook kullanmak `AzEnvironment` için, runbook doğru ortamı geçmek için parametre kullanın.  Kabul edilebilir değerler AzureCloud (Azure genel bulutu), AzureChinaCloud, AzureGermanCloud ve AzureUSGovernment'tır. Bu değerler kullanılarak `Get-AzEnvironment | select Name`alınabilir. Bu cmdlet'e bir değer aktarmazsanız, runbook varsayılan olarak AzureCloud'a geçer.

* PowerShell Galerisi'nde bulunan en son modül yerine belirli bir Azure PowerShell modülü sürümünü `ModuleVersionOverrides` kullanmak istiyorsanız, bu sürümleri **Update-AutomationAzureModulesForAccount** runbook'un isteğe bağlı parametresine aktarın. Örneğin, [Update-AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
) runbook'a bakın. `ModuleVersionOverrides` Parametrede belirtilmeyen Azure PowerShell modülleri PowerShell Galerisi'ndeki en son modül sürümleriyle güncelleştirilir. `ModuleVersionOverrides` Parametreye hiçbir şey aktarmazsanız, tüm modüller PowerShell Galerisi'ndeki en son modül sürümleriyle güncellenir. Bu davranış, Azure **Modüllerini Güncelleştir** düğmesiyle aynıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu konuda daha fazla bilgi edinmek için açık kaynak takiazure [modüllerini güncelleştir](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) ini ziyaret edin.
