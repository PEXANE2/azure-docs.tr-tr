---
title: Azure SYNAPSE Analytics (eski adıyla SQL DW) sık sorulan sorular
description: Bu makalede, müşteriler ve geliştiricilerden Azure SYNAPSE Analytics (eski adıyla SQL DW) hakkında sık sorulan sorular listelenmektedir
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: f7cac849caf478850707eb0a71b2369dcfa49520
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197190"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-frequently-asked-questions"></a>Azure SYNAPSE Analytics (eski adıyla SQL DW) sık sorulan sorular

## <a name="general"></a>Genel

S. Azure Synapse nedir?

A. Azure SYNAPSE, veri depolama ve büyük veri analizlerini birlikte getiren sınırsız bir analiz hizmetidir. Aynı zamanda sunucusuz isteğe bağlı veya sağlanmış kaynakları kullanarak koşullarınızda verileri sorgulama özgürlüğü sunar. Azure SYNAPSE, bu iki çalışma LDS 'yi, anında ve makine öğrenimi ihtiyaçları için veri alma, hazırlama, yönetme ve sunma konularında birleştirilmiş bir deneyimle birlikte sunar. Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics nedir](sql-data-warehouse-overview-what-is.md).

S. Azure SQL veri ambarı 'na ne oldu?

A. Azure SYNAPSE, Azure SQL veri ambarı (SQL DW) gelişmiştir. Sektörde önde gelen veri ambarını, yeni bir performans ve özellik düzeyiyle gerçekleştirdik. Mevcut veri ambarı iş yüklerinizi Azure SYNAPSE ile üretimde çalıştırmaya devam edebilir ve önizleme aşamasında olan yeni yeteneklerden otomatik olarak yararlanabilirsiniz. Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics nedir?](sql-data-warehouse-overview-what-is.md).

S. SQL Analytics nedir?

A. SQL Analytics, Azure SYNAPSE ile genel kullanıma sunulan kurumsal veri ambarı özelliklerini ifade eder. Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics nedir](sql-data-warehouse-overview-what-is.md).

S. Azure SYNAPSE kullanmaya başlama Nasıl yaparım??

