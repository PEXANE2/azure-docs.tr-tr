---
title: Azure SQL veritabanı 'nda yönetilen bir örnek için bağlantı mimarisi | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği iletişimi ve bağlantı mimarisi hakkında bilgi edinin ve bileşenlerin yönetilen örneğe trafiği nasıl yönlendirmiş olduğunu öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: fasttrack-edit
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 04/16/2019
ms.openlocfilehash: 960320e280a613a537f1918d93e4584a13a0b374
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309969"
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

Yönetilen örnekler, yedeklemeler için Azure depolama, telemetri için Azure Event Hubs, kimlik doğrulaması için Azure Active Directory Azure Key Vault Saydam Veri Şifrelemesi (TDE) ve birkaç Azure platform hizmeti sağlar Güvenlik ve desteklenebilirlik özellikleri. Yönetilen örnekler bu hizmetlere bağlantılar oluşturur.

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

İstemciler, form `<mi_name>.<dns_zone>.database.windows.net`içeren bir ana bilgisayar adı kullanarak yönetilen bir örneğe bağlanır. Bu ana bilgisayar adı, ortak bir etki alanı adı sistemi (DNS) bölgesinde kayıtlı ve genel olarak çözümlenebildiği halde özel bir IP adresi olarak çözümlenir. , `zone-id` Kümeyi oluşturduğunuzda otomatik olarak oluşturulur. Yeni oluşturulan bir küme, ikincil yönetilen bir örnek barındırıyorsa, birincil kümeyle bölge KIMLIĞINI paylaşır. Daha fazla bilgi için bkz. [otomatik yük devretme gruplarını kullanarak birden çok veritabanının saydam ve eşgüdümlü yük devretmesini etkinleştirme](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets).

Bu özel IP adresi, yönetilen örneğin iç yük dengeleyicisine aittir. Yük dengeleyici, trafiği yönetilen örneğin ağ geçidine yönlendirir. Aynı küme içinde birden çok yönetilen örnek çalıştırılabildiğinden ağ geçidi, trafiği doğru SQL altyapısı hizmetine yönlendirmek için yönetilen örneğin ana bilgisayar adını kullanır.

Yönetim ve Dağıtım Hizmetleri, bir dış yük dengeleyiciye eşlenen bir [Yönetim uç noktası](#management-endpoint) kullanarak yönetilen örneğe bağlanır. Trafik yalnızca yönetilen örneğin yönetim bileşenlerinin kullandığı önceden tanımlanmış bir bağlantı noktası kümesi üzerinde alındığında düğümlere yönlendirilir. Düğümlerdeki yerleşik bir güvenlik duvarı yalnızca Microsoft IP aralıklarından gelen trafiğe izin verecek şekilde ayarlanmıştır. Sertifikalar, Yönetim bileşenleri ile yönetim düzlemi arasındaki iletişimin hepsini karşılıklı olarak doğrular.

## <a name="management-endpoint"></a>Yönetim uç noktası

Microsoft, yönetilen örneği bir yönetim uç noktası kullanarak yönetir. Bu uç nokta, örneğin sanal kümesinin içindedir. Yönetim uç noktası, ağ düzeyindeki yerleşik bir güvenlik duvarı tarafından korunur. Uygulama düzeyinde, karşılıklı sertifika doğrulaması tarafından korunur. Uç noktanın IP adresini bulmak için, bkz. [Yönetim uç NOKTASıNıN IP adresini belirleme](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Bağlantılar yönetilen örnek içinde (yedeklemeler ve denetim günlükleri gibi) başlatıldığında trafik yönetim uç noktasının genel IP adresinden başlar. Yalnızca yönetilen örneğin IP adresine izin vermek için güvenlik duvarı kuralları ayarlayarak, yönetilen bir örnekten ortak hizmetlere erişimi sınırlayabilirsiniz. Daha fazla bilgi için bkz. [yönetilen örneğin yerleşik güvenlik duvarını doğrulama](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Yönetilen örnek bölgesi içindeki Azure hizmetlerine giden trafik en iyi duruma getirilir ve bu nedenle yönetilen örnek yönetimi uç noktası genel IP adresi olarak kullanılamaz. Bu nedenle, depolama için en yaygın olarak depolama için IP tabanlı güvenlik duvarı kuralları kullanmanız gerekirse, hizmetin yönetilen örnekten farklı bir bölgede olması gerekir.

## <a name="network-requirements"></a>Ağ gereksinimleri

Yönetilen bir örneği sanal ağın içindeki ayrılmış bir alt ağda dağıtın. Alt ağ şu özelliklere sahip olmalıdır:

- **Ayrılmış alt ağ:** Yönetilen örneğin alt ağı kendisiyle ilişkili başka bir bulut hizmeti içeremez ve bir ağ geçidi alt ağı olamaz. Alt ağ herhangi bir kaynak ve yönetilen örnek içeremez ve daha sonra alt ağdaki diğer kaynak türlerini ekleyemezsiniz.
- **Ağ güvenlik grubu (NSG):** Sanal ağla ilişkili bir NSG 'nin, diğer kurallardan önce [gelen güvenlik kurallarını](#mandatory-inbound-security-rules) ve [giden güvenlik kurallarını](#mandatory-outbound-security-rules) tanımlamanız gerekir. Yönetilen örnek, yönlendirme bağlantıları için yapılandırıldığında, bağlantı noktası 1433 ve bağlantı noktaları 11000-11999 ' deki trafiği filtreleyerek, yönetilen örneğin veri uç noktasına erişimi denetlemek için NSG kullanabilirsiniz.
- **Kullanıcı tanımlı yol (UDR) tablosu:** Sanal ağla ilişkili bir UDR tablosu belirli [girdileri](#user-defined-routes)içermelidir.
- **Hizmet uç noktası yok:** Yönetilen örnek alt ağıyla ilişkili hizmet uç noktası yok. Sanal ağı oluştururken hizmet uç noktaları seçeneğinin devre dışı olduğundan emin olun.
- **Yeterli IP adresi:** Yönetilen örnek alt ağı en az 16 IP adresine sahip olmalıdır. Önerilen minimum değer 32 IP adresleridir. Daha fazla bilgi için bkz. [yönetilen örnekler için alt ağın boyutunu belirleme](sql-database-managed-instance-determine-size-vnet-subnet.md). Yönetilen örnekleri, [yönetilen örnekler için ağ gereksinimlerini](#network-requirements)karşılayacak şekilde yapılandırdıktan sonra [, var olan ağda](sql-database-managed-instance-configure-vnet-subnet.md) dağıtabilirsiniz. Aksi takdirde, [Yeni bir ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md)oluşturun.

> [!IMPORTANT]
> Hedef alt ağda bu özellikler yoksa, yeni bir yönetilen örnek dağıtamazsınız. Yönetilen bir örnek oluşturduğunuzda, ağ kurulum üzerinde uyumsuz değişiklikler yapılmasını engellemek için alt ağa bir ağ hedefi ilkesi uygulanır. Son örnek alt ağdan kaldırıldıktan sonra, ağ hedefi ilkesi de kaldırılır.

### <a name="mandatory-inbound-security-rules"></a>Zorunlu gelen güvenlik kuralları

| Ad       |Port                        |Protocol|Kaynak           |Hedef|Action|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |Any              |MI ALT AĞI  |Allow |
|mi_subnet   |Any                         |Any     |MI ALT AĞI        |MI ALT AĞI  |Allow |
|health_probe|Any                         |Any     |AzureLoadBalancer|MI ALT AĞI  |Allow |

### <a name="mandatory-outbound-security-rules"></a>Zorunlu giden güvenlik kuralları

| Ad       |Port          |Protocol|Kaynak           |Hedef|Action|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |80, 443, 12000|TCP     |MI ALT AĞI        |AzureCloud |Allow |
|mi_subnet   |Any           |Any     |MI ALT AĞI        |MI ALT AĞI  |Allow |

> [!IMPORTANT]
> 9000, 9003, 1438, 1440, 1452 ve bağlantı noktaları 80, 443, 12000 için tek bir gelen kuralı olduğundan emin olun. Her bağlantı noktası için gelen ve giden kuralları ayrı olarak yapılandırılırsa Azure Resource Manager dağıtımlar aracılığıyla yönetilen örnek sağlama başarısız olur. Bu bağlantı noktaları ayrı kurallarda ise, dağıtım hata kodu ile başarısız olur`VnetSubnetConflictWithIntendedPolicy`

\*Mı alt ağı, 10. x. x. x/y biçimindeki alt ağın IP adresi aralığını ifade eder. Bu bilgileri, Azure portal alt ağ özelliklerinde bulabilirsiniz.

> [!IMPORTANT]
> Gerekli gelen güvenlik kuralları 9000, 9003, 1438, 1440 ve 1452 bağlantı noktalarında _herhangi bir_ kaynaktan gelen trafiğe izin verse de, bu bağlantı noktaları yerleşik bir güvenlik duvarı tarafından korunur. Daha fazla bilgi için bkz. [Yönetim uç noktası adresini belirleme](sql-database-managed-instance-find-management-endpoint-ip-address.md).
> [!NOTE]
> Yönetilen bir örnekte işlemsel çoğaltma kullanırsanız ve bir yayımcı veya dağıtıcı olarak herhangi bir örnek veritabanı kullanıyorsanız, alt ağın güvenlik kurallarında bağlantı noktası 445 (TCP Giden) öğesini açın. Bu bağlantı noktası, Azure dosya paylaşımının erişimine izin verir.

### <a name="user-defined-routes"></a>Kullanıcı tanımlı yollar

|Ad|Adres ön eki|Sonraki atlama|
|----|--------------|-------|
|subnet_to_vnetlocal|MI ALT AĞI|Sanal ağ|
|mi-13-64-11-sonrakii-Internet|13.64.0.0/11|Internet|
|mi-13-96-13-sonrakii-Internet|13.96.0.0/13|Internet|
|mi-13-104-14-sonrakii-Internet|13.104.0.0/14|Internet|
|mi-20-8-sonrakii-Internet|20.0.0.0/8|Internet|
|mi-23-96-13-sonrakii-Internet|23.96.0.0/13|Internet|
|mi-40-64-10-sonrakii-Internet|40.64.0.0/10|Internet|
|mi-42-159-16-sonrakii-Internet|42.159.0.0/16|Internet|
|mi-51-8-sonrakii-Internet|51.0.0.0/8|Internet|
|mi-52-8-sonrakii-Internet|52.0.0.0/8|Internet|
|mi-64-4-18-sonrakii-Internet|64.4.0.0/18|Internet|
|mi-65-52-14-sonrakii-Internet|65.52.0.0/14|Internet|
|mi-66-119-144-20-sonrakii-Internet|66.119.144.0/20|Internet|
|mi-70-37-17-sonrakii-Internet|70.37.0.0/17|Internet|
|mi-70-37-128-18-sonrakii-Internet|70.37.128.0/18|Internet|
|mi-91-190-216-21-sonrakii-Internet|91.190.216.0/21|Internet|
|mi-94-245-64-18-sonrakii-Internet|94.245.64.0/18|Internet|
|mi-103-9-8-22-sonrakii-Internet|103.9.8.0/22|Internet|
|mi-103-25-156-22-sonrakii-Internet|103.25.156.0/22|Internet|
|mi-103-36-96-22-sonrakii-Internet|103.36.96.0/22|Internet|
|mi-103-255-140-22-sonrakii-Internet|103.255.140.0/22|Internet|
|mi-104-40-13-sonrakii-Internet|104.40.0.0/13|Internet|
|mi-104-146-15-sonrakii-Internet|104.146.0.0/15|Internet|
|mi-104-208-13-sonrakii-Internet|104.208.0.0/13|Internet|
|mi-111-221-16-20-sonrakii-Internet|111.221.16.0/20|Internet|
|mi-111-221-64-18-sonrakii-Internet|111.221.64.0/18|Internet|
|mi-129-75-16-sonrakii-Internet|129.75.0.0/16|Internet|
|mi-131-253-16-sonrakii-Internet|131.253.0.0/16|Internet|
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
|mi-167-220-16-sonrakii-Internet|167.220.0.0/16|Internet|
|mi-168-61-16-sonrakii-Internet|168.61.0.0/16|Internet|
|mi-168-62-15-sonrakii-Internet|168.62.0.0/15|Internet|
|mi-191-232-13-sonrakii-Internet|191.232.0.0/13|Internet|
|mi-192-32-16-sonrakii-Internet|192.32.0.0/16|Internet|
|mi-192-48-225-24-sonrakii-Internet|192.48.225.0/24|Internet|
|mi-192-84-159-24-sonrakii-Internet|192.84.159.0/24|Internet|
|mi-192-84-160-23-sonrakii-Internet|192.84.160.0/23|Internet|
|mi-192-100-102-24-sonrakii-Internet|192.100.102.0/24|Internet|
|mi-192-100-103-24-sonrakii-Internet|192.100.103.0/24|Internet|
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
||||

Ayrıca, sanal ağ geçidi veya sanal ağ gereci (NVA) aracılığıyla şirket içi özel IP aralıklarına sahip trafiği bir hedef olarak yönlendirmek için yol tablosuna giriş ekleyebilirsiniz.

Sanal ağ özel bir DNS içeriyorsa, özel DNS sunucusunun ortak DNS kayıtlarını çözümleyebilmesi gerekir. Azure AD kimlik doğrulaması gibi ek özelliklerin kullanılması ek FQDN 'lerin çözümlenmesini gerektirebilir. Daha fazla bilgi için bkz. [özel DNS ayarlama](sql-database-managed-instance-custom-dns.md).

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [SQL veritabanı gelişmiş veri güvenliği](sql-database-managed-instance.md).
- [Yeni bir Azure sanal ağını](sql-database-managed-instance-create-vnet-subnet.md) veya yönetilen örnekleri dağıtabileceğiniz mevcut bir [Azure sanal ağını](sql-database-managed-instance-configure-vnet-subnet.md) ayarlamayı öğrenin.
- Yönetilen örnekleri dağıtmak istediğiniz [alt ağın boyutunu hesaplayın](sql-database-managed-instance-determine-size-vnet-subnet.md) .
- Yönetilen bir örnek oluşturmayı öğrenin:
  - [Azure Portal](sql-database-managed-instance-get-started.md).
  - [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)kullanarak.
  - [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)kullanarak.
  - [Bir Azure Resource Manager şablonu kullanarak (SSMS ile birlikte bulunan JumpBox kullanarak)](https://portal.azure.com/). 
