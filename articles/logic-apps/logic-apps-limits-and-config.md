---
title: Limitler ve yapılandırma
description: Azure Mantık Uygulamaları için süre, iş ortası ve kapasite gibi hizmet sınırlarının yanı sıra, izin verilebilmesi IÇIN IP adresleri gibi yapılandırma değerleri
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 04/17/2020
ms.openlocfilehash: c650cfcbfeddaa83d8bf3127024ac77b93456a57
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683142"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Azure Logic Apps için sınırlar ve yapılandırma bilgileri

Bu makalede, Azure Mantık Uygulamaları ile otomatik iş akışları oluşturma ve çalıştırma nın sınırları ve yapılandırma ayrıntıları açıklanmaktadır. Power Automate için [Power Automate'deki Sınırlar ve yapılandırmaya](https://docs.microsoft.com/flow/limits-and-config)bakın.

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Tanım limitleri

Burada tek bir mantık uygulaması tanımı için sınırları şunlardır:

| Adı | Sınır | Notlar |
| ---- | ----- | ----- |
| İş akışı başına eylemler | 500 | Bu sınırı genişletmek için, gerektiğinde iç içe iş akışları ekleyebilirsiniz. |
| Eylemler için iç içe geçme derinliğine izin | 8 | Bu sınırı genişletmek için, gerektiğinde iç içe iş akışları ekleyebilirsiniz. |
| Abonelik başına bölge başına iş akışları | 1000 | |
| İş akışı başına tetikleyiciler | 10 | Kod görünümünde çalışırken, tasarımcı değil |
| Geçiş kapsamı servis talepleri sınırı | 25 | |
| İş akışı başına değişkenler | 250 | |
| İfade başına karakterler | 8,192 | |
| Için maksimum boyut`trackedProperties` | 16.000 karakter |
| Adı `action` veya`trigger` | 80 karakter | |
| Uzunluk`description` | 256 karakter | |
| Maksimum`parameters` | 50 | |
| Maksimum`outputs` | 10 | |
||||

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Süre ve bekletme sınırlarını çalıştırma

Burada tek bir mantık uygulaması çalıştırmak için sınırları şunlardır:

| Adı | Çok kiracı sınırı | Entegrasyon hizmeti ortamı sınırı | Notlar |
|------|--------------------|---------------------------------------|-------|
| Çalıştırma süresi | 90 gün | 366 gün | Çalıştırma süresi, bir çalıştırmanın başlangıç saati ve *başlangıç saatinde* iş akışı ayarı, [**gün içinde geçmiş bekletme**](#change-duration)tarafından belirtilen sınır kullanılarak hesaplanır. <p><p>90 gün olan varsayılan sınırı değiştirmek için [değişiklik çalışma süresine](#change-duration)bakın. |
| Depolamada bekletme çalıştırma | 90 gün | 366 gün | Çalıştırma bekletme, bir çalıştırmanın başlangıç saati ve iş akışı ayarı tarafından *geçerli zamanda* belirtilen sınır kullanılarak hesaplanır, gün içinde [**geçmiş bekletme.**](#change-retention) Bir çalıştırma tamamlansa da, bekletme hesaplaması her zaman çalıştırmanın başlangıç saatini kullanır. Bir çalıştırmanın süresi *geçerli* bekletme sınırını aştığında, çalışma çalışma geçmişinden kaldırılır. <p><p>Bu ayarı değiştirirseniz, önceki sınır ne olursa olsun, geçerli sınır her zaman bekletme hesaplamak için kullanılır. Örneğin, bekletme sınırını 90 günden 30 güne düşürürseniz, 60 günlük bir çalışma çalışma geçmişinden kaldırılır. Bekletme süresini 30 günden 60 güne çıkarsanız, 20 günlük bir çalışma, çalışma geçmişinde 40 gün daha kalır. <p><p>90 gün olan varsayılan sınırı değiştirmek [için, depolama alanında değişiklik çalışması bekletme](#change-retention)bakın. |
| Minimum yineleme aralığı | 1 saniye | 1 saniye ||
| Maksimum yineleme aralığı | 500 gün | 500 gün ||
|||||

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-run-retention-in-storage"></a>Depolama alanında çalışma süresini ve çalıştırma tutmasüresini değiştirme

Çalışma süresi için varsayılan sınırı değiştirmek ve depolamaalanında bekletme çalıştırmak için aşağıdaki adımları izleyin. Maksimum sınırı artırmak için, gereksinimlerinizle ilgili yardım için [Logic Apps ekibine başvurun.](mailto://logicappsemail@microsoft.com)

> [!NOTE]
> Çok kiracılı Azure'daki mantıksal uygulamalariçin 90 günlük varsayılan sınır, maksimum sınırla aynıdır. Yalnızca bu değeri azaltabilirsiniz.
> Tümleştirme hizmeti ortamındaki mantıksal uygulamalar için, 90 günlük varsayılan sınırı azaltabilir veya artırabilirsiniz.

1. [Azure portalına](https://portal.azure.com)gidin. Portal arama kutusunda, Logic **uygulamalarını**bulun ve seçin.

1. Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı seçin ve açın.

1. Mantık uygulamasının menüsünde **İş Akışı ayarlarını**seçin.

1. **Çalışma Zamanı seçenekleri**altında, gün **listesindeki Run geçmişi bekletme,** **Özel'i**seçin.

1. İstediğindiğiniz gün sayısını değiştirmek için kaydırıcıyı sürükleyin.

1. İşiniz bittiğinde, İş **Akışı ayarları** araç çubuğunda **Kaydet'i**seçin.

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>Eşzamanlılık, döngü ve debatching sınırları

Burada tek bir mantık uygulaması çalıştırmak için sınırları şunlardır:

| Adı | Sınır | Notlar |
| ---- | ----- | ----- |
| Eşpara birimini tetikleme | - Eşzamanlılık denetimi kapatıldığında sınırsız <p><p>- Eşzamanlılık denetimi açık olduğunda varsayılan sınır olan 25,eşzamanlılığı etkinleştirdikten sonra geri alamazsınız. Varsayılan değeri 1 ile 50 arasında bir değere değiştirebilirsiniz. | Bu sınır, aynı anda veya paralel olarak çalıştırılabilen en yüksek mantık uygulaması örneksayısını açıklar. <p><p>**Not**: Eşzamanlılık açık olduğunda, SplitOn sınırı [dizileri ayırmak](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch)için 100 öğeye düşürülür. <p><p>Varsayılan sınırı 1 ile 50 arasında bir değere dahil olarak değiştirmek [Trigger instances sequentially](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger)için [bkz.](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency) |
| Maksimum bekleme çalıştırmaları | - Eşzamanlılık olmadan, en az bekleme çalıştırma sayısı 1 iken, en büyük sayı 50'dir. <p><p>- Eşzamanlılık ile, en az bekleme çalıştırma sayısı 10 artı eşzamanlı çalıştırma sayısıdır (tetik eşzamanlılık). Maksimum sayıyı 100'e kadar kapsamlı olarak değiştirebilirsiniz. | Bu sınır, mantık uygulamanız zaten en yüksek eşzamanlı örnekleri çalıştırırken çalıştırmak için beklenebilir mantık uygulaması örneklerinin en yüksek sayısını açıklar. <p><p>Varsayılan sınırı değiştirmek için [bkz.](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs) |
| Foreach dizi öğeleri | 100.000 | Bu sınır, "her biri için" bir döngü işleyebilir dizi öğeleri nin en yüksek sayısını açıklar. <p><p>Daha büyük dizileri filtrelemek için [sorgu eylemini](logic-apps-perform-data-operations.md#filter-array-action)kullanabilirsiniz. |
| Foreach eşzamanlılık | Eşzamanlılık denetimi kapatıldığında varsayılan sınır 20'dir. Varsayılan değeri 1 ile 50 arasında bir değere değiştirebilirsiniz. | Bu sınır, aynı anda veya paralel olarak çalıştırılabilen en yüksek "her" döngü yineleme sayısıdır. <p><p>Varsayılan sınırı 1 ile 50 arasında bir değere dahil olarak değiştirmek için bkz: ["her biri için" eşzamanlılık sınırını değiştir](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency) veya ["her biri için" döngüleri sırayla çalıştır.](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each) |
| SplitOn öğeleri | - 100.000 tetik eşzamanlılık olmadan <p><p>- Tetik eşzamanlılık ile 100 | Bir dizi döndüren tetikleyiciler için, "Foreach" döngüsü kullanmak yerine dizi öğelerini işleme için [birden çok iş akışı örneğine bölen veya debatchle](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) eden bir 'SplitOn' özelliği kullanan bir ifade belirtebilirsiniz. Bu ifade, her dizi öğesi için bir iş akışı örneği oluşturmak ve çalıştırmak için kullanılacak diziye başvurur. <p><p>**Not**: Eşzamanlılık açık olduğunda, SplitOn sınırı 100 öğeye düşürülür. |
| Yinelemelere kadar | - Varsayılan değer: 60 <p><p>- Maksimum: 5.000 | |
||||

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>İşleme sınırları

Burada tek bir mantık uygulaması tanımı için sınırları şunlardır:

### <a name="multi-tenant-logic-apps-service"></a>Çok kiracılı Logic Apps hizmeti

| Adı | Sınır | Notlar |
| ---- | ----- | ----- |
| Eylem: 5 dakikada bir yürütme | 100.000 varsayılan sınırdır, ancak 300.000 maksimum sınırdır. | Varsayılan sınırı değiştirmek için, [bkz.](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode) Veya gerektiğinde iş yükünü birden fazla mantık uygulamasına dağıtabilirsiniz. |
| Eylem: Eşzamanlı giden aramalar | Yaklaşık 2.500 | Eşzamanlı istek sayısını azaltabilir veya gerektiğinde süreyi azaltabilirsiniz. |
| Çalışma süresi bitiş noktası: Eşzamanlı gelen aramalar | ~1.000 | Eşzamanlı istek sayısını azaltabilir veya gerektiğinde süreyi azaltabilirsiniz. |
| Çalışma süresi bitiş noktası: Aramaları 5 dakikada okuma  | 60.000 | İş yükünü gerektiğinde birden fazla uygulamaya dağıtabilirsiniz. |
| Çalışma zamanı bitiş noktası: 5 dakikada çağrı çağırma | 45.000 | İş yükünü gerektiğinde birden fazla uygulamaya dağıtabilirsiniz. |
| 5 dakikada içerik elde | 600 MB | İş yükünü gerektiğinde birden fazla uygulamaya dağıtabilirsiniz. |
||||

### <a name="integration-service-environment-ise"></a>Entegrasyon hizmet ortamı (ISE)

İşte [Premium Ise SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)için iş için iş göreme limitleri:

| Adı | Sınır | Notlar |
|------|-------|-------|
| Temel birim yürütme sınırı | Altyapı kapasitesi %80'e ulaştığında sistem daraltıldı | Ayda ~160 milyon eylem yürütmesi olan dakika başına ~4.000 eylem yürütmesi sağlar | |
| Ölçek birim yürütme sınırı | Altyapı kapasitesi %80'e ulaştığında sistem daraltıldı | Her ölçek birimi dakikada ~2.000 ek eylem yürütmesi sağlayabilir, bu da ayda ~80 milyon daha fazla eylem yürütmesi dir. | |
| Ekleyebileceğiniz maksimum ölçek birimleri | 10 | |
||||

Normal işlemede bu sınırların üzerine çıkmak veya bu sınırların üzerine çıkabilecek yük testi çalıştırmak için gereksinimlerinizle ilgili yardım için [Logic Apps ekibine başvurun.](mailto://logicappsemail@microsoft.com)

> [!NOTE]
> [Geliştirici ISE SKU'nun](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) yayınlanmış bir sınırı, ölçeklendirme kapasitesi ve hizmet düzeyi sözleşmesi (SLA) yoktur. Bu SKU'yi yalnızca deneme, geliştirme ve test için kullanın, üretim veya performans testi için değil.

<a name="gateway-limits"></a>

## <a name="gateway-limits"></a>Ağ geçidi sınırları

Azure Logic Apps, ağ geçidi aracılığıyla ekler ve güncelleştirmeler de dahil olmak üzere yazma işlemlerini destekler. Ancak, bu [işlemlerin yük boyutu nda sınırlamaları](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem#considerations)vardır.

<a name="request-limits"></a>

## <a name="http-limits"></a>HTTP sınırları

Burada tek bir giden veya gelen HTTP arama için sınırları şunlardır:

#### <a name="timeout"></a>Zaman aşımı

Bazı bağlayıcı işlemleri eşzamanlı aramalar yapar veya webhook isteklerini dinler, bu nedenle bu işlemler için zaman aşımı bu sınırlardan daha uzun olabilir. Daha fazla bilgi için, belirli bağlayıcı ve aynı zamanda [İş Akışı tetikleyicileri ve eylemleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)için teknik ayrıntılara bakın.

| Adı | Çok kiracı sınırı | Entegrasyon hizmeti ortamı sınırı | Notlar |
|------|--------------------|---------------------------------------|-------|
| Giden istek | 120 saniye <br>(2 dakika) | 240 saniye <br>(4 dakika) | Giden isteklere örnek olarak HTTP tetikleyicileri tarafından yapılan aramalar verilebilir. <p><p>**İpucu**: Daha uzun süren işlemler [için, bir eşzamanlı yoklama deseni](../logic-apps/logic-apps-create-api-app.md#async-pattern) veya [bir until döngüsü](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action)kullanın. |
| Gelen isteği | 120 saniye <br>(2 dakika) | 240 saniye <br>(4 dakika) | Gelen isteklere örnek olarak istek tetikleyicileri ve webhook tetikleyicileri tarafından alınan çağrılar verilebilir. <p><p>**Not**: Özgün arayanın yanıtı alabilmesi için, başka bir mantık uygulamasını iç içe iş akışı olarak çağırmadığınız sürece yanıttaki tüm adımların sınır içinde tamamlanması gerekir. Daha fazla bilgi için [bkz.](../logic-apps/logic-apps-http-endpoint.md) |
|||||

<a name="message-size-limits"></a>

#### <a name="message-size"></a>İleti boyutu

| Adı | Çok kiracı sınırı | Entegrasyon hizmeti ortamı sınırı | Notlar |
|------|--------------------|---------------------------------------|-------|
| İleti boyutu | 100 MB | 200 MB | İmKB etiketli konektörler, İmKB olmayan konektör limitlerini değil, İmKB limitini kullanır. <p><p>Bu sınırı aşmak için [bkz.](../logic-apps/logic-apps-handle-large-messages.md) Ancak, bazı bağlayıcılar ve API'ler ötme yi ve hatta varsayılan sınırı desteklemeyebilir. |
| Yığınlı ileti boyutu | 1 GB | 5 GB | Bu sınır, yerel olarak ötme yi destekleyen veya çalışma zamanı yapılandırmalarında öbeklenmeyi etkinleştirmenize olanak tanıyan eylemler için geçerlidir. <p><p>Tümleştirme hizmeti ortamı için Logic Apps altyapısı bu sınırı destekler, ancak konektörlerin motor sınırına kadar kendi yığınlama sınırları vardır (örneğin, [Azure Blob Depolama konektörünAPi başvurusuna](https://docs.microsoft.com/connectors/azureblob/)bakın. Yığınlama hakkında daha fazla bilgi için [bkz.](../logic-apps/logic-apps-handle-large-messages.md) |
|||||

#### <a name="character-limits"></a>Karakter sınırları

| Adı | Notlar |
|------|-------|
| İfade değerlendirme limiti | 131.072 karakter | `@concat()`, `@base64()`, `@string()` ifadeler bu sınırdan daha uzun olamaz. |
| URL karakter sınırı isteyin | 16.384 karakter |
|||

<a name="retry-policy-limits"></a>

#### <a name="retry-policy"></a>Yeniden deneme ilkesi

| Adı | Sınır | Notlar |
| ---- | ----- | ----- |
| Yeniden deneme sayısı | 90 | Varsayılan değer 4'tür. Varsayılanı değiştirmek için [yeniden deneme ilkesi parametresini](../logic-apps/logic-apps-workflow-actions-triggers.md)kullanın. |
| En fazla gecikmede yeniden dene | 1 gün | Varsayılanı değiştirmek için [yeniden deneme ilkesi parametresini](../logic-apps/logic-apps-workflow-actions-triggers.md)kullanın. |
| Yeniden deneme en az gecikme süresi | 5 saniye | Varsayılanı değiştirmek için [yeniden deneme ilkesi parametresini](../logic-apps/logic-apps-workflow-actions-triggers.md)kullanın. |
||||

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Özel konektör limitleri

Web API'lerinden oluşturabileceğiniz özel bağlayıcılar için sınırlar aşağıda veda edebilirsiniz.

| Adı | Çok kiracı sınırı | Entegrasyon hizmeti ortamı sınırı | Notlar |
|------|--------------------|---------------------------------------|-------|
| Özel bağlayıcı sayısı | Azure aboneliği başına 1000 | Azure aboneliği başına 1000 ||
| Özel bir bağlayıcı için dakika başına istek sayısı | Bağlantı başına dakika başına 500 istek | *Özel bağlayıcı* başına dakikada 2.000 istek ||
|||

<a name="managed-identity"></a>

## <a name="managed-identities"></a>Yönetilen kimlikler

| Adı | Sınır |
|------|-------|
| Mantık uygulaması başına yönetilen kimlikler | Sistem tarafından atanan kimlik veya 1 kullanıcı tarafından atanan kimlik |
| Azure aboneliğinde bölge başına yönetilen kimliğe sahip mantık uygulamalarının sayısı | 250 |
|||

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Entegrasyon hesabı sınırları

Her Azure aboneliğinde şu tümleştirme hesabı sınırları vardır:

* Azure bölgesi başına bir [Ücretsiz katman](../logic-apps/logic-apps-pricing.md#integration-accounts) tümleştirme hesabı

* Geliştirici [ve Premium SK'lar](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)arasında [tümleştirme hizmeti ortamlarında (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) entegrasyon hesapları da dahil olmak üzere toplam 1.000 toplam tümleştirme hesabı.

* Geliştirici veya [Premium](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)olsun, her Ise, 5 toplam entegrasyon hesapları ile sınırlıdır:

  | İmKB SKU | Entegrasyon hesabı sınırları |
  |---------|----------------------------|
  | **Premium** | 5 toplam - [Standart](../logic-apps/logic-apps-pricing.md#integration-accounts) hesaplar sadece, ücretsiz bir Standart hesabı da dahil olmak üzere. Ücretsiz veya Temel hesaplara izin verilmez. |
  | **Geliştirici** | 5 toplam - [Ücretsiz](../logic-apps/logic-apps-pricing.md#integration-accounts) (1 hesapla sınırlıdır) ve [Standart](../logic-apps/logic-apps-pricing.md#integration-accounts) birleştirilmiş veya tüm Standart hesaplar. Temel hesaplara izin verilmez. Deneme, geliştirme ve sınama için [Geliştirici SKU'yu](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) kullanın, ancak üretim veya performans testi için kullanmayın. |
  |||

İmKB'ye dahil olan tümleştirme hesaplarının ötesine eklediğiniz tümleştirme hesapları için ek maliyetler uygulanır. ISE'ler için fiyatlandırma ve faturalandırmanın nasıl çalıştığını öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](https://azure.microsoft.com/pricing/details/logic-apps/)bakın.

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Tümleştirme hesabı başına yapı sınırları

Burada, her tümleştirme hesabı katmanı için yapı ların sayısının sınırları vesi,
Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](https://azure.microsoft.com/pricing/details/logic-apps/)bakın. Tümleştirme hesapları için fiyatlandırma ve faturalandırmanın nasıl çalıştığını öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#integration-accounts)bakın.

> [!NOTE]
> Serbest katmanı üretim senaryoları için değil, yalnızca araştırmacı senaryolar için kullanın. Bu katman, iş verime ve kullanımı kısıtlar ve hizmet düzeyi sözleşmesi (SLA) yoktur.

| Yapıt | Ücretsiz | Temel | Standart |
|----------|------|-------|----------|
| EDI ticaret anlaşmaları | 10 | 1 | 1000 |
| EDI ticaret ortakları | 25 | 2 | 1000 |
| Haritalar | 25 | 500 | 1000 |
| Şemalar | 25 | 500 | 1000 |
| Bütünleştirilmiş kodlar | 10 | 25 | 1000 |
| Sertifikalar | 25 | 2 | 1000 |
| Toplu iş yapılandırmaları | 5 | 1 | 50 |
||||

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Artefakt kapasite sınırları

| Yapıt | Sınır | Notlar |
| -------- | ----- | ----- |
| Derleme | 8 MB | 2 MB'dan büyük dosyaları yüklemek için bir [Azure depolama hesabı ve blob kapsayıcısı](../logic-apps/logic-apps-enterprise-integration-schemas.md)kullanın. |
| Harita (XSLT dosyası) | 8 MB | 2 MB'dan büyük dosyaları yüklemek için [Azure Logic Apps REST API - Haritalar'ı](https://docs.microsoft.com/rest/api/logic/maps/createorupdate)kullanın. <p><p>**Not**: Bir haritanın başarıyla işleyebilir veri veya kayıt miktarı, Azure Logic Apps'taki ileti boyutu ve eylem zaman ayarı sınırlarını temel almaktadır. Örneğin, [HTTP ileti boyutu ve zaman ayarı sınırlarına](#request-limits)dayalı bir HTTP eylemi kullanırsanız, işlem HTTP zaman sınırı içinde tamamlanırsa, bir harita verileri HTTP ileti boyutu sınırına kadar işleyebilir. |
| Şema | 8 MB | 2 MB'dan büyük dosyaları yüklemek için bir [Azure depolama hesabı ve blob kapsayıcısı](../logic-apps/logic-apps-enterprise-integration-schemas.md)kullanın. |
||||

<a name="integration-account-throughput-limits"></a>

### <a name="throughput-limits"></a>İşleme sınırları

| Çalışma zamanı bitiş noktası | Ücretsiz | Temel | Standart | Notlar |
|------------------|------|-------|----------|-------|
| Aramaları 5 dakikada okuyun | 3,000 | 30,000 | 60.000 | İş yükünü gerektiğinde birden fazla hesaba dağıtabilirsiniz. |
| 5 dakikada çağrı çağırma | 3,000 | 30,000 | 45.000 | İş yükünü gerektiğinde birden fazla hesaba dağıtabilirsiniz. |
| Aramaları 5 dakikada izleme | 3,000 | 30,000 | 45.000 | İş yükünü gerektiğinde birden fazla hesaba dağıtabilirsiniz. |
| Eşzamanlı çağrıları engelleme | ~1.000 | ~1.000 | ~1.000 | Tüm SUS'lar için de aynı. Eşzamanlı istek sayısını azaltabilir veya gerektiğinde süreyi azaltabilirsiniz. |
||||

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>B2B protokolü (AS2, X12, EDIFACT) ileti boyutu

B2B protokolleri için geçerli ileti boyutu sınırları şunlardır:

| Adı | Çok kiracı sınırı | Entegrasyon hizmeti ortamı sınırı | Notlar |
|------|--------------------|---------------------------------------|-------|
| AS2 | v2 - 100 MB<br>v1 - 50 MB | v2 - 200 MB <br>v1 - 50 MB | Decode ve kodlamak için geçerlidir |
| X12 | 50 MB | 50 MB | Decode ve kodlamak için geçerlidir |
| EDIFACT | 50 MB | 50 MB | Decode ve kodlamak için geçerlidir |
||||

<a name="disable-delete"></a>

## <a name="disabling-or-deleting-logic-apps"></a>Mantık uygulamalarını devre dışı bırakma veya silme

Bir mantık uygulamasını devre dışı dışı bettiğinizde, hiçbir yeni çalıştırma anında yapılmaz. Devam eden ve bekleyen tüm çalıştırmalar bitene kadar devam ediyor ve bu da tamamlanması zaman alabilir.

Mantıksal uygulamayı sildiğinizde yeni çalıştırma başlatılmaz. Devam eden ve bekleme durumunda olan tüm çalıştırmalar iptal edilir. Binlerce çalıştırma varsa iptal işleminin tamamlanması zaman alabilir.

<a name="configuration"></a>

## <a name="firewall-configuration-ip-addresses-and-service-tags"></a>Güvenlik duvarı yapılandırması: IP adresleri ve hizmet etiketleri

Azure Logic Apps'ın gelen ve giden aramalar için kullandığı IP adresleri, mantık uygulamanızın bulunduğu bölgeye bağlıdır. Aynı bölgedeki *tüm* mantık uygulamaları aynı IP adresi aralıklarını kullanır. **HTTP** ve **HTTP + OpenAPI** istekleri gibi bazı Power [Otomatikleştir](https://docs.microsoft.com/power-automate/getting-started) çağrıları doğrudan Azure Logic Apps hizmetinden geçer ve burada listelenen IP adreslerinden gelir. Power Automate tarafından kullanılan IP adresleri hakkında daha fazla bilgi için [Power Automate'deki Sınırlar ve yapılandırmaya](https://docs.microsoft.com/flow/limits-and-config#ip-address-configuration)bakın.

> [!TIP]
> Güvenlik kuralları oluştururken karmaşıklığı azaltmaya yardımcı olmak için, bu bölümde daha sonra açıklanan her bölge için Logic Apps IP adreslerini belirtmek yerine isteğe bağlı olarak [hizmet etiketlerini](../virtual-network/service-tags-overview.md)kullanabilirsiniz. Bu etiketler, Logic Apps hizmetinin kullanılabildiği bölgelerde çalışır:
>
> * **LogicAppsManagement**: Logic Apps hizmeti için gelen IP adresi önekleri temsil eder.
> * **LogicApps**: Logic Apps hizmetinin giden IP adresi önekleri temsil eder.

* [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)için, sabit veya ayrılmış IP adresleri [özel bağlayıcılar](../logic-apps/custom-connector-overview.md) ve [yönetilen bağlayıcılar](../connectors/apis-list.md#managed-api-connectors)(örneğin, Azure Depolama, SQL Server, Office 365 Outlook) için kullanılamaz.

* Mantık uygulamalarınızın doğrudan [HTTP](../connectors/connectors-native-http.md), HTTP [+ Swagger](../connectors/connectors-native-http-swagger.md)ve diğer HTTP istekleriyle yaptığı çağrıları desteklemek için, mantık uygulamalarınızın bulunduğu bölgelere bağlı olarak Mantık Uygulamaları hizmeti tarafından kullanılan tüm [gelen](#inbound) *ve* [giden](#outbound) IP adresleriyle güvenlik duvarınızı ayarlayın. Bu adresler bu bölümdeki **Gelen** ve **Giden** başlıklar altında görünür ve bölgeye göre sıralanır.

* [Yönetilen bağlayıcıların](../connectors/apis-list.md#managed-api-connectors) yaptığı aramaları desteklemek için, mantık uygulamalarınızın bulunduğu bölgelere bağlı olarak güvenlik duvarınızı bu bağlayıcılar tarafından kullanılan *tüm* [giden](#outbound) IP adresleriyle ayarlayın. Bu adresler bu bölümdeki **Giden başlık** altında görünür ve bölgeye göre sıralanır.

* Entegrasyon hizmeti ortamında (İmKB) çalışan mantık uygulamaları için iletişimi etkinleştirmek [için, bu bağlantı noktalarını açtığınıza](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#network-ports-for-ise)emin olun.

* Mantıksal uygulamalarınızda güvenlik duvarı ve güvenlik [duvarı kuralları](../storage/common/storage-network-security.md)kullanan Azure depolama hesaplarına erişmekte sorun yaşıyorsanız, erişimi [etkinleştirmek için çeşitli seçenekleriniz](../connectors/connectors-create-api-azureblobstorage.md#access-storage-accounts-behind-firewalls)vardır.

  Örneğin, mantık uygulamaları güvenlik duvarı kurallarını kullanan ve aynı bölgede bulunan depolama hesaplarına doğrudan erişemez. Ancak, [bölgenizdeki yönetilen bağlayıcılar için giden IP adreslerine](../logic-apps/logic-apps-limits-and-config.md#outbound)izin verirseniz, mantıksal uygulamalarınız Azure Tablo Depolama alanı veya Azure Sıra Depolama bağlayıcılarını kullanmanız dışında farklı bir bölgede bulunan depolama hesaplarına erişebilir. Tablo Depolama veya Sıra Depolama'nıza erişmek için, bunun yerine HTTP tetikleyicisini ve eylemlerini kullanabilirsiniz. Diğer seçenekler için, [güvenlik duvarlarının arkasındaki Access depolama hesaplarına](../connectors/connectors-create-api-azureblobstorage.md#access-storage-accounts-behind-firewalls)bakın.

<a name="inbound"></a>

### <a name="inbound-ip-addresses"></a>Gelen IP adresleri

Bu bölümde yalnızca Azure Logic Apps hizmetiiçin gelen IP adresleri listelenebilmiştir. Azure Kamu'nuz [varsa,](#azure-government-inbound)bkz.

> [!TIP]
> Güvenlik kuralları oluştururken karmaşıklığı azaltmaya yardımcı olmak için, her bölge için gelen Logic Apps IP adresi önekleri belirtmek yerine [isteğe](../virtual-network/service-tags-overview.md)bağlı olarak hizmet etiketi olan **LogicAppsManagement'ı**kullanabilirsiniz. Bu etiket, Logic Apps hizmetinin kullanılabildiği bölgelerde çalışır.

<a name="multi-tenant-inbound"></a>

#### <a name="multi-tenant-azure---inbound-ip-addresses"></a>Çok kiracılı Azure - Gelen IP adresleri

| Çok kiracılı bölge | IP |
|---------------------|----|
| Doğu Avustralya | 13.75.153.66, 104.210.89.222, 104.210.89.244, 52.187.231.161 |
| Güneydoğu Avustralya | 13.73.115.153, 40.115.78.70, 40.115.78.237, 52.189.216.28 |
| Güney Brezilya | 191.235.86.199, 191.235.95.229, 191.235.94.220, 191.234.166.198 |
| Orta Kanada | 13.88.249.209, 52.233.30.218, 52.233.29.79, 40.85.241.105 |
| Doğu Kanada | 52.232.129.143, 52.229.125.57, 52.232.133.109, 40.86.202.42 |
| Orta Hindistan | 52.172.157.194, 52.172.184.192, 52.172.191.194, 104.211.73.195 |
| Orta ABD | 13.67.236.76, 40.77.111.254, 40.77.31.87, 104.43.243.39 |
| Doğu Asya | 168.63.200.173, 13.75.89.159, 23.97.68.172, 40.83.98.194 |
| Doğu ABD | 137.135.106.54, 40.117.99.79, 40.117.100.228, 137.116.126.165 |
| Doğu ABD 2 | 40.84.25.234, 40.79.44.7, 40.84.59.136, 40.70.27.253 |
| Orta Fransa | 52.143.162.83, 20.188.33.169, 52.143.156.55, 52.143.158.203 |
| Güney Fransa | 52.136.131.145, 52.136.129.121, 52.136.130.89, 52.136.131.4 |
| Doğu Japonya | 13.71.146.140, 13.78.84.187, 13.78.62.130, 13.78.43.164 |
| Batı Japonya | 40.74.140.173, 40.74.81.13, 40.74.85.215, 40.74.68.85 |
| Güney Kore - Orta | 52.231.14.182, 52.231.103.142, 52.231.39.29, 52.231.14.42 |
| Güney Kore - Güney | 52.231.166.168, 52.231.163.55, 52.231.163.150, 52.231.192.64 |
| Orta Kuzey ABD | 168.62.249.81, 157.56.12.202, 65.52.211.164, 65.52.9.64 |
| Kuzey Avrupa | 13.79.173.49, 52.169.218.253, 52.169.220.174, 40.112.90.39 |
| Güney Afrika Kuzey | 102.133.228.4, 102.133.224.125, 102.133.226.199, 102.133.228.9 |
| Güney Afrika Batı | 102.133.72.190, 102.133.72.145, 102.133.72.184, 102.133.72.173 |
| Orta Güney ABD | 13.65.98.39, 13.84.41.46, 13.84.43.45, 40.84.138.132 |
| Güney Hindistan | 52.172.9.47, 52.172.49.43, 52.172.51.140, 104.211.225.152 |
| Güneydoğu Asya | 52.163.93.214, 52.187.65.81, 52.187.65.155, 104.215.181.6 |
| BAE Merkez | 20.45.75.193, 20.45.64.29, 20.45.64.87, 20.45.71.213 |
| Güney Birleşik Krallık | 51.140.79.109, 51.140.78.71, 51.140.84.39, 51.140.155.81 |
| Batı Birleşik Krallık | 51.141.48.98, 51.141.51.145, 51.141.53.164, 51.141.119.150 |
| Orta Batı ABD | 52.161.26.172, 52.161.8.128, 52.161.19.82, 13.78.137.247 |
| Batı Avrupa | 13.95.155.53, 52.174.54.218, 52.174.49.6, 52.174.49.6 |
| Batı Hindistan | 104.211.164.112, 104.211.165.81, 104.211.164.25, 104.211.157.237 |
| Batı ABD | 52.160.90.237, 138.91.188.137, 13.91.252.184, 157.56.160.212 |
| Batı ABD 2 | 13.66.224.169, 52.183.30.10, 52.183.39.67, 13.66.128.68 |
|||

<a name="azure-government-inbound"></a>

#### <a name="azure-government---inbound-ip-addresses"></a>Azure Genel - Gelen IP adresleri

| Azure Devlet bölgesi | IP |
|-------------------------|----|
| US Gov Arizona | 52.244.67.164, 52.244.67.64, 52.244.66.82 |
| US Gov Texas | 52.238.119.104, 52.238.112.96, 52.238.119.145 |
| US Gov Virginia | 52.227.159.157, 52.227.152.90, 23.97.4.36 |
| Orta US DoD | 52.182.49.204, 52.182.52.106 |
|||

<a name="outbound"></a>

### <a name="outbound-ip-addresses"></a>Giden IP adresleri

Bu bölümde, Azure Logic Apps hizmetinin giden IP adresleri ve yönetilen bağlayıcılar listelenebilmiştir. Azure Kamu'nuz [varsa,](#azure-government-outbound)bkz.

> [!TIP]
> Güvenlik kuralları oluştururken karmaşıklığı azaltmaya yardımcı olmak için, her bölge için giden Logic Apps IP adresi önekleri belirtmek yerine isteğe bağlı olarak [hizmet etiketiolan](../virtual-network/service-tags-overview.md) **LogicApps'ı**kullanabilirsiniz. Bu etiket, Logic Apps hizmetinin kullanılabildiği bölgelerde çalışır. Yönetilen bağlayıcılar için IP adreslerini kullanmaya devam etmelisiniz.

<a name="multi-tenant-outbound"></a>

#### <a name="multi-tenant-azure---outbound-ip-addresses"></a>Çok kiracılı Azure - Giden IP adresleri

| Çok kiracılı bölge | Mantık Uygulamaları IP | Yönetilen konektörler IP |
|---------------------|---------------|-----------------------|
| Doğu Avustralya | 13.75.149.4, 104.210.91.55, 104.210.90.241, 52.187.227.245, 52.187.226.96, 52.187.231.184, 52.187.229.130, 52.187.226.139 | 13.70.72.192 - 13.70.72.207, 13.72.243.10, 40.126.251.213, 52.237.214.72 |
| Güneydoğu Avustralya | 13.73.114.207, 13.77.3.139, 13.70.159.205, 52.189.222.77, 13.77.56.167, 13.77.58.136, 52.189.214.42, 52.189.220.75 | 13.70.136.174, 13.77.50.240 - 13.77.50.255, 40.127.80.34, 52.255.48.202 |
| Güney Brezilya | 191.235.82.221, 191.235.91.7, 191.234.182.26, 191.237.255.116, 191.234.161.168, 191.234.162.178, 191.234.161.28, 191.234.162.131 | 104.41.59.51, 191.232.38.129, 191.233.203.192 - 191.233.203.207, 191.232.191.157 |
| Orta Kanada | 52.233.29.92, 52.228.39.241, 52.228.39.244, 40.85.250.135, 40.85.250.212, 13.71.186.1, 40.85.252.47, 13.71.184.150 | 13.71.170.208 - 13.71.170.223, 52.228.33.76, 52.228.34.13, 52.228.42.205, 52.233.31.197, 52.237.24.126, 52.237.32.212 |
| Doğu Kanada | 52.232.128.155, 52.229.120.45, 52.229.126.25, 40.86.203.228, 40.86.228.93, 40.86.216.241, 40.86.226.149, 40.86.217.241 | 40.69.106.240 - 40.69.106.255, 52.229.120.52, 52.229.120.178, 52.229.123.98, 52.229.126.202, 52.242.35.152, 52.242.30.112 |
| Orta Hindistan | 52.172.154.168, 52.172.186.159, 52.172.185.79, 104.211.101.108, 104.211.102.62, 104.211.90.169, 104.211.90.162, 104.211.74.145 | 52.172.211.12, 104.211.81.192 - 104.211.81.207, 104.211.98.164, 52.172.212.129 |
| Orta ABD | 13.67.236.125, 104.208.25.27, 40.122.170.198, 40.113.218.230, 23.100.86.139, 23.100.87.24, 23.100.87.56, 23.100.82.16 | 13.89.171.80 - 13.89.171.95, 40.122.49.51, 52.173.245.164, 52.173.241.27 |
| Doğu Asya | 13.75.94.173, 40.83.127.19, 52.175.33.254, 40.83.73.39, 65.52.175.34, 40.83.77.208, 40.83.100.69, 40.83.75.165 | 13.75.36.64 - 13.75.36.79, 23.99.116.181, 52.175.23.169, 13.75.110.131 |
| Doğu ABD | 13.92.98.111, 40.121.91.41, 40.114.82.191, 23.101.139.153, 23.100.29.190, 23.101.136.201, 104.45.153.81, 23.101.132.208 | 40.71.11.80 - 40.71.11.95, 40.71.249.205, 40.114.40.132, 40.71.249.139 |
| Doğu ABD 2 | 40.84.30.147, 104.208.155.200, 104.208.158.174, 104.208.140.40, 40.70.131.151, 40.70.29.214, 40.70.26.154, 40.70.27.236 | 40.70.146.208 - 40.70.146.223, 52.232.188.154, 104.208.233.100, 104.209.247.23, 52.225.129.144 |
| Orta Fransa | 52.143.164.80, 52.143.164.15, 40.89.186.30, 20.188.39.105, 40.89.191.161, 40.89.188.169, 40.89.186.28, 40.89.190.104 | 40.79.130.208 - 40.79.130.223, 40.89.135.2, 40.89.186.239 |
| Güney Fransa | 52.136.132.40, 52.136.129.89, 52.136.131.155, 52.136.133.62, 52.136.139.225, 52.136.130.144, 52.136.140.226, 52.136.129.51 | 40.79.178.240 - 40.79.178.255, 52.136.133.184, 52.136.142.154 |
| Doğu Japonya | 13.71.158.3, 13.73.4.207, 13.71.158.120, 13.78.18.168, 13.78.35.229, 13.78.42.223, 13.78.21.155, 13.78.20.232 | 13.71.153.19, 13.78.108.0 - 13.78.108.15, 40.115.186.96, 13.73.21.230 |
| Batı Japonya | 40.74.140.4, 104.214.137.243, 138.91.26.45, 40.74.64.207, 40.74.76.213, 40.74.77.205, 40.74.74.21, 40.74.68.85 | 40.74.100.224 - 40.74.100.239, 40.74.130.77, 104.215.61.248, 104.215.27.24 |
| Güney Kore - Orta | 52.231.14.11, 52.231.14.219, 52.231.15.6, 52.231.10.111, 52.231.14.223, 52.231.77.107, 52.231.8.175, 52.231.9.39 | 52.231.18.208 - 52.231.18.223, 52.141.36.214, 52.141.1.104 |
| Güney Kore - Güney | 52.231.204.74, 52.231.188.115, 52.231.189.221, 52.231.203.118, 52.231.166.28, 52.231.153.89, 52.231.155.206, 52.231.164.23 | 52.231.147.0 - 52.231.147.15, 52.231.163.10, 52.231.201.173 |
| Orta Kuzey ABD | 168.62.248.37, 157.55.210.61, 157.55.212.238, 52.162.208.216, 52.162.213.231, 65.52.10.183, 65.52.9.96, 65.52.8.225 | 52.162.107.160 - 52.162.107.175, 52.162.242.161, 65.52.218.230, 52.162.126.4 |
| Kuzey Avrupa | 40.113.12.95, 52.178.165.215, 52.178.166.21, 40.112.92.104, 40.112.95.216, 40.113.4.18, 40.113.3.202, 40.113.1.181 | 13.69.227.208 - 13.69.227.223, 52.178.150.68, 104.45.93.9, 94.245.91.93, 52.169.28.181 |
| Güney Afrika Kuzey | 102.133.231.188, 102.133.231.117, 102.133.230.4, 102.133.227.103, 102.133.228.6, 102.133.230.82, 102.133.231.9, 102.133.231.51 | 13.65.86.57, 104.214.19.48 - 104.214.19.63, 104.214.70.191, 102.133.168.167 |
| Güney Afrika Batı | 102.133.72.98, 102.133.72.113, 102.133.75.169, 102.133.72.179, 102.133.72.37, 102.133.72.183, 102.133.72.132, 102.133.75.191 | 13.65.86.57, 104.214.19.48 - 104.214.19.63, 104.214.70.191, 102.133.72.85 |
| Orta Güney ABD | 104.210.144.48, 13.65.82.17, 13.66.52.232, 23.100.124.84, 70.37.54.122, 70.37.50.6, 23.100.127.172, 23.101.183.225 | 13.65.86.57, 104.214.19.48 - 104.214.19.63, 104.214.70.191, 52.171.130.92 |
| Güney Hindistan | 52.172.50.24, 52.172.55.231, 52.172.52.0, 104.211.229.115, 104.211.230.129, 104.211.230.126, 104.211.231.39, 104.211.227.229 | 13.71.125.22, 40.78.194.240 - 40.78.194.255, 104.211.227.225, 13.71.127.26 |
| Güneydoğu Asya | 13.76.133.155, 52.163.228.93, 52.163.230.166, 13.76.4.194, 13.67.110.109, 13.67.91.135, 13.76.5.96, 13.67.107.128 | 13.67.8.240 - 13.67.8.255, 13.76.231.68, 52.187.68.19, 52.187.115.69 |
| BAE Merkez | 20.45.75.200, 20.45.72.72, 20.45.75.236, 20.45.79.239, 20.45.67.170, 20.45.72.54, 20.45.67.134, 20.45.67.135 | 20.45.67.28, 20.45.67.45, 20.37.74.192 - 20.37.74.207, 40.120.8.0 - 40.120.8.31 |
| Güney Birleşik Krallık | 51.140.74.14, 51.140.73.85, 51.140.78.44, 51.140.137.190, 51.140.153.135, 51.140.28.225, 51.140.142.28, 51.140.158.24 | 51.140.80.51, 51.140.148.0 - 51.140.148.15, 51.140.61.124, 51.140.74.150 |
| Batı Birleşik Krallık | 51.141.54.185, 51.141.45.238, 51.141.47.136, 51.141.114.77, 51.141.112.112, 51.141.113.36, 51.141.118.119, 51.141.119.63 | 51.140.211.0 - 51.140.211.15, 51.141.47.105, 51.141.124.13, 51.141.52.185 |
| Orta Batı ABD | 52.161.27.190, 52.161.18.218, 52.161.9.108, 13.78.151.161, 13.78.137.179, 13.78.148.140, 13.78.129.20, 13.78.141.75 | 13.71.195.32 - 13.71.195.47, 52.161.102.22, 13.78.132.82, 52.161.101.204 |
| Batı Avrupa | 40.68.222.65, 40.68.209.23, 13.95.147.65, 23.97.218.130, 51.144.182.201, 23.97.211.179, 104.45.9.52, 23.97.210.126 | 13.69.64.208 - 13.69.64.223, 40.115.50.13, 52.174.88.118, 40.91.208.65, 52.166.78.89 |
| Batı Hindistan | 104.211.164.80, 104.211.162.205, 104.211.164.136, 104.211.158.127, 104.211.156.153, 104.211.158.123, 104.211.154.59, 104.211.154.7 | 104.211.146.224 - 104.211.146.239, 104.211.161.203, 104.211.189.218, 104.211.189.124 |
| Batı ABD | 52.160.92.112, 40.118.244.241, 40.118.241.243, 157.56.162.53, 157.56.167.147, 104.42.49.145, 40.83.164.80, 104.42.38.32 | 40.112.243.160 - 40.112.243.175, 104.40.51.248, 104.42.122.49, 40.112.195.87, 13.93.148.62 |
| Batı ABD 2 | 13.66.210.167, 52.183.30.169, 52.183.29.132, 13.66.210.167, 13.66.201.169, 13.77.149.159, 52.175.198.132, 13.66.246.219 | 13.66.140.128 - 13.66.140.143, 52.183.78.157, 52.191.164.250 |
||||

<a name="azure-government-outbound"></a>

#### <a name="azure-government---outbound-ip-addresses"></a>Azure Kamu - Giden IP adresleri

| Bölge | Mantık Uygulamaları IP | Yönetilen konektörler IP |
|--------|---------------|-----------------------|
| US Gov Arizona | 52.244.67.143, 52.244.65.66, 52.244.65.190 | 52.127.2.160 - 52.127.2.175, 52.244.69.0, 52.244.64.91 |
| US Gov Texas | 52.238.114.217, 52.238.115.245, 52.238.117.119 | 52.127.34.160 - 52.127.34.175, 40.112.40.25, 52.238.161.225 |
| US Gov Virginia | 13.72.54.205, 52.227.138.30, 52.227.152.44 | 52.127.42.128 - 52.127.42.143, 52.227.143.61, 52.227.162.91 |
| Orta US DoD | 52.182.48.215, 52.182.92.143 | 52.127.58.160 - 52.127.58.175, 52.182.54.8, 52.182.48.136 |
||||

## <a name="next-steps"></a>Sonraki adımlar

* [İlk mantık uygulamanızı](../logic-apps/quickstart-create-first-logic-app-workflow.md) nasıl oluşturabilirsiniz öğrenin  
* Yaygın [örnekler ve senaryolar](../logic-apps/logic-apps-examples-and-scenarios.md) hakkında bilgi edinin
