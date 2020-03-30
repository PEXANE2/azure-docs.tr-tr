---
title: Azure Log Analytics'te depolanan kişisel veriler için kılavuz| Microsoft Dokümanlar
description: Bu makalede, Azure Log Analytics'te depolanan kişisel verilerin nasıl yönetilenve bunları tanımlama ve kaldırma yöntemleri açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/18/2018
ms.openlocfilehash: a720627e1783d2e29ef180b7855132ea59444cab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248755"
---
# <a name="guidance-for-personal-data-stored-in-log-analytics-and-application-insights"></a>Log Analytics ve Application Insights'ta saklanan kişisel veriler için yönergeler

Log Analytics, kişisel verilerin bulunma olasılığının yüksek olduğu bir veri deposudur. Application Insights verilerini Log Analytics bölümüne saklar. Bu makalede, Log Analytics ve Application Insights'ta bu tür verilerin genellikle nerede bulunduğu ve bu tür verileri işlemek için kullanabileceğiniz yetenekler tartışılır.

> [!NOTE]
> Bu makalenin amaçları doğrultusunda _günlük verileri,_ Log Analytics çalışma alanına gönderilen veriler anlamına gelirken, _uygulama verileri_ Application Insights tarafından toplanan veriler anlamına gelir.

[!INCLUDE [gdpr-dsr-and-stp-note](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Kişisel veri işleme stratejisi

Sonuçta özel verilerinizi işleyeceğiniz stratejiyi belirlemek size ve şirketinize kalmış olsa da (eğer hiç varsa), aşağıdaki bazı olası yaklaşımlardır. Teknik açıdan en az tercih edilene kadar tercih sırasına göre listelenirler:

* Mümkün olduğunda, toplanan verilerin toplanmasını durdurun, gizlemeyi, anonimleştirin veya toplanan verileri "özel" olarak kabul edilmekten dışlayacak şekilde ayarlayın. Bu, çok pahalı ve etkili bir veri işleme stratejisi oluşturma gereksinimini _kurtararak, tercih_ edilen yaklaşımdır.
* Mümkün olmadığı durumlarda, veri platformu ve performans üzerindeki etkisini azaltmak için verileri normalleştirmeye çalışın. Örneğin, açık bir Kullanıcı Kimliği günlüğe kaydetmek yerine, kullanıcı adını ve ayrıntılarını başka bir yerde günlüğe kaydedilebilen dahili bir kimlikle ilişkilendirecek bir arama verileri oluşturun. Bu şekilde, kullanıcınızdan biri sizden kişisel bilgilerini silmenizi isterse, yalnızca kullanıcıya karşılık gelen arama tablosundaki satırı silmenin yeterli olması mümkündür. 
* Son olarak, özel verilerin toplanması gerekiyorsa, bir kullanıcıyla ilişkili özel verileri dışa aktarma ve silme ile ilgili yükümlülüklerinizin yerine getirilmesi için temizleme API yolu ve varolan sorgu API yolu etrafında bir işlem oluşturun. 

## <a name="where-to-look-for-private-data-in-log-analytics"></a>Log Analytics'te özel verilere nerede bakın?

Log Analytics, verilerinize şema yazarken her alanı özel değerlerle geçersiz kılmanızı sağlayan esnek bir depodur. Ayrıca, herhangi bir özel şema yutulabilir. Bu nedenle, özel verilerin belirli çalışma alanınızda tam olarak nerede bulunacağını söylemek mümkün değildir. Ancak aşağıdaki konumlar, envanterinizdeki iyi başlangıç noktalarıdır:

### <a name="log-data"></a>Günlük verileri

* *IP adresleri*: Log Analytics birçok farklı tabloda çeşitli IP bilgileri toplar. Örneğin, aşağıdaki sorgu, IPv4 adreslerinin son 24 saat içinde toplandığı tüm tabloları gösterir:
    ```
    search * 
    | where * matches regex @'\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.|$)){4}\b' //RegEx originally provided on https://stackoverflow.com/questions/5284147/validating-ipv4-addresses-with-regexp
    | summarize count() by $table
    ```
* *Kullanıcı II'leri*: Kullanıcı işlleri çoğunlukta çüpte bir çüpte çüpte bir çüpka çÜ Arama komutunu kullanarak tüm veri setinizde belirli bir kullanıcı adını arayabilirsiniz:
    ```
    search "[username goes here]"
    ```
  Sadece insan tarafından okunabilir kullanıcı adları için değil, aynı zamanda doğrudan belirli bir kullanıcıya geri izlenebilir GUIDs için bakmak unutmayın!
* *Aygıt TBm'leri*: Kullanıcı t,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,, Bunun sorun olabileceği tabloları tanımlamak için kullanıcı kimliklerinin yukarıda listelendiği yaklaşımı kullanın. 
* *Özel veriler*: Log Analytics, özel günlükler ve özel alanlar, [HTTP Veri Toplayıcıapi](../../azure-monitor/platform/data-collector-api.md) ve sistem olay günlüklerinin bir parçası olarak toplanan özel veriler gibi çeşitli yöntemlerle toplamayı sağlar. Bunların tümü özel veri içermeye açıktır ve bu tür verilerin var olup olmadığını doğrulamak için incelenmelidir.
* *Çözümle yakalanan veriler*: Çözüm mekanizması açık uçlu olduğundan, uyumluluğu sağlamak için çözümler tarafından oluşturulan tüm tabloları gözden geçirmenizi öneririz.

### <a name="application-data"></a>Uygulama verileri

* *IP adresleri*: Application Insights varsayılan olarak tüm IP adres alanlarını "0.0.0.0" olarak gizlemekle birlikte, oturum bilgilerini korumak için gerçek kullanıcı IP'si ile bu değeri geçersiz kılmak oldukça yaygın bir desendir. Aşağıdaki Analytics sorgusu, IP adresi sütununda son 24 saat içinde "0.0.0.0" dışında değerler içeren herhangi bir tabloyu bulmak için kullanılabilir:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Kullanıcı Bilgileri*: Varsayılan olarak, Application Insights kullanıcı ve oturum izleme için rasgele oluşturulan t'ler kullanır. Ancak, uygulamayla daha alakalı bir kimlik depolamak için bu alanların geçersiz kılınmasını görmek yaygındır. Örneğin: kullanıcı adları, AAD GUIDs, vb. Bu kişiler genellikle kişisel veri olarak kapsam içi olarak kabul edilir ve bu nedenle uygun şekilde ele alınmalıdır. Tavsiyemiz her zaman bu tadilleri gizlemek veya anonim hale getirmektir. Bu değerlerin yaygın olarak bulunduğu alanlar arasında session_Id, user_Id, user_AuthenticatedId, user_AccountId yanı sıra özel Boyutlar yer alır.
* *Özel veriler*: Application Insights, herhangi bir veri türüne bir dizi özel boyut eklenenize olanak tanır. Bu boyutlar *herhangi bir* veri olabilir. Son 24 saat içinde toplanan özel boyutları tanımlamak için aşağıdaki sorguyu kullanın:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Bellek içi ve aktarım verileri*: Application Insights özel durumları, istekleri, bağımlılık çağrılarını ve izlemeleri izler. Özel veriler genellikle kod ve HTTP çağrı düzeyinde toplanabilir. Bu tür verileri tanımlamak için özel durumları, istekleri, bağımlılıkları ve izleme tablolarını gözden geçirin. Bu verileri gizlemek için mümkün [sevese telemetri başharflerini](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) kullanın.
* *Anlık Görüntü Hata Ayıklama yakalar:* Uygulama Öngörüleri'ndeki Anlık Görüntü [Hata Ayıklama](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) özelliği, uygulamanızın üretim örneğinde bir özel durum yakalandığında hata ayıklama anlık görüntülerini toplamanızı sağlar. Anlık görüntüler, yığındaki her adımda yerel değişkenlerin değerlerinin yanı sıra özel durumlara giden tam yığın izini de ortaya çıkarır. Ne yazık ki, bu özellik anlık noktaların seçici olarak silinmesine veya anlık görüntü içindeki verilere programlı erişime izin vermez. Bu nedenle, varsayılan anlık görüntü tutma oranı uyumluluk gereksinimlerinizi karşılamazsa, öneri özelliği kapatmaktır.

## <a name="how-to-export-and-delete-private-data"></a>Özel verilerin dışa aktarılması ve silinmesi

Daha önce kişisel veri işleme bölümü [için stratejide](#strategy-for-personal-data-handling) belirtildiği gibi, mümkünse, özel verilerin toplanmasını devre dışı kakmak, gizleme veya anonimleştirme veya başka bir şekilde "özel" olarak kabul edilmesinden kaldırmak için değiştirerek veri toplama ilkenizi yeniden yapılandırmanız __önemle__ tavsiye edilir. Verilerin işlenmesi, en önemlisi, bir strateji tanımlamanız ve otomatikleştirmeniz, müşterilerinizin verileriyle etkileşimkurabilmek için bir arayüz oluşturmanız ve devam eden bakım maliyetleri için size ve ekibinize maliyetler le sonuçlanır. Ayrıca, Log Analytics ve Application Insights için hesaplama açısından maliyetlidir ve çok büyük miktarda eşzamanlı sorgu veya temizleme API çağrıları, Log Analytics işleviyle olan diğer tüm etkileşimleri olumsuz etkileme potansiyeline sahiptir. Bununla, özel verilerin toplanması gereken bazı geçerli senaryolar gerçekten de vardır. Bu gibi durumlarda, veriler bu bölümde açıklandığı gibi ele alınmalıdır.

[!INCLUDE [gdpr-intro-sentence](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Görüntüleme ve dışa aktarma

Hem görünüm hem de veri istekleri için [Log Analytics sorgu API'si](https://dev.loganalytics.io/) veya [Application Insights sorgu API'si](https://dev.applicationinsights.io/quickstart) kullanılmalıdır. Mantığı kullanıcılarınıza sunmak için uygun bir veri şekli dönüştürmek için uygulamak için size kalmış. [Azure İşlevler,](https://azure.microsoft.com/services/functions/) bu tür bir mantığı barındırmak için harika bir yer dir.

> [!IMPORTANT]
>  Tasfiye operasyonlarının büyük çoğunluğu SLA'dan çok daha hızlı tamamlanabilirken, **tasfiye operasyonlarının tamamlanması için resmi SLA,** kullanılan veri platformu üzerindeki ağır etkileri nedeniyle 30 gün olarak belirlenmiştir. Bu otomatik bir işlemdir; bir işlemin daha hızlı yürütülmesini istemenin bir yolu yoktur.

### <a name="delete"></a>Sil

> [!WARNING]
> Log Analytics'teki silmeler yıkıcıdır ve geri döndürülemez! Lütfen idamlarında çok dikkatli olun.

Bir *tasfiye* API yolunu niçin kullandığınızı bir gizlilik kapsamında kullanıma sunmuş bulunuyoruz. Bu yol, bunu yaparken ortaya çıkan risk, potansiyel performans etkisi ve tüm toplulaştırmaları, ölçümleri ve Log Analytics verilerinizin diğer yönlerini çarpıtma potansiyeli nedeniyle dikkatli bir şekilde kullanılmalıdır. Özel [verileri](#strategy-for-personal-data-handling) işlemek için alternatif yaklaşımlar için kişisel veri işleme stratejisi bölümüne bakın.

Temizleme, Azure'daki hiçbir uygulamanın veya kullanıcının (kaynak sahibi dahil) Azure Kaynak Yöneticisi'nde açıkça bir rol verilmeden yürütme iznine sahip olmayacağı son derece ayrıcalıklı bir işlemdir. Bu rol _Data Purger'dır_ ve veri kaybı potansiyeli nedeniyle ihtiyatlı bir şekilde görevlendirilmelidir. 

> [!IMPORTANT]
> Sistem kaynaklarını yönetmek için, temizleme istekleri saatte 50 istekle azaltılır. Yüklemi tasfiye gerektiren tüm kullanıcı kimliklerini içeren tek bir komut göndererek temizleme isteklerinin yürütülmesini toplu olarak kullanmalısınız. Birden çok kimlik belirtmek için [in işleci](/azure/kusto/query/inoperator) kullanın. Sonuçların beklendiğini doğrulamak için temizleme isteğini yürütmeden önce sorguyu çalıştırmanız gerekir. 



Azure Kaynak Yöneticisi rolü atandıktan sonra iki yeni API yolu kullanılabilir: 

#### <a name="log-data"></a>Günlük verileri

* [POST temizleme](https://docs.microsoft.com/rest/api/loganalytics/workspaces%202015-03-20/purge) - silmek için veri parametrelerini belirten bir nesne alır ve bir referans GUID döndürür 
* GET temizleme durumu - POST temizleme çağrısı, temizleme API'nizin durumunu belirlemek için arayabilirsiniz bir URL içerecek bir 'x-ms-durum konumu' üstbilgisi döndürecektir. Örnek:

    ```
    x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/Microsoft.OperationalInsights/workspaces/[WorkspaceName]/operations/purge-[PurgeOperationId]?api-version=2015-03-20
    ```

> [!IMPORTANT]
>  Temizleme operasyonlarının büyük çoğunluğunun SLA'mızdan çok daha hızlı tamamlanmasını beklerken, Log Analytics tarafından kullanılan veri platformu üzerindeki ağır etkileri nedeniyle, **temizleme işlemlerinin tamamlanması için resmi SLA 30 gün olarak belirlenmiştir.** 

#### <a name="application-data"></a>Uygulama verileri

* [POST temizleme](https://docs.microsoft.com/rest/api/application-insights/components/purge) - silmek için veri parametrelerini belirten bir nesne alır ve bir referans GUID döndürür
* GET temizleme durumu - POST temizleme çağrısı, temizleme API'nizin durumunu belirlemek için arayabilirsiniz bir URL içerecek bir 'x-ms-durum konumu' üstbilgisi döndürecektir. Örnek:

   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

> [!IMPORTANT]
>  Tasfiye operasyonlarının büyük çoğunluğu SLA'dan çok daha hızlı tamamlanabilirken, Application Insights tarafından kullanılan veri platformu üzerindeki ağır etkileri nedeniyle, **temizleme işlemlerinin tamamlanması için resmi SLA 30 gün olarak belirlenmiştir.**

## <a name="next-steps"></a>Sonraki adımlar
- Log Analytics verilerinin nasıl toplandığı, işlendiği ve güvenli hale nasıl sağlandığı hakkında daha fazla bilgi edinmek için [Log Analytics veri güvenliği](../../azure-monitor/platform/data-security.md)bölümüne bakın.
- Application Insights verilerinin nasıl toplandığı, işlendiği ve güvenli hale nasıl sağlandığı hakkında daha fazla bilgi edinmek için [Bkz.](../../azure-monitor/app/data-retention-privacy.md)
