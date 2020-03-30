---
title: Azure Ön Kapı'da arka uç lar ve arka uç havuzları| Microsoft Dokümanlar
description: Bu makalede, Arka uçlar ve arka uç havuzları Ön Kapı yapılandırmasında ne lerdir açıklanır.
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
ms.openlocfilehash: 18b165d83bfa154348842542bd8323a40330aa2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80293462"
---
# <a name="backends-and-backend-pools-in-azure-front-door"></a>Azure Ön Kapı'da arka uç lar ve arka uç havuzları
Bu makalede, Azure Ön Kapı ile uygulama dağıtımınızın nasıl eşlenizlenene ilişkin kavramlar açıklanmaktadır. Ayrıca uygulama arka uçları etrafında Front Door yapılandırmasında farklı terimler açıklar.

## <a name="backends"></a>Arka Uçlar
Arka uç, bir uygulamanın bir bölgedeki dağıtım örneğine eşittir. Ön Kapı hem Azure hem de Azure olmayan arka uçları destekler, bu nedenle bölge yalnızca Azure bölgeleriyle sınırlı değildir. Ayrıca, şirket içi veri merkeziniz veya başka bir buluttaki bir uygulama örneği olabilir.

Ön Kapı arka uçları, istemci isteklerine hizmet edebilen ana bilgisayar adı veya uygulamanızın genel IP'sine başvurur. Arka uçlar veritabanı katmanınız, depolama katmanınız ve benzeri ile karıştırılmamalıdır. Arka uçlar, uygulama arka uçunuzu genel bitiş noktası olarak görülmelidir. Ön Kapı arka uç havuzuna bir arka uç eklediğinizde, aşağıdakileri de eklemeniz gerekir:

- **Arka uç ana bilgisayar türü.** Eklemek istediğiniz kaynak türü. Ön Kapı, uygulama hizmeti, bulut hizmeti veya depolama dan uygulama arka uçlarınızın otomatik olarak keşfini destekler. Azure'da farklı bir kaynak ve hatta Azure olmayan bir arka uç istiyorsanız, **Özel ana bilgisayar'ı**seçin.

    >[!IMPORTANT]
    >Yapılandırma sırasında, ARKA uç Ön Kapı ortamlarından erişilemez olup olmadığını API'ler doğrulamaz. Ön Kapının arka uca ulaşabileceğinden emin ol.

- **Abonelik ve Backend ana bilgisayar adı.** Arka uç ana **bilgisayar** türü için Özel ana bilgisayar seçmediyseniz, kullanıcı arabirimi'nde uygun aboneliği ve buna karşılık gelen arka uç ana bilgisayar adını seçerek arka uçunuzu seçin.

