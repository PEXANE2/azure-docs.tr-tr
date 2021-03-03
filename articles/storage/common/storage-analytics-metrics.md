---
title: Azure Depolama Analizi ölçümleri (klasik)
description: Azure depolama 'da Depolama Analizi ölçümlerini nasıl kullanacağınızı öğrenin. İşlem ve kapasite ölçümleri, ölçümlerin nasıl depolandığı, ölçümlerin nasıl ETKİNLEŞTİRİ, ve daha birçok konuda bilgi edinin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 01/29/2021
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.custom: monitoring, devx-track-csharp
ms.openlocfilehash: d900ffa4481ba2b6deb21a8325f3f8def8084f84
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101714741"
---
# <a name="azure-storage-analytics-metrics-classic"></a>Azure Depolama Analizi ölçümleri (klasik)

**31 ağustos 2023** ' de, *Klasik ölçümler* olarak da bilinen depolama Analizi ölçümler kullanımdan kaldırılacak. Daha fazla bilgi için [resmi duyuruya](https://azure.microsoft.com/updates/azure-storage-classic-metrics-will-be-retired-on-31-august-2023/) bakın. Klasik ölçümleri kullanıyorsanız, söz konusu tarihten önce Azure İzleyici'deki ölçümlere geçin. Bu makale geçişi yapmanıza yardımcı olur. 

