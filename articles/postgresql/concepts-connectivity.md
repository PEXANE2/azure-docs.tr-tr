---
title: Geçici bağlantı hatalarını işleme - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL - Single Server için Azure Veritabanı için geçici bağlantı hatalarını nasıl işleyeceğinizi öğrenin.
keywords: postgresql bağlantısı,bağlantı dizesi,bağlantı sorunları,geçici hata,bağlantı hatası
author: jasonwhowell
ms.author: jasonh
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 8138512dfc893f3523c5ad78a93aef1bcdedfe70
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768046"
---
# <a name="handling-transient-connectivity-errors-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı'nda geçici bağlantı hatalarını işleme - Tek Sunucu

Bu makalede, PostgreSQL için Azure Veritabanı'na bağlanan geçici hataların nasıl işleyeceğiniaçıklanmaktadır.

## <a name="transient-errors"></a>Geçici hatalar

Geçici hata olarak da bilinen geçici hata, kendi kendini çözecek bir hatadır. Genellikle bu hatalar bırakılan veritabanı sunucusuna bir bağlantı olarak tezahür eder. Ayrıca bir sunucuya yeni bağlantılar açılamaz. Geçici hatalar örneğin donanım veya ağ hatası oluştuğunda oluşabilir. Başka bir nedeni dışarı haddelenmiş olan bir PaaS hizmetinin yeni bir sürümü olabilir. Bu olayların çoğu sistem tarafından 60 saniyeden kısa bir sürede otomatik olarak azaltılır. Bulutta uygulama tasarlamak ve geliştirmek için en iyi uygulama geçici hatalar beklemektir. Herhangi bir bileşende herhangi bir zamanda gerçekleşebileceğini ve bu durumları işlemek için uygun mantığa sahip olabileceklerini varsayalım.

## <a name="handling-transient-errors"></a>Geçici hataları işleme

Geçici hatalar yeniden deneme mantığı kullanılarak ele alınmalıdır. Göz önünde bulundurulması gereken durumlar:

* Bağlantıyı açmaya çalıştığınızda bir hata oluşur
* Sunucu tarafına boşta bağlantı bırakılır. Bir komut vermeye çalıştığınızda yürütülemez
* Şu anda bir komutuyguluyor etkin bir bağlantı bırakılır.

Birinci ve ikinci durumda oldukça düz ele almak için vardır. Bağlantıyı yeniden açmaya çalışın. Başarılı olduğunuzda, geçici hata sistem tarafından azaltıldı. PostgreSQL için Azure Veritabanınızı yeniden kullanabilirsiniz. Bağlantıyı yeniden denemeden önce beklemenizi öneririz. İlk yeniden denemeler başarısız olursa geri çek. Bu şekilde sistem hata durumunu aşmak için kullanılabilir tüm kaynakları kullanabilirsiniz. Takip etmek için iyi bir desen:

* İlk denemenizden önce 5 saniye bekleyin.
* Her bir yeniden deneme için, 60 saniyeye kadar bekleme yi katlanarak artırır.
* Uygulamanızın işlemin başarısız olduğunu düşündüğü en yüksek yeniden deneme sayısını ayarlayın.

Etkin bir işlemle bağlantı başarısız olduğunda, kurtarmaişlemini doğru şekilde işlemek daha zordur. İki durum vardır: Hareket yalnızca salt okunursa, bağlantıyı yeniden açmak ve hareketi yeniden denemek güvenlidir. Ancak, hareket veritabanına da yazıyorsa, hareketin geri alınıp alınıp alınıp alınıp alınmayacağına veya geçici hata gerçekleşmeden önce başarılı olup olmadığını belirlemeniz gerekir. Bu durumda, veritabanı sunucusundan taahhüt onayını almamış olabilirsiniz.

Bunu yapmanın bir yolu, tüm yeniden denemeler için kullanılan istemci üzerinde benzersiz bir kimlik oluşturmaktır. Bu benzersiz kimliği işlemin bir parçası olarak sunucuya aktarın ve benzersiz bir kısıtlaması olan bir sütunda saklarsınız. Bu şekilde hareketi güvenli bir şekilde yeniden deneyebilirsiniz. Önceki işlem geri alındıysa ve istemci sistemde henüz benzersiz bir kimlik oluşturmazsa başarılı olur. Önceki işlem başarıyla tamamlandığı için, benzersiz kimlik daha önce depolanmışsa yinelenen anahtar ihlalini gösteren başarısız olur.

Programınız Üçüncü taraf ara yazılımlar aracılığıyla PostgreSQL için Azure Veritabanı ile iletişim kurduğunda, satıcıya ara yazılımın geçici hatalar için yeniden deneme mantığı içerip içermediğini sorun.

Mantığınızı yeniden denediğinizi test ettiğinizden emin olun. Örneğin, PostgreSQL sunucusu için Azure Veritabanınızın bilgi işlem kaynaklarını ölçeklerken veya küçülterek kodunuzu yürütmeyi deneyin. Uygulamanız, bu işlem sırasında karşılaşılan kısa kapalı kalma süresini sorunsuz bir şekilde ele almalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [PostgreSQL için Azure Veritabanı bağlantısı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)