- **Arka uç ana bilgisayar üstbilgisi**. Her istek için arka uca gönderilen ana bilgisayar üstbilgi değeri. Daha fazla bilgi için [Backend ana bilgisayar üstbilgisine](#hostheader)bakın.

- **Öncelik**. Tüm trafik için birincil hizmet arka uçunu kullanmak istediğinizde öncelikleri farklı arka uçlarınıza atayın. Ayrıca, birincil veya yedek arka uçlar kullanılamıyorsa yedekler sağlayın. Daha fazla bilgi için [Öncelik'e](front-door-routing-methods.md#priority)bakın.

- **Ağırlık**. Trafiği bir dizi arka uça eşit olarak veya ağırlık katsayılarına göre dağıtmak için farklı arka uçlarınıza ağırlık atayın. Daha fazla bilgi için [Ağırlıklar'a](front-door-routing-methods.md#weighted)bakın.

### <a name="backend-host-header"></a><a name = "hostheader"></a>Arka uç ana bilgisayar üstbilgi

Front Door tarafından arka uca iletilen istekler, arka uç hedeflenen kaynağı almak için kullandığı bir ana bilgi alanı içerir. Bu alanın değeri genellikle arka uç URI'den gelir ve ana bilgisayar ve bağlantı noktası vardır.

Örneğin, yapılan `www.contoso.com` bir istek ana bilgisayar üstbilgiwww.contoso.com. Arka uçunuzu yapılandırmak için Azure portalını kullanıyorsanız, bu alanın varsayılan değeri arka uç sahibinin adıdır. Arka ucunuz contoso-westus.azurewebsites.net, Azure portalında, arka uç ana bilgisayar üstbilgisinin otomatik doldurulan değeri contoso-westus.azurewebsites.net. Ancak, bu alanı açıkça ayarlamadan Azure Kaynak Yöneticisi şablonları veya başka bir yöntem kullanırsanız, Ön Kapı gelen ana bilgisayar adını ana bilgisayar üstbilgisinin değeri olarak gönderir. İstek www\.contoso.com için yapılmışsa ve arka uçboş bir üstbilgi alanına sahip contoso-westus.azurewebsites.net ise,\.Ön Kapı ana bilgisayarı www contoso.com olarak ayarlar.

Çoğu uygulama arka uçları (Azure Web Uygulamaları, Blob depolama ve Bulut Hizmetleri) ana bilgisayar üstbilgisinin arka uç etki alanıyla eşleşmesini gerektirir. Ancak, arka uçunuzu giden ön uç ana bilgisayar, www.contoso.net gibi farklı bir ana bilgisayar adı kullanır.

Arka uç, arka uç ana bilgisayar adı ile eşleşecek şekilde ana bilgisayar üstbilgisini gerektiriyorsa, arka uç ana bilgisayar üstbilgisinin ana bilgisayar adını arka ucunu içerdiğinden emin olun.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Arka uç ana bilgisayar üstbilgisini arka uç için yapılandırma

Arka uç **üstbilgi** alanını arka uç havuzu bölümünde ki bir arka uç için yapılandırmak için:

1. Ön Kapı kaynağınızı açın ve yapılandırmak için arka uçlu arka uç havuzunu seçin.

2. Bunu yapmadıysanız bir arka uç ekleyin veya varolan bir tanesini edin.

3. Arka uç ana bilgisayar üstbilgi alanını özel bir değere ayarlayın veya boş bırakın. Gelen isteğin ana bilgisayar adı ana bilgisayar üstbilgi değeri olarak kullanılır.

## <a name="backend-pools"></a>Arka uç havuzları
Front Door'daki arka uç havuzu, uygulaması için benzer trafik alan arka uç kümesini ifade eder. Başka bir deyişle, aynı trafiği alan ve beklenen davranışla yanıt veren, dünya genelindeuygulama örneklerinizin mantıksal bir gruplandırmasIdır. Bu arka uçlar farklı bölgeler arasında veya aynı bölge içinde dağıtılır. Tüm arka uçlar Etkin/Etkin dağıtım modunda veya Etkin/Pasif yapılandırmaolarak tanımlanan modda olabilir.

Arka uç havuzu, farklı arka uçların sağlık sondaları aracılığıyla nasıl değerlendirilmesi gerektiğini tanımlar. Ayrıca, aralarında yük dengelemenin nasıl oluştuğunu da tanımlar.

### <a name="health-probes"></a>Sistem durumu araştırmaları
Ön Kapı, yapılandırılmış arka uçlarınızın her birine periyodik HTTP/HTTPS sonda istekleri gönderir. Sonda istekleri, son kullanıcı isteklerinizi dengelemek için her arka uça olan yakınlığı ve sistem durumunu belirler. Arka uç havuzu için sistem durumu sondaayarları, uygulama arka uçlarının sistem durumu durumunu nasıl yokladiğimizı tanımlar. Yük dengeleme yapılandırması için aşağıdaki ayarlar kullanılabilir:

- **Yol**: Arka uç havuzundaki tüm arka uçlar için sonda istekleri için kullanılan URL. Örneğin, arka uçlarınızdan biri contoso-westus.azurewebsites.net ve yol /probe/test.aspx olarak ayarlanmışsa, protokolün HTTP olarak ayarlı olduğunu varsayarak Ön Kapı ortamları http\://contoso-westus.azurewebsites.net/probe/test.aspx'a sistem durumu sondası istekleri gönderir.

- **Protokol**: Sağlık sondası isteklerini Ön Kapı'dan http veya HTTPS protokolü ile arka uçlarınıza gönderip göndermeyeceklerini tanımlar.

- **Yöntem**:Sağlık sondaları göndermek için kullanılacak HTTP yöntemi. Seçenekler get veya HEAD (varsayılan) içerir.
    > [!NOTE]
    > Arka uçlarınıza daha düşük yük ve maliyet için, Ön Kapı sağlık sondaları için HEAD isteklerini kullanmanızı önerir.

- **Aralık (saniye)**: Arka uçlarınıza sağlık sondalarının sıklığını veya Ön Kapı ortamlarının her birinin sonda gönderdiği aralıkları tanımlar.

    >[!NOTE]
    >Daha hızlı arızalar için aralığı daha düşük bir değere ayarlayın. Değer ne kadar düşükse, arka uçlarınızın aldığı sağlık sondası hacmi de o kadar yüksek olur. Örneğin, aralık dünya çapında 100 Ön Kapı POP'u ile 30 saniyeye ayarlanırsa, her arka uç dakikada yaklaşık 200 sonda isteği alır.

Daha fazla bilgi için [Bkz. Sağlık sondaları.](front-door-health-probes.md)

### <a name="load-balancing-settings"></a>Yük dengeleme ayarları
Arka uç havuzu için yük dengeleme ayarları, sistem sondalarını nasıl değerlendirdiğimizi tanımlar. Bu ayarlar, arka uç sağlıklı veya sağlıksız olup olmadığını belirler. Ayrıca arka uç havuzunda farklı arka uçlar arasındaki trafiği nasıl dengelerek kontrol ederler. Yük dengeleme yapılandırması için aşağıdaki ayarlar kullanılabilir:

- **Örnek boyutu**. Arka uç sağlık değerlendirmesi için kaç tane sağlık sondası örneği düşünmemiz gerektiğini tanımlar.

- **Başarılı örnek boyutu**. Daha önce belirtildiği gibi örnek boyutunu tanımlar, arka uç sağlıklı aramak için gerekli başarılı örneklerin sayısı. Örneğin, bir Ön Kapı sağlık sondası aralığının 30 saniye, örnek boyutunun 5 ve başarılı örnek boyutunun 3 olduğunu varsayalım. Arka uç için sağlık sondalarını her değerlendirdiğimizde, 150 saniyenin (5 x 30) üzerindeki son beş örneğine bakıyoruz. Arka uçun sağlıklı olduğunu bildirmek için en az üç başarılı sonda gereklidir.

- **Gecikme hassasiyeti (ek gecikme)**. Front Door'un gecikme ölçer duyarlılık aralığında arka uçlara istek göndermesini mi yoksa isteği en yakın arka uca iletmesini mi istediğinizi tanımlar.

Daha fazla bilgi için bkz: [En Az gecikme tabanlı yönlendirme yöntemi.](front-door-routing-methods.md#latency)

## <a name="next-steps"></a>Sonraki adımlar

- [Ön Kapı profili oluşturma](quickstart-create-front-door.md)
- [Ön Kapı nasıl çalışır?](front-door-routing-architecture.md)