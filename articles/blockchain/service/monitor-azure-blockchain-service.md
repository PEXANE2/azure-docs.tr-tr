---
title: Azure blok zinciri hizmetini izleme (ABS)
description: Azure Izleyici aracılığıyla Azure blok zinciri hizmetini izleme
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: coborn
ms.openlocfilehash: 8c2dc6afeaa00e4c7455940cbdf5a7acd6e17394
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75780411"
---
# <a name="monitor-azure-blockchain-service-through-azure-monitor"></a>Azure Izleyici aracılığıyla Azure blok zinciri hizmetini izleme  

Müşteriler, Azure blok zinciri hizmeti 'nde (ABS) üretim sınıfı blok zinciri senaryolarını çalıştırırken, kaynakların kullanılabilirlik, performans ve işlemler için izlenmesi kritik hale gelir. Bu makalede, Azure blok zinciri hizmeti tarafından oluşturulan izleme verileri ve bir hizmetin, üretim sınıfı ortamlarını yönetmek üzere analiz ve uyarı vermek üzere Azure Izleyici 'nin çeşitli özelliklerini ve tümleştirmelerini nasıl kullanabileceği açıklanmaktadır.  

## <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?

Azure blok zinciri hizmeti, Azure 'da tam bir yığın izleme hizmeti olan Azure Izleyici 'yi kullanarak izleme verileri oluşturur. Bu, Azure kaynaklarınızı izlemeye yönelik eksiksiz bir özellik kümesi sağlar. Azure Izleyici hakkında daha fazla bilgi için bkz. Azure [izleyici Ile Azure kaynaklarını izleme](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource).
 

Aşağıdaki bölümler, Azure blok zinciri hizmetinden toplanan belirli verileri açıklayarak ve veri toplamayı yapılandırmaya ve bu verileri Azure araçları ile çözümlemeye yönelik örnekler sunarak bu makaleyi oluşturur.

## <a name="monitor-data-collected-from-azure-blockchain-service"></a>Azure blok zinciri hizmeti 'nden toplanan verileri izleme  

