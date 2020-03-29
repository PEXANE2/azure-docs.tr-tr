---
title: Azure Blockchain Hizmetini İzleme (ABS)
description: Azure Monitor üzerinden Azure Blockchain Hizmetini İzleme
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: v-umha
ms.openlocfilehash: 6f2a91a8ffce67d3c4008a7587f2787f6446c341
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76293258"
---
# <a name="monitor-azure-blockchain-service-through-azure-monitor"></a>Azure Monitor aracılığıyla Azure Blockchain Hizmetini Izleyin  

Müşteriler Azure Blockchain Hizmeti'nde (ABS) üretim sınıfı blockchain senaryoları çalıştırdırken, kullanılabilirlik, performans ve işlemler için kaynakları izlemek kritik öneme gelir. Bu makalede, Azure Blockchain Hizmeti tarafından oluşturulan izleme verileri ve üretim sınıfı ortamlarını yönetmek için Azure Monitor'un çeşitli özelliklerini ve tümleştirmelerini analiz etmek ve uyarmak için nasıl kullanabileceği açıklanmaktadır.  

## <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?

Azure Blockchain Hizmeti, Azure kaynaklarınızı izlemek için eksiksiz bir özellik kümesi sağlayan Azure'da tam bir yığın izleme hizmeti olan Azure Monitor'u kullanarak izleme verileri oluşturur. Azure Monitor hakkında daha fazla bilgi için Azure [Monitor ile Azure kaynaklarını izleme](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource)'ye bakın.
 

Aşağıdaki bölümler, Azure Blockchain Hizmeti'nden toplanan belirli verileri açıklayarak ve veri toplamayı yapılandırmak ve bu verileri Azure araçlarıyla çözümleme için örnekler sağlayarak bu makaleye dayanmaktadır.

## <a name="monitor-data-collected-from-azure-blockchain-service"></a>Azure Blockchain Hizmeti'nden toplanan verileri izleme  

