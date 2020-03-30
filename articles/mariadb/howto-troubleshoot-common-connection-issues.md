---
title: Sorun giderme bağlantı sorunları - MariaDB için Azure Veritabanı
description: Yeniden deneme gerektiren geçici hatalar, güvenlik duvarı sorunları ve kesintiler de dahil olmak üzere MariaDB için Azure Veritabanı'na bağlantı sorunlarını nasıl gidereceklerini öğrenin.
author: jan-eng
ms.author: janeng
ms.service: mariadb
ms.topic: troubleshooting
ms.date: 3/18/2020
ms.openlocfilehash: d134bcd0c5f9bfde0fdb095122d54848873174be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536369"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı bağlantı sorunlarını giderme

Bağlantı sorunları, şunları dahil olmak üzere çeşitli nedenlerden kaynaklanabilir:

* Güvenlik duvarı ayarları
* Bağlantı zaman ayarı
* Yanlış giriş bilgileri
* MariaDB kaynakları için bazı Azure Veritabanında ulaşılan maksimum sınır
* Hizmet altyapısı ile ilgili sorunlar
* Bakım hizmetinde yapılıyor
* Sunucunun bilgi işlem dağılımı vCores sayısını ölçekleme veya farklı bir hizmet katmanına hareket ettirerek değiştirilir

Genel olarak, MariaDB için Azure Veritabanı'na bağlantı sorunları aşağıdaki gibi sınıflandırılabilir:

* Geçici hatalar (kısa ömürlü veya aralıklı)
* Kalıcı veya geçici olmayan hatalar (düzenli olarak yineleyen hatalar)

## <a name="troubleshoot-transient-errors"></a>Geçici hataları giderme

Geçici hatalar bakım yapıldığında oluşur, sistem donanım veya yazılımda bir hatayla karşılaşır veya sunucunuzun vCores veya servis katmanını değiştirirsiniz. MariaDB hizmeti için Azure Veritabanı yerleşik yüksek kullanılabilirlik sağlar ve bu tür sorunları otomatik olarak azaltmak üzere tasarlanmıştır. Ancak, uygulamanız sunucuya olan bağlantısını genellikle en fazla 60 saniyeden kısa bir süre için kaybeder. Bazı olayların bazen kısa sürede hafifletilmesi, örneğin büyük bir işlemin uzun süren bir kurtarma işlemine neden olması gibi sürebilir.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Geçici bağlantı sorunlarını çözme adımları

1. Hataların uygulama tarafından raporlandığı süre içinde meydana gelen bilinen kesintiler için [Microsoft Azure Hizmet Panosu'nu](https://azure.microsoft.com/status) denetleyin.
2. MariaDB için Azure Veritabanı gibi bir bulut hizmetine bağlanan uygulamalar geçici hatalar beklemeli ve kullanıcılara uygulama hatası olarak bunları yüzeye çıkarmak yerine bu hataları işlemek için yeniden deneme mantığı uygulamalıdır. En iyi uygulamalar ve geçici hataları işlemek için tasarım yönergeleri [için MariaDB için Azure Veritabanı için geçici bağlantı hatalarının işlenmesini](concepts-connectivity.md) gözden geçirin.
3. Bir sunucu kaynak sınırlarına yaklaştıkça, hatalar geçici bağlantı sorunu gibi görünebilir. [MariaDB için Azure Veritabanındaki Sınırlamalar'a](concepts-limits.md)bakın.
4. Bağlantı sorunları devam ederse veya uygulamanızın hatayla karşılaşma süresi 60 saniyeyi aşarsa veya belirli bir günde birden fazla hata oluşumu nu görürseniz, Azure Destek sitesinde **Destek Al'ı** seçerek bir [Azure](https://azure.microsoft.com/support/options) destek isteği dosyalayın.

## <a name="troubleshoot-persistent-errors"></a>Kalıcı hataları giderme

Uygulama ısrarla MariaDB için Azure Veritabanı'na bağlanamazsa, genellikle aşağıdakilerden biriyle ilgili bir sorunu gösterir:

* Güvenlik duvarı yapılandırması: MariaDB sunucusu veya istemci tarafındaki güvenlik duvarı için Azure Veritabanı bağlantıları engelliyor.
* İstemci tarafında ağ yeniden yapılandırması: Yeni bir IP adresi veya proxy sunucusu eklendi.
* Kullanıcı hatası: Örneğin, bağlantı dizesindeki sunucu adı veya kullanıcı adındaeksik * \@* sunucu adı soneki gibi bağlantı parametrelerini yanlış yazdınız olabilir.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Kalıcı bağlantı sorunlarını çözme adımları

1. İstemci IP adresine izin vermek için [güvenlik duvarı kuralları](howto-manage-firewall-portal.md) ayarlayın. Yalnızca geçici sınama amacıyla, başlangıç IP adresi olarak 0.0.0.0'ı ve bitiş IP adresi olarak 255.255.255.255 kullanarak bir güvenlik duvarı kuralı ayarlayın. Bu, sunucuyu tüm IP adreslerine açar. Bu, bağlantı sorununuzu çözerse, bu kuralı kaldırın ve uygun şekilde sınırlı bir IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun.
2. İstemci ve internet arasındaki tüm güvenlik duvarlarında, 3306 bağlantı noktasının giden bağlantılar için açık olduğundan emin olun.
3. Bağlantı dizenizi ve diğer bağlantı ayarlarınızı doğrulayın. [MariaDB için uygulamaların Azure Veritabanına nasıl bağlanışla bağlanabildiğini](howto-connection-string.md)gözden geçirin.
4. Panodaki servis durumunu kontrol edin. Bölgesel bir kesinti olduğunu düşünüyorsanız, yeni bir bölgeye kurtarılan adımlar [için MariaDB için Azure Veritabanı ile iş sürekliliğine genel bakış](concepts-business-continuity.md) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [MariaDB için Azure Veritabanı için geçici bağlantı hatalarının işlenmesi](concepts-connectivity.md)
