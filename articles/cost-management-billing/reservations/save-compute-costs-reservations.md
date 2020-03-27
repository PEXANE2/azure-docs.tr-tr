---
title: Azure Rezervasyonları nedir?
description: Sanal makineleriniz, SQL veritabanlarınız, Azure Cosmos DB ve diğer kaynak maliyetlerinde tasarruf etmek için Azure Ayrılmış Sanal Makine Örnekleri ve fiyatlandırma hakkında bilgi edinin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: banders
ms.openlocfilehash: 6277a7e7dc5891a3bc67c298a31344284c92e31d
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235646"
---
# <a name="what-are-azure-reservations"></a>Azure Rezervasyonları nedir?

Azure Rezervasyonları birden çok ürün için bir yıllık veya üç yıllık planları kabul ederek tasarruf etmenize yardımcı olur. Taahhütte bulunarak kullandığınız kaynakları indirimli satın alabilirsiniz. Rezervasyonlar, kaynak maliyetlerinizi kullandıkça öde fiyatlarına kıyasla %72'ye varan oranda azaltabilir. Rezervasyonlar bir faturalandırma indirimi sağlar ve kaynaklarınızın çalışma zamanı durumunu etkilemez. Rezervasyon satın aldıktan sonra, eşleşen kaynaklara otomatik olarak indirim uygulanır.

Bir rezervasyonun ödemesini peşin veya aylık olarak yapabilirsiniz. Peşin ve aylık rezervasyonların toplam maliyeti aynıdır ve aylık ödemeyi seçtiğinizde ekstra ücret ödemezsiniz. Aylık ödeme, üçüncü taraf ürünleri için değil Azure rezervasyonları için kullanılabilir.

[Azure portalından](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) da rezervasyon satın alabilirsiniz.

## <a name="why-buy-a-reservation"></a>Neden rezervasyon satın almalıyım?

Rezervasyonları destekleyen tutarlı bir kaynak kullanımınız varsa, bir rezervasyon satın almak size maliyetleri düşürme seçeneği getirir. Örneğin, bir hizmetin örneklerini rezervasyon olmadan sürekli olarak çalıştırdığınızda kullandıkça öde tarifesine göre ücretlendirilirsiniz. Rezervasyon satın aldığınız anda rezervasyon indirimini elde edersiniz. Kaynaklar artık kullandıkça öde tarifelerine göre ücretlendirilmez.

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Satın alma sonrasında, rezervasyon indirimi rezervasyonu satın alırken seçtiğiniz özniteliklerle eşleşen kaynak kullanımına otomatik olarak uygulanır. Öznitelikler SKU, bölgeler (uygun olduğunda) ve kapsamdan oluşur. Rezervasyon tasarruflarının nereye uygulanacağını rezervasyon kapsamı belirler.

İndirimin nasıl uygulanacağı hakkında daha fazla bilgi için bkz. [Ayrılmış örnek indirimini uygulama](reservation-discount-application.md).