Azure Blockchain Hizmeti, Azure kaynaklarından gelen [verileri izlemede](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data) açıklanan diğer Azure kaynaklarıyla aynı tür izleme verilerini toplar. Azure Blockchain Hizmeti tarafından oluşturulan günlüklerin ve ölçümlerin ayrıntılı bir başvurusu için [Azure Blockchain Hizmeti veri referansını izleyin'](#monitor-azure-blockchain-service-data-reference) e bakın.

Her Azure Blockchain Hizmeti üyesi kaynağı için Azure portalındaki genel bakış sayfası, işlenen ve işlenen bloklar da dahil olmak üzere işlemlerin kısa bir görünümünü içerir. Bu verilerin bir kısmı otomatik olarak toplanır ve Azure Blockchain Hizmeti üye kaynağını oluşturduktan sonra analiz edilebilecek, ek yapılandırma yla ek veri toplamayı etkinleştirebilirsiniz.

## <a name="diagnostic-settings"></a>Tanılama ayarları  

Platform ölçümleri ve Etkinlik günlüğü otomatik olarak toplanır, ancak kaynak günlüklerini toplamak veya Azure Monitor'un dışına iletmek için bir tanılama ayarı oluşturmanız gerekir. Azure portalı, CLI veya PowerShell'i kullanarak bir tanıa ayar oluşturmak için ayrıntılı işlem için [Azure'da platform günlükleri ve ölçümleri toplamak için tanılama ayarını oluştur'a](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) bakın.

Tanılama ayarı oluşturduğunuzda, hangi günlük kategorilerinin toplandığını belirtirsiniz. Azure Blockchain Hizmeti kategorileri aşağıda listelenmiştir.

**Blockchain proxy günlükleri** – NGNIX proxy günlüklerini izlemek istiyorsanız kategoriyi seçin. Tüm müşteri hareket ayrıntıları denetim ve hata ayıklama amacıyla kullanılabilir.  

**Blockchain uygulama günlükleri** – Yönetilen hizmettarafından barındırılan blockchain uygulamasının günlüklerini almak için kategoriyi seçin. Örneğin, bir ABS-Quorum üyesi için bu günlükler Quorum'un kendisinden gelen günlükler olacaktır.  

**Metrik istekler**: Azure Cosmos DB'den Azure Ölçümleri'nde otomatik olarak toplanan tanı ayarındaki hedeflere metrik veri toplama seçeneğini seçin. Her iki veri türüni birlikte analiz etmek ve azure verilerini Azure Monitor'un dışına göndermek için kaynak günlükleriyle metrik veriler toplayın.

## <a name="analyze-metric-data"></a>Metrik verileri analiz edin  

Azure Blockchain Hizmeti ölçümlerini Metrics explorer ile analiz edebilir, ABS kaynak bıçağındaki İzleme bölümü altında Ölçümler sekmesine gidebilirsiniz. Aracı kullanma yla ilgili ayrıntılar için [Azure Ölçümleri Gezgini ile başlarken](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) bkz. Azure Blockchain Hizmeti için tam ölçümler ad alanı Azure Blockchain Service standart ölçümlerinde yer alıyor.

Bir filtre eklerken veya abs üyesinin işlem düğümleri ve doğrulayıcı düğümleri başına metrik değerler sağlayan ölçümleri bölerken **düğüm** boyutunu kullanabilirsiniz.

## <a name="analyze-log-data"></a>Günlük verilerini analiz etme

Azure Blockchain Hizmeti üyelerinizi izlemenize yardımcı olmak için Günlük arama çubuğuna girebileceğiniz bazı sorgular aşağıda verebilirsiniz. Bu sorgular [yeni dil](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)ile çalışır.

Blockchain uygulama günlüklerinde hata koşullarını sorgulamak için aşağıdaki sorguyu kullanın:

```
BlockchainApplicationLog | where BlockchainMessage contains "ERROR" or BlockchainMessage contains "fatal"

```

Blockchain proxy günlüklerinde hata koşullarını sorgulamak için aşağıdaki sorguyu  


```
BlockchainProxyLog
| filter Code != 200
| limit 500

```
Sorguyu belirli bir zaman aralığı için filtrelemek için Azure günlüklerinde bulunan zaman filtrelerini kullanabilirsiniz.

## <a name="monitor-azure-blockchain-service-data-reference"></a>Azure Blockchain Hizmeti veri başvurularını izleme  

Bu makalede, Azure Blockchain Hizmetinin performansını ve kullanılabilirliğini analiz etmek için toplanan günlük ve metrik verilerin bir başvurusu verilmektedir.  

### <a name="resource-logs"></a>Kaynak günlükleri

Tüm kaynak günlükleri, blockchain hizmetine özgü birkaç benzersiz özelliklere sahip üst düzey ortak bir şemayı paylaşır. Makaleye bakabilirsiniz [Üst düzey kaynak günlükleri şeması,](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-schema#top-level-resource-logs-schema)Azure Blockchain Hizmeti belirli özellikleri ayrıntıları aşağıda kapsanan  

Aşağıdaki tabloda, Azure Monitor Günlükleri veya Azure Depolama'da toplandığında Azure Blockchain proxy günlüklerinin özellikleri listelenir.  


| Özellik adı  | Açıklama |
|:---|:---|
| time | İşlemin gerçekleştiği tarih ve saat (UTC). |
| Resourceıd  | Günlüklerin etkin olduğu Azure Blockchain Hizmeti kaynağı.  |
| category  |Azure Blockchain Hizmeti için, mümkün olan değerler Proxy günlükleri ve **Applicationlogs'tır.** **Proxylogs** |
| operationName  | Bu olaytarafından temsil edilen işlemin adı.   |
| Günlük düzeyi  | Varsayılan olarak, Azure Blockchain Hizmeti **Bilgi günlüğü** düzeyini sağlar.   |
| DüğümKonum  | Blockchain üyesinin dağıtıldığı Azure bölgesi.  |
| BlockchainNodeName  | İşlemin gerçekleştirildiği Azure Blockchain Service üyesinin düğümünün adı.   |
| EthMethod  | Altta yatan blockchain protokolü tarafından çağrılan yöntem, Quorum'da eth_sendTransactions, eth_getBlockByNumber vb. olabilir.  |
| Aracı  | Web tarayıcısı Mozilla, Edge vb. gibi bir kullanıcı adına hareket eden kullanıcı aracısı Değerlere örnek olarak şunlar verilebilir: "Mozilla/5.0 (Linux x64) node.js/8.16.0 v8/6.2.414.77"  |
| Kod   | HTTP hata kodları. Genellikle 4XX ve 5XX hata koşullarıdır.  |
| NodeHost  | Düğümün DNS adı.   |
| İstekMeMeAd | HTTP yöntemi olarak adlandırılan, olası değerler burada üye oluşturmak için PUT, mevcut üye nin ayrıntılarını almak için GET, üye silmek için DELETE, üye güncellemek için YAMA.   |
| BlockchainÜyeAdı  | Kullanıcı tarafından sağlanan Azure Blockchain Hizmeti üye adı.  |
| Konsorsiyum | Kullanıcı tarafından sağlanan konsorsiyumun adı.   |
| Remote  | İsteğin geldiği istemcinin IP'si.  |
| İstek Boyutu  | Baytlarda yapılan isteğin boyutu.  |
| İstek Süresi  | İsteğin milisaniye cinsinden süresi.|




Aşağıdaki tabloda Azure Blockchain uygulama günlüklerinin özellikleri listelenir.


| Özellik adı  | Açıklama |
|:---|:---|
| time | İşlemin gerçekleştiği tarih ve saat (UTC). |
| Resourceıd  | Günlüklerin etkin olduğu Azure Blockchain Hizmeti kaynağı.|
| category  |Azure Blockchain Hizmeti için, olası değer **Proxy günlükleri** ve **Applicationlogs**vardır.  |
| operationName  | Bu olaytarafından temsil edilen işlemin adı.   |
| Günlük düzeyi  | Varsayılan olarak, Azure Blockchain Hizmeti **Bilgi günlüğü** düzeyini sağlar.   |
| DüğümKonum  | Blockchain üyesinin dağıtıldığı Azure bölgesi.  |
| BlockchainNodeName  | İşlemin gerçekleştirildiği Azure Blockchain Service üyesinin düğümünün adı.   |
| BlockchainMessage    | Bu alan, veri düz günlükleri olan Blockchain uygulama günlüğü içerecektir. ABS-Quorum için, bu Quorum günlükleri olurdu. Bu bilgi, hata, uyarı ve yürütülen eylem hakkında daha fazla bilgi veren bir dize ne tür günlük girişi hakkında bilgi vardır.   |
| Kiracı Kimliği    | Azure Blockchain Hizmeti'nin bölgeye özgü kiracısı. Bu alanın biçimi https://westlake-rp-prod. <region>.cloudapp.azure.com, dağıtılan üyenin Azure bölgesini belirttiği yerdir.       |
| SourceSystem   | Sistem günlükleri doldurur, bu durumda **Azure'dur.**    |



### <a name="metrics"></a>Ölçümler

Aşağıdaki tablolarda Azure Blockchain Hizmeti için toplanan platform ölçümleri listelenir. Tüm ölçümler ad alanı Azure **Blockchain Service** standart ölçümlerinde depolanır.

Azure Monitor desteklenen tüm ölçümlerin (Azure Blockchain Hizmeti dahil) listesi için Bkz. [Azure Monitor desteklenen ölçümler.](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported)

### <a name="blockchain-metrics"></a>Blockchain ölçümleri

Aşağıdaki tabloda Azure Blockchain Hizmeti üye kaynağı için toplanan Blockchain ölçümleri listesi belirtilmiştir.


| Ölçüm adı | Birim  |  Toplama türü| Açıklama   |
|---|---|---|---|
| Bekleyen İşlemler   | Sayı  |  Ortalama | İndir edilmeyi bekleyen hareket sayısı.   |
| İşlenmiş Bloklar   | Sayı  | Toplam  |  Her zaman aralığında işlenen blok sayısı. Şu anda blok boyutu 5 saniye, dolayısıyla bir dakika içinde her düğüm 5 dakika içinde 12 blok ve 60 blok işleyecek.   |
|İşlenen İşlemler    | Sayı  | Toplam  | Bir blokta işlenen hareket sayısı.    |
|Sıraya Ait İşlemler    |  Sayı | Ortalama  | Hemen alınamayan hareket sayısı. Çünkü onlar sıradışı geldi ve gelecekteki bir önceki işlem gelmesi için bekliyor olabilir. Veya, iki işlem sadece bir kez (nonce) ve aynı gaz değeri kullanılan aynı numaraya sahip olabilir, bu nedenle ikinci bir mayınlı olamaz.   |

### <a name="connection-metrics"></a>Bağlantı ölçümleri  

Aşağıdaki tabloda Azure Blockchain Hizmeti üye kaynağı için toplanan farklı bağlantı ölçümleri listelenir. Bunlar NGINX proxy ölçümleridir.


| Ölçüm adı | Birim  |  Toplama türü| Açıklama |
|---|---|---|---|
| Kabul Edilen Bağlantılar   | Sayı  |  Toplam | Kabul edilen istemci bağlantılarının toplam sayısı.   |
| Etkin Bağlantılar  | Sayı  | Ortalama  |  Bekleyen bağlantılar da dahil olmak üzere etkin istemci bağlantılarının geçerli sayısı.    |
|İşlenen Bağlantılar    | Sayı  | Toplam  | İşlenen bağlantıların toplam sayısı. Genellikle, bazı kaynak sınırlarına ulaşılmadığı sürece parametre değeri kabul edilen bağlantılarla aynıdır.     |
|Ele Alınması İstekleri     |  Sayı | Toplam  | Toplam istemci isteği sayısı.  |


### <a name="performance-metrics"></a>Performans Ölçümleri

Aşağıdaki tabloda, Azure Blockchain üye kaynağının düğümlerinin her biri için toplanan performans ölçümleri listelenir.  


| Ölçüm adı | Birim  |  Toplama türü| Açıklama   |
|---|---|---|---|
| CPU Kullanım yüzdesi   | Yüzde  |  Maks | CPU kullanım yüzdesi.     |
| IO Okuma Baytları   | Kilobayt   | Toplam  |  IO toplamı blockchain üye kaynağının tüm düğümleri arasında bayt okuyun.      |
|IO Bayt yaz     | Kilobayt   | Toplam  | IO toplamı blockchain üye kaynağının tüm düğümleri arasında bayt yazar.     |
|Bellek Sınırı       |  Gigabayt   | Ortalama    | Düğüm başına blockchain işlemi için maksimum bellek kullanılabilir. |
|Bellek Kullanımı     | Gigabayt  |  Ortalama | Kullanılan bellek miktarı tüm düğümler arasında ortalama.  |
| Bellek Kullanım Yüzdesi     | Yüzde   | Ortalama  |  Kullanılan belleğin yüzdesi tüm düğümler arasında ortalama.       |
|Depolama Kullanımı      | Gigabayt   | Ortalama  | Kullanılan GB depolama alanı tüm düğümler arasında ortalama olarak kullanılır.       |


## <a name="next-steps"></a>Sonraki Adımlar

Blockchain verilerini yakalamak ve Azure Olay Ağıtı'na dönüştürmek için [Blockchain Data Manager](https://docs.microsoft.com/azure/blockchain/service/data-manager) hakkında daha fazla bilgi edinin.
