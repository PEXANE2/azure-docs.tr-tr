---
title: Veri akışlarını eşleme sorunlarını giderme
description: Azure Data Factory 'de veri akışı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
ms.author: makromer
author: kromerm
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 09/11/2020
ms.openlocfilehash: cc87694686bd5143b03d690286bd3171cf8b0e18
ms.sourcegitcommit: 983eb1131d59664c594dcb2829eb6d49c4af1560
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99222158"
---
# <a name="troubleshoot-mapping-data-flows-in-azure-data-factory"></a>Azure Data Factory veri akışlarını eşleme sorunlarını giderme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Azure Data Factory veri akışlarını eşlemek için ortak sorun giderme yöntemleri incelenmektedir.

## <a name="common-errors-and-messages"></a>Sık karşılaşılan hatalar ve mesajlar

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Hata kodu: DF-yürütücü-Sourceınvalidpayload
- **İleti**: kapsayıcı mevcut olmadığından veri önizleme, hata ayıklama ve işlem hattı veri akışı yürütme başarısız oldu
- **Nedenler**: veri kümesi depolamada bulunmayan bir kapsayıcı içerdiğinde
- **Öneri**: veri kümenizde başvurulan kapsayıcının bulunduğundan veya erişilebilir olduğundan emin olun.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Hata kodu: DF-yürütücü-Systemımplicıtkartezyen

- **İleti**: iç birleşim için örtülü Kartezyen ürün desteklenmiyor, bunun yerıne çapraz birleşim kullanın. Birleşimde kullanılan sütunlar, satırlar için benzersiz bir anahtar oluşturmaktır.
- **Nedenler**: mantıksal planlar arasında iç birleşim için örtülü Kartezyen ürün desteklenmez. Birleşimde kullanılan sütunlar benzersiz anahtarı oluştururken, ilişkinin her iki tarafındaki en az bir sütun gereklidir.
- **Öneri**: eşitlik tabanlı olmayan BIRLEŞTIRMELER IÇIN özel çapraz katılmayı kabul etmeniz gerekir.

### <a name="error-code-df-executor-systeminvalidjson"></a>Hata kodu: DF-yürütücü-SystemInvalidJson

- **İleti**: JSON ayrıştırma hatası, desteklenmeyen kodlama veya çok satırlı
- **Nedenler**: JSON dosyası ile ilgili olası sorunlar: desteklenmeyen kodlama, bozuk BAYTLAR veya JSON kaynağı birçok iç içe satırda tek bir belge olarak kullanılıyor
- **Öneri**: JSON dosyasının kodlamasının desteklendiğini doğrulayın. JSON veri kümesi kullanan kaynak dönüşümünde, ' JSON ayarları ' ' nı genişletin ve ' tek belge ' öğesini açın.
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Hata kodu: DF-yürütücü-Yayınzaman aşımı

- **İleti**: yayın birleşimi zaman aşımı hatası, yayın akışının hata ayıklama çalıştırmaları ve 300 saniye içinde iş çalıştırmaları sırasında 60 saniye içinde veri ürettiğinden emin olun
- Nedenler: yayın, hata ayıklama çalıştırmaları ve iş çalıştırmalarının 300 saniye içinde varsayılan 60 saniye zaman aşımına **neden olur**. Yayın için seçilen akış bu sınırın içinde veri üretmede çok büyük görünüyor.
- **Öneri**: JOIN, Exists ve Lookup için veri akışı dönüştürmelerinizin en iyileştirme sekmesini kontrol edin. Yayın için varsayılan seçenek "Auto" dır. "Auto" ayarlandıysa veya sol veya sağ tarafı "düzeltildi" altında yayınlanacak şekilde ayarlıyorsanız, daha büyük bir Azure Integration Runtime yapılandırması ayarlayabilir veya yayını kapatabilirsiniz. Veri akışlarında en iyi performans için önerilen yaklaşım, Spark 'ın "Auto" kullanarak yayınmasına ve bellek için Iyileştirilmiş Azure IR kullanmasına izin vermedir.

Hata ayıklama işlem hattı çalıştırmasıyla veri akışını bir hata ayıklama testi yürütmesinde yürütüyorsunuz, Bu koşulla daha sık karşılaşabilirsiniz. Bunun nedeni, ADF 'nin yayını daha hızlı bir hata ayıklama deneyimi sağlamak için yayın zaman aşımını 60 saniye olarak kısıtlar. Bunu tetiklenen bir çalıştırmadan 300 saniyelik zaman aşımına göre uzatmak isterseniz, veri akışı işlem hattı yürütme etkinliğinizdeki tanımlı Azure IR kullanmak için, etkinlik çalışma zamanı kullan > hata ayıkla seçeneğini kullanabilirsiniz.

