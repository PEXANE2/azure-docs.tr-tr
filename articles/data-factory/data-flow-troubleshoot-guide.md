---
title: Veri akışlarını eşleme sorunlarını giderme
description: Azure Data Factory veri akışı sorunlarını giderme hakkında bilgi edinin.
ms.author: makromer
author: kromerm
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 09/11/2020
ms.openlocfilehash: 3c73d1652d4fdaa34fa65e7186298bafc0a905a9
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102453266"
---
# <a name="troubleshoot-mapping-data-flows-in-azure-data-factory"></a>Azure Data Factory veri akışlarını eşleme sorunlarını giderme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Azure Data Factory veri akışlarını eşlemek için ortak sorun giderme yöntemleri incelenmektedir.

## <a name="common-error-codes-and-messages"></a>Ortak hata kodları ve iletileri 

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Hata kodu: DF-yürütücü-Sourceınvalidpayload
- **İleti**: kapsayıcı mevcut olmadığından veri önizleme, hata ayıklama ve işlem hattı veri akışı yürütme başarısız oldu
- **Neden**: bir veri kümesi depolamada mevcut olmayan bir kapsayıcı içeriyor.
- **Öneri**: veri kümenizde başvurulan kapsayıcının var olduğundan ve erişilebilir olduğundan emin olun.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Hata kodu: DF-yürütücü-Systemımplicıtkartezyen

- **İleti**: iç birleşim için örtülü Kartezyen ürün desteklenmiyor, bunun yerıne çapraz birleşim kullanın. Birleşimde kullanılan sütunlar, satırlar için benzersiz bir anahtar oluşturmaktır.
- **Neden**: mantıksal planlar arasında iç birleştirmeler için örtülü Kartezyen ürünler desteklenmez. Birleşimde sütunları kullanıyorsanız, ilişkinin her iki tarafında da en az bir sütun içeren benzersiz bir anahtar oluşturun.
- **Öneri**: eşitlik tabanlı olmayan BIRLEŞIMLER IÇIN özel çapraz birleştirme kullanın.

### <a name="error-code-df-executor-systeminvalidjson"></a>Hata kodu: DF-yürütücü-SystemInvalidJson

- **İleti**: JSON ayrıştırma hatası, desteklenmeyen kodlama veya çok satırlı
- **Neden**: JSON dosyası ile ilgili olası sorunlar: desteklenmeyen kodlama, bozuk bayt veya JSON kaynağı birçok iç içe satırda tek bir belge olarak kullanılıyor.
- **Öneri**: JSON dosyasının kodlamasının desteklendiğini doğrulayın. JSON veri kümesi kullanan kaynak dönüşümünde **JSON ayarları** ' nı genişletin ve **tek belgeyi** açın.
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Hata kodu: DF-yürütücü-Yayınzaman aşımı

- **İleti**: yayın birleşimi zaman aşımı hatası, yayın akışının hata ayıklama çalıştırmaları ve 300 saniye içinde iş çalıştırmaları sırasında 60 saniye içinde veri ürettiğinden emin olun
- **Neden**: yayın, hata ayıklama çalıştırmaları ve iş çalıştırmaları için 300 saniye varsayılan zaman aşımı süresi 60 saniyedir. Yayın için seçilen akış bu sınırın içinde veri üretmede çok büyük.
- **Öneri**: JOIN, Exists ve Lookup için veri akışı dönüştürmelerinizin **en iyileştirme** sekmesini kontrol edin. Yayın için varsayılan seçenek **Auto**' dır. **Otomatik** olarak ayarlandıysa ya da sol veya sağ tarafını **sabit** altında yayınla el ile ayarlıyorsanız, daha büyük bir Azure TÜMLEŞTIRME çalışma zamanı (IR) yapılandırması ayarlayabilir veya yayını kapatabilirsiniz. Veri akışlarında en iyi performansı elde etmek için, **Otomatik** kullanarak Spark 'ın yayınlanmasını ve bellek için iyileştirilmiş bir Azure IR kullanmanızı öneririz. 
 
  Hata ayıklama işlem hattı çalıştırmasıyla veri akışını bir hata ayıklama testi yürütmesinde çalıştırıyorsanız, Bu koşulla daha sık çalıştırabilirsiniz. Bunun nedeni, Azure Data Factory daha hızlı bir hata ayıklama deneyimi sağlamak için yayının zaman aşımını 60 saniyeye kısıtlar. Zaman aşımını tetiklenen bir çalıştırmanın 300 saniyelik zaman aşımı süresine genişletebilirsiniz. Bunu yapmak için,   >  veri akışı işlem hattı yürütme etkinliğinizdeki tanımlı Azure IR kullanmak için **etkinlik çalışma zamanını kullan** seçeneğini kullanabilirsiniz.

