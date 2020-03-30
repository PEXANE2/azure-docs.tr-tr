---
title: Azure Özel Bağlantı hizmeti nedir?
description: Azure Özel Bağlantı hizmeti hakkında bilgi edinin.
services: private-link
author: sumeetmittal
ms.service: private-link
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: sumi
ms.openlocfilehash: 2cc6c577abdb3698ef6aca1f1f04d239f09d119c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280436"
---
# <a name="what-is-azure-private-link-service"></a>Azure Özel Bağlantı hizmeti nedir?

Azure Private Link hizmeti, Azure Private Link tarafından desteklenen kendi hizmetinizin başvurusudur. Azure Standart Yük [Dengeleyicisi'nin](../load-balancer/load-balancer-standard-overview.md) arkasında çalışan hizmetiniz, özel bağlantı erişimi için etkinleştirilebilir, böylece hizmetinizdeki tüketiciler kendi VNet'lerinden özel olarak erişebilir. Müşterileriniz VNet'lerinin içinde özel bir bitiş noktası oluşturabilir ve bu hizmetle eşleyebilir. Bu makalede, servis sağlayıcı tarafı ile ilgili kavramlar açıklanmaktadır. 

## <a name="workflow"></a>İş akışı

![Private Link hizmet iş akışı](media/private-link-service-overview/private-link-service-workflow.png)

### <a name="create-your-private-link-service"></a>Özel Bağlantı Hizmetinizi Oluşturun

- Uygulamanızı sanal ağınızdaki standart bir yük dengeleyicinin arkasında çalışacak şekilde yapılandırın. Uygulamanız zaten standart bir yük dengeleyicisi arkasında yapılandırıldıysa, bu adımı atlayabilirsiniz.   
- Yukarıdaki yük bakiyesini referans alan özel bağlantı hizmeti oluşturun. Yük dengeleyici seçim işleminde, trafiği almak istediğiniz ön uç IP yapılandırmasını seçin. Özel Bağlantı Hizmeti için NAT IP adresleri için bir alt ağ seçin. Alt ağda en az sekiz NAT IP adresi bulunması önerilir. Tüm tüketici trafiği, bu özel IP adresleri havuzundan hizmet sağlayıcısına ait gibi görünür. Özel Bağlantı Hizmeti için uygun özellikleri/ayarları seçin.    

    > [!NOTE]
    > Azure Özel Bağlantı Hizmeti yalnızca Standart Yük Dengeleyicisi'nde desteklenir. 
    
### <a name="share-your-service"></a>Hizmetinizi paylaşın

Bir Özel Bağlantı hizmeti oluşturduktan sonra Azure, hizmetiniz için sağladığınız ada bağlı olarak genel olarak benzersiz adlandırılmış "takma ad" oluşturur. Hizmetinizin diğer adını veya kaynak URI'sini müşterilerinizle çevrimdışı olarak paylaşabilirsiniz. Tüketiciler, diğer adı veya KAYNAK URI'yi kullanarak Özel Bağlantı bağlantısı başlatabilir.
 
### <a name="manage-your-connection-requests"></a>Bağlantı isteklerinizi yönetme

Bir tüketici bir bağlantı başlattıktan sonra, hizmet sağlayıcısı bağlantı isteğini kabul edebilir veya reddedebilir. Tüm bağlantı istekleri, Private Link hizmetindeki **privateendpointconnections** özelliği altında listelenir.
 
### <a name="delete-your-service"></a>Hizmetinizi silme

Private Link hizmeti artık kullanılıyorsa, silebilirsiniz. Ancak, hizmeti silmeden önce, hizmetle ilişkili özel bitiş noktası bağlantısı olmadığından emin olun. Tüm bağlantıları reddedebilir ve hizmeti silebilirsiniz.

## <a name="properties"></a>Özellikler

Private Link hizmeti aşağıdaki özellikleri belirtir: 

|Özellik |Açıklama  |
|---------|---------|
|Devlet Sağlama (provisioningState)  |Özel Bağlantı hizmeti için geçerli sağlama durumunu listeleyen salt okunur özelliği. Yürürlükteki hüküm veren devletler şunlardır: "Silme; Başarısız oldu; Başarılı oldu; Güncelleme". Sağlama durumu "Başarılı" olduğunda, Private Link hizmetinizi başarıyla sağlamış olursunuz.        |
|Diğer ad (diğer ad)     | Diğer ad, hizmetiniz için benzersiz bir salt okunur dizedir. Hizmetiniz için müşteri verilerini maskelemenize yardımcı olur ve aynı zamanda hizmetiniz için paylaşılaması kolay bir ad oluşturur. Bir Özel Bağlantı hizmeti oluşturduğunuzda, Azure hizmetinizin diğer adını müşterilerinizle paylaşabilir. Müşterileriniz bu diğer adı hizmetinize bağlantı istemek için kullanabilir.          |
|Görünürlük (görünürlük)     | Görünürlük, Özel Bağlantı hizmetinizin pozlama ayarlarını kontrol eden özelliktir. Hizmet sağlayıcıları, rol tabanlı erişim denetimi (RBAC) izinleri, sınırlı abonelikler kümesi veya tüm Azure abonelikleri ile hizmetlerine maruz kalma süresini aboneliklerle sınırlamayı seçebilir.          |
|Otomatik Onay (autoApproval)    |   Otomatik onay, Özel Bağlantı hizmetine otomatik erişimi denetler. Otomatik onay listesinde belirtilen abonelikler, bu aboneliklerde özel uç noktalardan bağlantı istendiğinde otomatik olarak onaylanır.          |
|Yük Dengeleyici Frontend IP Konfigürasyonu (loadBalancerFrontendIpConfigurations)    |    Private Link hizmeti, Standart Yük Dengeleyicisinin ön uç IP adresine bağlıdır. Hizmet için mukadder tüm trafik SLB ön ucuna ulaşacaktır. SLB kurallarını, bu trafiği uygulamalarınızın çalıştığı uygun arka uç havuzlarına yönlendirecek şekilde yapılandırabilirsiniz. Yük dengeleyici frontend IP yapılandırmaları NAT IP yapılandırmalarından farklıdır.      |
|NAT IP Yapılandırması (ipConfigurations)    |    Bu özellik, Özel Bağlantı hizmeti için NAT (Ağ Adresi Çevirisi) IP yapılandırmasına başvurur. NAT IP, bir hizmet sağlayıcısının sanal ağındaki herhangi bir alt ağdan seçilebilir. Private Link hizmeti, Özel Bağlantı trafiğinde hedef tarafı NAT-ing gerçekleştirir. Bu, kaynak (tüketici tarafı) ve hedef (servis sağlayıcı) adres alanı arasında IP çakışması olmamasını sağlar. Hedef tarafta (servis sağlayıcı tarafında), Hizmetiniz tarafından alınan tüm paketler için Kaynak IP olarak NAT IP adresi ve hizmetiniz tarafından gönderilen tüm paketler için hedef IP olarak gösterilecektir.       |
|Özel bitiş noktası bağlantıları (privateEndpointConnections)     |  Bu özellik, Özel Bağlantı hizmetine bağlanan özel uç noktaları listeler. Birden çok özel uç nokta aynı Özel Bağlantı hizmetine bağlanabilir ve hizmet sağlayıcısı durumu tek tek özel uç noktalar için denetleyebilir.        |
|TCP Proxy V2 (EnableProxyProtocol)     |  Bu özellik, hizmet sağlayıcısının hizmet tüketicisi hakkındaki bağlantı bilgilerini almak için tcp proxy v2 kullanmasına olanak tanır. Servis Sağlayıcı proxy protokolü v2 üstbilgi ayrıştırmak edebilmek için alıcı configs kurmakla sorumludur.        |
|||


### <a name="details"></a>Ayrıntılar

- Özel Bağlantı hizmetine aynı bölgedeki onaylı özel uç noktalardan erişilebilir. Özel bitiş noktasına aynı sanal ağdan, bölgesel olarak bakıldığında VNet'lerden, küresel olarak bakıldığında VNet'lerden ve özel VPN veya ExpressRoute bağlantıları kullanılarak şirket içinde ulaşılabilir. 
 
- Özel Bağlantı Hizmeti oluşturulurken, kaynağın yaşam döngüsü için bir ağ arabirimi oluşturulur. Bu arabirim müşteri tarafından yönetilemez.
 
- Özel Bağlantı Hizmeti, sanal ağ ve Standart Yük Dengeleyicisi ile aynı bölgede dağıtılmalıdır.  
 
- Tek bir Özel Bağlantı Hizmetine farklı VNet'lere, aboneliklere ve/veya Active Directory kiracılarına ait birden çok Özel Uç Noktadan erişilebilir. Bağlantı bir bağlantı iş akışı ile kurulur. 
 
- Aynı Standart Yük Dengeleyicisi'nde farklı ön uç IP yapılandırmaları kullanılarak birden fazla Private Link hizmeti oluşturulabilir. Standart Yük Dengeleyicisi ve abonelik başına oluşturabileceğiniz Özel Bağlantı hizmetlerinin sayısının sınırları vardır. Ayrıntılar için [Azure sınırlarına](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits)bakın.
 
- Private Link hizmetinde birden fazla NAT IP yapılandırması olabilir. Birden fazla NAT IP yapılandırması seçmek, servis sağlayıcılarının ölçeklendirmesine yardımcı olabilir. Bugün, hizmet sağlayıcılar Özel Bağlantı hizmeti başına en fazla sekiz NAT IP adresi atayabilir. Her NAT IP adresiyle, TCP bağlantılarınız için daha fazla bağlantı noktası atayabilir ve böylece ölçeklendirebilirsiniz. Özel Bağlantı hizmetine birden çok NAT IP adresi ekledikten sonra, NAT IP adreslerini silemezsiniz. Bu, NAT IP adreslerini silerken etkin bağlantıların etkilenmemesini sağlamak için yapılır.


## <a name="alias"></a>Diğer ad

**Diğer ad,** hizmetiniz için benzersiz bir addır. Hizmetiniz için müşteri verilerini maskelemenize yardımcı olur ve aynı zamanda hizmetiniz için paylaşılaması kolay bir ad oluşturur. Bir Özel Bağlantı hizmeti oluşturduğunuzda, Azure hizmetinizin müşterilerinizle paylaşabileceği bir takma ad oluşturur. Müşterileriniz bu diğer adı hizmetinize bağlantı istemek için kullanabilir.

Takma ad üç bölümden oluşur: *Önek*. *YOL GÖSTERICI*. *Sonek*

- Önek hizmet adıdır. Kendi önek seçebilirsiniz. "Diğer Ad" oluşturulduktan sonra değiştiremezsiniz, bu nedenle önekinizi uygun şekilde seçin.  
- GUID platformu tarafından sağlanacaktır. Bu, adın genel olarak benzersiz hale getirilen bir hale yardımcı olur. 
- Sonek Azure tarafından eklenir: *region*.azure.privatelinkservice 

Tam takma ad: *Önek*. {GUID}. *bölge*.azure.privatelinkservice  

## <a name="control-service-exposure"></a>Kontrol hizmeti pozlama

Private Link hizmeti, "Görünürlük" ayarı ile hizmetinizin maruz kalmasını kontrol etme seçenekleri sunar. Hizmeti sahip olduğunuz farklı VNet'lerden tüketim için özel hale getirebilir, güvendiğiniz sınırlı abonelikler kümesiyle pozlamayı kısıtlayabilir veya tüm Azure aboneliklerinin Özel Bağlantı'da bağlantı isteyebilmeleri için herkese açık hale getirebilirsiniz Hizmet. Görünürlük ayarlarınız, bir tüketicinin hizmetinize bağlanıp bağlanamayacağına karar verir. 

## <a name="control-service-access"></a>Hizmet erişimini kontrol edin

Özel Bağlantı hizmetinize maruz kalma (görünürlük ayarı tarafından kontrol edilen) tüketiciler, VNet'lerinde özel bir bitiş noktası oluşturabilir ve Özel Bağlantı hizmetinize bağlantı talep edebilir. Özel bitiş noktası bağlantısı, Özel Bağlantı hizmeti nesnesinde "Beklemede" durumunda oluşturulur. Servis sağlayıcı, bağlantı isteğine göre hareket etmekle yükümlüdür. Bağlantıyı onaylayabilir, bağlantıyı reddedebilir veya bağlantıyı silebilirsiniz. Yalnızca onaylanan bağlantılar, Özel Bağlantı hizmetine trafik gönderebilir.

Bağlantıları onaylama eylemi, Özel Bağlantı hizmetindeki otomatik onay özelliği kullanılarak otomatikleştirilmiş olabilir. Otomatik Onay, hizmet sağlayıcıların hizmetlerine otomatik erişim için bir abonelik kümesini önceden onaylama olanağıdır. Müşterilerin, servis sağlayıcılarının otomatik onay listesine eklemeleri için aboneliklerini çevrimdışı olarak paylaşmaları gerekir. Otomatik onay, görünürlük dizisinin bir alt kümesidir. Görünürlük pozlama ayarlarını kontrol ederken, otomatik onay hizmetinizin onay ayarlarını kontrol eder. Bir müşteri otomatik onay listesindeki bir abonelikten bağlantı isterse, bağlantı otomatik olarak onaylanır ve bağlantı kurulur. Hizmet sağlayıcıların artık isteği el ile onaylaması gerekmez. Diğer taraftan, bir müşteri otomatik onay dizisinde değil de görünürlük dizisindeki bir abonelikten bağlantı isterse, istek servis sağlayıcısına ulaşır, ancak servis sağlayıcısının bağlantıları el ile onaylaması gerekir.

## <a name="getting-connection-information-using-tcp-proxy-v2"></a>TCP Proxy v2 kullanarak bağlantı Bilgileri alma

Özel bağlantı hizmeti kullanırken, özel uç noktadan gelen paketlerin kaynak IP adresi, sağlayıcının sanal ağından ayrılan NAT IP'yi kullanarak servis sağlayıcı tarafında çevrilmiş ağ adresidir (NAT). Bu nedenle uygulamalar, hizmet tüketicilerinin gerçek kaynak IP adresi yerine ayrılan NAT IP adresini alır. Uygulamanızın tüketici tarafından gerçek kaynak IP adresine ihtiyacı varsa, hizmetinizde Proxy protokolünü etkinleştirebilir ve proxy protokolü üstbilgisinden bilgileri alabilirsiniz. Kaynak IP adresine ek olarak, proxy iletişim üstbilgisi de özel bitiş noktasının LinkID'sini taşır. Kaynak IP adresi ve LinkID kombinasyonu, hizmet sağlayıcıların tüketicilerini benzersiz bir şekilde tanımlamalarına yardımcı olabilir. Proxy Protokolü hakkında daha fazla bilgi için burayı ziyaret [edin.](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) 

Bu bilgiler, özel bir Tür-Uzunluk-Değer (TLV) vektörü kullanılarak aşağıdaki gibi kodlanır:

Özel TLV detayları:

|Alan |Uzunluk (Sekizli)  |Açıklama  |
|---------|---------|----------|
|Tür  |1        |PP2_TYPE_AZURE (0xEE)|
|Uzunluk  |2      |Değer uzunluğu|
|Değer  |1     |PP2_SUBTYPE_AZURE_PRIVATEENDPOINT_LINKID (0x01)|
|  |4        |Özel bitiş noktasının LINKID'sini temsil eden UINT32 (4 bayt). Küçük endian biçiminde kodlanmış.|

 > [!NOTE]
 > Hizmet sağlayıcısı, standart yük dengeleyicisinin arkasındaki hizmetin, özel bağlantı hizmetinde proxy protokolü etkinleştirildiğinde [belirtime](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) göre proxy protokol üstbilgisini ayrıştıracak şekilde yapılandırıldığından emin olmakiçin sorumludur. Proxy protokolü ayarı özel bağlantı hizmetinde etkinleştirilirse, ancak servis sağlayıcının hizmeti üstbilgiyi ayrıştıracak şekilde yapılandırılmamışsa, istek başarısız olur. Benzer şekilde, servis sağlayıcının hizmeti bir proxy iletişim üstbilgisi beklerken, ayar özel bağlantı hizmetinde etkinleştirilmemişse, istek başarısız olur. Proxy protokolü ayarı etkinleştirildikten sonra, üstbilgide istemci bilgisi olmamasına rağmen proxy iletişim üstbilgisi ana bilgisayardan arka uç sanal makinelere kadar HTTP/TCP sistem durumu sondalarına da dahil edilecektir. 

## <a name="limitations"></a>Sınırlamalar

Özel Bağlantı hizmetini kullanırken bilinen sınırlamalar şunlardır:
- Yalnızca Standart Yük Dengeleyicisi ile desteklenir 
- Yalnızca IPv4 trafiğini destekler
- Yalnızca TCP trafiğini destekler

## <a name="next-steps"></a>Sonraki adımlar
- [Azure PowerShell'i kullanarak özel bağlantı hizmeti oluşturma](create-private-link-service-powershell.md)
- [Azure CLI'yi kullanarak özel bir bağlantı hizmeti oluşturma](create-private-link-service-cli.md)
