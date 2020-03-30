---
title: Geçici bağlantı hataları - MariaDB için Azure Veritabanı
description: MariaDB için Azure Veritabanı için geçici bağlantı hatalarını nasıl işleyeceğinizi öğrenin.
keywords: mysql bağlantısı,bağlantı dizesi,bağlantı sorunları,geçici hata,bağlantı hatası
author: jan-eng
ms.author: janeng
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 26a6ac4412f1dff450cc087382dc9b0fce443f0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532204"
---
# <a name="handling-of-transient-connectivity-errors-for-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı için geçici bağlantı hatalarının işlenmesi

Bu makalede, MariaDB için Azure Veritabanı'na bağlanan geçici hataların nasıl işleyeceğiniaçıklanmaktadır.

## <a name="transient-errors"></a>Geçici hatalar

Geçici hata olarak da bilinen geçici hata, kendi kendini çözecek bir hatadır. Genellikle bu hatalar bırakılan veritabanı sunucusuna bir bağlantı olarak tezahür eder. Ayrıca bir sunucuya yeni bağlantılar açılamaz. Geçici hatalar örneğin donanım veya ağ hatası oluştuğunda oluşabilir. Başka bir nedeni dışarı haddelenmiş olan bir PaaS hizmetinin yeni bir sürümü olabilir. Bu olayların çoğu sistem tarafından 60 saniyeden kısa bir sürede otomatik olarak azaltılır. Bulutta uygulama tasarlamak ve geliştirmek için en iyi uygulama geçici hatalar beklemektir. Herhangi bir bileşende herhangi bir zamanda gerçekleşebileceğini ve bu durumları işlemek için uygun mantığa sahip olabileceklerini varsayalım.

## <a name="handling-transient-errors"></a>Geçici hataları işleme

Geçici hatalar yeniden deneme mantığı kullanılarak ele alınmalıdır. Göz önünde bulundurulması gereken durumlar:

* Bağlantıyı açmaya çalıştığınızda bir hata oluşur
* Sunucu tarafına boşta bağlantı bırakılır. Bir komut vermeye çalıştığınızda yürütülemez
* Şu anda bir komutuyguluyor etkin bir bağlantı bırakılır.

Birinci ve ikinci durumda oldukça düz ele almak için vardır. Bağlantıyı yeniden açmaya çalışın. Başarılı olduğunuzda, geçici hata sistem tarafından azaltıldı. MariaDB için Azure Veritabanınızı yeniden kullanabilirsiniz. Bağlantıyı yeniden denemeden önce beklemenizi öneririz. İlk yeniden denemeler başarısız olursa geri çek. Bu şekilde sistem hata durumunu aşmak için kullanılabilir tüm kaynakları kullanabilirsiniz. Takip etmek için iyi bir desen:

* İlk denemenizden önce 5 saniye bekleyin.
* Her bir yeniden deneme için, 60 saniyeye kadar bekleme yi katlanarak artırır.
* Uygulamanızın işlemin başarısız olduğunu düşündüğü en yüksek yeniden deneme sayısını ayarlayın.

Etkin bir işlemle bağlantı başarısız olduğunda, kurtarmaişlemini doğru şekilde işlemek daha zordur. İki durum vardır: Hareket yalnızca salt okunursa, bağlantıyı yeniden açmak ve hareketi yeniden denemek güvenlidir. Ancak, hareket veritabanına da yazıyorsa, hareketin geri alınıp alınıp alınıp alınıp alınmayacağına veya geçici hata gerçekleşmeden önce başarılı olup olmadığını belirlemeniz gerekir. Bu durumda, veritabanı sunucusundan taahhüt bildirimi almamış olabilirsiniz.

Bunu yapmanın bir yolu, tüm yeniden denemeler için kullanılan istemci üzerinde benzersiz bir kimlik oluşturmaktır. Bu benzersiz kimliği işlemin bir parçası olarak sunucuya aktarın ve benzersiz bir kısıtlaması olan bir sütunda saklarsınız. Bu şekilde hareketi güvenli bir şekilde yeniden deneyebilirsiniz. Önceki işlem geri alındıysa ve istemci sistemde henüz benzersiz bir kimlik oluşturmazsa başarılı olur. Önceki işlem başarıyla tamamlandığı için, benzersiz kimlik daha önce depolanmışsa yinelenen anahtar ihlalini gösteren başarısız olur.

Programınız Üçüncü taraf ara yazılımlar aracılığıyla MariaDB için Azure Veritabanı ile iletişim kurduğunda, satıcıya ara yazılımın geçici hatalar için yeniden deneme mantığı içerip içermediğini sorun.

Mantığınızı yeniden denediğinizi test ettiğinizden emin olun. Örneğin, MariaDB sunucusu için Azure Veritabanınızın bilgi işlem kaynaklarını ölçeklerken veya küçülterek kodunuzu yürütmeyi deneyin. Uygulamanız, bu işlem sırasında karşılaşılan kısa kapalı kalma süresini sorunsuz bir şekilde ele almalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [MariaDB için Azure Veritabanı bağlantı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)
