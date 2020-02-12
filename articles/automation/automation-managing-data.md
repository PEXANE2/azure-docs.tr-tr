---
title: Azure Otomasyonu verilerini yönetme
description: Bu makalede, Azure Otomasyonu ortamının yönetilmesi için birden çok konu yer almaktadır.  Şu anda Azure Otomasyonu 'nda veri saklama ve Azure Otomasyonu olağanüstü durum kurtarmayı yedekleme dahil.
services: automation
ms.subservice: shared-capabilities
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: daa5bab7c8d4cbe98ffe9a8a8a4b66da029fef5c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75421898"
---
# <a name="managing-azure-automation-data"></a>Azure Otomasyonu verilerini yönetme
Bu makalede, Azure Otomasyonu ortamının yönetilmesi için birden çok konu yer almaktadır.

## <a name="data-retention"></a>Veri saklama
Azure Otomasyonu 'nda bir kaynağı sildiğinizde, kalıcı olarak kaldırılmadan önce denetim amaçlarıyla 90 gün boyunca tutulur.  Bu süre içinde kaynağı göremez veya kullanamazsınız.  Bu ilke ayrıca, silinen bir Otomasyon hesabına ait olan kaynaklar için de geçerlidir.

Azure Otomasyonu, 90 günden eski işleri otomatik olarak siler ve kalıcı olarak kaldırır.

Aşağıdaki tabloda, farklı kaynaklar için bekletme ilkesi özetlenmektedir.

| Veriler | İlke |
|:--- |:--- |
| Hesaplar |Hesap bir kullanıcı tarafından silindikten sonra 90 gün sonra kalıcı olarak kaldırıldı. |
| Varlıklar |Varlık bir kullanıcı tarafından silindikten 90 gün sonra veya varlığı tutan hesap bir kullanıcı tarafından silindikten sonra 90 gün sonra kalıcı olarak kaldırıldı. |
| Modüller |Modül bir kullanıcı tarafından silindikten sonra 90 gün sonra veya modülü tutan hesap bir kullanıcı tarafından silindikten sonra 90 gün sonra kalıcı olarak kaldırıldı. |
| Runbook'lar |Kaynak kullanıcı tarafından silindikten sonra 90 gün veya kaynağı tutan hesap bir kullanıcı tarafından silindikten sonra 90 gün sonra kalıcı olarak kaldırıldı. |
| İş |Son değiştirildikten sonra 90 gün sonra silindi ve kalıcı olarak kaldırıldı. Bu, iş tamamlandıktan, durdurulduğunda veya askıya alındıktan sonra olabilir. |
| Düğüm konfigürasyonları/MOF dosyaları |Yeni bir düğüm yapılandırması oluşturulduktan sonra eski düğüm yapılandırması 90 gün sonra kalıcı olarak kaldırılır. |
| DSC düğümleri |Windows PowerShell 'de Azure portal veya [Unregister-AzureRMAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/unregister-azurermautomationdscnode) cmdlet 'ini kullanarak bir düğümün Otomasyon hesabından kaydolduktan sonra 90 gün sonra kalıcı olarak kaldırıldı. Düğüm tutan hesap bir kullanıcı tarafından silindikten sonra da düğümler 90 gün sonra kalıcı olarak kaldırılır. |
| Düğüm raporları |Bu düğüm için yeni bir rapor oluşturulduktan 90 gün sonra kalıcı olarak kaldırıldı |

Bekletme ilkesi tüm kullanıcılar için geçerlidir ve şu anda özelleştirilemiyor.

Ancak, verileri daha uzun bir süre tutmanız gerekiyorsa, runbook iş günlüklerini Azure Izleyici günlüklerine iletebilirsiniz.  Daha fazla bilgi için [Azure Otomasyonu iş verilerini Azure izleyici günlüklerine ilet](automation-manage-send-joblogs-log-analytics.md)' i inceleyin.   

## <a name="backing-up-azure-automation"></a>Azure Otomasyonunu Yedekleme
Microsoft Azure bir Otomasyon hesabını sildiğinizde, hesaptaki tüm nesneler runbook 'lar, modüller, konfigürasyonlar, ayarlar, işler ve varlıklar gibi silinir. Hesap silindikten sonra nesneler kurtarılamıyor.  Otomasyon hesabınızın içeriğini silmeden önce yedeklemek için aşağıdaki bilgileri kullanabilirsiniz. 

