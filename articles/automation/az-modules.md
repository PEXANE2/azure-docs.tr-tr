---
title: Azure Otomasyonu'nda Az modülleri desteği
description: Bu makalede, Azure Otomasyonu'nda Az modüllerinin kullanımı hakkında bilgi verilmektedir.
services: automation
ms.subservice: shared-capabilities
ms.date: 02/08/2019
ms.topic: conceptual
ms.openlocfilehash: 21fa1c4faa4a080b9b495e1481fdadcd7e8bea10
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619472"
---
# <a name="az-module-support-in-azure-automation"></a>Azure Otomasyonu'nda Az modülleri desteği

Azure Otomasyonu, runbook'larınızda [Azure PowerShell Az modülü](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) kullanımını destekler. Rollup Az modülü yeni veya mevcut Otomasyon hesaplarında otomatik olarak alınmaz. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure Otomasyonu'nda rollup Az modüllerini kullanırken göz önünde bulundurulması gereken birçok şey vardır. Runbook'lar ve modüller Otomasyon hesabınızdaki üst düzey çözümler tarafından kullanılabilir. Çalışma defterlerinin düzenlenmesi veya modüllerin yükseltilmesi, runbook'larınızla ilgili sorunlara neden olabilir. Yeni Az modüllerini almadan önce tüm runbook'ları ve çözümleri ayrı bir Otomasyon hesabında dikkatle test etmelisiniz. Modüllerde yapılan herhangi bir değişiklik [Başlat/Durdur](automation-solution-vm-management.md) çözümlerini olumsuz etkileyebilir. Otomasyon hesaplarında herhangi bir çözüm içeren modülleri ve runbook'ları değiştirmenizi önermiyoruz. Bu davranış Az modüllerine özgü değildir. Otomasyon hesabınızda herhangi bir değişiklik tanıtırken dikkate alınmalıdır.

Otomasyon hesabınıza bir Az modülü almak, runbook'ların kullandığı PowerShell oturumundaki modülü otomatik olarak almaz. Modüller aşağıdaki durumlarda PowerShell oturumuna aktarılır:

* Runbook bir modülden cmdlet çağırdığında
* Bir runbook `Import-Module` modülü cmdlet ile açıkça içe aktattığında
* Runbook modüle bağlı olarak başka bir modül aldığında

> [!IMPORTANT]
> Otomasyon hesabındaki runbook'ların Az modüllerini veya [AzureRM](https://www.powershellgallery.com/packages/AzureRM/6.13.1) modüllerini ancak her ikisini de almadığından emin olun. Bir runbook AzureRM modüllerinden önce Az modülleri alırsa, runbook tamamlar. Ancak, [Get_SerializationSettings](troubleshoot/runbooks.md#get-serializationsettings) cmdlet atıfta bir hata iş akışları gösterir ve cmdlets düzgün yürütülmeyebilir. Runbook Az modüllerinden önce AzureRM modüllerini içe alırsa, runbook da tamamlar. Ancak bu durumda, hem Az hem de AzureRM'in aynı oturumda içe aktarılamilere veya aynı runbook'ta kullanılayamadıklarını belirten iş akışlarında bir hata alırsınız.

## <a name="migrating-to-az-modules"></a>Az modüllerine geçiş

Bir test Otomasyonu hesabında Az modüllerine geçişi test etmeniz önerilir. Bu hesabı oluşturduktan sonra, modüllerle çalışmak için bu bölümdeki yönergeleri kullanabilirsiniz.

### <a name="stop-and-unschedule-all-runbooks-that-use-azurerm-modules"></a>AzureRM modüllerini kullanan tüm runbook'ları durdurup zamanlamadan önce

AzureRM modüllerini kullanan varolan runbook'ları çalıştırmadığınızdan emin olmak için, etkilenen tüm runbook'ları durdurun ve zamanlayın. Bu örneğe benzer kod çalıştırarak hangi zamanlamaların var olduğunu ve hangi zamanlamaların kaldırılacak larını görebilirsiniz:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Gerekirse, gelecekte runbook'larınız için yeniden zamanlayabildiğinizden emin olmak için her zamanlamayı ayrı ayrı gözden geçirmeniz önemlidir.

### <a name="import-the-az-modules"></a>Az modüllerini alma

>[!NOTE]
>Runbook'larınızın yalnızca gerekli Az modüllerini içe aktarsın. Tüm Az modüllerini içerdiğinden, rollup Az modüllerini ithal etmeyin. Bu kılavuz tüm modüller için aynıdır.

[Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) modülü diğer Az modülleri için bir bağımlılıktır. Bu nedenle, runbook'larınızın diğer modülleri içeri aktarmadan önce bu modülü Otomasyon hesabınıza aktarması gerekir.

Azure portalındaki modülleri almak için:

1. Otomasyon **hesabınızdan, Paylaşılan Kaynaklar**altında **Modülleri** seçin. 
2. Galeriye Gözat sayfasını açmak için **Galeriye Gözat'ı** tıklatın.  
3. Arama çubuğuna, modül adını girin, `Az.Accounts`örneğin, . 
4. PowerShell Modülü sayfasında, modülü Otomasyon hesabınıza almak için **İçe Aktar'ı** tıklatın.

![Otomasyon hesabından modülleri alma](media/az-modules/import-module.png)

Bu alma işlemi, satın alma modülü aranarak [PowerShell Galerisi](https://www.powershellgallery.com) üzerinden de yapılabilir. Modülü bulduktan sonra, modülü seçin, **Azure Otomasyon** sekmesini seçin ve **Azure Otomasyonuna Dağıt'ı**tıklatın.

![Modülleri doğrudan galeriden alma](media/az-modules/import-gallery.png)

## <a name="testing-your-runbooks"></a>Runbook'larınızı test etme

Az modüllerini Otomasyon hesabına aktardıktan sonra, modülleri kullanmak için runbook'larınızı düzenlemeye başlayabilirsiniz. AzureRM (veya AzureRm) önekinin Az olarak değiştirilmesi dışında cmdlets'lerin çoğu AzureRM modülüyle aynı adlara sahiptir. Bu adlandırma kuralını izlemeyen modüllerin listesi için [özel durumlar listesine](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters)bakın.

Yeni cmdlets kullanmak için bir runbook modifikasyonu test `Enable-AzureRmAlias -Scope Process` etmek için bir yolu runbook başında kullanarak gereğidir. Bu komutu runbook'unuza ekleyerek komut dosyası değişiklik yapmadan çalıştırılabilir.

## <a name="after-migration-details"></a>Geçiş sonrası ayrıntılar

Geçiş tamamlandıktan sonra, Otomasyon hesabında AzureRM modüllerini kullanarak runbook'ları başlatmayı denemeyin. AzureRM modüllerini hesapta almamanız veya güncelleştirmemenizi de önerilir. Az'a geçirilen hesabı düşünün ve yalnızca Az modülleriyle çalıştırın. 

Yeni bir Otomasyon hesabı oluşturduğunuzda, varolan AzureRM modülleri hala yüklenir. Öğretici runbook'ları AzureRM cmdlets ile güncellemeye devam edebilirsiniz. Ancak, bu runbook'ları çalıştırmamalısınız.

## <a name="next-steps"></a>Sonraki adımlar

Az modüllerini kullanma hakkında daha fazla bilgi edinmek için Az [modülüne başlarken](/powershell/azure/get-started-azureps?view=azps-1.1.0)bkz.