### <a name="error-code-df-executor-conversion"></a>Hata kodu: DF-yürütücü-dönüştürme

- **İleti**: geçersiz bir karakter nedeniyle tarih veya saate dönüştürme başarısız oldu
- **Nedenler**: veriler beklenen biçimde değil
- **Öneri**: doğru veri türünü kullanın

### <a name="error-code-df-executor-invalidcolumn"></a>Hata kodu: DF-yürütücü-ınvalidcolumn

- **İleti**: sorguda sütun adının belirtilmesi gerekir, bir SQL işlevi kullanılıyorsa bir diğer ad ayarlayın
- **Nedenler**: sütun adı belirtilmedi
- **Öneri**: min ()/Max () vb. gıbı bir SQL işlevi kullanılıyorsa bir diğer ad ayarlayın.

 ### <a name="error-code-df-executor-drivererror"></a>Hata kodu: DF-yürütücü-DriverError
- **İleti**: INT96, ADF veri akışı tarafından desteklenmeyen eski zaman damgası türüdür. Lütfen sütun türünü en son türlere yükseltmeyi göz önünde bulundurun.
- **Nedenler**: sürücü hatası
- **Öneri**: INT96, ADF veri akışı tarafından desteklenmeyen eski zaman damgası türüdür. Sütun türünü en son türlere yükseltmeyi göz önünde bulundurun.

 ### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Hata kodu: DF-yürütücü-Blockcountexceedslimıterror
- **İleti**: kaydedilmemiş blok sayısı 100.000 blok üst sınırını aşamaz. Blob yapılandırmasını denetleyin.
- **Nedenler**: bir blob 'da en fazla 100.000 kaydedilmemiş blok olabilir.
- **Öneri**: daha fazla bilgi için bu sorunla Ilgili olarak Microsoft ürün ekibine başvurun

 ### <a name="error-code-df-executor-partitiondirectoryerror"></a>Hata kodu: DF-yürütücü-PartitionDirectoryError
- **İleti**: belirtilen kaynak yolu birden çok bölümlenmiş dizine sahip (örn. <Source Path> /<bölüm kök dizini 1>/a = 10/b = 20, <Source Path> /<bölüm kök dizin 2>/c = 10/d = 30) veya diğer dosya ya da bölümlenmemiş dizin ile bölümlenmiş dizin (örneğin <Source Path> /<bölüm kök dizini 1>/a = 10/b = 20, <Source Path> /Dizin 2/FILE1), bölüm kök dizinini kaynak yolundan kaldırın ve ayrı kaynak dönüşümünde okuyun.
- **Nedenler**: kaynak yolunda birden çok bölümlenmiş dizin veya diğer dosya ya da bölümlenmemiş dizine sahip bölümlenmiş dizin bulunur.
- **Öneri**: bölümlenmiş kök dizini kaynak yolundan kaldırın ve ayrı kaynak dönüşümünde okuyun.

 ### <a name="error-code-df-executor-outofmemoryerror"></a>Hata kodu: DF-yürütücü-OutOfMemoryError
- **İleti**: küme, yürütme sırasında yetersiz bellek sorunuyla karşılaştı, lütfen daha büyük çekirdek sayısı ve/veya bellek için iyileştirilmiş işlem türü ile bir tümleştirme çalışma zamanı kullanmayı deneyin
- **Nedenler**: kümede bellek tükençalışıyor
- **Öneri**: hata ayıklama kümeleri geliştirme amaçlarıyla yöneliktir. Yük çalıştırmak için veri örnekleme, uygun işlem türü ve boyuttan yararlanın. En iyi performansı elde etmek için ayarlama için [veri akışı performansını eşleme kılavuzuna](concepts-data-flow-performance.md) bakın.

 ### <a name="error-code-df-executor-illegalargument"></a>Hata kodu: DF-yürütücü-düzeneği Galargument
- **İleti**: lütfen bağlı hizmetinizdeki erişim anahtarının doğru olduğundan emin olun
- **Nedenler**: hesap adı veya erişim anahtarı yanlış
- **Öneri**: bağlı hizmetinizde belirtilen hesap adının veya erişim anahtarının doğru olduğundan emin olun. 

 ### <a name="error-code-df-executor-invalidtype"></a>Hata kodu: DF-yürütücü-ınvalidtype