Rezervasyon kapsamının nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Kapsam rezervasyonları](prepare-buy-reservation.md#scope-reservations).


## <a name="flexibility-with-azure-reservations"></a>Azure rezervasyonlarıyla esneklik

Azure Ayrılmış Sanal Makine Örnekleri, değişen ihtiyaçlarınızın karşılanmasına yardımcı olma esnekliği sağlar. Bir rezervasyonu aynı türdeki başka bir rezervasyonla değiştirebilirsiniz. İhtiyaç duymadığınız rezervasyonları iade ederek de 12 aylık kayan pencerede 50.000 ABD dolarına kadar para iadesi alabilirsiniz. Para iadesi üst sınırı, Microsoft ile sözleşmenizin kapsamındaki tüm rezervasyonlar için geçerlidir.

Daha fazla bilgi için bkz. [Azure Rezervasyonları için self servis değişimler ve para iadeleri](exchange-and-refund-azure-reservations.md)


## <a name="charges-covered-by-reservation"></a>Rezervasyon kapsamındaki ücretler

- **Ayrılmış Sanal Makine Örneği** - Bir rezervasyon yalnızca sanal makine işlem maliyetlerini içerir. Ek yazılım, Windows, ağ veya depolama ücretlerini kapsamaz.
- **Azure Depolama ayrılmış kapasitesi** - Bir rezervasyon, Blob depolama veya Azure Data Lake 2. Nesil depolama için standart depolama hesaplarının depolama kapasitesini kapsar. Rezervasyon, bant genişliğini veya işlem oranlarını kapsamaz.
- **Azure Cosmos DB ayrılmış kapasitesi** - Rezervasyon yalnızca kaynaklarınız için sağlanan aktarım hızını kapsar. Depolama ve ağ ücretlerini kapsamaz.
- **SQL veritabanı ayrılmış sanal çekirdeği** - Rezervasyona yalnızca işlem maliyetleri dahildir. SQL lisansı ayrı faturalandırılır.
- **SQL Veri Ambarı** - Rezervasyon cDWU kullanımını kapsar. SQL Veri Ambarı’nın kullanımıyla ilişkili depolama ve ağ ücretlerini kapsamaz.
- **Azure Databricks** - Rezervasyon yalnızca DBU kullanımını kapsar. İşlem, depolama ve ağ iletişimi gibi diğer ücretler ayrı olarak uygulanır.
- **App Service taban fiyatı** - Rezervasyon, damga pulu kullanımını kapsar. Çalışanlara uygulanmaz, bu nedenle damga puluyla ilişkili diğer tüm kaynaklar ayrı olarak ücretlendirilir.
- **MySQL için Azure Veritabanı** - Rezervasyona yalnızca işlem maliyetleri dahildir. MySQL Veritabanı sunucusuyla ilişkili yazılım, ağ veya depolama ücretleri rezervasyonun kapsamında değildir.
- **PostgreSQL için Azure Veritabanı** - Rezervasyona yalnızca işlem maliyetleri dahildir. PostgreSQL Veritabanı sunucusuyla ilişkili yazılım, ağ veya depolama ücretleri rezervasyonun kapsamında değildir.
- **MariaDB için Azure Veritabanı** - Rezervasyona yalnızca işlem maliyetleri dahildir. MariaDB Veritabanı sunucusuyla ilişkili yazılım, ağ veya depolama ücretleri rezervasyonun kapsamında değildir.
- **Azure Veri Gezgini** - Rezervasyon sair gider ücretlerini kapsar. Kümelerle ilişkili işlem, ağ veya depolama ücretleri rezervasyonun kapsamında değildir.
- **Azure Cache for Redis** - Rezervasyona yalnızca işlem maliyetleri dahildir. Redis önbellek örnekleriyle ilişkili ağ veya depolama ücretleri rezervasyonun kapsamında değildir.
- **Azure Ayrılmış Konak** - Ayrılmış konağa yalnızca işlem maliyetleri dahil edilir.
- **Azure Disk Depolama rezervasyonları** - Rezervasyon yalnızca P30 ve üzeri boyuta sahip olan premium SSD'leri kapsar. Diğer disk türlerini veya P30 altındaki boyutları kapsamaz.

Yazılım planları:

- **SUSE Linux** - Rezervasyon yazılım planı maliyetlerini kapsar. Yalnızca SUSE ölçümlerine indirim uygulanır; sanal makine kullanımına uygulanmaz.
- **Red Hat Planları** - Rezervasyon yazılım planı maliyetlerini kapsar. Yalnızca RedHat ölçümlerine indirim uygulanır; sanal makine kullanımına uygulanmaz.
- **CloudSimple Azure VMware Çözümü** - Rezervasyon VMWare CloudSimple Düğümlerini kapsar. Yine de ek yazılım ücretleri uygulanabilir.
- **Azure Red Hat OpenShift** - Rezervasyon OpenShift maliyetlerine uygulanır; Azure altyapı maliyetlerine uygulanmaz.

Windows sanal makineleri ve SQL Veritabanı için, yazılım maliyetlerine rezervasyon indirimi uygulanmaz. [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) ile lisanslama maliyetlerini kapsayabilirsiniz.


## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Aşağıdaki makalelerle Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinin:
    - [Azure Ayırmalarını yönetme](manage-reserved-vm-instance.md)
    - [Kullandıkça öde fiyatlarına tabi aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
    - [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
    - [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](reserved-instance-windows-software-costs.md)
    - [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](/partner-center/azure-reservations)

- Hizmet planları için rezervasyonlar hakkında daha fazla bilgi edinin:
    - [Azure Ayrılmış VM Örnekleri ile Sanal Makineler](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Azure Cosmos DB ayrılmış kapasitesine sahip Azure Cosmos DB kaynakları](../../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Azure SQL Veritabanı ayrılmış kapasitesine sahip SQL Veritabanı işlem kaynakları](../../sql-database/sql-database-reserved-capacity.md) Yazılım planlarına yönelik rezervasyonlar hakkında daha fazla bilgi edinin:
    - [Azure Ayrılmış Sanal Makine Örneklerinden Red Hat yazılım planları](../../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Azure Ayrılmış Sanal Makine Örneklerinden SESU yazılım planları](../../virtual-machines/linux/prepay-suse-software-charges.md)
