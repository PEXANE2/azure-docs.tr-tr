---
title: Ayrılmış kapasiteye sahip işlem için ön ödeme - Redis için Azure Önbelleği
description: Rezerve edilmiş kapasiteye sahip Redis hesaplama kaynakları için Azure Önbelleği için ön ödeme
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: aded023c9f4c045f612e33d32c1e3ac71afddf02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77530308"
---
# <a name="prepay-for-azure-cache-for-redis-compute-resources-with-reserved-capacity"></a>Rezerve edilmiş kapasiteye sahip Redis hesaplama kaynakları için Azure Önbelleği için ön ödeme

Redis için Azure Önbelleği artık, istediğiniz kadar öde fiyatlarıyla karşılaştırıldığında hesaplama kaynakları için ön ödeme yaparak tasarruf etmenizi sağlıyor. Redis için Azure Önbellek ayrılmış kapasite ile, işlem maliyetlerinde önemli bir indirim elde etmek için bir veya üç yıllık bir süre için önbellek tetaz. Redis için Azure Önbelleği'ni satın almak için Azure bölgesini, hizmet katmanını ve terimini belirtmeniz gerekir.

Redis örnekleri için rezervasyonu belirli Azure Önbelleğine atamanız gerekmez. Redis için zaten çalışan bir Azure Önbelleği veya yeni dağıtılanlar, ayrılmış önbellek boyutuna kadar ayrılmış fiyatlandırmadan otomatik olarak yararlanır. Bir rezervasyon satın alarak, bir veya üç yıllık bir süre için işlem maliyetleri için ön ödeme yapıyorsunuz. Bir rezervasyon satın alır almaz, Redis için Azure Önbelleği, rezervasyon öznitelikleriyle eşleşen ödeme ücretleri artık kullandıkça öde fiyatlarıüzerinden ücretlendirilmez. Rezervasyon, önbellekle ilişkili ağ veya depolama ücretlerini kapsamaz. Rezervasyon döneminin sonunda, faturalandırma avantajı nın süresi doluyor ve Redis için Azure Önbelleği size ödeme fiyatından faturalandırılır. Rezervasyonlar otomatik olarak yenilemez. Fiyatlandırma bilgileri için [Redis için Azure Önbelleği'ne bakın.](https://azure.microsoft.com/pricing/details/cache)

Azure [portalında](https://portal.azure.com/)Redis için Azure Önbelleği'ni rezerve edilmiş kapasite satın alabilirsiniz. Ayrılmış kapasiteyi satın almak için:

* Kullandıkça öde oranlarına sahip en az bir Kurumsal veya bireysel abonelik için sahip rolünde olmalısınız.
* Kurumsal abonelikler için, [EA portal](https://ea.azure.com/)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Veya bu ayar devre dışı bırakılmışsa, abonelikte bir EA Yöneticisi olmalısınız.
* Bulut Çözüm Sağlayıcısı (CSP) programı için, Redis ayrılmış kapasite için yalnızca yönetici aracılar veya satış temsilcileri Azure Önbelleği satın alabilir.

Kurumsal müşterilerin ve Sizden Önce Öde müşterilerinin rezervasyon satın alımları için nasıl ücretlendirildikine ilişkin ayrıntılar için, [Kurumsal kaydınız için Azure rezervasyon kullanımını anlayın](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) ve [Kullan-Kaç'a Göre aboneliğiniz için Azure rezervasyon kullanımını anlayın.](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)


## <a name="determine-the-right-cache-size-before-purchase"></a>Satın almadan önce doğru önbellek boyutunu belirleme

Rezervasyonun boyutu, belirli bir bölge içinde varolan veya yakında dağıtılacak önbellek tarafından kullanılan ve aynı hizmet katmanını kullanan toplam işlem miktarına dayanmalıdır.

Örneğin, genel bir amaç, Gen5 – 32 vCore önbelleği ve iki bellek optimize edilmiş, Gen5 – 16 vCore önbellekleri çalıştırdığınızı varsayalım. Ayrıca, önümüzdeki ay içinde ek bir genel amaç, Gen5 - 32 vCore veritabanı sunucusu ve bir bellek optimize, Gen5 - 16 vCore veritabanı sunucusu dağıtmak için plan varsayalım. Bu kaynaklara en az 1 yıl boyunca ihtiyacınız olacağını bildiğinizi varsayalım. Bu durumda, bir 64 (2x32) vCores, tek veritabanı genel amaçlı 1 yıl rezervasyon - Gen5 ve 48 (2x16 + 16) tek veritabanı bellek için 1 yıl rezervasyon optimize - Gen5 satın almalısınız


## <a name="buy-azure-cache-for-redis-reserved-capacity"></a>Redis ayrılmış kapasite için Azure Önbelleği satın alın

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. **Tüm hizmetler** > **rezervasyonlarını**seçin.
3. **Satınalma** rezervasyonları bölmesinde Ekle'yi seçin ve ardından önbellekleriniz için yeni bir rezervasyon satın almak **için Redis için Azure Önbelleği'ni** seçin.
4. Gerekli alanları doldurun. Seçtiğiniz özniteliklerle eşleşen varolan veya yeni veritabanları ayrılmış kapasite indirimini almaya hak kazanır. İndirimalan Redis örnekleri için Azure Önbelleğinizin gerçek sayısı, seçilen kapsam ve miktara bağlıdır.


![Ayrılmış fiyatlandırmaya genel bakış](media/cache-reserved-pricing/cache-reserved-price.png)


Aşağıdaki tabloda gerekli alanları açıklanmaktadır.

| Alan | Açıklama |
| :------------ | :------- |
| Abonelik   | Redis için Azure Önbelleği için ödeme yapmak için kullanılan abonelik, ayrılmış kapasite rezervasyonu. Abonelikteki ödeme yöntemi, Redis için Azure Önbelleği için ön maliyetlerden ücretlendirilir. Abonelik türü bir kurumsal sözleşme (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya gidilecek kadar öde fiyatlandırması (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) ile bireysel bir anlaşma olmalıdır. Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Gittikçe öde fiyatlandırmasına sahip tek bir abonelik için, ücretler abonelikteki kredi kartına veya fatura ödeme yöntemine faturalandırılır.
| Kapsam | Rezervasyonun kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsam) kapsayabilir. Seçerseniz: </br></br> **Paylaşılan,** rezervasyon indirimi, faturalandırma bağlamınızdaki tüm aboneliklerde çalışan Redis örnekleri için Azure Önbelleğine uygulanır. Kurumsal müşteriler için paylaşılan kapsam kayıttır ve kayıt içindeki tüm abonelikleri içerir. Kullandıkça Öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan tüm Kullandıkça Öde abonelikleridir.</br></br> **Tek abonelik,** rezervasyon indirimi bu abonelikteki Redis örnekleri için Azure Önbelleğine uygulanır. </br></br> **Tek kaynak grubu,** rezervasyon indirimi, seçili abonelikteki Redis örnekleri için Azure Önbelleğine ve bu abonelik içindeki seçili kaynak grubuna uygulanır.
| Bölge | Redis için Azure Önbelleği tarafından kapsanan Azure bölgesi, kapasite rezervasyonu için ayrılmıştır.
| Fiyatlandırma katmanı | Redis sunucuları için Azure Önbelleği için hizmet katmanı.
| Sözleşme Dönemi | Bir yıl ya da üç yıl
| Miktar | Redis için Azure Önbelleği'nde satın alınan işlem kaynaklarının miktarı ayrılmış kapasite rezervasyonu. Miktar, seçili Azure bölgesinde ve hizmet katmanında rezerve edilen ve fatura landırma indirimini alacak bir dizi önbellektir. Örneğin, Doğu ABD bölgesinde toplam önbellek kapasitesine sahip Redis sunucuları için bir Azure Önbelleği çalıştırıyor veya çalıştırmayı planlıyorsanız, tüm önbelleklerin yararını en üst düzeye çıkarmak için miktarı 26 olarak belirtirsiniz.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="cache-size-flexibility"></a>Önbellek boyutu esnekliği

Önbellek boyutu esnekliği, ayrılmış kapasite avantajını kaybetmeden bir hizmet katmanı ve bölge içinde yukarı veya aşağı ölçeklendirmenize yardımcı olur.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyon indirimi, rezervasyon kapsamı ve öznitelikleriyle eşleşen Redis örnekleri için Azure Önbelleğine otomatik olarak uygulanır. Azure portalı, PowerShell, Azure CLI veya API aracılığıyla rezervasyonkapsamını güncelleştirebilirsiniz.

*  Redis için Azure Önbelleği'ne ayrılmış kapasite indirimlerinin nasıl [uygulandığını](../cost-management-billing/reservations/understand-azure-cache-for-redis-reservation-charges.md) öğrenmek için bkz.

* Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

    * [Azure Rezervasyonları nedir?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
    * [Azure Ayırmalarını yönetme](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
    * [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
    * [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-mysql)
    * [Kurumsal kaydınız için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
    * [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)