Azure depolama, bir depolama hizmetine yapılan isteklerle ilgili toplanan işlem istatistiklerini ve kapasite verilerini içeren ölçümleri depolamak için Depolama Analizi çözümünü kullanır. İşlemler API işlem düzeyinde ve depolama hizmeti düzeyinde raporlanır. Kapasite, depolama hizmeti düzeyinde raporlanır. Ölçüm verileri şu şekilde kullanılabilir:
- Depolama hizmeti kullanımını çözümleyin.
- Depolama hizmetinde yapılan isteklerle ilgili sorunları tanılayın.
- Hizmet kullanan uygulamaların performansını geliştirir.

 Depolama Analizi ölçümler, varsayılan olarak yeni depolama hesapları için etkinleştirilmiştir. [Azure Portal](https://portal.azure.com/)ölçümleri PowerShell kullanarak veya Azure CLI kullanarak yapılandırabilirsiniz. Adım adım yönergeler için bkz. [Azure Storage analitik ölçümlerini etkinleştirme ve yönetme (klasik)](./manage-storage-analytics-logs.md). Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi programlı bir şekilde etkinleştirebilirsiniz. Her hizmet için Depolama Analizi etkinleştirmek üzere hizmet özelliklerini ayarlama işlemlerini kullanın.  

> [!NOTE]
> Depolama Analizi ölçümler Azure Blob depolama, Azure kuyruk depolama, Azure Tablo depolama ve Azure dosyaları için kullanılabilir.
> Depolama Analizi ölçümler artık klasik ölçümlerdir. Depolama ölçümlerini Depolama Analizi ölçümleri yerine [Azure izleyici 'de](../blobs/monitor-blob-storage.md) kullanmanızı öneririz.

## <a name="transaction-metrics"></a>İşlem ölçümleri  
 Güçlü bir veri kümesi, her depolama hizmeti için saatlik veya dakika aralıklarında, giriş ve çıkış, kullanılabilirlik, hata ve kategorilere ayrılan istek yüzdelerini içeren istenen API işlemi için kaydedilir. İşlem ayrıntılarının tüm listesi için bkz. [ölçüm tablosu şeması depolama Analizi](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

 İşlem verileri, hizmet düzeyinde ve API işlem düzeyinde kaydedilir. Hizmet düzeyinde, tüm istenen API işlemlerini özetleyen istatistikler, hizmette hiçbir istek yapılmasa bile, her saat bir tablo varlığına yazılır. API işlem düzeyinde, yalnızca işlem bu saat içinde isteniyorsa, bir varlığa istatistikler yazılır.  

 Örneğin, blob hizmetinizde bir **GetBlob** işlemi gerçekleştirirseniz depolama Analizi ölçümleri isteği günlüğe kaydeder ve BLOB hizmeti ve **GetBlob** işlemi için toplanan verilere ekler. Saat boyunca bir **GetBlob** işlemi istenmemişse, bu işlem için *$MetricsTransactionsBlob* bir varlık yazılmaz.  

 İşlem ölçümleri Kullanıcı istekleri ve Depolama Analizi tarafından yapılan istekler için kaydedilir. Örneğin, Günlükler ve tablo varlıkları yazmak için Depolama Analizi tarafından yapılan istekler kaydedilir.

## <a name="capacity-metrics"></a>Kapasite ölçümleri  

> [!NOTE]
>  Şu anda kapasite ölçümleri yalnızca blob hizmeti için kullanılabilir.

 Bir depolama hesabının blob hizmeti için kapasite verileri günlük olarak kaydedilir ve iki tablo varlığı yazılır. Bir varlık Kullanıcı verileri için istatistikler sağlar ve diğeri `$logs` depolama Analizi tarafından kullanılan blob kapsayıcısı hakkında istatistikler sağlar. *$MetricsCapacityBlob* tablosu aşağıdaki istatistikleri içerir:  

- **Kapasite**: depolama hesabının blob hizmeti tarafından bayt cinsinden kullanılan depolama miktarı.  
- **Containercount**: depolama hesabının blob hizmetindeki blob kapsayıcıları sayısı.  
- **ObjectCount**: depolama hesabının blob hizmetindeki işlenen ve kaydedilmemiş blok veya sayfa Blobları sayısı.  

  Kapasite ölçümleri hakkında daha fazla bilgi için bkz. [ölçüm tablosu şeması depolama Analizi](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

## <a name="how-metrics-are-stored"></a>Ölçümler nasıl depolanır  

 Depolama hizmetlerinin her biri için tüm ölçüm verileri, bu hizmet için ayrılmış üç tabloda depolanır. Bir tablo, işlem bilgileri için, bir tablo dakikalık işlem bilgileri için ve başka bir tablo ise kapasite bilgilerine yöneliktir. İşlem ve dakikalık işlem bilgileri, istek ve yanıt verilerinden oluşur. Kapasite bilgileri, depolama kullanım verilerinden oluşur. Aşağıdaki tabloda açıklandığı gibi adlı tablolarda, bir depolama hesabının blob hizmetine yönelik saat ölçümleri, dakika ölçümleri ve kapasiteye erişilir.  

|Ölçüm düzeyi|Tablo adları|Sürümler için desteklenir|  
|-------------------|-----------------|----------------------------|  
|Saatlik ölçümler, birincil konum|-$MetricsTransactionsBlob<br />-$MetricsTransactionsTable<br />-$MetricsTransactionsQueue|Yalnızca 15 Ağustos 2013 ' den önceki sürümler. Bu adlar hala desteklenirken, izleyen tabloları kullanarak geçiş yapmanızı öneririz.|  
|Saatlik ölçümler, birincil konum|-$MetricsHourPrimaryTransactionsBlob<br />-$MetricsHourPrimaryTransactionsTable<br />-$MetricsHourPrimaryTransactionsQueue<br />-$MetricsHourPrimaryTransactionsFile|Tüm sürümler. Dosya hizmeti ölçümleri desteği yalnızca 5 Nisan 2015 ve üzeri sürümlerde kullanılabilir.|  
|Dakikalık ölçümler, birincil konum|-$MetricsMinutePrimaryTransactionsBlob<br />-$MetricsMinutePrimaryTransactionsTable<br />-$MetricsMinutePrimaryTransactionsQueue<br />-$MetricsMinutePrimaryTransactionsFile|Tüm sürümler. Dosya hizmeti ölçümleri desteği yalnızca 5 Nisan 2015 ve üzeri sürümlerde kullanılabilir.|  
|Saatlik ölçümler, ikincil konum|-$MetricsHourSecondaryTransactionsBlob<br />-$MetricsHourSecondaryTransactionsTable<br />-$MetricsHourSecondaryTransactionsQueue|Tüm sürümler. Okuma Erişimli Coğrafi olarak yedekli çoğaltmanın etkinleştirilmesi gerekir.|  
|Dakikalık ölçümler, ikincil konum|-$MetricsMinuteSecondaryTransactionsBlob<br />-$MetricsMinuteSecondaryTransactionsTable<br />-$MetricsMinuteSecondaryTransactionsQueue|Tüm sürümler. Okuma Erişimli Coğrafi olarak yedekli çoğaltmanın etkinleştirilmesi gerekir.|  
|Kapasite (yalnızca blob hizmeti)|$MetricsCapacityBlob|Tüm sürümler.|  

 Bu tablolar, bir depolama hizmeti uç noktası için Depolama Analizi etkinleştirildiğinde otomatik olarak oluşturulur. Bunlar, depolama hesabının ad alanı aracılığıyla erişilir. Örneğin, `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")` . Ölçüm tabloları bir listeleme işleminde görünmez ve doğrudan tablo adı ile erişilmelidir.

## <a name="metrics-alerts"></a>Ölçüm uyarıları
[Azure Portal](https://portal.azure.com) uyarı ayarlamayı düşünün, böylece depolama hizmetlerinizin davranışındaki önemli değişiklikler konusunda otomatik olarak bilgilendirilirsiniz. Adım adım yönergeler için bkz. [ölçüm uyarıları oluşturma](./manage-storage-analytics-logs.md).

Bu ölçüm verilerini ayrılmış bir biçimde indirmek için bir Depolama Gezgini aracı kullanırsanız, verileri çözümlemek için Microsoft Excel 'i kullanabilirsiniz. Kullanılabilir Depolama Gezgini araçlarının bir listesi için bkz. [Azure Storage istemci araçları](./storage-explorers.md).

> [!IMPORTANT]
> Bir depolama olayı ile buna karşılık gelen saatlik veya dakikalık ölçüm verileri kaydedilirken bir gecikme olabilir. Dakikalık ölçümler söz konusu olduğunda, birkaç dakikalık veriler aynı anda yazılabilir. Bu sorun, geçerli dakikalık işlem için daha önceki dakikadan elde edilen işlemlere neden olabilir. Bu sorun oluştuğunda, uyarı hizmeti yapılandırılan uyarı aralığı için kullanılabilir tüm ölçüm verilerine sahip olmayabilir, bu da uyarıların beklenmedik şekilde tetiklerine neden olabilir.
>

## <a name="billing-on-storage-metrics"></a>Depolama ölçümlerinde faturalandırma
Ölçümler için tablo varlıkları oluşturmak için yazma istekleri, tüm Azure depolama işlemleri için geçerli olan standart ücretler üzerinden ücretlendirilir.  

Bir istemciye ait ölçüm verilerinin okuma ve silme istekleri de standart ücretler üzerinden faturalandırılabilir. Bir veri saklama ilkesi yapılandırdıysanız, Azure depolama eski ölçüm verilerini sildiğinde ücretlendirilmez. Analiz verilerini silerseniz, hesabınız silme işlemleri için ücretlendirilir.  

Ölçüm tabloları tarafından kullanılan kapasite da faturalanabilir. Ölçüm verilerini depolamak için kullanılan kapasite miktarını tahmin etmek için aşağıdaki bilgileri kullanın:  

-   Her saat bir hizmet her bir hizmette her API 'yi kullanıyorsa, hizmet düzeyi ve API düzeyi bir Özet etkinleştirdiyseniz ölçüm işlem tablolarında yaklaşık 148 KB veri depolanır.  
-   Her saat içinde bir hizmet, hizmette her API 'yi kullanıyorsa, yalnızca bir hizmet düzeyi Özeti etkinleştirdiyseniz ölçüm işlem tablolarında yaklaşık 12 KB 'lık veri depolanır.  
-   Blobların kapasite tablosunda Günlükler için tercih ettiğiniz her gün eklenen iki satır vardır. Bu senaryo, bu tablonun boyutunun yaklaşık 300 bayta kadar arttığı anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama hesabı izleme](https://www.windowsazure.com/manage/services/storage/how-to-monitor-a-storage-account/)   
* [Depolama Analizi ölçüm tablosu şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema)   
* [Günlüğe kaydedilen işlemleri ve durum iletilerini Depolama Analizi](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)   
* [Günlüğe kaydetme Depolama Analizi](storage-analytics-logging.md)
