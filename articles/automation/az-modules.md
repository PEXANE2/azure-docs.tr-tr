---
title: Azure Otomasyonu'nda Az modüllerini kullanma
description: Bu makalede, Azure Otomasyonu 'nda az modüller kullanılarak bilgiler sağlanmaktadır
services: automation
ms.subservice: shared-capabilities
ms.date: 02/08/2019
ms.topic: conceptual
ms.openlocfilehash: dfbf54c19aef00cbda886a4531797cda7ef3a191
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76986113"
---
# <a name="az-module-support-in-azure-automation"></a>Azure Otomasyonu 'nda az Module desteği

Azure Otomasyonu, runbook 'larınızda [Azure PowerShell az modülünü](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) kullanma yeteneğini destekler. Az Module, yeni veya mevcut Otomasyon hesaplarında otomatik olarak içeri aktarılmaz. Bu makalede, az modüllerin Azure Otomasyonu ile nasıl kullanılacağı açıklanır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Azure Otomasyonu 'nda az Module kullanırken dikkate alınması gereken pek çok şey vardır. Runbook 'lar ve modüller, Otomasyon hesabınızdaki daha üst düzey çözümler tarafından kullanılabilir. Runbook 'ları veya modülleri yükseltmek, runbook 'larınızla ilgili sorunlara neden olabilir. Yeni `Az` modüllerini içeri aktarmadan önce, tüm runbook 'ları ve çözümleri ayrı bir Otomasyon hesabında dikkatle sınamanız gerekir. Modüllerle ilgili değişiklikler, [Başlat/Durdur](automation-solution-vm-management.md) çözümünü olumsuz etkileyebilir. Herhangi bir çözümü içeren Otomasyon hesaplarında modül ve Runbook 'ların değiştirilmesini önermiyoruz. Bu davranış az modüllere özgü değildir. Otomasyon hesabınızda herhangi bir değişikliğe giriş yaparken bu davranış göz önünde bulundurulmalıdır.

Otomasyon hesabınızda bir `Az` modülünü içeri aktarmak runbook 'ların kullandığı PowerShell oturumunda modülü otomatik olarak içeri aktarmaz. Modüller aşağıdaki durumlarda PowerShell oturumuna aktarılır:

* Bir modülden bir cmdlet bir runbook 'tan çağrıldığında
* Bir runbook, `Import-Module` cmdlet 'i ile açıkça içeri aktardığında
* Modüle bağlı başka bir modül bir PowerShell oturumuna içeri aktarılmışsa

> [!IMPORTANT]
> Bir Otomasyon hesabındaki runbook 'ların yalnızca runbook 'lar tarafından kullanılan PowerShell oturumlarına `Az` veya `AzureRM` modülleri içeri aktardığından emin olmak önemlidir. Bir runbook 'taki `AzureRM` önce `Az` içeri aktarıldıysa runbook tamamlanır, ancak [get_SerializationSettings yöntemine başvuruda bulunan bir hata](troubleshoot/runbooks.md#get-serializationsettings) iş akışlarında gösterilir ve cmdlet 'ler düzgün şekilde yürütülmeyebilir. `AzureRM` içeri aktarırsanız `Az` ve Runbook 'larınız yine de tamamlanacaktır, ancak iş akışlarında her iki `Az` ve `AzureRM` aynı oturumda veya aynı runbook 'ta kullanılan bir hata olduğunu belirten bir hata görürsünüz.

## <a name="migrating-to-az-modules"></a>Az modüllere geçme

Bir test Otomasyonu hesabındaki Azurerd modülleri yerine az modüller kullanarak geçişi test etmeniz önerilir. Otomasyon hesabı oluşturulduktan sonra geçişinizin sorunsuz şekilde çalışmasını sağlamak için aşağıdaki adımları kullanabilirsiniz:

### <a name="stop-and-unschedule-all-runbook-that-uses-azurerm-modules"></a>Azurerd modülleri kullanan tüm runbook 'u durdurma ve zamanlamayı kaldırma

