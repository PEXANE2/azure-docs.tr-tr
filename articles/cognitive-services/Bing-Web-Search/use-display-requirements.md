---
title: Bing Arama API'leri için gereksinimleri kullanma ve görüntüleme
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızdaki Bing Arama API'leri arama sonuçlarını görüntüleme gereksinimleri.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: 5575668f164b97142e7c4b2ddb2608c3173426a6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60499894"
---
# <a name="bing-search-api-use-and-display-requirements"></a>Bing Arama API’si kullanım ve görüntüleme gereksinimleri

Bu kullanım ve görüntüleme gereksinimleri, ilişki, meta veriler ve diğer sinyaller dahil olmak üzere aşağıdaki Bing Arama API'leri tüm içerik ve ilgili bilgilerin uygulanması için geçerlidir.

- Bing Özel Arama
- Bing Varlık Arama
- Bing Resim Arama
- Bing Haber Arama
- Bing Video Arama
- Bing Görsel Arama
- Bing Web Araması
- Bing Yazım Denetimi
- Bing Otomatik Öneri

## <a name="definitions"></a>Tanımlar


|Sözleşme Dönemi  |Açıklama  |
|---------|---------|
|Yanıt     | Bir yanıtta döndürülen sonuç kategorisi. Örneğin, Bing Web Araması API'si bir yanıt, Web sayfası sonuçları, görüntü, video, görsel ve haber kategorilerine yanıt içerebilir. |
|Yanıt     | Bir arama API 'sine yapılan tek bir çağrıya yanıt olarak alınan tüm yanıtlar ve ilgili veriler. |
|Sonuç    | Bir yanıtdaki bilgi öğesi. Örneğin, tek bir haber makalesiyle bağlantılı veri kümesi, bir haber yanıtı ile sonuçlanır. |
|API Arama    | toplu olarak, Bing Özel Arama, Varlık Arama, Resim Arama, Haber Arama, Video Arama, Görsel Arama, yerel Iş arama ve Web Araması API 'Leri. |

## <a name="bing-spell-check-and-bing-autosuggest-api-restrictions"></a>Bing Yazım Denetimi ve Bing Otomatik Öneri API'si kısıtlamaları

Yapma:

- Bing Yazım Denetimi veya Bing Otomatik Öneri API 'Lerinden aldığınız verileri kopyalayın, depolayın veya önbelleğe alın.
- Makine öğrenimi veya benzer algoritmik etkinliğinin bir parçası olarak Bing Yazım Denetimi veya Bing Otomatik Öneri API 'Lerinden aldığınız verileri kullanın. Bu verileri, ya da üçüncü tarafların sunabileceği yeni veya mevcut hizmetleri eğlendirmek, değerlendirmek veya geliştirmek için kullanmayın.

## <a name="bing-search-apis"></a>Bing Arama API’leri

> [!NOTE]
> Bu bölümdeki gereksinimler yalnızca Bing Yazım Denetimi veya Bing Otomatik Öneri içermeyen arama API 'Leri için geçerlidir. 

### <a name="internet-search-experience-requirements"></a>Internet araması deneyimi gereksinimleri

Yanıtlarda döndürülen tüm veriler yalnızca İnternet araması deneyimlerinde kullanılabilir. Internet araması deneyimi, görünen içeriğin gösterildiği anlamına gelir: 

- Son kullanıcının doğrudan sorgusuna uygun ve yanıt veriyor ya da arama ilgi ve amacını (örneğin, Kullanıcı tarafından belirtilen arama sorgusu) başka bir şekilde belirtir. 

- Kullanıcıların yanıtın veri kaynaklarını bulmasına ve bu kaynaklara gitmenize yardımcı olur. Örneğin, yanıttaki köprülerden tıklatılabilir bağlantılar sağlama.

- Kullanıcının seçim yapmak için birden çok sonuç içerir. 

- , Kullanıcıların aramasını sağlayan bir yerleşimde bulunur.

