---
title: Azure ön kapı hizmeti 'nde arka uçların ve arka uç havuzları | Microsoft Docs
description: Bu makalede, ön kapı yapılandırmasında arka uçların ve arka uç havuzlarının ne olduğu açıklanmaktadır.
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
ms.openlocfilehash: b764799d3f40cef24a0412ac950026af650d4ec7
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74229027"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Azure ön kapı hizmeti 'nde arka uçların ve arka uç havuzları
Bu makalede, Azure ön kapı hizmeti ile uygulama dağıtımınızı eşleme kavramları açıklanmaktadır. Ayrıca, uygulama arka uçları etrafında ön kapı yapılandırmasındaki farklı koşulları da açıklar.

## <a name="backends"></a>Arka Uçlar
Bir arka uç, bir bölgedeki dağıtım örneğine eşittir. Ön kapı hizmeti hem Azure hem de Azure olmayan arka uçları destekler, bu nedenle bölge yalnızca Azure bölgeleriyle sınırlandırılır. Ayrıca, şirket içi veri merkeziniz veya başka bir buluttaki bir uygulama örneği olabilir.

Ön kapı hizmeti backenler, uygulamanızın ana bilgisayar adına veya genel IP 'ye başvurur ve bu da istemci isteklerine hizmet verebilir. Arka uçların veritabanı katmanınız, depolama katmanınız ve benzeri bir şekilde karıştırılmamalıdır. Backenler, uygulamanızın arka ucunuzun genel uç noktası olarak görüntülenmelidir. Ön kapı arka uç havuzunda bir arka uç eklediğinizde şunları da eklemeniz gerekir:

- **Arka uç konak türü**. Eklemek istediğiniz kaynak türü. Ön kapı hizmeti, App Service 'ten, bulut hizmetinden veya depolama alanından uygulamanızın arka türlerini otomatik olarak destekler. Azure 'da veya Azure dışı arka uçta farklı bir kaynak istiyorsanız **özel ana bilgisayar**' ı seçin.

    >[!IMPORTANT]
    >Yapılandırma sırasında, arka uçta ön kapı ortamlarından erişilemezse API 'Ler doğrulanmaz. Ön kapıya ait arka uca ulaşabildiğinizden emin olun.

- **Abonelik ve arka uç ana bilgisayar adı**. Arka uç konak türü için **özel konak** seçmediyseniz, Kullanıcı arabiriminde uygun aboneliği ve ilgili arka uç ana bilgisayar adını seçerek arka ucunuzu seçin.

