---
title: Veri akışları sorunlarını giderme
description: Azure Data Factory 'de veri akışı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
ms.author: makromer
author: kromerm
manager: anandsub
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.openlocfilehash: 2edd5b661240b6156cf8a02059b2b9a668c402f3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83829129"
---
# <a name="troubleshoot-data-flows-in-azure-data-factory"></a>Azure Data Factory veri akışları sorunlarını giderme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede Azure Data Factory veri akışları için genel sorun giderme yöntemleri incelenmektedir.

## <a name="common-errors-and-messages"></a>Sık karşılaşılan hatalar ve mesajlar

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Hata kodu: DF-yürütücü-Sourceınvalidpayload
- **İleti**: kapsayıcı mevcut olmadığından veri önizleme, hata ayıklama ve işlem hattı veri akışı yürütme başarısız oldu
- **Nedenler**: veri kümesi depolamada bulunmayan bir kapsayıcı içerdiğinde
- **Öneri**: veri kümenizde başvurulan kapsayıcının bulunduğundan veya erişilebilir olduğundan emin olun.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Hata kodu: DF-yürütücü-Systemımplicıtkartezyen

- **İleti**: iç birleşim için örtülü Kartezyen ürün desteklenmiyor, bunun yerıne çapraz birleşim kullanın. Birleşimde kullanılan sütunlar, satırlar için benzersiz bir anahtar oluşturmaktır.
- **Nedenler**: mantıksal planlar arasında iç birleşim için örtülü Kartezyen ürün desteklenmez. Birleşimde kullanılan sütunlar benzersiz anahtarı oluşturursa, ilişkinin her iki taraflarından en az bir sütun gereklidir.
- **Öneri**: eşitlik tabanlı olmayan BIRLEŞTIRMELER IÇIN özel çapraz katılmayı kabul etmeniz gerekir.

### <a name="error-code-df-executor-systeminvalidjson"></a>Hata kodu: DF-yürütücü-SystemInvalidJson

- **İleti**: JSON ayrıştırma hatası, desteklenmeyen kodlama veya çok satırlı
- **Nedenler**: JSON dosyası ile ilgili olası sorunlar: desteklenmeyen kodlama, bozuk BAYTLAR veya JSON kaynağı birçok iç içe satırda tek bir belge olarak kullanılıyor
- **Öneri**: JSON dosyasının kodlamasının desteklendiğini doğrulayın. JSON veri kümesi kullanan kaynak dönüşümünde, ' JSON ayarları ' ' nı genişletin ve ' tek belge ' öğesini açın.
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Hata kodu: DF-yürütücü-Yayınzaman aşımı

- **İleti**: yayın birleşimi zaman aşımı hatası, yayın akışının hata ayıklama çalıştırmaları ve 300 saniye içinde iş çalıştırmaları sırasında 60 saniye içinde veri ürettiğinden emin olun
- **Nedenler**: yayının hata ayıklama çalıştırmaları ve 300 saniye olarak iş çalıştırmaları sırasında varsayılan 60 saniye zaman aşımı süresi vardır. Yayın için seçilen akış, bu sınırın içinde veri üretmek için büyük görünüyor.
- **Öneri**: JOIN, Exists ve Lookup için veri akışı dönüştürmelerinizin en iyileştirme sekmesini kontrol edin. Yayın için varsayılan seçenek "Auto" dır. Bu ayarlandıysa veya "fixed" altında yayın için sol veya sağ tarafını el ile ayarlıyorsanız, daha büyük bir Azure Integration Runtime yapılandırması ayarlayabilir veya yayını kapatabilirsiniz. Veri akışlarında en iyi performans için önerilen yaklaşım, Spark 'ın "Auto" kullanarak yayınmasına ve bellek için Iyileştirilmiş Azure IR kullanmasına izin vermedir.

### <a name="error-code-df-executor-conversion"></a>Hata kodu: DF-yürütücü-dönüştürme

- **İleti**: geçersiz bir karakter nedeniyle tarih veya saate dönüştürme başarısız oldu
- **Nedenler**: veriler beklenen biçimde değil
- **Öneri**: doğru veri türünü kullanın

### <a name="error-code-df-executor-invalidcolumn"></a>Hata kodu: DF-yürütücü-ınvalidcolumn

- **İleti**: sorguda sütun adının belirtilmesi gerekir, bir SQL işlevi kullanılıyorsa bir diğer ad ayarlayın
- **Nedenler**: sütun adı belirtilmedi
- **Öneri**: min ()/Max () vb. gıbı bir SQL işlevi kullanılıyorsa bir diğer ad ayarlayın.

### <a name="error-code-getcommand-outputasync-failed"></a>Hata kodu: GetCommand OutputAsync başarısız oldu

- **İleti**: veri akışı hata ayıklama ve veri önizleme sırasında: GetCommand OutputAsync ile başarısız oldu...
- **Nedenler**: Bu bir arka uç hizmet hatasıdır. İşlemi yeniden deneyebilir ve hata ayıklama oturumunuzu da yeniden başlatabilirsiniz.
- **Öneri**: yeniden dene ve yeniden Başlat sorunu çözmezse, müşteri desteğine başvurun.

### <a name="error-code-hit-unexpected-exception-and-execution-failed"></a>Hata kodu: beklenmeyen özel duruma ulaşıldı ve yürütme başarısız oldu

- **İleti**: veri akışı etkinlik yürütmesi sırasında: beklenmeyen bir özel durum ve yürütme başarısız oldu.
- **Nedenler**: Bu bir arka uç hizmet hatasıdır. İşlemi yeniden deneyebilir ve hata ayıklama oturumunuzu da yeniden başlatabilirsiniz.
- **Öneri**: yeniden dene ve yeniden Başlat sorunu çözmezse, müşteri desteğine başvurun.

## <a name="general-troubleshooting-guidance"></a>Genel sorun giderme kılavuzu

1. Veri kümesi bağlantılarınızın durumunu denetleyin. Her kaynak ve havuz dönüşümünde, kullandığınız her veri kümesi için bağlı hizmeti ziyaret edin ve bağlantıları test edin.
1. Veri akışı tasarımcısından dosya ve tablo bağlantılarınızın durumunu denetleyin. Hata ayıklama üzerinde geçiş yapın ve verilerinize erişebildiğinizden emin olmak için kaynak dönüşümlerinizin veri önizlemesine tıklayın.
1. Her şey veri önizlemesinden iyi görünüyorsa, işlem hattı tasarımcısına gidin ve veri akışınızı bir işlem hattı etkinliğine yerleştirin. Uçtan uca bir test için işlem hattının hatalarını ayıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:
*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
*  [ADF eşleme veri akışları performans Kılavuzu](concepts-data-flow-performance.md)
