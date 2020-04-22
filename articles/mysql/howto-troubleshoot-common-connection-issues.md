---
title: Sorun giderme bağlantı sorunları - MySQL için Azure Veritabanı
description: Yeniden deneme gerektiren geçici hatalar, güvenlik duvarı sorunları ve kesintiler de dahil olmak üzere MySQL için Azure Veritabanı'na bağlantı sorunlarını nasıl gidereceklerini öğrenin.
keywords: mysql bağlantısı,bağlantı dizesi,bağlantı sorunları,geçici hata,bağlantı hatası
author: jasonwhowell
ms.author: jasonh
ms.service: mysql
ms.topic: troubleshooting
ms.date: 3/18/2020
ms.openlocfilehash: b22d47d177c4606396b0c8b2279301121c905ca2
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768284"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-mysql"></a>MySQL için Azure Veritabanı bağlantı sorunlarını giderme

Bağlantı sorunları, şunları dahil olmak üzere çeşitli nedenlerden kaynaklanabilir:

* Güvenlik duvarı ayarları
* Bağlantı zaman ayarı
* Yanlış giriş bilgileri
* MySQL kaynakları için bazı Azure Veritabanında ulaşılan maksimum sınır
* Hizmet altyapısı ile ilgili sorunlar
* Bakım hizmetinde yapılıyor
* Sunucunun bilgi işlem dağılımı vCores sayısını ölçekleme veya farklı bir hizmet katmanına hareket ettirerek değiştirilir

Genel olarak, MySQL için Azure Veritabanı'na bağlantı sorunları aşağıdaki gibi sınıflandırılabilir:

* Geçici hatalar (kısa ömürlü veya aralıklı)
* Kalıcı veya geçici olmayan hatalar (düzenli olarak yineleyen hatalar)

## <a name="troubleshoot-transient-errors"></a>Geçici hataları giderme

Geçici hatalar bakım yapıldığında oluşur, sistem donanım veya yazılımda bir hatayla karşılaşır veya sunucunuzun vCores veya servis katmanını değiştirirsiniz. MySQL için Azure Veritabanı yerleşik yüksek kullanılabilirlik sağlar ve bu tür sorunları otomatik olarak azaltmak için tasarlanmıştır. Ancak, uygulamanız sunucuya olan bağlantısını genellikle en fazla 60 saniyeden kısa bir süre için kaybeder. Bazı olayların bazen kısa sürede hafifletilmesi, örneğin büyük bir işlemin uzun süren bir kurtarma işlemine neden olması gibi sürebilir.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Geçici bağlantı sorunlarını çözme adımları

1. Hataların uygulama tarafından raporlandığı süre içinde meydana gelen bilinen kesintiler için [Microsoft Azure Hizmet Panosu'nu](https://azure.microsoft.com/status) denetleyin.
2. MySQL için Azure Veritabanı gibi bir bulut hizmetine bağlanan uygulamalar geçici hatalar beklemeli ve kullanıcılara uygulama hatası olarak bunları yüzeye çıkarmak yerine bu hataları işlemek için yeniden deneme mantığını uygulamalıdır. En iyi uygulamalar ve geçici hataları işlemek için tasarım yönergeleri [için MySQL için Azure Veritabanı için geçici bağlantı hatalarının işlenmesini](concepts-connectivity.md) gözden geçirin.
3. Bir sunucu kaynak sınırlarına yaklaştıkça, hatalar geçici bağlantı sorunu gibi görünebilir. [MySQL için Azure Veritabanındaki Sınırlamalar'a](concepts-limits.md)bakın.
4. Bağlantı sorunları devam ederse veya uygulamanızın hatayla karşılaşma süresi 60 saniyeyi aşarsa veya belirli bir günde birden fazla hata oluşumu nu görürseniz, Azure Destek sitesinde **Destek Al'ı** seçerek bir [Azure](https://azure.microsoft.com/support/options) destek isteği dosyalayın.

## <a name="troubleshoot-persistent-errors"></a>Kalıcı hataları giderme

Uygulama Kalıcı olarak MySQL için Azure Veritabanı'na bağlanamazsa, genellikle aşağıdakilerden biriyle ilgili bir sorunu gösterir:

* Sunucu güvenlik duvarı yapılandırması: MySQL sunucu güvenlik duvarı için Azure Veritabanı'nın proxy sunucuları ve ağ geçitleri de dahil olmak üzere istemcinizden gelen bağlantılara izin verecek şekilde yapılandırıldığından emin olun.
* İstemci güvenlik duvarı yapılandırması: İstemcinizin güvenlik duvarı veritabanı sunucunuzla bağlantıya izin vermelidir. Bazı güvenlik duvarlarında MySQL gibi uygulama adlarının yanı sıra, izin verilemeyeceğiniz sunucunun IP adresleri ve bağlantı noktaları.
* Kullanıcı hatası: Bağlantı dizesindeki sunucu adı veya kullanıcı adındaeksik * \@* sunucu adı eki gibi bağlantı parametrelerini yanlış yazmış olabilirsiniz.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Kalıcı bağlantı sorunlarını çözme adımları

1. İstemci IP adresine izin vermek için [güvenlik duvarı kuralları](howto-manage-firewall-using-portal.md) ayarlayın. Yalnızca geçici sınama amacıyla, başlangıç IP adresi olarak 0.0.0.0'ı ve bitiş IP adresi olarak 255.255.255.255 kullanarak bir güvenlik duvarı kuralı ayarlayın. Bu, sunucuyu tüm IP adreslerine açar. Bu, bağlantı sorununuzu çözerse, bu kuralı kaldırın ve uygun şekilde sınırlı bir IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun.
2. İstemci ve internet arasındaki tüm güvenlik duvarlarında, 3306 bağlantı noktasının giden bağlantılar için açık olduğundan emin olun.
3. Bağlantı dizenizi ve diğer bağlantı ayarlarınızı doğrulayın. [MySQL için uygulamaları Azure Veritabanına nasıl bağlayabilirsiniz'u](howto-connection-string.md)gözden geçirin.
4. Panodaki servis durumunu kontrol edin. Bölgesel bir kesinti olduğunu düşünüyorsanız, yeni bir bölgeye kurtarılan adımlar [için MySQL için Azure Veritabanı ile iş sürekliliğine genel bakış](concepts-business-continuity.md) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [MySQL için Azure Veritabanı için geçici bağlantı hatalarının işlenmesi](concepts-connectivity.md)