- **İleti**: yayın birleştirmesi zaman aşımı hatası, bu sorundan kaçınmak için JOIN/Exists/Lookup dönüşümünde yayın ' ı ' seçeneğini belirleyebilirsiniz. Performansı artırmak için JOIN seçeneğini yayınlamak istiyorsanız, yayın akışının hata ayıklama çalıştırmaları ve 300 saniye içinde, iş çalıştırmaları sırasında 60 saniye içinde veri oluşturabilmesine dikkat edin.
- **Neden**: yayın, hata ayıklama çalıştırmaları ve iş çalıştırmaları 300 saniye için varsayılan zaman aşımı süresi olan 60 saniyedir. Yayın birleşimi üzerinde, yayın için seçilen akış bu sınırın içinde veri üretmede çok büyük. Bir yayın birleşimi kullanılmazsa, veri akışı tarafından varsayılan yayın aynı sınıra ulaşabilirler.
- **Öneri**: yayın seçeneğini devre dışı bırakın veya işlemenin 60 saniyeden fazla sürebileceği büyük veri akışlarını yayınlamaktan kaçının. Yayınlanacak daha küçük bir akış seçin. Büyük Azure SQL veri ambarı tabloları ve kaynak dosyaları genellikle iyi bir seçimdir. Bir yayın katılımı yokluğunda, bu hata oluşursa daha büyük bir küme kullanın.

### <a name="error-code-df-executor-conversion"></a>Hata kodu: DF-yürütücü-dönüştürme

- **İleti**: geçersiz bir karakter nedeniyle tarih veya saate dönüştürme başarısız oldu
- **Neden**: veriler beklenen biçimde değil.
- **Öneri**: doğru veri türünü kullanın.

### <a name="error-code-df-executor-invalidcolumn"></a>Hata kodu: DF-yürütücü-ınvalidcolumn
- **İleti**: sorguda sütun adının belirtilmesi gerekir, bir SQL işlevi kullanılıyorsa bir diğer ad ayarlayın
- **Neden**: sütun adı belirtilmedi.
- **Öneri**: min () veya Max () gıbı bir SQL işlevi kullanıyorsanız, bir diğer ad belirleyin.

### <a name="error-code-df-executor-drivererror"></a>Hata kodu: DF-yürütücü-DriverError
- **İleti**: INT96, ADF veri akışı tarafından desteklenmeyen eski zaman damgası türüdür. Lütfen sütun türünü en son türlere yükseltmeyi göz önünde bulundurun.
- **Neden**: sürücü hatası.
- **Öneri**: INT96, Azure Data Factory veri akışı tarafından desteklenmeyen eski bir zaman damgası türüdür. Sütun türünü en son türe yükseltmeyi düşünün.

### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Hata kodu: DF-yürütücü-Blockcountexceedslimıterror
- **İleti**: kaydedilmemiş blok sayısı 100.000 blok üst sınırını aşamaz. Blob yapılandırmasını denetleyin.
- **Neden**: bir Blobun en fazla işlenmemiş blok sayısı 100.000 ' dir.
- **Öneri**: Bu sorunla ilgili daha fazla bilgi için Microsoft ürün ekibine başvurun.