- **İleti**: lütfen parametre türünün geçirilen değer türüyle eşleştiğinden emin olun. İşlem hatlarından kayan parametrelerin geçirilmesi Şu anda desteklenmiyor.
- **Nedenler**: belirtilen tür ile gerçek parametre değeri arasında uyumsuz veri türleri
- **Öneri**: bir veri akışına geçirilen parametre değerlerinizin, belirtilen türle eşleştiğinden emin olun.

 ### <a name="error-code-df-executor-columnunavailable"></a>Hata kodu: DF-yürütücü-ColumnUnavailable
- **İleti**: ifadede kullanılan sütun adı kullanılamıyor veya geçersiz
- **Nedenler**: ifadelerde geçersiz veya kullanılamayan sütun adı kullanıldı
- **Öneri**: ifadelerde kullanılan sütun adlarını denetle

 ### <a name="error-code-df-executor-parseerror"></a>Hata kodu: DF-yürütücü-ParseError
- **İleti**: ifade ayrıştırılamıyor
- **Nedenler**: ifadede biçimlendirme nedeniyle hatalar ayrıştırılıyor
- **Öneri**: ifadede biçimlendirmeyi denetle

### <a name="error-code-getcommand-outputasync-failed"></a>Hata kodu: GetCommand OutputAsync başarısız oldu

- **İleti**: veri akışı hata ayıklama ve veri önizleme sırasında: GetCommand OutputAsync ile başarısız oldu...
- **Nedenler**: Bu bir arka uç hizmet hatasıdır. İşlemi yeniden deneyebilir ve hata ayıklama oturumunuzu da yeniden başlatabilirsiniz.
- **Öneri**: yeniden dene ve yeniden Başlat sorunu çözmezse, müşteri desteğine başvurun.

### <a name="error-code-hit-unexpected-exception-and-execution-failed"></a>Hata kodu: beklenmeyen özel duruma ulaşıldı ve yürütme başarısız oldu

- **İleti**: veri akışı etkinlik yürütmesi sırasında: beklenmeyen bir özel durum ve yürütme başarısız oldu.
- **Nedenler**: Bu bir arka uç hizmet hatasıdır. İşlemi yeniden deneyebilir ve hata ayıklama oturumunuzu da yeniden başlatabilirsiniz.
- **Öneri**: yeniden dene ve yeniden Başlat sorunu çözmezse, müşteri desteğine başvurun.

### <a name="error-code-debug-data-preview-no-output-data-on-join"></a>Hata kodu: hata ayıklama veri önizlemesi, birleşimde çıkış verisi yok

- **İleti**: çok az sayıda satır örneklenmemesinin nedeni büyük miktarda null değer veya eksik değer vardır. Hata ayıklama satırı sınırını güncelleştirmeyi ve verileri yenilemeyi deneyin.
- **Nedenler**: JOIN koşulu hiçbir satırla eşleşmedi veya veri önizleme sırasında yüksek sayıda null değer ile sonuçlandı.
- **Öneri**: hata ayıklama ayarları ' na gidin ve kaynak satır sınırında satır sayısını artırın. Daha fazla veri işlemek için yeterli miktarda veri akışı kümesine sahip bir Azure IR seçtiğinizden emin olun.

### <a name="error-code-validation-error-at-source-with-multiline-csv-files"></a>Hata kodu: çok satırlı CSV dosyaları ile kaynakta doğrulama hatası 

- **İleti**: aşağıdaki hata iletilerinden birini görebilirsiniz:
   - Son sütun null veya eksik.
   - Kaynaktaki şema doğrulaması başarısız oluyor.
   - Şema içeri aktarma, UX 'de doğru şekilde gösterilemedi ve son sütunda adında yeni bir satır karakteri bulunur.
- **Nedenler**: Şu anda çok satırlı CSV kaynağı olan eşleme veri akışında, \r\n as satır sınırlayıcısı ile çalışmaz. Bazen satır içindeki ek satırlar, kesme kaynağı değerlerini döndürür. 
- **Öneri**: dosya kaynağında \n yerine bir satır sınırlayıcısı olarak bir dosya oluşturun. Ya da bir satır sınırlayıcısı olarak \r\n ile \n arasında CSV dosyasını dönüştürmek için kopyalama etkinliğini kullanın.

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Hata kodu: DF-yürütücü-Sourceınvalidpayload
- **İleti**: kapsayıcı mevcut olmadığından veri önizleme, hata ayıklama ve işlem hattı veri akışı yürütme başarısız oldu
- **Nedenler**: veri kümesi depolamada bulunmayan bir kapsayıcı içerdiğinde
- **Öneri**: veri kümenizde başvurulan kapsayıcının bulunduğundan veya erişilebilir olduğundan emin olun.


 ### <a name="error-code-df-executor-systemimplicitcartesian"></a>Hata kodu: DF-yürütücü-Systemımplicıtkartezyen