Azure blok zinciri hizmeti, Azure kaynaklarından gelen [verileri izleme](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data) bölümünde açıklanan diğer Azure kaynaklarıyla aynı türde izleme verilerini toplar. Azure blok zinciri hizmeti tarafından oluşturulan günlüklere ve ölçümlere ilişkin ayrıntılı bir başvuru için bkz. [Azure blok zinciri hizmeti veri başvurusunu izleme](#monitor-azure-blockchain-service-data-reference) .

Her Azure blok zinciri hizmeti üye kaynağı için Azure portal genel bakış sayfası, işlenen istekler ve işlenen bloklar dahil olmak üzere işlemlerin kısa bir görünümünü içerir. Bu verilerden bazıları otomatik olarak toplanır ve Azure blok zinciri hizmeti üye kaynağını oluşturduktan sonra ek yapılandırma ile ek veri toplamayı etkinleştirebilmeniz durumunda analiz için kullanılabilir.

## <a name="diagnostic-settings"></a>Tanılama ayarları  

Platform ölçümleri ve etkinlik günlüğü otomatik olarak toplanır, ancak kaynak günlüklerini toplamak veya Azure Izleyici dışında iletmek için bir tanılama ayarı oluşturmanız gerekir. Azure portal, CLı veya PowerShell kullanarak bir tanılama ayarı oluşturmaya yönelik ayrıntılı süreç için [Azure 'da platform günlüklerini ve ölçümlerini toplamak üzere tanılama ayarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) konusuna bakın.

Bir tanılama ayarı oluşturduğunuzda hangi günlük kategorilerinin toplanacağını belirlersiniz. Azure blok zinciri hizmeti kategorileri aşağıda listelenmiştir.

**Blok zinciri proxy günlükleri** – NGNX proxy günlüklerini izlemek istiyorsanız kategoriyi seçin. Tüm müşteri işlemi ayrıntıları denetim ve hata ayıklama amacıyla kullanılabilir.  

**Blok zinciri uygulama günlükleri** – yönetilen hizmet tarafından barındırılan blok zinciri uygulamasının günlüklerini almak için kategoriyi seçin. Örneğin, bir ABS-Quorum üyesi için bu Günlükler, Çekirdekte olan Günlükler olacaktır.  

**Ölçüm istekleri**: Azure ölçümlerinde otomatik olarak toplanan tanılama ayarındaki hedeflere Azure Cosmos DB ölçüm verileri toplama seçeneğini belirleyin. Her iki veri türünü birlikte analiz etmek ve ölçüm verilerini Azure Izleyici dışında göndermek için kaynak günlükleriyle ölçüm verileri toplayın.

## <a name="analyze-metric-data"></a>Ölçüm verilerini çözümleme  

Azure blok zinciri hizmeti için ölçümleri Ölçüm Gezgini ile analiz edebilir, ABS kaynak dikey penceresinde Izleme bölümü altında ölçümler sekmesine gidebilirsiniz. Aracı kullanma hakkında ayrıntılı bilgi için bkz. [Azure Ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) kullanmaya başlama. Azure blok zinciri hizmeti 'nin tüm ölçümleri, Azure blok zinciri hizmeti standart ölçümleri ad alanıdır.

Bir filtre eklerken veya ölçümleri bölerek **düğüm** boyutunu kullanarak, temel olarak işlem düğümleri başına ölçüm DEĞERLERI ve ABS üyesinin Doğrulayıcı düğümlerini sağlar.

## <a name="analyze-log-data"></a>Günlük verilerini analiz etme

Azure blok zinciri hizmeti üyelerinizi izlemenize yardımcı olması için günlük araması çubuğuna girebileceğiniz bazı sorgular aşağıda verilmiştir. Bu sorguları çalışmak [yeni dil](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

Blok zinciri uygulama günlüklerinde hata koşullarını sorgulamak için aşağıdaki sorguyu kullanın:

```
BlockchainApplicationLog | where BlockchainMessage contains "ERROR" or BlockchainMessage contains "fatal"

```

Blok zinciri ara sunucu günlüklerindeki hata koşullarını sorgulamak için aşağıdaki sorguyu kullanın  


```
BlockchainProxyLog
| filter Code != 200
| limit 500

```
Sorguyu belirli bir zaman aralığı için filtrelemek üzere Azure günlüklerinde bulunan zaman filtrelerini kullanabilirsiniz.

## <a name="monitor-azure-blockchain-service-data-reference"></a>Azure blok zinciri hizmeti veri başvurusunu izleme  

Bu makalede, Azure blok zinciri hizmeti 'nin performansını ve kullanılabilirliğini çözümlemek üzere toplanan günlük ve ölçüm verilerine yönelik bir başvuru sağlanmaktadır.  

### <a name="resource-logs"></a>Kaynak günlükleri

Tüm kaynak günlükleri, blok zinciri hizmetine özgü birkaç benzersiz özellik ile en üst düzey ortak bir şemayı paylaşır. [En üst düzey kaynak günlükleri şeması](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-schema#top-level-resource-logs-schema)makalesine başvurabilirsiniz, Azure blok zinciri hizmetine özgü özelliklerin ayrıntılarına aşağıda kapsanacak  

Aşağıdaki tabloda, Azure Izleyici günlüklerinde veya Azure Storage 'da toplandıklarında Azure blok zinciri ara sunucu günlüklerinin özellikleri listelenmektedir.  


| Özellik adı  | Açıklama |
|:---|:---|
| time | Tarih ve saat (UTC) işlemi oluştuğunda. |
| RESOURCEID  | Günlüklerin etkinleştirildiği Azure blok zinciri hizmeti kaynağı.  |
| category  |Azure blok zinciri hizmeti için, olası değerler **proxylogs** ve **applicationlogs**' lardır. |
| operationName  | Bu olayla temsil edilen işlemin adı.   |
| Günlük düzeyi  | Varsayılan olarak, Azure blok zinciri hizmeti **bilgilendirme** günlüğü düzeyine izin vermez.   |
| NodeLocation  | Blok zinciri üyesinin dağıtıldığı Azure bölgesi.  |
| BlockchainNodeName  | İşlemin gerçekleştirileceği Azure blok zinciri hizmeti üyesinin düğümünün adı.   |
| Etmi yöntemi  | Temel alınan blok zinciri protokolü tarafından çağrılan yöntem, çekirdekte eth_sendTransactions, eth_getBlockByNumber vb. olabilir.  |
| Aracı  | Web tarayıcısı Mozilla, Edge vb. gibi bir kullanıcı adına davranan Kullanıcı Aracısı. Değer örnekleri şunlardır: "Mozilla/5.0 (Linux x64) Node. js/8.16.0 V8/6.2.414.77"  |
| Kodlayın   | HTTP hata kodları. Genellikle 4XX ve 5XX hata koşullarıdır.  |
| NodeHost  | Düğümün DNS adı.   |
| RequestMethodName | HTTP yöntemi çağrıldı, buradaki olası değerler üye Oluştur, var olan üyenin ayrıntılarını alma, silme üyesi için SILME, üyeyi güncelleştirmek için Düzeltme Eki.   |
| BlockchainMemberName  | Kullanıcı tarafından sunulan Azure blok zinciri hizmeti üye adı.  |
| Konsorsiyum | Kullanıcı tarafından sağlandığı şekilde konsorsiyumun adı.   |
| Remote  | İsteğin geldiği istemcinin IP 'si.  |
| RequestSize  | İsteğin bayt cinsinden boyutu.  |
| RequestTime  | İstek süresi (milisaniye).|




Aşağıdaki tabloda, Azure blok zinciri uygulama günlüklerinin özellikleri listelenmektedir.


| Özellik adı  | Açıklama |
|:---|:---|
| time | Tarih ve saat (UTC) işlemi oluştuğunda. |
| RESOURCEID  | Günlüklerin etkinleştirildiği Azure blok zinciri hizmeti kaynağı.|
| category  |Azure blok zinciri hizmeti için, mümkün olan değer **proxylogs** ve **applicationlogs**' lardır.  |
| operationName  | Bu olayla temsil edilen işlemin adı.   |
| Günlük düzeyi  | Varsayılan olarak, Azure blok zinciri hizmeti **bilgilendirme** günlüğü düzeyine izin vermez.   |
| NodeLocation  | Blok zinciri üyesinin dağıtıldığı Azure bölgesi.  |
| BlockchainNodeName  | İşlemin gerçekleştirileceği Azure blok zinciri hizmeti üyesinin düğümünün adı.   |
| BlockchainMessage    | Bu alan, veri düz günlükleri olan blok zinciri uygulama günlüğünü içerecektir. ABS-Quorum için, bu çekirdek günlüklerine sahip olur. Bu, bilgi, hata, uyarı ve yürütülen eylem hakkında daha fazla bilgi veren bir dize olan günlük girdisi türü hakkında bilgi içerir.   |
| Değerine    | Azure blok zinciri hizmeti 'nin bölgeye özgü kiracısı. Bu alanın biçimi https://westlake-rp-prod.<region>. cloudapp.azure.com WHERE bölgesi, dağıtılan üyenin Azure bölgesini belirtir.       |
| SourceSystem   | Bu durumda **Azure**olan Günlükler, sistem tarafından doldurulur.    |



### <a name="metrics"></a>Ölçümler

Aşağıdaki tablolarda, Azure blok zinciri hizmeti için toplanan platform ölçümleri listelenmektedir. Tüm ölçümler, **Azure blok zinciri hizmeti** standart ölçümleri ad alanında depolanır.

Azure Izleyici tarafından desteklenen tüm ölçümlerin (Azure blok zinciri hizmeti dahil) listesi için bkz. [Azure izleyici desteklenen ölçümler](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).

### <a name="blockchain-metrics"></a>Blok zinciri ölçümleri

Aşağıdaki tablo, Azure blok zinciri hizmeti üye kaynağı için toplanan blok zinciri ölçümlerinin listesini belirtir.


| Ölçüm adı | Birim  |  Toplama türü| Açıklama   |
|---|---|---|---|
| Bekleyen Işlemler   | Sayı  |  Ortalama | Mined bekleniyor işlem sayısı.   |
| İşlenen bloklar   | Sayı  | Toplam  |  Her zaman aralığında işlenen blokların sayısı. Şu anda blok boyutu 5 saniyedir, bu nedenle her düğüm 12 blok ve 60 blok 5 dakika içinde işlem görür.   |
|İşlenen Işlemler    | Sayı  | Toplam  | Bir blokta işlenen işlem sayısı.    |
|Kuyruğa alınmış Işlemler    |  Sayı | Ortalama  | Hemen küçük olmayan işlem sayısı. Bunun nedeni, sıra dışı geldiği ve gelecekte bir önceki işlemin gelmesini beklediği için olabilir. Ya da iki işlem yalnızca bir kez (nonce) ve aynı gaz değeri ile aynı olabilir, bu nedenle ikinci diğeri küçük olamaz.   |

### <a name="connection-metrics"></a>Bağlantı ölçümü  

Aşağıdaki tabloda, Azure blok zinciri hizmeti üye kaynağı için toplanan farklı bağlantı ölçümleri listelenmektedir. Bunlar NGıNX ara sunucu ölçümleridir.


| Ölçüm adı | Birim  |  Toplama türü| Açıklama |
|---|---|---|---|
| Kabul edilen bağlantılar   | Sayı  |  Toplam | Kabul edilen istemci bağlantılarının toplam sayısı.   |
| Etkin bağlantılar  | Sayı  | Ortalama  |  Bekleyen bağlantılar dahil etkin istemci bağlantılarının geçerli sayısı.    |
|İşlenmiş bağlantılar    | Sayı  | Toplam  | İşlenen bağlantıların toplam sayısı. Genellikle, bazı kaynak sınırlarına ulaşılmadığı sürece parametre değeri kabul edilen bağlantılarla aynıdır.     |
|İşlenmiş Istekler     |  Sayı | Toplam  | İstemci isteklerinin toplam sayısı.  |


### <a name="performance-metrics"></a>Performans Ölçümleri

Aşağıdaki tabloda, Azure blok zinciri üye kaynağı düğümlerinin her biri için toplanan performans ölçümleri listelenmektedir.  


| Ölçüm adı | Birim  |  Toplama türü| Açıklama   |
|---|---|---|---|
| CPU kullanım yüzdesi   | Yüzde  |  Maks. | CPU kullanımının yüzdesi.     |
| GÇ okuma baytları   | Kilobayt   | Toplam  |  Blok zinciri üye kaynağının tüm düğümlerinde GÇ okuma baytlarının toplamı.      |
|GÇ Yazma baytları     | Kilobayt   | Toplam  | GÇ toplamı blok zinciri üye kaynağının tüm düğümlerinde baytları yazar.     |
|Bellek sınırı       |  Gigabayt   | Ortalama    | Düğüm başına blok zinciri işlemi için kullanılabilir maksimum bellek. |
|Memory Usage     | Gigabayt  |  Ortalama | Tüm düğümlerde ortalama kullanılan bellek miktarı.  |
| Bellek kullanım yüzdesi     | Yüzde   | Ortalama  |  Tüm düğümlerde ortalama kullanılan bellek yüzdesi.       |
|Depolama alanı kullanımı      | Gigabayt   | Ortalama  | Tüm düğümlerde ortalama kullanılan GB depolama alanı.       |


## <a name="next-steps"></a>Sonraki Adımlar

Blok zinciri verilerini Azure Event Grid yakalamak ve dönüştürmek için [veri Yöneticisi blok zinciri](https://docs.microsoft.com/azure/blockchain/service/data-manager) hakkında daha fazla bilgi edinin.
