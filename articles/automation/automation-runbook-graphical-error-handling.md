---
title: Azure Otomasyonu grafik runbook’larında hata işleme
description: Bu makalede, Azure Otomasyonu grafik runbook’larında hata işleme mantığının nasıl uygulanacağı açıklanmıştır.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: dec715ec6741f4429d8b1d4f620ef3cb82d4c1d3
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77649985"
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Azure Otomasyonu grafik runbook’larında hata işleme

Azure Otomasyonu grafik runbook 'unuzu göz önünde bulundurmanız gereken önemli bir tasarım prensibi, runbook 'un yürütme sırasında karşılaşabileceğiniz sorunların kimliğidir. Bu sorunlar arasında başarı, beklenen hata durumları ve beklenmeyen hata koşulları sayılabilir.

Genellikle, bir runbook etkinliğiyle oluşan Sonlandırılmamış bir hata varsa, Windows PowerShell, hatadan bağımsız olarak, takip eden herhangi bir etkinliği işleyerek etkinliği işler. Hata büyük olasılıkla bir özel durum oluşturur, ancak sonraki etkinliğin çalışmasına yine de izin verilir.

Grafik runbook 'manız, yürütme sorunlarıyla başa çıkmak için hata işleme kodu içermelidir. Bir etkinliğin çıkışını doğrulamak veya bir hatayı işlemek için bir PowerShell kod etkinliği kullanabilir, etkinliğin çıkış bağlantısında koşullu mantık tanımlayabilir veya başka bir yöntem uygulayabilirsiniz.

Azure Otomasyonu grafik runbook 'ları, hata işlemeyi dahil etme özelliğiyle geliştirilmiştir. Bundan böyle özel durumları sonlandırıcı olmayan hatalara dönüştürebilir ve etkinlikler arasında hata bağlantıları oluşturabilirsiniz. İyileştirilmiş işlem, runbook 'larınızın hataları yakalayıp gerçekleşen veya beklenmeyen koşulları yönetmesine olanak tanır. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="powershell-error-types"></a>PowerShell hata türleri

Runbook yürütmesi sırasında oluşabilecek PowerShell hatalarının türleri şunlardır: hatalar sonlandırılıyor ve Sonlandırıcı olmayan hatalar.
 
### <a name="terminating-error"></a>Hata sonlandırılıyor

Sonlandırma hatası, yürütme sırasında bir komutu veya betiği yürütmeyi tamamen durduran ciddi bir hatadır. Örnek olmayan cmdlet 'ler, bir cmdlet 'in çalışmasını engelleyen sözdizimi hataları ve diğer önemli hatalar sayılabilir.

### <a name="non-terminating-error"></a>Sonlandırılmamış hata

Sonlandırıcı olmayan bir hata, yürütmenin hata koşuluna rağmen devam etmesine izin veren önemli olmayan bir hatadır. Örnek, dosya bulunamadı hataları ve izin sorunları gibi işletimsel hataları içerir.

## <a name="when-to-use-error-handling"></a>Hata işleme ne zaman kullanılır

Kritik bir etkinlik bir hata veya özel durum oluşturduğunda runbook 'inizdeki hata işlemeyi kullanın. Runbook 'taki bir sonraki etkinliğin işlemesini ve hatayı uygun şekilde işlemesini engellemeniz önemlidir. Runbook 'larınız bir iş veya hizmet işlemleri işlemini desteklerken hatayı işlemek özellikle önemlidir.

Bir hata üreten her etkinlik için başka bir etkinliğe işaret eden bir hata bağlantısı ekleyebilirsiniz. Hedef etkinlik, kod etkinliği, bir cmdlet 'in çağrılması, başka bir runbook 'un çağrılması vb. dahil olmak üzere herhangi bir türde olabilir. Hedef etkinliğin Ayrıca, normal veya hata bağlantıları giden bağlantıları olabilir. Bağlantılar, runbook 'un bir kod etkinliğine ölçeklendirmeden karmaşık hata işleme mantığı uygulamasına izin verir.

