---
title: Azure Otomasyonu verilerini yönetme
description: Bu makalede, bir Azure Otomasyon ortamını yönetmek için birden çok konu yer alıyor.  Şu anda Azure Otomasyonu'nda Veri Saklama ve Azure Otomasyonu Olağanüstü Durum Kurtarma'yı yedekleme yi içerir.
services: automation
ms.subservice: shared-capabilities
ms.date: 03/23/2020
ms.topic: conceptual
ms.openlocfilehash: f917e9c64a932d75fd0f6b14c9e0f35808467355
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984666"
---
# <a name="managing-azure-automation-data"></a>Azure Otomasyonu verilerini yönetme

Bu makalede, Azure Otomasyon ortamında veri yönetimi için birden çok konu içerir.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="data-retention"></a>Veri saklama

Azure Otomasyonu'nda bir kaynağı sildiğinizde, kalıcı kaldırma işleminden önce denetim amacıyla birkaç gün saklanır. Bu süre zarfında kaynağı göremiyor veya kullanamazsınız. Bu ilke, silinmiş bir Otomasyon hesabına ait kaynaklar için de geçerlidir.

Aşağıdaki tabloda farklı kaynaklar için bekletme ilkesi özetlenmiştir.

| Veriler | İlke |
|:--- |:--- |
| Hesaplar |Bir hesap, kullanıcı nın hesabı silmesinden 30 gün sonra kalıcı olarak kaldırılır. |
| Varlıklar |Bir varlık, kullanıcı nın silmesinden 30 gün sonra veya kullanıcı varlığı elinde bulunduran bir hesabı sildikten 30 gün sonra kalıcı olarak kaldırılır. |
| DSC Düğümleri |Bir DSC düğümü, Azure portalı veya Windows PowerShell'deki [Kayıt Dışı-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-3.7.0) cmdlet kullanılarak Bir Otomasyon hesabından kaydedilmeden 30 gün sonra kalıcı olarak kaldırılır. Bir düğüm, kullanıcı düğümü tutan hesabı sildikten 30 gün sonra da kalıcı olarak kaldırılır. |
| İşler |Bir iş, örneğin, iş tamamlandıktan sonra, durdurulduktan veya askıya alındıktan 30 gün sonra silinir ve kalıcı olarak kaldırılır. |
| Modüller |Bir modül, kullanıcı nın silmesinden 30 gün sonra veya kullanıcı modülü tutan hesabı sildikten 30 gün sonra kalıcı olarak kaldırılır. |
| Düğüm Yapılandırmaları/MOF Dosyaları |Yeni bir düğüm yapılandırması oluşturulduktan 30 gün sonra eski bir düğüm yapılandırması kalıcı olarak kaldırılır. |
| Düğüm Raporları |Düğüm raporu, bu düğüm için yeni bir rapor oluşturulduktan 90 gün sonra kalıcı olarak kaldırılır. |
| Runbook'lar |Runbook, kullanıcı nın kaynağı silmesinden 30 gün sonra veya kullanıcı nın kaynağı elinde bulunduran hesabı silmesinden 30 gün sonra kalıcı olarak kaldırılır. |

Bekletme ilkesi tüm kullanıcılar için geçerlidir ve şu anda özelleştirici olamaz. Ancak, verileri daha uzun süre saklamanız gerekiyorsa, [Azure Otomasyon iş verilerini Azure Monitor günlüklerine iletebilirsiniz.](automation-manage-send-joblogs-log-analytics.md)

## <a name="data-backup"></a>Veri yedekleme

Azure'da bir Otomasyon hesabını sildiğinizde, hesaptaki tüm nesneler silinir. Nesneler runbook'lar, modüller, yapılandırmalar, ayarlar, işler ve varlıkları içerir. Hesap silindikten sonra kurtarılamazlar. Silmeden önce Otomasyon hesabınızın içeriğini yedeklemek için aşağıdaki bilgileri kullanabilirsiniz.

### <a name="runbooks"></a>Runbook'lar

