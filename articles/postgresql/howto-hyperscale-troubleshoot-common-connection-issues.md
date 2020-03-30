---
title: Sorun Giderme bağlantıları - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'ndaki bağlantı sorunlarını nasıl gidereceklerini öğrenin
keywords: postgresql bağlantısı,bağlantı dizesi,bağlantı sorunları,geçici hata,bağlantı hatası
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/8/2019
ms.openlocfilehash: c064aca484f85c44dada9888012140784a96863f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74977514"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'na bağlantı sorunlarını giderme

Bağlantı sorunları, şu gibi birkaç şeyden kaynaklanabilir:

* Güvenlik duvarı ayarları
* Bağlantı zaman ayarı
* Yanlış oturum açma bilgileri
* Sunucu grubu için bağlantı sınırına ulaşıldı
* Hizmet altyapısı ile ilgili sorunlar
* Servis bakımı
* Koordinatör düğümü yeni donanım üzerinde başarısız

Genel olarak, Hyperscale bağlantı sorunları aşağıdaki gibi sınıflandırılabilir:

* Geçici hatalar (kısa ömürlü veya aralıklı)
* Kalıcı veya geçici olmayan hatalar (düzenli olarak yineleyen hatalar)

## <a name="troubleshoot-transient-errors"></a>Geçici hataları giderme

Geçici hatalar çeşitli nedenlerden dolayı oluşur. En yaygın sistem Bakım, donanım veya yazılım ile hata ve koordinatör düğüm vCore yükseltmeleri içerir.

Hyperscale sunucu grubu düğümleri için yüksek kullanılabilirlik etkinleştirme otomatik olarak bu tür sorunları azaltabilirsiniz. Ancak, başvurunuz yine de kısa bir süre bağlantısını kaybetmeye hazır olmalıdır. Ayrıca, büyük bir işlemin uzun süren bir kurtarma işlemine neden olması gibi diğer olayların hafifletilmesi daha uzun sürebilir.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Geçici bağlantı sorunlarını çözme adımları

1. Uygulamanın hataları bildirdiği süre içinde meydana gelen bilinen kesintiler için [Microsoft Azure Hizmet Panosu'nu](https://azure.microsoft.com/status) denetleyin.
2. Hyperscale (Citus) gibi bir bulut hizmetine bağlanan uygulamalar geçici hatalar beklemeli ve düzgün tepki vermelidir. Örneğin, uygulamalar, kullanıcılara uygulama hataları olarak yüzeye çıkarmak yerine bu hataları işlemek için yeniden deneme mantığını uygulamalıdır.
3. Sunucu grubu kaynak sınırlarına yaklaştıkça, hatalar geçici bağlantı sorunları gibi görünebilir. Düğüm RAM'ini artırmak veya alt düğümler eklemek ve verileri yeniden dengelemek yardımcı olabilir.
4. Bağlantı sorunları devam ederse veya 60 saniyeden uzun sürerse veya günde bir defadan fazla gerçekleşirse, Azure Destek sitesinde **Destek Al'ı** seçerek bir [Azure](https://azure.microsoft.com/support/options) destek isteği nde bulunun.

## <a name="troubleshoot-persistent-errors"></a>Kalıcı hataları giderme

Uygulama ısrarla Hyperscale'e (Citus) bağlanamazsa, en yaygın nedenler güvenlik duvarı yanlış yapılandırması veya kullanıcı hatasıdır.

* Koordinatör düğüm güvenlik duvarı yapılandırması: Hyperscale sunucu güvenlik duvarının proxy sunucuları ve ağ geçitleri de dahil olmak üzere istemcinizden gelen bağlantılara izin verecek şekilde yapılandırıldığından emin olun.
* İstemci güvenlik duvarı yapılandırması: İstemcinizin güvenlik duvarı veritabanı sunucunuzla bağlantıya izin vermelidir. Bazı güvenlik duvarları yalnızca ada göre uygulamaya izin vermekle kalmıyor, aynı zamanda sunucunun IP adreslerine ve bağlantı noktalarına da izin verir.
* Kullanıcı hatası: Bağlantı dizesini iki kez denetleyin. Sunucu adı gibi parametreleri yanlış yazmış olabilirsiniz. Azure portalında çeşitli dil çerçeveleri ve psql bağlantı dizeleri bulabilirsiniz. Hyperscale (Citus) sunucu grubunuzun **Bağlantı dizeleri** sayfasına gidin. Ayrıca Hyperscale (Citus) kümelerinin sadece bir veritabanına sahip olduğunu ve önceden tanımlanmış adının **citus**olduğunu unutmayın.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Kalıcı bağlantı sorunlarını çözme adımları

1. İstemci IP adresine izin vermek için [güvenlik duvarı kuralları](howto-hyperscale-manage-firewall-using-portal.md) ayarlayın. Yalnızca geçici sınama amacıyla, başlangıç IP adresi olarak 0.0.0.0'ı ve bitiş IP adresi olarak 255.255.255.255 kullanarak bir güvenlik duvarı kuralı ayarlayın. Bu kural sunucuyu tüm IP adreslerine açar. Kural bağlantı sorununuzu çözerse, kaldırın ve uygun şekilde sınırlı bir IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun.
2. İstemci ve internet arasındaki tüm güvenlik duvarlarında, 5432 bağlantı noktasının giden bağlantılar için açık olduğundan emin olun.
3. Bağlantı dizenizi ve diğer bağlantı ayarlarınızı doğrulayın.
4. Panodaki servis durumunu kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

* [PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'nda Güvenlik Duvarı kuralları](concepts-hyperscale-firewall-rules.md) kavramlarını öğrenin
* [PostgreSQL için Azure Veritabanı için güvenlik duvarı kurallarını nasıl yönetebilirsiniz bkz.](howto-hyperscale-manage-firewall-using-portal.md)
