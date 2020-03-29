---
title: Bing Arama API'leri için kullanım ve görüntüleme gereksinimleri
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızda Bing Arama API'lerinden gelen arama sonuçlarını görüntüleme gereksinimleri.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: 5575668f164b97142e7c4b2ddb2608c3173426a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "60499894"
---
# <a name="bing-search-api-use-and-display-requirements"></a>Bing Arama API’si kullanım ve görüntüleme gereksinimleri

Bu kullanım ve görüntüleme gereksinimleri, ilişkiler, meta veriler ve diğer sinyaller de dahil olmak üzere aşağıdaki Bing Arama API'lerinden içeriğin ve ilişkili bilgilerin uygulanması için geçerlidir.

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
|Yanıt     | Yanıt olarak döndürülen bir sonuç kategorisi. Örneğin, Bing Web Arama API'sinden gelen bir yanıt, web sayfası sonuçları, resim, video, görsel ve haber kategorilerinde yanıtlar içerebilir. |
|Yanıt     | Arama API'sine yapılan tek bir çağrıya yanıt olarak alınan tüm yanıtlar ve ilişkili veriler. |
|Sonuç    | Yanıttaki bir bilgi öğesi. Örneğin, tek bir haber makalesiyle bağlantılı veri kümesi, bir haber yanıtıyla sonuçlanır. |
|API Arama    | topluca, Bing Özel Arama, Varlık Arama, Görüntü Arama, Haber Arama, Video Arama, Görsel Arama, Yerel İş Arama ve Web Arama API'leri. |

## <a name="bing-spell-check-and-bing-autosuggest-api-restrictions"></a>Bing Yazım Denetimi ve Bing Otomatik Öner API kısıtlamaları

Yapma:

- Bing Yazım Denetimi'nden veya Bing Otomatik Öner API'lerinden aldığınız verileri kopyalayın, depolayın veya önbelleğe alın.
- Bing Yazım Denetimi veya Bing Otomatik Öner API'lerden aldığınız verileri herhangi bir makine öğrenimi veya benzer algoritmik etkinliğin bir parçası olarak kullanın. Bu verileri, sizin veya üçüncü tarafların sunabileceği yeni veya mevcut hizmetleri eğitmek, değerlendirmek veya geliştirmek için kullanmayın.

## <a name="bing-search-apis"></a>Bing Arama API’leri

> [!NOTE]
> Bu bölümdeki gereksinimler yalnızca Bing Yazım Denetimi veya Bing Otomatik Öner'i içermeyen Arama API'leri için geçerlidir. 

### <a name="internet-search-experience-requirements"></a>Internet arama deneyimi gereksinimleri

Yanıtlarda döndürülen tüm veriler yalnızca internet arama deneyimlerinde kullanılabilir. Bir internet arama deneyimi görüntülenen içerik anlamına gelir: 

- Son kullanıcının doğrudan sorgusuyla veya arama ilgi alanı ve amaçlarının (örneğin, kullanıcı tarafından gösterilen bir arama sorgusu) diğer göstergesiyle alakalı ve yanıt verir. 

- Kullanıcıların yanıtın veri kaynaklarını bulmasına ve bu kaynaklara gitmesini sağlar. Örneğin, yanıttaki köprülerden tıklanabilir bağlantılar sağlar.

- Kullanıcının seçebilmek için birden çok sonuç içerir. 

- Kullanıcıların arama yapmalarını sağlayan bir yerleşimdedir.

- İçeriğin bir internet arama sonucu olduğuna dair görünür bir gösterge içerir. Örneğin, içeriğin "web'den" olduğunu belirten bir ifade.

- Bing Arama API verilerinizin geçerli yasaları veya üçüncü taraf haklarını ihlal etmediğinden emin olmak için diğer uygun önlemleri içerir. Hangi önlemlerin uygun olabileceğini belirlemek için yasal danışmanlarınıza danışın.

Bu internet arama deneyimi gereksinimlerinin tek istisnası, bu makalede daha sonra açıklandığı gibi URL keşfidir. 

### <a name="restrictions"></a>Kısıtlamalar

Yapma:

- Yanıtlardan gelen verileri kopyalama, depolama veya önbelleğe alma [(hizmetin sürekliliğinin](#continuity-of-service)izin verdiği ölçüde bekletme hariç. 

- Arama API'lerinden alınan verileri herhangi bir makine öğrenimi veya benzer algoritmik etkinliğin bir parçası olarak kullanın. Bu verileri, sizin veya üçüncü tarafların sunabileceği yeni veya mevcut hizmetleri eğitmek, değerlendirmek veya geliştirmek için kullanmayın.

- Yasaların gerektirdiği veya Microsoft tarafından kabul edilmedikçe, sonuçların içeriğini (başka bir gereksinimi ihlal etmeyecek şekilde yeniden biçimvermek dışında) değiştirin. 

- Sonuç içeriğiyle ilişkili atıf bilgilerini ve URL'leri atla.

- Yasalar gerektirmedikçe veya Microsoft tarafından kabul edilmedikçe, bir sipariş veya sıralama sağlandığında, ihmal dahil olmak üzere yeniden sipariş verin. 

    > [!NOTE]
    > Bu gereksinim, Bing Özel Arama API'si için portal üzerinden uygulanan yeniden sıralama için geçerli değildir.

- Yanıtın herhangi bir bölümündeki diğer içeriği, kullanıcının diğer içeriğin yanıtın bir parçası olduğuna inanmasına yol açacak şekilde görüntüleyin. 

- Yanıtın herhangi bir bölümünü görüntüleyen herhangi bir sayfada Microsoft tarafından sağverilmeyecek reklamları görüntüleyin. 

- Yanıtlar içeren sayfalarda herhangi bir reklam görüntüleme:
    - Bing Görüntüsü, Haber Arama, Video Arama veya Görsel Arama API'lerinden
    - Bunlar, öncelikle (veya yalnızca) görüntü, haber ve/veya video veya görsel arama sonuçlarıyla filtrelenir veya sınırlıdır.

### <a name="notices-and-branding"></a>Bildirimler ve markalama 
Yapın:

- Kullanıcı deneyimindeki (UX) kullanıcıya arama sorgusu girebilme olanağı sunan her noktaya yakın olan [Microsoft Gizlilik Bildirimi'ne](https://go.microsoft.com/fwlink/?LinkId=521839)işlevsel bir köprü ekleyin. Köprümicrosoft **Gizlilik Bildirimi'ni**etiketle.

- Kullanıcıya arama sorgusu giriş yapma olanağı sunan UX'deki her noktaya yakın bing ticari marka kullanım [yönergeleriyle](https://go.microsoft.com/fwlink/?linkid=833278)tutarlı olarak Bing markasını belirgin bir şekilde görüntüleyin. Bu tür bir marka, Microsoft'un internet arama deneyimine güç verdiğini kullanıcıya açıkça belirtmelidir.

- Microsoft kullanımınız için yazılı olarak aksi belirtmediği sürece Bing Web Arama, Resim Arama, Haber Arama, Video Arama ve Görsel Arama API'lerinden görüntülenen her yanıtı (veya yanıtın bir kısmını) Microsoft'a atfedebilirsiniz. Bu, [Bing Ticari Marka Kullanım Yönergeleri'nde](https://go.microsoft.com/fwlink/?linkid=833278)açıklanmıştır. 

Yapma:

- Microsoft özel kullanımınız için yazılı olarak aksi belirtmediği sürece, Bing Özel Arama API'sinden görüntülenen yanıtları (veya yanıtların bölümlerini) Microsoft'a atfedin.

### <a name="transferring-responses"></a>Yanıtları aktarma

Bir kullanıcının bir Arama API'sinden bir yanıtı başka bir kullanıcıya (örneğin, bir mesajlaşma uygulaması veya sosyal medya gönderimi gibi) aktarmasını sağlarsanız, aşağıdaki ler geçerlidir: 

- Aktarılan yanıtlar şunları yapmalı:
  - Aktaran kullanıcıya görüntülenen yanıtların içeriğinden değiştirilmemiş içerikten oluşur. Değişiklikleri biçimlendirmek caizdir.
  - Meta veri formuna herhangi bir veri eklemeyin.
  - Bing Web, Resim, Haber, Video ve Görsel API'lerden gelen yanıtlar için, yanıtın Bing tarafından desteklenen bir internet arama deneyimi aracılığıyla elde edildiğini belirten görüntü dili. Örneğin, "Powered by Bing" veya "Bing'de bu resim hakkında daha fazla bilgi edinin" gibi bir dil görüntüleyebilir veya Bing logosunu kullanabilirsiniz.
  - Bing Özel Arama API'sinden gelen yanıtlar için, yanıtın bir internet arama deneyimi aracılığıyla elde edildiğini belirten bir dil görüntüleyin. Örneğin, "Bu arama sonucu hakkında daha fazla bilgi edinin" gibi bir dil görüntüleyebilirsiniz.
  - Yanıtı oluşturmak için kullanılan tam sorguyu belirgin bir şekilde görüntüleyin.
  - Doğrudan veya arama motoru (bing.com, m.bing.com veya özel arama hizmetiniz aracılığıyla yanıtın altında yatan kaynağa belirgin bir bağlantı veya benzer bir atıf ekleyin).
- Yanıtların aktarımını otomatikleştiremeyebilirsiniz. Bir aktarım, bir yanıtı aktarma niyetini açıkça iptal eden bir kullanıcı eylemi tarafından başlatılmalıdır.
- Bir kullanıcının yalnızca aktaran kullanıcının sorgusuna yanıt olarak görüntülenen yanıtları aktarmasını etkinleştirebilirsiniz.

### <a name="continuity-of-service"></a>Hizmetin sürekliliği 

Arama API yanıtlarından herhangi bir veriyi kopyalamayın, depolamayın veya önbelleğe almayın. Ancak, hizmet erişiminin ve veri oluşturmanın sürekliliğini sağlamak için, sonuçları yalnızca aşağıdaki koşullar altında saklayabilirsiniz:

#### <a name="device"></a>Cihaz

Bir kullanıcının sorgu nun verildiği andan itibaren (i) 24 saat veya (ii) kullanıcı güncelleştirilmiş sonuçlar için başka bir sorgu gönderene kadar, yalnızca tutulan sonuçların kullanılabilmiş olması koşuluyla, bir aygıttaki sonuçları bir aygıtta saklamasını sağlayabilirsiniz:

- Kullanıcının söz konusu cihazda daha önce bu kullanıcıya iade edilen sonuçlara erişmesini sağlamak için (örneğin, hizmet kesintisi durumunda).
- Kullanıcının sinyallerine dayalı olarak (örneğin, beklenen hizmet kesintisi durumunda) kullanıcının gereksinimleri beklentisiyle kişiselleştirilmiş proaktif sorgunuz için döndürülen sonuçları depolamak için.

#### <a name="server"></a>Sunucu

Tek bir kullanıcıya özgü sonuçları, kontrol ettiğiniz bir sunucuda güvenli bir şekilde saklayabilir ve yalnızca tutulan sonuçları görüntüleyebilirsiniz:

- Kullanıcının çözümde daha önce bu kullanıcıya döndürülen sonuçların geçmiş raporuna erişmesini sağlamak için. Sonuçlar (i) son kullanıcının ilk sorgusundan itibaren 21 günden fazla tutulamaz ve (ii) kullanıcının yeni veya yinelenen sorgusuna yanıt olarak görüntülenemez.
- Kullanıcının sinyallerine dayalı olarak, kullanıcının gereksinimleri beklentisiyle kişiselleştirilmiş proaktif sorgunuz için döndürülen sonuçları depolamak için. Bu sonuçları sorgu nun zamanından itibaren 24 saat veya (ii) kullanıcı güncelleştirilmiş sonuçlar için başka bir sorgu gönderene kadar daha az bir süre için depolayabilirsiniz.

Tutulduğunda, belirli bir kullanıcının sonuçları başka bir kullanıcının sonuçlarıyla hesaplanamaz. Diğer bir süre, her kullanıcının sonuçları ayrı olarak tutulmalıdır.

### <a name="general"></a>Genel 

Tüm tutulan sonuçların sunumu için:

- Sorgunun gönderildiği saate ait açık ve görünür bir bildirim ekleyin.
- Yeniden sorgulamak ve güncelleştirilmiş sonuçlar elde etmek için kullanıcıya bir düğme veya benzer araçlar sunun. 
- Sonuçların sunumunda Bing markasını koruyun.
- Depolanan sonuçları belirtilen zaman dilimleri içinde silin (ve gerekirse yeni bir sorguyla yenileyin).

### <a name="non-display-url-discovery"></a>Görüntülendirilmeyen URL bulma 

Arama yanıtlarını yalnızca kullanıcınızdan veya müşterinizden gelen bir sorguya yanıt veren bilgi kaynaklarının URL'lerini keşfetmek amacıyla internet dışı bir arama deneyiminde kullanabilirsiniz. Bu tür URL'leri sağladığınız bir raporda veya benzer yanıtta kopyalayabilirsiniz:

- Yalnızca bu kullanıcıya veya müşteriye, bu sorguya yanıt olarak.
- Yalnızca sorguyla ilgili önemli ek değerli içerik içeriyorsa.

Arama API'lerinin önceki bölümleri aşağıdakiler dışında bu ekran dışı kullanım için geçerli değildir: 

- Arama yanıtından daha önce açıklanan sınırlı URL kopyalama dışında herhangi bir veriyi veya içeriği önbelleme, kopyalamaveya saklamayın.
- Arama API'lerinden alınan verileri (URL'ler dahil) kullanmanızın yürürlükteki yasaları veya üçüncü taraf haklarını ihlal etmediğinden emin olun.
- Arama API'lerinden alınan verileri (URL'ler dahil) herhangi bir arama dizininin veya makine öğreniminin veya benzer algoritmik etkinliğin bir parçası olarak kullanmayın. Bu verileri sizin veya üçüncü tarafların sunabileceği hizmetleri oluşturmak, değerlendirmek veya iyileştirmek için kullanmayın.

## <a name="gdpr-compliance"></a>GDPR uyumluluğu  

Avrupa Birliği Genel Veri Koruma Yönetmeliği'ne (GDPR) tabi olan ve Arama API'lerine, Bing Yazım Denetimi API'sine veya Bing Otomatik Öner API'ye yapılan aramalarla bağlantılı olarak işlenen kişisel verilerle ilgili olarak, sizin ve Microsoft'un GDPR altında bağımsız veri denetleyicileri. GDPR'ye uyumunuzdan bağımsız olarak siz sorumlusunuz.  