`AzureRM` cmdlet 'leri kullanan var olan runbook 'ları çalıştırtığınızdan emin olmak için `AzureRM` modülleri kullanan tüm runbook 'ları durdurmanız ve zamanlamayı geri almanız gerekir. Hangi zamanlamaların mevcut olduğunu ve hangi zamanlamaların kaldırılması gerektiğini aşağıdaki örnek çalıştırılarak görebilirsiniz:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Gerekirse runbook 'larınız için bu planı ileride yeniden zamanlayamazsınız emin olmak için her bir zamanlamayı ayrı olarak gözden geçirmeniz önemlidir.

### <a name="import-the-az-modules"></a>Az modülleri içeri aktar

Yalnızca runbook 'larınız için gereken az modülleri içeri aktarın. İçeri aktarılacak tüm `Az.*` modülleri içerdiğinden, toplama `Az` modülünü içeri aktarmayın. Bu kılavuz tüm modüller için aynıdır.

[Az. Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) modülü, diğer `Az.*` modülleri için bir bağımlılıkdır. Bu nedenle, diğer modülleri içeri aktarmadan önce Bu modülün Otomasyon hesabınıza aktarılması gerekir.

Otomasyon hesabınızdan, **paylaşılan kaynaklar**altında **modüller** ' i seçin. **Galeriye gözatamıyorum** sayfasını açmak Için **gözatamıyorum** ' ı tıklatın.  Arama çubuğuna modül adını girin (örneğin, `Az.Accounts`). PowerShell modülü sayfasında, modülü Otomasyon hesabınıza aktarmak için **Içeri aktar** ' a tıklayın.

![Otomasyon hesabından modülleri içeri aktar](media/az-modules/import-module.png)

Bu içeri aktarma işlemi, modül aranarak [PowerShell Galerisi](https://www.powershellgallery.com) aracılığıyla da yapılabilir. Modülü bulduktan sonra seçin ve **Azure Otomasyonu** sekmesinde **Azure Otomasyonu 'na dağıt**' a tıklayın.

![Modülleri doğrudan Galeriden içeri aktar](media/az-modules/import-gallery.png)

## <a name="test-your-runbooks"></a>Runbook 'larınızı test edin

`Az` modülleri Otomasyon hesabınıza alındıktan sonra, artık runbook 'larınızı az modülünü kullanacak şekilde düzenleyebilirsiniz. Cmdlet 'lerinin çoğunluğu `AzureRM` dışında aynı ada sahiptir `Az`olarak değiştirilmiştir. Bu işlemi takip eden modüllerin bir listesi için bkz. [özel durumlar listesi](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters).

Runbook 'unuzu yeni cmdlet 'leri kullanacak şekilde değiştirmeden önce runbook 'larınızı test etmenin bir yolu, bir runbook 'un başlangıcında `Enable-AzureRMAlias -Scope Process` kullanmaktır. Bunu runbook 'a ekleyerek, runbook 'niz değişiklik yapılmadan çalıştırılabilir.

## <a name="after-migration-details"></a>Geçiş ayrıntılarından sonra

Geçiş işlemi tamamlandıktan sonra, hesapta daha fazla `AzureRM` modül kullanarak runbook 'ları başlatmayın. Bu hesapta `AzureRM` modülleri içeri veya güncelleştirme de önerilir. Bu andan itibaren, bu hesabı `Az`' a geçirilmiş ve yalnızca `Az` modüllerle çalışacak şekilde düşünün. Yeni bir Otomasyon hesabı oluşturulduğunda mevcut `AzureRM` modülleri yine de yüklenecektir ve öğretici runbook 'lar yine de `AzureRM` cmdlet 'leriyle yazılır. Bu runbook 'lar çalıştırılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Az modüller kullanma hakkında daha fazla bilgi edinmek için bkz. [az Module ile çalışmaya](/powershell/azure/get-started-azureps?view=azps-1.1.0)başlama.
