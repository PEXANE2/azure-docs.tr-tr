---
title: Yönetilen bir örnek için bağlantı mimarisi
description: Azure SQL Veritabanı yönetilen örnek iletişim iúlemi ve bağlantı mimarisinin yanı sıra bileşenlerin yönetilen örne nasıl yönlendirildiği hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: fasttrack-edit
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: e4d6098b7b4de76461e924fc7d42d039046d7ce5
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677162"
---
# <a name="connectivity-architecture-for-a-managed-instance-in-azure-sql-database"></a>Azure SQL Veritabanı'nda yönetilen bir örnek için bağlantı mimarisi

Bu makalede, Azure SQL Veritabanı yönetilen örnekte iletişim açıklanmaktadır. Ayrıca, bağlantı mimarisini ve bileşenlerin trafiği yönetilen örniğe nasıl yönlendirildiğini de açıklar.  

SQL Veritabanı yönetilen örneği Azure sanal ağının ve yönetilen örneklere adanmış alt ağ içine yerleştirilir. Bu dağıtım şunları sağlar:

- Güvenli bir özel IP adresi.
- Şirket içi ağı yönetilen bir örneğe bağlama yeteneği.
- Yönetilen bir örneği bağlantılı bir sunucuya veya başka bir şirket içi veri deposuna bağlama yeteneği.
- Yönetilen bir örneği Azure kaynaklarına bağlama yeteneği.

## <a name="communication-overview"></a>İletişime genel bakış

Aşağıdaki diyagram, yönetilen bir örne bağlanan varlıkları gösterir. Ayrıca, yönetilen örnekle iletişim kurması gereken kaynakları da gösterir. Diyagramın altındaki iletişim işlemi, yönetilen örne veri kaynağı olarak bağlanan müşteri uygulamalarını ve araçlarını temsil eder.  

![Bağlantı mimarisindeki varlıklar](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Yönetilen örnek, hizmet (PaaS) sunan bir platformdur. Microsoft, bu hizmeti telemetri veri akışlarına dayalı olarak yönetmek için otomatik aracılar (yönetim, dağıtım ve bakım) kullanır. Microsoft yönetimden sorumlu olduğundan, müşteriler yönetilen örnek sanal küme makinelerine Uzak Masaüstü Protokolü (RDP) aracılığıyla erişemez.

Son kullanıcılar veya uygulamalar tarafından başlatılan bazı SQL Server işlemleri, yönetilen örneklerin platformla etkileşimkurmasını gerektirebilir. Bir durumda yönetilen bir örnek veritabanı oluşturulmasıdır. Bu kaynak Azure portalı, PowerShell, Azure CLI ve REST API'si aracılığıyla ortaya çıkarır.

Yönetilen örnekler, yedeklemeler için Azure Depolama, telemetri için Azure Etkinlik Hub'ları, kimlik doğrulaması için Azure Active Directory, Saydam Veri Şifreleme için Azure Key Vault (TDE) ve güvenlik ve desteklenebilirlik özellikleri sağlayan birkaç Azure platformu hizmeti gibi Azure hizmetlerine bağlıdır. Yönetilen örnekler bu hizmetlere bağlantılar yapar.

Tüm iletişimler sertifikalar kullanılarak şifrelenir ve imzalanır. İletişim kuran tarafların güvenilirliğini kontrol etmek için yönetilen örnekler, bu sertifikaları sertifika iptal listeleri aracılığıyla sürekli olarak doğrular. Sertifikalar iptal edilirse, yönetilen örnek verileri korumak için bağlantıları kapatır.

## <a name="high-level-connectivity-architecture"></a>Üst düzey bağlantı mimarisi

Yüksek düzeyde, yönetilen bir örnek bir hizmet bileşenleri kümesidir. Bu bileşenler, müşterinin sanal ağ alt ağında çalışan özel bir yalıtılmış sanal makine kümesinde barındırılır. Bu makineler sanal bir küme oluştururlar.

Sanal küme, yönetilen birden çok örneği barındırabilir. Gerekirse, müşteri alt ağda verilen örneklerin sayısını değiştirdiğinde küme otomatik olarak genişler veya sözleşme ler verir.

Müşteri uygulamaları yönetilen örneklere bağlanabilir ve sanal ağ, eşlenen sanal ağ veya VPN veya Azure ExpressRoute ile bağlanan ağ içindeki veritabanlarını sorgulayabilir ve güncelleyebilir. Bu ağ bir bitiş noktası ve özel bir IP adresi kullanmalıdır.  

![Bağlantı mimarisi diyagramı](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Microsoft yönetimi ve dağıtım hizmetleri sanal ağ dışında çalışır. Yönetilen bir örnek ve Microsoft hizmetleri, ortak IP adreslerine sahip uç noktalarüzerinden bağlanır. Yönetilen bir örnek giden bir bağlantı oluşturduğunda, son Ağ Adresi Çevirisi (NAT) alırken bağlantı, bu genel IP adresinden geliyor gibi görünmesini sağlar.

Yönetim trafiği müşterinin sanal ağında akar. Bu, sanal ağın altyapısının öğelerinin, örneğin başarısız olmasını ve kullanılamaz hale gelmesiyle yönetim trafiğine zarar verebildiği anlamına gelir.

> [!IMPORTANT]
> Microsoft, müşteri deneyimini ve hizmet kullanılabilirliğini iyileştirmek için Azure sanal ağ altyapısı öğelerine ağ amacı ilkesi uygular. İlke, yönetilen örneğin nasıl çalıştığını etkileyebilir. Bu platform mekanizması, ağ gereksinimlerini kullanıcılara saydam bir şekilde iletır. İlkenin ana hedefi, ağ yanlış yapılandırmasını önlemek ve normal yönetilen örnek işlemlerini sağlamaktır. Yönetilen bir örneği sildiğinizde, ağ amacı ilkesi de kaldırılır.

## <a name="virtual-cluster-connectivity-architecture"></a>Sanal küme bağlantısı mimarisi

Yönetilen örnekler için bağlantı mimarisine daha derin bir dalış yapalım. Aşağıdaki diyagram sanal kümenin kavramsal düzenini gösterir.

![Sanal kümenin bağlantı mimarisi](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

İstemciler, formu `<mi_name>.<dns_zone>.database.windows.net`olan bir ana bilgisayar adı kullanarak yönetilen bir örneğe bağlanır. Bu ana bilgisayar adı, ortak bir Etki Alanı Adı Sistemi (DNS) bölgesinde kayıtlı olmasına ve genel olarak çözülebilir olmasına rağmen özel bir IP adresine giderilir. Küme `zone-id` yi oluşturduğunuzda otomatik olarak oluşturulur. Yeni oluşturulan bir küme ikincil yönetilen bir örneği barındırıyorsa, bölge kimliğini birincil kümeyle paylaşır. Daha fazla bilgi için [bkz.](sql-database-auto-failover-group.md#enabling-geo-replication-between-managed-instances-and-their-vnets)

Bu özel IP adresi yönetilen örneğin iç yük dengeleyicisine aittir. Yük dengeleyicisi trafiği yönetilen örneğin ağ geçidine yönlendirir. Birden çok yönetilen örnek aynı küme içinde çalıştırılabildiği için, ağ geçidi trafiği doğru SQL altyapısı hizmetine yönlendirmek için yönetilen örneğin ana bilgisayar adını kullanır.

Yönetim ve dağıtım hizmetleri, harici bir yük dengeleyicisine eşleyen bir [yönetim bitiş noktası](#management-endpoint) kullanarak yönetilen bir örne bağlanır. Trafik düğümlere yalnızca yönetilen örneğin yönetim bileşenlerinin kullandığı önceden tanımlanmış bir bağlantı noktası kümesinde alınırsa yönlendirilir. Düğümlerde yerleşik bir güvenlik duvarı, yalnızca Microsoft IP aralıklarından gelen trafiğe izin verecek şekilde ayarlanır. Sertifikalar, yönetim bileşenleri ve yönetim düzlemi arasındaki tüm iletişimi karşılıklı olarak doğrular.

## <a name="management-endpoint"></a>Yönetim uç noktası

Microsoft yönetilen örneği bir yönetim bitiş noktası kullanarak yönetir. Bu bitiş noktası, örneğin sanal kümesinin içindedir. Yönetim bitiş noktası, ağ düzeyinde yerleşik bir güvenlik duvarı tarafından korunur. Uygulama düzeyinde, karşılıklı sertifika doğrulaması ile korunur. Bitiş noktasının IP adresini bulmak için [bkz.](sql-database-managed-instance-find-management-endpoint-ip-address.md)

Bağlantılar yönetilen örnek içinde başladığında (yedeklemeler ve denetim günlüklerinde olduğu gibi), trafik yönetim bitiş noktasının ortak IP adresinden başlar gibi görünür. Yalnızca yönetilen örneğin IP adresine izin verecek şekilde güvenlik duvarı kuralları ayarlayarak yönetilen bir örnekten kamu hizmetlerine erişimi sınırlandırabilirsiniz. Daha fazla bilgi için bkz. [Yönetilen örneğin yerleşik güvenlik duvarını doğrula.](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md)

> [!NOTE]
> Yönetilen örneğin bulunduğu bölgenin içindeki Azure hizmetlerine giden trafik en iyi duruma sunulur ve bu nedenle yönetilen örnek yönetimi bitiş noktası public IP adresine uygun değildir. Bu nedenle, en sık depolama alanı için IP tabanlı güvenlik duvarı kurallarını kullanmanız gerekiyorsa, hizmetin yönetilen örnekten farklı bir bölgede olması gerekir.

## <a name="service-aided-subnet-configuration"></a>Hizmet destekli alt ağ yapılandırması

Müşteri güvenliği ve yönetilebilirlik gereksinimlerini karşılamak için Yönetilen Örnek, el kitabından hizmet destekli alt ağ yapılandırmasına geçiş yapıyor.

Servis destekli subnet yapılandırması ile kullanıcı veri (TDS) trafiğinin tam kontrolündeyken Yönetilen Örnek, SLA'yı yerine getirmek için yönetim trafiğinin kesintisiz akışını sağlamakla yükümlüdür.

Otomatik ağ yapılandırma sıyrık yönetimini sağlamak ve hizmet bitiş noktalarını etkinleştirmek için hizmet destekli alt ağ yapılandırması sanal ağ [alt ağ delegasyonu](../virtual-network/subnet-delegation-overview.md) özelliğinin üzerine oluşturur. Hizmet bitiş noktaları, yedeklemeleri / denetim günlüklerini tutan depolama hesaplarındasanal ağ güvenlik duvarı kurallarını yapılandırmak için kullanılabilir.

### <a name="network-requirements"></a>Ağ gereksinimleri 

Yönetilen bir örneği sanal ağ içinde özel bir alt ağa dağıtın. Alt ağ şu özelliklere sahip olmalıdır:

- **Özel alt ağ:** Yönetilen örneğin alt ağı, kendisiyle ilişkili başka bir bulut hizmeti içeremez ve ağ geçidi alt ağı olamaz. Alt ağ yönetilen örnek dışında herhangi bir kaynak içeremez ve daha sonra alt ağa diğer kaynak türleri ekemezsiniz.
- **Subnet delegasyonu:** Yönetilen örneğin alt netkaynak sağlayıcısına `Microsoft.Sql/managedInstances` devredilmesi gerekir.
- **Ağ güvenlik grubu (NSG):** Bir NSG yönetilen örneğin alt ağı ile ilişkili olması gerekir. Yönetilen örnek yeniden yönlendirme bağlantıları için yapılandırıldığında bağlantı noktası 1433 ve port 11000-11999 üzerinde trafik filtreleyerek yönetilen örneğin veri bitiş noktasına erişimi denetlemek için bir NSG kullanabilirsiniz. Hizmet, yönetim trafiğinin kesintisiz akışına izin vermek için gereken geçerli [kuralları](#mandatory-inbound-security-rules-with-service-aided-subnet-configuration) otomatik olarak sağlar ve tutar.
- **Kullanıcı tanımlı rota (UDR) tablosu:** UDR tablosunun yönetilen örneğin alt ağıyla ilişkilendirilmesi gerekir. Sanal ağ ağ geçidi veya sanal ağ cihazı (NVA) aracılığıyla hedef olarak şirket içi özel IP aralıklarına sahip trafiği yönlendirmek için rota tablosuna girişler ekleyebilirsiniz. Hizmet, yönetim trafiğinin kesintisiz akışına izin vermek için geçerli [girişleri](#user-defined-routes-with-service-aided-subnet-configuration) otomatik olarak sağlar ve tutar.
- **Yeterli IP adresleri:** Yönetilen örnek alt netinin en az 16 IP adresi olmalıdır. Önerilen minimum 32 IP adresidir. Daha fazla bilgi için bkz. [yönetilen örnekler için alt netboyutunu belirleyin.](sql-database-managed-instance-determine-size-vnet-subnet.md) [Yönetilen örnekler](#network-requirements)için ağ gereksinimlerini karşılamak üzere yapılandırdıktan sonra yönetilen örnekleri [varolan ağda](sql-database-managed-instance-configure-vnet-subnet.md) dağıtabilirsiniz. Aksi takdirde, yeni bir [ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md)oluşturun.

> [!IMPORTANT]
> Yönetilen bir örnek oluşturduğunuzda, ağ kurulumunda uyumlu olmayan değişiklikleri önlemek için alt ağ da ağ amacı ilkesi uygulanır. Son örnek alt ağdan kaldırıldıktan sonra, ağ amaç ilkesi de kaldırılır.

### <a name="mandatory-inbound-security-rules-with-service-aided-subnet-configuration"></a>Hizmet destekli subnet yapılandırması ile zorunlu gelen güvenlik kuralları 

| Adı       |Bağlantı noktası                        |Protokol|Kaynak           |Hedef|Eylem|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |SqlManagement    |MI ALT AĞ  |İzin Ver |
|            |9000, 9003                  |TCP     |CorpnetSaw       |MI ALT AĞ  |İzin Ver |
|            |9000, 9003                  |TCP     |CorpnetPublic    |MI ALT AĞ  |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |MI ALT AĞ        |MI ALT AĞ  |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|MI ALT AĞ  |İzin Ver |

### <a name="mandatory-outbound-security-rules-with-service-aided-subnet-configuration"></a>Hizmet destekli subnet yapılandırması ile zorunlu giden güvenlik kuralları 

| Adı       |Bağlantı noktası          |Protokol|Kaynak           |Hedef|Eylem|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |443, 12000    |TCP     |MI ALT AĞ        |AzureCloud |İzin Ver |
|mi_subnet   |Herhangi biri           |Herhangi biri     |MI ALT AĞ        |MI ALT AĞ  |İzin Ver |

### <a name="user-defined-routes-with-service-aided-subnet-configuration"></a>Hizmet destekli subnet yapılandırması ile kullanıcı tanımlı rotalar 

|Adı|Adres ön eki|Sonraki Hop|
|----|--------------|-------|
|subnet-to-vnetlocal|MI ALT AĞ|Sanal ağ|
|mi-13-64-11-nexthop-internet|13.64.0.0/11|Internet|
|mi-13-104-14-nexthop-internet|13.104.0.0/14|Internet|
|mi-20-33-16-nexthop-internet|20.33.0.0/16|Internet|
|mi-20-34-15-nexthop-internet|20.34.0.0/15|Internet|
|mi-20-36-14-nexthop-internet|20.36.0.0/14|Internet|
|mi-20-40-13-nexthop-internet|20.40.0.0/13|Internet|
|mi-20-48-12-nexthop-internet|20.48.0.0/12|Internet|
|mi-20-64-10-nexthop-internet|20.64.0.0/10|Internet|
|mi-20-128-16-nexthop-internet|20.128.0.0/16|Internet|
|mi-20-135-16-nexthop-internet|20.135.0.0/16|Internet|
|mi-20-136-16-nexthop-internet|20.136.0.0/16|Internet|
|mi-20-140-15-nexthop-internet|20.140.0.0/15|Internet|
|mi-20-143-16-nexthop-internet|20.143.0.0/16|Internet|
|mi-20-144-14-nexthop-internet|20.144.0.0/14|Internet|
|mi-20-150-15-nexthop-internet|20.150.0.0/15|Internet|
|mi-20-160-12-nexthop-internet|20.160.0.0/12|Internet|
|mi-20-176-14-nexthop-internet|20.176.0.0/14|Internet|
|mi-20-180-14-nexthop-internet|20.180.0.0/14|Internet|
|mi-20-184-13-nexthop-internet|20.184.0.0/13|Internet|
|mi-20-192-10-nexthop-internet|20.192.0.0/10|Internet|
|mi-40-64-10-nexthop-internet|40.64.0.0/10|Internet|
|mi-51-4-15-nexthop-internet|51.4.0.0/15|Internet|
|mi-51-8-16-nexthop-internet|51.8.0.0/16|Internet|
|mi-51-10-15-nexthop-internet|51.10.0.0/15|Internet|
|mi-51-18-16-nexthop-internet|51.18.0.0/16|Internet|
|mi-51-51-16-nexthop-internet|51.51.0.0/16|Internet|
|mi-51-53-16-nexthop-internet|51.53.0.0/16|Internet|
|mi-51-103-16-nexthop-internet|51.103.0.0/16|Internet|
|mi-51-104-15-nexthop-internet|51.104.0.0/15|Internet|
|mi-51-132-16-nexthop-internet|51.132.0.0/16|Internet|
|mi-51-136-15-nexthop-internet|51.136.0.0/15|Internet|
|mi-51-138-16-nexthop-internet|51.138.0.0/16|Internet|
|mi-51-140-14-nexthop-internet|51.140.0.0/14|Internet|
|mi-51-144-15-nexthop-internet|51.144.0.0/15|Internet|
|mi-52-96-12-nexthop-internet|52.96.0.0/12|Internet|
|mi-52-112-14-nexthop-internet|52.112.0.0/14|Internet|
|mi-52-125-16-nexthop-internet|52.125.0.0/16|Internet|
|mi-52-126-15-nexthop-internet|52.126.0.0/15|Internet|
|mi-52-130-15-nexthop-internet|52.130.0.0/15|Internet|
|mi-52-132-14-nexthop-internet|52.132.0.0/14|Internet|
|mi-52-136-13-nexthop-internet|52.136.0.0/13|Internet|
|mi-52-145-16-nexthop-internet|52.145.0.0/16|Internet|
|mi-52-146-15-nexthop-internet|52.146.0.0/15|Internet|
|mi-52-148-14-nexthop-internet|52.148.0.0/14|Internet|
|mi-52-152-13-nexthop-internet|52.152.0.0/13|Internet|
|mi-52-160-11-nexthop-internet|52.160.0.0/11|Internet|
|mi-52-224-11-nexthop-internet|52.224.0.0/11|Internet|
|mi-64-4-18-nexthop-internet|64.4.0.0/18|Internet|
|mi-65-52-14-nexthop-internet|65.52.0.0/14|Internet|
|mi-66-119-144-20-nexthop-internet|66.119.144.0/20|Internet|
|mi-70-37-17-nexthop-internet|70.37.0.0/17|Internet|
|mi-70-37-128-18-nexthop-internet|70.37.128.0/18|Internet|
|mi-91-190-216-21-nexthop-internet|91.190.216.0/21|Internet|
|mi-94-245-64-18-nexthop-internet|94.245.64.0/18|Internet|
|mi-103-9-8-22-nexthop-internet|103.9.8.0/22|Internet|
|mi-103-25-156-24-nexthop-internet|103.25.156.0/24|Internet|
|mi-103-25-157-24-nexthop-internet|103.25.157.0/24|Internet|
|mi-103-25-158-23-nexthop-internet|103.25.158.0/23|Internet|
|mi-103-36-96-22-nexthop-internet|103.36.96.0/22|Internet|
|mi-103-255-140-22-nexthop-internet|103.255.140.0/22|Internet|
|mi-104-40-13-nexthop-internet|104.40.0.0/13|Internet|
|mi-104-146-15-nexthop-internet|104.146.0.0/15|Internet|
|mi-104-208-13-nexthop-internet|104.208.0.0/13|Internet|
|mi-111-221-16-20-nexthop-internet|111.221.16.0/20|Internet|
|mi-111-221-64-18-nexthop-internet|111.221.64.0/18|Internet|
|mi-129-75-16-nexthop-internet|129.75.0.0/16|Internet|
|mi-131-107-16-nexthop-internet|131.107.0.0/16|Internet|
|mi-131-253-1-24-nexthop-internet|131.253.1.0/24|Internet|
|mi-131-253-3-24-nexthop-internet|131.253.3.0/24|Internet|
|mi-131-253-5-24-nexthop-internet|131.253.5.0/24|Internet|
|mi-131-253-6-24-nexthop-internet|131.253.6.0/24|Internet|
|mi-131-253-8-24-nexthop-internet|131.253.8.0/24|Internet|
|mi-131-253-12-22-nexthop-internet|131.253.12.0/22|Internet|
|mi-131-253-16-23-nexthop-internet|131.253.16.0/23|Internet|
|mi-131-253-18-24-nexthop-internet|131.253.18.0/24|Internet|
|mi-131-253-21-24-nexthop-internet|131.253.21.0/24|Internet|
|mi-131-253-22-23-nexthop-internet|131.253.22.0/23|Internet|
|mi-131-253-24-21-nexthop-internet|131.253.24.0/21|Internet|
|mi-131-253-32-20-nexthop-internet|131.253.32.0/20|Internet|
|mi-131-253-61-24-nexthop-internet|131.253.61.0/24|Internet|
|mi-131-253-62-23-nexthop-internet|131.253.62.0/23|Internet|
|mi-131-253-64-18-nexthop-internet|131.253.64.0/18|Internet|
|mi-131-253-128-17-nexthop-internet|131.253.128.0/17|Internet|
|mi-132-245-16-nexthop-internet|132.245.0.0/16|Internet|
|mi-134-170-16-nexthop-internet|134.170.0.0/16|Internet|
|mi-134-177-16-nexthop-internet|134.177.0.0/16|Internet|
|mi-137-116-15-nexthop-internet|137.116.0.0/15|Internet|
|mi-137-135-16-nexthop-internet|137.135.0.0/16|Internet|
|mi-138-91-16-nexthop-internet|138.91.0.0/16|Internet|
|mi-138-196-16-nexthop-internet|138.196.0.0/16|Internet|
|mi-139-217-16-nexthop-internet|139.217.0.0/16|Internet|
|mi-139-219-16-nexthop-internet|139.219.0.0/16|Internet|
|mi-141-251-16-nexthop-internet|141.251.0.0/16|Internet|
|mi-146-147-16-nexthop-internet|146.147.0.0/16|Internet|
|mi-147-243-16-nexthop-internet|147.243.0.0/16|Internet|
|mi-150-171-16-nexthop-internet|150.171.0.0/16|Internet|
|mi-150-242-48-22-nexthop-internet|150.242.48.0/22|Internet|
|mi-157-54-15-nexthop-internet|157.54.0.0/15|Internet|
|mi-157-56-14-nexthop-internet|157.56.0.0/14|Internet|
|mi-157-60-16-nexthop-internet|157.60.0.0/16|Internet|
|mi-167-105-16-nexthop-internet|167.105.0.0/16|Internet|
|mi-167-220-16-nexthop-internet|167.220.0.0/16|Internet|
|mi-168-61-16-nexthop-internet|168.61.0.0/16|Internet|
|mi-168-62-15-nexthop-internet|168.62.0.0/15|Internet|
|mi-191-232-13-nexthop-internet|191.232.0.0/13|Internet|
|mi-192-32-16-nexthop-internet|192.32.0.0/16|Internet|
|mi-192-48-225-24-nexthop-internet|192.48.225.0/24|Internet|
|mi-192-84-159-24-nexthop-internet|192.84.159.0/24|Internet|
|mi-192-84-160-23-nexthop-internet|192.84.160.0/23|Internet|
|mi-192-197-157-24-nexthop-internet|192.197.157.0/24|Internet|
|mi-193-149-64-19-nexthop-internet|193.149.64.0/19|Internet|
|mi-193-221-113-24-nexthop-internet|193.221.113.0/24|Internet|
|mi-194-69-96-19-nexthop-internet|194.69.96.0/19|Internet|
|mi-194-110-197-24-nexthop-internet|194.110.197.0/24|Internet|
|mi-198-105-232-22-nexthop-internet|198.105.232.0/22|Internet|
|mi-198-200-130-24-nexthop-internet|198.200.130.0/24|Internet|
|mi-198-206-164-24-nexthop-internet|198.206.164.0/24|Internet|
|mi-199-60-28-24-nexthop-internet|199.60.28.0/24|Internet|
|mi-199-74-210-24-nexthop-internet|199.74.210.0/24|Internet|
|mi-199-103-90-23-nexthop-internet|199.103.90.0/23|Internet|
|mi-199-103-122-24-nexthop-internet|199.103.122.0/24|Internet|
|mi-199-242-32-20-nexthop-internet|199.242.32.0/20|Internet|
|mi-199-242-48-21-nexthop-internet|199.242.48.0/21|Internet|
|mi-202-89-224-20-nexthop-internet|202.89.224.0/20|Internet|
|mi-204-13-120-21-nexthop-internet|204.13.120.0/21|Internet|
|mi-204-14-180-22-nexthop-internet|204.14.180.0/22|Internet|
|mi-204-79-135-24-nexthop-internet|204.79.135.0/24|Internet|
|mi-204-79-179-24-nexthop-internet|204.79.179.0/24|Internet|
|mi-204-79-181-24-nexthop-internet|204.79.181.0/24|Internet|
|mi-204-79-188-24-nexthop-internet|204.79.188.0/24|Internet|
|mi-204-79-195-24-nexthop-internet|204.79.195.0/24|Internet|
|mi-204-79-196-23-nexthop-internet|204.79.196.0/23|Internet|
|mi-204-79-252-24-nexthop-internet|204.79.252.0/24|Internet|
|mi-204-152-18-23-nexthop-internet|204.152.18.0/23|Internet|
|mi-204-152-140-23-nexthop-internet|204.152.140.0/23|Internet|
|mi-204-231-192-24-nexthop-internet|204.231.192.0/24|Internet|
|mi-204-231-194-23-nexthop-internet|204.231.194.0/23|Internet|
|mi-204-231-197-24-nexthop-internet|204.231.197.0/24|Internet|
|mi-204-231-198-23-nexthop-internet|204.231.198.0/23|Internet|
|mi-204-231-200-21-nexthop-internet|204.231.200.0/21|Internet|
|mi-204-231-208-20-nexthop-internet|204.231.208.0/20|Internet|
|mi-204-231-236-24-nexthop-internet|204.231.236.0/24|Internet|
|mi-205-174-224-20-nexthop-internet|205.174.224.0/20|Internet|
|mi-206-138-168-21-nexthop-internet|206.138.168.0/21|Internet|
|mi-206-191-224-19-nexthop-internet|206.191.224.0/19|Internet|
|mi-207-46-16-nexthop-internet|207.46.0.0/16|Internet|
|mi-207-68-128-18-nexthop-internet|207.68.128.0/18|Internet|
|mi-208-68-136-21-nexthop-internet|208.68.136.0/21|Internet|
|mi-208-76-44-22-nexthop-internet|208.76.44.0/22|Internet|
|mi-208-84-21-nexthop-internet|208.84.0.0/21|Internet|
|mi-209-240-192-19-nexthop-internet|209.240.192.0/19|Internet|
|mi-213-199-128-18-nexthop-internet|213.199.128.0/18|Internet|
|mi-216-32-180-22-nexthop-internet|216.32.180.0/22|Internet|
|mi-216-220-208-20-nexthop-internet|216.220.208.0/20|Internet|
|mi-23-96-13-nexthop-internet|23.96.0.0/13|Internet|
|mi-42-159-16-nexthop-internet|42.159.0.0/16|Internet|
|mi-51-13-17-nexthop-internet|51.13.0.0/17|Internet|
|mi-51-107-16-nexthop-internet|51.107.0.0/16|Internet|
|mi-51-116-16-nexthop-internet|51.116.0.0/16|Internet|
|mi-51-120-16-nexthop-internet|51.120.0.0/16|Internet|
|mi-51-120-128-17-nexthop-internet|51.120.128.0/17|Internet|
|mi-51-124-16-nexthop-internet|51.124.0.0/16|Internet|
|mi-102-37-18-nexthop-internet|102.37.0.0/18|Internet|
|mi-102-133-16-nexthop-internet|102.133.0.0/16|Internet|
|mi-199-30-16-20-nexthop-internet|199.30.16.0/20|Internet|
|mi-204-79-180-24-nexthop-internet|204.79.180.0/24|Internet|
||||

\*MI SUBNET, x.x.x.x/y formundaki alt net için IP adres aralığını ifade eder. Bu bilgileri Azure portalında, alt ağ özelliklerinde bulabilirsiniz.

Ayrıca, sanal ağ ağ geçidi veya sanal ağ cihazı (NVA) aracılığıyla hedef olarak şirket içi özel IP aralıklarına sahip trafiği yönlendirmek için rota tablosuna girişler ekleyebilirsiniz.

Sanal ağ özel bir DNS içeriyorsa, özel DNS sunucusu nun genel dns kayıtlarını çözebilmesi gerekir. Azure AD Kimlik Doğrulaması gibi ek özellikleri kullanmak için ek FQDN'lerin çözülmesi gerekebilir. Daha fazla bilgi için [bkz.](sql-database-managed-instance-custom-dns.md)

### <a name="networking-constraints"></a>Ağ kısıtlamaları

**TLS 1.2 giden bağlantılara uygulanır**: Ocak 2020'de Microsoft, tüm Azure hizmetlerinde hizmet içi trafik için TLS 1.2'yi uygulamıştır. Azure SQL Veritabanı yönetilen örnek için, bu TLS 1.2'nin çoğaltma için kullanılan giden bağlantılarda ve SQL Server'a bağlı sunucu bağlantılarında zorlanmasına neden oldu. Yönetilen Örnek ile 2016'dan daha eski SQL Server sürümlerini kullanıyorsanız, lütfen [TLS 1.2 özel güncelleştirmelerinin](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server) uygulandığından emin olun.

Aşağıdaki sanal ağ özellikleri şu anda Yönetilen Örnek ile desteklenmez:
- **Microsoft peering**: Yönetilen Örnek'in bulunduğu sanal ağla doğrudan veya geçişli olarak ekspres rota devrelerine [bakan Microsoft'un](../expressroute/expressroute-faqs.md#microsoft-peering) sanal ağ içindeki Yönetilen Örnek bileşenleri ile kullanılabilirlik sorunlarına neden olmasına bağlı hizmetler arasındaki trafik akışını etkilemesini etkinleştirme. Microsoft'un zaten etkin olduğu sanal ağa Yönetilen Örnek dağıtımlarının başarısız olması beklenir.
- **Küresel sanal ağ eşlemesi**: Azure bölgeleri genelinde [sanal ağ eşleme](../virtual-network/virtual-network-peering-overview.md) bağlantısı, [belgelenmiş yük dengeleyici kısıtlamaları](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)nedeniyle Yönetilen Örnek için çalışmıyor.
- **AzurePlatformDNS**: Platform DNS çözünürlüğünü engellemek için AzurePlatformDNS [hizmet etiketini](../virtual-network/service-tags-overview.md) kullanmak Yönetilen Örneği kullanılamaz hale getirir. Yönetilen Örnek, motor içinde DNS çözünürlüğü için müşteri tanımlı DNS'yi desteklese de platform işlemleri için platform DNS'ye bağımlılık vardır.
- **NAT ağ geçidi**: Belirli genel IP adresiyle giden bağlantıyı denetlemek için [Sanal Ağ NAT'ı](../virtual-network/nat-overview.md) kullanmak Yönetilen Örneği kullanılamaz hale getirir. Yönetilen Örnek hizmeti şu anda Sanal Ağ NAT ile gelen ve giden akışların birlikteliğini sağlamayan temel yük dengeleyicisinin kullanımıyla sınırlıdır.

### <a name="deprecated-network-requirements-without-service-aided-subnet-configuration"></a>[Amortismana uğradı] Hizmet destekli subnet yapılandırması olmayan ağ gereksinimleri

Yönetilen bir örneği sanal ağ içinde özel bir alt ağa dağıtın. Alt ağ şu özelliklere sahip olmalıdır:

- **Özel alt ağ:** Yönetilen örneğin alt ağı, kendisiyle ilişkili başka bir bulut hizmeti içeremez ve ağ geçidi alt ağı olamaz. Alt ağ yönetilen örnek dışında herhangi bir kaynak içeremez ve daha sonra alt ağa diğer kaynak türleri ekemezsiniz.
- **Ağ güvenlik grubu (NSG):** Sanal ağla ilişkili bir NSG, gelen güvenlik kurallarını ve [giden güvenlik](#mandatory-outbound-security-rules) [kurallarını](#mandatory-inbound-security-rules) diğer kurallardan önce tanımlamalıdır. Yönetilen örnek yeniden yönlendirme bağlantıları için yapılandırıldığında bağlantı noktası 1433 ve port 11000-11999 üzerinde trafik filtreleyerek yönetilen örneğin veri bitiş noktasına erişimi denetlemek için bir NSG kullanabilirsiniz.
- **Kullanıcı tanımlı rota (UDR) tablosu:** Sanal ağla ilişkili bir UDR tablosu belirli [girişleri](#user-defined-routes)içermelidir.
- **Hizmet bitiş noktası yok:** Hiçbir hizmet bitiş noktası yönetilen örneğin alt ağıyla ilişkilendirilmemelidir. Sanal ağ oluştururken hizmet uç noktaları seçeneğinin devre dışı bırakıldığından emin olun.
- **Yeterli IP adresleri:** Yönetilen örnek alt netinin en az 16 IP adresi olmalıdır. Önerilen minimum 32 IP adresidir. Daha fazla bilgi için bkz. [yönetilen örnekler için alt netboyutunu belirleyin.](sql-database-managed-instance-determine-size-vnet-subnet.md) [Yönetilen örnekler](#network-requirements)için ağ gereksinimlerini karşılamak üzere yapılandırdıktan sonra yönetilen örnekleri [varolan ağda](sql-database-managed-instance-configure-vnet-subnet.md) dağıtabilirsiniz. Aksi takdirde, yeni bir [ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md)oluşturun.

> [!IMPORTANT]
> Hedef alt ağ bu özelliklere sahip değilse, yönetilen yeni bir örneği dağıtamazsınız. Yönetilen bir örnek oluşturduğunuzda, ağ kurulumunda uyumlu olmayan değişiklikleri önlemek için alt ağ da ağ amacı ilkesi uygulanır. Son örnek alt ağdan kaldırıldıktan sonra, ağ amaç ilkesi de kaldırılır.

### <a name="mandatory-inbound-security-rules"></a>Zorunlu gelen güvenlik kuralları

| Adı       |Bağlantı noktası                        |Protokol|Kaynak           |Hedef|Eylem|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |Herhangi biri              |MI ALT AĞ  |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |MI ALT AĞ        |MI ALT AĞ  |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|MI ALT AĞ  |İzin Ver |

### <a name="mandatory-outbound-security-rules"></a>Zorunlu giden güvenlik kuralları

| Adı       |Bağlantı noktası          |Protokol|Kaynak           |Hedef|Eylem|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |443, 12000    |TCP     |MI ALT AĞ        |AzureCloud |İzin Ver |
|mi_subnet   |Herhangi biri           |Herhangi biri     |MI ALT AĞ        |MI ALT AĞ  |İzin Ver |

> [!IMPORTANT]
> 9000, 9003, 1438, 1440, 1452 bağlantı noktaları için yalnızca bir gelen kuralı ve 443, 12000 bağlantı noktaları için bir giden kural olduğundan emin olun. Her bağlantı noktası için gelen ve giden kurallar ayrı ayrı yapılandırılırsa, Azure Kaynak Yöneticisi dağıtımları aracılığıyla Yönetilen Örnek sağlama başarısız olur. Bu bağlantı noktaları ayrı kurallarda ise, dağıtım hata kodu ile başarısız olur`VnetSubnetConflictWithIntendedPolicy`

\*MI SUBNET, x.x.x.x/y formundaki alt net için IP adres aralığını ifade eder. Bu bilgileri Azure portalında, alt ağ özelliklerinde bulabilirsiniz.

> [!IMPORTANT]
> Gerekli gelen güvenlik kuralları 9000, 9003, 1438, 1440 ve 1452 bağlantı _noktalarındaherhangi_ bir kaynaktan gelen trafiğe izin vermesine rağmen, bu bağlantı noktaları yerleşik bir güvenlik duvarı tarafından korunur. Daha fazla bilgi için [bkz.](sql-database-managed-instance-find-management-endpoint-ip-address.md)

> [!NOTE]
> Yönetilen bir örnekte işlem çoğaltma kullanıyorsanız ve herhangi bir örnek veritabanını yayımcı veya dağıtıcı olarak kullanıyorsanız, alt netin güvenlik kurallarında 445 (TCP giden) bağlantı noktasını açın. Bu bağlantı noktası, Azure dosya paylaşımına erişime izin verir.

### <a name="user-defined-routes"></a>Kullanıcı tanımlı rotalar

|Adı|Adres ön eki|Sonraki Hop|
|----|--------------|-------|
|subnet_to_vnetlocal|MI ALT AĞ|Sanal ağ|
|mi-13-64-11-nexthop-internet|13.64.0.0/11|Internet|
|mi-13-104-14-nexthop-internet|13.104.0.0/14|Internet|
|mi-20-33-16-nexthop-internet|20.33.0.0/16|Internet|
|mi-20-34-15-nexthop-internet|20.34.0.0/15|Internet|
|mi-20-36-14-nexthop-internet|20.36.0.0/14|Internet|
|mi-20-40-13-nexthop-internet|20.40.0.0/13|Internet|
|mi-20-48-12-nexthop-internet|20.48.0.0/12|Internet|
|mi-20-64-10-nexthop-internet|20.64.0.0/10|Internet|
|mi-20-128-16-nexthop-internet|20.128.0.0/16|Internet|
|mi-20-135-16-nexthop-internet|20.135.0.0/16|Internet|
|mi-20-136-16-nexthop-internet|20.136.0.0/16|Internet|
|mi-20-140-15-nexthop-internet|20.140.0.0/15|Internet|
|mi-20-143-16-nexthop-internet|20.143.0.0/16|Internet|
|mi-20-144-14-nexthop-internet|20.144.0.0/14|Internet|
|mi-20-150-15-nexthop-internet|20.150.0.0/15|Internet|
|mi-20-160-12-nexthop-internet|20.160.0.0/12|Internet|
|mi-20-176-14-nexthop-internet|20.176.0.0/14|Internet|
|mi-20-180-14-nexthop-internet|20.180.0.0/14|Internet|
|mi-20-184-13-nexthop-internet|20.184.0.0/13|Internet|
|mi-20-192-10-nexthop-internet|20.192.0.0/10|Internet|
|mi-40-64-10-nexthop-internet|40.64.0.0/10|Internet|
|mi-51-4-15-nexthop-internet|51.4.0.0/15|Internet|
|mi-51-8-16-nexthop-internet|51.8.0.0/16|Internet|
|mi-51-10-15-nexthop-internet|51.10.0.0/15|Internet|
|mi-51-18-16-nexthop-internet|51.18.0.0/16|Internet|
|mi-51-51-16-nexthop-internet|51.51.0.0/16|Internet|
|mi-51-53-16-nexthop-internet|51.53.0.0/16|Internet|
|mi-51-103-16-nexthop-internet|51.103.0.0/16|Internet|
|mi-51-104-15-nexthop-internet|51.104.0.0/15|Internet|
|mi-51-132-16-nexthop-internet|51.132.0.0/16|Internet|
|mi-51-136-15-nexthop-internet|51.136.0.0/15|Internet|
|mi-51-138-16-nexthop-internet|51.138.0.0/16|Internet|
|mi-51-140-14-nexthop-internet|51.140.0.0/14|Internet|
|mi-51-144-15-nexthop-internet|51.144.0.0/15|Internet|
|mi-52-96-12-nexthop-internet|52.96.0.0/12|Internet|
|mi-52-112-14-nexthop-internet|52.112.0.0/14|Internet|
|mi-52-125-16-nexthop-internet|52.125.0.0/16|Internet|
|mi-52-126-15-nexthop-internet|52.126.0.0/15|Internet|
|mi-52-130-15-nexthop-internet|52.130.0.0/15|Internet|
|mi-52-132-14-nexthop-internet|52.132.0.0/14|Internet|
|mi-52-136-13-nexthop-internet|52.136.0.0/13|Internet|
|mi-52-145-16-nexthop-internet|52.145.0.0/16|Internet|
|mi-52-146-15-nexthop-internet|52.146.0.0/15|Internet|
|mi-52-148-14-nexthop-internet|52.148.0.0/14|Internet|
|mi-52-152-13-nexthop-internet|52.152.0.0/13|Internet|
|mi-52-160-11-nexthop-internet|52.160.0.0/11|Internet|
|mi-52-224-11-nexthop-internet|52.224.0.0/11|Internet|
|mi-64-4-18-nexthop-internet|64.4.0.0/18|Internet|
|mi-65-52-14-nexthop-internet|65.52.0.0/14|Internet|
|mi-66-119-144-20-nexthop-internet|66.119.144.0/20|Internet|
|mi-70-37-17-nexthop-internet|70.37.0.0/17|Internet|
|mi-70-37-128-18-nexthop-internet|70.37.128.0/18|Internet|
|mi-91-190-216-21-nexthop-internet|91.190.216.0/21|Internet|
|mi-94-245-64-18-nexthop-internet|94.245.64.0/18|Internet|
|mi-103-9-8-22-nexthop-internet|103.9.8.0/22|Internet|
|mi-103-25-156-24-nexthop-internet|103.25.156.0/24|Internet|
|mi-103-25-157-24-nexthop-internet|103.25.157.0/24|Internet|
|mi-103-25-158-23-nexthop-internet|103.25.158.0/23|Internet|
|mi-103-36-96-22-nexthop-internet|103.36.96.0/22|Internet|
|mi-103-255-140-22-nexthop-internet|103.255.140.0/22|Internet|
|mi-104-40-13-nexthop-internet|104.40.0.0/13|Internet|
|mi-104-146-15-nexthop-internet|104.146.0.0/15|Internet|
|mi-104-208-13-nexthop-internet|104.208.0.0/13|Internet|
|mi-111-221-16-20-nexthop-internet|111.221.16.0/20|Internet|
|mi-111-221-64-18-nexthop-internet|111.221.64.0/18|Internet|
|mi-129-75-16-nexthop-internet|129.75.0.0/16|Internet|
|mi-131-107-16-nexthop-internet|131.107.0.0/16|Internet|
|mi-131-253-1-24-nexthop-internet|131.253.1.0/24|Internet|
|mi-131-253-3-24-nexthop-internet|131.253.3.0/24|Internet|
|mi-131-253-5-24-nexthop-internet|131.253.5.0/24|Internet|
|mi-131-253-6-24-nexthop-internet|131.253.6.0/24|Internet|
|mi-131-253-8-24-nexthop-internet|131.253.8.0/24|Internet|
|mi-131-253-12-22-nexthop-internet|131.253.12.0/22|Internet|
|mi-131-253-16-23-nexthop-internet|131.253.16.0/23|Internet|
|mi-131-253-18-24-nexthop-internet|131.253.18.0/24|Internet|
|mi-131-253-21-24-nexthop-internet|131.253.21.0/24|Internet|
|mi-131-253-22-23-nexthop-internet|131.253.22.0/23|Internet|
|mi-131-253-24-21-nexthop-internet|131.253.24.0/21|Internet|
|mi-131-253-32-20-nexthop-internet|131.253.32.0/20|Internet|
|mi-131-253-61-24-nexthop-internet|131.253.61.0/24|Internet|
|mi-131-253-62-23-nexthop-internet|131.253.62.0/23|Internet|
|mi-131-253-64-18-nexthop-internet|131.253.64.0/18|Internet|
|mi-131-253-128-17-nexthop-internet|131.253.128.0/17|Internet|
|mi-132-245-16-nexthop-internet|132.245.0.0/16|Internet|
|mi-134-170-16-nexthop-internet|134.170.0.0/16|Internet|
|mi-134-177-16-nexthop-internet|134.177.0.0/16|Internet|
|mi-137-116-15-nexthop-internet|137.116.0.0/15|Internet|
|mi-137-135-16-nexthop-internet|137.135.0.0/16|Internet|
|mi-138-91-16-nexthop-internet|138.91.0.0/16|Internet|
|mi-138-196-16-nexthop-internet|138.196.0.0/16|Internet|
|mi-139-217-16-nexthop-internet|139.217.0.0/16|Internet|
|mi-139-219-16-nexthop-internet|139.219.0.0/16|Internet|
|mi-141-251-16-nexthop-internet|141.251.0.0/16|Internet|
|mi-146-147-16-nexthop-internet|146.147.0.0/16|Internet|
|mi-147-243-16-nexthop-internet|147.243.0.0/16|Internet|
|mi-150-171-16-nexthop-internet|150.171.0.0/16|Internet|
|mi-150-242-48-22-nexthop-internet|150.242.48.0/22|Internet|
|mi-157-54-15-nexthop-internet|157.54.0.0/15|Internet|
|mi-157-56-14-nexthop-internet|157.56.0.0/14|Internet|
|mi-157-60-16-nexthop-internet|157.60.0.0/16|Internet|
|mi-167-105-16-nexthop-internet|167.105.0.0/16|Internet|
|mi-167-220-16-nexthop-internet|167.220.0.0/16|Internet|
|mi-168-61-16-nexthop-internet|168.61.0.0/16|Internet|
|mi-168-62-15-nexthop-internet|168.62.0.0/15|Internet|
|mi-191-232-13-nexthop-internet|191.232.0.0/13|Internet|
|mi-192-32-16-nexthop-internet|192.32.0.0/16|Internet|
|mi-192-48-225-24-nexthop-internet|192.48.225.0/24|Internet|
|mi-192-84-159-24-nexthop-internet|192.84.159.0/24|Internet|
|mi-192-84-160-23-nexthop-internet|192.84.160.0/23|Internet|
|mi-192-197-157-24-nexthop-internet|192.197.157.0/24|Internet|
|mi-193-149-64-19-nexthop-internet|193.149.64.0/19|Internet|
|mi-193-221-113-24-nexthop-internet|193.221.113.0/24|Internet|
|mi-194-69-96-19-nexthop-internet|194.69.96.0/19|Internet|
|mi-194-110-197-24-nexthop-internet|194.110.197.0/24|Internet|
|mi-198-105-232-22-nexthop-internet|198.105.232.0/22|Internet|
|mi-198-200-130-24-nexthop-internet|198.200.130.0/24|Internet|
|mi-198-206-164-24-nexthop-internet|198.206.164.0/24|Internet|
|mi-199-60-28-24-nexthop-internet|199.60.28.0/24|Internet|
|mi-199-74-210-24-nexthop-internet|199.74.210.0/24|Internet|
|mi-199-103-90-23-nexthop-internet|199.103.90.0/23|Internet|
|mi-199-103-122-24-nexthop-internet|199.103.122.0/24|Internet|
|mi-199-242-32-20-nexthop-internet|199.242.32.0/20|Internet|
|mi-199-242-48-21-nexthop-internet|199.242.48.0/21|Internet|
|mi-202-89-224-20-nexthop-internet|202.89.224.0/20|Internet|
|mi-204-13-120-21-nexthop-internet|204.13.120.0/21|Internet|
|mi-204-14-180-22-nexthop-internet|204.14.180.0/22|Internet|
|mi-204-79-135-24-nexthop-internet|204.79.135.0/24|Internet|
|mi-204-79-179-24-nexthop-internet|204.79.179.0/24|Internet|
|mi-204-79-181-24-nexthop-internet|204.79.181.0/24|Internet|
|mi-204-79-188-24-nexthop-internet|204.79.188.0/24|Internet|
|mi-204-79-195-24-nexthop-internet|204.79.195.0/24|Internet|
|mi-204-79-196-23-nexthop-internet|204.79.196.0/23|Internet|
|mi-204-79-252-24-nexthop-internet|204.79.252.0/24|Internet|
|mi-204-152-18-23-nexthop-internet|204.152.18.0/23|Internet|
|mi-204-152-140-23-nexthop-internet|204.152.140.0/23|Internet|
|mi-204-231-192-24-nexthop-internet|204.231.192.0/24|Internet|
|mi-204-231-194-23-nexthop-internet|204.231.194.0/23|Internet|
|mi-204-231-197-24-nexthop-internet|204.231.197.0/24|Internet|
|mi-204-231-198-23-nexthop-internet|204.231.198.0/23|Internet|
|mi-204-231-200-21-nexthop-internet|204.231.200.0/21|Internet|
|mi-204-231-208-20-nexthop-internet|204.231.208.0/20|Internet|
|mi-204-231-236-24-nexthop-internet|204.231.236.0/24|Internet|
|mi-205-174-224-20-nexthop-internet|205.174.224.0/20|Internet|
|mi-206-138-168-21-nexthop-internet|206.138.168.0/21|Internet|
|mi-206-191-224-19-nexthop-internet|206.191.224.0/19|Internet|
|mi-207-46-16-nexthop-internet|207.46.0.0/16|Internet|
|mi-207-68-128-18-nexthop-internet|207.68.128.0/18|Internet|
|mi-208-68-136-21-nexthop-internet|208.68.136.0/21|Internet|
|mi-208-76-44-22-nexthop-internet|208.76.44.0/22|Internet|
|mi-208-84-21-nexthop-internet|208.84.0.0/21|Internet|
|mi-209-240-192-19-nexthop-internet|209.240.192.0/19|Internet|
|mi-213-199-128-18-nexthop-internet|213.199.128.0/18|Internet|
|mi-216-32-180-22-nexthop-internet|216.32.180.0/22|Internet|
|mi-216-220-208-20-nexthop-internet|216.220.208.0/20|Internet|
|mi-23-96-13-nexthop-internet|23.96.0.0/13|Internet|
|mi-42-159-16-nexthop-internet|42.159.0.0/16|Internet|
|mi-51-13-17-nexthop-internet|51.13.0.0/17|Internet|
|mi-51-107-16-nexthop-internet|51.107.0.0/16|Internet|
|mi-51-116-16-nexthop-internet|51.116.0.0/16|Internet|
|mi-51-120-16-nexthop-internet|51.120.0.0/16|Internet|
|mi-51-120-128-17-nexthop-internet|51.120.128.0/17|Internet|
|mi-51-124-16-nexthop-internet|51.124.0.0/16|Internet|
|mi-102-37-18-nexthop-internet|102.37.0.0/18|Internet|
|mi-102-133-16-nexthop-internet|102.133.0.0/16|Internet|
|mi-199-30-16-20-nexthop-internet|199.30.16.0/20|Internet|
|mi-204-79-180-24-nexthop-internet|204.79.180.0/24|Internet|
||||

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için, [SQL Veritabanı gelişmiş veri güvenliği](sql-database-managed-instance.md)bakın.
- Yönetilen örnekleri dağıtabileceğiniz yeni bir Azure sanal ağı veya mevcut bir [Azure sanal ağı](sql-database-managed-instance-configure-vnet-subnet.md) nasıl [kuracağınızı](sql-database-managed-instance-create-vnet-subnet.md) öğrenin.
- Yönetilen örnekleri dağıtmak istediğiniz [alt ağın boyutunu hesaplayın.](sql-database-managed-instance-determine-size-vnet-subnet.md)
- Yönetilen bir örneği nasıl oluşturabilirsiniz öğrenin:
  - Azure [portalından.](sql-database-managed-instance-get-started.md)
  - [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)kullanarak .
  - Azure [Kaynak Yöneticisi şablonu](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)kullanarak.
  - Azure [Kaynak Yöneticisi şablonu kullanarak (JumpBox'ı kullanarak, SSMS dahil)](https://azure.microsoft.com/resources/templates/201-sqlmi-new-vnet-w-jumpbox/). 
