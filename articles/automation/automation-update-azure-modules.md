---
title: Azure Otomasyonu'nda Azure modüllerini güncelleştirin
description: Bu makalede, Azure Otomasyonu'nda varsayılan olarak sağlanan ortak Azure PowerShell modüllerini nasıl güncelleştirebileceğiniz açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 06/14/2019
ms.topic: conceptual
ms.openlocfilehash: 721bb3579fec10df88be471d67f68c0846aa9432
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632120"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Azure Otomasyonu'nda Azure PowerShell modüllerini güncelleştirme

Otomasyon Hesabınızdaki Azure modüllerini güncellemek için açık kaynak olarak kullanılabilen [Azure modüllerini güncelleştir](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update)ini kullanmanız gerekir. Azure modüllerinizi güncellemek için **Update-AutomationAzureModulesForAccount** runbook'u kullanmaya başlamak için, GitHub'daki [Azure modüllerini güncelleştir'den](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) indirin. Daha sonra Otomasyon Hesabınıza aktarabilir veya komut dosyası olarak çalıştırabilirsiniz. Otomasyon Hesabınızda runbook'u nasıl içe aktarılaceksiniz öğrenmek için [bkz.](manage-runbooks.md#importing-a-runbook)

En yaygın AzureRM PowerShell modülleri varsayılan olarak her Otomasyon hesabında sağlanır. Azure ekibi Azure modüllerini düzenli olarak güncelleştirir, bu nedenle güncel tutmak için Otomasyon hesaplarınızdaki modülleri güncellemek için [Update-AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) runbook'u kullanmak isteyeceksiniz.

Modüller ürün grubu tarafından düzenli olarak güncelleştirilenden, dahil edilen cmdlets ile değişiklikler oluşabilir. Bu eylem, parametreyi yeniden adlandırma veya bir cmdlet'i tamamen küçümseme gibi değişiklik türüne bağlı olarak runbook'larınızı olumsuz etkileyebilir.

Runbook'larınızı ve otomatikleştirdikleri işlemleri etkilemeyi önlemek için, devam etmeden önce test edin ve doğrulayın. Bu amaca yönelik özel bir Otomasyon hesabınız yoksa, runbook'larınızın geliştirilmesi sırasında birçok farklı senaryoyu test edebilmeniz için bir hesap oluşturmayı düşünün. Bu test, PowerShell modüllerini güncelleme küçücük değişiklikler içermelidir.

Komut dosyalarınızı yerel olarak geliştirirseniz, aynı sonuçları aldığınızdan emin olmak için test yaparken Otomasyon Hesabınızda bulunan modül sürümlerinin yerel olarak kullanılması önerilir. Sonuçlar doğrulandıktan ve gerekli değişiklikleri uyguladıktan sonra, değişiklikleri üretime taşıyabilirsiniz.

> [!NOTE]
> Yeni bir Otomasyon hesabı en son modülleri içermeyebilir.

> [!NOTE]
> Otomasyon'un kutudan sağladığı global modülleri silemezsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure Modüllerinizi güncelleştirmek için bu işlemi kullanırken göz önünde bulundurulması gereken bazı hususlar şunlardır:

* Bu runbook varsayılan olarak **Azure** ve **AzureRm** modüllerinin güncelleştirilmesini destekler. Bu runbook, **Az** modüllerinin güncellenmesini de destekler. Bu [runbook](https://github.com/microsoft/AzureAutomation-Account-Modules-Update/blob/master/README.md) ile modülleri güncelleme `Az` hakkında daha fazla bilgi için Azure modüllerini güncelleştir ini gözden geçirin README. Otomasyon Hesabınızdaki `Az` modülleri kullanırken göz önünde bulundurulması gereken diğer önemli faktörler de var, daha fazla bilgi edinmek için, Bkz. Otomasyon [Hesabınızdaki Az modüllerini kullanma](az-modules.md).

* Bu runbook'u başlatmadan önce, Otomasyon hesabınızın bir [Azure Run As hesap kimlik bilgisi](manage-runas-account.md) oluşturduğundan emin olun.

* Bu kodu runbook yerine normal bir PowerShell komut dosyası olarak kullanabilirsiniz: önce [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) `-Login $false` komutunu kullanarak Azure'da oturum açın, ardından komut dosyasına geçebilirsiniz.

* Egemen bulutlar üzerinde bu runbook kullanmak `AzureRmEnvironment` için, runbook doğru ortamı geçmek için parametre kullanın.  Kabul edilebilir değerler **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**ve **AzureUSGovernment'tır.** Bu değerler kullanarak `Get-AzureRmEnvironment | select Name`alınabilir. Bu parametreye bir değer aktarmazsanız, runbook varsayılan olarak Azure genel **bulutu AzureCloud**

* PowerShell Galerisi'nde mevcut olan en son sürümyerine belirli bir Azure PowerShell modülü `ModuleVersionOverrides` sürümünü kullanmak istiyorsanız, bu sürümleri **Update-AutomationAzureModulesForAccount** runbook'unun isteğe bağlı parametresine aktarın. Örneğin, [Update-AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
) runbook'a bakın. `ModuleVersionOverrides` Parametrede belirtilmeyen Azure PowerShell modülleri PowerShell Galerisi'ndeki en son modül sürümleriyle güncelleştirilir. `ModuleVersionOverrides` Parametreye hiçbir şey aktarmazsanız, tüm modüller PowerShell Galerisi'ndeki en son modül sürümleriyle güncellenir. Bu davranış, Azure **Modüllerini Güncelleştir** düğmesiyle aynıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu konuda daha fazla bilgi edinmek için açık kaynak takiazure [modüllerini güncelleştir](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) ini ziyaret edin.
