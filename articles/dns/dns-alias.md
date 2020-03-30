---
title: Takma ad kayıtlarına genel bakış - Azure DNS
description: Bu makalede, Microsoft Azure DNS'deki diğer ad kayıtları desteği hakkında bilgi edinin.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 08/09/2019
ms.author: rohink
ms.openlocfilehash: 271770935cf4cb83d4abc6e82a4f4b13ffe865b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295497"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS diğer ad kayıtlarına genel bakış

Azure DNS diğer ad kayıtları, DNS kayıt kümesindeki niteliklerdir. DNS bölgenizdeki diğer Azure kaynaklarına başvururlar. Örneğin, A kaydı yerine Azure genel IP adresine başvuran bir takma ad kaydı kümesi oluşturabilirsiniz. Takma adınız, bir Azure genel IP adresi hizmeti örneğine dinamik olarak puan lar ayarlayın. Sonuç olarak, diğer ad kaydı dns çözümü sırasında kendisini sorunsuz bir şekilde güncelleştirir.

Bir Azure DNS bölgesinde aşağıdaki kayıt türleri için bir diğer ad kayıt kümesi desteklenir: 

- A
- AAAA
- CNAME

> [!NOTE]
> [Azure Trafik Yöneticisi profilini](../traffic-manager/quickstart-create-traffic-manager-profile.md) işaret etmek için A veya AAAA kayıt türleri için takma ad kaydı kullanmayı planlıyorsanız, Trafik Yöneticisi profilinin yalnızca dış uç [noktaları](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints)olduğundan emin olmalısınız. Trafik Yöneticisi'nde harici uç noktalar için IPv4 veya IPv6 adresini sağlamanız gerekir. Uç noktalarda tam nitelikli alan adları (FQDNs) kullanamazsınız. İdeal olarak, statik IP adreslerini kullanın.

## <a name="capabilities"></a>Özellikler

- **DNS A/AAAA kayıt kümesinden ortak bir IP kaynağına işaret edin.** Bir A/AAAA kayıt kümesi oluşturabilir ve bunu ortak bir IP kaynağını (standart veya temel) işaret etmek üzere ayarlanmış bir takma ad kaydı yapabilirsiniz. Genel IP adresi değişirse veya silinirse DNS kaydı otomatik olarak değişir. Yanlış IP adreslerine işaret eden sarkan DNS kayıtları önlenir.

   Kaynak başına 20 diğer ad kaydı kümesi nin geçerli bir sınırı vardır.

- **DNS A/AAAA/CNAME kayıt kümesinden Trafik Yöneticisi profilini işaret edin.** Bir A/AAAA veya CNAME kayıt kümesi oluşturabilir ve trafik yöneticisi profiline işaret etmek için takma ad kayıtlarını kullanabilirsiniz. Geleneksel CNAME kayıtları bir bölge tepe için desteklenmez gibi, bir bölge apeks trafik yönlendirmeniz gerektiğinde özellikle yararlıdır. Örneğin, Trafik Yöneticisi profilinizin myprofile.trafficmanager.net ve işletmenizin DNS bölgesinin contoso.com olduğunu söyleyin. contoso.com (bölge tepe noktası) için A/AAAA türünden bir takma ad kaydı kümesi oluşturabilir ve myprofile.trafficmanager.net işaret edebilirsiniz.
- **Azure İçerik Dağıtım Ağı (CDN) bitiş noktasını işaret edin.** Bu, Azure depolama ve Azure CDN kullanarak statik web siteleri oluşturduğunuzda yararlıdır.
- **Aynı bölge içinde ayarlanan başka bir DNS kaydını işaret edin.** Diğer ad kayıtları aynı türdeki diğer kayıt kümelerine başvurabilir. Örneğin, bir DNS CNAME kayıt kümesi başka bir CNAME kayıt kümesinin diğer adı olabilir. Bu düzenleme, bazı kayıt kümelerinin diğer adlar ve bazı takma ad olmayan lar olmasını istiyorsanız yararlıdır.

## <a name="scenarios"></a>Senaryolar

Alias kayıtları için birkaç yaygın senaryo vardır.

### <a name="prevent-dangling-dns-records"></a>Sarkan DNS kayıtlarını önleme

Geleneksel DNS kayıtları ile ortak bir sorun sarkan kayıtlarıdır. Örneğin, IP adreslerindeki değişiklikleri yansıtacak şekilde güncelleştirilen DNS kayıtları. Sorun özellikle A/AAAA veya CNAME kayıt türlerinde oluşur.

Geleneksel bir DNS bölge kaydıyla, hedef IP veya CNAME artık yoksa, onunla ilişkili DNS kaydının el ile güncelleştirilmelidir. Bazı kuruluşlarda, işlem sorunları veya rollerin ayrılması ve ilişkili izin düzeyleri nedeniyle el ile güncelleştirme zamanında gerçekleşmeyebilir. Örneğin, bir rolün bir uygulamaya ait bir CNAME veya IP adresini silme yetkisi olabilir. Ancak, bu hedeflere işaret eden DNS kaydını güncellemek için yeterli yetkiye sahip değildir. DNS kaydının güncelleştirilmesinde gecikme, kullanıcılar için bir kesintiye neden olabilir.

Diğer ad kayıtları, bir DNS kaydının yaşam döngüsünü Azure kaynağıyla sıkıca bağlayarak sarkan başvuruları önler. Örneğin, genel bir IP adresini veya Trafik Yöneticisi profilini işaret etmek için takma ad kaydı olarak nitelikli bir DNS kaydı düşünün. Bu temel kaynakları silerseniz, DNS diğer adı kaydı boş bir kayıt kümesi olur. Artık silinen kaynağa başvurulmez.

### <a name="update-dns-record-set-automatically-when-application-ip-addresses-change"></a>Uygulama IP adresleri değiştiğinde DNS kayıt kümesini otomatik olarak güncelleştirme

Bu senaryo öncekine benzer. Belki bir uygulama taşınır veya temeldeki sanal makine yeniden başlatılır. Bir takma ad kaydı, altta yatan genel IP kaynağı için IP adresi değiştiğinde otomatik olarak güncellenir. Bu, kullanıcıları eski genel IP adresine atanan başka bir uygulamaya yönlendirmenin olası güvenlik risklerini önler.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Bölge tepe noktasında yük dengeli uygulamaları barındır

DNS protokolü, bölge tepe noktasında CNAME kayıtlarının atanmasını engeller. Örneğin etki alanınız contoso.com; somelabel.contoso.com için CNAME kayıtları oluşturabilirsiniz; ancak kendisi için CNAME oluşturamaz contoso.com.
Bu kısıtlama, [Azure Trafik Yöneticisi'nin](../traffic-manager/traffic-manager-overview.md)arkasında yük dengeli uygulamaları olan uygulama sahipleri için bir sorun teşkil ediyor. Trafik Yöneticisi profilini kullanmak bir CNAME kaydı oluşturulmasını gerektirdiğinden, bölge tepesinden Trafik Yöneticisi profilini işaret etmek mümkün değildir.

Bu sorun diğer ad kayıtları kullanılarak çözülür. CNAME kayıtlarının aksine, bölge uçlarında takma ad kayıtları oluşturulur ve uygulama sahipleri bölge uçlarını dış uç noktaları olan bir Trafik Yöneticisi profiline işaret etmek için bunu kullanabilir. Uygulama sahipleri, DNS bölgesindeki herhangi bir başka etki alanı için kullanılan aynı Trafik Yöneticisi profiline işaret eder.

Örneğin, contoso.com ve\.www contoso.com aynı Trafik Yöneticisi profilini gösterebilir. Azure Trafik Yöneticisi profilleriyle takma ad kayıtlarını kullanma hakkında daha fazla bilgi edinmek için Sonraki adımlar bölümüne bakın.

### <a name="point-zone-apex-to-azure-cdn-endpoints"></a>Azure CDN uç noktalarına nokta bölgesi apeksi

Trafik Yöneticisi profilinde olduğu gibi, DNS bölge uçnoktanızı Azure CDN uç noktalarına çekmek için takma ad kayıtlarını da kullanabilirsiniz. Bu, Azure depolama ve Azure CDN kullanarak statik web siteleri oluşturduğunuzda yararlıdır. Daha sonra DNS adınıza "www" vermeden web sitesine erişebilirsiniz.

Örneğin, statik web siteniz `www.contoso.com, your users can access your site using contoso.com` dns adına www prepend gerek kalmadan adlandırılır.

Daha önce açıklandığı gibi, CNAME kayıtları bölge tepe noktasında desteklenmez. Bu nedenle, cdn bitiş noktanıza contoso.com işaret etmek için cname kaydı kullanamazsınız. Bunun yerine, bölge tepe noktasını doğrudan CDN bitiş noktasına çekmek için bir takma ad kaydı kullanabilirsiniz.

> [!NOTE]
> Akamai'den Azure CDN için bir bölge uç noktasını CDN uç noktalarına işaretlemek şu anda desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Diğer ad kayıtları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Öğretici: Azure genel IP adresine başvurmak için diğer ad kaydı yapılandırma](tutorial-alias-pip.md)
- [Öğretici: Traffic Manager ile tepe etki alanı adlarını desteklemek için diğer ad kaydı yapılandırma](tutorial-alias-tm.md)
- [DNS SSS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