- **İleti**: iç birleşim için örtülü Kartezyen ürün desteklenmiyor, bunun yerıne çapraz birleşim kullanın. Birleşimde kullanılan sütunlar, satırlar için benzersiz bir anahtar oluşturmaktır.
- **Nedenler**: mantıksal planlar arasında iç birleşim için örtülü Kartezyen ürün desteklenmez. Birleşimde kullanılan sütunlar benzersiz anahtarı oluşturursa
- **Öneri**: eşitlik tabanlı olmayan BIRLEŞTIRMELER IÇIN çapraz katılmayı kabul etmeniz gerekir.


 ### <a name="error-code-df-executor-systeminvalidjson"></a>Hata kodu: DF-yürütücü-SystemInvalidJson
- **İleti**: JSON ayrıştırma hatası, desteklenmeyen kodlama veya çok satırlı
- **Nedenler**: JSON dosyası ile ilgili olası sorunlar: desteklenmeyen kodlama, bozuk BAYTLAR veya JSON kaynağı birçok iç içe satırda tek bir belge olarak kullanılıyor
- **Öneri**: JSON dosyasının kodlamasının desteklendiğini doğrulayın. JSON veri kümesi kullanan kaynak dönüşümünde, ' JSON ayarları ' ' nı genişletin ve ' tek belge ' öğesini açın.


 ### <a name="error-code-df-executor-broadcasttimeout"></a>Hata kodu: DF-yürütücü-Yayınzaman aşımı
- **İleti**: yayın birleştirmesi zaman aşımı hatası, bu sorundan kaçınmak için JOIN/Exists/Lookup dönüşümünde yayın ' ı ' seçeneğini belirleyebilirsiniz. Performansı artırmak için JOIN seçeneğini yayınlamak istiyorsanız, yayın akışının hata ayıklama çalıştırmaları ve 300 saniye içinde, iş çalıştırmaları sırasında 60 saniye içinde veri oluşturabilmesine dikkat edin.
- **Nedenler**: yayının hata ayıklama çalıştırmaları ve 300 saniye olarak iş çalıştırmaları sırasında varsayılan 60 saniye zaman aşımı süresi vardır. Yayın katılımı sırasında, yayın için seçilen akış bu sınırın içinde veri üretmek için çok büyük görünüyor. Bir yayın birleşimi kullanılmazsa, veri akışı tarafından gerçekleştirilen varsayılan yayın aynı sınıra ulaşabilir
- **Öneri**: yayın seçeneğini devre dışı bırakın veya işlemenin 60 saniyeden fazla sürebileceği büyük veri akışlarını yayınlamaktan kaçının. Bunun yerine, yayınlanacak daha küçük bir akış seçin. Büyük SQL/DW tabloları ve kaynak dosyaları genellikle hatalı adaylardır. Bir yayın katılımı yokluğunda, hata oluşursa daha büyük bir küme kullanın.


 ### <a name="error-code-df-executor-conversion"></a>Hata kodu: DF-yürütücü-dönüştürme
- **İleti**: geçersiz bir karakter nedeniyle tarih veya saate dönüştürme başarısız oldu
- **Nedenler**: veriler beklenen biçimde değil
- **Öneri**: doğru veri türünü kullanın


 ### <a name="error-code-df-executor-invalidcolumn"></a>Hata kodu: DF-yürütücü-ınvalidcolumn
- **İleti**: sorguda sütun adının belirtilmesi gerekir, bir SQL işlevi kullanılıyorsa bir diğer ad ayarlayın
- **Nedenler**: sütun adı belirtilmedi.


 ### <a name="error-code-df-executor-drivererror"></a>Hata kodu: DF-yürütücü-DriverError
- **İleti**: INT96, ADF veri akışı tarafından desteklenmeyen eski zaman damgası türüdür. Lütfen sütun türünü en son türlere yükseltmeyi göz önünde bulundurun.
- **Nedenler**: bir sürücü hatasıdır.
- **Öneri**: INT96, ADF veri akışı tarafından desteklenmeyen eski zaman damgası türüdür. Lütfen sütun türünü en son türlere yükseltmeyi göz önünde bulundurun.


 ### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Hata kodu: DF-yürütücü-Blockcountexceedslimıterror
