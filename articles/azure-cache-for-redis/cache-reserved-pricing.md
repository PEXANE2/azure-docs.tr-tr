---
title: Ayrılmış kapasiteye sahip işlem için ön ödeme-Redsıs için Azure önbelleği
description: Ayrılmış kapasiteye sahip Redsıs işlem kaynakları için Azure önbelleği için ön ödeme
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: aded023c9f4c045f612e33d32c1e3ac71afddf02
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77530308"
---
# <a name="prepay-for-azure-cache-for-redis-compute-resources-with-reserved-capacity"></a>Ayrılmış kapasiteye sahip Redsıs işlem kaynakları için Azure önbelleği için ön ödeme

Redo için Azure Cache, Kullandıkça Öde fiyatlarıyla karşılaştırıldığında işlem kaynakları için ön ödeme yaparak paradan tasarruf etmenize yardımcı olur. Redin ayrılmış kapasitesi için Azure önbelleği ile, işlem maliyetlerine göre önemli bir indirim elde etmek için bir veya üç yıllık dönemde ön ödemeli bir taahhüt yaparsınız. Redsıs ayrılmış kapasitesi için Azure önbelleği satın almak üzere Azure bölgesini, hizmet katmanını ve terimi belirtmeniz gerekir.

Redsıs örnekleri için ayırmayı belirli Azure önbelleğine atamanız gerekmez. Zaten Redsıs veya yeni dağıtılan bir Azure önbelleği, ayrılmış önbelleğin büyüklüğüne kadar, ayrılmış fiyatlandırma avantajını otomatik olarak alır. Bir rezervasyon satın alarak, bir veya üç yıllık bir dönem için işlem maliyetleri için ön ödeme yaparsınız. Bir rezervasyon satın alarak, rezervasyon öznitelikleriyle eşleşen Redsıs işlem ücretleri için Azure önbelleği artık Kullandıkça Öde tarifelerine göre ücretlendirilir. Bir ayırma, önbellekle ilişkili ağ veya depolama ücretlerini kapsamaz. Rezervasyon döneminin sonunda, faturalandırma avantajı sona erer ve redin için Azure önbelleği, Kullandıkça Öde fiyatı üzerinden faturalandırılır. Ayırmalar otomatik olarak yenilemez. Fiyatlandırma bilgileri için bkz. [Red, ayrılmış kapasite teklifi Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache).

