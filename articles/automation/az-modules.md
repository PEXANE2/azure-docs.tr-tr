---
title: Azure Otomasyonu'nda Az modülleri desteği
description: Bu makalede, Azure Otomasyonu'nda Az modüllerinin kullanımı hakkında bilgi verilmektedir.
services: automation
ms.subservice: shared-capabilities
ms.date: 02/08/2019
ms.topic: conceptual
ms.openlocfilehash: 776834937d81a3ba84e3c1db56496a7d951d7982
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548353"
---
# <a name="az-module-support-in-azure-automation"></a>Azure Otomasyonu'nda Az modülleri desteği

Azure Otomasyonu, runbook'larınızda [Azure PowerShell Az modülü](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) kullanımını destekler. Az modülü yeni veya mevcut Otomasyon hesaplarında otomatik olarak alınmaz. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure Otomasyonu'nda Az modüllerini kullanırken göz önünde bulundurulması gereken birçok şey vardır. Runbook'lar ve modüller Otomasyon hesabınızdaki üst düzey çözümler tarafından kullanılabilir. Çalışma defterlerinin düzenlenmesi veya modüllerin yükseltilmesi, runbook'larınızla ilgili sorunlara neden olabilir. Yeni `Az` modülleri almadan önce tüm runbook'ları ve çözümleri ayrı bir Otomasyon hesabında dikkatle test etmelisiniz. Modüllerde yapılan herhangi bir değişiklik [Başlat/Durdur](automation-solution-vm-management.md) çözümlerini olumsuz etkileyebilir. Otomasyon hesaplarında herhangi bir çözüm içeren modülleri ve runbook'ları değiştirmenizi önermiyoruz. Bu davranış Az modüllerine özgü değildir. Bu davranış, Otomasyon hesabınızda herhangi bir değişiklik tanıtırken göz önünde bulundurulmalıdır.

Otomasyon hesabınızda bir `Az` modül almak, runbook'ların kullandığı PowerShell oturumundamodülü otomatik olarak almaz. Modüller aşağıdaki durumlarda PowerShell oturumuna aktarılır:

* Bir modülden bir cmdlet çalıştırıldığında
* Bir runbook `Import-Module` cmdlet ile açıkça içe aktıldığında
* Modüle bağlı olarak başka bir modül PowerShell oturumuna alındığında

> [!IMPORTANT]
> Bir Otomasyon hesabındaki runbook'ların her ikisi `Az` tarafından `AzureRM` kullanılan PowerShell oturumlarına yalnızca içe aktardığından veya modüller aldığından emin olmak önemlidir. Daha `Az` önce `AzureRM` bir runbook'ta içe aktarılırsa, runbook tamamlanır, ancak [Get_SerializationSettings](troubleshoot/runbooks.md#get-serializationsettings) cmdlet'e atıfta bulunan bir hata iş akışlarında belirebilir ve cmdlets düzgün yürütülmeyebilir. İçe `AzureRM` aktarırsanız ve `Az`sonra runbook'unuzun tamamlanması na rağmen, iş `Az` akışlarında hem aynı oturumda içe aktarılabilen hem `AzureRM` de aynı runbook'ta kullanılamayan bir hata alırsınız.

## <a name="migrating-to-az-modules"></a>Az modüllerine geçiş

Bir test Otomasyonu hesabında Az modüllerine geçişi test etmeniz önerilir. Otomasyon hesabı oluşturulduktan sonra, modüllerle çalışmak için bu bölümdeki yönergeleri kullanabilirsiniz.

### <a name="stop-and-unschedule-all-runbooks-that-use-azurerm-cmdlets"></a>AzureRM cmdlets kullanan tüm runbook'ları durdurup zamanlamadan uzakdur

Cmdlets kullanan `AzureRM` mevcut runbook'ları çalıştırmadığınızdan emin olmak için, modülleri kullanan `AzureRM` tüm runbook'ları durdurmalı ve zamanlamadan vazgeçmelisiniz. Hangi zamanlamaların var olduğunu ve bu örneğe benzer kod çalıştırarak hangi zamanlamaların kaldırılması gerektiğini görebilirsiniz.

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Gerekirse runbook'larınız için gelecekte yeniden zamanlayabileceğinizden emin olmak için her zamanlamayı ayrı ayrı gözden geçirmeniz önemlidir.

### <a name="import-the-az-modules"></a>Az modüllerini alma

Yalnızca runbook'larınız için gerekli olan Az modüllerini içeri aktarın. Tüm `Az.*` modülleri içerdiği `Az` nden toplama modüllerini içe aktarmayın. Bu kılavuz tüm modüller için aynıdır.

[Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) modülü diğer `Az.*` modüller için bir bağımlılıktır. Bu nedenle, diğer modülleri içe aktarmadan önce bu modülün Otomasyon hesabınıza aktarılması gerekir.

Otomasyon **hesabınızdan, Paylaşılan Kaynaklar**altında **Modülleri** seçin. **Galeriye Gözat** sayfasını açmak için **Galeriye Gözat'ı** tıklatın.  Arama çubuğuna modül adını girin (örneğin). `Az.Accounts` PowerShell Modülü sayfasında, modülü Otomasyon hesabınıza almak için **İçe Aktar'ı** tıklatın.

![Otomasyon hesabından modülleri alma](media/az-modules/import-module.png)

Bu alma işlemi, satın alma modülü aranarak [PowerShell Galerisi](https://www.powershellgallery.com) üzerinden de yapılabilir. Modülü bulduktan sonra, modülü seçin ve **Azure Otomasyonu** sekmesi altında, **Azure Otomasyonuna Dağıt'ı**tıklatın.

![Modülleri doğrudan galeriden alma](media/az-modules/import-gallery.png)

## <a name="testing-your-runbooks"></a>Runbook'larınızı test etme

`Az` Modüller Otomasyon hesabınıza aktarıladıktan sonra, Az modüllerini kullanmak için runbook'larınızı düzenlemeye başlayabilirsiniz. Cmdlets çoğunluğu aynı adlara sahip `AzureRM` dışında değiştirildi `Az`. Bu adlandırma kuralını izlemeyen modüllerin listesi için [özel durumlar listesine](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters)bakın.

Yeni cmdlets kullanmak için bir runbook modifikasyonu test `Enable-AzureRMAlias -Scope Process` etmek için bir yolu runbook başında kullanarak gereğidir. Bu komutu runbook'unuza ekleyerek komut dosyası değişiklik yapmadan çalıştırılabilir.

## <a name="after-migration-details"></a>Geçiş sonrası ayrıntılar

Geçiş tamamlandıktan sonra, Artık Otomasyon hesabındaki `AzureRM` modülleri kullanarak runbook'ları başlatmaya çalışmayın. Ayrıca, hesaptaki modülleri içe `AzureRM` aktarmamanız veya güncelleştirmemeleri de önerilir. Yalnızca `Az` `Az` aktarılan hesabı düşünün ve yalnızca modüllerle çalıştırın. 

Yeni bir Otomasyon hesabı oluşturulduğunda, varolan `AzureRM` modüller hala yüklenir. Öğretici runbook'ları cmdlets ile `AzureRM` güncellemeye devam edebilirsiniz. Bu runbook'ları çalıştırmamalısın.

## <a name="next-steps"></a>Sonraki adımlar

Az modüllerini kullanma hakkında daha fazla bilgi edinmek için Az [modülüne başlarken](/powershell/azure/get-started-azureps?view=azps-1.1.0)bkz.
