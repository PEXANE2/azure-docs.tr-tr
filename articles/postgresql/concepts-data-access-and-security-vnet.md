---
title: Sanal ağ kuralları - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL - Single Server için Azure Veritabanına bağlanmak için sanal ağ (vnet) hizmet uç noktalarını nasıl kullanacağınızı öğrenin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 512ad8f93da53afb618491cd1769645d8edb0b14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75965838"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı için Sanal Ağ hizmet bitiş noktalarını ve kurallarını kullanma - Tek Sunucu

*Sanal ağ kuralları,* PostgreSQL sunucusu için Azure Veritabanınızın sanal ağlardaki belirli alt ağlardan gönderilen iletişimleri kabul edip etmediğini kontrol eden bir güvenlik duvarı güvenlik özelliğidir. Bu makalede, sanal ağ kuralı özelliğinin, PostgreSQL sunucusu için Azure Veritabanınızla güvenli bir şekilde iletişim edeyimize izin vermek için neden bazen en iyi seçeneğiniz olduğu açıklanmaktadır.

Sanal ağ kuralı oluşturmak için öncelikle bir [sanal ağ][vm-virtual-network-overview] (VNet) ve kuralın başvurulması için sanal ağ hizmeti [bitiş noktası][vm-virtual-network-service-endpoints-overview-649d] olmalıdır. Aşağıdaki resim, Virtual Network hizmeti bitiş noktasının PostgreSQL için Azure Veritabanı ile nasıl çalıştığını göstermektedir:

![VNet Hizmet Bitiş Noktası'nın nasıl çalıştığına örnek](media/concepts-data-access-and-security-vnet/vnet-concept.png)

> [!NOTE]
> Bu özellik, PostgreSQL için Azure Veritabanı'nın Genel Amaç ve Bellek Optimize Edilmiş sunucular için dağıtıldığı Azure genel bulutunun tüm bölgelerinde kullanılabilir.
> VNet'in eşlemesi durumunda, trafik hizmet bitiş noktalarıyla ortak bir VNet Ağ Geçidi'nden akıyorsa ve eşe akması gerekiyorsa, lütfen Ağ Geçidi VNet'teki Azure Sanal Makinelerin PostgreSQL sunucusu için Azure Veritabanı'na erişmesine izin vermek için bir ACL/VNet kuralı oluşturun.

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminoloji ve açıklama

**Sanal ağ:** Azure aboneliğinizle ilişkili sanal ağlarınız olabilir.

**Alt ağ:** Sanal ağ **alt ağlar**içerir. Sahip olduğunuz tüm Azure sanal makineleri (VM'ler) alt ağlara atanır. Bir alt ağ birden çok VM veya diğer işlem düğümleri içerebilir. Sanal ağınızın dışındaki bilgi işlem düğümleri, güvenliğinizi erişime izin verecek şekilde yapılandırmadığınız sürece sanal ağınıza erişemez.

**Sanal Ağ hizmeti bitiş noktası:** [Sanal Ağ hizmeti bitiş noktası,][vm-virtual-network-service-endpoints-overview-649d] özellik değerleri bir veya daha fazla resmi Azure hizmet türü adlarını içeren bir alt ağdır. Bu makalede, SQL Veritabanı adlı Azure hizmetini ifade eden **Microsoft.Sql'in**tür adı ile ilgileniyoruz. Bu hizmet etiketi, PostgreSQL ve MySQL hizmetleri için Azure Veritabanı için de geçerlidir. **Microsoft.Sql** hizmet etiketini vnet hizmet bitiş noktasına uygularken, alt ağdaki MySQL sunucuları için tüm Azure SQL Veritabanı, PostgreSQL için Azure Veritabanı ve Azure Veritabanı için hizmet bitiş noktası trafiğini yapılandıracağını unutmayın. 

**Sanal ağ kuralı:** PostgreSQL sunucusu için Azure Veritabanınız için sanal ağ kuralı, PostgreSQL sunucusu için Azure Veritabanınızın erişim denetim listesinde (ACL) listelenen bir alt ağdır. PostgreSQL sunucusu için Azure Veritabanınızın ACL'sinde olmak için alt ağ **Microsoft.Sql** türü adını içermelidir.

Sanal ağ kuralı, PostgreSQL sunucusu için Azure Veritabanınıza alt ağdaki her düğümden iletişimkabul etmesini söyler.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Sanal ağ kuralının avantajları

Siz harekete geçene kadar, alt ağlarınızdaki VM'ler PostgreSQL sunucusu için Azure Veritabanınızla iletişim kuramaz. İletişimi kuran eylemlerden biri, sanal ağ kuralının oluşturulmasıdır. VNet kuralı yaklaşımını seçmenin mantığı, güvenlik duvarı tarafından sunulan rakip güvenlik seçeneklerini içeren bir karşılaştırma ve karşılaştırma tartışması gerektirir.

### <a name="a-allow-access-to-azure-services"></a>A. Azure hizmetlerine erişime izin ver

Bağlantı güvenlik bölmesi, **Azure hizmetlerine erişime izin ver**olarak etiketlenmiş bir **AGİ/KAPAMA** düğmesine sahiptir. **ON** ayarı, tüm Azure IP adreslerinden ve tüm Azure alt ağlarından iletişim sağlar. Bu Azure IP'leri veya alt ağları size ait olmayabilir. Bu **ON** ayarı büyük olasılıkla PostgreSQL Veritabanı için Azure Veritabanınızın olmasını istediğinizden daha açıktır. Sanal ağ kuralı özelliği çok daha ince parçalı denetim sunar.

### <a name="b-ip-rules"></a>B. IP kuralları

PostgreSQL güvenlik duvarı için Azure Veritabanı, postgreSQL Veritabanı için Azure Veritabanı'nda iletişimlerin kabul edildiği IP adres aralıklarını belirtmenize olanak tanır. Bu yaklaşım, Azure özel ağının dışındaki kararlı IP adresleri için idealdir. Ancak Azure özel ağındaki birçok düğüm *dinamik* IP adresleriyle yapılandırılır. VM'nizin yeniden başlatılması gibi dinamik IP adresleri değişebilir. Bir üretim ortamında, bir güvenlik duvarı kuralı dinamik bir IP adresi belirtmek aptallık olacaktır.

VM'iniz için *statik* bir IP adresi alarak IP seçeneğini kurtarabilirsiniz. Ayrıntılar için, [Azure portalını kullanarak sanal bir makineiçin özel IP adreslerini yapılandırın' a][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]bakın.

Ancak, statik IP yaklaşımının yönetilmesi zor olabilir ve ölçekte yapıldığında maliyetlidir. Sanal ağ kurallarının oluşturulması ve yönetilmesi daha kolaydır.

### <a name="c-cannot-yet-have-azure-database-for-postgresql-on-a-subnet-without-defining-a-service-endpoint"></a>C. Bir hizmet bitiş noktası tanımlamadan bir alt ağda PostgreSQL için Azure Veritabanı henüz olamaz

**Microsoft.Sql** sunucunuz sanal ağınızdaki bir alt ağda düğümse, sanal ağdaki tüm düğümler PostgreSQL sunucusu için Azure Veritabanınızla iletişim kurabilir. Bu durumda, Sanal M'leriniz herhangi bir sanal ağ kuralına veya IP kuralına gerek kalmadan PostgreSQL için Azure Veritabanı ile iletişim kurabilir.

Ancak Ağustos 2018 itibariyle, PostgreSQL hizmeti için Azure Veritabanı henüz doğrudan bir alt ağa atanabilecek hizmetler arasında yer almaz.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Sanal ağ kuralları hakkında ayrıntılar

Bu bölümde sanal ağ kuralları hakkında çeşitli ayrıntılar açıklanmaktadır.

### <a name="only-one-geographic-region"></a>Sadece bir coğrafi bölge

Her Sanal Ağ hizmeti bitiş noktası yalnızca bir Azure bölgesi için geçerlidir. Bitiş noktası, diğer bölgelerin alt ağdan iletişimi kabul etmesini sağlamaz.

Herhangi bir sanal ağ kuralı, temel bitiş noktasının geçerli olduğu bölgeyle sınırlıdır.

### <a name="server-level-not-database-level"></a>Sunucu düzeyinde değil, veritabanı düzeyi

Her sanal ağ kuralı, yalnızca sunucudaki belirli bir veritabanı için değil, PostgreSQL sunucusu için tüm Azure Veritabanınız için geçerlidir. Başka bir deyişle, sanal ağ kuralı veritabanı düzeyinde değil, sunucu düzeyinde geçerlidir.

#### <a name="security-administration-roles"></a>Güvenlik yönetimi rolleri

Sanal Ağ hizmeti uç noktalarının yönetiminde güvenlik rollerinin ayrılması vardır. Eylem aşağıdaki rollerin her birinden gereklidir:

- **Ağ Yöneticisi:** &nbsp; Bitiş noktasını açın.
- **Veritabanı Yöneticisi:** &nbsp; Verilen alt ağı PostgreSQL sunucusu için Azure Veritabanına eklemek için erişim denetim listesini (ACL) güncelleştirin.

*RBAC alternatif:*

Network Admin ve Database Admin rolleri sanal ağ kurallarını yönetmek için gerekenden daha fazla yetenek vardır. Yalnızca yeteneklerinin bir alt kümesi gereklidir.

Yalnızca gerekli yetenek alt kümesine sahip tek bir özel rol oluşturmak için Azure'da [rol tabanlı erişim denetimi (RBAC)][rbac-what-is-813s] kullanma seçeneğiniz vardır. Özel rol, Ağ Yöneticisi'ni veya Veritabanı Yöneticisini dahil etmek yerine kullanılabilir. Bir kullanıcıyı özel bir role eklerseniz, kullanıcıyı diğer iki ana yönetici rolüne eklerseniz, güvenlik maruziyetinizin yüzey alanı daha düşüktür.

> [!NOTE]
> Bazı durumlarda PostgreSQL için Azure Veritabanı ve VNet alt ağı farklı aboneliklerdedir. Bu gibi durumlarda aşağıdaki yapılandırmaları sağlamanız gerekir:
> - Her iki abonelik de aynı Azure Etkin Dizin kiracısında olmalıdır.
> - Kullanıcı, hizmet uç noktalarını etkinleştirme ve verilen Sunucuya bir VNet alt ağı ekleme gibi işlemleri başlatmak için gerekli izinlere sahiptir.
> - Her iki aboneliğin de **Microsoft.Sql** kaynak sağlayıcısının kayıtlı olduğundan emin olun. Daha fazla bilgi için [kaynak yöneticisi-kayıt][resource-manager-portal]

## <a name="limitations"></a>Sınırlamalar

PostgreSQL için Azure Veritabanı için sanal ağ kuralları özelliği aşağıdaki sınırlamaları vardır:

- Bir Web Uygulaması, VNet/subnet'teki özel bir IP'ye eşlenebilir. Verilen VNet/alt ağdan hizmet bitiş noktaları açık olsa bile, Web App'tan sunucuya bağlantılarvnet/subnet kaynağına değil, Azure genel IP kaynağına sahip olur. Bir Web Uygulamasından VNet güvenlik duvarı kurallarına sahip bir sunucuya bağlantı sağlamak için, Azure hizmetlerinin sunucudaki sunucuya erişmesine izin vermelisiniz.

- PostgreSQL için Azure Veritabanınızın güvenlik duvarında her sanal ağ kuralı bir alt ağa başvurur. Tüm bu başvurulan alt ağlar, PostgreSQL için Azure Veritabanı'nı barındıran aynı coğrafi bölgede barındırılmalıdır.

- PostgreSQL sunucusu için her Azure Veritabanı'nda herhangi bir sanal ağ için en fazla 128 ACL girişi olabilir.

- Sanal ağ kuralları yalnızca Azure Kaynak Yöneticisi sanal ağları için geçerlidir; ve [klasik dağıtım modeli][arm-deployment-model-568f] ağları için değil.

- **Microsoft.Sql** hizmet etiketini kullanarak Sanal Ağ Hizmeti bitiş noktalarını PostgreSQL için Azure Veritabanı'na açmak, tüm Azure Veritabanı hizmetleri için uç noktaları da sağlar: MySQL için Azure Veritabanı, PostgreSQL için Azure Veritabanı, Azure SQL Veritabanı ve Azure SQL Veri Ambarı.

- VNet hizmet uç noktaları için destek yalnızca Genel Amaç ve Bellek Optimize Edilmiş sunucular içindir.

- Güvenlik duvarında, IP adresi aralıkları aşağıdaki ağ öğeleri için geçerlidir, ancak sanal ağ kuralları aşağıdakileri yapmaz:
    - [Siteden Siteye (S2S) sanal özel ağ (VPN)][vpn-gateway-indexmd-608y]
    - [ExpressRoute][expressroute-indexmd-744v] ile şirket içi

## <a name="expressroute"></a>ExpressRoute

Ağınız [ExpressRoute][expressroute-indexmd-744v]kullanımı yla Azure ağına bağlıysa, her devre Microsoft Edge'de iki genel IP adresiyle yapılandırılır. İki IP adresi, Azure Genel Eşleme'yi kullanarak Azure Depolama gibi Microsoft Hizmetlerine bağlanmak için kullanılır.

Devrenizden PostgreSQL için Azure Veritabanı'na iletişime izin vermek için, devrelerinizin genel IP adresleri için IP ağ kuralları oluşturmanız gerekir. ExpressRoute devrenizin genel IP adreslerini bulmak için Azure portalını kullanarak ExpressRoute ile bir destek bileti açın.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>VNET Hizmet Bitiş Noktalarını açmadan sunucunuza VNET Güvenlik Duvarı kuralı ekleme

Yalnızca bir Güvenlik Duvarı kuralı nı ayarlamak sunucunun VNet'e güvenli hale getirilen bir şey olduğunu göstermez. Güvenliğin etkili olması **için** VNet hizmet uç noktalarını açmanız gerekir. Hizmet bitiş noktalarını **Açık'ı**açtığınızda, VNet alt ağınız **Kapalı'dan** **Açık'a**geçişi tamamlayana kadar kapalı kalma süresi yle karşı karşıya kalır. Bu özellikle büyük VNets bağlamında geçerlidir. Geçiş sırasında kapalı kalma süresini azaltmak veya ortadan kaldırmak için **Yok SaymaHizmetiSon Nokta** bayrağını kullanabilirsiniz.

Azure CLI veya portalını kullanarak **Yok SaymaHizmetiBitiş Noktası** bayrağını ayarlayabilirsiniz.

## <a name="related-articles"></a>İlgili makaleler:
- [Azure sanal ağlar][vm-virtual-network-overview]
- [Azure sanal ağ hizmeti bitiş noktaları][vm-virtual-network-service-endpoints-overview-649d]

## <a name="next-steps"></a>Sonraki adımlar
VNet kuralları oluşturma yla ilgili makaleler için bkz:
- [Azure portalını kullanarak PostgreSQL VNet kuralları için Azure Veritabanı oluşturma ve yönetme](howto-manage-vnet-using-portal.md)
- [Azure CLI'yi kullanarak PostgreSQL VNet kuralları için Azure Veritabanı oluşturma ve yönetme](howto-manage-vnet-using-cli.md)


<!-- Link references, to text, Within this same GitHub repo. -->
[arm-deployment-model-568f]: ../azure-resource-manager/management/deployment-models.md

[vm-virtual-network-overview]: ../virtual-network/virtual-networks-overview.md

[vm-virtual-network-service-endpoints-overview-649d]: ../virtual-network/virtual-network-service-endpoints-overview.md

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

[expressroute-indexmd-744v]: ../expressroute/index.yml

[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md