[Azure Portal](https://portal.azure.com/)redsıs ayrılmış kapasitesi Için Azure önbelleği satın alabilirsiniz. Ayrılmış kapasiteyi satın almak için:

* Kullandıkça Öde tarifesine sahip en az bir kuruluş veya ayrı bir abonelik için sahip rolünde olmanız gerekir.
* Kurumsal abonelikler için, [EA portal](https://ea.azure.com/)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Ya da bu ayar devre dışı bırakılırsa, abonelikte bir EA yöneticisi olmanız gerekir.
* Bulut çözümü sağlayıcısı (CSP) programı için yalnızca yönetici aracıları veya satış aracıları, Red, ayrılmış kapasite için Azure önbelleği satın alabilir.

Kurumsal müşterilerin ve kullandıkça öde müşterilerinin rezervasyon satın alma işlemleri için nasıl ücretlendirilildiği hakkındaki ayrıntılar için bkz. [Kurumsal kaydınız Için Azure ayırma kullanımını anlayın](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) ve [Kullandıkça Öde aboneliğiniz için Azure rezervasyon kullanımını anlayın](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage).


## <a name="determine-the-right-cache-size-before-purchase"></a>Satın almadan önce doğru önbellek boyutunu belirle

Ayırma boyutu, belirli bir bölgedeki ve aynı hizmet katmanını kullanan, var olan veya hemen dağıtılmış bir önbellek tarafından kullanılan işlem Toplam miktarına bağlı olmalıdır.

Örneğin, bir genel amaç, 5. nesil – 32 sanal çekirdek önbelleği ve iki bellek için iyileştirilmiş, 5. nesil – 16 sanal çekirdek önbelleğinizi kullandığınızı varsayalım. Ayrıca, bir sonraki ay içinde, ek bir genel amaç, 5. nesil – 32 sanal çekirdek veritabanı sunucusu ve bir bellek için iyileştirilmiş, 5. nesil – 16 sanal çekirdek veritabanı sunucusu arasında dağıtmayı planladığınızı varsayalım. En az 1 yılda bu kaynaklara ihtiyacınız olacağını bildiğinizi varsayalım. Bu durumda, tek veritabanı belleği için iyileştirilmiş-5. nesil için 64 (2x32) sanal çekirdekleri, tek veritabanı için 1 yıl ayırma genel amaçlı-5. nesil ve 48 (2x16 + 16) sanal çekirdek 1 yıl ayırması satın almalısınız


## <a name="buy-azure-cache-for-redis-reserved-capacity"></a>Redsıs ayrılmış kapasitesi için Azure önbelleği satın alın

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm hizmet** > **ayırmaları**' ni seçin.
3. **Ekle** ' yi seçin ve ardından satın alma rezervasyonları bölmesinde önbelleğiniz için yeni bir ayırma satın almak üzere **Redsıs için Azure önbelleği** ' ni seçin.
4. Gerekli alanları doldur. ' Yi seçtiğiniz özniteliklerle eşleşen mevcut veya yeni veritabanları, ayrılmış kapasite indirimi elde etmek için nitelendir ' ı seçin. İndirimin alınacağı Redsıs örnekleri için Azure önbelleğinizin gerçek sayısı, seçilen kapsama ve miktara bağlıdır.


![Ayrılmış fiyatlandırmaya genel bakış](media/cache-reserved-pricing/cache-reserved-price.png)


Aşağıdaki tablo gerekli alanları açıklar.

| Alan | Açıklama |
| :------------ | :------- |
| Abonelik   | Reda için Azure önbelleği için ödeme yapmak üzere kullanılan abonelik, ayrılmış kapasite ayırması. Abonelik üzerindeki ödeme yöntemi, Red, ayrılmış kapasite rezervasyonu için Azure önbelleğinin ön maliyetlerini ücretlendirilmiştir. Abonelik türü bir kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya kullandıkça öde fiyatlandırması (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) içeren tek bir anlaşma olmalıdır. Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde fiyatlandırmasına sahip bireysel bir abonelik için ücretler, abonelik üzerindeki kredi kartına veya fatura ödeme yöntemine faturalandırılır.
| Kapsam | Ayırma kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Şunları seçerseniz: </br></br> **, Rezervasyon**indirimi, faturalandırma bağlamınızın içindeki aboneliklerde çalışan redsıs örnekleri Için Azure önbelleğine uygulanır. Kurumsal müşteriler için, paylaşılan kapsam kayıt içindedir ve kayıt dahilindeki tüm abonelikleri içerir. Kullandıkça Öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan tüm Kullandıkça Öde abonelikleridir.</br></br> **Tek abonelik**, rezervasyon indirimi Bu abonelikteki redsıs örnekleri Için Azure önbelleğine uygulanır. </br></br> **Tek kaynak grubu**, ayırma indirimi seçili abonelikteki redsıs örnekleri ve bu abonelik içindeki seçili kaynak grubu Için Azure önbelleğine uygulanır.
| Bölge | Reddir için Azure önbelleğinin kapsamına giren Azure bölgesi ayrılmış kapasite ayırması.
| Fiyatlandırma katmanı | Redsıs sunucuları için Azure önbelleğinin hizmet katmanı.
| Sözleşme Dönemi | Bir yıl veya üç yıl
| Miktar | Reda için Azure önbelleği içinde satın alınan işlem kaynakları miktarı, ayrılmış kapasite ayırması. Bu miktar, seçili Azure bölgesindeki ve hizmet katmanındaki ayrılan ve fatura iskontosunu alacak olan bir dizi önbelleklerdir. Örneğin, Doğu ABD bölgesinde toplam önbellek kapasitesi olan Redsıs sunucuları için bir Azure önbelleği çalıştırıyorsanız veya çalıştırmayı planlıyorsanız, tüm önbellekler için avantajı en üst düzeye çıkarmak üzere miktarı 26 olarak belirtirsiniz.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="cache-size-flexibility"></a>Önbellek boyutu esnekliği

Önbellek boyutu esnekliği, ayrılmış kapasite avantajını kaybetmeden bir hizmet katmanı ve bölgesi dahilinde ölçeği büyütme veya küçültme olanakları sağlar.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyon indirimi, ayırma kapsamı ve öznitelikleriyle eşleşen Redsıs örnekleri için Azure önbelleğine otomatik olarak uygulanır. Azure portal, PowerShell, Azure CLı veya API aracılığıyla rezervasyon kapsamını güncelleştirebilirsiniz.

*  Ayrılmış kapasite iskontolarının redin için Azure önbelleğine nasıl uygulanacağını öğrenmek için bkz [. Azure rezervasyon ıskontosunu anlama](../cost-management-billing/reservations/understand-azure-cache-for-redis-reservation-charges.md)

* Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

    * [Azure ayırmaları nelerdir?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
    * [Azure Ayırmalarını yönetme](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
    * [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
    * [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-mysql)
    * [Kurumsal kaydınız için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
    * [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)

