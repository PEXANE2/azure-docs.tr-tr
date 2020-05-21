---
title: Azure Otomasyonu verilerinin yönetilmesi
description: Bu makalede, veri saklama ve yedekleme dahil olmak üzere Azure Otomasyonu 'nda veri yönetimi kavramları sunulmaktadır.
services: automation
ms.subservice: shared-capabilities
ms.date: 03/23/2020
ms.topic: conceptual
ms.openlocfilehash: 44fe626abd71c13b7f16aa07c4ddf261ff3cacea
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715435"
---
# <a name="management-of-azure-automation-data"></a>Azure Otomasyonu verilerinin yönetilmesi

Bu makalede, verileri bir Azure Otomasyonu ortamında yönetmeye yönelik çeşitli konular yer almaktadır.

## <a name="data-retention"></a>Veri saklama

Azure Otomasyonu 'nda bir kaynağı sildiğinizde, kalıcı kaldırma işleminden önce denetim amaçları için birkaç gün boyunca tutulur. Bu süre içinde kaynağı göremez veya kullanamazsınız. Bu ilke, silinen bir Otomasyon hesabına ait olan kaynaklar için de geçerlidir.

Aşağıdaki tabloda, farklı kaynaklar için bekletme ilkesi özetlenmektedir.

| Veriler | İlke |
|:--- |:--- |
| Hesaplar |Bir hesap, bir kullanıcı tarafından silindikten sonra 30 gün sonra kalıcı olarak kaldırılır. |
| Varlıklar |Bir varlık, bir Kullanıcı onu sildikten 30 gün sonra veya bir kullanıcı varlığı tutan bir hesabı sildikten sonra 30 gün sonra kalıcı olarak kaldırılır. |
| DSC düğümleri |DSC düğümü, Windows PowerShell 'de Azure portal veya [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-3.7.0) cmdlet 'ini kullanarak bir Otomasyon hesabından silinen 30 gün sonra kalıcı olarak kaldırılır. Düğüm, düğüm tutan hesabı sildikten sonra 30 gün sonra da kalıcı olarak kaldırılır. |
| İşler |Bir iş silinir ve değişiklik sonrasında 30 gün sonra kalıcı olarak kaldırılır. Örneğin, iş tamamlandıktan sonra, durdurulur veya askıya alınır. |
| Modül |Bir modül, bir Kullanıcı onu sildikten 30 gün sonra veya bir kullanıcı modülü tutan hesabı sildikten sonra 30 gün sonra kalıcı olarak kaldırılır. |
| Düğüm konfigürasyonları/MOF dosyaları |Yeni bir düğüm yapılandırması oluşturulduktan sonra, eski bir düğüm yapılandırması 30 gün sonra kalıcı olarak kaldırılır. |
| Düğüm raporları |Düğüm raporu, bu düğüm için yeni rapor oluşturulduktan 90 gün sonra kalıcı olarak kaldırılır. |
| Runbook'lar |Bir runbook, bir Kullanıcı kaynağı sildikten 30 gün sonra veya bir Kullanıcı kaynağı tutan hesabı sildikten sonra 30 gün sonra kalıcı olarak kaldırılır. |

Bekletme ilkesi tüm kullanıcılar için geçerlidir ve şu anda özelleştirilemiyor. Ancak, verileri daha uzun bir süre tutmanız gerekiyorsa [Azure Otomasyonu iş verilerini Azure izleyici günlüklerine iletebilirsiniz](automation-manage-send-joblogs-log-analytics.md).

## <a name="data-backup"></a>Veri yedekleme

Azure 'da bir Otomasyon hesabını sildiğinizde, hesaptaki tüm nesneler silinir. Nesneler runbook 'ları, modülleri, konfigürasyonları, ayarları, işleri ve varlıkları içerir. Hesap silindikten sonra kurtarılamaz. Otomasyon hesabınızın içeriğini silmeden önce yedeklemek için aşağıdaki bilgileri kullanabilirsiniz.

### <a name="runbooks"></a>Runbook'lar

Windows PowerShell 'de Azure portal veya [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) cmdlet 'ini kullanarak runbook 'larınızı betik dosyalarına aktarabilirsiniz. Bu komut dosyalarını, [Azure Otomasyonu 'nda runbook 'Ları yönetme](manage-runbooks.md)bölümünde anlatıldığı gibi başka bir Otomasyon hesabına içeri aktarabilirsiniz.