Runbook'larınızı Azure portalını veya Windows PowerShell'deki [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) cmdlet'i kullanarak komut dosyası dosyalarına dışa aktarabilirsiniz. Bu komut dosyası dosyalarını Azure Otomasyonu'ndaki [Yönet runbook'larında](manage-runbooks.md)belirtildiği gibi başka bir Otomasyon hesabına aktarabilirsiniz.

### <a name="integration-modules"></a>Tümleştirme modülleri

Azure Otomasyon'dan tümleştirme modüllerini dışa aktaramazsınız. Bunları Otomasyon hesabı dışında kullanılabilir hale getirmelisiniz.

### <a name="assets"></a>Varlıklar

Azure Otomasyon varlıklarını dışa aktaramazsınız: sertifikalar, bağlantılar, kimlik bilgileri, zamanlamalar ve değişkenler. Bunun yerine, bu varlıkların ayrıntılarını not etmek için Azure portalını ve Azure cmdlets'i kullanabilirsiniz. Ardından, başka bir Otomasyon hesabına aktardığınız runbook'lar tarafından kullanılan varlıkları oluşturmak için bu ayrıntıları kullanın.

Cmdlets kullanarak şifrelenmiş değişkenlerin veya kimlik bilgilerinin parola alanlarının değerlerini alamazsınız. Bu değerleri bilmiyorsanız, bunları bir runbook'ta alabilirsiniz. Değişken değerleri almak için [Azure Otomasyonundaki Değişken varlıklar'a](shared-resources/variables.md)bakın. Kimlik bilgisi değerlerini alma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'ndaki Kimlik Bilgileri varlıklarına](shared-resources/credentials.md)bakın.

 ### <a name="dsc-configurations"></a>DSC yapılandırmaları

DSC yapılandırmalarınızı Azure portalını veya Windows PowerShell'deki [Dışa Aktarma-AzAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/az.automation/export-azautomationdscconfiguration?view=azps-3.7.0
) cmdlet'ini kullanarak komut dosyası dosyalarına dışa aktarabilirsiniz. Bu yapılandırmaları başka bir Otomasyon hesabında n için içe aktarabilir ve kullanabilirsiniz.

## <a name="geo-replication-in-azure-automation"></a>Azure Otomasyonunda Coğrafi çoğaltma

Azure Otomasyon hesaplarında coğrafi çoğaltma standarttır. Hesabınızı ayarlarken birincil bir bölge seçersiniz. İç Otomasyon coğrafi çoğaltma hizmeti hesaba otomatik olarak ikincil bir bölge atar. Hizmet daha sonra, birincil bölgeden ikincil bölgeye hesap verilerini sürekli olarak yedekler. Birincil ve ikincil bölgelerin tam listesi [İş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure Eşli Bölgeler'](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)de bulunabilir. 

Otomasyon coğrafi çoğaltma hizmeti tarafından oluşturulan yedekleme, Otomasyon varlıklarının, yapılandırmalarının ve benzerlerinin tam bir kopyasıdır. Birincil bölge aşağı iner ve veri kaybederse bu yedekleme kullanılabilir. Birincil bölgeye ait verilerin kaybolması olası durumlarda, Microsoft bu verileri kurtarmaya çalışır. Şirket birincil verileri kurtaramazsa, otomatik hata yı kullanır ve Azure aboneliğiniz aracılığıyla durumu size bildirir. 

Bölgesel bir hata olduğunda Otomasyon coğrafi çoğaltma hizmetine doğrudan dış müşteriler erişemez. Bölgesel arızalar sırasında Otomasyon yapılandırmasını ve runbook'ları korumak istiyorsanız:

1. Birincil Otomasyon hesabınızın coğrafi bölgesiyle eşleştirmek için ikinci bir bölge seçin.

2. İkincil bölgede bir Otomasyon hesabı oluşturun.

3. Birincil hesapta, runbook'larınızı komut dosyası dosyaları olarak dışa aktarın.

4. Runbook'ları ikincil bölgedeki Otomasyon hesabınıza aktarın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu'nda güvenli varlıklar hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda güvenli varlıkları şifrele'ye](automation-secure-asset-encryption.md)bakın.

* Coğrafi çoğaltma hakkında daha fazla bilgi edinmek [için, bkz.](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication)