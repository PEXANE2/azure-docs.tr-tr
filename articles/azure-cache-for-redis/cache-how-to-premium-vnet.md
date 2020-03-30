---
title: Sanal Ağı Yapılandırma - Redis için Premium Azure Önbelleği
description: Redis örnekleri için Premium katman Azure Önbelleği için Sanal Ağ desteğini nasıl oluşturup yöneteceğinizi öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 05/15/2017
ms.openlocfilehash: 6c7c041565f6376e7f8b8b84f5076b30c1eec7bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278122"
---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-cache-for-redis"></a>Redis için Premium Azure Önbelleği için Sanal Ağ Desteği nasıl yapılandırılır?
Redis için Azure Önbellek, kümeleme, kalıcılık ve sanal ağ desteği gibi Premium katman özellikleri de dahil olmak üzere önbellek boyutu ve özellikleri seçiminde esneklik sağlayan farklı önbellek tekliflerine sahiptir. VNet, buluttaki özel bir ağdır. Redis örneği için bir Azure Önbelleği bir VNet ile yapılandırıldığında, genel olarak ele alınamaz ve yalnızca VNet içindeki sanal makinelerden ve uygulamalardan erişilebilir. Bu makalede, Redis örneği için birinci sınıf bir Azure Önbelleği için sanal ağ desteğinin nasıl yapılandırılabildiği açıklanmaktadır.

> [!NOTE]
> Redis için Azure Önbelleği hem klasik hem de Kaynak Yöneticisi VNet'leri destekler.
> 
> 

