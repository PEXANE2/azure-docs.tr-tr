---
title: Azure hizmet durumu bildirimleri nelerdir?
description: Hizmet durumu bildirimleri, Microsoft Azure tarafından yayınlanan hizmet durumu iletilerini görüntülemenize olanak tanır.
ms.topic: conceptual
ms.date: 4/12/2018
ms.openlocfilehash: e40538ac98bbc7b79311d4fb0da7568d56a84e18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77653977"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme

Hizmet durumu bildirimleri Azure tarafından yayımlanır ve aboneliğiniz altındaki kaynaklar hakkında bilgiler içerir. Bu bildirimler, etkinlik günlüğü olaylarının bir alt sınıfıdır ve etkinlik günlüğünde de bulunabilir. Hizmet durumu bildirimleri, sınıfa bağlı olarak bilgilendirilebilir veya işlem yapılabilir.

Hizmet sağlık bildirimleri çeşitli sınıflar vardır:  

- **Gerekli eylem:** Azure hesabınızda olağandışı bir şey olduğunu fark edebilir ve bunu düzeltmek için sizinle birlikte çalışabilir. Azure, yapmanız gereken eylemleri veya Azure mühendisliğine nasıl başvurabileceğinizi veya nasıl destekleyeceğinizi ayrıntılı olarak anlatan bir bildirim gönderir.  
- **Olay:** Hizmeti etkileyen bir olay şu anda aboneliğinizdeki kaynaklardan birini veya birkaçını etkiliyor.  
- **Bakım:** Aboneliğiniz altındaki kaynaklardan birini veya daha fazlasını etkileyebilecek planlı bir bakım etkinliği.  
- **Bilgi:** Kaynak kullanımınızı iyileştirmenize yardımcı olabilecek olası optimizasyonlar. 
- **Güvenlik:** Azure'da çalışan çözümleriniz ile ilgili acil güvenlikle ilgili bilgiler.

Her hizmet durumu bildirimi, kaynaklarınızın kapsamı ve etkisi yle ilgili ayrıntıları içerir. Ayrıntılar şunlardır:

Özellik adı | Açıklama
-------- | -----------
Kanal | Aşağıdaki değerlerden biri: Yönetici veya **İşlem** **.**
correlationId | Genellikle dize biçiminde bir GUID. Aynı eyleme ait olaylar genellikle aynı korelasyonid paylaşır.
olayDataId | Bir olayın benzersiz tanımlayıcısı.
eventName | Bir olayın başlığı.
düzey | Bir olayın düzeyi
kaynakProviderName | Etkilenen kaynak için kaynak sağlayıcısının adı.
resourceType| Etkilenen kaynağın kaynak türü.
altDurum | Genellikle ilgili REST çağrısının HTTP durum kodu, ancak bir alt durumu açıklayan diğer dizeleri de içerebilir. Örneğin: Tamam (HTTP Durum Kodu: 200), Oluşturuldu (HTTP Durum Kodu: 201), Kabul Edildi (HTTP Durum Kodu: 202), İçerik Yok (HTTP Durum Kodu: 204), Kötü İstek (HTTP Durum Kodu: 400), Bulunamadı (HTTP Durum Kodu: 404), Çakışma (HTTP Durum Kodu: 409), Dahili Sunucu Hata (HTTP Durum Kodu: 500), Hizmet Kullanılamıyor (HTTP Durum Kodu: 503) ve Ağ Geçidi Zaman Ası (HTTP Durum Kodu: 504).
olayTimestamp | Etkinlik Azure hizmeti tarafından oluşturulduğunda, olaya karşılık gelen isteği işleyen zaman damgası.
teslimTimestamp | Olay sorgu için kullanılabilir hale geldiğinde zaman damgası.
subscriptionId | Bu olayın günlüğe kaydedildiği Azure aboneliği.
durum | İşlemin durumunu açıklayan dize. Bazı ortak değerler şunlardır: **Başlatılan**, **Devam Eden**, **Başarılı**, **Başarısız**, **Etkin**, ve **Çözüldü**.
operationName | Operasyonun adı.
category | Bu özellik her zaman **ServiceHealth**olduğunu.
resourceId | Etkilenen kaynağın Kaynak Kimliği.
Özellikler.başlık | Bu iletişim için yerelleştirilmiş başlık. İngilizce varsayılandır.
Özellikler.iletişim | HTML biçimlendirmesi ile iletişimin yerelleştirilmiş ayrıntıları. İngilizce varsayılandır.
Properties.incidentType | Aşağıdaki değerlerden biri: **ActionRequired**, **Informational**, **Incident**, **Maintenance**veya **Security**.
Özellikler.trackingId | Bu olayın ilişkili olduğu olay. Bir olayla ilgili olayları ilişkilendirmek için bunu kullanın.
Özellikler.etkilenenHizmetler | Olaydan etkilenen hizmetleri ve bölgeleri tanımlayan kaçak bir JSON blob. Özellik, her biri **ServiceName'e**sahip bir hizmet listesi ve her biri **regionname'si**olan etkilenen bölgelerin listesini içerir.
Özellikler.defaultLanguageTitle | İngilizce iletişim.
Özellikler.defaultLanguageContent | Html biçimlendirmesi veya düz metin olarak İngilizce iletişim.
Özellikler.sahne | **Olay**ve **Güvenlik** için olası değerler **Etkin,** **Çözümlü** veya **RCA'dır.** **ActionGerekli** veya **Informational** için tek değer **Etkin'dir.** **Bakım** için şunlardır: **Etkin**, **Planlı**, **Devam ,** **İptal ,** Yeniden **Zamanlanmış**, **Çözüldü**, veya **Tamamlandı**.
Özellikler.communicationId | Bu olayın ilişkili olduğu iletişim.

### <a name="details-on-service-health-level-information"></a>Hizmet durumu düzeyi bilgileri yle ilgili ayrıntılar

**Gerekli Eylem** (properties.incidentType == ActionGerekli)
- Bilgilendirme - Yönetici eylemi, varolan hizmetlerin etkisini önlemek için gereklidir.
    
**Bakım** (properties.incidentType == Bakım)
- Uyarı - Acil bakım
- Informational - Standart planlı bakım

**Bilgi** (properties.incidentType == Bilgi)
- Bilgilendirme - Yönetici, varolan hizmetlerin etkisini önlemek için gerekli olabilir.

**Güvenlik** (properties.incidentType == Security)
- Uyarı - Varolan hizmetleri etkileyen ve yönetici eylemi gerektirebilecek güvenlik danışma belgesi.
- Bilgilendirme - Varolan hizmetleri etkileyen güvenlik danışmanlığı.

**Hizmet Sorunları** (properties.incidentType == Olay)
- Hata - Birden çok bölgede birden çok hizmete erişen yaygın sorunlar, geniş bir müşteri kümesini etkiliyor.
- Uyarı - Belirli hizmetlere ve/veya belirli bölgelere erişen sorunlar, bir müşteri alt kümesini etkiler.
- Bilgilendirme - Hizmet kullanılabilirliğini etkilemeden yönetim işlemlerini ve/veya gecikme durumunu etkileyen sorunlar.