- **İleti**: kaydedilmemiş blok sayısı 100.000 blok üst sınırını aşamaz. Blob yapılandırmasını denetleyin.
- **Nedenler**: bir blob 'da en fazla 100.000 kaydedilmemiş blok olabilir.
- **Öneri**: daha fazla bilgi için lütfen bu sorunla Ilgili olarak Microsoft ürün ekibine başvurun

 ### <a name="error-code-df-executor-partitiondirectoryerror"></a>Hata kodu: DF-yürütücü-PartitionDirectoryError
- **İleti**: belirtilen kaynak yolu birden çok bölümlenmiş dizine sahip (örn. <Source Path> /<bölüm kök dizini 1>/a = 10/b = 20, <Source Path> /<bölüm kök dizin 2>/c = 10/d = 30) veya diğer dosya ya da bölümlenmemiş dizin ile bölümlenmiş dizin (örn. <Source Path> /<bölüm kök dizini 1>/a = 10/b = 20, <Source Path> /Dizin 2/FILE1), bölüm kök dizinini kaynak yolundan kaldırın ve ayrı kaynak dönüşümünde okuyun.
- **Nedenler**: kaynak yolunda birden çok bölümlenmiş dizin veya diğer dosya ya da bölümlenmemiş dizine sahip bölümlenmiş dizin bulunur.
- **Öneri**: bölümlenmiş kök dizini kaynak yolundan kaldırın ve ayrı kaynak dönüşümünde okuyun.


 ### <a name="error-code-df-executor-outofmemoryerror"></a>Hata kodu: DF-yürütücü-OutOfMemoryError
- **İleti**: küme, yürütme sırasında yetersiz bellek sorunuyla karşılaştı, lütfen daha büyük çekirdek sayısı ve/veya bellek için iyileştirilmiş işlem türü ile bir tümleştirme çalışma zamanı kullanmayı deneyin
- **Nedenler**: kümede bellek tükençalışıyor.
- **Öneri**: hata ayıklama kümeleri geliştirme amaçlarıyla yöneliktir. Yük çalıştırmak için veri örneklemesi uygun işlem türü ve boyutundan yararlanın. En iyi performans için veri akışlarını ayarlamaya yönelik [veri akışı performans kılavuzuna](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-performance) bakın.


 ### <a name="error-code-df-executor-illegalargument"></a>Hata kodu: DF-yürütücü-düzeneği Galargument
- **İleti**: lütfen bağlı hizmetinizdeki erişim anahtarının doğru olduğundan emin olun.
- **Nedenler**: hesap adı veya erişim anahtarı yanlış.
- **Öneri**: Lütfen doğru hesap adı veya erişim anahtarı sağlayın.


 ### <a name="error-code-df-executor-invalidtype"></a>Hata kodu: DF-yürütücü-ınvalidtype
- **İleti**: lütfen parametre türünün geçirilen değer türüyle eşleştiğinden emin olun. İşlem hatlarından kayan parametrelerin geçirilmesi Şu anda desteklenmiyor.
- **Nedenler**: belirtilen tür ile gerçek parametre değeri arasında uyumsuz veri türleri
- **Öneri**: Lütfen doğru veri türleri sağlayın.


 ### <a name="error-code-df-executor-columnunavailable"></a>Hata kodu: DF-yürütücü-ColumnUnavailable
- **İleti**: ifadede kullanılan sütun adı kullanılamıyor veya geçersiz.
- **Nedenler**: ifadelerde geçersiz veya kullanılamayan sütun adı kullanılır.
- **Öneri**: ifadelerde kullanılan sütun adlarını denetleyin.


 ### <a name="error-code-df-executor-parseerror"></a>Hata kodu: DF-yürütücü-ParseError
- **İleti**: ifade ayrıştırılamıyor.
- **Nedenler**: ifade, biçimlendirme nedeniyle hataları ayrıştırmıştır.
- **Öneri**: ifadede biçimlendirmeyi denetleyin.


 ### <a name="error-code-df-executor-outofdiskspaceerror"></a>Hata kodu: DF-yürütücü-OutOfDiskSpaceError
- **İleti**: iç sunucu hatası
- **Nedenler**: kümede disk alanı tükeniyor.
- **Öneri**: Lütfen işlem hattını yeniden deneyin. Sorun devam ederse müşteri desteğine başvurun.


 ### <a name="error-code-df-executor-storeisnotdefined"></a>Hata kodu: DF-yürütücü-StoreIsNotDefined
