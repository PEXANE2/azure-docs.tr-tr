---
title: Ayrılmış işlem fiyatlandırması-PostgreSQL için Azure veritabanı-hiper ölçek (Citus)
description: PostgreSQL için Azure veritabanı-hiper ölçek (Citus) için ön ödeme ayrılmış kapasiteye sahip işlem kaynakları
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: dcf38a3bcd41ba41bb7cf3b16d022a7a45734c4a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85218071"
---
# <a name="prepay-for-azure-database-for-postgresql---hyperscale-citus-compute-resources-with-reserved-capacity"></a>PostgreSQL için Azure veritabanı-hiper ölçek (Citus) için ön ödeme ayrılmış kapasiteye sahip işlem kaynakları

PostgreSQL için Azure veritabanı – Hyperscale (Citus), Kullandıkça Öde fiyatlarına kıyasla işlem kaynakları için ön ödeme yaparak paradan tasarruf etmenize yardımcı olur. Hyperscale (Citus) ayrılmış kapasitesi ile, işlem maliyetlerine göre önemli bir indirim elde etmek için bir veya üç yıllık dönem için Hiperscale (Citus) sunucu grubu üzerinde ön taahhüt taahhüdünü yaparsınız. Hiperscale (Citus) ayrılmış kapasitesini satın almak için Azure bölgesini, rezervasyon terimini ve faturalandırma sıklığını belirtmeniz gerekir.

> [!IMPORTANT]
> Bu sayfa, PostgreSQL için Azure veritabanı – hiper ölçek (Citus) için ayrılmış kapasiteye yöneliktir. PostgreSQL için Azure veritabanı – tek sunucu için ayrılmış kapasite hakkında bilgi için [Bu sayfaya](/azure/postgresql/concept-reserved-pricing) bakın.