Diğer premium önbellek özellikleri hakkında daha fazla bilgi için [redis premium katmanı için Azure Önbelleğine Giriş'e](cache-premium-tier-intro.md)bakın.

## <a name="why-vnet"></a>Neden VNet?
[Azure Sanal Ağ (VNet)](https://azure.microsoft.com/services/virtual-network/) dağıtımı, erişimi daha da kısıtlamak için Azure Önbelleğiniz redis için gelişmiş güvenlik ve yalıtımın yanı sıra alt ağlar, erişim denetim ilkeleri ve diğer özellikler sağlar.

## <a name="virtual-network-support"></a>Sanal ağ desteği
Sanal Ağ (VNet) desteği, önbellek oluşturma sırasında Redis için **Yeni Azure Önbelleğinde** yapılandırılır. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Bir premium fiyatlandırma katmanı seçtikten sonra, önbelleğinizle aynı abonelikte ve konumda bulunan bir VNet seçerek Redis VNet entegrasyonunu yapılandırabilirsiniz. Yeni bir VNet kullanmak için, önce [Azure portalını kullanarak sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network) veya Azure [portalını kullanarak sanal ağ (klasik) oluşturma](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) adımlarını izleyerek oluşturun ve ardından premium önbelleğinizi oluşturmak ve yapılandırmak için Redis blade için Yeni Azure **Önbelleğine** geri dönün.

Yeni önbelleğiniz için VNet'i yapılandırmak **için Redis için Yeni Azure Önbelleğinde** Sanal **Ağ'ı** tıklatın ve açılan listeden istediğiniz VNet'i seçin.

![Sanal ağ][redis-cache-vnet]

**Subnet** açılır listesinden istediğiniz alt ağı seçin.  İstenirse, **Statik IP adresi**belirtin. **Statik IP adresi** alanı isteğe bağlıdır ve hiçbiri belirtilmemişse, seçili alt ağdan bir tane seçilir.

> [!IMPORTANT]
> Redis için bir Azure Önbelleği'ni Bir Kaynak Yöneticisi VNet'e dağıtırken, önbellek Redis örnekleri için Azure Önbelleği dışında başka kaynak içermeyen özel bir alt ağda olmalıdır. Redis için bir Azure Önbelleğini kaynak yöneticisi VNet'e diğer kaynakları içeren bir alt ağa dağıtma girişiminde bulunulması durumunda, dağıtım başarısız olur.
> 
> 

![Sanal ağ][redis-cache-vnet-ip]

> [!IMPORTANT]
> Azure her alt ağda bazı IP adresleri ayırır ve bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumluluğu için ayrılmış ve Azure hizmetleri için kullanılan üç adres daha vardır. Daha fazla bilgi için, bu [alt ağlar içinde IP adresleri nin kullanılmasıyla ilgili herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Azure VNET altyapısı tarafından kullanılan IP adreslerine ek olarak, alt netteki her Redis örneği parça başına iki IP adresi ve yük dengeleyicisi için bir ek IP adresi kullanır. Kümeslenmiş olmayan bir önbelleğe sahip olduğu düşünülür.
> 
> 

Önbellek oluşturulduktan sonra, **Kaynak menüsünden** **Sanal Ağ'ı** tıklatarak VNet yapılandırmasını görüntüleyebilirsiniz.

![Sanal ağ][redis-cache-vnet-info]

VNet kullanırken Redis için Azure Önbelleğinize bağlanmak için, bağlantı dizesinde önbelleğinizin ana bilgisayar adını aşağıdaki örnekte gösterildiği gibi belirtin:

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-cache-for-redis-vnet-faq"></a>Redis VNet SSS için Azure Önbelleği
Aşağıdaki liste, Redis ölçekleme için Azure Önbelleği hakkında sık sorulan soruların yanıtlarını içerir.

* Redis ve VNets için Azure Önbelleği ile ilgili yaygın bazı yanlış yapılandırma sorunları nelerdir?
* [Önbelleğimin sanal ağ üzerinde çalıştığını nasıl doğrulayabilirim?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* Bir VNET'te Redis için Azure Önbelleğime bağlanmaya çalışırken, uzak sertifikanın geçersiz olduğunu belirten bir hata alıyorum?
* [Standart veya temel önbellekle VNets kullanabilir miyim?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* Redis için Azure Önbelleği oluşturmak neden bazı alt ağlarda başarısız olur da diğerlerinde başarısız olmaz?
* [Alt ağ adresi alanı gereksinimleri nelerdir?](#what-are-the-subnet-address-space-requirements)
* [Tüm önbellek özellikleri VNET'te önbellek barındırırken çalışır mı?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

### <a name="what-are-some-common-misconfiguration-issues-with-azure-cache-for-redis-and-vnets"></a>Redis ve VNets için Azure Önbelleği ile ilgili yaygın bazı yanlış yapılandırma sorunları nelerdir?
Redis için Azure Önbelleği bir VNet'te barındırıldığında, aşağıdaki tablolardaki bağlantı noktaları kullanılır. 

>[!IMPORTANT]
>Aşağıdaki tablolardaki bağlantı noktaları engellenirse, önbellek düzgün çalışmayabilir. Bu bağlantı noktalarından birinin veya daha fazlasının engellenmesi, Bir VNet'te Redis için Azure Önbelleği kullanırken en yaygın yanlış yapılandırma sorunudur.
> 
> 

- [Giden bağlantı noktası gereksinimleri](#outbound-port-requirements)
- [Gelen bağlantı noktası gereksinimleri](#inbound-port-requirements)

#### <a name="outbound-port-requirements"></a>Giden bağlantı noktası gereksinimleri

Dokuz giden bağlantı noktası gereksinimleri vardır. Bu aralıklarda giden istekler, önbelleğin çalışması için gerekli olan diğer hizmetlere giden veya internode iletişim için Redis alt ağına dahil edilir. Coğrafi çoğaltma için, birincil ve ikincil önbelleğin alt ağları arasındaki iletişim için ek giden gereksinimler vardır.

| Bağlantı noktaları | Yön | Ulaşım Protokolü | Amaç | Yerel IP | Uzak IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Giden |TCP |Azure Depolama/PKI (Internet) üzerindeki Redis bağımlılıkları | (Redis alt ağı) |* |
| 443 | Giden | TCP | Azure Anahtar Kasası'na redis bağımlılığı | (Redis alt ağı) | AzureKeyVault <sup>1</sup> |
| 53 |Giden |TCP/UDP |Redis'in DNS (Internet/VNet) bağımlılığı | (Redis alt ağı) | 168.63.129.16 ve 169.254.169.254 <sup>2</sup> ve alt net <sup>3</sup> için herhangi bir özel DNS sunucusu |
| 8443 |Giden |TCP |Redis için iç iletişim | (Redis alt ağı) | (Redis alt ağı) |
| 10221-10231 |Giden |TCP |Redis için iç iletişim | (Redis alt ağı) | (Redis alt ağı) |
| 20226 |Giden |TCP |Redis için iç iletişim | (Redis alt ağı) |(Redis alt ağı) |
| 13000-13999 |Giden |TCP |Redis için iç iletişim | (Redis alt ağı) |(Redis alt ağı) |
| 15000-15999 |Giden |TCP |Redis ve Geo-Replication için dahili iletişim | (Redis alt ağı) |(Redis alt ağı) (Coğrafi yineleme eş alt ağı) |
| 6379-6380 |Giden |TCP |Redis için iç iletişim | (Redis alt ağı) |(Redis alt ağı) |

<sup>1</sup> Kaynak Yöneticisi Ağ Güvenlik Grupları ile 'AzureKeyVault' hizmet etiketini kullanabilirsiniz.

<sup>2</sup> Microsoft'un sahip olduğu bu IP adresleri, Azure DNS'ye hizmet veren Ana Bilgisayar VM adresine yönelik olarak kullanılır.

<sup>3</sup> Özel DNS sunucusu olmayan alt ağlar veya özel DNS'yi yok sayan yeni redis önbellekleri için gerekli değildir.

#### <a name="geo-replication-peer-port-requirements"></a>Coğrafi çoğaltma eş bağlantı noktası gereksinimleri

Azure Sanal Ağlar'daki önbellekler arasında coğrafi çoğaltma kullanıyorsanız, önerilen yapılandırmanın hem gelen hem de giden yol tariflerindeki tüm alt ağ için 15000-15999 bağlantı noktalarının engelini kaldırmak olduğunu, böylece tüm çoğaltma bileşenlerinin alt ağda, gelecekteki bir coğrafi hata durumunda bile birbirleriyle doğrudan iletişim kurabilir.

#### <a name="inbound-port-requirements"></a>Gelen bağlantı noktası gereksinimleri

Sekiz gelen bağlantı noktası aralığı gereksinimleri vardır. Bu aralıklarda gelen istekler, aynı VNET'te barındırılan diğer hizmetlerden veya Redis alt ağı iletişimine dahili olarak gelen isteklerdir.

| Bağlantı noktaları | Yön | Ulaşım Protokolü | Amaç | Yerel IP | Uzak IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Gelen |TCP |Redis'e istemci iletişimi, Azure yük dengeleme | (Redis alt ağı) | (Redis subnet), Sanal Ağ, Azure Yük Dengeleyici <sup>1</sup> |
| 8443 |Gelen |TCP |Redis için iç iletişim | (Redis alt ağı) |(Redis alt ağı) |
| 8500 |Gelen |TCP/UDP |Azure yük dengeleme | (Redis alt ağı) |Azure Load Balancer |
| 10221-10231 |Gelen |TCP |Redis için iç iletişim | (Redis alt ağı) |(Redis subnet), Azure Yük Dengeleyicisi |
| 13000-13999 |Gelen |TCP |Redis Kümeleri için istemci iletişimi, Azure yük dengeleme | (Redis alt ağı) |Sanal Ağ, Azure Yük Dengeleyicisi |
| 15000-15999 |Gelen |TCP |Redis Kümeleri, Azure yük Dengeleme ve Coğrafi Çoğaltma için istemci iletişimi | (Redis alt ağı) |Sanal Ağ, Azure Yük Dengeleyicisi, (Coğrafi çoğaltma eş alt ağı) |
| 16001 |Gelen |TCP/UDP |Azure yük dengeleme | (Redis alt ağı) |Azure Load Balancer |
| 20226 |Gelen |TCP |Redis için iç iletişim | (Redis alt ağı) |(Redis alt ağı) |

<sup>1</sup> NSG kurallarını yazarken 'AzureLoadBalancer' (Kaynak Yöneticisi) (veya klasik için 'AZURE_LOADBALANCER' Hizmet Etiketini kullanabilirsiniz.

#### <a name="additional-vnet-network-connectivity-requirements"></a>Ek VNET ağ bağlantısı gereksinimleri

Redis için Azure Önbelleği için başlangıçta sanal ağda karşılanmayan ağ bağlantısı gereksinimleri vardır. Redis için Azure Önbelleği, sanal ağ içinde kullanıldığında aşağıdaki öğelerin tüminin düzgün çalışmasını gerektirir.

* Dünya çapında Azure Depolama uç noktalarına giden ağ bağlantısı. Buna, Redis için Azure Önbelleği ile aynı bölgede bulunan uç noktaların yanı sıra **diğer** Azure bölgelerinde bulunan depolama bitiş noktaları da dahildir. Azure Depolama uç noktaları aşağıdaki DNS etki alanları altında çözüme kavuşturulmaktadır: *table.core.windows.net,* *blob.core.windows.net,* *queue.core.windows.net*ve *file.core.windows.net.* 
* *ocsp.msocsp.com,* *mscrl.microsoft.com*ve *crl.microsoft.com*giden ağ bağlantısı. Bu bağlantı, SSL işlevselliğini desteklemek için gereklidir.
* Sanal ağ için DNS yapılandırması, önceki noktalarda belirtilen tüm uç noktaları ve etki alanlarını çözebilme yeteneğine sahip olmalıdır. Bu DNS gereksinimleri, sanal ağ için geçerli bir DNS altyapısının yapılandırılmasını ve bakımının sağlanmasıyla karşılanabilir.
* Aşağıdaki DNS etki alanları altında çözümleyen aşağıdaki Azure İzleme uç noktalarına giden ağ bağlantısı: shoebox2-black.shoebox2.metrics.nsatc.net, north-prod2.prod2.metrics.nsatc.net azglobal-black.azglobal.metrics.nsatc.net , shoebox2-red.shoebox2.metrics.nsatc.net, east-prod2.prod2.metrics.nsatc.net, azglobal-red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Önbelleğimin sanal ağ üzerinde çalıştığını nasıl doğrulayabilirim?

>[!IMPORTANT]
>Bir VNET'te barındırılan Redis için Bir Azure Önbelleği'ne bağlanırken, önbellek istemcileriniz aynı VNET'te veya aynı Azure bölgesinde VNET eşlemeli bir VNET'te olmalıdır. Global VNET Peering şu anda desteklenmez. Bu herhangi bir test uygulamaları veya tanılama ping araçları içerir. İstemci uygulaması nın nerede barındırıldığına bakılmaksızın, Ağ güvenlik grupları, istemcinin ağ trafiğinin Redis örneğine ulaşmasına izin verilecek şekilde yapılandırılmalıdır.
>
>

Bağlantı noktası gereksinimleri önceki bölümde açıklandığı şekilde yapılandırıldıktan sonra, önbelleğinizin aşağıdaki adımları gerçekleştirerek çalıştığını doğrulayabilirsiniz.

- [Reboot](cache-administration.md#reboot) Önbellek düğümlerinin tümünün yeniden başlatılAcağını. Gerekli önbellek bağımlılıklarının tümüne ulaşılamazsa [(Gelen bağlantı noktası gereksinimlerinde](cache-how-to-premium-vnet.md#inbound-port-requirements) ve [Giden bağlantı noktası gereksinimlerinde](cache-how-to-premium-vnet.md#outbound-port-requirements)belirtildiği gibi), önbellek başarıyla yeniden başlatılamaz.
- Önbellek düğümleri yeniden başlatıldıktan sonra (Azure portalındaki önbellek durumu tarafından bildirilen) aşağıdaki testleri gerçekleştirebilirsiniz:
  - önbellek uç noktasını (bağlantı noktası 6380 kullanarak) önbellekle aynı VNET içinde olan bir makineden [tcping](https://www.elifulkerson.com/projects/tcping.php)kullanarak ping. Örnek:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    `tcping` Araç bağlantı noktasının açık olduğunu bildiriyorsa, önbellek VNET'teki istemcilerden bağlantı için kullanılabilir.

  - Sınamak için başka bir yolu önbelleğe bağlanır ve ekler ve önbellekten bazı öğeleri alır (StackExchange.Redis kullanarak basit bir konsol uygulaması olabilir) bir test önbellek istemcisi oluşturmaktır. Örnek istemci uygulamasını önbellekle aynı VNET'te bulunan bir VM'ye yükleyin ve önbelleğe bağlantı doğrulamak için çalıştırın.


### <a name="when-trying-to-connect-to-my-azure-cache-for-redis-in-a-vnet-why-am-i-getting-an-error-stating-the-remote-certificate-is-invalid"></a>Bir VNET'te Redis için Azure Önbelleğime bağlanmaya çalışırken, uzak sertifikanın geçersiz olduğunu belirten bir hata alıyorum?

Bir VNET'te Redis için Bir Azure Önbelleğine bağlanmaya çalışırken, şu gibi bir sertifika doğrulama hatası görürsünüz:

`{"No connection is available to service this operation: SET mykey; The remote certificate is invalid according to the validation procedure.; …"}`

Bunun nedeni, IP adresine göre ana bilgisayara bağlanmanız olabilir. Ana bilgisayar adını kullanmanızı öneririz. Başka bir deyişle, aşağıdakileri kullanın:     

`[mycachename].redis.windows.net:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

Aşağıdaki bağlantı dizesine benzer IP adresini kullanmaktan kaçının:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

DNS adını çözemiyorsanız, bazı istemci kitaplıkları `sslHost` StackExchange.Redis istemcisi tarafından sağlanan yapılandırma seçeneklerini içerir. Bu, sertifika doğrulama için kullanılan ana bilgisayar adını geçersiz kılmanızı sağlar. Örnek:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False;sslHost=[mycachename].redis.windows.net`

### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Standart veya temel önbellekle VNets kullanabilir miyim?
VNets sadece premium önbelleklerle kullanılabilir.

### <a name="why-does-creating-an-azure-cache-for-redis-fail-in-some-subnets-but-not-others"></a>Redis için Azure Önbelleği oluşturmak neden bazı alt ağlarda başarısız olur da diğerlerinde başarısız olmaz?
Redis için bir Azure Önbelleği'ni Kaynak Yöneticisi VNet'e dağıtıyorsanız, önbelleğin başka kaynak türü içermeyen özel bir alt ağda olması gerekir. Redis için bir Azure Önbelleği'ni diğer kaynakları içeren kaynak yöneticisi VNet alt ağına dağıtma girişiminde bulunulması durumunda, dağıtım başarısız olur. Redis için yeni bir Azure Önbelleği oluşturmadan önce alt ağdaki varolan kaynakları silmeniz gerekir.

Yeterli IP adresiniz olduğu sürece, klasik bir VNet'e birden çok kaynak türü dağıtabilirsiniz.

### <a name="what-are-the-subnet-address-space-requirements"></a>Alt ağ adresi alanı gereksinimleri nelerdir?
Azure her alt ağda bazı IP adresleri ayırır ve bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumluluğu için ayrılmış ve Azure hizmetleri için kullanılan üç adres daha vardır. Daha fazla bilgi için, bu [alt ağlar içinde IP adresleri nin kullanılmasıyla ilgili herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure VNET altyapısı tarafından kullanılan IP adreslerine ek olarak, alt netteki her Redis örneği parça başına iki IP adresi ve yük dengeleyicisi için bir ek IP adresi kullanır. Kümeslenmiş olmayan bir önbelleğe sahip olduğu düşünülür.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Tüm önbellek özellikleri VNET'te önbellek barındırırken çalışır mı?
Önbelleğiniz Bir VNET'in parçası olduğunda, önbelleğe yalnızca VNET'teki istemciler erişebilir. Sonuç olarak, aşağıdaki önbellek yönetimi özellikleri şu anda çalışmıyor.

* Redis Console - Redis Console, VNET'in dışındaki yerel tarayıcınızda çalıştığından önbelleğinize bağlanamaz.


## <a name="use-expressroute-with-azure-cache-for-redis"></a>Redis için Azure Önbelleği ile ExpressRoute'u kullanma

Müşteriler bir [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) devresini sanal ağ altyapılarına bağlayarak şirket içi ağlarını Azure'a genişletebilir. 

Varsayılan olarak, yeni oluşturulan ExpressRoute devresi Bir VNET'te zorunlu tünel oluşturma (varsayılan rotanın reklamı, 0.0.0.0/0) gerçekleştirmez. Sonuç olarak, giden Internet bağlantısına doğrudan VNET'ten izin verilir ve istemci uygulamaları Redis için Azure Önbelleği de dahil olmak üzere diğer Azure uç noktalarına bağlanabilir.

Ancak, ortak bir müşteri yapılandırması, giden Internet trafiğini şirket içinde akmaya zorlayan zorunlu tünelleme (varsayılan rotanın reklamını) kullanmaktır. Bu trafik akışı, giden trafik şirket içinde engellenirse, Redis için Azure Önbelleği örneğinin bağımlılıklarıyla iletişim kuramamasına neden olursa, Redis için Azure Önbelleği ile bağlantıyı bozar.

Çözüm, Redis için Azure Önbelleği'ni içeren alt ağda kullanıcı tanımlı (veya daha fazla) bir (veya daha fazla) rota (ÜD) tanımlamaktır. UDR, varsayılan rota yerine onurlandırılacak alt ağa özgü yolları tanımlar.

Mümkünse, aşağıdaki yapılandırmayı kullanmanız önerilir:

* ExpressRoute yapılandırması 0.0.0.0/0'ın reklamını ve varsayılan olarak tüm giden trafiği şirket içinde sağlar.
* Redis için Azure Önbelleği'ni içeren alt ağa uygulanan UDR, TCP/IP trafiğinin genel internete yönlendirdiği 0.0.0.0/0'ı tanımlar; örneğin bir sonraki atlama türünü 'Internet' olarak ayarlayarak.

Bu adımların birleşik etkisi, alt ağ düzeyi UDR'nin ExpressRoute zorunlu tünellemeden önce geldiği ve böylece Redis için Azure Önbelleğinden giden Internet erişiminin sağlanmasıdır.

ExpressRoute'u kullanarak şirket içi bir uygulamadan Redis için Bir Azure Önbelleğine bağlanmak performans nedenlerinden dolayı tipik bir kullanım senaryosu değildir (Redis istemcileri için en iyi performans Azure Önbelleği Redis için Azure Önbelleği ile aynı bölgede olmalıdır) .

>[!IMPORTANT] 
>UDR'de tanımlanan rotalar, ExpressRoute yapılandırması tarafından reklamı yapılan rotalardan önce gelmek için yeterince özel **olmalıdır.** Aşağıdaki örnek, geniş 0.0.0.0/0 adres aralığını kullanır ve bu nedenle, daha spesifik adres aralıkları kullanılarak rota reklamları tarafından yanlışlıkla geçersiz kılınabilir.

>[!WARNING]  
>Redis için Azure Önbelleği, genel bakış **yolundan özel eşleme yoluna doğru yanlış bir şekilde çapraz reklam veren**ExpressRoute yapılandırmalarıyla desteklenmez. Genel olarak yapılandırılan ExpressRoute yapılandırmaları, büyük bir Microsoft Azure IP adres aralığı kümesi için Microsoft'tan rota reklamları alır. Bu adres aralıkları özel bakış yolunda yanlış bir şekilde çapraz olarak duyurulursa, sonuç olarak Redis örneğinin alt ağındaki Azure Önbelleğinden gelen tüm giden ağ paketleri müşterinin şirket içi ağına yanlış bir şekilde zorla tünel kazılmış olur. Altyapı. Bu ağ akışı Redis için Azure Önbelleğini kırar. Bu sorunun çözümü, özel bakan yolu için kamu bakan yolu çapraz reklam yolları durdurmaktır.


Kullanıcı tanımlı rotalardaki arka plan bilgileri bu [genel bakışta](../virtual-network/virtual-networks-udr-overview.md)mevcuttur.

ExpressRoute hakkında daha fazla bilgi için [ExpressRoute teknik genel bakış](../expressroute/expressroute-introduction.md)bilgisine bakın.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliğini nasıl kullanacağınızı öğrenin.

* [Redis Premium katmanı için Azure Önbelleğine Giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png
