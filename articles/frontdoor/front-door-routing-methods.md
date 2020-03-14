---
title: Azure ön kapı hizmeti-trafik yönlendirme yöntemleri | Microsoft Docs
description: Bu makale, ön kapı tarafından kullanılan farklı trafik yönlendirme yöntemlerini anlamanıza yardımcı olur
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
ms.openlocfilehash: bd1278db43ba31ed78f13a826a330e16c3bc8d57
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79280800"
---
# <a name="front-door-routing-methods"></a>Ön kapı yönlendirme yöntemleri

Azure ön kapı hizmeti, HTTP/HTTPS trafiğinizi çeşitli hizmet uç noktalarına nasıl yönlendirildiğini öğrenmek için çeşitli trafik yönlendirme yöntemlerini destekler. İstemci isteklerinizin ön kapıya ulaşması sayesinde, isteklerin en iyi arka uç örneğine iletilmesini sağlamak için yapılandırılmış yönlendirme yöntemi uygulanır. 

Trafik yönlendirmesinde ön kapıda kullanılabilen dört ana kavram vardır:

* **[Gecikme süresi](#latency):** Gecikme tabanlı yönlendirme, isteklerin duyarlık aralığı içinde kabul edilebilir en düşük gecikme süreli arka uçlara gönderilmesini sağlar. Temel olarak, Kullanıcı istekleriniz ağ gecikme süresine göre "en yakın" arka uç kümesine gönderilir.
* **[Öncelik](#priority):** Tüm trafik için bir birincil hizmet arka ucu kullanmak istediğinizde, farklı arka uçlarınız için öncelikler atayabilir ve birincil veya yedek arka uçları kullanılamaz duruma geldiğinde yedeklemeler sağlayabilirsiniz.
* **[Ağırlıklı](#weighted):** Trafiği eşit veya ağırlık katlarına göre bir dizi arka uçlara dağıtmak istediğinizde, farklı arka uçlarınızın ağırlıklarını atayabilirsiniz.
* **Oturum benzeşimi:** Kullanıcı oturumu hala etkin olduğu ve arka uç örneği hala sistem durumu araştırmalarını temel alarak sağlıklı bir şekilde raporladığında, bir kullanıcının sonraki isteklerinin aynı arka uca gönderilmesini istediğinizde, ön uç Konakları veya etki alanlarınız için oturum benzeşimini yapılandırabilirsiniz. 

Tüm Front Door yapılandırmaları, arka uç durumu ve otomatik anında global yük devretme izlemesini kapsar. Daha fazla bilgi için bkz. [ön kapı arka uç izleme](front-door-health-probes.md). Ön kapaağınız, tek bir yönlendirme yöntemini temel alarak çalışacak şekilde yapılandırılabilir ve uygulamanızın ihtiyaçlarına bağlı olarak, en iyi yönlendirme topolojisini oluşturmak için bu yönlendirme yöntemlerinin birden çok birini veya tümünü birlikte kullanabilirsiniz.

## <a name = "latency"></a>En düşük gecikme süreleri tabanlı trafik-yönlendirme

Arka uçlarda iki veya daha fazla konumda arka uçlar dağıtmak, trafiği Son kullanıcılarınıza ' en yakın ' olan konuma yönlendirerek birçok uygulamanın yanıt hızını iyileştirebilir. Ön kapı yapılandırmanıza yönelik varsayılan trafik yönlendirme yöntemi, istekleri son kullanıcılarınızın isteği alan ön kapı ortamından en yakın arka uca iletir. Azure ön kapı hizmeti 'nin her noktaya yayın mimarisiyle birlikte kullanıldığında, bu yaklaşım son kullanıcılarınızın her birinin konumlarına göre kişiselleştirilmiş en yüksek performansı almasını sağlar.

' En yakın ' arka ucu coğrafi mesafede ölçülen şekilde en yakın değildir. Bunun yerine, ön kapı ağ gecikmesini ölçerek en yakın arka uçları belirler. [Ön kapısının yönlendirme mimarisi](front-door-routing-architecture.md)hakkında daha fazla bilgi edinin. 

Genel karar akışı aşağıda verilmiştir:

| Kullanılabilir arka uçlar | Öncelik | Gecikme sinyali (sistem durumu araştırmasına göre) | Çizgi |
|-------------| ----------- | ----------- | ----------- |
| İlk olarak, etkin olan ve sistem durumu araştırması için sağlıklı (200 Tamam) olarak döndürülen tüm arka uçları seçin. Yani, A, B, C, D, E ve F altı arka ucu bulunur ve C 'ler arasında iyi durumda ve E 'nin devre dışı bırakılmış olduğunu varsayalım. Bu nedenle, kullanılabilir arka uçların listesi A, B, D ve F 'dir.  | Ardından, kullanılabilir olanlar arasından en üst düzey ön uçlar seçilir. Deyin, arka uç A, B ve D öncelik 1 ve arka uç F 'nin önceliği 2 ' dir. Bu nedenle, seçilen arka uçlar A, B ve D olacaktır.| Gecikme aralığına sahip arka uçları seçin (en az gecikme süresi & gecikme süresi duyarlılığı). A 'nın 15 MS, B ve 60 MS olması, isteğin bulunduğu ön kapı ortamından 30 MS, gecikme süresi duyarlılığı 30 ms ise, en yakın arka uçta 30 MS 'nin ötesinde en düşük gecikme süresine sahip olduğunu varsayalım. | Son olarak, ön kapı, belirtilen ağırlıkların oranıyla, son arka uçların seçili havuzu arasındaki trafiği bir kez daha yuvarlar. Yani, arka uç A 'nın ağırlığı 5, arka uç B ise 8 ağırlığa sahipse trafik, arka uçların A ve B arasındaki 5:8 oranına göre dağıtılır. |

>[!NOTE]
> Varsayılan olarak, gecikme süresi duyarlılığı özelliği 0 MS olarak ayarlanır, diğer bir deyişle, her zaman isteği kullanılabilir en hızlı arka uca iletir.


## <a name = "priority"></a>Öncelik tabanlı trafik-yönlendirme

Genellikle bir kuruluş, birincil hizmetlerinin kapanması olasılığına karşı bir veya daha fazla yedekleme hizmeti dağıtarak Hizmetleri için güvenilirlik sağlamak istemektedir. Sektör genelinde, bu topoloji etkin/beklemede veya etkin/Pasif dağıtım topolojisi olarak da adlandırılır. ' Priority ' trafik-yönlendirme yöntemi, Azure müşterilerinin bu yük devretme modelini kolayca uygulamasına olanak tanır.

Varsayılan ön kapılarınız, arka uçların eşit öncelik listesini içerir. Varsayılan olarak, ön kapı trafiği yalnızca üst öncelikli arka uçlara (öncelik için en düşük değer), arka uçların birincil kümesi olarak gönderir. Birincil arka uçlar kullanılamıyorsa, ön kapı trafiği, trafiği ikincil arka uç kümesine yönlendirir (öncelik için ikinci en düşük değer). Birincil ve ikincil arka uçların her ikisi de kullanılabilir değilse, trafik üçüncü satıra gider ve bu şekilde devam eder. Arka ucun kullanılabilirliği, yapılandırılan durumu (etkin veya devre dışı) ve sistem durumu araştırmaları tarafından belirlendiği şekilde devam eden arka uç sistem durumunu temel alır.

### <a name="configuring-priority-for-backends"></a>Backenler için önceliği yapılandırma

Ön kapı yapılandırmasındaki arka uç havuzunuzdaki her arka uçların her biri, 1 ile 5 arasında bir sayı olabilen ' Priority ' adlı bir özelliğe sahiptir. Azure ön kapı hizmeti ile arka uç önceliğini, her arka uç için bu özelliği kullanarak açık olarak yapılandırırsınız. Bu özellik 1 ile 5 arasında bir değerdir. Daha düşük değerler daha yüksek bir öncelik temsil eder. Backenler, öncelik değerlerini paylaşabilir.

## <a name = "weighted"></a>Ağırlıklı trafik-yönlendirme yöntemi
' Ağırlıklı ' trafik-yönlendirme yöntemi, trafiği eşit bir şekilde dağıtmanıza veya önceden tanımlanmış bir ağırlığa kullanmanıza olanak sağlar.

Ağırlıklı trafik-yönlendirme yönteminde, arka uç havuzunuzun ön kapı yapılandırmasındaki her bir arka uca bir ağırlık atarsınız. Ağırlık 1-1000 arasında bir tamsayıdır. Bu parametre varsayılan ' 50 ' ağırlığını kullanır.

Kabul edilen gecikme duyarlılığı içindeki kullanılabilir arka uçların listesi arasında (belirtildiği gibi), trafik, belirtilen ağırlıkların oranıyla hepsini bir kez deneme mekanizmasıyla dağıtılır. Gecikme duyarlılığı 0 milisaniyeye ayarlanmışsa, bu özellik, aynı ağ gecikme süresine sahip iki arka uç olmadığı takdirde geçerli olmaz. 

Ağırlıklı yöntem bazı faydalı senaryolara izin verebilir:

* **Aşamalı uygulama yükseltme**: yeni bir arka uca yönlendirmek için trafik yüzdesi ayırın ve trafiği diğer arka uçlarla eşit olacak şekilde zaman içinde artırın.
* **Azure 'A uygulama geçişi**: hem Azure hem de dış arka uçlarla bir arka uç havuzu oluşturun. Arka uçların kalınlığını, yeni arka uçları tercih etmek için ayarlayın. Bu ayarı, yeni arka uçların devre dışı bırakılması, daha sonra en düşük ağırlıklarla birlikte atamak ve daha fazla trafik aldıkları düzeylere göre daha yavaş bir şekilde artırmak için kademeli olarak ayarlayabilirsiniz. Daha sonra son olarak tercih edilen arka uçları devre dışı bırakıp havuzdan kaldırma.  
* **Bulut-ek kapasite için**: şirket içi bir dağıtımı, ön kapıya geri yerleştirerek buluta hızlıca genişletin. Bulutta ek kapasiteye ihtiyaç duyduğunuzda, daha fazla arka uç ekleyebilir veya etkinleştirebilir ve trafiğin her bir arka uca ne kadar zaman gidebileceklerini belirtebilirsiniz.

## <a name = "affinity"></a>Oturum benzeşimi
Varsayılan olarak, oturum benzeşimi olmadan, ön kapı aynı istemciden gelen istekleri, Yük Dengeleme yapılandırmasına göre, özellikle farklı arka uçlara yapılan gecikme süreleri veya aynı kullanıcıdan farklı istekler olduğunda farklı bir ön kapılı ortamda yer alır. Ancak belirli diğer senaryolardaki bazı durum bilgisi olan uygulamalar, aynı kullanıcıdan gelen ardışık isteklerin ilk isteği işleyen aynı arka uca yönlendirilmesini tercih etmektedir. Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı arka uçta tutmak istediğinizde kullanışlıdır. Ön kapılı yönetilen tanımlama bilgilerini kullanarak Azure ön kapı hizmeti, arka uç sağlıklı olduğu ve Kullanıcı oturumunun süresinin dolmadığı sürece, sonraki trafiği işlenmek üzere bir kullanıcı oturumundan aynı arka uca yönlendirebilir. 

Oturum benzeşimi, yapılandırılan etki alanlarınızın (veya alt etki alanlarınızın) her biri için ön uç konağı seviyesinde etkinleştirilebilir. Bu özellik etkinleştirildikten sonra Front Door kullanıcı oturumuna bir tanımlama bilgisi ekler. Front Door, tanımlama bilgisi tabanlı oturum benzeşimi sayesinde aynı IP adresini kullanan farklı kullanıcıları dahi tanımlayabilir ve bu sayede trafiğin farklı arka uçlarınız arasında daha eşit bir şekilde dağıtılmasını sağlayabilir.

Front Door şu an için yalnızca oturum tanımlama bilgisini desteklediğinden tanımlama bilgisinin ömrü kullanıcı oturumunun ömrüyle aynıdır. 

> [!NOTE]
> Ortak proxy 'ler oturum benzeşimine engel olabilir. Bunun nedeni, bir oturumun oluşturulması için ön kapıların, aynı kaynağı isteyen diğer istemcilerin tanımlama bilgilerini kesintiye uğradığı için, yanıt önbelleklenebilir bir oturum benzeşimi tanımlama bilgisi eklemesi gerektirmesidir. Buna karşı korunmak için, arka uç, bu denendiğinde önbelleklenebilir bir yanıt gönderirse oturum benzeşimi kurulmaz. Oturum zaten kurulduysa, arka ucun yanıtının önbelleklenebilir olup olmadığı önemi yoktur.
> Yanıt bir HTTP 304 durum koduna sahip **olmadığı takdirde** , oturum benzeşimi aşağıdaki koşullarda kurulacaktır:
> - Yanıtta, "özel" veya mağaza yok "gibi, önbelleğe almayı önleyen ```Cache-Control``` üst bilgisi için ayarlanan belirli değerler vardır.
> - Yanıt, süresi dolmayan bir ```Authorization``` üst bilgisi içeriyor.
> - Yanıtta bir HTTP 302 durum kodu vardır.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
