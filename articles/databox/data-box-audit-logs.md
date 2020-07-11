---
title: Azure Data Box, Azure Data Box Heavy olayları için denetim günlükleri | Microsoft Docs
description: Azure Data Box ve Azure Data Box Heavy siparişinizin çeşitli aşamalarında toplanan Data Box için tam denetim günlüklerini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 07/10/2020
ms.author: alkohli
ms.openlocfilehash: 50dbe9ab649a708fb36b1c9e4fe89bccadc7ea90
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209980"
---
# <a name="audit-logs-for-your-azure-data-box-and-azure-data-box-heavy"></a>Azure Data Box ve Azure Data Box Heavy için denetim günlükleri

Günlükler, zaman içinde gerçekleşen ayrı olayların sabit ve zaman damgalarına ait kayıtlardır. Günlükler, cihazınızdan tanılama, denetim ve güvenlik bilgilerini içerir.  

Bir Data Box veya Data Box Heavy sırası, işlemi sırasında aşağıdaki adımlardan geçer: sipariş, ayarlama, veri kopyalama, döndürme, Azure 'a yükleme ve doğrulama ve veri Erimi. Bu adımların her biri için tüm olaylar denetlenir ve günlüğe kaydedilir.

Bu makale, günlüklerin türleri ve toplanan bilgilerin yanı sıra günlüklerin konumunu içeren Data Box denetim günlükleri hakkında bilgiler içerir. 

Bu makaledeki bilgiler, Data Box ve Data Box Heavy için geçerlidir. Sonraki bölümlerde, Data Box yapılan tüm başvurular Data Box Heavy için de geçerlidir. Azure 'da çalışan Data Box hizmetinden toplanan Günlükler Bu makalede ele alınmamıştır. 


## <a name="about-audit-logs"></a>Denetim günlükleri hakkında 

Data Box, aşağıdaki Günlükler toplanır:

- **Sistem günlükleri** -Windows tabanlı bir cihaz Data Box, tüm donanım, yazılım ve sistem olayları günlüğe kaydedilir. Bu olayların bir kümesi, sistem denetim günlüklerinde toplanır ve raporlanır. 

- **Güvenlik** -Windows tabanlı bir cihaz olmak Data Box, tüm güvenlik olayları günlüğe kaydedilir. Bu olayların bir kümesi, güvenlik denetim günlüklerinde toplanır ve raporlanır. 

- **Uygulama** -bu günlükler yalnızca Data Box özgüdür. Bu Günlükler, çalıştıran Data Box hizmetlere yanıt olarak cihazda oluşturulan tüm olayları içerir.

Bu günlüklerin her biri aşağıdaki bölümde ele alınmıştır.

### <a name="system-logs"></a>Sistem günlükleri

Aşağıdaki sistem günlüğü olay kimlikleri Data Box sistem denetim günlükleri olarak toplanır:

|Olay sağlayıcısı adı     |Toplanan olay KIMLIĞI   |Olay açıklaması   |
|-------------------|----------|----------------|
|Microsoft-Windows-çekirdek-genel|12  |İşletim sisteminin yeniden başlatıldığı UTC saati.   |
|                                |13  |İşletim sisteminin kapatıldığı UTC saati. |
|    |                              |
|Microsoft-Windows-Kernel-Power  |41  |Sistem temiz bir kapatmadan yeniden başlatıldı.| 
|    |                              |
|Microsoft-Windows-BitLocker-Sürücü|Tümü|    |

### <a name="security-logs"></a>Güvenlik günlükleri

Aşağıdaki güvenlik günlüğü olay kimlikleri Data Box güvenlik denetim günlükleri olarak toplanır:

|Olay sağlayıcısı adı                   |Toplanan olay KIMLIĞI    |Olay açıklaması       |
|--------------------------------------|------------|----------|
|Microsoft-Windows-Güvenlik-Denetim   |4624        |Oturum başarıyla açıldı. |
|                                      |4625        |Hesap oturum açma başarısız oldu. Bilinmeyen Kullanıcı adı veya hatalı parola. |
|                                     

### <a name="application-logs"></a>Uygulama günlükleri

Aşağıdaki uygulama günlüğü olay kimlikleri, Data Box paket denetim günlüklerinin bir parçası olarak toplanır.     

- **Microsoft-Azure-DataBox-OOBE-Auditing** -yerel kullanıcı arabiriminde oluşan olayları içerir. 
- **Microsoft-Azure-DataBox-reprovision-Audit** -Data Box cihazının yeniden sağlanmasından ilgili olayları içerir. Data Box yeniden sağlanması cihaz, yerel kullanıcı arabirimi aracılığıyla sıfırlandığında oluşur. Bu seçeneği, var olan paylaşımları kaldırarak ve yeniden sağlama ya da cihaz sıfırlamasının parçası olarak paylaşımları yeniden oluşturarak kopyaladığınız verileri silmek istediğinizde seçersiniz.
- **Microsoft-Azure-DataBox-HcsMgmt-Audit** -yalnızca cihaz Azure veri merkezine gönderilmeden önce **sevk hazırlığı** adımla ilgili olayları içerir. 
- **Microsoft-Azure-DataBox-IfxAudit** -iş hakkında ürünün farklı varlıkları tarafından günlüğe kaydedilen iletileri içerir, bazı akışlarda neler olduğu hakkında daha fazla bilgi gösteren Günlükler.

