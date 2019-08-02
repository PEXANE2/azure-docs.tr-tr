---
title: Para tasarrufu sağlamak için Azure SQL veritabanı sanal çekirdekleri için ön ödeme | Microsoft Docs
description: İşlem maliyetlerinizi kaydetmek için Azure SQL veritabanı ayrılmış kapasitesini nasıl satın alacağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 07/19/2019
ms.openlocfilehash: adbc2c8818a3d7eff679dadc2245fb0ff8686fc2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566931"
---
# <a name="prepay-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Azure SQL veritabanı ayrılmış kapasitesi ile SQL veritabanı işlem kaynakları için ön ödeme

Kullandıkça Öde fiyatlarıyla karşılaştırıldığında işlem kaynaklarını önceden ödeyerek Azure SQL veritabanı ile tasarruf edin. Azure SQL veritabanı ayrılmış kapasitesi ile, işlem maliyetlerine göre önemli bir indirim elde etmek için SQL veritabanı 'nda bir veya üç yıllık bir dönem için ön taahhüt yaparsınız. SQL veritabanı ayrılmış kapasitesini satın almak için Azure bölgesini, dağıtım türünü, performans katmanını ve terimi belirtmeniz gerekir.


Ayırmayı belirli SQL veritabanı örneklerine (tek veritabanları, elastik havuzlar veya yönetilen örnekler) atamanız gerekmez. Zaten çalışmakta olan veya yeni dağıtılmış olan eşleşen SQL veritabanı örnekleri, otomatik olarak avantajı alır. Bir rezervasyon satın alarak, bir veya üç yıllık bir dönem için işlem maliyetleri için ön ödeme yaparsınız. Bir ayırma satın alarak, rezervasyon öznitelikleriyle eşleşen SQL veritabanı işlem ücretleri artık Kullandıkça Öde tarifelerine göre ücretlendirilir. Bir ayırma, SQL veritabanı örneğiyle ilişkili yazılım, ağ veya depolama ücretlerini kapsamaz. Rezervasyon döneminin sonunda, faturalandırma avantajı sona erer ve SQL veritabanları Kullandıkça Öde fiyatı üzerinden faturalandırılır. Ayırmalar otomatik olarak yenilemez. Fiyatlandırma bilgileri için bkz. [SQL veritabanı ayrılmış kapasitesi teklifi](https://azure.microsoft.com/pricing/details/sql-database/managed/).

[Azure Portal](https://portal.azure.com)Azure SQL veritabanı ayrılmış kapasitesini satın alabilirsiniz. SQL veritabanı ayrılmış kapasitesini satın almak için:

- Kullandıkça Öde tarifesine sahip en az bir kuruluş veya ayrı bir abonelik için sahip rolünde olmanız gerekir.
- Kurumsal abonelikler için, **ayrılmış örneklerin eklenmesi** [EA portalında](https://ea.azure.com)etkinleştirilmelidir. Ya da bu ayar devre dışı bırakılırsa, abonelikte bir EA yöneticisi olmanız gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için, yalnızca yönetici aracıları veya satış aracıları SQL veritabanı ayrılmış kapasitesi satın alabilir.

Kurumsal müşterilerin ve kullandıkça öde müşterilerinin, rezervasyon satın alma işlemleri için nasıl ücretlendirilildiği hakkındaki ayrıntılar, bkz. [Kurumsal kaydınız Için Azure ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage-ea.md) ve [Kullandıkça Öde için Azure rezervasyon kullanımını anlama abonelik](../billing/billing-understand-reserved-instance-usage.md).

## <a name="determine-the-right-sql-size-before-purchase"></a>Satın almadan önce doğru SQL boyutunu belirleme

Ayırma boyutu, mevcut veya hemen dağıtılmış tek veritabanları, elastik havuzlar veya belirli bir bölgedeki yönetilen örnekler tarafından kullanılan ve aynı performans katmanını ve donanım üretimini kullanan toplam işlem miktarına bağlı olmalıdır.

Örneğin, bir genel amaç, 5. nesil – 16 sanal çekirdek elastik havuzu ve iki iş açısından kritik, 5. nesil – 4 sanal çekirdek tek veritabanı çalıştırdığınızı varsayalım. Ayrıca, sonraki ayda, ek bir genel amaç, 5. nesil – 16 sanal çekirdek elastik havuzu ve bir iş açısından kritik, 5. nesil – 32 sanal çekirdek elastik havuzu dahilinde dağıtmayı planladığınızı varsayalım. Ayrıca, bu kaynaklara en az 1 yıl boyunca ihtiyacınız olacağını bildiğinizi varsayalım. Bu durumda, tek veritabanı/elastik havuz iş açısından kritik-5. nesil için bir 32 (2x16) Vcore, tek veritabanı/elastik havuz genel amaçlı-5. nesil ve 40 (2x4 + 32) sanal çekirdek 1 yıl ayırması satın almanız gerekir.

## <a name="buy-sql-database-reserved-capacity"></a>Ayrılmış SQL veritabanı kapasitesini satın alma

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm hizmet** > **ayırmaları**' ni seçin.
3. **Ekle** ' yi seçin ve ardından satın alma rezervasyonları bölmesinde SQL veritabanı ' **nı seçerek SQL** veritabanı için yeni bir ayırma satın alın.
4. Gerekli alanları doldur. Mevcut veya yeni tek veritabanları, elastik havuzlar veya bir ayrılmış kapasite indirimi elde etmek üzere seçtiğiniz özniteliklerle eşleşen yönetilen örnekler. İndirimin alınacağı SQL veritabanı örneklerinizin gerçek sayısı, seçilen kapsama ve miktara bağlıdır.
    ![SQL veritabanı ayrılmış kapasite satın alımını göndermeden önce ekran görüntüsü](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

Aşağıdaki tablo gerekli alanları açıklar.

| Alan      | Açıklama|
|------------|--------------|
|Subscription|SQL veritabanı ayrılmış kapasite ayırması için ödeme yapmak üzere kullanılan abonelik. Abonelik üzerindeki ödeme yöntemi, SQL veritabanı ayrılmış kapasite ayırması için ön maliyetler üzerinden ücretlendirilir. Abonelik türü bir kurumsal anlaşma olmalıdır (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya kullandıkça öde fiyatlandırmasıyla tek bir anlaşma (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P). Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde fiyatlandırmasına sahip bireysel bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır.|
|`Scope`       |VCore rezervasyonunun kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Şunları seçerseniz: <br/><br/>**, Sanal**çekirdek ayırma indirimi, faturalandırma bağlamınızın içindeki aboneliklerde çalışan SQL veritabanı örneklerine uygulanır. Kurumsal müşteriler için, paylaşılan kapsam kayıt içindedir ve kayıt dahilindeki tüm abonelikleri içerir. Kullandıkça Öde müşterileri için, paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan tüm Kullandıkça Öde aboneliklerdir.<br/><br/>**Tek abonelik**, sanal çekirdek ayırma indirimi Bu abonelikteki SQL veritabanı örneklerine uygulanır. <br/><br/>**Tek kaynak grubu**, rezervasyon indirimi SEÇILI abonelikteki SQL veritabanı örneklerine ve bu abonelikte seçili kaynak grubuna uygulanır.|
|Bölge      |SQL veritabanı ayrılmış kapasite rezervasyonu kapsamındaki Azure bölgesi.|
|Dağıtım Türü|Ayırmasını satın almak istediğiniz SQL kaynak türü.|
|Performans Katmanı|SQL veritabanı örnekleri için hizmet katmanı.
|Terim        |Bir yıl veya üç yıl.|
|Miktar    |SQL veritabanı ayrılmış kapasite ayırma içinde satın alınan işlem kaynakları miktarı. Bu miktar, seçili Azure bölgesindeki ve performans katmanındaki, ayrılan ve fatura iskontosunu alacak olan sanal çekirdekler sayısıdır. Örneğin, Doğu ABD bölgesinde 5. nesil 16 sanal çekirdeklerin toplam işlem kapasitesine sahip SQL veritabanı örneklerini çalıştırmayı planlıyorsanız veya planlarken, tüm örneklerin avantajını en üst düzeye çıkarmak için miktarı 16 olarak belirtirsiniz. |

1. **Maliyetler** bölümünde SQL veritabanı ayrılmış kapasite rezervasyonunun maliyetini gözden geçirin.
1. **Satın al**'ı seçin.
1. Satın alımınızın durumunu görmek için **Bu ayırmayı görüntüle** ' yi seçin.

## <a name="cancellations-and-exchanges"></a>İptaller ve alışverişlerde

SQL veritabanı ayrılmış kapasite ayırmayı iptal etmeniz gerekirse,% 12 erken sonlandırma ücreti olabilir. Para iadeleri satın aldığınız fiyattan veya geçerli rezervasyon fiyatından düşük olana göre hesaplanır. Para iadesi yıllık 50.000 ABD doları ile sınırlıdır. Yapılacak para iadesi, %12 erken sonlandırma ücreti kesildikten sonra kalan kullanım süresine göre hesaplanır. İptal etmek için Azure portal rezervasyonuna gidin ve **para iadesi**' ni seçin.

SQL Veritabanı yedek kapasite rezervasyonunuzun bölgesini, dağıtım türünü, performans katmanını veya süresini değiştirmeniz gerekiyorsa aynı ya da daha yüksek maliyete sahip olan başka bir rezervasyonla değiştirebilirsiniz. Yeni ayırma işleminin başlangıç tarihi değiştirilen ayırma işleminin başlangıç tarihiyle aynı olmaz. 1 veya 3 yıllık süre, yeni ayırma işlemini oluşturduğunuz tarihten itibaren başlatılır. Exchange için Azure portal rezervasyon bölümüne gidin ve **Exchange**' i seçin.

Rezervasyonları Exchange veya para iadesi hakkında daha fazla bilgi için bkz. [rezervasyon alışverişi ve para](../billing/billing-azure-reservations-self-service-exchange-and-refund.md)iadesi.

## <a name="vcore-size-flexibility"></a>vCore boyutu esnekliği

vCore boyutu esnekliği, ayrılan kapasite avantajını kaybetmeden bir performans katmanı ve bölgesi içinde ölçeği büyütme veya küçültme olanakları sağlar. SQL veritabanı ayrılmış kapasitesi Ayrıca, etkin veritabanlarınızı, normal işlemlerinizin (aynı bölge ve performans katmanı dahilinde) bir parçası olarak, ayrılan kapasiteyi kaybetmeden geçici olarak havuzlar halinde taşıma esnekliği sağlar sağlar. Ayırdığınızda uygulanmamış bir arabelleği tutarak, bütçenize aşmadan performans artışlarını etkin bir şekilde yönetebilirsiniz.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

VCore ayırma indirimi, SQL veritabanı ayrılmış kapasite ayırma kapsamı ve öznitelikleriyle eşleşen SQL veritabanı örneklerinin sayısına otomatik olarak uygulanır. [Azure Portal](https://portal.azure.com), POWERSHELL, CLı veya API aracılığıyla SQL veritabanı ayrılmış kapasite ayırmasının kapsamını güncelleştirebilirsiniz.

SQL veritabanı ayrılmış kapasite ayırmayı yönetme hakkında bilgi edinmek için bkz. [SQL veritabanı ayrılmış kapasitesini yönetme](../billing/billing-manage-reserved-vm-instance.md).

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](../billing/billing-save-compute-costs-reservations.md)
- [Azure Ayırmalarını yönetme](../billing/billing-manage-reserved-vm-instance.md)
- [Azure ayırmaları indirimi 'nı anlama](../billing/billing-understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlayın](../billing/billing-understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlayın](../billing/billing-understand-reserved-instance-usage-ea.md)
- [Iş Ortağı Merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmaları](https://docs.microsoft.com/partner-center/azure-reservations)