### <a name="integration-modules"></a>Tümleştirme modülleri

Azure Otomasyonu 'ndan tümleştirme modüllerini dışarı aktarabilirsiniz. Onları Otomasyon hesabı dışında kullanılabilir yapmanız gerekir.

### <a name="assets"></a>Varlıklar

Azure Otomasyonu varlıklarını dışarı veremezsiniz: sertifikalar, bağlantılar, kimlik bilgileri, zamanlamalar ve değişkenler. Bunun yerine, Azure portal ve Azure cmdlet 'lerini kullanarak bu varlıkların ayrıntılarını görebilirsiniz. Daha sonra, başka bir Otomasyon hesabına aktardığınız runbook 'lar tarafından kullanılan varlıkları oluşturmak için bu ayrıntıları kullanın.

Cmdlet 'leri kullanarak, şifrelenmiş değişkenlerin veya kimlik bilgilerinin parola alanlarının değerlerini alamazsınız. Bu değerleri bilmiyorsanız bir runbook 'ta alabilirsiniz. Değişken değerlerini almak için bkz. [Azure Otomasyonu 'Nda değişken varlıkları](shared-resources/variables.md). Kimlik bilgisi değerlerini alma hakkında daha fazla bilgi edinmek için bkz. [Azure Automation 'Da kimlik bilgisi varlıkları](shared-resources/credentials.md).

 ### <a name="dsc-configurations"></a>DSC yapılandırması

Windows PowerShell 'de Azure portal veya [Export-AzAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/az.automation/export-azautomationdscconfiguration?view=azps-3.7.0
) cmdlet 'INI kullanarak DSC yapılandırmalarını betik dosyalarına aktarabilirsiniz. Bu konfigürasyonları başka bir Otomasyon hesabında içeri aktarabilir ve kullanabilirsiniz.

## <a name="geo-replication-in-azure-automation"></a>Azure Otomasyonu 'nda coğrafi çoğaltma

Coğrafi çoğaltma, Azure Otomasyonu hesaplarında standarttır. Hesabınızı ayarlarken bir birincil bölge seçersiniz. İç otomasyon coğrafi çoğaltma hizmeti, hesaba otomatik olarak bir ikincil bölge atar. Hizmet daha sonra birincil bölgedeki hesap verilerini ikincil bölgeye sürekli olarak yedekler. Birincil ve ikincil bölgelerin tam listesi [iş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure eşlenmiş bölgeler '](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)de bulunabilir. 

Automation coğrafi çoğaltma hizmeti tarafından oluşturulan yedekleme, Otomasyon varlıklarının, yapılandırmaların ve benzeri tam bir kopyasıdır. Bu yedekleme, birincil bölge aşağı gittiğinde ve verileri kaybederse kullanılabilir. Birincil bir bölgenin verilerinin kaybedilmesi olası olmayan olayda, Microsoft bunu kurtarmaya çalışır. Şirket birincil verileri kurtaramaz, otomatik yük devretme kullanır ve Azure aboneliğinizde bu durumu bilgilendirir. 

Bir bölgesel hata varsa, Otomasyon coğrafi çoğaltma hizmetine doğrudan dış müşteriler erişemez. Bölgesel hatalarda Otomasyon yapılandırmasını ve Runbook 'larını sürdürmek istiyorsanız:

1. Birincil Otomasyon hesabınızın coğrafi bölgesiyle eşleştirmek için bir ikincil bölge seçin.

2. İkincil bölgede bir Otomasyon hesabı oluşturun.

3. Birincil hesapta, runbook 'larınızı betik dosyası olarak dışarı aktarın.

4. Runbook 'ları ikincil bölgedeki Otomasyon hesabınıza aktarın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Automation 'da güvenli varlıklar hakkında daha fazla bilgi edinmek için bkz. [Azure Automation 'da güvenli varlıkları şifreleme](automation-secure-asset-encryption.md).

* Coğrafi çoğaltma hakkında daha fazla bilgi edinmek için bkz. [etkin coğrafi çoğaltma oluşturma ve kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication).