---
title: Azure SQL Veritabanı güvenlik özellikleri
description: Bu makalede, Azure SQL Veritabanı'nın Azure'daki müşteri verilerini nasıl koruduğuna ilgili genel bir açıklama verilmektedir.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2020
ms.author: terrylan
ms.openlocfilehash: ad6d3992f03802174eb03aa30b57b8d3dac1d6c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942966"
---
# <a name="azure-sql-database-security-features"></a>Azure SQL Veritabanı güvenlik özellikleri    
Azure SQL Veritabanı, Azure'da ilişkisel veritabanı hizmeti sağlar. Müşteri verilerini korumak ve müşterilerin ilişkisel veritabanı hizmetinden beklediği güçlü güvenlik özellikleri sağlamak için SQL Veritabanı'nın kendi güvenlik yetenekleri kümeleri vardır. Bu özellikler, Azure'dan devralınan denetimler üzerine inşa edilir.

## <a name="security-capabilities"></a>Güvenlik özellikleri

### <a name="usage-of-the-tds-protocol"></a>TDS protokolünün kullanımı
Azure SQL Veritabanı, veritabanının yalnızca Varsayılan TCP/1433 bağlantı noktası üzerinden erişilebilir olmasını gerektiren yalnızca tabular veri akışı (TDS) protokolünü destekler.

### <a name="azure-sql-database-firewall"></a>Azure SQL Veritabanı güvenlik duvarı
Azure SQL Veritabanı, müşteri verilerinin korunmasına yardımcı olmak için, varsayılan olarak aşağıda gösterildiği gibi TÜM SQL Veritabanı sunucusuna erişimi engelleyen bir güvenlik duvarı işlevi içerir.

![Azure SQL Veritabanı güvenlik duvarı](./media/infrastructure-sql/sql-database-firewall.png)

Ağ geçidi güvenlik duvarı adresleri sınırlayabilir ve bu da müşterilerin parçalı denetimin kabul edilebilir IP adresleriaralıklarını belirtmesine olanak tanır. Güvenlik duvarı, her isteğin menşeli IP adresine dayalı erişim sağlar.

Müşteriler, bir yönetim portalı kullanarak veya Azure SQL Veritabanı Yönetimi REST API'sini kullanarak güvenlik duvarı yapılandırmasını gerçekleştirebilir. Varsayılan olarak Azure SQL Veritabanı ağ geçidi güvenlik duvarı, tüm müşteri TDS'nin Azure SQL veritabanı örneklerine erişimini engeller. Müşteriler, kaynak ve hedef internet adreslerine, protokollere ve bağlantı noktası numaralarına göre Azure SQL Veritabanı bağlantılarına izin vermek için erişim denetim listelerini (ALA'lar) kullanarak erişimi yapılandırmalıdır.

### <a name="dosguard"></a>Dosguard
Hizmet reddi (DoS) saldırıları, DoSGuard adlı bir SQL Veritabanı ağ geçidi hizmeti tarafından azaltılır. DoSGuard, IP adreslerinden başarısız girişleri etkin bir şekilde izler. Belirli bir IP adresinden belirli bir süre içinde birden fazla başarısız giriş varsa, IP adresinin önceden tanımlanmış bir süre için hizmetteki kaynaklara erişmesi engellenir.

Ayrıca Azure SQL Veritabanı ağ geçidi şunları gerçekleştirir:

- Veritabanı sunucularına bağlandığında TDS FIPS 140-2 doğrulanmış şifreli bağlantıları uygulamak için güvenli kanal yeteneği görüşmeleri.
- Müşterilerden gelen bağlantıları kabul ederken durumlu TDS paket denetimi. Ağ geçidi bağlantı bilgilerini doğrular ve TDS paketlerindeki bağlantı dizesinde belirtilen veritabanı adını temel alarak uygun fiziksel sunucuya iletir.

Azure SQL Veritabanı teklifinin ağ güvenliği için kapsamlı ilke, yalnızca hizmetin çalışmasına izin vermek için gereken bağlantı ve iletişime izin vermektir. Diğer tüm bağlantı noktaları, protokoller ve bağlantılar varsayılan olarak engellenir. Sanal yerel alan ağları (VLAN'ler) ve ALA'lar, kaynak ve hedef ağlara, protokollere ve bağlantı noktası numaralarına göre ağ iletişimini kısıtlamak için kullanılır.

Ağ tabanlı ALA'ları uygulamak için onaylanan mekanizmalar, yönlendiriciler ve yük dengeleyicileri üzerinde ALAK'lar içerir. Bu mekanizmalar, müşteri tarafından yapılandırılan Azure ağ, konuk VM güvenlik duvarı ve Azure SQL Veritabanı ağ geçidi güvenlik duvarı kuralları tarafından yönetilir.

## <a name="data-segregation-and-customer-isolation"></a>Veri ayırma ve müşteri yalıtımı
Azure üretim ağı, herkese açık sistem bileşenlerinin iç kaynaklardan ayrılması için yapılandırılmıştır. Genel kullanıma açık Azure portalına erişim sağlayan web sunucuları ile müşteri uygulama örneklerinin ve müşteri verilerinin bulunduğu temel Azure sanal altyapısı arasında fiziksel ve mantıksal sınırlar vardır.

Herkesin erişebileceği tüm bilgiler Azure üretim ağı içinde yönetilir. Üretim ağı iki faktörlü kimlik doğrulama ve sınır koruma mekanizmalarına tabidir, önceki bölümde açıklanan güvenlik duvarı ve güvenlik özellik kümesini kullanır ve sonraki bölümlerde belirtildiği gibi veri yalıtım işlevlerini kullanır.

### <a name="unauthorized-systems-and-isolation-of-the-fc"></a>Yetkisiz sistemler ve FC izolasyon
Kumaş denetleyicisi (FC) Azure kumaşının merkezi orkestratörlüğü olduğundan, özellikle müşteri uygulamalarında potansiyel olarak tehlikeye giren FA'lardan kaynaklanan tehditleri azaltmak için önemli denetimler yapılır. FC, aygıt bilgileri (örneğin, MAC adresi) FC içinde önceden yüklenmeyen herhangi bir donanım tanımaz. FC'deki DHCP sunucuları, önyüklemeye istekli oldukları düğümlerin MAC adreslerinin listelerini yapılandırmış. Yetkisiz sistemler bağlı olsa bile, kumaş envanterine dahil edilmezler ve bu nedenle kumaş envanterindeki herhangi bir sistemle iletişim kurmaya bağlı veya yetkili değildirler. Bu, yetkisiz sistemlerin FC ile iletişim kurma ve VLAN ve Azure'a erişim sağlama riskini azaltır.

### <a name="vlan-isolation"></a>VLAN yalıtımı
Azure üretim ağı mantıksal olarak üç ana VLAN'a ayrılmıştır:

- Ana VLAN: Güvenilmeyen müşteri düğümlerini birbirine bağlar.
- FC VLAN: Güvenilir FC'ler ve destek sistemleri içerir.
- VLAN aygıt: Güvenilir ağ ve diğer altyapı aygıtlarını içerir.

### <a name="packet-filtering"></a>Paket filtreleme
IPFilter ve düğümlerin kök işletim sistemi ve konuk işletim sistemi üzerinde uygulanan yazılım güvenlik duvarları bağlantı kısıtlamaları zorlar ve VM'ler arasında yetkisiz trafiği önler.

### <a name="hypervisor-root-os-and-guest-vms"></a>Hipervizör, kök işletim sistemi ve konuk VM'ler
Root OS'nin konuk VM'lerden ve konuk VM'lerden yalıtımı hipervizör ve kök işletim sistemi tarafından yönetilir.

### <a name="types-of-rules-on-firewalls"></a>Güvenlik duvarlarında kural türleri
Bir kural olarak tanımlanır:

{Src IP, Src Port, Hedef IP, Hedef Bağlantı Noktası, Hedef Protokolü, Giriş/Çıkış, Stateful/Stateless, Stateful Flow Timeout}.

Eşzamanlı boşta karakter (SYN) paketlerine yalnızca kurallardan herhangi biri izin verirse girip çıkmasına izin verilir. Azure, TCP için, ilke olarak Yalnızca SYN olmayan tüm paketlere VM'ye giriş veya çıkışa izin verdiği halsiz kurallar kullanır. Güvenlik öncül herhangi bir ana bilgisayar yığını daha önce bir SYN paketi görmedim eğer syn olmayan bir yoksayma esnek olmasıdır. TCP protokolünün kendisi durumludur ve devletsiz SYN tabanlı kuralla birlikte, durum lu bir uygulamanın genel bir davranışını sağlar.

Azure, Kullanıcı Veri Gramı Protokolü (UDP) için durum sallanması na uygun bir kural kullanır. Bir UDP paketi her kuralla eşleştiğinde, diğer yönde bir ters akış oluşturulur. Bu akış yerleşik bir zaman ayarı vardır.

Müşteriler, Azure'un sağladığı güvenlik duvarlarını üstüne kurmaktan sorumludur. Burada müşteriler gelen ve giden trafiğin kurallarını tanımlayabilir.

### <a name="production-configuration-management"></a>Üretim konfigürasyon yönetimi
Standart güvenli yapılandırmalar, Azure ve Azure SQL Veritabanı'ndaki ilgili işlem ekipleri tarafından korunur. Üretim sistemlerindeki tüm yapılandırma değişiklikleri merkezi bir takip sistemi aracılığıyla belgelenir ve izlenir. Yazılım ve donanım değişiklikleri merkezi izleme sistemi üzerinden izlenir. ACL ile ilgili ağ değişiklikleri bir ACL yönetim hizmeti kullanılarak izlenir.

Azure'daki tüm yapılandırma değişiklikleri hazırlama ortamında geliştirilir ve sınanmaktadır ve daha sonra üretim ortamında dağıtılır. Yazılım yapılarının test in bir parçası olarak gözden geçirilir. Güvenlik ve gizlilik denetimleri giriş kontrol listesi kriterlerinin bir parçası olarak gözden geçirilir. Değişiklikler, ilgili dağıtım ekibi tarafından zamanlanan aralıklarda dağıtılır. Sürümler, üretime gönderilmeden önce ilgili dağıtım ekibi personeli tarafından gözden geçirilir ve imzalanır.

Değişiklikler başarı için izlenir. Bir hata senaryosunda, değişiklik önceki durumuna geri alınır veya atanan personelin onayı ile başarısızlığı gidermek için bir düzeltme dağıtılır. Kaynak Deposu, Git, TFS, Master Data Services (MDS), koşucular, Azure güvenlik izleme, FC ve WinFabric platformu, Azure sanal ortamındaki yapılandırma ayarlarını merkezi olarak yönetmek, uygulamak ve doğrulamak için kullanılır.

Benzer şekilde, donanım ve ağ değişiklikleri yapı gereksinimlerine bağlılıklarını değerlendirmek için doğrulama adımları oluşturmuştur. Sürümler, yığın daki ilgili grupların eşgüdümlü bir değişim danışma kurulu (CAB) aracılığıyla gözden geçirilir ve yetkilendirilir.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısını korumak için ne yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri veri koruması](protection-customer-data.md)
