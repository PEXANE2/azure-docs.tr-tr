---
title: Özel üstbilgiler kullanarak denetim günlüklerine veri ekleme-FHıR için Azure API
description: Bu makalede, FHıR için Azure API 'sindeki özel HTTP üst bilgileri kullanılarak denetim günlüklerine nasıl veri ekleneceği açıklanmaktadır.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: dseven
ms.author: matjazl
author: matjazl
ms.date: 10/13/2019
ms.openlocfilehash: 068af40ed42d0211eed6e1a315016bb8ecc40d05
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85954222"
---
# <a name="add-data-to-audit-logs-by-using-custom-http-headers"></a>Özel HTTP üstbilgilerini kullanarak denetim günlüklerine veri ekleme

Azure hızlı sağlık birlikte çalışabilirlik kaynakları (FHıR) API 'sinde, bir Kullanıcı, arama sisteminden gelen günlüklere daha fazla bilgi eklemek isteyebilir.

Örneğin, API kullanıcısının kimliği bir dış sistem tarafından doğrulandığında, bu sistem, çağrıyı FHıR API 'sine iletir. FHıR API katmanında, çağrı iletildiğinden özgün kullanıcı hakkındaki bilgiler kayboldu. Denetim veya yönetim amaçlarıyla bu kullanıcı bilgilerinin günlüğe saklanması ve korunması gerekebilir. Çağıran sistem, Kullanıcı kimliği, çağıran konum veya HTTP üst bilgilerinde, arama iletileceği için gerçekleştirilecek diğer gerekli bilgileri sağlayabilir.

Bu veri akışını aşağıdaki diyagramda görebilirsiniz:

:::image type="content" source="media/custom-headers/custom-headers-diagram.png" alt-text="Özel üstbilgiler diyagramı":::

Çeşitli bilgi türlerini yakalamak için özel üst bilgileri kullanabilirsiniz. Örnek:

* Kimlik veya yetkilendirme bilgileri
* Çağıranın kaynağı
* Kaynak kuruluş
* İstemci sistem ayrıntıları (elektronik sistem durumu kaydı, hasta portalı)

> [!IMPORTANT]
> Özel üst bilgilerde gönderilen bilgilerin, Azure günlük Izleme 'de mevcut olduktan sonra 30 gün boyunca Microsoft iç günlük sisteminde depolandığını unutmayın. Özel üstbilgilere eklemeden önce tüm bilgileri şifrelemeyi öneririz. Müşteri üstbilgileri aracılığıyla herhangi bir FI bilgisi iletmemelisiniz.

HTTP başlıklarınız için aşağıdaki adlandırma kuralını kullanmanız gerekir: X-MS-AZUREFHıR-AUDIT-AUDIT- \<name> .

Bu HTTP üstbilgileri, günlüğe eklenen bir özellik çantasına dahildir. Örnek:

* X-MS-AZUREFHıR-AUDIT-USERıD: 1234 
* X-MS-AZUREFHıR-AUDIT-USERLOCATION: XXXX
* X-MS-AZUREFHıR-AUDIT-XYZ: 1234

Bu bilgiler daha sonra günlükteki Özellikler sütununa eklendiğinde JSON olarak serileştirilir. Örnek:

```json
{ "X-MS-AZUREFHIR-AUDIT-USERID" : "1234",
"X-MS-AZUREFHIR-AUDIT-USERLOCATION" : "XXXX",
"X-MS-AZUREFHIR-AUDIT-XYZ" : "1234" }
```
 
Herhangi bir HTTP üst bilgisinde olduğu gibi, aynı üst bilgi adı farklı değerlerle tekrarlanabilir. Örnek:

* X-MS-AZUREFHıR-AUDIT-USERLOCATION: Hospyürüla
* X-MS-AZUREFHıR-AUDIT-USERLOCATION: acil durum

Günlüğe eklendiğinde, değerler virgülle ayrılmış bir liste ile birleştirilir. Örnek:

{"X-MS-AZUREFHıR-AUDIT-USERLOCATION": "Hospyürüla, acil durum"}
 
En fazla 10 benzersiz üst bilgi ekleyebilirsiniz (farklı değerlere sahip aynı üstbilginin tekrarları yalnızca bir tane olarak sayılır). Bir üst bilgi için maksimum değer olan değerin toplam uzunluğu 2048 karakterdir.

Firefly C# istemci API kitaplığı kullanıyorsanız, kod şuna benzer:

```C#
FhirClient client;
client = new FhirClient(serverUrl);
client.OnBeforeRequest += (object sender, BeforeRequestEventArgs e) =>
{
    // Add custom headers to be added to the logs
    e.RawRequest.Headers.Add("X-MS-AZUREFHIR-AUDIT-UserLocation", "HospitalA");
};
client.Get("Patient");
```
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, FHıR için Azure API 'sindeki özel üst bilgileri kullanarak denetim günlüklerine veri eklemeyi öğrendiniz. Daha sonra, FHıR için Azure API 'sinde yapılandırabileceğiniz diğer ek ayarlar hakkında bilgi edinin.
 
>[!div class="nextstepaction"]
>[Ek ayarlar](azure-api-for-fhir-additional-settings.md)
