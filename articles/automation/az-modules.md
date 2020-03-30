---
title: Azure Otomasyonu'nda Az modüllerini kullanma
description: Bu makale, Azure Otomasyonunda Az modüllerini kullanarak bilgi sağlar
services: automation
ms.subservice: shared-capabilities
ms.date: 02/08/2019
ms.topic: conceptual
ms.openlocfilehash: dfbf54c19aef00cbda886a4531797cda7ef3a191
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76986113"
---
# <a name="az-module-support-in-azure-automation"></a>Azure Otomasyonu'nda Az modülleri desteği

Azure otomasyonu, runbook'larınızda [Azure Powershell Az modüllerini](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) kullanma olanağını destekler. Az modülü yeni veya mevcut Otomasyon Hesaplarında otomatik olarak alınmaz. Bu makalede, Azure Otomasyonu ile Az modüllerinin nasıl kullanılacağı açıklanmaktadır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure Otomasyonu'nda Az modüllerini kullanırken göz önünde bulundurulması gereken birçok şey vardır. Runbook'lar ve modüller Otomasyon Hesabınızdaki üst düzey çözümler tarafından kullanılabilir. Çalışma defterlerinin düzenlenmesi veya modüllerin yükseltilmesi, runbook'larınızla ilgili sorunlara neden olabilir. Yeni `Az` modülleri almadan önce tüm runbook'ları ve çözümleri ayrı bir Otomasyon Hesabında dikkatle test etmelisiniz. Modüllerde yapılan herhangi bir değişiklik [Başlat/Durdur](automation-solution-vm-management.md) çözümlerini olumsuz etkileyebilir. Otomasyon Hesapları'nda herhangi bir çözüm içeren modülleri ve runbook'ları değiştirmenizi önermiyoruz. Bu davranış Az modüllerine özgü değildir. Bu davranış, Otomasyon Hesabınızda herhangi bir değişiklik tanıtırken göz önünde bulundurulmalıdır.

Otomasyon Hesabınızda bir `Az` modül almak, runbook'ların kullandığı PowerShell oturumundamodülü otomatik olarak almaz. Modüller aşağıdaki durumlarda PowerShell oturumuna aktarılır:

* Bir modülden bir cmdlet çalıştırıldığında
* Bir runbook `Import-Module` cmdlet ile açıkça içe aktıldığında
* Modüle bağlı olarak başka bir modül PowerShell oturumuna alındığında

> [!IMPORTANT]
> Bir Otomasyon Hesabı'ndaki runbook'ların her ikisi `Az` `AzureRM` tarafından kullanılan PowerShell oturumlarına yalnızca içe aktardığından veya modüllerden aktardığından emin olmak önemlidir. Daha `Az` önce `AzureRM` bir runbook'ta içe aktarılırsa, runbook tamamlanır, ancak [get_SerializationSettings yöntemine atıfta bulunan](troubleshoot/runbooks.md#get-serializationsettings) bir hata iş akışlarında gösterir ve cmdlets düzgün yürütülmemiş olabilir. İçe `AzureRM` aktarırsanız ve `Az` runbook'unuzun tamamlanması na rağmen, iş akışlarında `Az` `AzureRM` aynı oturumda hem içe aktarılamayacağını hem de aynı runbook'ta kullanılamayacağını belirten bir hata görürsünüz.

## <a name="migrating-to-az-modules"></a>Az modüllerine geçiş

Bir test OtomasyonU Hesabında AzureRM modülleri yerine Az modüllerini kullanma geçişini test etmeniz önerilir. Otomasyon Hesabı oluşturulduktan sonra, geçişinizin sorunsuz geçmesini sağlamak için aşağıdaki adımları kullanabilirsiniz:

### <a name="stop-and-unschedule-all-runbook-that-uses-azurerm-modules"></a>AzureRM modüllerini kullanan tüm runbook'ları durdurup zamanlamadan uzakdur

Cmdlets kullanan `AzureRM` mevcut runbook'ları çalıştırmadığınızdan emin olmak için, modülleri kullanan `AzureRM` tüm runbook'ları durdurmalı ve zamanlamadan vazgeçmelisiniz. Aşağıdaki örneği çalıştırarak hangi zamanlamaların var olduğunu ve hangi zamanlamaların kaldırılması gerektiğini görebilirsiniz:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Gerekirse runbook'larınız için gelecekte yeniden zamanlayabileceğinizden emin olmak için her zamanlamayı ayrı ayrı gözden geçirmeniz önemlidir.

### <a name="import-the-az-modules"></a>Az modüllerini alma

Yalnızca runbook'larınız için gerekli olan Az modüllerini içeri aktarın. İthal edilecek tüm `Az` `Az.*` modülleri içerdiğinden, toplama modüllerini içe aktarmayın. Bu kılavuz tüm modüller için aynıdır.

[Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) modülü diğer `Az.*` modüller için bir bağımlılıktır. Bu nedenle, diğer modülleri içe aktarmadan önce bu modülün Otomasyon Hesabınıza aktarılması gerekir.

Otomasyon **Hesabınızdan, Paylaşılan Kaynaklar**altında **Modülleri** seçin. **Galeriye Gözat** sayfasını açmak için **Galeriye Gözat'ı** tıklatın.  Arama çubuğuna modül adını girin (örneğin). `Az.Accounts` PowerShell Modülü sayfasında, modülü Otomasyon Hesabınıza almak için **İçe Aktar'ı** tıklatın.

![Otomasyon Hesabından İthalat Modülleri](media/az-modules/import-module.png)

Bu alma işlemi, [powershell galerisi](https://www.powershellgallery.com) üzerinden modül arayarak da yapılabilir. Modülü bulduktan sonra, modülü seçin ve **Azure Otomasyonu** sekmesi altında, **Azure Otomasyonuna Dağıt'ı**tıklatın.

![Modülleri doğrudan galeriden alma](media/az-modules/import-gallery.png)

## <a name="test-your-runbooks"></a>Runbook'larınızı test edin

`Az` Modüller Otomasyon Hesabınıza alındıktan sonra, artık runbook'larınızı az modüllerini kullanmak üzere düzenlemeye başlayabilirsiniz. Cmdlets çoğunluğu dışında `AzureRM` aynı adı vardır . `Az` Bu işlemi izlemeyen modüllerin listesi için [özel durumlar listesine](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters)bakın.

Runbook'unuzu yeni cmdlets'i kullanmak için değiştirmeden önce test `Enable-AzureRMAlias -Scope Process` etmenin bir yolu, bir runbook'un başında kullanmaktır. Bunu runbook'unuza ekleyerek, runbook'unuzun çalışma kitabı değişiklik yapmadan çalıştırılabilir.

## <a name="after-migration-details"></a>Geçiş ten sonra ayrıntılar

Geçiş tamamlandıktan sonra, hesaptaki modülleri `AzureRM` kullanarak runbook'ları başlatmayın. Ayrıca, bu hesaptaki modülleri `AzureRM` içe aktarmamanız veya güncellememeleri de önerilir. Şu andan itibaren, bu hesabın `Az`yalnızca `Az` modüllerle geçiş ini ve çalışmasını göz önünde bulundurun. Yeni bir Otomasyon Hesabı oluşturulduğunda mevcut `AzureRM` modüller hala yüklenir ve öğretici runbook'lar cmdlets ile `AzureRM` yazılacaktır. Bu runbook'lar çalıştırılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Az modüllerini kullanma hakkında daha fazla bilgi edinmek için Az [modülüne başlarken](/powershell/azure/get-started-azureps?view=azps-1.1.0)bkz.
