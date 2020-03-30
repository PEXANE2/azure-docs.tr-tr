---
title: Azure Ön Kapı - trafik yönlendirme yöntemleri | Microsoft Dokümanlar
description: Bu makale, Front Door tarafından kullanılan farklı trafik yönlendirme yöntemlerini anlamanıza yardımcı olur
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 69ef68dafc2385eb5614179c3d04265250383104
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471549"
---
# <a name="front-door-routing-methods"></a>Ön Kapı yönlendirme yöntemleri

Azure Ön Kapı, HTTP/HTTPS trafiğinizi çeşitli hizmet bitiş noktalarına nasıl yönlendireceklerini belirlemek için çeşitli trafik yönlendirme yöntemlerini destekler. İstemci isteklerinizin her biri Ön Kapı'ya ulaştığında, isteklerin en iyi arka uç örneğine iletilmesini sağlamak için yapılandırılan yönlendirme yöntemi uygulanır. 

Front Door'da trafik yönlendirmesi için dört ana kavram vardır:

* ** [Gecikme sayısı](#latency):** Gecikme tabanlı yönlendirme, isteklerin duyarlılık aralığında kabul edilebilir en düşük gecikme geri uçlarına gönderilmesini sağlar. Temel olarak, kullanıcı istekleriniz ağ gecikmesiyle ilgili olarak "en yakın" arka uç kümesine gönderilir.
* ** [Öncelik](#priority):** Tüm trafik için birincil hizmet arka ucunu kullanmak istediğinizde öncelikleri farklı arka uçlarınıza atayabilir ve birincil veya yedek arka uçların kullanılamaması durumunda yedekler sağlayabilirsiniz.
* ** [Ağırlıklı](#weighted):** Trafiği bir dizi arka uça eşit olarak veya ağırlık katsayılarına göre dağıtmak istediğinizde farklı arka uçlarınıza ağırlık atayabilirsiniz.
* **Oturum Affinity:** Kullanıcı oturumu hala etkin olduğu ve arka uç örneği sistem durumu sondalarına bağlı olarak sağlıklı rapor verdiği sürece, kullanıcıdan sonraki isteklerin aynı arka uca gönderilmesini istediğinizde ön uç ana bilgisayarlarınıza veya etki alanlarınıza oturum afiyetini yapılandırabilirsiniz. 

Tüm Front Door yapılandırmaları, arka uç durumu ve otomatik anında global yük devretme izlemesini kapsar. Daha fazla bilgi için Ön [Kapı Arka Uç İzleme'ye](front-door-health-probes.md)bakın. Ön Kapınız tek bir yönlendirme yöntemini temel alan çalışacak şekilde yapılandırılabilir ve uygulama gereksinimlerinize bağlı olarak en iyi yönlendirme topolojisi oluşturmak için bu yönlendirme yöntemlerinin birden birini veya tümlerini birlikte kullanabilirsiniz.

## <a name="lowest-latencies-based-traffic-routing"></a><a name = "latency"></a>En düşük gecikme litrafik yönlendirmesi

Arka uçları dünya çapında iki veya daha fazla konumda dağıtmak, trafiği son kullanıcılarınıza 'en yakın' konuma yönlendirme yaparak birçok uygulamanın yanıt verme yeteneğini artırabilir. Ön Kapı yapılandırmanız için varsayılan trafik yönlendirme yöntemi, son kullanıcılarınızdan gelen istekleri, isteği alan Ön Kapı ortamından en yakın arka uca iletir. Azure Ön Kapı'nın Anycast mimarisiyle birleştiğinde, bu yaklaşım son kullanıcılarınızın konumlarına göre maksimum performansı kişiselleştirmesini sağlar.

Coğrafi uzaklıkla ölçülen en yakın arka uç mutlaka en yakın değildir. Bunun yerine, Ön Kapı ağ gecikmesini ölçerek en yakın arka uçları belirler. [Front Door yönlendirme mimarisi](front-door-routing-architecture.md)hakkında daha fazla bilgi edinin. 

Aşağıda genel karar akışı:

| Kullanılabilir arka uçlar | Öncelik | Gecikme sinyali (sağlık sondası temel alınca) | Ağırlık |
|-------------| ----------- | ----------- | ----------- |
| İlk olarak, sistem durumu sondası için etkin leştirilmiş ve sağlıklı (200 OK) döndürülen tüm arka uçları seçin. Diyelim ki, Altı arka uç A, B, C, D, E ve F, ve bunların arasında C sağlıksız ve E devre dışı. Yani, kullanılabilir arka uçların listesi A, B, D ve F'dir.  | Daha sonra, kullanılabilir olanlar arasında en yüksek öncelik arka uçları seçilir. Diyelim ki, arka uç A, B ve D'nin önceliği 1, arka uç F'nin ise 2 önceliğe sahip olması gerekir. Yani, seçilen arka uçlar A, B ve D olacaktır.| Gecikme aralığı (ms belirtilen en az gecikme & gecikme hassasiyeti) ile backends seçin. Diyelim ki, A 15 ms, B 30 ms ve D, isteğin indiği Ön Kapı ortamından 60 ms uzaktaysa ve gecikme hassasiyeti 30 ms ise, en düşük gecikme havuzu arka uç A ve B'den oluşuyor, çünkü D, A'nın en yakın arka ucundan 30 ms'in üzerindedir. | Son olarak, Ön Kapı belirtilen ağırlıkların oranı backends son seçilen havuz arasında trafik robin yuvarlak olacaktır. Diyelim ki, arka uç A'nın ağırlığı 5, arka uç B'nin ağırlığı 8 ise, trafik arka uçlar Arasında 5:8 oranında dağıtılır. |

>[!NOTE]
> Varsayılan olarak, gecikme duyarlılığı özelliği 0 ms olarak ayarlanır, yani isteği her zaman kullanılabilir en hızlı arka uca iletin.


## <a name="priority-based-traffic-routing"></a><a name = "priority"></a>Öncelik tabanlı trafik yönlendirme

Genellikle bir kuruluş, birincil hizmetlerinin düşmesi durumunda bir veya daha fazla yedekleme hizmeti dağıtarak hizmetleri için güvenilirlik sağlamak ister. Endüstri genelinde bu topoloji aktif/bekleme veya Aktif/Pasif dağıtım topolojisi olarak da adlandırılır. 'Öncelik' trafik yönlendirme yöntemi, Azure müşterilerinin bu hata değiştirme desenini kolayca uygulamasına olanak tanır.

Varsayılan Ön Kapınız, eşit öncelikli arka uçlar listesi içerir. Varsayılan olarak, Ön Kapı trafiği yalnızca birincil arka uç kümesi olan en üst öncelik arka uçlarına (öncelik için en düşük değer) gönderir. Birincil arka uçlar kullanılamıyorsa, Ön Kapı trafiği ikincil arka uç kümesine (öncelik için ikinci en düşük değer) yönlendirir. Hem birincil hem de ikincil arka uçlar kullanılamıyorsa, trafik üçüncüye gider ve böyle devam eder. Arka uç kullanılabilirliği yapılandırılmış duruma (etkin veya devre dışı) ve sağlık sondaları tarafından belirlenen devam eden arka uç sağlık durumuna bağlıdır.

### <a name="configuring-priority-for-backends"></a>Arka uçlar için önceliği yapılandırma

Ön Kapı yapılandırması içindeki arka uç havuzundaki arka uçların her birinin 'Öncelik' adı verilen ve 1 ile 5 arasında bir sayı olabilecek bir özelliği vardır. Azure Ön Kapı ile, her arka uç için bu özelliği kullanarak arka uç önceliğini açıkça yapılandırırsınız. Bu özellik 1 ile 5 arasında bir değerdir. Daha düşük değerler daha yüksek bir önceliği temsil ediyor. Arka uçlar öncelik değerlerini paylaşabilir.

## <a name="weighted-traffic-routing-method"></a><a name = "weighted"></a>Ağırlıklı trafik yönlendirme yöntemi
'Ağırlıklı' trafik yönlendirme yöntemi, trafiği eşit olarak dağıtmanıza veya önceden tanımlanmış bir ağırlıklandırma kullanmanıza olanak tanır.

Ağırlıklı trafik yönlendirme yönteminde, arka uç havuzunuzun Ön Kapı yapılandırmasında her arka uça bir ağırlık atarsınız. Ağırlık 1-1000 arasında bir tamsayıdır. Bu parametre varsayılan '50' ağırlığı kullanır.

Kabul edilen gecikme hassasiyeti içinde kullanılabilir geri uçlar listesi arasında (belirtildiği gibi), trafik belirtilen ağırlıkların oranı bir round-robin mekanizması nda dağıtılır. Gecikme duyarlılığı 0 milisaniye olarak ayarlanmışsa, aynı ağ gecikmesi olan iki arka uç olmadığı sürece bu özellik etkili olmaz. 

Ağırlıklı yöntem bazı yararlı senaryolar sağlar:

* **Aşamalı uygulama yükseltmesi**: Yeni bir arka uça yönlendirmek için trafiğin yüzdesini ayırın ve diğer arka uçlarla eşit hale getirmek için zaman içinde trafiği kademeli olarak artırın.
* **Azure'a uygulama geçişi**: Hem Azure hem de harici arka uçlarla bir arka uç havuzu oluşturun. Yeni arka uçları tercih etmek için arka uçların ağırlığını ayarlayın. Bunu yavaş yavaş yeni arka uçları devre dışı bırakarak, daha sonra en düşük ağırlıkları atayarak, yavaş yavaş en çok trafik aldıkları seviyelere artırarak ayarlayabilirsiniz. Sonra nihayet daha az tercih edilen arka uçları devre dışı bırakıp havuzdan kaldırıyoruz.  
* **Ek kapasite için bulut patlaması**: Ön Kapının arkasına koyarak şirket içi bir dağıtımı buluta hızla genişletin. Bulutta fazladan kapasiteye ihtiyacınız olduğunda, daha fazla arka uç ekleyebilir veya etkinleştirebilir ve trafiğin hangi bölümünün her arka uça gittiğini belirtebilirsiniz.

## <a name="session-affinity"></a><a name = "affinity"></a>Oturum Benzeşimi
Varsayılan olarak, oturum yakınlığı olmadan, Ön Kapı, özellikle farklı arka uçlara geçiş gecikmeleri olarak veya aynı farklı istekler değişirken, yük dengeleme yapılandırmasına dayalı olarak aynı istemciden gelen istekleri farklı arka uçlara iletir kullanıcı farklı bir Ön Kapı ortamına iner. Ancak belirli diğer senaryolardaki bazı durum bilgisi olan uygulamalar, aynı kullanıcıdan gelen ardışık isteklerin ilk isteği işleyen aynı arka uca yönlendirilmesini tercih etmektedir. Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı arka uçta tutmak istediğinizde kullanışlıdır. Azure Ön Kapı yönetilen tanımlama bilgilerini kullanarak, arka uç sağlıklı olduğu ve kullanıcı oturumunun süresi dolmadığı sürece, sonraki trafiği kullanıcı oturumundan aynı arka uca yönlendirebilir. 

Oturum benzeşimi, yapılandırılan etki alanlarınızın (veya alt etki alanlarınızın) her biri için ön uç konağı seviyesinde etkinleştirilebilir. Bu özellik etkinleştirildikten sonra Front Door kullanıcı oturumuna bir tanımlama bilgisi ekler. Front Door, tanımlama bilgisi tabanlı oturum benzeşimi sayesinde aynı IP adresini kullanan farklı kullanıcıları dahi tanımlayabilir ve bu sayede trafiğin farklı arka uçlarınız arasında daha eşit bir şekilde dağıtılmasını sağlayabilir.

Front Door şu an için yalnızca oturum tanımlama bilgisini desteklediğinden tanımlama bilgisinin ömrü kullanıcı oturumunun ömrüyle aynıdır. 

> [!NOTE]
> Kamu vekilleri oturum afiniteile engelleyebilir. Bunun nedeni, bir oturum oluşturmanın Ön Kapı'nın yanıta bir oturum afinite tanımlama bilgisi eklemesini gerektirmesidir ve yanıt önbelleğe alınamıyorsa, aynı kaynağı isteyen diğer istemcilerin tanımlama bilgilerini bozacak şekilde yapılamaz. Buna karşı korumak için, arka uç bu girişimde bulunulduğunda önbelleğe alınabilir bir yanıt gönderirse oturum yakınlığı **kurulmaz.** Oturum zaten kurulmuşsa, arka uçtan gelen yanıtın önbelleğe alınmış olması önemli değildir.
> YanıtıN HTTP 304 durum kodu **yoksa,** oturum yakınlığı aşağıdaki durumlarda belirlenir:
> - Yanıt, "özel" veya ```Cache-Control``` mağaza yok" gibi önbelleğe alma önleme üstbilgi için ayarlanmış özel değerlere sahiptir.
> - Yanıt, süresi ```Authorization``` dolmamış bir üstbilgi içerir.
> - Yanıtın bir HTTP 302 durum kodu vardır.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
