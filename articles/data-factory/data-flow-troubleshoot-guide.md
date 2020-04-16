---
title: Veri akışlarını giderme
description: Azure Veri Fabrikası'nda veri akışı sorunlarını nasıl gidereceklerini öğrenin.
services: data-factory
ms.author: makromer
author: kromerm
manager: anandsub
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 04/02/2020
ms.openlocfilehash: e9e9b10cc9bae029fe11fb2bd1f8b76cf120744a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417819"
---
# <a name="troubleshoot-data-flows-in-azure-data-factory"></a>Azure Veri Fabrikası'nda veri akışlarını giderme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Azure Veri Fabrikası'nda veri akışları için yaygın sorun giderme yöntemleri inceleilmiştir.

## <a name="common-errors-and-messages"></a>Sık karşılaşılan hatalar ve iletiler

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Hata kodu: DF-Executor-SourceInvalidPayload
- **İleti**: Kapsayıcı olmadığı için veri önizleme, hata ayıklama ve veri akışı yürütme başarısız oldu
- **Nedenleri**: Dataset depolama alanında bulunmayan bir kapsayıcı içeriyorsa
- **Öneri**: Veri setinizde başvurulan kapsayıcının var olduğundan veya erişilebilir olduğundan emin olun.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Hata kodu: DF-Executor-SystemImplicitCartesian

- **Mesaj**: INNER join için örtük kartezyen ürün desteklenmez, bunun yerine CROSS JOIN'i kullanın. Birleştirme'de kullanılan sütunlar satırlar için benzersiz bir anahtar oluşturmalıdır.
- **Nedenleri**: Mantıksal planlar arasında INNER birleştirme için örtük kartezyen ürün desteklenmez. Birleştirme'de kullanılan sütunlar benzersiz anahtar oluşturursa, ilişkinin her iki tarafından da en az bir sütun gerekir.
- **Öneri**: Eşitliğe dayalı olmayan birleştirmeler için CUSTOM CROSS JOIN'i tercih etmeniz gerekir.

### <a name="error-code-df-executor-systeminvalidjson"></a>Hata kodu: DF-Executor-SystemInvalidJson

- **İleti**: JSON ayrıştma hatası, desteklenmeyen kodlama veya çok satırlı
- **Nedenleri**: JSON dosyasıyla ilgili olası sorunlar: desteklenmeyen kodlama, bozuk baytlar veya JSON kaynağını iç içe birçok satırda tek bir belge olarak kullanma
- **Öneri**: JSON dosyasının kodlamasının desteklendigini doğrulayın. JSON veri kümesini kullanan Kaynak dönüşümünde ,'JSON Ayarları'nı genişletin ve 'Tek Belge'yi açın.
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Hata kodu: DF-Executor-BroadcastTimeout

- **İleti**: Yayın birleştirme zaman aşımı hatası, yayın akışının hata ayıklama çalıştırmalarında 60 saniye ve iş çalıştırmalarında 300 saniye içinde veri ürettiğinden emin olun
- **Nedenleri**: Yayın, hata ayıklama çalıştırmalarında 60 saniye ve iş çalıştırmalarında 300 saniye lik varsayılan zaman aşımına sahiptir. Yayın için seçilen akış, bu sınır içinde veri üretmek için büyük görünüyor.
- **Öneri**: İşlemin 60 saniyeden fazla sürebileceği büyük veri akışları yayınlamaktan kaçının. Bunun yerine yayın için daha küçük bir akış seçin. Büyük SQL/DW tabloları ve kaynak dosyaları genellikle kötü adaylardır.

### <a name="error-code-df-executor-conversion"></a>Hata kodu: DF-Executor-Dönüşüm

- **İleti**: Geçersiz bir karakter nedeniyle başarısız olan bir tarihe veya saate dönüştürme
- **Nedenleri**: Veriler beklenen biçimde değil
- **Öneri**: Doğru veri türünü kullanın

### <a name="error-code-df-executor-invalidcolumn"></a>Hata kodu: DF-Executor-Geçersiz Sütun

- **İleti**: Sütun adı sorguda belirtilmelidir, sql işlevi kullanıyorsanız bir takma ad ayarlama
- **Nedenleri**: Sütun adı belirtilmedi
- **Öneri**: Min()/max(), vb. gibi bir SQL işlevi kullanıyorsanız bir diğer ad ayarlayın.

### <a name="error-code-getcommand-outputasync-failed"></a>Hata kodu: GetCommand OutputAsync başarısız oldu

- **İleti**: Veri Akışı hata ayıklama ve veri önizlemesi sırasında: GetCommand OutputAsync ile başarısız oldu ...
- **Nedenleri**: Bu bir arka uç hizmet hatasıdır. İşlemi yeniden deneyebilir ve hata ayıklama oturumunuzu yeniden başlatabilirsiniz.
- **Öneri**: Yeniden deneyin ve yeniden başlatın sorunu çözmezse, müşteri desteğine başvurun.

### <a name="error-code-hit-unexpected-exception-and-execution-failed"></a>Hata kodu: Beklenmeyen özel durum isabet ve yürütme başarısız oldu

- **İleti**: Veri Akışı etkinlik yürütme sırasında: Hit beklenmeyen özel durum ve yürütme başarısız oldu.
- **Nedenleri**: Bu bir arka uç hizmet hatasıdır. İşlemi yeniden deneyebilir ve hata ayıklama oturumunuzu yeniden başlatabilirsiniz.
- **Öneri**: Yeniden deneyin ve yeniden başlatın sorunu çözmezse, müşteri desteğine başvurun.

## <a name="general-troubleshooting-guidance"></a>Genel sorun giderme kılavuzu

1. Veri kümesi bağlantılarınızın durumunu kontrol edin. Her Kaynak ve Lavabo dönüşümünde, kullandığınız her veri kümesi için Bağlantılı Hizmet'i ziyaret edin ve bağlantıları test edin.
1. Veri akışı tasarımcısından dosyanızın ve tablo bağlantılarınızın durumunu denetleyin. Veri Ayıklama'yı açın ve verilerinize erişebildiğinizden emin olmak için Kaynak dönüşümlerinizde Veri Önizleme'yi tıklatın.
1. Veri önizlemesinden her şey iyi görünüyorsa, Pipeline tasarımcısına gidin ve veri akışınızı bir boru hattı etkinliğine koyun. Uç-uç testi için ardışık bölümü hata ayıklama.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme yardımı için şu kaynakları deneyin:
*  [Veri Fabrikası günlüğü](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Veri Fabrikası özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN forumu](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Veri Fabrikası için Yığın Taşma forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Veri Fabrikası hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
*  [ADF haritalama veri akışları Performans Kılavuzu](concepts-data-flow-performance.md)
