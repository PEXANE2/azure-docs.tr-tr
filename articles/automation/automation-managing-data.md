---
title: Azure Otomasyonu verilerini yönetme
description: Bu makalede, bir Azure Otomasyon ortamını yönetmek için birden çok konu yer alıyor.  Şu anda Azure Otomasyonu'nda Veri Saklama ve Azure Otomasyonu Olağanüstü Durum Kurtarma'yı yedekleme yi içerir.
services: automation
ms.subservice: shared-capabilities
ms.date: 03/23/2020
ms.topic: conceptual
ms.openlocfilehash: 340fa19b6f9f07e192123900bc96b07bb016542f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132891"
---
# <a name="managing-azure-automation-data"></a>Azure Otomasyonu verilerini yönetme

Bu makalede, bir Azure Otomasyon ortamını yönetmek için birden çok konu yer alıyor.

## <a name="data-retention"></a>Veri saklama

Azure Otomasyonu'nda bir kaynağı sildiğinizde, kalıcı olarak kaldırılmadan önce denetim amacıyla 30 gün saklanır. Bu süre zarfında kaynağı göremiyor veya kullanamazsınız. Bu ilke, silinen bir otomasyon hesabına ait kaynaklar için de geçerlidir.

Azure Otomasyonu 30 günden eski işleri otomatik olarak siler ve kalıcı olarak kaldırır.

Aşağıdaki tabloda farklı kaynaklar için bekletme ilkesi özetlenmiştir.

| Veri | İlke |
|:--- |:--- |
| Hesaplar |Hesap kullanıcı tarafından silindikten 30 gün sonra kalıcı olarak kaldırılır. |
| Varlıklar |Kıymet in bir kullanıcı tarafından silinmeden 30 gün sonra veya varlığı elinde bulunduran hesabın kullanıcı tarafından silinmeden 30 gün sonra kalıcı olarak kaldırılır. |
| Modules |Modül kullanıcı tarafından silindikten 30 gün sonra veya modülü tutan hesap kullanıcı tarafından silindikten 30 gün sonra kalıcı olarak kaldırılır. |
| Runbook'lar |Kaynağın kullanıcı tarafından silinmeden 30 gün sonra veya kaynağı elinde bulunduran hesap kullanıcı tarafından silindikten 30 gün sonra kalıcı olarak kaldırılır. |
| İşler |Son değiştirildikten 30 gün sonra silinmiş ve kalıcı olarak kaldırılmış. Bu, iş tamamlandıktan, durdurulduktan veya askıya alındıktan sonra olabilir. |
| Düğüm Yapılandırmaları/MOF Dosyaları |Eski düğüm yapılandırması, yeni bir düğüm yapılandırması oluşturulduktan 30 gün sonra kalıcı olarak kaldırılır. |
| DSC Düğümleri |Düğüm, Azure portalı veya Windows PowerShell'deki [Kayıt Dışı-AzureRMAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/unregister-azurermautomationdscnode) cmdlet'i kullanarak Otomasyon Hesabından kayıt dışı landıktan 30 gün sonra kalıcı olarak kaldırıldı. Düğümleri tutan hesap kullanıcı tarafından silindikten 30 gün sonra da kalıcı olarak kaldırılır. |
| Düğüm Raporları |Bu düğüm için yeni bir rapor oluşturulduktan 90 gün sonra kalıcı olarak kaldırıldı |

Bekletme ilkesi tüm kullanıcılar için geçerlidir ve şu anda özelleştirilemez.

Ancak, verileri daha uzun süre saklamanız gerekiyorsa, runbook iş günlüklerini Azure Monitor günlüklerine iletebilirsiniz. Daha fazla bilgi için [Azure Automation iş verilerini Azure Monitor günlüklerine iletin.](automation-manage-send-joblogs-log-analytics.md)

## <a name="backing-up-azure-automation"></a>Azure Otomasyonu'nun yedeklemi

