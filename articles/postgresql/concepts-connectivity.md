---
title: Geçici bağlantı hatalarını işleme-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı-tek sunucu için geçici bağlantı hatalarını nasıl işleyeceğinizi öğrenin.
keywords: PostgreSQL bağlantısı, bağlantı dizesi, bağlantı sorunları, geçici hata, bağlantı hatası
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 026a0edf24d349c4b445d6229d3b1ad73decf87d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82097838"
---
# <a name="handling-transient-connectivity-errors-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı'nda geçici bağlantı hatalarını işleme - Tek Sunucu

Bu makalede PostgreSQL için Azure veritabanı 'na bağlanan geçici hataların nasıl işleneceği açıklanır.

## <a name="transient-errors"></a>Geçici hatalar

Geçici arıza olarak da bilinen geçici bir hata, kendisini çözecek bir hatadır. Genellikle bu hatalar, bırakılmakta olan veritabanı sunucusuna bağlantı olarak bildirimde yer vardır. Ayrıca sunucuya yeni bağlantılar açılamaz. Geçici hatalar, örneğin, donanım veya ağ hatası oluştuğunda meydana gelebilir. Başka bir nedenden dolayı PaaS hizmetinin kullanıma alınmış yeni bir sürümü olabilir. Bu olayların çoğu, sistem tarafından 60 saniyeden daha az bir süre içinde otomatik olarak azalır. Bulutta uygulama tasarlamaya ve geliştirmeye yönelik en iyi yöntem geçici hataların beklenmektedir. Her zaman herhangi bir bileşende ve bu durumları işlemek için uygun mantığın yerine gelebileceğini varsayın.

## <a name="handling-transient-errors"></a>Geçici hataları işleme

Geçici hatalar, yeniden deneme mantığı kullanılarak işlenmelidir. Göz önünde bulundurulması gereken durumlar:

* Bir bağlantıyı açmaya çalıştığınızda bir hata oluşur
* Sunucu tarafında boştaki bir bağlantı bırakılır. Bir komut verilmenize çalıştığınızda yürütülemiyor
* Şu anda bir komutun yürütüldüğü etkin bir bağlantı bırakılır.

İlk ve ikinci durum, işlemek için oldukça ileri bir işlemdir. Bağlantıyı tekrar açmayı deneyin. Başarılı olduğunuzda, geçici hata sistem tarafından azaltılmıştır. PostgreSQL için Azure veritabanınızı yeniden kullanabilirsiniz. Bağlantıyı yeniden denemeden önce beklemeleri önerilir. İlk yeniden denemeler başarısız olursa geri dönün. Bu şekilde, sistem hata durumu aşmak için kullanılabilen tüm kaynakları kullanabilir. İzlenecek iyi bir örüntü şunlardır:

* İlk yeniden denemeden önce 5 saniye bekleyin.
* Aşağıdaki her yeniden denemede, bekleme süresini 60 saniyeye kadar artırın.
* Uygulamanızın işlemi başarısız olduğu noktada en fazla yeniden deneme sayısını ayarlayın.

Etkin bir işlem başarısız olursa, kurtarmanın doğru şekilde işlenmesi daha zordur. İki durum vardır: işlem, doğası halinde salt okunurdur, bağlantıyı yeniden açmak ve işlemi yeniden denemek güvenlidir. Ancak işlem veritabanına yazıyorsanız, işlemin geri alınması veya geçici hata yapılmadan önce başarılı olup olmadığını belirlemelisiniz. Bu durumda, yalnızca veritabanı sunucusundan kayıt onayını almamış olabilirsiniz.

Bunu yapmanın bir yolu, istemci üzerinde tüm yeniden denemeler için kullanılan benzersiz bir KIMLIK oluşturmak içindir. Bu benzersiz KIMLIĞI işlemin bir parçası olarak sunucuya geçirin ve benzersiz bir kısıtlamaya sahip bir sütunda saklayın. Bu şekilde işlemi güvenle yeniden deneyebilirsiniz. Önceki işlem geri alınırsa ve istemci tarafından oluşturulan benzersiz KIMLIK henüz sistemde yoksa başarılı olur. Önceki işlem başarıyla tamamlandığından, benzersiz KIMLIK daha önce depolanıyorsa yinelenen bir anahtar ihlaline işaret eder.

Programınız üçüncü taraf ara yazılım aracılığıyla PostgreSQL için Azure veritabanı ile iletişim kurduğunda, satıcıdan geçici hatalar için yeniden deneme mantığı içerip içermediğini öğrenin.

Yeniden deneme mantığını test ettiğinizden emin olun. Örneğin, PostgreSQL için Azure veritabanı sunucusu için işlem kaynaklarını ölçeklendirirken veya azaltılarken kodunuzu yürütmeyi deneyin. Uygulamanız herhangi bir sorun olmadan bu işlem sırasında karşılaşılan kısa kapalı kalma süresini işlemelidir.

## <a name="next-steps"></a>Sonraki adımlar

* [PostgreSQL için Azure Veritabanı bağlantısı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)
