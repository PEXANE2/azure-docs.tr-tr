---
title: SSS - Azure DNS
description: Bu makalede, Azure DNS hakkında sık sorulan sorular hakkında bilgi edinin
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 6/15/2019
ms.author: rohink
ms.openlocfilehash: 76b19cfb3c00a26d81eab81f67d8e156a520f377
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77121726"
---
# <a name="azure-dns-faq"></a>Azure DNS SSS

## <a name="about-azure-dns"></a>Azure DNS Hakkında

### <a name="what-is-azure-dns"></a>Azure DNS nedir?

Alan Adı Sistemi (DNS), bir web sitesini veya hizmet adını IP adresine çevirir veya çözer. Azure DNS, DNS etki alanları için bir barındırma hizmetidir. Microsoft Azure altyapılarını kullanarak ad çözümlemesi sağlar. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Azure DNS'deki DNS etki alanları, DNS ad sunucularının Azure küresel ağında barındırılır. Bu sistem, her DNS sorgusunun kullanılabilir en yakın DNS sunucusu tarafından yanıtlanması için Anycast ağlarını kullanır. Azure DNS, etki alanınız için hızlı performans ve yüksek kullanılabilirlik sağlar.

Azure DNS, Azure Kaynak Yöneticisi'ni temel almaktadır. Azure DNS, rol tabanlı erişim denetimi, denetim günlükleri ve kaynak kilitleme gibi Kaynak Yöneticisi özelliklerinden yararlanır. Etki alanlarını ve kayıtları Azure portalı, Azure PowerShell cmdlets ve çapraz platform Azure CLI üzerinden yönetebilirsiniz. Otomatik DNS yönetimi gerektiren uygulamalar, REST API ve SDK'lar aracılığıyla hizmetle tümleştirilebilir.

### <a name="how-much-does-azure-dns-cost"></a>Azure DNS'nin maliyeti nedir?

Azure DNS faturalandırma modeli, Azure DNS'de barındırılan DNS bölgeleri sayısını temel alır. Ayrıca aldıkları DNS sorgularının sayısına da bağlıdır. İndirimler kullanıma göre sağlanmaktadır.

Daha fazla bilgi için [Azure DNS fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/dns/)bakın.

### <a name="what-is-the-sla-for-azure-dns"></a>Azure DNS hizmetinin SLA değeri kaçtır?

Azure, geçerli DNS isteklerinin en az bir Azure DNS ad sunucusundan %100 yanıt almasını garanti eder.

Daha fazla bilgi için [Azure DNS SLA sayfasına](https://azure.microsoft.com/support/legal/sla/dns)bakın.

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>DNS bölgesi nedir? DNS etki alanıyla aynı mı? 

Etki alanı, etki alanı adı sisteminde benzersiz bir addır. Örneğin: contoso.com.

DNS bölgesi, belirli bir etki alanına ait DNS kayıtlarını barındırmak için kullanılır. Örneğin, etki alanı contoso.com birkaç DNS kaydı içerebilir. Kayıtlar, posta sunucusu nun mail.contoso.com\.ve bir web sitesi için www contoso.com içerebilir. Bu kayıtlar Contoso.com DNS bölgesinde barındırılır.

Etki alanı adı *sadece bir addır.* DNS bölgesi, etki alanı adı için DNS kayıtlarını içeren bir veri kaynağıdır. Azure DNS’yi kullanarak bir DNS bölgesi barındırabilir ve Azure'da bir etki alanının DNS kayıtlarını yönetebilirsiniz. Ayrıca, DNS ad sunucularının Internet'ten DNS sorgularını yanıtlamasını sağlar.

### <a name="do-i-need-to-buy-a-dns-domain-name-to-use-azure-dns"></a>Azure DNS'yi kullanmak için bir DNS etki alanı adı satın almam gerekiyor mu? 

Mutlaka.

Azure DNS'de bir DNS bölgesini barındırmak için bir etki alanı satın almanız gerekmez. Etki alanı adına sahip olmadan istediğiniz zaman bir DNS bölgesi oluşturabilirsiniz. Bu bölge için DNS sorguları yalnızca bölgeye atanan Azure DNS ad sunucularına yönlendirildiklerinde çözülür.

DNS bölgenizi genel DNS hiyerarşisine bağlamak için etki alanı adını satın almanız gerekir. Ardından, dünyanın herhangi bir yerinden gelen DNS sorguları DNS bölgenizi bulur ve DNS kayıtlarınız ile yanıtlar.

## <a name="azure-dns-features"></a>Azure DNS özellikleri

### <a name="are-there-any-restrictions-when-using-alias-records-for-a-domain-name-apex-with-traffic-manager"></a>Trafik Yöneticisi ile bir etki alanı adı apeks için takma ad kayıtları kullanırken herhangi bir kısıtlama var mı?

Evet. Azure Trafik Yöneticisi ile statik genel IP adreslerini kullanmanız gerekir. Statik bir IP adresi kullanarak **Dış uç nokta** hedefini yapılandırın. 

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS, DNS tabanlı trafik yönlendirmeyi veya bitiş noktası başarısızolup olmadığını destekliyor mu?

DNS tabanlı trafik yönlendirme ve bitiş noktası ayarı Trafik Yöneticisi tarafından sağlanır. Traffic Manager, Azure DNS ile kullanılabilecek ayrı bir Azure hizmetidir. Daha fazla bilgi için [Trafik Yöneticisi'ne genel bakış](../traffic-manager/traffic-manager-overview.md)bakın.

Azure DNS yalnızca belirli bir DNS kaydı için her DNS sorgusunun her zaman aynı DNS yanıtını aldığı statik DNS etki alanlarını barındırmayı destekler.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS etki alanı adı kaydını destekliyor mu?

Hayır. Azure DNS şu anda alan adları satın alma seçeneğini desteklemez. Etki alanları satın almak için bir üçüncü taraf etki alanı adı kayıt şirketi kullanmanız gerekir. Kayıt şirketi genellikle küçük bir yıllık ücret alır. Etki alanları daha sonra DNS kayıtlarının yönetimi için Azure DNS'de barındırılabilir. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md).

Alan adları satın alma özelliği Azure biriktirme listesinde izlenir. [Bu özellik için desteğinizi kaydetmek için](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar)geri bildirim sitesini kullanın.

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS DNS'yi destekliyor mu?

Hayır. Azure DNS şu anda Etki Alanı Adı Sistemi Güvenlik Uzantılarını (DNSSEC) desteklemez.

DNSSEC özelliği Azure DNS biriktirme listesinde izlenir. [Bu özellik için desteğinizi kaydetmek için](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support)geri bildirim sitesini kullanın.

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Azure DNS bölge aktarımlarını (AXFR/IXFR) destekliyor mu?

Hayır. Azure DNS şu anda bölge aktarımlarını desteklemiyor. DNS bölgeleri [Azure CLI kullanılarak Azure DNS'ye aktarılabilir.](dns-import-export.md) DNS kayıtları Azure [DNS yönetim portalı,](dns-operations-recordsets-portal.md) [REST API,](https://docs.microsoft.com/powershell/module/az.dns) [SDK,](dns-sdk.md) [PowerShell cmdlets](dns-operations-recordsets.md)veya [CLI aracı](dns-operations-recordsets-cli.md)üzerinden yönetilir.

Bölge aktarım özelliği Azure DNS biriktirme listesinde izlenir. [Bu özellik için desteğinizi kaydetmek için](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c)geri bildirim sitesini kullanın.

### <a name="does-azure-dns-support-url-redirects"></a>Azure DNS, URL'yi destekliyor mu?

Hayır. URL yönlendirme hizmetleri bir DNS hizmeti değildir. DNS düzeyinde değil, HTTP düzeyinde çalışırlar. Bazı DNS sağlayıcıları, genel tekliflerinin bir parçası olarak bir URL yönlendirme hizmetini paketler. Bu hizmet şu anda Azure DNS tarafından desteklenmez.

URL yönlendirme özelliği Azure DNS biriktirme listesinde izlenir. [Bu özellik için desteğinizi kaydetmek için](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape)geri bildirim sitesini kullanın.

### <a name="does-azure-dns-support-the-extended-ascii-encoding-8-bit-set-for-txt-record-sets"></a>Azure DNS, TXT kayıt kümeleri için genişletilmiş ASCII kodlama (8 bit) kümesini destekliyor mu?

Evet. Azure DNS, TXT kayıt kümeleri için genişletilmiş ASCII kodlama kümesini destekler. Ancak Azure REST API'lerinin, SDK'ların, PowerShell'in ve CLI'nin en son sürümünü kullanmanız gerekir. 1 Ekim 2017 veya SDK 2.1'den daha eski sürümler genişletilmiş ASCII kümesini desteklemez. 

Örneğin, genişletilmiş ASCII karakteri \128 olan bir TXT kaydının değeri olarak bir dize sağlayabilirsiniz. Bir örnek "abcd\128efgh." Azure DNS, 128 olan bu karakterin dahili gösteriminde bayt değerini kullanır. DNS çözümlemesi sırasında, bu bayt değeri yanıt olarak döndürülür. Ayrıca "abc" ve "\097\098\099" çözünürlüğü söz konusu olduğunda değiştirilebilir olduğunu unutmayın. 

TXT kayıtları için [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) bölge dosya ana biçimi kaçış kurallarını takip ediyoruz. Örneğin, `\` şimdi aslında RFC başına her şeyi kaçar. TXT `A\B` kayıt değeri olarak belirtirseniz, temsil edilir ve sadece `AB`olarak çözülür. TXT kaydının gerçekten çözümde `A\B` olmasını istiyorsanız, `\` tekrar kaçmanız gerekir. Örnek olarak, `A\\B`belirtin.

Bu destek şu anda Azure portalından oluşturulan TXT kayıtları için kullanılamaz.

## <a name="alias-records"></a>Diğer ad kayıtları

### <a name="what-are-some-scenarios-where-alias-records-are-useful"></a>Takma ad kayıtlarının yararlı olduğu bazı senaryolar nelerdir?

[Azure DNS takma adı kayıtlarına genel bakışbölümündeki](dns-alias.md)senaryolar bölümüne bakın.

### <a name="what-record-types-are-supported-for-alias-record-sets"></a>Diğer ad kayıt kümeleri için hangi kayıt türleri desteklenir?

Diğer ad kayıt kümeleri, bir Azure DNS bölgesinde aşağıdaki kayıt türleri için desteklenir:
 
- A 
- AAAA
- CNAME 

### <a name="what-resources-are-supported-as-targets-for-alias-record-sets"></a>Diğer ad kayıt kümeleri için hedef olarak hangi kaynaklar desteklenir?

- **DNS A/AAAA kayıt kümesinden ortak bir IP kaynağına işaret edin.** Bir A/AAAA kayıt kümesi oluşturabilir ve bunu ortak bir IP kaynağını işaret etmek için ayarlanmış bir takma ad kaydı yapabilirsiniz.
- **DNS A/AAAA/CNAME kayıt kümesinden Trafik Yöneticisi profilini işaret edin.** Bir Trafik Yöneticisi profilinin CNAME'sini DNS CNAME kayıt kümesinden işaret edebilirsiniz. Bir örnek contoso.trafficmanager.net. Artık, DNS bölgenizde ayarlanan bir A veya AAAA kaydından dış uç noktaları olan bir Trafik Yöneticisi profiline de işaret edebilirsiniz.
- **Azure İçerik Dağıtım Ağı (CDN) bitiş noktasını işaret edin.** Bu, Azure depolama ve Azure CDN kullanarak statik web siteleri oluşturduğunuzda yararlıdır.
- **Aynı bölge içinde ayarlanan başka bir DNS kaydını işaret edin.** Diğer ad kayıtları, aynı türden diğer kayıt kümelerine başvurulabilir. Örneğin, bir DNS CNAME kayıt kümesinin aynı türdeki başka bir CNAME kayıt kümesine diğer ad olmasını sağlayabilirsiniz. Bu düzenleme, bazı kayıt kümelerinin diğer adlar ve bazı takma ad olmayan lar olmasını istiyorsanız yararlıdır.

### <a name="can-i-create-and-update-alias-records-from-the-azure-portal"></a>Azure portalından takma ad kayıtları oluşturabilir ve güncelleyebilir miyim?

Evet. Azure REST API'leri, PowerShell, CLI ve SDK'larla birlikte Azure portalında takma ad kayıtları oluşturabilir veya yönetebilirsiniz.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-deleted-when-the-underlying-public-ip-is-deleted"></a>Diğer ad kayıtları, temel genel IP silindiğinde DNS kayıt setimin silindiğinden emin olmaya yardımcı olur mu?

Evet. Bu özellik, diğer ad kayıtlarının temel özelliklerinden biridir. Uygulamanızın kullanıcıları için olası kesintileri önlemenize yardımcı olur.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-updated-to-the-correct-ip-address-when-the-underlying-public-ip-address-changes"></a>Takma ad kayıtları, temel genel IP adresi değiştiğinde DNS kayıt setimin doğru IP adresine güncelleştirildiğinden emin olmaya yardımcı olur mu?

Evet. Bu özellik, diğer ad kayıtlarının temel özelliklerinden biridir. Uygulamanız için olası kesintileri veya güvenlik risklerini önlemenize yardımcı olur.

### <a name="are-there-any-restrictions-when-using-alias-record-sets-for-a-or-aaaa-records-to-point-to-traffic-manager"></a>Trafik Yöneticisi'ni işaret etmek için A veya AAAA kayıtları için takma ad kayıt kümelerini kullanırken herhangi bir kısıtlama var mı?

Evet. Trafik Yöneticisi profilini A veya AAAA kayıt kümesinden takma ad olarak işaret etmek için, Trafik Yöneticisi profilinin yalnızca harici uç noktaları kullanması gerekir. Trafik Yöneticisi'nde harici uç noktaları oluşturduğunuzda, uç noktaların gerçek IP adreslerini sağlayın.

### <a name="is-there-an-additional-charge-to-use-alias-records"></a>Takma ad kayıtlarını kullanmak için ek bir ücret var mı?

Diğer ad kayıtları, geçerli bir DNS kayıt kümesindeki bir niteliktir. Takma ad kayıtları için ek fatura yok.

## <a name="use-azure-dns"></a>Azure DNS'yi kullanma

### <a name="can-i-co-host-a-domain-by-using-azure-dns-and-another-dns-provider"></a>Azure DNS ve başka bir DNS sağlayıcısı nı kullanarak bir etki alanını barındırabilir miyim?

Evet. Azure DNS, etki alanlarını diğer DNS hizmetleriyle birlikte barındırmayı destekler.

Ortak barındırma yı ayarlamak için, etki alanı için NS kayıtlarını her iki sağlayıcının ad sunucularını işaret edecek şekilde değiştirin. Ad sunucusu (NS), etki alanı için hangi sağlayıcıların DNS sorguları aldığını denetler. Bu NS kayıtlarını Azure DNS'de, diğer sağlayıcıda ve üst bölgede değiştirebilirsiniz. Üst bölge genellikle etki alanı adı kayıt şirketi aracılığıyla yapılandırılır. DNS delegasyonu hakkında daha fazla bilgi için Bkz. [DNS etki alanı delegasyonu.](dns-domain-delegation.md)

Ayrıca, etki alanı için DNS kayıtları nın her iki DNS sağlayıcısı arasında eşit olduğundan emin olun. Azure DNS şu anda DNS bölge aktarımlarını desteklemez. DNS kayıtları [Azure DNS yönetim portalı,](dns-operations-recordsets-portal.md) [REST API,](https://docs.microsoft.com/rest/api/dns/) [SDK,](dns-sdk.md) [PowerShell cmdlets](dns-operations-recordsets.md)veya [CLI aracı](dns-operations-recordsets-cli.md)kullanılarak senkronize edilmelidir.

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>Etki alanımı dört Azure DNS ad sunucusuna da temsilci olarak vermem gerekiyor mu?

Evet. Azure DNS her DNS bölgesine dört ad sunucusu atar. Bu düzenleme fay yalıtımı ve artan esneklik içindir. Azure DNS SLA'ya hak kazanmak için etki alanınızı dört ad sunucusuna da devredin.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Azure DNS'nin kullanım sınırları nelerdir?

Azure DNS kullandığınızda aşağıdaki varsayılan sınırlar geçerlidir.

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Bir Azure DNS bölgesini kaynak grupları arasında veya abonelikler arasında taşıyabilir miyim?

Evet. DNS bölgeleri kaynak grupları arasında veya abonelikler arasında taşınabilir.

Bir DNS bölgesini taşıdığınızda DNS sorguları üzerinde hiçbir etkisi yoktur. Bölgeye atanan ad sunucuları aynı kalır. DNS sorguları boyunca normal olarak işlenir.

DNS bölgelerinin nasıl taşınacağına ilişkin daha fazla bilgi ve yönergeler için [bkz.](../azure-resource-manager/management/move-resource-group-and-subscription.md)

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>DNS değişikliklerinin etkili olması ne kadar sürer?

Yeni DNS bölgeleri ve DNS kayıtları genellikle Azure DNS ad sunucularında hızlı bir şekilde görünür. Zamanlama birkaç saniye.

Varolan DNS kayıtlarındaki değişiklikler biraz daha uzun sürebilir. Bunlar genellikle 60 saniye içinde Azure DNS ad sunucularında görünür. DNS istemcileri ve Azure DNS dışındaki DNS özyinelemeli çözümleyiciler tarafından önbelleğe alma da zamanlamaetkileyebilir. Bu önbellek süresini denetlemek için, her kayıt kümesinin Canlı Yayın (TTL) özelliğini kullanın.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>DNS bölgelerimi yanlışlıkla silmeye karşı nasıl koruyabilirim?

Azure DNS, Azure Kaynak Yöneticisi kullanılarak yönetilir. Azure DNS, Azure Kaynak Yöneticisi'nin sağladığı erişim denetimi özelliklerinden yararlanır. Rol tabanlı erişim denetimi, kullanıcıların DNS bölgelerine ve kayıt kümelerine erişim lerini okuduğu veya yazdığı denetimleri denetler. Kaynak kilitleri, DNS bölgelerinin ve kayıt kümelerinin yanlışlıkla değiştirilmesini veya silinmesini önler.

Daha fazla bilgi için Bkz. [DNS bölgelerini ve kayıtlarını koru.](dns-protect-zones-recordsets.md)

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Azure DNS'de SPF kayıtlarını nasıl ayarlıyorum?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-name-servers-resolve-over-ipv6"></a>Azure DNS ad sunucuları IPv6 üzerinden çözülür mü? 

Evet. Azure DNS ad sunucuları çift yığındır. Çift yığın, IPv4 ve IPv6 adreslerine sahip oldukları anlamına gelir. DNS bölgenize atanan Azure DNS ad sunucularının IPv6 adresini bulmak için nslookup gibi bir araç kullanın. `nslookup -q=aaaa <Azure DNS Nameserver>` bunun bir örneğidir.

### <a name="how-do-i-set-up-an-idn-in-azure-dns"></a>Azure DNS'de nasıl bir IDN ayarlıyorum?

Uluslararası etki alanı adları (IDN'ler) her DNS adını [punycode](https://en.wikipedia.org/wiki/Punycode)kullanarak kodlar. DNS sorguları bu cılız kodlanmış adlar kullanılarak yapılır.

Azure DNS'deki IDN'leri yapılandırmak için bölge adını veya kayıt kümesi adını punycode'a dönüştürün. Azure DNS şu anda yerleşik olarak punycode'a veya punycode'dan dönüşüm yapılmasını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DNS hakkında daha fazla bilgi edinin.](dns-overview.md)

- [Azure DNS'yi özel etki alanları için nasıl kullanacağınız hakkında daha fazla bilgi edinin.](private-dns-overview.md)

- [DNS bölgeleri ve kayıtları hakkında daha fazla bilgi edinin.](dns-zones-records.md)

- [Azure DNS ile başlayın.](dns-getstarted-portal.md)