Microsoft Azure'da bir otomasyon hesabını sildiğinizde, runbook'lar, modüller, yapılandırmalar, ayarlar, işler ve varlıklar dahil olmak üzere hesaptaki tüm nesneler silinir. Hesap silindikten sonra nesneler kurtarılamaz.  Silmeden önce otomasyon hesabınızın içeriğini yedeklemek için aşağıdaki bilgileri kullanabilirsiniz.

### <a name="runbooks"></a>Runbook'lar

Runbook'larınızı Azure portalını veya Windows PowerShell'deki [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) cmdlet'i kullanarak komut dosyası dosyalarına dışa aktarabilirsiniz. Bu komut dosyası [dosyaları, Runbook Oluşturma veya Alma'da](/previous-versions/azure/dn643637(v=azure.100))açıkldığı gibi başka bir otomasyon hesabına aktarılabilir.

### <a name="integration-modules"></a>Tümleştirme modülleri

Azure Otomasyon'dan tümleştirme modüllerini dışa aktaramazsınız. Otomasyon hesabının dışında kullanılabilir olduğundan emin olmalısınız.

### <a name="assets"></a>Varlıklar

Azure Otomasyonundan [varlık](/previous-versions/azure/dn939988(v=azure.100)) dışa aktaramazsınız.  Azure portalını kullanarak değişkenlerin, kimlik bilgilerinin, sertifikaların, bağlantıların ve zamanlamaların ayrıntılarını not almalısınız.  Daha sonra, başka bir otomasyona aktardığınız runbook'lar tarafından kullanılan varlıkları el ile oluşturmanız gerekir.

Azure [cmdlets'i,](https://docs.microsoft.com/powershell/module/azurerm.automation#automation) şifrelenmemiş varlıkların ayrıntılarını almak ve bunları gelecekteki başvurulara kaydetmek veya başka bir otomasyon hesabında eşdeğer varlıklar oluşturmak için kullanabilirsiniz.

Cmdlets kullanarak şifrelenmiş değişkenlerin veya kimlik bilgilerinin parola alanının değerini alamazsınız. Bu değerleri bilmiyorsanız, [Bunları Get-AutomationVariable ve Get-AutomationPSCredential](/previous-versions/azure/dn940012(v=azure.100)) etkinliklerini kullanarak bir runbook'tan alabilirsiniz. [Get-AutomationPSCredential](/previous-versions/azure/dn940015(v=azure.100))

Azure Otomasyonundan sertifika dışa aktaramazsınız. Azure dışında sertifikaların kullanılabilir olduğundan emin olmalısınız.

### <a name="dsc-configurations"></a>DSC yapılandırmaları

Yapılandırmalarınızı Azure portalını veya Windows PowerShell'deki [Azure-AzureRmAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/azurerm.automation/export-azurermautomationdscconfiguration) cmdlet'ini kullanarak komut dosyası dosyalarına dışa aktarabilirsiniz. Bu yapılandırmalar başka bir otomasyon hesabında içe aktarılabilir ve kullanılabilir.

## <a name="geo-replication-in-azure-automation"></a>Azure Otomasyonunda Coğrafi çoğaltma

Azure Otomasyon hesaplarında standart olan coğrafi çoğaltma, hesap verilerini artıklık için farklı bir coğrafi bölgeye yedekler. Hesabınızı ayarlarken birincil bir bölge seçebilirsiniz ve ardından otomatik olarak ikinci bir bölge atanır. Birincil bölgeden kopyalanan ikincil veriler, veri kaybı durumunda sürekli olarak güncelleştirilir.  

Aşağıdaki tabloda kullanılabilir birincil ve ikincil bölge eşleşmeleri gösterilmektedir.

| Birincil | İkincil |
| --- | --- |
| Orta Güney ABD |Orta Kuzey ABD |
| ABD Doğu 2 |Orta ABD |
| Batı Avrupa |Kuzey Avrupa |
| Güneydoğu Asya |Doğu Asya |
| Doğu Japonya |Batı Japonya |

Birincil bölge verilerinin kaybolması olası durumlarda, Microsoft bu verileri kurtarmaya çalışır. Birincil veriler kurtarılamazsa, coğrafi hata gerçekleştirilir ve etkilenen müşteriler abonelikleri aracılığıyla bu konuda bilgilendirilir.