### <a name="runbooks"></a>Runbook'lar
Windows PowerShell 'de Azure portal veya [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) cmdlet 'ini kullanarak runbook 'larınızı betik dosyalarına aktarabilirsiniz.  Bu betik dosyaları, [runbook oluşturma veya Içeri aktarma](/previous-versions/azure/dn643637(v=azure.100))konusunda anlatıldığı gibi başka bir Otomasyon hesabına aktarılabilir.

### <a name="integration-modules"></a>Tümleştirme modülleri
Azure Otomasyonu 'ndan tümleştirme modüllerini dışarı aktarabilirsiniz.  Otomasyon hesabı dışında kullanılabilir olduklarından emin olmanız gerekir.

### <a name="assets"></a>Varlıklar
Azure Otomasyonu 'ndan [varlıkları](/previous-versions/azure/dn939988(v=azure.100)) dışarı aktarabilirsiniz.  Azure portal kullanarak değişkenlerin, kimlik bilgilerinin, sertifikaların, bağlantıların ve zamanlamaların ayrıntılarını dikkate almalısınız.  Daha sonra, başka bir otomasyona aktardığınız runbook 'lar tarafından kullanılan tüm varlıkları el ile oluşturmanız gerekir.

Şifrelenmemiş varlıkların ayrıntılarını almak ve daha sonra başvurmak veya başka bir Otomasyon hesabında eşdeğer varlıklar oluşturmak için [Azure cmdlet 'lerini](https://docs.microsoft.com/powershell/module/azurerm.automation#automation) kullanabilirsiniz.

Cmdlet 'leri kullanarak, şifrelenmiş değişkenler veya kimlik bilgilerinin parola alanı için değeri alamazsınız.  Bu değerleri bilmiyorsanız [Get-AutomationVariable](/previous-versions/azure/dn940012(v=azure.100)) ve [Get-AutomationPSCredential](/previous-versions/azure/dn940015(v=azure.100)) etkinliklerini kullanarak bir runbook 'tan alabilirsiniz.

Sertifikaları Azure Otomasyonu 'ndan dışarı aktarabilirsiniz.  Azure dışında tüm sertifikaların kullanılabilir olduğundan emin olmanız gerekir.

### <a name="dsc-configurations"></a>DSC yapılandırması
Windows PowerShell 'de Azure portal veya [Export-AzureRmAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/azurerm.automation/export-azurermautomationdscconfiguration) cmdlet 'ini kullanarak, yapılandırmaları betik dosyalarına aktarabilirsiniz. Bu yapılandırma, başka bir Otomasyon hesabında içeri ve kullanılabilir.

## <a name="geo-replication-in-azure-automation"></a>Azure Otomasyonu 'nda coğrafi çoğaltma
Coğrafi çoğaltma, Azure Otomasyonu hesaplarında standart, hesap verilerini yedeklilik için farklı bir coğrafi bölgeye yedekler. Hesabınızı ayarlarken bir birincil bölge seçebilirsiniz ve ardından bu bir ikincil bölge otomatik olarak atanır. Birincil bölgeden kopyalanmış olan ikincil veriler, veri kaybı durumunda sürekli olarak güncelleştirilir.  

Aşağıdaki tabloda kullanılabilir birincil ve ikincil bölge eşleştirmeleri gösterilmektedir.

| Birincil | İkincil |
| --- | --- |
| Orta Güney ABD |Orta Kuzey ABD |
| ABD Doğu 2 |Orta ABD |
| Batı Avrupa |Kuzey Avrupa |
| Güneydoğu Asya |Doğu Asya |
| Doğu Japonya |Batı Japonya |

Birincil bölge verilerinin kaybolduğu olası bir olayda, Microsoft bunu kurtarmaya çalışır. Birincil veriler kurtarılamazsa coğrafi Yük devretme gerçekleştirilir ve etkilenen müşteriler, abonelikleri üzerinden bu hakkında bildirim alınacaktır.