İşte, çeşitli olay sağlayıcılarını ve her durumda toplanan ilgili olay kimliklerini özetleyen bir tablo.

|Olay sağlayıcısı adı    |Olay Kimliği    | Notlar |
|-----------------|-----------------|-------------------|
|Microsoft-Azure-DataBox-OOBE-Auditing |4624        |Oturum başarıyla açıldı.|
|                                      |4625        |Hesap oturum açma başarısız oldu. Bilinmeyen Kullanıcı adı veya hatalı parola.|
|                                     |4634        |Günlüğe kaydet olayı.|
|                                   |  | |
|Microsoft-Azure-DataBox-reprovision-denetim    |65001       |Başarılı yeniden sağlama olayı.|
|                                                  |65002       |Olay yeniden sağlanamadı.|
|                                                  |                 |         |
|Microsoft-Azure-DataBox-HcsMgmt-denetim        |65003       |Göndermeye Hazırlama durum olayı NotStarted, sürüyor, başarısız, Iptal edildi, başarılı, Scancompletedwithsorunlar, SucceededWithWarnings          |
|                                                  |                 |     |
|Microsoft-Azure-DataBox-IfxAudit    |Tümü |Tüm olaylar, kodda denetim günlüğü API 'SI ile günlüğe kaydedilir |

Aşağıda, Izleme çerçevesi (IFX) denetim günlüğüne bir örnek verilmiştir:

|     Görev/Iş/API                              |     Günlüğe kaydedilen olaylar                                                                                                              | 
|-----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|     Temizleme                                   |     Temizleme işinin başlatılması, tamamlanması veya başarısızlığı ile ilgili olaylar günlüğe kaydedilir. |                                              
|     Cihazı müşteri sevkiyatı için hazırla    |     Cihazın sevkiyat için hazırlanması için başlatma, tamamlama veya iş arızasından ilgili olaylar günlüğe kaydedilir. |
|     Sağlama                                 |     Bir cihaz sağlama işinin başlaması, tamamlanması veya başarısızlığı ile ilgili olaylar günlüğe kaydedilir.|
|     Paket işini denetle                       |     Gözetim günlüklerinin zincirini oluşturan bir denetim paketi işinin başlaması, tamamlanması veya başarısızlığı ile ilgili olaylar günlüğe kaydedilir.|
|     Disk üzerine yazma                          |     Diskin üzerine yazma hatası günlüğe yazıldı.|
|     Uzak PowerShell 'i etkinleştirme veya devre dışı bırakma     |     Cihazdaki uzak PowerShell 'i etkinleştirme veya devre dışı bırakma ile ilgili olaylar günlüğe kaydedilir. |
|     Yüklemeyi aşama ayrıntılarını al               |     Bilgisayarlarda yazılımın aşamalarda yüklenmesiyle ilgili olaylar, Azure veri merkezinde günlüğe kaydedilir.|
|     BitLocker birimini kilit açma veya kilitleme           |     Olaylar, *basevolume* ve *hcsdata* biriminin BitLocker durumunu belirtmek için günlüğe kaydedilir.|
|     Diski Temizleme                              |     Silinemeyen fiziksel disklerin başarısızlığı ile ilgili olaylar ve cihazdaki tüm fiziksel diskler başarıyla silindiğinde olaylar günlüğe kaydedilir. |
|     Yerel kullanıcıyı etkinleştirme veya devre dışı bırakma               |     StorSimpleAdmin ve pod Supportadminuser için yerel kullanıcı hesaplarını etkinleştirme veya devre dışı bırakma ile ilgili olaylar günlüğe kaydedilir.| 
|     Parola sıfırlama                          |     Yerel StorSimpleAdmin kullanıcısı için başarılı veya başarısız parola sıfırlamasına ilişkin olaylar günlüğe kaydedilir. |


İFX denetim günlüklerinden ayrı olarak, Data Box için gözetim denetim günlüklerinin Zinciri de toplanır. Bu Günlükler gerçek zamanlı olarak görüntülenemez, ancak yalnızca iş tamamlandıktan sonra ve veriler Data Box disklerinden silinir. Bu Günlükler, IFX denetim günlüklerinde bulunan bilgilerin bir alt kümesini içerir.

Gözetim denetim günlüklerinin zinciri hakkında daha fazla bilgi için bkz. [Data silinme sonrasında gözetim günlüklerinin zincirini al](data-box-logs.md#get-chain-of-custody-logs-after-data-erasure).

<!-- write a few lines about order history and link out to the detailed section on order history-->

## <a name="access-audit-logs"></a>Denetim günlüklerine erişme

Bu Günlükler Azure 'da depolanır ve doğrudan erişilemez. Bu günlüklere erişmeniz gerekiyorsa bir destek bileti dosyası yapın. Daha fazla bilgi için bkz. [iletişim Microsoft desteği](data-box-disk-contact-microsoft-support.md). 

Destek bileti dosyalandığında Microsoft bu günlüklere erişmenizi sağlar.


## <a name="next-steps"></a>Sonraki adımlar

- [Data Box ve Data Box Heavy ilgili sorunları nasıl giderebileceğinizi](data-box-troubleshoot.md)öğrenin.
