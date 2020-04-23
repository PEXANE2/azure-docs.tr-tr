---
title: Bağlantı sorunlarını giderme-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı-tek sunucu ile bağlantı sorunlarını giderme hakkında bilgi edinin.
keywords: PostgreSQL bağlantısı, bağlantı dizesi, bağlantı sorunları, geçici hata, bağlantı hatası
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 771d7f5b037fde1144b18dc4ed0dee7aecac6744
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82100218"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı ile ilgili bağlantı sorunlarını giderme-tek sunucu

Bağlantı sorunlarına aşağıdakiler de dahil olmak üzere çeşitli öğeler neden olmuş olabilir:

* Güvenlik duvarı ayarları
* Bağlantı zaman aşımı
* Hatalı oturum açma bilgileri
* Bazı PostgreSQL kaynakları için Azure veritabanı kaynaklarına maksimum sınıra ulaşıldı
* Hizmetin altyapısıyla ilgili sorunlar
* Hizmette gerçekleştirilen bakım
* Sunucunun işlem ayırması, sanal çekirdek sayısı ölçeklenerek veya farklı bir hizmet katmanına taşınarak değiştirilir

Genellikle, PostgreSQL için Azure veritabanı bağlantı sorunları şu şekilde sınıflandırılabilir:

* Geçici hatalar (kısa ömürlü veya aralıklı)
* Kalıcı veya geçici olmayan hatalar (düzenli olarak yinelenen hatalar)

## <a name="troubleshoot-transient-errors"></a>Geçici hatalarda sorun giderme

Bakım gerçekleştirildiğinde geçici hatalar meydana gelir, sistem donanım veya yazılımla ilgili bir hatayla karşılaştığında veya sunucunuzun sanal çekirdeklerini veya hizmet katmanını değiştirirsiniz. PostgreSQL için Azure veritabanı hizmeti yerleşik yüksek kullanılabilirliğe sahiptir ve bu tür sorunları otomatik olarak azaltmak için tasarlanmıştır. Bununla birlikte, uygulamanız, genellikle 60 saniyeden kısa bir süre boyunca sunucu bağlantısını kaybeder. Büyük bir işlemin uzun süre çalışan bir kurtarmaya neden olduğu gibi bazı olayların hafifletmek daha uzun sürebilir.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Geçici bağlantı sorunlarını giderme adımları

1. Uygulama tarafından hataların bildirildiği zaman içinde oluşan bilinen kesintiler için [Microsoft Azure hizmet panosunu](https://azure.microsoft.com/status) denetleyin.
2. PostgreSQL için Azure veritabanı gibi bir bulut hizmetine bağlanan uygulamalar, bunları kullanıcılara uygulama hataları olarak eklemek yerine bu hataları işlemek için geçici hataları ve yeniden deneme mantığını uygulamalıdır. [PostgreSQL Için Azure veritabanı için geçici bağlantı hatalarının işlenmesini](concepts-connectivity.md) , en iyi uygulamalar ve geçici hataları işlemeye yönelik tasarım yönergeleri için gözden geçirin.
3. Sunucu kaynak sınırlarına yaklaşıyorsa, hatalar geçici bağlantı sorunu olabilir. [PostgreSQL Için Azure veritabanı 'Ndaki sınırlamalara](concepts-limits.md)bakın.
4. Bağlantı sorunları devam ederse veya uygulamanızın hatayla karşılaştığı süre 60 saniye değerini aşarsa veya hatanın belirli bir gün içinde birden çok kez yinelendiğini görürseniz, [Azure](https://azure.microsoft.com/support/options) destek sitesinde **Destek Al** ' ı seçerek bir Azure destek isteği dosyası sağlayın.

## <a name="troubleshoot-persistent-errors"></a>Kalıcı hatalarda sorun giderme

Uygulama, PostgreSQL için Azure veritabanı 'na kalıcı olarak bağlanamazsa, genellikle aşağıdakilerden biriyle ilgili bir sorun olduğunu gösterir:

* Sunucu güvenlik duvarı yapılandırması: PostgreSQL için Azure veritabanı sunucu güvenlik duvarının, proxy sunucuları ve ağ geçitleri dahil olmak üzere istemcinizden gelen bağlantılara izin verecek şekilde yapılandırıldığından emin olun.
* İstemci güvenlik duvarı yapılandırması: istemcinizdeki güvenlik duvarı, veritabanı sunucunuza yönelik bağlantılara izin vermelidir. Bazı güvenlik duvarlarındaki PostgreSQL gibi uygulama adlarının yanı sıra, izin verilmeyen sunucunun IP adreslerine ve bağlantı noktalarına izin verilmesi gerekir.
* Kullanıcı hatası: bağlantı dizesindeki sunucu adı veya Kullanıcı adında eksik * \@ServerName* son eki gibi yanlış bağlantı parametreleri olabilir.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Kalıcı bağlantı sorunlarını giderme adımları

1. İstemci IP adresine izin vermek için [güvenlik duvarı kuralları](howto-manage-firewall-using-portal.md) ayarlayın. Yalnızca geçici test amacıyla, başlangıç IP adresi olarak 0.0.0.0 kullanarak bir güvenlik duvarı kuralı ayarlayın ve bitiş IP adresi olarak 255.255.255.255 kullanın. Bu, sunucuyu tüm IP adreslerine açar. Bu, bağlantı sorununuzu giderirse, bu kuralı kaldırın ve uygun şekilde sınırlı bir IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun.
2. İstemci ile İnternet arasındaki tüm güvenlik duvarlarında, bağlantı noktası 5432 ' ın giden bağlantılar için açık olduğundan emin olun.
3. Bağlantı dizenizi ve diğer bağlantı ayarlarını doğrulayın.
4. Panodaki hizmet durumunu denetleyin. Bölgesel bir kesinti olduğunu düşünüyorsanız, yeni bir bölgeye kurtarma adımları için [PostgreSQL Için Azure veritabanı ile iş sürekliliği 'Ne genel bakış](concepts-business-continuity.md) konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [PostgreSQL için Azure veritabanı geçici bağlantı hatalarının işlenmesi](concepts-connectivity.md)
