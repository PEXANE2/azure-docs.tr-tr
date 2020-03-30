---
title: Maliyetleri azaltın
description: Azure SQL Veritabanı'nı nasıl satın alacağınızı öğrenin bilgi işlem maliyetlerinizden tasarruf etmek için ayrılmış kapasite.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 08/29/2019
ms.openlocfilehash: 0713e1ed98cc87b95cad1f84672148cd062e0b58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979984"
---
# <a name="save-costs-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Azure SQL Veritabanı ayrılmış kapasiteye sahip SQL Veritabanı işlem kaynakları için maliyet tasarrufu

Azure SQL Veritabanı ile, kullanabileceğiniz kadar öde fiyatlarıyla karşılaştırıldığında işlem kaynakları için bir rezervasyon taahhüdünde bulunmak la paradan tasarruf edin. Azure SQL Veritabanı ayrılmış kapasite ile, işlem maliyetlerinde önemli bir indirim elde etmek için bir veya üç yıllık bir süre için SQL Veritabanı kullanımı için bir taahhütte bulunabilirsiniz. SQL Veritabanı ayrılmış kapasitesatın almak için Azure bölgesini, dağıtım türünü, performans katmanını ve terimini belirtmeniz gerekir.


Rezervasyonu belirli SQL Veritabanı örneklerine (tek veritabanları, elastik havuzlar veya yönetilen örnekler) atamanız gerekmez. Zaten çalışan veya yeni dağıtılan SQL Veritabanı örneklerini eşleştirme, otomatik olarak avantaj elde eder. Bir rezervasyon satın alarak, bir veya üç yıllık bir süre için bilgi işlem maliyetleri için kullanım taahhüt eder. Bir rezervasyon satın alır almaz, rezervasyon öznitelikleriyle eşleşen SQL Veritabanı işlem ücretleri artık kullandıkça öde fiyatlarıüzerinden ücretlendirilmez. Rezervasyon, SQL Veritabanı örneğiyle ilişkili yazılım, ağ veya depolama ücretlerini kapsamaz. Rezervasyon döneminin sonunda, faturalandırma avantajı sona erer ve SQL Veritabanları size ödeme fiyatından faturalandırılır. Rezervasyonlar otomatik olarak yenilemez. Fiyatlandırma bilgileri için SQL [Veritabanı ayrılmış kapasite teklifine](https://azure.microsoft.com/pricing/details/sql-database/managed/)bakın.

[Azure portalında](https://portal.azure.com)Azure SQL Veritabanı rezerve edilen kapasiteyi satın alabilirsiniz. [Peşin olarak veya aylık ödemelerle](../cost-management-billing/reservations/monthly-payments-reservations.md) rezervasyon ödemesi yapın. SQL Veritabanı ayrılmış kapasite satın almak için:

- Kullandıkça öde oranlarına sahip en az bir Kurumsal veya bireysel abonelik için sahip rolünde olmalısınız.
- Kurumsal abonelikler için, [EA portal](https://ea.azure.com)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Veya bu ayar devre dışı bırakılmışsa, abonelikte bir EA Yöneticisi olmalısınız.
- Bulut Çözüm Sağlayıcısı (CSP) programı için, yalnızca yönetici aracılar veya satış aracıları SQL Veritabanı ayrılmış kapasite satın alabilirsiniz.

Kurumsal müşterilerin ve Sizden Önce Öde müşterilerinin rezervasyon satın alımları için nasıl ücretlendirildikine ilişkin ayrıntılar, [Kurumsal kaydınız için Azure rezervasyon kullanımını anlayın](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md) ve [Kullan-Kaç'a Göre Aboneliğiniz için Azure rezervasyon kullanımını anlayın.](../cost-management-billing/reservations/understand-reserved-instance-usage.md)

## <a name="determine-the-right-sql-size-before-purchase"></a>Satın almadan önce doğru SQL boyutunu belirleme

Rezervasyonun boyutu, belirli bir bölge içinde varolan veya yakında dağıtılacak tek veritabanları, elastik havuzlar veya yönetilen örnekler tarafından kullanılan ve aynı performans katmanı ve donanım oluşturma yı kullanan toplam işlem miktarına dayanmalıdır.

Örneğin, bir genel amaç, Gen5 - 16 vCore elastik havuz ve iki iş kritik, Gen5 - 4 vCore tek veritabanları çalıştırdığınızı varsayalım. Ayrıca, önümüzdeki ay içinde ek bir genel amaç, Gen5 - 16 vCore elastik havuz ve bir iş kritik, Gen5 - 32 vCore elastik havuz dağıtmak için plan varsayalım. Ayrıca, en az 1 yıl boyunca bu kaynaklara ihtiyacınız olacağını bildiğinizi varsayalım. Bu durumda, bir 32 (2x16) vCores, tek veritabanı / elastik havuz genel amaç için 1 yıl rezervasyon - Gen5 ve 40 (2x4 + 32) vCore 1 tek veritabanı / elastik havuz iş kritik için rezervasyon - Gen5 satın almalısınız.

## <a name="buy-sql-database-reserved-capacity"></a>SQL Veritabanı ayrılmış kapasitesi satın alma

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Tüm hizmetler** > **rezervasyonlarını**seçin.
3. **Satınalma** rezervasyonları bölmesinde Ekle'yi seçin ve ardından SQL Veritabanı için yeni bir rezervasyon satın almak için **SQL Veritabanı'nı** seçin.
4. Gerekli alanları doldurun. Varolan veya yeni tek veritabanları, elastik havuzlar veya seçtiğiniz özniteliklerle eşleşen yönetilen örnekler ayrılmış kapasite indirimini almaya hak kazanır. İndirimi alan SQL Veritabanı örneklerinizin gerçek sayısı, seçilen kapsam ve miktara bağlıdır.
    ![SQL Veritabanı rezerve kapasite satın alma göndermeden önce ekran görüntüsü](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

Aşağıdaki tabloda gerekli alanları açıklanmaktadır.

| Alan      | Açıklama|
|------------|--------------|
|Abonelik|SQL Veritabanı için ödeme yapmak için kullanılan abonelik kapasite rezervasyonu rezerve etti. Abonelikteki ödeme yöntemi, SQL Veritabanı rezerve edilen kapasite rezervasyonu için ön maliyetlerden tahsil edilir. Abonelik türü bir kurumsal sözleşme (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya gidilecek kadar öde fiyatlandırması (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) ile bireysel bir anlaşma olmalıdır. Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Gittikçe öde fiyatlandırmasına sahip tek bir abonelik için, ücretler abonelikteki kredi kartına veya fatura ödeme yöntemine faturalandırılır.|
|Kapsam       |vCore rezervasyonun kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Seçerseniz: <br/><br/>**Paylaşılan,** vCore rezervasyon indirimi, faturalandırma bağlamınızdaki tüm aboneliklerde çalışan SQL Veritabanı örneklerine uygulanır. Kurumsal müşteriler için paylaşılan kapsam kayıttır ve kayıt içindeki tüm abonelikleri içerir. Kullandıkça Öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan tüm Kullandıkça Öde abonelikleridir.<br/><br/>**Tek abonelik,** vCore rezervasyon indirimi bu abonelikteki SQL Veritabanı örneklerine uygulanır. <br/><br/>**Tek kaynak grubu,** rezervasyon indirimi seçilen abonelikteki SQL Veritabanı örneklerine ve bu abonelik içindeki seçili kaynak grubuna uygulanır.|
|Bölge      |SQL Veritabanı tarafından kapsanan Azure bölgesi kapasite rezervasyonu ayrılmıştır.|
|Dağıtım Türü|Rezervasyon satın almak istediğiniz SQL kaynak türü.|
|Performans Katmanı|SQL Veritabanı örnekleri için hizmet katmanı.
|Sözleşme Dönemi        |Bir ya da üç yıl.|
|Miktar    |SQL Veritabanı'nda satın alınan işlem kaynaklarının miktarı kapasite rezervasyonu nu rezerve etti. Miktar, seçili Azure bölgesinde ve Performans katmanında rezerve edilen ve faturalandırma indirimini alacak bir dizi vCore'dur. Örneğin, Doğu ABD bölgesinde Gen5 16 vCores'un toplam bilgi işlem kapasitesine sahip SQL Veritabanı örneklerini çalıştırıyor veya çalıştırmayı planlıyorsanız, tüm örneklerin yararını en üst düzeye çıkarmak için miktarı 16 olarak belirtirsiniz. |

1. **Maliyetler** bölümünde SQL Veritabanı rezerve edilen kapasite rezervasyonunun maliyetini gözden geçirin.
1. **Satın al**'ı seçin.
1. Satın alma durumunu görmek için **bu Rezervasyonu Görüntüle'yi** seçin.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

## <a name="vcore-size-flexibility"></a>vCore boyut esnekliği

vCore boyut esnekliği, ayrılmış kapasite avantajını kaybetmeden bir performans katmanı ve bölge içinde yukarı veya aşağı ölçeklendirmenize yardımcı olur. SQL Veritabanı ayrılmış kapasite de ayrılmış kapasite kaybetmeden normal işlemlerin bir parçası olarak (aynı bölge ve performans katmanı içinde) havuzları ve tek veritabanları arasında geçici olarak sıcak veritabanları taşımak için esneklik sağlar Yarar. Rezervasyonunuzda uygulanmamış bir arabellek tutarak, bütçenizi aşmadan performans artışlarını etkin bir şekilde yönetebilirsiniz.

## <a name="limitation"></a>Sınırlama

DTU tabanlı (temel, standart veya premium) SQL veritabanlarını rezerve edemezsiniz.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

## <a name="next-steps"></a>Sonraki adımlar

vCore rezervasyon iskontosu, SQL Veritabanı ayrılmış kapasite ayırma kapsamı ve öznitelikleriyle eşleşen SQL Veritabanı örneklerinin sayısına otomatik olarak uygulanır. SQL Veritabanı rezerve edilen kapasite rezervasyonunun kapsamını [Azure portalı,](https://portal.azure.com)PowerShell, CLI veya API üzerinden güncelleştirebilirsiniz.

SQL Veritabanı ayrılmış kapasite rezervasyonlarını nasıl yöneteceklerini öğrenmek için [bkz.](../cost-management-billing/reservations/manage-reserved-vm-instance.md)

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Rezervasyonları nedir?](../cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Azure Ayırmalarını yönetme](../cost-management-billing/reservations/manage-reserved-vm-instance.md)
- [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](../cost-management-billing/reservations/understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)
- [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)