A. [Daha fazla bilgi Için](https://info.microsoft.com/ww-landing-azure-sql-data-warehouse-contactme.html) [Azure Ücretsiz hesabıyla](https://azure.microsoft.com/free/sql-data-warehouse/) çalışmaya başlayabilir veya satışlarla iletişim kurun. 

S. Azure SYNAPSE veri güvenliği için ne sunuyor?

A. Azure SYNAPSE, TDE ve denetimi gibi verileri korumak için çeşitli çözümler sunar. Daha fazla bilgi için bkz. [güvenlik](sql-data-warehouse-overview-manage-security.md).

S. Azure SYNAPSE 'in hangi yasal veya iş standartlarından uyumlu olduğunu nereden bulabilirim?

A. SOC ve ISO gibi ürüne göre çeşitli uyumluluk teklifleri için [Microsoft Uyumluluk](https://www.microsoft.com/trustcenter/compliance/complianceofferings) sayfasını ziyaret edin. İlk olarak, uyumluluk başlığına göre öğesini seçin. Ardından, hangi hizmetlerin Azure SYNAPSE uyumlu olduğunu görmek için sayfanın sağ tarafındaki Microsoft 'un kapsam içi bulut Hizmetleri bölümünde Azure ' ı genişletin.

S. Power BI bağlanabilir miyim?

A. Evet! Power BI Azure SYNAPSE ile doğrudan sorguyu desteklediğinden, bu, çok sayıda kullanıcı veya gerçek zamanlı veriler için tasarlanmamıştır. Power BI performansını daha iyi hale getirebilmenizi sağlamak için, Azure Analysis Services veya Analysis Service IaaS üzerinde Power BI kullanmayı göz önünde bulundurun.

S. SQL Analytics kapasite sınırları nelerdir?

A. Geçerli [Kapasite limitleri](sql-data-warehouse-service-capacity-limits.md) sayfamıza bakın. 

S. Neden Ölçeklendirmeliyim/Duraklat/devam eden uzun sürüyor?

A. Birkaç etken, işlem yönetimi işlemlerinin süresini etkileyebilir. Uzun süre çalışan işlemler için ortak bir durum işlem geri alma işlemi olur. Bir ölçek veya duraklatma işlemi başlatıldığında, tüm gelen oturumlar engellenir ve sorgular kaldırılır. Sistemin kararlı durumda kalmasını sağlamak için, bir işlemin bir işlem yapılmadan önce geri alınması gerekir. İşlemin ne kadar büyük olması ve günlük boyutunun daha büyük olması, işlemin daha uzun olması sistem kararlı duruma geri yüklenmeye karşı daha uzundur.

## <a name="user-support"></a>Kullanıcı desteği

S. Bir özellik isteğim var ve nereden gönderebilirim?

A. Bir özellik isteğiniz varsa [UserVoice](https://feedback.azure.com/forums/307516-sql-data-warehouse) sayfamıza gönderebilirsiniz

S. X 'i nasıl yapabilirim?

A. Azure SYNAPSE ile geliştirmeye yönelik yardım için [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw) sayfamızda soru sorabilirsiniz. 

S. Nasıl yaparım? bir destek bileti mi gönder?

A. [Destek biletleri](sql-data-warehouse-get-started-create-support-ticket.md) , Azure Portal aracılığıyla dosyalanır.

## <a name="sql-languagefeature-support"></a>SQL dil/özellik desteği 

S. Hangi veri türleri destekleniyor?

A. Bkz. [veri türleri](sql-data-warehouse-tables-data-types.md).

S. Hangi tablo özelliklerini destekliyoruz?

A. Birçok özellik desteklenir. Desteklenmeyen özellikler, [Desteklenmeyen tablo özelliklerinde](sql-data-warehouse-tables-data-types.md)bulunabilir.

## <a name="tooling-and-administration"></a>Araç ve yönetim

S. SQL Analytics REST API 'Leri destekliyor mu?

A. Evet. SQL veritabanıyla birlikte kullanılabilecek çoğu REST işlevi de SQL Analytics ile kullanılabilir. API bilgilerini, REST belge sayfaları veya [MSDN](https://msdn.microsoft.com/library/azure/mt163685.aspx)içinde bulabilirsiniz.


## <a name="loading"></a>Yükleniyor

S. Hangi istemci sürücülerini destekliyoruz?

A. SQL Analytics 'in sürücü desteği [bağlantı dizeleri](sql-data-warehouse-connection-strings.md) sayfasında bulunabilir

S: PolyBase tarafından hangi dosya biçimleri destekleniyor?

Y: Orc, RC, Parquet ve düz olarak sınırlandırılmış metin

S: PolyBase 'i kullanarak hangi veri kaynaklarına bağlanabilirim? 

Y: [Azure Data Lake Storage](sql-data-warehouse-load-from-azure-data-lake-store.md) ve [Azure Storage blob 'ları](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

S: Azure Storage Bloblarına veya ADLS 'e bağlanılırken hesaplama Işlemi mümkün midir? 

Y: Hayır, PolyBase yalnızca depolama bileşenleriyle etkileşime girer. 

S: HDI 'ye bağlanabilir miyim?

Y: HDI,,, bir ya da. Bu durumda, bir SQL Analytics veri ambarına bu verileri yükleyebilirsiniz. Ancak, HDI örneğine aşağı itme hesaplaması oluşturamazsınız. 

## <a name="next-steps"></a>Sonraki adımlar
Azure SYNAPSE hakkında bir bütün olarak daha fazla bilgi için bkz. [genel bakış](sql-data-warehouse-overview-faq.md) sayfası.