### <a name="error-code-df-executor-partitiondirectoryerror"></a>Hata kodu: DF-yürütücü-PartitionDirectoryError
- **İleti**: belirtilen kaynak yolu birden çok bölümlenmiş dizine sahip (örn. <Source Path> /<bölüm kök dizini 1>/a = 10/b = 20, <Source Path> /<bölüm kök dizin 2>/c = 10/d = 30) veya diğer dosya ya da bölümlenmemiş dizin ile bölümlenmiş dizin (örneğin <Source Path> /<bölüm kök dizini 1>/a = 10/b = 20, <Source Path> /Dizin 2/FILE1), bölüm kök dizinini kaynak yolundan kaldırın ve ayrı kaynak dönüşümünde okuyun.
- **Neden**: kaynak yolunda birden çok bölümlenmiş dizin ya da başka bir dosya ya da bölümlenmemiş dizin içeren bölümlenmiş bir dizin bulunur.
- **Öneri**: bölümlenmiş kök dizini kaynak yoldan kaldırın ve ayrı kaynak dönüşümünde okuyun.

### <a name="error-code-df-executor-invalidtype"></a>Hata kodu: DF-yürütücü-ınvalidtype
- **İleti**: lütfen parametre türünün geçirilen değer türüyle eşleştiğinden emin olun. İşlem hatlarından kayan parametrelerin geçirilmesi Şu anda desteklenmiyor.
- **Neden**: belirtilen tür için veri türü gerçek parametre değeri ile uyumlu değil.
- **Öneri**: veri akışına geçirilen parametre değerlerinin, belirtilen türle eşleştiğinden emin olun.

### <a name="error-code-df-executor-columnunavailable"></a>Hata kodu: DF-yürütücü-ColumnUnavailable
- **İleti**: ifadede kullanılan sütun adı kullanılamıyor veya geçersiz
- **Neden**: bir ifadede geçersiz veya kullanılamayan bir sütun adı kullanıldı.
- **Öneri**: ifadelerde sütun adlarını denetleyin.

### <a name="error-code-df-executor-parseerror"></a>Hata kodu: DF-yürütücü-ParseError
- **İleti**: ifade ayrıştırılamıyor
- **Neden**: yanlış biçimlendirme nedeniyle bir ifade Ayrıştırma hataları oluşturdu.
- **Öneri**: ifadedeki biçimlendirmeyi denetleyin.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Hata kodu: DF-yürütücü-Systemımplicıtkartezyen
- **İleti**: iç birleşim için örtülü Kartezyen ürün desteklenmiyor, bunun yerıne çapraz birleşim kullanın. Birleşimde kullanılan sütunlar, satırlar için benzersiz bir anahtar oluşturmaktır.
- **Neden**: mantıksal planlar arasında iç birleştirmeler için örtülü Kartezyen ürünler desteklenmez. Birleşimde sütunları kullanıyorsanız, benzersiz bir anahtar oluşturun.
- **Öneri**: eşitlik tabanlı olmayan BIRLEŞTIRMELER IÇIN çapraz birleştirme kullanın.

### <a name="error-code-df-executor-systeminvalidjson"></a>Hata kodu: DF-yürütücü-SystemInvalidJson
- **İleti**: JSON ayrıştırma hatası, desteklenmeyen kodlama veya çok satırlı
- **Neden**: JSON dosyası ile ilgili olası sorunlar: desteklenmeyen kodlama, bozuk bayt veya JSON kaynağı birçok iç içe satırda tek bir belge olarak kullanılıyor.
- **Öneri**: JSON dosyasının kodlamasının desteklendiğini doğrulayın. JSON veri kümesi kullanan kaynak dönüşümünde **JSON ayarları** ' nı genişletin ve **tek belgeyi** açın.



### <a name="error-code-df-executor-conversion"></a>Hata kodu: DF-yürütücü-dönüştürme
- **İleti**: geçersiz bir karakter nedeniyle tarih veya saate dönüştürme başarısız oldu
- **Neden**: veriler beklenen biçimde değil.
- **Öneri**: doğru veri türünü kullanın.


### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Hata kodu: DF-yürütücü-Blockcountexceedslimıterror
- **İleti**: kaydedilmemiş blok sayısı 100.000 blok üst sınırını aşamaz. Blob yapılandırmasını denetleyin.
- **Neden**: bir Blobun en fazla işlenmemiş blok sayısı 100.000 ' dir.
- **Öneri**: Bu sorunla ilgili daha fazla bilgi için Microsoft ürün ekibine başvurun.

