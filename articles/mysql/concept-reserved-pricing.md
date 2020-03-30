---
title: Ayrılmış kapasiteye sahip işlem için ön ödeme - MySQL için Azure Veritabanı
description: Ayrılmış kapasiteye sahip MySQL işlem kaynakları için Azure Veritabanı için ön ödeme
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 78c8750de7189bad33e9bbc766a3d7543a646f6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159362"
---
# <a name="prepay-for-azure-database-for-mysql-compute-resources-with-reserved-capacity"></a>Ayrılmış kapasiteye sahip MySQL işlem kaynakları için Azure Veritabanı için ön ödeme

MySQL için Azure Veritabanı artık işlem kaynakları için ön ödeme yaparak, istediğiniz kadar öde fiyatlarıyla karşılaştırıldığında paradan tasarruf etmenizi sağlıyor. MySQL için Azure Veritabanı rezerve etme kapasitesiyle, işlem maliyetlerinde önemli bir indirim elde etmek için mysql sunucusunda bir veya üç yıllık bir süre için ön taahhütte bulunabilirsiniz. MySQL ayrılmış kapasite için Azure Veritabanı'nı satın almak için Azure bölgesini, dağıtım türünü, performans katmanını ve terimini belirtmeniz gerekir. </br>

MySQL sunucuları için rezervasyonu belirli Azure Veritabanına atamanız gerekmez. MySQL için veya yeni dağıtılanlar için zaten çalışan bir Azure Veritabanı, ayrılmış fiyatlandırmadan otomatik olarak yararlanır. Bir rezervasyon satın alarak, bir veya üç yıllık bir süre için işlem maliyetleri için ön ödeme yapıyorsunuz. Bir rezervasyon satın alır almaz, MySQL bilgisayar için Azure veritabanı, rezervasyon öznitelikleriyle eşleşen ücretler artık kullandıkça öde fiyatlarıüzerinden ücretlendirilmez. Rezervasyon, MySQL Veritabanı sunucusuyla ilişkili yazılım, ağ veya depolama ücretlerini kapsamaz. Rezervasyon döneminin sonunda, faturalandırma avantajının süresi doluyor ve MySQL için Azure Veritabanı size ödeme fiyatından faturalandırılır. Rezervasyonlar otomatik olarak yenilemez. Fiyatlandırma bilgileri için [MySQL ayrılmış kapasite teklifi için Azure Veritabanı'na](https://azure.microsoft.com/pricing/details/mysql/)bakın. </br>

Azure [portalında](https://portal.azure.com/)MySQL ayrılmış kapasite için Azure Veritabanı'nı satın alabilirsiniz. Ayrılmış kapasiteyi satın almak için:

* Kullandıkça öde oranlarına sahip en az bir Kurumsal veya bireysel abonelik için sahip rolünde olmalısınız.
* Kurumsal abonelikler için, [EA portal](https://ea.azure.com/)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Veya bu ayar devre dışı bırakılmışsa, abonelikte bir EA Yöneticisi olmalısınız.
* Bulut Çözüm Sağlayıcısı (CSP) programı için, MySQL ayrılmış kapasite için azure veritabanını yalnızca yönetici aracılar veya satış temsilcileri satın alabilir. </br>

Kurumsal müşterilerin ve Sizden Önce Öde müşterilerinin rezervasyon satın alımları için nasıl ücretlendirildikine ilişkin ayrıntılar, [Kurumsal kaydınız için Azure rezervasyon kullanımını anlayın](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) ve [Kullan-Kaç'a Göre Aboneliğiniz için Azure rezervasyon kullanımını anlayın.](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)


## <a name="determine-the-right-database-size-before-purchase"></a>Satın almadan önce doğru veritabanı boyutunu belirleme

Rezervasyon un boyutu, belirli bir bölge içinde varolan veya yakında dağıtılacak sunucu tarafından kullanılan ve aynı performans katmanı ve donanım oluşturmayı kullanan toplam işlem miktarına dayanmalıdır.</br>

Örneğin, genel bir amaç, Gen5 - 32 vCore MySQL veritabanı ve iki bellek optimize, Gen5 - 16 vCore MySQL veritabanları çalıştırdığınızı varsayalım. Ayrıca, önümüzdeki ay içinde ek bir genel amaç, Gen5 - 32 vCore veritabanı sunucusu ve bir bellek optimize, Gen5 - 16 vCore veritabanı sunucusu dağıtmak için plan varsayalım. Bu kaynaklara en az 1 yıl boyunca ihtiyacınız olacağını bildiğinizi varsayalım. Bu durumda, bir 64 (2x32) vCores, tek veritabanı genel amaçlı 1 yıl rezervasyon - Gen5 ve 48 (2x16 + 16) tek veritabanı bellek için 1 yıl rezervasyon optimize - Gen5 satın almalısınız


## <a name="buy-azure-database-for-mysql-reserved-capacity"></a>MySQL ayrılmış kapasite için Azure Veritabanı satın alın

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. **Tüm hizmetler** > **rezervasyonlarını**seçin.
3. Satın alma rezervasyonları bölmesinde **Ekle'yi** seçin ve MySQL veritabanlarınız için yeni bir rezervasyon satın almak **için MySQL için Azure Veritabanı'nı** seçin.
4. Gerekli alanları doldurun. Seçtiğiniz özniteliklerle eşleşen varolan veya yeni veritabanları ayrılmış kapasite indirimini almaya hak kazanır. İndirimalan MySQL sunucuları için Azure Veritabanınızın gerçek sayısı, seçilen kapsam ve miktara bağlıdır.


![Ayrılmış fiyatlandırmaya genel bakış](media/concepts-reserved-pricing/mysql-reserved-price.png)


Aşağıdaki tabloda gerekli alanları açıklanmaktadır.

| Alan | Açıklama |
| :------------ | :------- |
| Abonelik   | MySQL için Azure Veritabanı için ödeme yapmak için kullanılan abonelik, ayrılmış kapasite rezervasyonu. Abonelikteki ödeme yöntemi, MySQL ayrılmış kapasite rezervasyonu için Azure Veritabanı için ön maliyetlerden ücretlendirilir. Abonelik türü bir kurumsal sözleşme (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya gidilecek kadar öde fiyatlandırması (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) ile bireysel bir anlaşma olmalıdır. Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Gittikçe öde fiyatlandırmasına sahip tek bir abonelik için, ücretler abonelikteki kredi kartına veya fatura ödeme yöntemine faturalandırılır.
| Kapsam | vCore rezervasyonun kapsamı bir aboneliği veya birden çok aboneliği (paylaşılan kapsamı) kapsayabilir. Seçerseniz: </br></br> **Paylaşılan,** vCore rezervasyon indirimi, faturalandırma bağlamınızdaki tüm aboneliklerde çalışan MySQL sunucuları için Azure Veritabanı'na uygulanır. Kurumsal müşteriler için paylaşılan kapsam kayıttır ve kayıt içindeki tüm abonelikleri içerir. Kullandıkça Öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan tüm Kullandıkça Öde abonelikleridir.</br></br> **Tek abonelik,** vCore rezervasyon indirimi bu abonelikteki MySQL sunucuları için Azure Veritabanı'na uygulanır. </br></br> **Tek kaynak grubu,** rezervasyon indirimi seçili abonelikteki MySQL sunucuları için Azure Veritabanı'na ve bu abonelik içindeki seçili kaynak grubuna uygulanır.
| Bölge | MySQL için Azure Veritabanı tarafından kapsanan Azure bölgesi, kapasite rezervasyonu için ayrılmıştır.
| Dağıtım Türü | Rezervasyonu satın almak istediğiniz MySQL kaynak türü için Azure Veritabanı.
| Performans Katmanı | MySQL sunucuları için Azure Veritabanı için hizmet katmanı.
| Sözleşme Dönemi | Bir yıl
| Miktar | MySQL için Azure Veritabanı'nda satın alınan işlem kaynaklarının miktarı ayrılmış kapasite rezervasyonu. Miktar, seçili Azure bölgesinde ve Performans katmanında rezerve edilen ve faturalandırma indirimini alacak bir dizi vCore'dur. Örneğin, Doğu ABD bölgesinde Gen5 16 vCores'un toplam bilgi işlem kapasitesine sahip MySQL sunucuları için bir Azure Veritabanı çalıştırıyor veya çalıştırmayı planlıyorsanız, tüm sunucuların yararını en üst düzeye çıkarmak için miktarı 16 olarak belirtirsiniz.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="vcore-size-flexibility"></a>vCore boyut esnekliği

vCore boyut esnekliği, ayrılmış kapasite avantajını kaybetmeden bir performans katmanı ve bölge içinde yukarı veya aşağı ölçeklendirmenize yardımcı olur. 

## <a name="need-help--contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

## <a name="next-steps"></a>Sonraki adımlar

vCore rezervasyon indirimi, MySQL için Azure Veritabanı ile eşleşen MySQL sunucuları için Azure Veritabanı sayısına otomatik olarak uygulanır. MySQL ayrılmış kapasite rezervasyonu için Azure veritabanının kapsamını Azure portalı, PowerShell, CLI veya API üzerinden güncelleştirebilirsiniz. </br></br>
MySQL için Azure Veritabanı'nı nasıl yöneteceksiniz?

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Rezervasyonları nedir?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
* [Azure Ayırmalarını yönetme](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
* [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
* [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-mysql)
* [Kurumsal kaydınız için rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
* [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)

