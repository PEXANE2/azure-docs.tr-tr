---
title: Yönetilen örnek için bağlantı mimarisi
description: Azure SQL veritabanı yönetilen örneği iletişimi ve bağlantı mimarisi hakkında bilgi edinin ve bileşenlerin yönetilen örneğe trafiği nasıl yönlendirmiş olduğunu öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: fasttrack-edit
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: 9f341c3c2c299ca358b2a42210f04c6399fe2892
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773611"
---
# <a name="connectivity-architecture-for-a-managed-instance-in-azure-sql-database"></a>Azure SQL veritabanı 'nda yönetilen bir örnek için bağlantı mimarisi

Bu makalede, Azure SQL veritabanı yönetilen örneğinde iletişim açıklanmaktadır. Ayrıca, bağlantı mimarisini ve bileşenlerin yönetilen örneğe trafiği nasıl yönlendirdiğinden de açıklanmaktadır.  

SQL veritabanı yönetilen örneği, Azure sanal ağının içine ve yönetilen örneklere ayrılmış alt ağa yerleştirilir. Bu dağıtım şunları sağlar:

- Güvenli bir özel IP adresi.
- Şirket içi bir ağı yönetilen bir örneğe bağlama özelliği.
- Yönetilen bir örneği bağlantılı sunucuya veya başka bir şirket içi veri deposuna bağlama özelliği.
- Yönetilen bir örneği Azure kaynaklarına bağlama özelliği.

## <a name="communication-overview"></a>İletişime genel bakış

Aşağıdaki diyagramda, yönetilen bir örneğe bağlanan varlıklar gösterilmektedir. Ayrıca, yönetilen örnekle iletişim kurması gereken kaynakları gösterir. Diyagramın en altındaki iletişim işlemi, yönetilen örneğe veri kaynakları olarak bağlanan müşteri uygulamalarını ve araçları temsil eder.  

![Bağlantı mimarisi varlıkları](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Yönetilen örnek, hizmet olarak platform (PaaS) teklifi. Microsoft, bu hizmeti telemetri veri akışlarına göre yönetmek için otomatik aracıları (Yönetim, dağıtım ve bakım) kullanır. Microsoft yönetiminden sorumlu olduğundan, müşteriler yönetilen örnek sanal küme makinelerine Uzak Masaüstü Protokolü (RDP) üzerinden erişemez.

Son kullanıcılar veya uygulamalar tarafından başlatılan bazı SQL Server işlemler, platformla etkileşimde bulunmak için yönetilen örnekler gerektirebilir. Bir durum, yönetilen örnek veritabanının oluşturulması olur. Bu kaynak Azure portal, PowerShell, Azure CLı ve REST API aracılığıyla sunulur.

Yönetilen örnekler, yedeklemeler için Azure depolama, telemetri için Azure Event Hubs, kimlik doğrulaması için Azure Active Directory Azure Key Vault Saydam Veri Şifrelemesi (TDE) ve güvenlik ve desteklenebilirlik özellikleri sağlayan birkaç Azure platform hizmeti gibi Azure hizmetlerine bağımlıdır. Yönetilen örnekler bu hizmetlere bağlantılar oluşturur.

Tüm iletişimler şifrelenir ve sertifikalar kullanılarak imzalanır. İletişim kuran tarafların güvenilirliğini denetlemek için, yönetilen örnekler sertifika iptal listeleri aracılığıyla bu sertifikaları sürekli olarak doğrular. Sertifikalar iptal edildiğinde, yönetilen örnek, verileri korumak için bağlantıları kapatır.

## <a name="high-level-connectivity-architecture"></a>Üst düzey bağlantı mimarisi

Yüksek düzeyde, yönetilen bir örnek bir hizmet bileşenleri kümesidir. Bu bileşenler, müşterinin sanal ağ alt ağı içinde çalışan ayrılmış bir yalıtılmış sanal makine kümesinde barındırılır. Bu makineler bir sanal küme oluşturur.

Bir sanal küme birden çok yönetilen örneği barındırabilirler. Gerekirse, müşteri alt ağdaki sağlanan örneklerin sayısını değiştirdiğinde küme otomatik olarak genişletilir veya anlaşmaları yapılır.

Müşteri uygulamaları yönetilen örneklere bağlanabilir ve sanal ağ, eşlenen sanal ağ veya VPN ya da Azure ExpressRoute ile bağlı ağ içindeki veritabanlarını sorgulayabilir ve güncelleştirebilir. Bu ağ bir uç nokta ve özel bir IP adresi kullanmalıdır.  

![Bağlantı mimarisi diyagramı](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Microsoft Yönetim ve Dağıtım Hizmetleri sanal ağ dışında çalışır. Yönetilen bir örnek ve Microsoft Hizmetleri, genel IP adreslerine sahip uç noktalara bağlanır. Yönetilen bir örnek bir giden bağlantı oluşturduğunda, son ağ adresi çevirisi (NAT), bağlantının bu genel IP adresinden geldiği gibi görünmesine neden olur.

Yönetim trafiği müşterinin sanal ağı üzerinden akar. Diğer bir deyişle, sanal ağın altyapısının öğeleri, örnek başarısız olur ve kullanılamaz hale getirerek Yönetim trafiğine zarar verebilir.

> [!IMPORTANT]
> Microsoft, müşteri deneyimini ve hizmet kullanılabilirliğini geliştirmek için Azure sanal ağ altyapısı öğelerinde bir ağ hedefi ilkesi uygular. İlke, yönetilen örneğin nasıl çalıştığını etkileyebilir. Bu platform mekanizması, ağ gereksinimlerini kullanıcılara saydam şekilde iletir. İlkenin ana hedefi, ağ yanlış yapılandırılmasını önlemektir ve normal yönetilen örnek işlemlerini sağlamaktır. Yönetilen bir örneği sildiğinizde, ağ hedefi ilkesi de kaldırılır.

## <a name="virtual-cluster-connectivity-architecture"></a>Sanal küme bağlantısı mimarisi

Yönetilen örnekler için bağlantı mimarisine daha ayrıntılı bir şekilde bakalım. Aşağıdaki diyagramda, sanal kümenin kavramsal düzeni gösterilmektedir.

![Sanal kümenin bağlantı mimarisi](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

İstemciler, form içeren bir ana bilgisayar adı kullanarak yönetilen bir örneğe bağlanır `<mi_name>.<dns_zone>.database.windows.net` . Bu ana bilgisayar adı, ortak bir etki alanı adı sistemi (DNS) bölgesinde kayıtlı ve genel olarak çözümlenebildiği halde özel bir IP adresi olarak çözümlenir. , `zone-id` Kümeyi oluşturduğunuzda otomatik olarak oluşturulur. Yeni oluşturulan bir küme, ikincil yönetilen bir örnek barındırıyorsa, birincil kümeyle bölge KIMLIĞINI paylaşır. Daha fazla bilgi için bkz. [otomatik yük devretme gruplarını kullanarak birden çok veritabanının saydam ve eşgüdümlü yük devretmesini etkinleştirme](sql-database-auto-failover-group.md#enabling-geo-replication-between-managed-instances-and-their-vnets).

Bu özel IP adresi, yönetilen örneğin iç yük dengeleyicisine aittir. Yük dengeleyici, trafiği yönetilen örneğin ağ geçidine yönlendirir. Aynı küme içinde birden çok yönetilen örnek çalıştırılabildiğinden ağ geçidi, trafiği doğru SQL altyapısı hizmetine yönlendirmek için yönetilen örneğin ana bilgisayar adını kullanır.

Yönetim ve Dağıtım Hizmetleri, bir dış yük dengeleyiciye eşlenen bir [Yönetim uç noktası](#management-endpoint) kullanarak yönetilen örneğe bağlanır. Trafik yalnızca yönetilen örneğin yönetim bileşenlerinin kullandığı önceden tanımlanmış bir bağlantı noktası kümesi üzerinde alındığında düğümlere yönlendirilir. Düğümlerdeki yerleşik bir güvenlik duvarı yalnızca Microsoft IP aralıklarından gelen trafiğe izin verecek şekilde ayarlanmıştır. Sertifikalar, Yönetim bileşenleri ile yönetim düzlemi arasındaki iletişimin hepsini karşılıklı olarak doğrular.

## <a name="management-endpoint"></a>Yönetim uç noktası

Microsoft, yönetilen örneği bir yönetim uç noktası kullanarak yönetir. Bu uç nokta, örneğin sanal kümesinin içindedir. Yönetim uç noktası, ağ düzeyindeki yerleşik bir güvenlik duvarı tarafından korunur. Uygulama düzeyinde, karşılıklı sertifika doğrulaması tarafından korunur. Uç noktanın IP adresini bulmak için, bkz. [Yönetim uç NOKTASıNıN IP adresini belirleme](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Bağlantılar yönetilen örnek içinde (yedeklemeler ve denetim günlükleri gibi) başlatıldığında trafik yönetim uç noktasının genel IP adresinden başlar. Yalnızca yönetilen örneğin IP adresine izin vermek için güvenlik duvarı kuralları ayarlayarak, yönetilen bir örnekten ortak hizmetlere erişimi sınırlayabilirsiniz. Daha fazla bilgi için bkz. [yönetilen örneğin yerleşik güvenlik duvarını doğrulama](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Yönetilen örnek bölgesi içindeki Azure hizmetlerine giden trafik en iyi duruma getirilir ve bu nedenle yönetilen örnek yönetimi uç noktası genel IP adresi olarak kullanılamaz. Bu nedenle, depolama için en yaygın olarak depolama için IP tabanlı güvenlik duvarı kuralları kullanmanız gerekirse, hizmetin yönetilen örnekten farklı bir bölgede olması gerekir.

## <a name="service-aided-subnet-configuration"></a>Hizmet destekli alt ağ yapılandırması

Yönetilen Örnek, müşteri güvenlik ve yönetilebilirlik gereksinimlerini karşılamak için el ile yapılandırmadan hizmet destekli alt ağ yapılandırmasına geçmektedir.

Hizmet destekli alt ağ yapılandırması sayesinde kullanıcı veri (TDS) trafiği üzerinde tam denetim sahibi olurken yönetim trafiğinin SLA'ya uygun şekilde kesintisiz bir şekilde yönetilmesi Yönetilen Örneğin sorumluluğunda olur.

Hizmet destekli alt ağ yapılandırması, otomatik ağ yapılandırması yönetimi sağlamak ve hizmet uç noktalarını etkinleştirmek için sanal ağ [alt ağ temsili](../virtual-network/subnet-delegation-overview.md) özelliğinden faydalanır. Hizmet uç noktaları, yedeklemeleri/denetim günlüklerini tutan depolama hesaplarında sanal ağ güvenlik duvarı kuralları yapılandırmak için kullanılabilir.

### <a name="network-requirements"></a>Ağ gereksinimleri 

Yönetilen örneği sanal ağ içindeki ayrılmış bir alt ağa dağıtın. Alt ağ şu özelliklere sahip olmalıdır:

- **Ayrılmış alt ağ:** Yönetilen örneğin alt ağında kendisiyle ilişkili başka bir bulut hizmeti bulunamaz ve ağ geçidi alt ağı kullanılamaz. Alt ağda yönetilen örnek dışında başka bir kaynak bulunamaz ve sonrasında alt ağa farklı türde kaynaklar eklenemez.
- **Alt ağ temsili:** Yönetilen örneğin alt ağının, kaynak sağlayıcısına atanmış olması gerekir `Microsoft.Sql/managedInstances` .
- **Ağ güvenlik grubu (NSG):** Yönetilen örneğin alt ağıyla bir NSG ilişkilendirilmelidir. NSG'yi kullanarak 1433 numaralı bağlantı noktasındaki trafiği filtreleyebilir ve yönetilen örneğin veri uç noktasına erişimi denetleyebilir veya yönetilen örneğin yeniden yönlendirme bağlantıları için yapılandırılmış olması durumunda 11000-11999 arası bağlantı noktalarını kullanabilirsiniz. Hizmet, kesintisiz yönetim trafiği akışına izin vermek için gerekli olan [kuralları](#mandatory-inbound-security-rules-with-service-aided-subnet-configuration) otomatik olarak sağlayacak ve güncel tutacaktır.
- **Kullanıcı tanımlı yol (UDR) tablosu:** Yönetilen örneğin alt ağıyla bir UDR tablosu ilişkilendirilmelidir. Şirket içi özel IP aralıklarına sahip trafiği, sanal ağ geçidi veya sanal ağ gereci (NVA) aracılığıyla bir hedef olarak yönlendirmek için yol tablosuna giriş ekleyebilirsiniz. Hizmet, kesintisiz yönetim trafiği akışına izin vermek için gerekli olan [girişleri](#user-defined-routes-with-service-aided-subnet-configuration) otomatik olarak sağlayacak ve güncel tutacaktır.
- **Yeterli sayıda IP adresi:** Yönetilen örnek alt ağı en az 16 IP adresine sahip olmalıdır. Önerilen minimum değer 32 IP adresidir. Daha fazla bilgi için bkz. [Yönetilen örnek alt ağının boyutunu belirleme](sql-database-managed-instance-determine-size-vnet-subnet.md). Yönetilen örnekleri [var olan ağa](sql-database-managed-instance-configure-vnet-subnet.md) dağıtmak için [yönetilen örnekler için ağ gereksinimlerini](#network-requirements) karşılayacak şekilde yapılandırmanız gerekir. Bunu yapamazsanız [yeni bir ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md) oluşturabilirsiniz.

> [!IMPORTANT]
> Yönetilen bir örnek oluşturduğunuzda, ağ kurulum üzerinde uyumsuz değişiklikler yapılmasını engellemek için alt ağa bir ağ hedefi ilkesi uygulanır. Son örnek alt ağdan kaldırıldıktan sonra, ağ hedefi ilkesi de kaldırılır.

### <a name="mandatory-inbound-security-rules-with-service-aided-subnet-configuration"></a>Hizmet destekli alt ağ yapılandırması ile zorunlu gelen güvenlik kuralları 

| Name       |Bağlantı noktası                        |Protokol|Kaynak           |Hedef|Eylem|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |SqlManagement    |Mı ALT AĞı  |İzin Ver |
|            |9000, 9003                  |TCP     |Corpnetgördünüz       |Mı ALT AĞı  |İzin Ver |
|            |9000, 9003                  |TCP     |CorpnetPublic    |Mı ALT AĞı  |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |Mı ALT AĞı        |Mı ALT AĞı  |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|Mı ALT AĞı  |İzin Ver |

### <a name="mandatory-outbound-security-rules-with-service-aided-subnet-configuration"></a>Hizmet destekli alt ağ yapılandırması ile zorunlu giden güvenlik kuralları 

| Name       |Bağlantı noktası          |Protokol|Kaynak           |Hedef|Eylem|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |443, 12000    |TCP     |Mı ALT AĞı        |AzureCloud |İzin Ver |
|mi_subnet   |Herhangi biri           |Herhangi biri     |Mı ALT AĞı        |Mı ALT AĞı  |İzin Ver |

### <a name="user-defined-routes-with-service-aided-subnet-configuration"></a>Hizmet destekli alt ağ yapılandırmasıyla Kullanıcı tanımlı rotalar 

|Name|Adres ön eki|Sonraki atlama|
|----|--------------|-------|
|alt ağdan vnetlocal|Mı ALT AĞı|Sanal ağ|
|mi-13-64-11-sonrakii-Internet|13.64.0.0/11|Internet|
|mi-13-104-14-sonrakii-Internet|13.104.0.0/14|Internet|
|mi-20-33-16-sonrakii-Internet|20.33.0.0/16|Internet|
|mi-20-34-15-sonrakii-Internet|20.34.0.0/15|Internet|
|mi-20-36-14-sonrakii-Internet|20.36.0.0/14|Internet|
|mi-20-40-13-sonrakii-Internet|20.40.0.0/13|Internet|
|mi-20-48-12-sonrakii-Internet|20.48.0.0/12|Internet|
|mi-20-64-10-sonrakii-Internet|20.64.0.0/10|Internet|
|mi-20-128-16-sonrakii-Internet|20.128.0.0/16|Internet|
|mi-20-135-16-sonrakii-Internet|20.135.0.0/16|Internet|
|mi-20-136-16-sonrakii-Internet|20.136.0.0/16|Internet|
|mi-20-140-15-sonrakii-Internet|20.140.0.0/15|Internet|
|mi-20-143-16-sonrakii-Internet|20.143.0.0/16|Internet|
|mi-20-144-14-sonrakii-Internet|20.144.0.0/14|Internet|
|mi-20-150-15-sonrakii-Internet|20.150.0.0/15|Internet|
|mi-20-160-12-sonrakii-Internet|20.160.0.0/12|Internet|
|mi-20-176-14-sonrakii-Internet|20.176.0.0/14|Internet|
|mi-20-180-14-sonrakii-Internet|20.180.0.0/14|Internet|
|mi-20-184-13-sonrakii-Internet|20.184.0.0/13|Internet|
|mi-20-192-10-sonrakii-Internet|20.192.0.0/10|Internet|
|mi-40-64-10-sonrakii-Internet|40.64.0.0/10|Internet|
|mi-51-4-15-sonrakii-Internet|51.4.0.0/15|Internet|
|mi-51-8-16-sonrakii-Internet|51.8.0.0/16|Internet|
|mi-51-10-15-sonrakii-Internet|51.10.0.0/15|Internet|
|mi-51-18-16-sonrakii-Internet|51.18.0.0/16|Internet|
|mi-51-51-16-sonrakii-Internet|51.51.0.0/16|Internet|
|mi-51-53-16-sonrakii-Internet|51.53.0.0/16|Internet|
|mi-51-103-16-sonrakii-Internet|51.103.0.0/16|Internet|
|mi-51-104-15-sonrakii-Internet|51.104.0.0/15|Internet|
|mi-51-132-16-sonrakii-Internet|51.132.0.0/16|Internet|
|mi-51-136-15-sonrakii-Internet|51.136.0.0/15|Internet|
|mi-51-138-16-sonrakii-Internet|51.138.0.0/16|Internet|
|mi-51-140-14-sonrakii-Internet|51.140.0.0/14|Internet|
|mi-51-144-15-sonrakii-Internet|51.144.0.0/15|Internet|
|mi-52-96-12-sonrakii-Internet|52.96.0.0/12|Internet|
|mi-52-112-14-sonrakii-Internet|52.112.0.0/14|Internet|
|mi-52-125-16-sonrakii-Internet|52.125.0.0/16|Internet|
|mi-52-126-15-sonrakii-Internet|52.126.0.0/15|Internet|
|mi-52-130-15-sonrakii-Internet|52.130.0.0/15|Internet|
|mi-52-132-14-sonrakii-Internet|52.132.0.0/14|Internet|
|mi-52-136-13-sonrakii-Internet|52.136.0.0/13|Internet|
|mi-52-145-16-sonrakii-Internet|52.145.0.0/16|Internet|
|mi-52-146-15-sonrakii-Internet|52.146.0.0/15|Internet|
|mi-52-148-14-sonrakii-Internet|52.148.0.0/14|Internet|
|mi-52-152-13-sonrakii-Internet|52.152.0.0/13|Internet|
|mi-52-160-11-sonrakii-Internet|52.160.0.0/11|Internet|
|mi-52-224-11-sonrakii-Internet|52.224.0.0/11|Internet|
|mi-64-4-18-sonrakii-Internet|64.4.0.0/18|Internet|
|mi-65-52-14-sonrakii-Internet|65.52.0.0/14|Internet|
|mi-66-119-144-20-sonrakii-Internet|66.119.144.0/20|Internet|
|mi-70-37-17-sonrakii-Internet|70.37.0.0/17|Internet|
|mi-70-37-128-18-sonrakii-Internet|70.37.128.0/18|Internet|
|mi-91-190-216-21-sonrakii-Internet|91.190.216.0/21|Internet|
|mi-94-245-64-18-sonrakii-Internet|94.245.64.0/18|Internet|
|mi-103-9-8-22-sonrakii-Internet|103.9.8.0/22|Internet|
|mi-103-25-156-24-sonrakii-Internet|103.25.156.0/24|Internet|
|mi-103-25-157-24-sonrakii-Internet|103.25.157.0/24|Internet|
|mi-103-25-158-23-sonrakii-Internet|103.25.158.0/23|Internet|
|mi-103-36-96-22-sonrakii-Internet|103.36.96.0/22|Internet|
|mi-103-255-140-22-sonrakii-Internet|103.255.140.0/22|Internet|
|mi-104-40-13-sonrakii-Internet|104.40.0.0/13|Internet|
|mi-104-146-15-sonrakii-Internet|104.146.0.0/15|Internet|
|mi-104-208-13-sonrakii-Internet|104.208.0.0/13|Internet|
|mi-111-221-16-20-sonrakii-Internet|111.221.16.0/20|Internet|
|mi-111-221-64-18-sonrakii-Internet|111.221.64.0/18|Internet|
|mi-129-75-16-sonrakii-Internet|129.75.0.0/16|Internet|
|mi-131-107-16-sonrakii-Internet|131.107.0.0/16|Internet|
|mi-131-253-1-24-sonrakii-Internet|131.253.1.0/24|Internet|
|mi-131-253-3-24-sonrakii-Internet|131.253.3.0/24|Internet|
|mi-131-253-5-24-sonrakii-Internet|131.253.5.0/24|Internet|
|mi-131-253-6-24-sonrakii-Internet|131.253.6.0/24|Internet|
|mi-131-253-8-24-sonrakii-Internet|131.253.8.0/24|Internet|
|mi-131-253-12-22-sonrakii-Internet|131.253.12.0/22|Internet|
|mi-131-253-16-23-sonrakii-Internet|131.253.16.0/23|Internet|
|mi-131-253-18-24-sonrakii-Internet|131.253.18.0/24|Internet|
|mi-131-253-21-24-sonrakii-Internet|131.253.21.0/24|Internet|
|mi-131-253-22-23-sonrakii-Internet|131.253.22.0/23|Internet|
|mi-131-253-24-21-sonrakii-Internet|131.253.24.0/21|Internet|
|mi-131-253-32-20-sonrakii-Internet|131.253.32.0/20|Internet|
|mi-131-253-61-24-sonrakii-Internet|131.253.61.0/24|Internet|
|mi-131-253-62-23-sonrakii-Internet|131.253.62.0/23|Internet|
|mi-131-253-64-18-sonrakii-Internet|131.253.64.0/18|Internet|
|mi-131-253-128-17-sonrakii-Internet|131.253.128.0/17|Internet|
|mi-132-245-16-sonrakii-Internet|132.245.0.0/16|Internet|
|mi-134-170-16-sonrakii-Internet|134.170.0.0/16|Internet|
|mi-134-177-16-sonrakii-Internet|134.177.0.0/16|Internet|
|mi-137-116-15-sonrakii-Internet|137.116.0.0/15|Internet|
|mi-137-135-16-sonrakii-Internet|137.135.0.0/16|Internet|
|mi-138-91-16-sonrakii-Internet|138.91.0.0/16|Internet|
|mi-138-196-16-sonrakii-Internet|138.196.0.0/16|Internet|
|mi-139-217-16-sonrakii-Internet|139.217.0.0/16|Internet|
|mi-139-219-16-sonrakii-Internet|139.219.0.0/16|Internet|
|mi-141-251-16-sonrakii-Internet|141.251.0.0/16|Internet|
|mi-146-147-16-sonrakii-Internet|146.147.0.0/16|Internet|
|mi-147-243-16-sonrakii-Internet|147.243.0.0/16|Internet|
|mi-150-171-16-sonrakii-Internet|150.171.0.0/16|Internet|
|mi-150-242-48-22-sonrakii-Internet|150.242.48.0/22|Internet|
|mi-157-54-15-sonrakii-Internet|157.54.0.0/15|Internet|
|mi-157-56-14-sonrakii-Internet|157.56.0.0/14|Internet|
|mi-157-60-16-sonrakii-Internet|157.60.0.0/16|Internet|
|mi-167-105-16-sonrakii-Internet|167.105.0.0/16|Internet|
|mi-167-220-16-sonrakii-Internet|167.220.0.0/16|Internet|
|mi-168-61-16-sonrakii-Internet|168.61.0.0/16|Internet|
|mi-168-62-15-sonrakii-Internet|168.62.0.0/15|Internet|
|mi-191-232-13-sonrakii-Internet|191.232.0.0/13|Internet|
|mi-192-32-16-sonrakii-Internet|192.32.0.0/16|Internet|
|mi-192-48-225-24-sonrakii-Internet|192.48.225.0/24|Internet|
|mi-192-84-159-24-sonrakii-Internet|192.84.159.0/24|Internet|
|mi-192-84-160-23-sonrakii-Internet|192.84.160.0/23|Internet|
|mi-192-197-157-24-sonrakii-Internet|192.197.157.0/24|Internet|
|mi-193-149-64-19-sonrakii-Internet|193.149.64.0/19|Internet|
|mi-193-221-113-24-sonrakii-Internet|193.221.113.0/24|Internet|
|mi-194-69-96-19-sonrakii-Internet|194.69.96.0/19|Internet|
|mi-194-110-197-24-sonrakii-Internet|194.110.197.0/24|Internet|
|mi-198-105-232-22-sonrakii-Internet|198.105.232.0/22|Internet|
|mi-198-200-130-24-sonrakii-Internet|198.200.130.0/24|Internet|
|mi-198-206-164-24-sonrakii-Internet|198.206.164.0/24|Internet|
|mi-199-60-28-24-sonrakii-Internet|199.60.28.0/24|Internet|
|mi-199-74-210-24-sonrakii-Internet|199.74.210.0/24|Internet|
|mi-199-103-90-23-sonrakii-Internet|199.103.90.0/23|Internet|
|mi-199-103-122-24-sonrakii-Internet|199.103.122.0/24|Internet|
|mi-199-242-32-20-sonrakii-Internet|199.242.32.0/20|Internet|
|mi-199-242-48-21-sonrakii-Internet|199.242.48.0/21|Internet|
|mi-202-89-224-20-sonrakii-Internet|202.89.224.0/20|Internet|
|mi-204-13-120-21-sonrakii-Internet|204.13.120.0/21|Internet|
|mi-204-14-180-22-sonrakii-Internet|204.14.180.0/22|Internet|
|mi-204-79-135-24-sonrakii-Internet|204.79.135.0/24|Internet|
|mi-204-79-179-24-sonrakii-Internet|204.79.179.0/24|Internet|
|mi-204-79-181-24-sonrakii-Internet|204.79.181.0/24|Internet|
|mi-204-79-188-24-sonrakii-Internet|204.79.188.0/24|Internet|
|mi-204-79-195-24-sonrakii-Internet|204.79.195.0/24|Internet|
|mi-204-79-196-23-sonrakii-Internet|204.79.196.0/23|Internet|
|mi-204-79-252-24-sonrakii-Internet|204.79.252.0/24|Internet|
|mi-204-152-18-23-sonrakii-Internet|204.152.18.0/23|Internet|
|mi-204-152-140-23-sonrakii-Internet|204.152.140.0/23|Internet|
|mi-204-231-192-24-sonrakii-Internet|204.231.192.0/24|Internet|
|mi-204-231-194-23-sonrakii-Internet|204.231.194.0/23|Internet|
|mi-204-231-197-24-sonrakii-Internet|204.231.197.0/24|Internet|
|mi-204-231-198-23-sonrakii-Internet|204.231.198.0/23|Internet|
|mi-204-231-200-21-sonrakii-Internet|204.231.200.0/21|Internet|
|mi-204-231-208-20-sonrakii-Internet|204.231.208.0/20|Internet|
|mi-204-231-236-24-sonrakii-Internet|204.231.236.0/24|Internet|
|mi-205-174-224-20-sonrakii-Internet|205.174.224.0/20|Internet|
|mi-206-138-168-21-sonrakii-Internet|206.138.168.0/21|Internet|
|mi-206-191-224-19-sonrakii-Internet|206.191.224.0/19|Internet|
|mi-207-46-16-sonrakii-Internet|207.46.0.0/16|Internet|
|mi-207-68-128-18-sonrakii-Internet|207.68.128.0/18|Internet|
|mi-208-68-136-21-sonrakii-Internet|208.68.136.0/21|Internet|
|mi-208-76-44-22-sonrakii-Internet|208.76.44.0/22|Internet|
|mi-208-84-21-sonrakii-Internet|208.84.0.0/21|Internet|
|mi-209-240-192-19-sonrakii-Internet|209.240.192.0/19|Internet|
|mi-213-199-128-18-sonrakii-Internet|213.199.128.0/18|Internet|
|mi-216-32-180-22-sonrakii-Internet|216.32.180.0/22|Internet|
|mi-216-220-208-20-sonrakii-Internet|216.220.208.0/20|Internet|
|mi-23-96-13-sonrakii-Internet|23.96.0.0/13|Internet|
|mi-42-159-16-sonrakii-Internet|42.159.0.0/16|Internet|
|mi-51-13-17-sonrakii-Internet|51.13.0.0/17|Internet|
|mi-51-107-16-sonrakii-Internet|51.107.0.0/16|Internet|
|mi-51-116-16-sonrakii-Internet|51.116.0.0/16|Internet|
|mi-51-120-16-sonrakii-Internet|51.120.0.0/16|Internet|
|mi-51-120-128-17-sonrakii-Internet|51.120.128.0/17|Internet|
|mi-51-124-16-sonrakii-Internet|51.124.0.0/16|Internet|
|mi-102-37-18-sonrakii-Internet|102.37.0.0/18|Internet|
|mi-102-133-16-sonrakii-Internet|102.133.0.0/16|Internet|
|mi-199-30-16-20-sonrakii-Internet|199.30.16.0/20|Internet|
|mi-204-79-180-24-sonrakii-Internet|204.79.180.0/24|Internet|
||||

\*Mı alt ağı, x. x. x. x/y biçimindeki alt ağın IP adresi aralığını ifade eder. Bu bilgileri, Azure portal alt ağ özelliklerinde bulabilirsiniz.

Ayrıca, sanal ağ geçidi veya sanal ağ gereci (NVA) aracılığıyla şirket içi özel IP aralıklarına sahip trafiği bir hedef olarak yönlendirmek için yol tablosuna giriş ekleyebilirsiniz.

Sanal ağ özel bir DNS içeriyorsa, özel DNS sunucusunun ortak DNS kayıtlarını çözümleyebilmesi gerekir. Azure AD kimlik doğrulaması gibi ek özelliklerin kullanılması ek FQDN 'lerin çözümlenmesini gerektirebilir. Daha fazla bilgi için bkz. [özel DNS ayarlama](sql-database-managed-instance-custom-dns.md).

### <a name="networking-constraints"></a>Ağ kısıtlamaları

**TLS 1,2, giden bağlantılarda zorlanır**: Microsoft 2020 ' de, tüm Azure hizmetlerinde hizmet içi trafik için MICROSOFT için TLS 1,2. Azure SQL veritabanı yönetilen örneği için, SQL Server, çoğaltma ve bağlı sunucu bağlantıları için kullanılan giden bağlantılarda TLS 1,2 ile sonuçlandı. Yönetilen örnek ile 2016 daha eski SQL Server sürümlerini kullanıyorsanız, lütfen [TLS 1,2 'e özgü güncelleştirmelerin](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server) uygulandığından emin olun.

Aşağıdaki sanal ağ özellikleri şu anda yönetilen örnekle desteklenmiyor:
- **Microsoft eşlemesi**: Express rota devrelerinin üzerinde [Microsoft eşlemesini](../expressroute/expressroute-faqs.md#microsoft-peering) etkinleştirme, yönetilen örneğin bulunduğu sanal ağla doğrudan veya geçişli olarak etkinleştiriliyor, sanal ağ ve hizmet içindeki yönetilen örnek bileşenleri arasındaki trafik akışını etkiler ve bu da kullanılabilirlik sorunlarına neden olur. Microsoft eşlemesi zaten etkin olan sanal ağa yönelik yönetilen örnek dağıtımların başarısız olması beklenir.
- **Küresel sanal ağ eşlemesi**: Azure bölgelerinde [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md) bağlantısı, [belgelenen yük dengeleyici kısıtlamaları](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)nedeniyle yönetilen örnek için çalışmıyor.
- **AzurePlatformDNS**: platform DNS çözümlemesini engellemek için AzurePlatformDNS [Service etiketinin](../virtual-network/service-tags-overview.md) kullanılması yönetilen örneği kullanılamaz hale işleyebilir. Yönetilen örnek, altyapı içinde DNS çözümlemesi için müşteri tanımlı DNS 'i desteklese de platform için platform DNS işlemleri için bir bağımlılık vardır.
- **NAT ağ geçidi**: belırlı genel IP adresiyle giden bağlantıyı denetlemek Için [sanal ağ NAT](../virtual-network/nat-overview.md) kullanmak yönetilen örnek için kullanılamaz olarak işlenir. Yönetilen örnek hizmeti şu anda, sanal ağ NAT ile gelen ve giden akışların işbiresine sahip olmayan temel yük dengeleyicinin kullanımıyla sınırlıdır.

### <a name="deprecated-network-requirements-without-service-aided-subnet-configuration"></a>Kullanım dışı Hizmet destekli alt ağ yapılandırması olmayan ağ gereksinimleri

Yönetilen örneği sanal ağ içindeki ayrılmış bir alt ağa dağıtın. Alt ağ şu özelliklere sahip olmalıdır:

- **Ayrılmış alt ağ:** Yönetilen örneğin alt ağında kendisiyle ilişkili başka bir bulut hizmeti bulunamaz ve ağ geçidi alt ağı kullanılamaz. Alt ağda yönetilen örnek dışında başka bir kaynak bulunamaz ve sonrasında alt ağa farklı türde kaynaklar eklenemez.
- **Ağ güvenlik grubu (NSG):** Sanal ağla ilişkili bir NSG 'nin, diğer kurallardan önce [gelen güvenlik kurallarını](#mandatory-inbound-security-rules) ve [giden güvenlik kurallarını](#mandatory-outbound-security-rules) tanımlamanız gerekir. NSG'yi kullanarak 1433 numaralı bağlantı noktasındaki trafiği filtreleyebilir ve yönetilen örneğin veri uç noktasına erişimi denetleyebilir veya yönetilen örneğin yeniden yönlendirme bağlantıları için yapılandırılmış olması durumunda 11000-11999 arası bağlantı noktalarını kullanabilirsiniz.
- **Kullanıcı tanımlı yol (UDR) tablosu:** Sanal ağla ilişkili bir UDR tablosu belirli [girdileri](#user-defined-routes)içermelidir.
- **Hizmet uç noktası yok:** Yönetilen örnek alt ağıyla ilişkili hizmet uç noktası yok. Sanal ağı oluştururken hizmet uç noktaları seçeneğinin devre dışı olduğundan emin olun.
- **Yeterli sayıda IP adresi:** Yönetilen örnek alt ağı en az 16 IP adresine sahip olmalıdır. Önerilen minimum değer 32 IP adresidir. Daha fazla bilgi için bkz. [Yönetilen örnek alt ağının boyutunu belirleme](sql-database-managed-instance-determine-size-vnet-subnet.md). Yönetilen örnekleri [var olan ağa](sql-database-managed-instance-configure-vnet-subnet.md) dağıtmak için [yönetilen örnekler için ağ gereksinimlerini](#network-requirements) karşılayacak şekilde yapılandırmanız gerekir. Bunu yapamazsanız [yeni bir ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md) oluşturabilirsiniz.

> [!IMPORTANT]
> Hedef alt ağda bu özellikler yoksa, yeni bir yönetilen örnek dağıtamazsınız. Yönetilen bir örnek oluşturduğunuzda, ağ kurulum üzerinde uyumsuz değişiklikler yapılmasını engellemek için alt ağa bir ağ hedefi ilkesi uygulanır. Son örnek alt ağdan kaldırıldıktan sonra, ağ hedefi ilkesi de kaldırılır.

### <a name="mandatory-inbound-security-rules"></a>Zorunlu gelen güvenlik kuralları

| Name       |Bağlantı noktası                        |Protokol|Kaynak           |Hedef|Eylem|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |Herhangi biri              |Mı ALT AĞı  |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |Mı ALT AĞı        |Mı ALT AĞı  |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|Mı ALT AĞı  |İzin Ver |

### <a name="mandatory-outbound-security-rules"></a>Zorunlu giden güvenlik kuralları

| Name       |Bağlantı noktası          |Protokol|Kaynak           |Hedef|Eylem|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |443, 12000    |TCP     |Mı ALT AĞı        |AzureCloud |İzin Ver |
|mi_subnet   |Herhangi biri           |Herhangi biri     |Mı ALT AĞı        |Mı ALT AĞı  |İzin Ver |

> [!IMPORTANT]
> Bağlantı noktaları için 9000, 9003, 1438, 1440, 1452 ve bir giden kuralı için yalnızca bir gelen kuralı olduğundan emin olun 443, 12000. Her bağlantı noktası için gelen ve giden kuralları ayrı olarak yapılandırılırsa Azure Resource Manager dağıtımlar aracılığıyla yönetilen örnek sağlama başarısız olur. Bu bağlantı noktaları ayrı kurallarda ise, dağıtım hata kodu ile başarısız olur`VnetSubnetConflictWithIntendedPolicy`

\*Mı alt ağı, x. x. x. x/y biçimindeki alt ağın IP adresi aralığını ifade eder. Bu bilgileri, Azure portal alt ağ özelliklerinde bulabilirsiniz.

> [!IMPORTANT]
> Gerekli gelen güvenlik kuralları 9000, 9003, 1438, 1440 ve 1452 bağlantı noktalarında _herhangi bir_ kaynaktan gelen trafiğe izin verse de, bu bağlantı noktaları yerleşik bir güvenlik duvarı tarafından korunur. Daha fazla bilgi için bkz. [Yönetim uç noktası adresini belirleme](sql-database-managed-instance-find-management-endpoint-ip-address.md).

> [!NOTE]
> Yönetilen bir örnekte işlemsel çoğaltma kullanırsanız ve bir yayımcı veya dağıtıcı olarak herhangi bir örnek veritabanı kullanıyorsanız, alt ağın güvenlik kurallarında bağlantı noktası 445 (TCP Giden) öğesini açın. Bu bağlantı noktası, Azure dosya paylaşımının erişimine izin verir.

### <a name="user-defined-routes"></a>Kullanıcı tanımlı yollar

|Name|Adres ön eki|Sonraki atlama|
|----|--------------|-------|
|subnet_to_vnetlocal|Mı ALT AĞı|Sanal ağ|
|mi-13-64-11-sonrakii-Internet|13.64.0.0/11|Internet|
|mi-13-104-14-sonrakii-Internet|13.104.0.0/14|Internet|
|mi-20-33-16-sonrakii-Internet|20.33.0.0/16|Internet|
|mi-20-34-15-sonrakii-Internet|20.34.0.0/15|Internet|
|mi-20-36-14-sonrakii-Internet|20.36.0.0/14|Internet|
|mi-20-40-13-sonrakii-Internet|20.40.0.0/13|Internet|
|mi-20-48-12-sonrakii-Internet|20.48.0.0/12|Internet|
|mi-20-64-10-sonrakii-Internet|20.64.0.0/10|Internet|
|mi-20-128-16-sonrakii-Internet|20.128.0.0/16|Internet|
|mi-20-135-16-sonrakii-Internet|20.135.0.0/16|Internet|
|mi-20-136-16-sonrakii-Internet|20.136.0.0/16|Internet|
|mi-20-140-15-sonrakii-Internet|20.140.0.0/15|Internet|
|mi-20-143-16-sonrakii-Internet|20.143.0.0/16|Internet|
|mi-20-144-14-sonrakii-Internet|20.144.0.0/14|Internet|
|mi-20-150-15-sonrakii-Internet|20.150.0.0/15|Internet|
|mi-20-160-12-sonrakii-Internet|20.160.0.0/12|Internet|
|mi-20-176-14-sonrakii-Internet|20.176.0.0/14|Internet|
|mi-20-180-14-sonrakii-Internet|20.180.0.0/14|Internet|
|mi-20-184-13-sonrakii-Internet|20.184.0.0/13|Internet|
|mi-20-192-10-sonrakii-Internet|20.192.0.0/10|Internet|
|mi-40-64-10-sonrakii-Internet|40.64.0.0/10|Internet|
|mi-51-4-15-sonrakii-Internet|51.4.0.0/15|Internet|
|mi-51-8-16-sonrakii-Internet|51.8.0.0/16|Internet|
|mi-51-10-15-sonrakii-Internet|51.10.0.0/15|Internet|
|mi-51-18-16-sonrakii-Internet|51.18.0.0/16|Internet|
|mi-51-51-16-sonrakii-Internet|51.51.0.0/16|Internet|
|mi-51-53-16-sonrakii-Internet|51.53.0.0/16|Internet|
|mi-51-103-16-sonrakii-Internet|51.103.0.0/16|Internet|
|mi-51-104-15-sonrakii-Internet|51.104.0.0/15|Internet|
|mi-51-132-16-sonrakii-Internet|51.132.0.0/16|Internet|
|mi-51-136-15-sonrakii-Internet|51.136.0.0/15|Internet|
|mi-51-138-16-sonrakii-Internet|51.138.0.0/16|Internet|
|mi-51-140-14-sonrakii-Internet|51.140.0.0/14|Internet|
|mi-51-144-15-sonrakii-Internet|51.144.0.0/15|Internet|
|mi-52-96-12-sonrakii-Internet|52.96.0.0/12|Internet|
|mi-52-112-14-sonrakii-Internet|52.112.0.0/14|Internet|
|mi-52-125-16-sonrakii-Internet|52.125.0.0/16|Internet|
|mi-52-126-15-sonrakii-Internet|52.126.0.0/15|Internet|
|mi-52-130-15-sonrakii-Internet|52.130.0.0/15|Internet|
|mi-52-132-14-sonrakii-Internet|52.132.0.0/14|Internet|
|mi-52-136-13-sonrakii-Internet|52.136.0.0/13|Internet|
|mi-52-145-16-sonrakii-Internet|52.145.0.0/16|Internet|
|mi-52-146-15-sonrakii-Internet|52.146.0.0/15|Internet|
|mi-52-148-14-sonrakii-Internet|52.148.0.0/14|Internet|
|mi-52-152-13-sonrakii-Internet|52.152.0.0/13|Internet|
|mi-52-160-11-sonrakii-Internet|52.160.0.0/11|Internet|
|mi-52-224-11-sonrakii-Internet|52.224.0.0/11|Internet|
|mi-64-4-18-sonrakii-Internet|64.4.0.0/18|Internet|
|mi-65-52-14-sonrakii-Internet|65.52.0.0/14|Internet|
|mi-66-119-144-20-sonrakii-Internet|66.119.144.0/20|Internet|
|mi-70-37-17-sonrakii-Internet|70.37.0.0/17|Internet|
|mi-70-37-128-18-sonrakii-Internet|70.37.128.0/18|Internet|
|mi-91-190-216-21-sonrakii-Internet|91.190.216.0/21|Internet|
|mi-94-245-64-18-sonrakii-Internet|94.245.64.0/18|Internet|
|mi-103-9-8-22-sonrakii-Internet|103.9.8.0/22|Internet|
|mi-103-25-156-24-sonrakii-Internet|103.25.156.0/24|Internet|
|mi-103-25-157-24-sonrakii-Internet|103.25.157.0/24|Internet|
|mi-103-25-158-23-sonrakii-Internet|103.25.158.0/23|Internet|
|mi-103-36-96-22-sonrakii-Internet|103.36.96.0/22|Internet|
|mi-103-255-140-22-sonrakii-Internet|103.255.140.0/22|Internet|
|mi-104-40-13-sonrakii-Internet|104.40.0.0/13|Internet|
|mi-104-146-15-sonrakii-Internet|104.146.0.0/15|Internet|
|mi-104-208-13-sonrakii-Internet|104.208.0.0/13|Internet|
|mi-111-221-16-20-sonrakii-Internet|111.221.16.0/20|Internet|
|mi-111-221-64-18-sonrakii-Internet|111.221.64.0/18|Internet|
|mi-129-75-16-sonrakii-Internet|129.75.0.0/16|Internet|
|mi-131-107-16-sonrakii-Internet|131.107.0.0/16|Internet|
|mi-131-253-1-24-sonrakii-Internet|131.253.1.0/24|Internet|
|mi-131-253-3-24-sonrakii-Internet|131.253.3.0/24|Internet|
|mi-131-253-5-24-sonrakii-Internet|131.253.5.0/24|Internet|
|mi-131-253-6-24-sonrakii-Internet|131.253.6.0/24|Internet|
|mi-131-253-8-24-sonrakii-Internet|131.253.8.0/24|Internet|
|mi-131-253-12-22-sonrakii-Internet|131.253.12.0/22|Internet|
|mi-131-253-16-23-sonrakii-Internet|131.253.16.0/23|Internet|
|mi-131-253-18-24-sonrakii-Internet|131.253.18.0/24|Internet|
|mi-131-253-21-24-sonrakii-Internet|131.253.21.0/24|Internet|
|mi-131-253-22-23-sonrakii-Internet|131.253.22.0/23|Internet|
|mi-131-253-24-21-sonrakii-Internet|131.253.24.0/21|Internet|
|mi-131-253-32-20-sonrakii-Internet|131.253.32.0/20|Internet|
|mi-131-253-61-24-sonrakii-Internet|131.253.61.0/24|Internet|
|mi-131-253-62-23-sonrakii-Internet|131.253.62.0/23|Internet|
|mi-131-253-64-18-sonrakii-Internet|131.253.64.0/18|Internet|
|mi-131-253-128-17-sonrakii-Internet|131.253.128.0/17|Internet|
|mi-132-245-16-sonrakii-Internet|132.245.0.0/16|Internet|
|mi-134-170-16-sonrakii-Internet|134.170.0.0/16|Internet|
|mi-134-177-16-sonrakii-Internet|134.177.0.0/16|Internet|
|mi-137-116-15-sonrakii-Internet|137.116.0.0/15|Internet|
|mi-137-135-16-sonrakii-Internet|137.135.0.0/16|Internet|
|mi-138-91-16-sonrakii-Internet|138.91.0.0/16|Internet|
|mi-138-196-16-sonrakii-Internet|138.196.0.0/16|Internet|
|mi-139-217-16-sonrakii-Internet|139.217.0.0/16|Internet|
|mi-139-219-16-sonrakii-Internet|139.219.0.0/16|Internet|
|mi-141-251-16-sonrakii-Internet|141.251.0.0/16|Internet|
|mi-146-147-16-sonrakii-Internet|146.147.0.0/16|Internet|
|mi-147-243-16-sonrakii-Internet|147.243.0.0/16|Internet|
|mi-150-171-16-sonrakii-Internet|150.171.0.0/16|Internet|
|mi-150-242-48-22-sonrakii-Internet|150.242.48.0/22|Internet|
|mi-157-54-15-sonrakii-Internet|157.54.0.0/15|Internet|
|mi-157-56-14-sonrakii-Internet|157.56.0.0/14|Internet|
|mi-157-60-16-sonrakii-Internet|157.60.0.0/16|Internet|
|mi-167-105-16-sonrakii-Internet|167.105.0.0/16|Internet|
|mi-167-220-16-sonrakii-Internet|167.220.0.0/16|Internet|
|mi-168-61-16-sonrakii-Internet|168.61.0.0/16|Internet|
|mi-168-62-15-sonrakii-Internet|168.62.0.0/15|Internet|
|mi-191-232-13-sonrakii-Internet|191.232.0.0/13|Internet|
|mi-192-32-16-sonrakii-Internet|192.32.0.0/16|Internet|
|mi-192-48-225-24-sonrakii-Internet|192.48.225.0/24|Internet|
|mi-192-84-159-24-sonrakii-Internet|192.84.159.0/24|Internet|
|mi-192-84-160-23-sonrakii-Internet|192.84.160.0/23|Internet|
|mi-192-197-157-24-sonrakii-Internet|192.197.157.0/24|Internet|
|mi-193-149-64-19-sonrakii-Internet|193.149.64.0/19|Internet|
|mi-193-221-113-24-sonrakii-Internet|193.221.113.0/24|Internet|
|mi-194-69-96-19-sonrakii-Internet|194.69.96.0/19|Internet|
|mi-194-110-197-24-sonrakii-Internet|194.110.197.0/24|Internet|
|mi-198-105-232-22-sonrakii-Internet|198.105.232.0/22|Internet|
|mi-198-200-130-24-sonrakii-Internet|198.200.130.0/24|Internet|
|mi-198-206-164-24-sonrakii-Internet|198.206.164.0/24|Internet|
|mi-199-60-28-24-sonrakii-Internet|199.60.28.0/24|Internet|
|mi-199-74-210-24-sonrakii-Internet|199.74.210.0/24|Internet|
|mi-199-103-90-23-sonrakii-Internet|199.103.90.0/23|Internet|
|mi-199-103-122-24-sonrakii-Internet|199.103.122.0/24|Internet|
|mi-199-242-32-20-sonrakii-Internet|199.242.32.0/20|Internet|
|mi-199-242-48-21-sonrakii-Internet|199.242.48.0/21|Internet|
|mi-202-89-224-20-sonrakii-Internet|202.89.224.0/20|Internet|
|mi-204-13-120-21-sonrakii-Internet|204.13.120.0/21|Internet|
|mi-204-14-180-22-sonrakii-Internet|204.14.180.0/22|Internet|
|mi-204-79-135-24-sonrakii-Internet|204.79.135.0/24|Internet|
|mi-204-79-179-24-sonrakii-Internet|204.79.179.0/24|Internet|
|mi-204-79-181-24-sonrakii-Internet|204.79.181.0/24|Internet|
|mi-204-79-188-24-sonrakii-Internet|204.79.188.0/24|Internet|
|mi-204-79-195-24-sonrakii-Internet|204.79.195.0/24|Internet|
|mi-204-79-196-23-sonrakii-Internet|204.79.196.0/23|Internet|
|mi-204-79-252-24-sonrakii-Internet|204.79.252.0/24|Internet|
|mi-204-152-18-23-sonrakii-Internet|204.152.18.0/23|Internet|
|mi-204-152-140-23-sonrakii-Internet|204.152.140.0/23|Internet|
|mi-204-231-192-24-sonrakii-Internet|204.231.192.0/24|Internet|
|mi-204-231-194-23-sonrakii-Internet|204.231.194.0/23|Internet|
|mi-204-231-197-24-sonrakii-Internet|204.231.197.0/24|Internet|
|mi-204-231-198-23-sonrakii-Internet|204.231.198.0/23|Internet|
|mi-204-231-200-21-sonrakii-Internet|204.231.200.0/21|Internet|
|mi-204-231-208-20-sonrakii-Internet|204.231.208.0/20|Internet|
|mi-204-231-236-24-sonrakii-Internet|204.231.236.0/24|Internet|
|mi-205-174-224-20-sonrakii-Internet|205.174.224.0/20|Internet|
|mi-206-138-168-21-sonrakii-Internet|206.138.168.0/21|Internet|
|mi-206-191-224-19-sonrakii-Internet|206.191.224.0/19|Internet|
|mi-207-46-16-sonrakii-Internet|207.46.0.0/16|Internet|
|mi-207-68-128-18-sonrakii-Internet|207.68.128.0/18|Internet|
|mi-208-68-136-21-sonrakii-Internet|208.68.136.0/21|Internet|
|mi-208-76-44-22-sonrakii-Internet|208.76.44.0/22|Internet|
|mi-208-84-21-sonrakii-Internet|208.84.0.0/21|Internet|
|mi-209-240-192-19-sonrakii-Internet|209.240.192.0/19|Internet|
|mi-213-199-128-18-sonrakii-Internet|213.199.128.0/18|Internet|
|mi-216-32-180-22-sonrakii-Internet|216.32.180.0/22|Internet|
|mi-216-220-208-20-sonrakii-Internet|216.220.208.0/20|Internet|
|mi-23-96-13-sonrakii-Internet|23.96.0.0/13|Internet|
|mi-42-159-16-sonrakii-Internet|42.159.0.0/16|Internet|
|mi-51-13-17-sonrakii-Internet|51.13.0.0/17|Internet|
|mi-51-107-16-sonrakii-Internet|51.107.0.0/16|Internet|
|mi-51-116-16-sonrakii-Internet|51.116.0.0/16|Internet|
|mi-51-120-16-sonrakii-Internet|51.120.0.0/16|Internet|
|mi-51-120-128-17-sonrakii-Internet|51.120.128.0/17|Internet|
|mi-51-124-16-sonrakii-Internet|51.124.0.0/16|Internet|
|mi-102-37-18-sonrakii-Internet|102.37.0.0/18|Internet|
|mi-102-133-16-sonrakii-Internet|102.133.0.0/16|Internet|
|mi-199-30-16-20-sonrakii-Internet|199.30.16.0/20|Internet|
|mi-204-79-180-24-sonrakii-Internet|204.79.180.0/24|Internet|
||||

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [SQL veritabanı gelişmiş veri güvenliği](sql-database-managed-instance.md).
- [Yeni bir Azure sanal ağını](sql-database-managed-instance-create-vnet-subnet.md) veya yönetilen örnekleri dağıtabileceğiniz mevcut bir [Azure sanal ağını](sql-database-managed-instance-configure-vnet-subnet.md) ayarlamayı öğrenin.
- Yönetilen örnekleri dağıtmak istediğiniz [alt ağın boyutunu hesaplayın](sql-database-managed-instance-determine-size-vnet-subnet.md) .
- Yönetilen bir örnek oluşturmayı öğrenin:
  - [Azure Portal](sql-database-managed-instance-get-started.md).
  - [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)kullanarak.
  - [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)kullanarak.
  - [Bir Azure Resource Manager şablonu kullanarak (SSMS ile birlikte bulunan JumpBox kullanarak)](https://azure.microsoft.com/resources/templates/201-sqlmi-new-vnet-w-jumpbox/). 