### <a name="error-code-df-executor-partitiondirectoryerror"></a>Hata kodu: DF-yürütücü-PartitionDirectoryError
- **İleti**: belirtilen kaynak yolu birden çok bölümlenmiş dizine sahip (örn. *<Source Path> /<bölüm kök dizini 1>/a = 10/b = 20, <Source Path> /<bölüm kök dizin 2>/c = 10/d = 30*) veya diğer dosya ya da bölümlenmemiş dizin ile bölümlenmiş dizin (örn. *<Source Path> /<bölüm kök dizini 1>/A = 10/b = 20, <Source Path> /Dizin 2/FILE1*), bölüm kök dizinini kaynak yolundan kaldırın ve ayrı kaynak dönüşümünde okuyun.
- **Neden**: kaynak yolunda birden çok bölümlenmiş dizin ya da başka bir dosya ya da bölümlenmemiş dizin içeren bölümlenmiş bir dizin bulunur. 
- **Öneri**: bölümlenmiş kök dizini kaynak yoldan kaldırın ve ayrı kaynak dönüşümünde okuyun.

### <a name="error-code-getcommand-outputasync-failed"></a>Hata kodu: GetCommand OutputAsync başarısız oldu
- **İleti**: veri akışı hata ayıklama ve veri önizleme sırasında: GetCommand OutputAsync ile başarısız oldu...
- **Neden**: Bu hata bir arka uç hizmet hatasıdır. 
- **Öneri**: işlemi yeniden deneyin ve hata ayıklama oturumunuzu yeniden başlatın. Yeniden deneme ve yeniden başlatma sorunu çözmezse, müşteri desteğine başvurun. 

### <a name="error-code-df-executor-outofmemoryerror"></a>Hata kodu: DF-yürütücü-OutOfMemoryError
 
- **İleti**: küme, yürütme sırasında yetersiz bellek sorunuyla karşılaştı, lütfen daha büyük çekirdek sayısı ve/veya bellek için iyileştirilmiş işlem türü ile bir tümleştirme çalışma zamanı kullanmayı deneyin
- **Neden**: kümede bellek kalmadı.
- **Öneri**: hata ayıklama kümelerinin geliştirilmesi için tasarlanmıştır. Yük çalıştırmak için veri örneklemesi ve uygun bir işlem türü ve boyutu kullanın. Performans ipuçları için bkz. [veri akışı performans kılavuzunu eşleme](concepts-data-flow-performance.md).

### <a name="error-code-df-executor-illegalargument"></a>Hata kodu: DF-yürütücü-düzeneği Galargument

- **İleti**: lütfen bağlı hizmetinizdeki erişim anahtarının doğru olduğundan emin olun
- **Neden**: hesap adı veya erişim anahtarı yanlış.
- **Öneri**: bağlı hizmetinizde belirtilen hesap adının veya erişim anahtarının doğru olduğundan emin olun. 

### <a name="error-code-df-executor-invalidtype"></a>Hata kodu: DF-yürütücü-ınvalidtype
- **İleti**: lütfen parametre türünün geçirilen değer türüyle eşleştiğinden emin olun. İşlem hatlarından kayan parametrelerin geçirilmesi Şu anda desteklenmiyor.
- **Neden**: belirtilen tür için veri türü gerçek parametre değeri ile uyumlu değil. 
- **Öneri**: doğru veri türlerini sağlayın.

### <a name="error-code-df-executor-columnunavailable"></a>Hata kodu: DF-yürütücü-ColumnUnavailable
- **İleti**: ifadede kullanılan sütun adı kullanılamıyor veya geçersiz.
- **Neden**: bir ifadede geçersiz veya kullanılamayan bir sütun adı kullanılıyor.
- **Öneri**: ifadelerde kullanılan sütun adlarını denetleyin.


