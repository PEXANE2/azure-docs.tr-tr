---
title: Azure Otomasyonu grafik runbook’larında hata işleme
description: Bu makalede, Azure Otomasyonu grafik runbook’larında hata işleme mantığının nasıl uygulanacağı açıklanmıştır.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: f1aa605b3e6f32b260ea4a9eee9c056277fcd12d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367083"
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Azure Otomasyonu grafik runbook’larında hata işleme

Azure Otomasyon grafik çalışma kitabınızda göz önünde bulundurulması gereken önemli bir tasarım ilkesi, runbook'un yürütme sırasında karşılaşabileceği sorunların tanımlanmasıdır. Bu sorunlar arasında başarı, beklenen hata durumları ve beklenmeyen hata koşulları sayılabilir.

Genellikle, bir runbook etkinliğiyle oluşan sonlandırıcı olmayan bir hata varsa, Windows PowerShell hatadan bağımsız olarak aşağıdaki herhangi bir etkinliği işleyerek etkinliği işler. Hata büyük olasılıkla bir özel durum oluşturur, ancak sonraki etkinliğin çalışmasına yine de izin verilir.

Grafik çalışma kitabınız yürütme sorunlarıyla başa çıkmak için hata işleme kodunu içermelidir. Bir etkinliğin çıktısını doğrulamak veya bir hatayı işlemek için PowerShell kod etkinliği kullanabilir, etkinliğin çıktı bağlantısında koşullu mantık tanımlayabilir veya başka bir yöntem uygulayabilirsiniz.

Azure Otomasyonu grafiksel runbook’ları, hata işleme özelliğiyle geliştirilmiştir. Bundan böyle özel durumları sonlandırıcı olmayan hatalara dönüştürebilir ve etkinlikler arasında hata bağlantıları oluşturabilirsiniz. Geliştirilmiş işlem, runbook'unuzun hataları yakalamasına ve gerçekleşen veya beklenmeyen koşulları yönetmesine olanak tanır. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="powershell-error-types"></a>PowerShell hata türleri

Runbook yürütme sırasında oluşabilecek PowerShell hataları türleri sonlandırıcı hatalar ve sonlandırıcı olmayan hatalardır.
 
### <a name="terminating-error"></a>Sonlandırma hatası

Sonlandırma hatası, yürütme sırasında komutu veya komut dosyası yürütmeyi tamamen durduran ciddi bir hatadır. Örnekler arasında var olmayan cmdletler, bir cmdlet'in çalışmasını engelleyen sözdizimi hataları ve diğer önemli hatalar sayılabilir.

### <a name="non-terminating-error"></a>Sonlandırma hatası olmayan

Sonlandırmayan bir hata, hata durumuna rağmen yürütmenin devam etmesine izin veren ciddi olmayan bir hatadır. Örnek olarak, dosya da bulunamadı hataları ve izinsorunları gibi işlem hataları verilebilir.

## <a name="when-to-use-error-handling"></a>Hata işleme ne zaman kullanılır

Kritik bir etkinlik bir hata veya özel durum attığında runbook'unuzda hata işlemeyi kullanın. Runbook'taki bir sonraki etkinliğin işlenmesini önlemek ve hatayı uygun şekilde işlemek önemlidir. Runbook'larınız bir işletme veya hizmet işlemleri işlemini destekliyorsa hatayı işlemek özellikle önemlidir.

Hata oluşturabilen her etkinlik için, başka bir etkinliği gösteren bir hata bağlantısı ekleyebilirsiniz. Hedef etkinlik, kod etkinliği, cmdlet çağırma, başka bir runbook çağırma ve benzeri dahil olmak üzere her türlü olabilir. Hedef etkinlikte normal veya hata bağlantıları da olabilir. Bağlantılar, runbook'un bir kod etkinliğine başvurmadan karmaşık hata işleme mantığı uygulamasına olanak sağlar.

Önerilen uygulama, ortak işlevsellik ile özel bir hata işleme runbook oluşturmaktır, ancak bu uygulama zorunlu değildir. Örneğin, sanal bir makineyi başlatmaya ve üzerine bir uygulama yüklemeye çalışan bir runbook düşünün. VM doğru başlamazsa, aşağıdakileri

1. Bu sorun hakkında bir bildirim gönderir.
2. Bunun yerine yeni bir VM'yi otomatik olarak sağlayan başka bir runbook başlatır.

Bir çözüm, runbook'ta birinci adımı işleyen bir etkinliği işaret eden bir hata bağlantısına sahip olmaktır. Örneğin, runbook `Write-Warning` cmdlet'i [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.5.0) cmdlet gibi ikinci adımiçin bir aktiviteye bağlayabilir.

Bu iki aktiviteyi ayrı bir hata işleme runbook'a koyarak bu davranışı birçok runbook'ta kullanılmak üzere genelleştirebilirsiniz. Özgün runbook'unuzun bu hata işleme runbook'unu aramadan önce, verilerinden özel bir ileti oluşturup hata işleme runbook'una parametre olarak aktarabilir.

## <a name="how-to-use-error-handling"></a>Hata işleme nasıl kullanılır

Runbook'unuzdaki her etkinlik, özel durumları sonlandırıcı olmayan hatalara dönüştüren bir yapılandırma ayarı vardır. Bu ayar varsayılan olarak devre dışıdır. Runbook'unuzun hataları işlediği herhangi bir etkinlikte bu ayarı etkinleştirmenizi öneririz. Bu ayar, runbook'un bir hata bağlantısı kullanarak etkinlikteki sonlandırıcı ve sonlandırıcı olmayan hataları sonlandırıcı olmayan hatalar olarak işlemesini sağlar.  

Yapılandırma ayarını etkinleştirdikten sonra, runbook'unuzun hatayı işleyen bir etkinlik oluşturmasını sağlayın. Etkinlik herhangi bir hata üretirse, giden hata bağlantıları izlenir. Etkinlik de düzenli çıktı üretse bile düzenli bağlantılar izlenir.<br><br> ![Otomasyon runbook’u hata bağlantısı örneği](media/automation-runbook-graphical-error-handling/error-link-example.png)

Aşağıdaki örnekte, bir runbook bir VM bilgisayar adını içeren bir değişken alır. Daha sonra bir sonraki etkinlikle VM'yi başlatmaya çalışır.<br><br> ![Otomasyon runbook hata işleme örneği](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

Etkinlik `Get-AutomationVariable` ve [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0) cmdlet, özel durumları hatalara dönüştürecek şekilde yapılandırılır. Değişkeni almak veya VM'yi başlatmakta sorun varsa, kod hatalar oluşturur.<br><br> ![Otomasyon runbook hata işleme](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)etkinlik ayarları.

Hata bağlantıları, bu etkinliklerden `error management` tek bir kod etkinliğine akar. Bu etkinlik, geçerli özel durumu açıklayan iletiyi almak için işlemeyi durdurmak için `throw` anahtar sözcüğü kullanan basit bir PowerShell ifadesiyle `$Error.Exception.Message` yapılandırılır.<br><br> ![Otomasyon runbook hata işleme kodu örneği](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bağlantılar ve grafik runbook’larındaki bağlantı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu’nda grafik yazma](automation-graphical-authoring-intro.md#links-and-workflow).

* Runbook yürütme, runbook işlerinin izlenmesi ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda Runbook yürütme](automation-runbook-execution.md)bölümüne bakın.