- **Arka uç ana bilgisayar üstbilgisi**. Her istek için arka uca gönderilen ana bilgisayar üst bilgisi değeri. Daha fazla bilgi için bkz. [arka uç ana bilgisayar üstbilgisi](#hostheader).

- **Öncelik**. Tüm trafik için birincil hizmet arka ucu kullanmak istediğinizde farklı arka uçlarınız için öncelikler atayın. Ayrıca, birincil veya yedek arka uçları kullanılamaz durumdaysa yedeklemeler sağlayın. Daha fazla bilgi için bkz. [Öncelik](front-door-routing-methods.md#priority).

- **Ağırlık**. Trafiği eşit veya ağırlık katlarına göre bir dizi arka uçlara dağıtmak için farklı arka uçlarınızın ağırlıklarını atayın. Daha fazla bilgi için bkz. [ağırlıklar](front-door-routing-methods.md#weighted).

### <a name = "hostheader"></a>Arka uç ana bilgisayar üstbilgisi

Ön kapıya ait bir arka uca iletilen istekler, arka ucun hedeflenen kaynağı almak için kullandığı bir konak üstbilgisi alanı içerir. Bu alanın değeri genellikle arka uç URI 'sinden gelir ve ana bilgisayar ve bağlantı noktasına sahiptir.

Örneğin, www\.contoso.com için yapılan bir istek, www\.contoso.com ana bilgisayar başlığına sahip olur. Arka ucunuzu yapılandırmak için Azure portal kullanırsanız, bu alan için varsayılan değer, arka ucun ana bilgisayar adıdır. Arka ucunuz contoso-westus.azurewebsites.net ise Azure portal, arka uç ana bilgisayar üst bilgisi için, tekrar doldurulmuş değer contoso-westus.azurewebsites.net olur. Ancak, bu alanı açıkça ayarlamadan Azure Resource Manager şablonları veya başka bir yöntemi kullanırsanız, ön kapı hizmeti, ana bilgisayar adını konak üstbilgisinin değeri olarak gönderir. İstek, www\.contoso.com için yapıldıysa ve arka ucunuz boş bir üst bilgi alanına sahip contoso-westus.azurewebsites.net ise, ön kapı hizmeti, konak üstbilgisini www\.contoso.com olarak ayarlar.

Çoğu uygulama arka ucu (Azure Web Apps, BLOB depolama ve Cloud Services), ana bilgisayar üstbilgisinin arka ucun etki alanıyla eşleşmesini gerektirir. Ancak, arka ucunuza yönlendiren ön uç ana bilgisayarı, www\.contoso.azurefd.net gibi farklı bir ana bilgisayar adı kullanır.

Arka ucunuz konak üstbilgisinin arka uç ana bilgisayar adıyla eşleşmesini gerektiriyorsa, arka uç konak üstbilgisinin ana bilgisayar adının arka ucunu içerdiğinden emin olun.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Arka uç için arka uç ana bilgisayar üst bilgisini yapılandırma

Arka uç havuzu bölümünde arka uç için **arka uç ana bilgisayar üst bilgisi** alanını yapılandırmak için:

1. Ön kapı kaynağınızı açın ve yapılandırılacak arka uca sahip arka uç havuzunu seçin.

2. Yapmadıysanız, bir arka uç ekleyin veya var olanı düzenleyin.

3. Arka uç ana bilgisayar üst bilgisi alanını özel bir değere ayarlayın veya boş bırakın. Gelen istek için konak adı, ana bilgisayar üst bilgisi değeri olarak kullanılacaktır.

## <a name="backend-pools"></a>Arka uç havuzları
Ön kapı hizmetindeki bir arka uç havuzu, uygulamaları için benzer trafik alan arka uçların kümesini ifade eder. Diğer bir deyişle, aynı trafiği alan ve beklenen davranışla yanıt veren dünya genelindeki uygulama örneklerinizin mantıksal bir gruplandırmasıdır. Bu arka uçlar, farklı bölgelerde veya aynı bölgedeki bir kuruluşta dağıtılır. Tüm arka uçlar etkin/etkin dağıtım modunda ya da etkin/Pasif yapılandırma olarak tanımlanmış olabilir.

Arka uç havuzu, farklı arka uçların sistem durumu araştırmaları aracılığıyla nasıl değerlendirildiğini tanımlar. Ayrıca, bunlar arasında yük dengelemenin nasıl gerçekleşeceğini tanımlar.

### <a name="health-probes"></a>Sistem durumu araştırmaları
Ön kapı hizmeti, yapılandırılmış arka uçlarınızın her birine düzenli HTTP/HTTPS araştırma istekleri gönderir. Araştırma istekleri, Son Kullanıcı isteklerinizin yük dengelenmesi için her bir arka ucun yakınlığını ve sistem durumunu tespit edin. Arka uç havuzu için sistem durumu araştırma ayarları, uygulama arka uçları sistem durumunu nasıl yokladığımızda tanımlar. Yük dengeleme yapılandırması için aşağıdaki ayarlar kullanılabilir:

- **Yol**. Arka uç havuzundaki tüm arka uçlara yönelik araştırma istekleri için kullanılan URL. Örneğin, arka uçlarınızın biri contoso-westus.azurewebsites.net ise ve yol/Probe/test.aspx olarak ayarlandıysa, ön kapı hizmeti ortamları, protokol HTTP olarak ayarlandığında, http\://contoso-westus.azurewebsites.net/probe/test.aspx 'e durum araştırma istekleri gönderir.

- **Protokol**. Ön kapı hizmetinden gelen durum araştırma isteklerinin HTTP veya HTTPS protokolüyle arka uçlara gönderileceğini tanımlar.

- **Aralık (saniye)** . Arka uçlarınızda sistem durumu araştırmalarının sıklığını veya ön kapı ortamlarının her birinin bir araştırma gönderdiği aralıkları tanımlar.

    >[!NOTE]
    >Daha hızlı yük devretme için, aralığı daha düşük bir değere ayarlayın. Değer ne kadar düşükse, arka uçlarınızın aldığı durum araştırma hacmi daha yüksektir. Örneğin, Aralık 90 ön kapı ortamları veya pop 'Lar ile 30 saniyeye ayarlandıysa, her arka uç saniyede yaklaşık 3-5 araştırma isteği alır.

Daha fazla bilgi için bkz. [sistem durumu araştırmaları](front-door-health-probes.md).

### <a name="load-balancing-settings"></a>Yük Dengeleme ayarları
Arka uç havuzu için Yük Dengeleme ayarları, sistem durumu araştırmalarının nasıl değerlendirileceğini tanımlar. Bu ayarlar, arka ucun sağlıklı mı yoksa sağlıksız mi olduğunu belirlenir. Ayrıca, arka uç havuzundaki farklı arka uçlar arasında trafiğin yük dengelemesini de denetler. Yük dengeleme yapılandırması için aşağıdaki ayarlar kullanılabilir:

- **Örnek boyutu**. Arka uç sistem durumu değerlendirmesi için göz önünde bulundurmanız gereken sistem durumu araştırmalarının kaç örnek olduğunu tanımlar.

- **Başarılı örnek boyutu**. Daha önce belirtilen örnek boyutunu, arka uç durumunu çağırmak için gereken başarılı örnek sayısını tanımlar. Örneğin, ön kapılı bir sistem durumu araştırma aralığının 30 saniye, örnek boyutu 5 olduğunu ve başarılı örnek boyutunun 3 olduğunu varsayalım. Arka ucunuzun sistem durumu araştırmalarını değerlendirdiğimiz her seferinde, 150 saniyelik son beş örneği (5 x 30) inceleyeceğiz. Arka ucu sağlıklı olarak bildirmek için en az üç başarılı araştırma gereklidir.

- **Gecikme süresi duyarlılığı (ek gecikme)** . Ön kapısının isteği gecikme ölçümü duyarlılık aralığı içinde arka uçlara göndermesini veya isteği en yakın arka uca iletmesinin gerekip gerekmediğini tanımlar.

Daha fazla bilgi için bkz. [en az gecikmeli tabanlı yönlendirme yöntemi](front-door-routing-methods.md#latency).

## <a name="next-steps"></a>Sonraki adımlar

- [Ön kapı profili oluşturma](quickstart-create-front-door.md)
- [Ön kapı nasıl kullanılır?](front-door-routing-architecture.md)