### <a name="error-code-df-executor-parseerror"></a>Hata kodu: DF-yürütücü-ParseError
- **İleti**: ifade ayrıştırılamıyor.
- **Neden**: yanlış biçimlendirme nedeniyle bir ifade Ayrıştırma hataları oluşturdu. 
- **Öneri**: ifadedeki biçimlendirmeyi denetleyin.


 ### <a name="error-code-df-executor-outofdiskspaceerror"></a>Hata kodu: DF-yürütücü-OutOfDiskSpaceError
- **İleti**: iç sunucu hatası
- **Neden**: kümede disk alanı kalmadı.
- **Öneri**: işlem hattını yeniden deneyin. Bunu yapmak sorunu çözmezse müşteri desteği 'ne başvurun.


 ### <a name="error-code-df-executor-storeisnotdefined"></a>Hata kodu: DF-yürütücü-StoreIsNotDefined
- **İleti**: depo yapılandırması tanımlı değil. Bu hata, ardışık düzende geçersiz parametre atamasından kaynaklanıyor olabilir.
- **Neden**: belirlenmedi.
- **Öneri**: işlem hattının parametre değeri atamasını denetleyin. Bir parametre ifadesi geçersiz karakterler içeriyor olabilir.

### <a name="error-code-df-excel-invalidconfiguration"></a>Hata kodu: DF-Excel-ınvalidconfiguration
- **İleti**: Excel sayfası adı veya dizini gereklidir.
- **Neden**: belirlenmedi.
- **Öneri**: parametre değerini denetleyin. Excel verilerini okumak için çalışma sayfası adını veya dizinini belirtin.

- **İleti**: Excel sayfası adı ve dizini aynı anda mevcut olamaz.
- **Neden**: belirlenmedi.
- **Öneri**: parametre değerini denetleyin. Excel verilerini okumak için çalışma sayfası adını veya dizinini belirtin.

