---
title: Azure Ayırmaları nedir?
description: Sanal makineleriniz, SQL veritabanlarınız, Azure Cosmos DB ve diğer kaynak maliyetlerinde tasarruf etmek için Azure Ayrılmış Sanal Makine Örnekleri ve fiyatlandırma hakkında bilgi edinin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: banders
ms.openlocfilehash: b41435cb0b337edc2d73820c75da847913326315
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84015990"
---
# <a name="what-are-azure-reservations"></a>Azure Ayırmaları nedir?

Azure Rezervasyonları birden çok ürün için bir yıllık veya üç yıllık planları kabul ederek tasarruf etmenize yardımcı olur. Taahhütte bulunarak kullandığınız kaynakları indirimli satın alabilirsiniz. Rezervasyonlar, kaynak maliyetlerinizi kullandıkça öde fiyatlarına kıyasla %72'ye varan oranda azaltabilir. Rezervasyonlar bir faturalandırma indirimi sağlar ve kaynaklarınızın çalışma zamanı durumunu etkilemez. Rezervasyon satın aldıktan sonra, eşleşen kaynaklara otomatik olarak indirim uygulanır.

Bir rezervasyonun ödemesini peşin veya aylık olarak yapabilirsiniz. Peşin ve aylık rezervasyonların toplam maliyeti aynıdır ve aylık ödemeyi seçtiğinizde ekstra ücret ödemezsiniz. Aylık ödeme, üçüncü taraf ürünleri için değil Azure rezervasyonları için kullanılabilir.

[https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) adresindeki Azure portalından da rezervasyon satın alabilirsiniz.

## <a name="why-buy-a-reservation"></a>Neden rezervasyon satın almalıyım?

Rezervasyonları destekleyen tutarlı bir kaynak kullanımınız varsa, bir rezervasyon satın almak size maliyetleri düşürme seçeneği getirir. Örneğin, bir hizmetin örneklerini rezervasyon olmadan sürekli olarak çalıştırdığınızda kullandıkça öde tarifesine göre ücretlendirilirsiniz. Rezervasyon satın aldığınız anda rezervasyon indirimini elde edersiniz. Kaynaklar artık kullandıkça öde tarifelerine göre ücretlendirilmez.

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Satın alma sonrasında, rezervasyon indirimi rezervasyonu satın alırken seçtiğiniz özniteliklerle eşleşen kaynak kullanımına otomatik olarak uygulanır. Öznitelikler SKU, bölgeler (uygun olduğunda) ve kapsamdan oluşur. Rezervasyon tasarruflarının nereye uygulanacağını rezervasyon kapsamı belirler.

İndirimin nasıl uygulanacağı hakkında daha fazla bilgi için bkz. [Ayrılmış örnek indirimini uygulama](reservation-discount-application.md).

