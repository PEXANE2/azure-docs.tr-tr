---
title: Azure ön kapısı-trafik yönlendirme yöntemleri | Microsoft Docs
description: Bu makale, ön kapı tarafından kullanılan farklı trafik yönlendirme yöntemlerini anlamanıza yardımcı olur
services: front-door
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2020
ms.author: duau
ms.openlocfilehash: 2bc056620ff964747dfd83e7525cb5bfd2eb8e52
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91449135"
---
# <a name="front-door-routing-methods"></a>Ön kapı yönlendirme yöntemleri

Azure ön kapısının, HTTP/HTTPS trafiğinizi farklı hizmet uç noktalarına nasıl yönlendirildiğini öğrenmek için farklı türlerde trafik yönlendirme yöntemleri desteklenir. İstemciniz ön kapıya ulaşıldığında, isteklerin en iyi arka uç örneğine iletilmesini sağlamak için yapılandırılmış yönlendirme yöntemi uygulanır. 

Ön kapıda kullanılabilen dört trafik yönlendirme yöntemi vardır:

* **[Gecikme süresi](#latency):** Gecikme tabanlı yönlendirme, isteklerin duyarlık aralığı içinde kabul edilebilir en düşük gecikme süreli arka uçlara gönderilmesini sağlar. Temel olarak, Kullanıcı istekleriniz, ağ gecikmesi açısından arka uçların "en yakın" kümesine gönderilir.
* **[Öncelik](#priority):** Tüm trafiğe hizmet vermek üzere birincil arka uç yapılandırmak istediğinizde arka uçlarınız için öncelikler atayabilirsiniz. İkincil arka uç, birincil arka ucun kullanılamaz duruma gelmesi durumunda bir yedekleme olabilir.
* **[Ağırlıklı](#weighted):** Trafiği bir arka uç kümesi genelinde dağıtmak istediğinizde, arka uçlarınızın ağırlıklarını atayabilirsiniz. Ağırlık katsayılarını eşit bir şekilde dağıtmak veya bu döneme göre yapmak isteyip istemediğiniz.
* **[Oturum benzeşimi](#affinity):** Aynı son kullanıcıdan gelen isteklerin aynı arka uca gönderilmesini sağlamak için ön uç konaklarınız veya etki alanlarınız için oturum benzeşimini yapılandırabilirsiniz.

Tüm Front Door yapılandırmaları, arka uç durumu ve otomatik anında global yük devretme izlemesini kapsar. Daha fazla bilgi için bkz. [ön kapı arka uç izleme](front-door-health-probes.md). Ön kapılarınız, tek bir yönlendirme yönteminden birini temel alabilir. Ancak, uygulamanızın gereksinimlerine bağlı olarak, en iyi yönlendirme topolojisini oluşturmak için birden çok yönlendirme yöntemini de birleştirebilirsiniz.

## <a name="lowest-latencies-based-traffic-routing"></a><a name = "latency"></a>En düşük gecikme süreleri tabanlı trafik-yönlendirme

Arka uçlarda iki veya daha fazla konumda arka uçlar dağıtmak, son kullanıcılarınıza ' en yakın ' olan hedefe trafiği yönlendirerek uygulamalarınızın yanıt hızını iyileştirebilir. Ön kapı yapılandırmanıza yönelik varsayılan trafik yönlendirme yöntemi, istekleri son kullanıcılarınızın isteği alan ön kapı ortamının en yakın arka ucuna iletir. Azure ön kapısının tek noktaya yayın mimarisiyle birlikte bu yaklaşım, son kullanıcılarınızın her birinin konumlarına göre kişiselleştirilmiş en yüksek performansı almasını sağlar.

' En yakın ' arka ucu coğrafi mesafede ölçülen şekilde en yakın değildir. Bunun yerine, ön kapı ağ gecikmesini ölçerek en yakın arka uçları belirler. [Ön kapısının yönlendirme mimarisi](front-door-routing-architecture.md)hakkında daha fazla bilgi edinin. 

Genel karar akışı aşağıda verilmiştir:

| Kullanılabilir arka uçlar | Öncelik | Gecikme sinyali (sistem durumu araştırmasına göre) | Çizgi |
|-------------| ----------- | ----------- | ----------- |
| İlk olarak, etkin olan ve sistem durumu araştırması için sağlıklı (200 Tamam) olarak döndürülen tüm arka uçları seçin. Altı arka ucu A, B, C, D, E ve F varsa ve C 'ler arasında iyi durumda değilse ve E devre dışı bırakılır. Kullanılabilir arka uçların listesi A, B, D ve F 'dir.  | Bundan sonra, kullanılabilir olanlar arasındaki en üst öncelik arka uçları seçilir. A, B ve D arka ucu öncelik 1 ve arka uç F 'nin önceliği 2 ' dir. Ardından, seçilen arka uçlar A, B ve D olur.| Gecikme aralığına sahip arka uçları seçin (en az gecikme süresi & gecikme süresi duyarlılığı). Arka uç A 15 MS ise, B 30 MS ve D, isteğin bulunduğu ön kapıdan, gecikme süresi 30 MS 'den 60 ms ise en düşük gecikme süresi, bir olan en yakın arka uçta 30 MS 'nin ötesinde. | Son olarak, ön kapı, belirtilen ağırlıkların oranıyla, son arka uçların seçili havuzu arasındaki trafiği bir kez daha yuvarlar. Yani, arka uç A 'nın ağırlığı 5, arka uç B ise 8 ağırlığa sahipse trafik, arka uçların A ve B arasındaki 5:8 oranına göre dağıtılır. |

>[!NOTE]
> Varsayılan olarak, gecikme süresi duyarlılığı özelliği 0 MS olarak ayarlanır, diğer bir deyişle, her zaman isteği kullanılabilir en hızlı arka uca iletir.

## <a name="priority-based-traffic-routing"></a><a name = "priority"></a>Öncelik tabanlı trafik-yönlendirme

Genellikle bir kuruluş, birincil birinin kapanması durumunda birden fazla yedekleme hizmeti dağıtarak Hizmetleri için yüksek kullanılabilirlik sağlamak istemektedir. Sektör genelinde, bu topoloji etkin/beklemede veya etkin/Pasif dağıtım topolojisi olarak da adlandırılır. ' Priority ' trafik-yönlendirme yöntemi, Azure müşterilerinin bu yük devretme modelini kolayca uygulamasına olanak tanır.

Varsayılan ön kapılarınız, arka uçların eşit öncelik listesini içerir. Varsayılan olarak, ön kapı trafiği yalnızca üst öncelikli arka uçlara (öncelik için en düşük değer), arka uçların birincil kümesi olarak gönderir. Birincil arka uçlar kullanılamıyorsa, ön kapı trafiği, trafiği ikincil arka uç kümesine yönlendirir (öncelik için ikinci en düşük değer). Birincil ve ikincil arka uçların her ikisi de yoksa, trafik üçüncü olarak gider ve bu şekilde devam eder. Arka ucun kullanılabilirliği, yapılandırılan durumu (etkin veya devre dışı) ve sistem durumu araştırmaları tarafından belirlendiği şekilde devam eden arka uç sistem durumunu temel alır.

### <a name="configuring-priority-for-backends"></a>Backenler için önceliği yapılandırma

Ön kapı yapılandırmasının arka uç havuzundaki her arka uç, 1 ile 5 arasında bir sayı olabilen ' Priority ' adlı bir özelliğe sahiptir. Azure ön kapısının her arka uç için bu özelliği kullanarak arka uç önceliğini açıkça yapılandırırsınız. Bu özellik 1 ile 5 arasında bir değerdir. Daha düşük değerler daha yüksek bir öncelik temsil eder. Backenler, öncelik değerlerini paylaşabilir.

## <a name="weighted-traffic-routing-method"></a><a name = "weighted"></a>Ağırlıklı trafik-yönlendirme yöntemi
' Ağırlıklı ' trafik-yönlendirme yöntemi, trafiği eşit bir şekilde dağıtmanıza veya önceden tanımlanmış bir ağırlığa kullanmanıza olanak sağlar.

Ağırlıklı trafik-yönlendirme yönteminde, arka uç havuzunuzun ön kapı yapılandırmasındaki her bir arka uca bir ağırlık atarsınız. Ağırlık 1-1000 arasında bir tamsayıdır. Bu parametre varsayılan ' 50 ' ağırlığını kullanır.

Kabul edilebilir gecikme süresine sahip kullanılabilir arka uçların listesi ile trafik, belirtilen ağırlıklar oranını kullanarak hepsini bir kez deneme mekanizmasıyla dağıtılır. Gecikme duyarlılığı 0 milisaniyeye ayarlandıysa, aynı ağ gecikmesi ile iki arka uç olmadıkça bu özellik geçerli olmaz. 

Ağırlıklı yöntem bazı faydalı senaryolara izin verebilir:

* **Aşamalı uygulama yükseltme**: yeni bir arka uca yönlendiren trafik yüzdesini verir ve diğer arka uçlarla eşit olarak geçen trafiği giderek zaman içinde arttırır.
* **Azure 'A uygulama geçişi**: hem Azure hem de dış arka uçlarla bir arka uç havuzu oluşturun. Arka uçların kalınlığını, yeni arka uçları tercih etmek için ayarlayın. Bu ayarı, yeni arka uçların devre dışı bırakılması, daha sonra en düşük ağırlıklarla birlikte atamak ve daha fazla trafik aldıkları düzeylere göre daha yavaş bir şekilde artırmak için kademeli olarak ayarlayabilirsiniz. Daha sonra son olarak tercih edilen arka uçları devre dışı bırakıp havuzdan kaldırma.  
* **Bulut-ek kapasite için**: şirket içi bir dağıtımı, ön kapıya geri yerleştirerek buluta hızlıca genişletin. Bulutta ek kapasiteye ihtiyaç duyduğunuzda, daha fazla arka uç ekleyebilir veya etkinleştirebilir ve trafiğin her bir arka uca ne kadar zaman gidebileceklerini belirtebilirsiniz.

## <a name="session-affinity"></a><a name = "affinity"></a>Oturum Benzeşimi
Varsayılan olarak, oturum benzeşimi olmadan ön kapı, istekleri aynı istemciden farklı arka uçlara iletir. Durum bilgisi olan bazı uygulamalar veya belirli senaryolarda aynı kullanıcının istekleri, ilk isteği işleyen arka ucu tercih eder. Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı arka uçta tutmak istediğinizde kullanışlıdır. Azure ön kapısı, yönetilen tanımlama bilgilerini kullanarak bir kullanıcı oturumundan işlenmek üzere aynı arka uca trafiği yönlendirebilir.

Oturum benzeşimi, yapılandırılan etki alanlarınızın (veya alt etki alanlarınızın) her biri için ön uç konağı seviyesinde etkinleştirilebilir. Bu özellik etkinleştirildikten sonra Front Door kullanıcı oturumuna bir tanımlama bilgisi ekler. Front Door, tanımlama bilgisi tabanlı oturum benzeşimi sayesinde aynı IP adresini kullanan farklı kullanıcıları dahi tanımlayabilir ve bu sayede trafiğin farklı arka uçlarınız arasında daha eşit bir şekilde dağıtılmasını sağlayabilir.

Front Door şu an için yalnızca oturum tanımlama bilgisini desteklediğinden tanımlama bilgisinin ömrü kullanıcı oturumunun ömrüyle aynıdır. 

> [!NOTE]
> Ortak proxy 'ler oturum benzeşimine engel olabilir. Bunun nedeni, bir oturumun oluşturulması için ön kapıların, aynı kaynağı isteyen diğer istemcilerin tanımlama bilgilerini kesintiye uğradığı için, yanıt önbelleklenebilir bir oturum benzeşimi tanımlama bilgisi eklemesi gerektirmesidir. Buna karşı korunmak için, arka uç,  bu denendiğinde önbelleklenebilir bir yanıt gönderirse oturum benzeşimi kurulmaz. Oturum zaten kurulduysa, arka ucun yanıtının önbelleklenebilir olup olmadığı önemi yoktur.
> Yanıt bir HTTP 304 durum koduna sahip **olmadığı takdirde** , oturum benzeşimi aşağıdaki koşullarda kurulacaktır:
> - Yanıtta ```Cache-Control``` , "özel" veya mağaza yok "gibi, önbelleğe alma işlemini önleyen üst bilgi için ayarlanan belirli değerler vardır.
> - Yanıt, ```Authorization``` süresi dolmayan bir üst bilgi içeriyor.
> - Yanıtta bir HTTP 302 durum kodu vardır.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