- **İleti**: depo yapılandırması tanımlı değil. Bu hata, ardışık düzende geçersiz parametre atamasından kaynaklanıyor olabilir.
- **Nedenler**: belirlenmeyen
- **Öneri**: Lütfen işlem hattının parametre değeri atamasını denetleyin. Parametre ifadesi geçersiz karakterler içeriyor olabilir.


 ### <a name="error-code-df-excel-invalidconfiguration"></a>Hata kodu: DF-Excel-ınvalidconfiguration
- **İleti**: Excel sayfası adı veya dizini gereklidir.
- **Nedenler**: belirlenmeyen
- **Öneri**: lütfen parametre değerini denetleyin ve Excel verilerini okumak için sayfa adı ya da dizin belirtin.


 ### <a name="error-code-df-excel-invalidconfiguration"></a>Hata kodu: DF-Excel-ınvalidconfiguration
- **İleti**: Excel sayfası adı ve dizini aynı anda mevcut olamaz.
- **Nedenler**: belirlenmeyen
- **Öneri**: lütfen parametre değerini denetleyin ve Excel verilerini okumak için sayfa adı ya da dizin belirtin.


 ### <a name="error-code-df-excel-invalidconfiguration"></a>Hata kodu: DF-Excel-ınvalidconfiguration
- **İleti**: geçersiz Aralık belirtildi.
- **Nedenler**: belirlenmeyen
- **Öneri**: lütfen parametre değerini denetleyin ve başvuruya göre geçerli aralığı belirtin: [Excel özellikleri](https://docs.microsoft.com/azure/data-factory/format-excel#dataset-properties).


 ### <a name="error-code-df-excel-invaliddata"></a>Hata kodu: DF-Excel-ınvaliddata
- **İleti**: Excel çalışma sayfası yok.
- **Nedenler**: belirlenmeyen
- **Öneri**: lütfen parametre değerini denetleyin ve Excel verilerini okumak için geçerli sayfa adı veya dizin belirtin.

 ### <a name="error-code-df-excel-invaliddata"></a>Hata kodu: DF-Excel-ınvaliddata
- **İleti**: Excel dosyalarını farklı şemayla okuma işlemi şu anda desteklenmiyor.
- **Nedenler**: belirlenmeyen
- **Öneri**: doğru Excel dosyasını kullanın.


 ### <a name="error-code-df-excel-invaliddata"></a>Hata kodu: DF-Excel-ınvaliddata
- **İleti**: veri türü desteklenmiyor.
- **Nedenler**: belirlenmeyen
- **Öneri**: Excel dosyası sağ veri türlerini kullanın.

 ### <a name="error-code-df-excel-invalidconfiguration"></a>Hata kodu: DF-Excel-ınvalidconfiguration
- **İleti**: yalnızca. xlsx ve. xls desteklenirken geçersiz Excel dosyası sağlanıyor
- **Nedenler**: belirlenmeyen
- **Öneri**: Excel dosya uzantısının. xlsx veya. xls olduğundan emin olun.

## <a name="general-troubleshooting-guidance"></a>Genel sorun giderme kılavuzu
1. Veri kümesi bağlantılarınızın durumunu denetleyin. Her kaynak ve havuz dönüşümünde, kullandığınız her veri kümesi için bağlı hizmeti ziyaret edin ve bağlantıları test edin.
2. Veri akışı tasarımcısından dosya ve tablo bağlantılarınızın durumunu denetleyin. Hata ayıklama üzerinde geçiş yapın ve verilerinize erişebildiğinizden emin olmak için kaynak dönüşümlerinizin veri önizlemesine tıklayın.
3. Her şey veri önizlemesinden iyi görünüyorsa, işlem hattı tasarımcısına gidin ve veri akışınızı bir işlem hattı etkinliğine yerleştirin. Uçtan uca bir test için işlem hattının hatalarını ayıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:
*  [Data Factory blogu](https://techcommunity.microsoft.com/t5/azure-data-factory/bg-p/AzureDataFactoryBlog)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://www.youtube.com/channel/UC2S0k7NeLcEm5_IhHUwpN0g/videos)
*  [Soru sayfası Microsoft Q&](/answers/topics/azure-data-factory.html)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
*  [ADF eşleme veri akışları performans Kılavuzu](concepts-data-flow-performance.md)