Rezervasyon kapsamının nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Kapsam rezervasyonları](prepare-buy-reservation.md#scope-reservations).

## <a name="determine-what-to-purchase"></a>Neyin satın alınacağını saptama 

Azure Databricks dışındaki tüm rezervasyonlar saatlik olarak uygulanır. Tutarlı temel kullanımınızı temel alan rezervasyon satın alımlarınızı göz önünde bulundurun. Kullanım verilerinizi analiz ederek veya rezervasyon önerilerini kullanarak, satın alacağınız rezervasyonu belirleyebilirsiniz. Önerileri şurada bulabilirsiniz:

- Azure Danışmanı (yalnızca VM’ler)
- Azure portalında rezervasyon satın alma deneyimi
- Maliyet Yönetimi Power BI uygulaması
- API'ler 

Daha fazla bilgi için bkz.  [Satın alınacak rezervasyonu belirleme](determine-reservation-purchase.md) 

## <a name="buying-a-reservation"></a>Rezervasyon satın alma 

Azure portalından, API’lerden, PowerShell’den ve CLI’dan rezervasyon satın alabilirsiniz. 

Satın almak için Azure portalına gidin (https://ms.portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Docs) ) 

Daha fazla bilgi için bkz.  [Rezervasyon satın alma](prepare-buy-reservation.md) 

## <a name="how-is-a-reservation-billed"></a>Rezervasyon nasıl faturalandırılır? 

Rezervasyon ücreti, aboneliğe bağlı ödeme yöntemiyle tahsil edilir. Rezervasyon maliyeti, varsa parasal taahhüt bakiyenizden düşülür. Parasal taahhüt bakiyeniz rezervasyon maliyetini kapsamadığında fazla kullanım üzerinden faturalandırılırsınız. Kullandıkça öde tarifesine sahip tek bir plandan aboneliğiniz varsa, hesabınızda bulunan kredi kartından peşin alma işlemleri için hemen ücret alınır. Aylık ödemeler faturanızda görünür ve kredi kartınızdan aylık olarak ücret alınır. Fatura ile ücret aldığınızda ücretleri bir sonraki faturanızda görürsünüz. 

## <a name="permissions-to-view-and-manage-reservations"></a>Rezervasyonları görüntüleme ve yönetme izinleri 

Rezervasyon satın alan kullanıcı ve rezervasyonu faturalamak için kullanılan aboneliğin hesap yöneticisi, rezervasyon siparişinde ve rezervasyonun kendisinde Sahip rolü elde eder.

Rezervasyon siparişindeki veya rezervasyondaki rollere kullanıcı ekleyerek bir rezervasyonun yönetimi için temsilciler belirleyebilirsiniz. Rolleri Azure portalından veya API’leri ve PowerShell’i kullanarak atarsınız. 

Daha fazla bilgi için bkz.  [Rezervasyonu yönetebilecek kullanıcıları ekleme veya değiştirme](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation) 

## <a name="get-reservation-details-and-utilization-after-purchase"></a>Satın alma işleminden sonra rezervasyon ayrıntılarını ve kullanımını alma

Rezervasyonu görüntüleme izniniz varsa rezervasyonun kendisini ve kullanımını Azure portalında görebilirsiniz. Verileri, API’leri kullanarak da alabilirsiniz. 

Rezervasyonları Azure portalında görme hakkında daha fazla bilgi için bkz.  [Azure portalında rezervasyonları görüntüleme](view-reservations.md) 

## <a name="manage-reservations-after-purchase"></a>Satın alma sonrasında rezervasyonları yönetme 

Azure rezervasyonu satın aldıktan sonra, rezervasyonu farklı bir aboneliğe uygulamak için kapsamı güncelleştirebilir, rezervasyonu yöneten kişileri değiştirebilir, rezervasyonu daha küçük parçalara bölebilir veya örnek boyutu esnekliğini değiştirebilirsiniz. 

Daha fazla bilgi için bkz.  [Azure kaynakları için Rezervasyonları yönetme](manage-reserved-vm-instance.md) 

## <a name="flexibility-with-azure-reservations"></a>Azure rezervasyonlarıyla esneklik

Azure Ayrılmış Sanal Makine Örnekleri, değişen ihtiyaçlarınızın karşılanmasına yardımcı olma esnekliği sağlar. Bir rezervasyonu aynı türdeki başka bir rezervasyonla değiştirebilirsiniz. İhtiyaç duymadığınız rezervasyonları iade ederek de 12 aylık kayan pencerede 50.000 ABD dolarına kadar para iadesi alabilirsiniz. Para iadesi üst sınırı, Microsoft ile sözleşmenizin kapsamındaki tüm rezervasyonlar için geçerlidir.

Daha fazla bilgi için bkz. [Azure Rezervasyonları için self servis değişimler ve para iadeleri](exchange-and-refund-azure-reservations.md)


## <a name="charges-covered-by-reservation"></a>Rezervasyon kapsamındaki ücretler

- **Ayrılmış Sanal Makine Örneği** - Bir rezervasyon yalnızca sanal makine ve bulut hizmetleri işlem maliyetlerini içerir. Ek yazılım, Windows, ağ veya depolama ücretlerini kapsamaz.
- **Azure Depolama ayrılmış kapasitesi** - Bir rezervasyon, Blob depolama veya Azure Data Lake 2. Nesil depolama için standart depolama hesaplarının depolama kapasitesini kapsar. Rezervasyon, bant genişliğini veya işlem oranlarını kapsamaz.
- **Azure Cosmos DB ayrılmış kapasitesi** - Rezervasyon yalnızca kaynaklarınız için sağlanan aktarım hızını kapsar. Depolama ve ağ ücretlerini kapsamaz.
- **SQL veritabanı ayrılmış sanal çekirdeği** - Rezervasyona yalnızca işlem maliyetleri dahildir. SQL lisansı ayrı faturalandırılır.
- **Azure Synapse Analytics** - Rezervasyon cDWU kullanımını kapsar. Azure Synapse Analytics’in kullanımıyla ilişkili depolama ve ağ ücretlerini kapsamaz.
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
    - [Azure SQL Veritabanı ayrılmış kapasitesine sahip SQL Veritabanı işlem kaynakları](../../azure-sql/database/reserved-capacity-overview.md) Yazılım planlarına yönelik rezervasyonlar hakkında daha fazla bilgi edinin:
    - [Azure Ayrılmış Sanal Makine Örneklerinden Red Hat yazılım planları](../../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Azure Ayrılmış Sanal Makine Örneklerinden SESU yazılım planları](../../virtual-machines/linux/prepay-suse-software-charges.md)