Önerilen uygulama, yaygın işlevselliğe sahip bir özel hata işleme runbook 'unu oluşturmaktır, ancak bu yöntem zorunlu değildir. Örneğin, bir sanal makineyi başlatmaya ve üzerine bir uygulama yüklemeye çalışan bir runbook 'u düşünün. VM doğru şekilde başlamazsa:

1. Bu sorun hakkında bir bildirim gönderir.
2. Bunun yerine otomatik olarak yeni bir VM sağlayan başka bir runbook başlatır.

Bir çözüm, runbook 'ta birinci adımı işleyen bir etkinliğe işaret eden bir hata bağlantısına sahip olur. Örneğin, runbook, **Write-Warning** cmdlet 'ini ikinci adım Için, [Start-azautomationrunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.5.0) cmdlet 'i gibi bir etkinliğe bağlayabilirler.

Ayrıca, bu iki etkinliği daha önce önerilen kılavuzdan sonra ayrı bir hata işleme runbook 'una yerleştirerek, bu davranışı birçok runbook 'ta kullanmak üzere genelleştirebilirsiniz. Orijinal runbook 'unuzu bu hata işleme runbook 'unu çağırmadan önce, verilerinden özel bir ileti oluşturabilir ve ardından bunu runbook 'u hata işleme öğesine bir parametre olarak geçirebilir.

## <a name="how-to-use-error-handling"></a>Hata işleme nasıl kullanılır

Runbook 'inizdeki her etkinliğin özel durumları Sonlandırıcı olmayan hatalara açan bir yapılandırma ayarı vardır. Bu ayar varsayılan olarak devre dışıdır. Bu ayarı, runbook 'larınızın hataları işlediği herhangi bir etkinlikte etkinleştirmenizi öneririz. Bu yapılandırmayı etkinleştirmek, runbook 'un hem sonlandırmasını hem de Sonlandırılmamış hataları bir hata bağlantısı kullanarak sonlandırmasız hatalar olarak işlemesini sağlar.  

Yapılandırma ayarını etkinleştirdikten sonra, runbook 'un hatayı işleyen bir etkinlik oluşturmasını sağlayabilirsiniz. Etkinlik herhangi bir hata üretirse giden hata bağlantıları izlenir. Etkinlik normal çıktıyı da üretse bile normal bağlantılar izlenmez.<br><br> ![Otomasyon runbook’u hata bağlantısı örneği](media/automation-runbook-graphical-error-handling/error-link-example.png)

Aşağıdaki örnekte, bir runbook bir sanal makinenin bilgisayar adını içeren bir değişken alır. Daha sonra VM 'yi bir sonraki etkinlikle başlatmaya çalışır.<br><br> ![Otomasyon Runbook 'u hata işleme örneği](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

**Get-AutomationVariable** etkinliği ve [Start-azvm](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0) cmdlet 'i, özel durumları hatalara dönüştürmek üzere yapılandırılır. Değişkeni alma veya VM 'yi başlatma sorunları varsa, kod hata oluşturur.<br><br> ![Automation runbook hata-işlem etkinliği ayarları](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png).

Hata bağlantıları bu etkinliklerden tek bir **hata yönetimi** kodu etkinliğine akar. Bu etkinlik, geçerli özel durumu açıklayan iletiyi almak için `$Error.Exception.Message` birlikte işlemeyi durdurmak için *throw* anahtar sözcüğünü kullanan basit bir PowerShell ifadesiyle yapılandırılır.<br><br> ![Automation runbook hata işleme kodu örneği](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bağlantılar ve grafik runbook’larındaki bağlantı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu’nda grafik yazma](automation-graphical-authoring-intro.md#links-and-workflow).

* Runbook yürütmesi, runbook işlerinin izlenmesi ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu 'Nda runbook yürütme](automation-runbook-execution.md).