- **İleti**: geçersiz Aralık belirtildi.
- **Neden**: belirlenmedi.
- **Öneri**: parametre değerini denetleyin. Başvuruya göre geçerli bir Aralık belirtin. Daha fazla bilgi için bkz. [Excel özellikleri](./format-excel.md#dataset-properties).

- **İleti**: yalnızca. xlsx ve. xls desteklenirken geçersiz Excel dosyası sağlanıyor
- **Neden**: belirlenmedi.
- **Öneri**: Excel dosya uzantısının. xlsx veya. xls olduğundan emin olun.


 ### <a name="error-code-df-excel-invaliddata"></a>Hata kodu: DF-Excel-ınvaliddata
- **İleti**: Excel çalışma sayfası yok.
- **Neden**: belirlenmedi.
- **Öneri**: parametre değerini denetleyin. Excel verilerini okumak için geçerli bir çalışma sayfası adı veya dizini belirtin.

- **İleti**: Excel dosyalarını farklı şemayla okuma işlemi şu anda desteklenmiyor.
- **Neden**: belirlenmedi.
- **Öneri**: desteklenen bir Excel dosyası kullanın.

- **İleti**: veri türü desteklenmiyor.
- **Neden**: belirlenmedi.
- **Öneri**: desteklenen Excel dosyası veri türlerini kullanın.

### <a name="error-code-4502"></a>Hata kodu: 4502
- **İleti**: Integration Runtime altında azaltma nedeniyle hatalara neden olan çok fazla eşzamanlı MappingDataflow yürütmesi vardır.
- **Neden**: aynı anda çok sayıda veri akışı etkinliği çalıştırması, tümleştirme çalışma zamanı üzerinde eşzamanlı olarak gerçekleştirilir. Daha fazla bilgi için bkz. [Azure Data Factory sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits).
- **Öneri**: daha fazla veri akışı etkinliğini paralel olarak çalıştırmak istiyorsanız, bunları birden çok tümleştirme çalışma zamanı arasında dağıtın.


### <a name="error-code-invalidtemplate"></a>Hata kodu: ınvalidtemplate
- **İleti**: ardışık düzen ifadesi değerlendirilemiyor.
- **Neden**: veri akışı etkinliğinde geçirilen ardışık düzen ifadesi bir sözdizimi hatası nedeniyle doğru işlenmiyor.
- **Öneri**: ifadeyi doğrulamak için etkinlik izlemede etkinliğinizi denetleyin.

### <a name="error-code-2011"></a>Hata kodu: 2011
- **İleti**: etkinlik Azure Integration Runtime çalışıyor ve veri deposunun kimlik bilgilerinin şifresini çözemedi veya şirket içinde barındırılan bir Integration Runtime ile bağlı işlem başarısız oldu. Lütfen bu etkinlikle ilişkili bağlı hizmetlerin yapılandırmasını denetleyin ve uygun tümleştirme çalışma zamanı türünü kullandığınızdan emin olun.
- **Neden**: veri akışı, şirket içinde barındırılan tümleştirme çalışma zamanları üzerinde bağlı Hizmetleri desteklemez.
- **Öneri**: veri akışını yönetilen bir sanal ağ tümleştirme çalışma zamanı üzerinde çalışacak şekilde yapılandırın.

## <a name="miscellaneous-troubleshooting-tips"></a>Çeşitli sorun giderme ipuçları
- **Sorun**: beklenmeyen özel durum oluştu ve yürütme başarısız oldu.
    - **İleti**: veri akışı etkinlik yürütmesi sırasında: beklenmeyen bir özel durum ve yürütme başarısız oldu.
    - **Neden**: Bu hata bir arka uç hizmet hatasıdır. İşlemi yeniden deneyin ve hata ayıklama oturumunuzu yeniden başlatın.
    - **Öneri**: yeniden deneme ve yeniden başlatma sorunu çözmezse, müşteri desteğine başvurun.

-  **Sorun**: hata ayıklama veri önizlemesi sırasında birleşimde çıkış verisi yok.
    - **İleti**: çok az sayıda satır örneklenmemesinin nedeni büyük miktarda null değer veya eksik değer vardır. Hata ayıklama satırı sınırını güncelleştirmeyi ve verileri yenilemeyi deneyin.
    - **Neden**: JOIN koşulu hiçbir satırla eşleşmedi veya veri önizlemesi sırasında çok sayıda null değer ile sonuçlandı.
    - **Öneri**: **hata ayıklama ayarları**' nda, kaynak satır sınırdaki satır sayısını artırın. Daha fazla veri işlemek için yeterince büyük olan bir veri akışı kümesine sahip bir Azure IR seçtiğinizden emin olun.
    
- **Sorun**: çok satırlı CSV dosyaları olan kaynakta doğrulama hatası. 
    - **İleti**: şu hata iletilerinden birini görebilirsiniz:
        - Son sütun null veya eksik.
        - Kaynaktaki şema doğrulaması başarısız oluyor.
        - Şema içeri aktarma, UX 'de doğru şekilde gösterilemedi ve son sütunda adında yeni bir satır karakteri bulunur.
    - **Neden**: eşleme veri akışında, çok satırlı CSV kaynak dosyaları şu anda, \r\n satır sınırlayıcısı olarak kullanıldığında çalışmaz. Bazen satır dönüşlerinde ek satırlar hatalara neden olabilir. 
    - **Öneri**: \n 'yi, \ ' i kullanarak bir satır sınırlayıcısı olarak kullanarak kaynak üzerinde dosya oluşturun. Ya da CSV dosyasını bir satır sınırlayıcısı olarak \n kullanacak şekilde dönüştürmek için kopyalama etkinliğini kullanın.

## <a name="general-troubleshooting-guidance"></a>Genel sorun giderme kılavuzu
1. Veri kümesi bağlantılarınızın durumunu denetleyin. Her kaynak ve havuz dönüşümünde, kullanmakta olduğunuz her veri kümesi için bağlı hizmete gidin ve bağlantıları test edin.
2. Veri akışı tasarımcısında dosya ve tablo bağlantılarınızın durumunu denetleyin. Verilerinize erişebildiğinizden emin olmak için, hata ayıklama modunda kaynak dönüşümlerinizin **veri önizlemesi** ' ni seçin.
3. Her şey veri önizlemede doğru görünüyorsa, işlem hattı tasarımcısına gidin ve veri akışınızı bir işlem hattı etkinliğine yerleştirin. Uçtan uca bir test için işlem hattının hatalarını ayıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme konusunda daha fazla yardım için şu kaynaklara bakın:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