- İçeriğin Internet arama sonucu olduğunu gösteren görünür bir bildirim içerir. Örneğin, içeriğin "Web 'den" olduğu bir ifade.

- Bing Arama API 'nizin ilgili yasaları veya üçüncü taraf haklarını ihlal etmemesini sağlamak için diğer uygun ölçüleri içerir. Hangi ölçümlerin uygun olabileceğini belirlemek için yasal danışmanlarınızı inceleyin.

Bu Internet arama deneyimi gereksinimlerinin tek istisnası, bu makalenin ilerleyen kısımlarında açıklandığı gibi URL bulma amaçlıdır. 

### <a name="restrictions"></a>Kısıtlamalar

Yapma:

- Yanıtlardan herhangi bir veriyi kopyalayın, depolayın veya önbelleğe alma dışında ( [hizmetin sürekliliği](#continuity-of-service)nedeniyle izin verilen ölçüde saklama süresi hariç). 

- Herhangi bir makine öğrenimi veya benzer algoritmik etkinliğinin parçası olarak arama API 'Lerinden alınan verileri kullanın. Bu verileri, ya da üçüncü tarafların sunabileceği yeni veya mevcut hizmetleri eğlendirmek, değerlendirmek veya geliştirmek için kullanmayın.

- Yasalar için gerekli olmayan veya Microsoft tarafından kabul etmedikleri müddetçe, sonuçların içeriğini değiştirin (başka bir gereksinimi ihlal etmediği bir şekilde yeniden biçimlendirmek yerine). 

- Sonuç içeriğiyle ilişkili öznitelik bilgilerini ve URL 'Leri atlayın.

- Ya da Microsoft tarafından kabul etmedikleri müddetçe, bir sipariş ya da derecelendirme sağlandığında bir cevap olarak bir yanıt olarak görüntülenecek sonuçları, yok etme ile birlikte yeniden sıralayın. 

    > [!NOTE]
    > Bu gereksinim, Bing Özel Arama API'si portalı aracılığıyla uygulanan yeniden sıralama için geçerlidir.

- Bir yanıtın herhangi bir bölümünde diğer içerikleri, bir kullanıcıya diğer içeriğin yanıtın bir parçası olduğunu düşünmesine neden olacak şekilde görüntüleyin. 

- Yanıtın herhangi bir parçasını görüntüleyen herhangi bir sayfada Microsoft tarafından sağlanmayan tanıtımı görüntüleyin. 

- Yanıtları olan sayfalarda tüm reklamları görüntüleme:
    - Bing görüntüsünden, Haber Arama, Video Arama veya Görsel Arama API 'Leri
    - Bunlar, birincil olarak (ya da yalnızca) görüntü, haber ve/veya görsel arama sonuçlarına göre filtrelenir veya sınırlıdır.

### <a name="notices-and-branding"></a>Bildirimler ve marka 
Gösterme

- Göze çarpacak, kullanıcı deneyiminin (UX) bir kullanıcıya bir arama sorgusu girişi olanağı sunan her noktaya yakın bir [Microsoft gizlilik bildirimine](https://go.microsoft.com/fwlink/?LinkId=521839)yönelik işlevsel köprü içerir. Köprü **Microsoft gizlilik bildirimi**' ni etiketleyin.

- Belirgin bir şekilde, bir kullanıcıya arama sorgusu girişi olanağı sunan, UX 'teki her bir noktaya yakın olan [Bing marka kullanım yönergeleriyle](https://go.microsoft.com/fwlink/?linkid=833278)tutarlı Bing markalaması görüntüleyin. Bu marka, Microsoft 'un Internet arama deneyimini güçleme kullanıcısına açık bir durum vermelidir.

- Bing Web Araması, Resim Arama, Haber Arama, Video Arama ve Görsel Arama API 'Lerinde, Microsoft 'a başka bir şekilde yazılmadığı müddetçe, bu her yanıtı (veya yanıtın bir kısmını) öznitelik olarak kullanabilirsiniz. Bu, [Bing ticari marka kullanım yönergeleri](https://go.microsoft.com/fwlink/?linkid=833278)bölümünde açıklanmaktadır. 

Yapma:

- Microsoft 'un belirli bir kullanım için yazılmasında aksini belirtmedikleri müddetçe, Microsoft 'a Bing Özel Arama API'si gösterildiği öznitelik yanıtları (veya yanıtların bölümleri).

### <a name="transferring-responses"></a>Yanıtları aktarma

Bir kullanıcının bir ara API 'den bir iletiyi bir ileti uygulaması veya sosyal medya gönderme gibi başka bir kullanıcıya aktarmasını etkinleştirirseniz aşağıdakiler geçerlidir: 

- Aktarılan yanıtların şunları olması gerekir:
  - Aktarma kullanıcısına görüntülenen yanıtların içeriğinden değiştirilmemiş içerikten oluşur. Biçimlendirme değişiklikleri izin verilir.
  - Meta veri biçiminde herhangi bir veri eklemeyin.
  - Bing Web, görüntü, haber, video ve Visual API 'Lerinin yanıtları için, yanıtı Bing tarafından desteklenen bir internet arama deneyimi aracılığıyla elde edilen yanıtı belirten dilde görüntüleyin. Örneğin, "Bing tarafından destekleniyor" veya "Bu görüntü hakkında daha fazla bilgi edinin" gibi bir dil görüntüleyebilirsiniz veya Bing logosunu kullanabilirsiniz.
  - Bing Özel Arama API'si yanıtları için, yanıtın bir internet arama deneyimiyle elde edilmediğini belirten dili görüntüleyin. Örneğin, "Bu arama sonucu hakkında daha fazla bilgi edinin" gibi bir dil görüntüleyebilirsiniz.
  - Karşılığı, yanıtı oluşturmak için kullanılan tam sorguyu görüntüler.
  - Doğrudan ya da arama altyapısı (bing.com, m.bing.com veya özel arama hizmetiniz) aracılığıyla yanıtın temel kaynağına benzer bir bağlantı veya benzer bir öznitelik ekleyin.
- Yanıtların aktarımını otomatikleştiremeyebilirsiniz. Bir aktarım, bir kullanıcı eylemi tarafından bir yanıtı aktarmaya yönelik bir amaç açıkça açıkça anlaşılmalıdır.
- Bir kullanıcıyı yalnızca aktarım yapan kullanıcının sorgusuna yanıt olarak görüntülenen yanıtları aktarmaya izin verebilir.

### <a name="continuity-of-service"></a>Hizmetin sürekliliği 

Arama API 'SI yanıtlarındaki verileri kopyalamayın, depolamaya da önbelleğe vermeyin. Ancak, hizmet erişimi ve veri işleme sürekliliği sağlamak için sonuçları yalnızca aşağıdaki koşullarda koruyabilirsiniz:

#### <a name="device"></a>Cihaz

Bir kullanıcının, bir cihazdaki sonuçları sorgu zamanından daha az (i) 24 saat beklemek üzere veya (ii) bir kullanıcı güncelleştirilmiş sonuçlar için başka bir sorgu gönderene kadar, bu durum yalnızca kullanılabilir sonuçlar kullanılabilir:

- Kullanıcının cihazdaki bu kullanıcıya daha önce geri döndürülen sonuçlara erişmesini sağlamak için (örneğin, hizmet kesintisi olması durumunda).
- Bu kullanıcının sinyallerine (örneğin, beklenen hizmet kesintisi durumunda) bağlı olarak, kullanıcının ihtiyaçlarına göre olasılığına içinde kişiselleştirilmiş, proaktif sorgunuz için döndürülen sonuçları depolamak için.

#### <a name="server"></a>Sunucu

Tek bir kullanıcıya özgü sonuçları, denetlediğiniz bir sunucuda güvenli bir şekilde koruyabilir ve yalnızca tutulan sonuçları görüntüleyebilirsiniz:

- Kullanıcının çözümünüzde bir geçmiş raporuna daha önce geri döndürülen bir sonuç raporuna erişmesini sağlamak için. Sonuçlar, son kullanıcının ilk sorgusunun zamanından 21 günden uzun süre boyunca saklanmayabilir ve (ii) kullanıcının yeni veya yinelenen sorgusuna yanıt olarak görüntülenir.
- Bu kullanıcının sinyallerine bağlı olarak, kullanıcının ihtiyaçlarını olasılığına içinde kişiselleştirilmiş, proaktif sorgunuz için döndürülen sonuçları depolamak için. Bu sonuçları sorgu zamanından daha az (i) 24 saat boyunca saklayabilir veya bir kullanıcı güncelleştirilmiş sonuçlar için başka bir sorgu gönderene kadar (ii).

Her tutulurken, belirli bir kullanıcının sonuçları başka bir Kullanıcı sonuçlarıyla birlikte kullanılamaz. Diğer bir deyişle, her bir kullanıcının sonuçlarının ayrı olarak korunması ve teslim edilmesi gerekir.

### <a name="general"></a>Genel 

Korunan sonuçların tüm sunumu için:

- Sorgunun gönderilme zamanına açık ve görünür bir bildirim ekleyin.
- Kullanıcıya, güncelleştirilmiş sonuçları yeniden sorgulamak ve almak için bir düğme veya benzer bir yol sunun. 
- Bing markalamasını sonuçları sunumunda tutun.
- Belirtilen süreleri içindeki depolanan sonuçları silin (gerekiyorsa yeni bir sorgu ile Yenile).

### <a name="non-display-url-discovery"></a>Görüntüleme olmayan URL keşfi 

Yalnızca Kullanıcı veya müşteriden gelen bir sorguya yanıt veren bilgi kaynaklarının URL 'Lerini bulmaktan bağımsız olarak yalnızca İnternet dışı bir arama deneyiminde arama yanıtlarını kullanabilirsiniz. Bu URL 'Leri bir raporda veya sağladığınız benzer bir yanıta kopyalayabilirsiniz:

- Bu sorguya yanıt olarak yalnızca bu kullanıcı veya müşteri için.
- Yalnızca, sorguyla ilgili önemli miktarda ek değerli içerik içeriyorsa.

Arama API 'lerinin önceki bölümlerinde ve görüntüleme gereksinimlerinin kullanılması, aşağıdakiler hariç, bu görüntüleme dışı kullanım için geçerlidir: 

- Daha önce açıklanan sınırlı URL kopyalaması dışındaki verileri veya içeriği, arama yanıtından başka bir şekilde önbelleğe alma, kopyalama veya depolama.
- Arama API 'Lerinden alınan veri (URL 'Ler dahil) kullanmanın ilgili yasaları veya üçüncü taraf haklarını ihlal etmemesini sağlayın.
- Arama API 'Lerinden alınan verileri (URL 'Ler dahil), herhangi bir arama dizini veya makine öğrenimi veya benzer algoritmik etkinliğinin parçası olarak kullanmayın. Bu verileri, sizin veya üçüncü tarafların sunabileceği Hizmetleri eğitme, değerlendirme veya geliştirme amacıyla kullanmayın.

## <a name="gdpr-compliance"></a>GDPR uyumluluğu  

Avrupa Birliği Genel Veri Koruma Yönetmeliği (GDPR) ile ilgili herhangi bir kişisel veri konusuyla ilgili ve arama API 'Leri, Bing Yazım Denetimi API'si veya Bing Otomatik Öneri API'si çağrılarıyla bağlantılı olarak işlenen, siz ve Microsoft 'un GDPR altında bağımsız veri denetleyicileri olduğunu anlamış olursunuz. GDPR ile uyumluluktan bağımsız olarak sorumlusunuz.  