Ayırmayı belirli bir hiper ölçek (Citus) sunucu gruplarına atamanız gerekmez. Zaten çalışan bir hiper ölçek (Citus) sunucu grubu veya yeni dağıtılmış olanlar, ayrılmış fiyatlandırma avantajını otomatik olarak alır. Bir rezervasyon satın alarak, bir veya üç yıllık bir dönem için işlem maliyetleri için ön ödeme yaparsınız. Bir ayırma satın alarak, rezervasyon öznitelikleriyle eşleşen Hiperscale (Citus) işlem ücretleri artık Kullandıkça Öde tarifelerine göre ücretlendirilir. Bir ayırma, Hyperscale (Citus) sunucu gruplarıyla ilişkili yazılım, ağ veya depolama ücretlerini kapsamaz. Rezervasyon döneminin sonunda, faturalandırma avantajı sona erer ve Hiperscale (Citus) sunucu grupları Kullandıkça Öde fiyatı üzerinden faturalandırılır. Ayırmalar autorenew değildir. Fiyatlandırma bilgileri için bkz. [PostgreSQL Için Azure veritabanı – Hyperscale (Citus) ayrılmış kapasite teklifi](https://azure.microsoft.com/pricing/details/postgresql/hyperscale-citus/).

[Azure Portal](https://portal.azure.com/)hiper ölçek (Citus) ayrılmış kapasitesini satın alabilirsiniz. [Peşin olarak veya aylık ödemelerle](https://docs.microsoft.com/azure/cost-management-billing/reservations/monthly-payments-reservations) rezervasyon ödemesi yapın. Ayrılmış kapasiteyi satın almak için:

* Kullandıkça Öde tarifesine sahip en az bir kuruluş veya ayrı bir abonelik için sahip rolünde olmanız gerekir.
* Kurumsal abonelikler için, [EA portal](https://ea.azure.com/)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Ya da bu ayar devre dışı bırakılırsa, abonelikte bir EA yöneticisi olmanız gerekir.
* Bulut çözümü sağlayıcısı (CSP) programı için, yalnızca yönetici aracıları veya satış aracıları, hiper ölçek (Citus) ayrılmış kapasitesini satın alabilir.

Kurumsal müşterilerin ve kullandıkça öde müşterilerinin rezervasyon satın alma işlemleri için nasıl ücretlendirilildiği hakkında daha fazla bilgi için bkz. [Kurumsal kaydınız Için Azure ayırma kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) ve [Kullandıkça Öde aboneliğiniz için Azure rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage).

## <a name="determine-the-right-server-group-size-before-purchase"></a>Satın almadan önce doğru sunucu grubu boyutunu belirle

Ayırma boyutu, belirli bir bölgedeki Hiperscale (Citus) sunucu gruplarındaki mevcut veya hemen dağıtılmış düzenleyici ve çalışan düğümleri tarafından kullanılan işlem Toplam miktarına bağlı olmalıdır.

Örneğin, 16 sanal çekirdek Düzenleyicisi ve üç 8 sanal çekirdek çalışan düğümü ile bir hiper ölçek (Citus) sunucu grubu çalıştırdığınızı varsayalım. Ayrıca, bir sonraki ay içinde, 32 sanal çekirdek Düzenleyicisi ve iki 4 sanal çekirdek çalışan düğümü ile ek bir hiper ölçek (Citus) sunucu grubu dağıtmayı planladığınızı varsayalım. Bu kaynaklara en az bir yıl boyunca ihtiyacınız olacağını varsayalım.

Bu durumda, için bir yıllık rezervasyon satın almalısınız

* Total 16 Vçekirdeler + 32 sanal çekirdek = 48 sanal çekirdekler düğümleri
* Toplam üç düğüm x 8 sanal çekirdek + 2 düğüm x 4 sanal çekirdek = 24 + 8 = 32 sanal çekirdek çalışan düğümleri

## <a name="buy-azure-database-for-postgresql-reserved-capacity"></a>PostgreSQL için Azure veritabanı ayrılmış kapasitesi satın alın

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm hizmetler** > **Rezervasyonlar**’ı seçin.
3. **Ekle** ' yi seçin ve ardından satın alma rezervasyonları bölmesinde PostgreSQL **için Azure veritabanı** ' nı seçerek PostgreSQL veritabanlarınız için yeni bir ayırma satın alın.
4. Satın almak için Hyperscale (Citus) Işlem türünü seçin ve **Seç** düğmesine tıklayın.
5. **Ürünler** sekmesinde seçilen işlem türü için miktarı gözden geçirin.
4. Satın alımınızın tamamlanmasını sağlamak için **satın al + gözden geçir** sekmesine geçin.

Aşağıdaki tablo gerekli alanları açıklar.

| Alan        | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Abonelik | PostgreSQL için Azure veritabanı için ödeme yapmak üzere kullanılan abonelik ayrılmış kapasite ayırması. Abonelik üzerindeki ödeme yöntemi, PostgreSQL için Azure veritabanı ayrılmış kapasite rezervasyonuna ait ön maliyetlere göre ücretlendirilir. Abonelik türü bir kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya kullandıkça öde fiyatlandırması (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) içeren tek bir anlaşma olmalıdır. Kurumsal bir abonelik için ücretler, kayıt parasal taahhüt bakiyesinden düşülür veya fazla kullanım olarak ücretlendirilir. Kullandıkça Öde fiyatlandırmasına sahip bireysel bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır.                                                                                  |
| Kapsam        | VCore rezervasyonunun kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Şunları seçerseniz: <br><br> **,** Sanal çekirdek ayırma indirimi, faturalandırma bağlamınızın içindeki aboneliklerde çalışan hiper ölçek (Citus) sunucu gruplarına uygulanır. Kurumsal müşteriler için, paylaşılan kapsam kayıt içindedir ve kayıt dahilindeki tüm abonelikleri içerir.  Kullandıkça Öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan tüm Kullandıkça Öde abonelikleridir. <br><br> **Tek abonelik,** sanal çekirdek ayırma indirimi Bu abonelikteki hiper ölçek (Citus) sunucu gruplarına uygulanır. <br><br> **Tek kaynak grubu,** ayırma indirimi seçili abonelikteki hiper ölçek (Citus) sunucu gruplarına ve bu abonelik içindeki seçili kaynak grubuna uygulanır. |
| Bölge       | PostgreSQL için Azure veritabanı – Hyperscale (Citus) ayrılmış kapasite rezervasyonu kapsamındaki Azure bölgesi.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Terim         | Bir yıllık veya üç yıllık.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Miktar     | Hiperscale (Citus) ayrılmış kapasite rezervasyonu içinde satın alınan işlem kaynakları miktarı. Özellikle, ayrılan ve fatura iskontosunu alacak olan seçili Azure bölgesindeki düzenleyici veya çalışan düğümü sanal çekirdekleri sayısı. Örneğin, Doğu ABD bölgesinde 64 düzenleyici düğümü sanal çekirdekleri ve 32 çalışan düğümü sanal çekirdekleri için toplam işlem kapasitesine sahip hiper ölçek (veya çalıştırmayı planlama) çalıştırıyorsanız, tüm sunucuların avantajını en üst düzeye çıkarmak için sırasıyla, Koordinatör ve çalışan düğümleri için 64 ve 32 olarak belirtirsiniz.                                                                                                                                                                                                                                                     |



## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="vcore-size-flexibility"></a>vCore boyutu esnekliği

vCore boyutu esnekliği, ayrılmış kapasite avantajını kaybetmeden bir bölgedeki düzenleyiciyi ve çalışan düğümlerini ölçeklendirmenize yardımcı olur.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

VCore ayırma indirimi, PostgreSQL için Azure veritabanı ayrılmış kapasite ayırma kapsamı ve öznitelikleri ile eşleşen hiper ölçek (Citus) sunucu grubu sayısına otomatik olarak uygulanır. PostgreSQL için Azure veritabanı – Hyperscale (Citus) ayrılmış kapasite ayırmasının kapsamını Azure portal, PowerShell, CLı veya API aracılığıyla güncelleştirebilirsiniz.

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Ayrılmış Sanal Makine Örnekleri nedir?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
* [Azure Ayırmalarını yönetme](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
* [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
* [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-postgresql)
* [Kurumsal kaydınız için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
* [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)
