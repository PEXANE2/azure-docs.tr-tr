---
title: Azure IoT Hub Cihaz Sağlama Hizmeti - Simetrik anahtar attestation
description: Bu makalede, IoT Aygıt Sağlama Hizmeti (DPS) kullanarak simetrik anahtar attestation kavramsal bir bakış sağlar.
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 0e3d343c0a68dd527e4e8e8d23e5b3843a216a78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271518"
---
# <a name="symmetric-key-attestation"></a>Simetrik anahtar kanıtlama

Bu makalede, Aygıt Sağlama Hizmeti ile simetrik anahtarlar kullanırken kimlik attestation işlemi açıklanır. 

Simetrik anahtar attestation bir Aygıt Sağlama Hizmeti örneği ile bir aygıtın kimlik doğrulaması için basit bir yaklaşımdır. Bu attestation yöntemi, aygıt sağlamada yeni olan veya sıkı güvenlik gereksinimleri olmayan geliştiriciler için bir "Merhaba dünya" deneyimini temsil eder. [TPM](concepts-tpm-attestation.md) veya [X.509 sertifikası](concepts-security.md#x509-certificates) kullanan aygıt attestation'ı daha güvenlidir ve daha sıkı güvenlik gereksinimleri için kullanılmalıdır.

Simetrik anahtar kayıtları, sınırlı güvenlik işlevine sahip eski aygıtların Azure IoT aracılığıyla buluta bootstrap için harika bir yol sağlar. Eski aygıtlarla simetrik anahtar attestation hakkında daha fazla bilgi için, [eski cihazlarla simetrik tuşların nasıl kullanılacağına](how-to-legacy-device-symm-key.md)bakın.


## <a name="symmetric-key-creation"></a>Simetrik anahtar oluşturma

Varsayılan olarak, Aygıt Sağlama Hizmeti, Otomatik oluşturma anahtarları seçeneği etkinken yeni kayıtlar kaydedildiğinde varsayılan uzunluğu 32 bayt olan yeni simetrik **anahtarlar** oluşturur.

![Otomatik Simetrik tuşlar üret](./media/concepts-symmetric-key-attestation/auto-generate-keys.png)

Bu seçeneği devre dışı bırakarak kayıtlar için kendi simetrik anahtarlarınızı da sağlayabilirsiniz. Kendi simetrik tuşlarınızı belirtirken, anahtarlarınızın 16 bayt ile 64 bayt arasında bir anahtar uzunluğu olmalıdır. Ayrıca, simetrik tuşlar geçerli Base64 formatında sağlanmalıdır.



## <a name="detailed-attestation-process"></a>Ayrıntılı attestation süreci

Aygıt Sağlama Hizmeti ile simetrik anahtar attestation cihazları tanımlamak için IoT hub'ları tarafından desteklenen aynı [Güvenlik belirteçleri](../iot-hub/iot-hub-devguide-security.md#security-token-structure) kullanılarak gerçekleştirilir. Bu güvenlik belirteçleri [Paylaşılan Erişim İmzası (SAS) belirteçleridir.](../service-bus-messaging/service-bus-sas.md) 

SAS belirteçleri simetrik anahtar kullanılarak oluşturulan bir hashed *imza* var. İmza, attestation sırasında sunulan bir güvenlik belirteci orijinal olup olmadığını doğrulamak için Aygıt Sağlama Hizmeti tarafından yeniden oluşturulur.

SAS belirteçleri aşağıdaki formu vardır:

`SharedAccessSignature sig={signature}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Her belirteç bileşenleri şunlardır:

| Değer | Açıklama |
| --- | --- |
| {imza} |HMAC-SHA256 imza dizesi. Tek tek kayıtlar için, bu imza karma gerçekleştirmek için simetrik anahtar (birincil veya ikincil) kullanılarak üretilir. Kayıt grupları için, karma gerçekleştirmek için kayıt grubu anahtarından türetilen bir anahtar kullanılır. Karma formun bir ileti üzerinde gerçekleştirilir: `URL-encoded-resourceURI + "\n" + expiry`. **Önemli**: HMAC-SHA256 hesaplamasını gerçekleştirmek için kullanılmadan önce anahtar base64'ten deşifre edilmelidir. Ayrıca, imza sonucu URL kodlanmış olmalıdır. |
| {resourceURI} |Bu belirteçle erişilebilen kayıt bitiş noktasının URI'si, Aygıt Sağlama Hizmeti örneği için kapsam kimliğiyle başlar. Örneğin, `{Scope ID}/registrations/{Registration ID}` |
| {son kullanma tarihi} |UTF8 dizeleri 1 Ocak 1970 tarihinde 00:00:00 UTC beri saniye sayısı için. |
| {URL kodlanmış-resourceURI} |Küçük harf li kaynak URI'nin küçük harf URL kodlaması |
| {policyName} |Bu belirteç için başvuran paylaşılan erişim ilkesinin adı. Simetrik anahtar attestation ile karşılık verirken kullanılan ilke adı **kayıttır.** |

Bir aygıt tek bir kayıtla doğrulandığında, aygıt SAS belirteci için hashed imzasını oluşturmak için tek tek kayıt girişinde tanımlanan simetrik anahtarı kullanır.

SAS belirteci oluşturan kod örnekleri için [Bkz.](../iot-hub/iot-hub-devguide-security.md#security-token-structure)

Simetrik anahtar attestation için güvenlik belirteçleri oluşturma Azure IoT C SDK tarafından desteklenir. Tek bir kaydı kanıtlamak için Azure IoT C SDK'yı kullanan bir örnek için, [simetrik anahtarlara](quick-create-simulated-device-symm-key.md)sahip simüle edilmiş bir aygıt sağlama konusuna bakın.


## <a name="group-enrollments"></a>Grup Kayıtları

Grup kayıtları için simetrik anahtarlar, sağlama yaparken doğrudan aygıtlar tarafından kullanılmaz. Bunun yerine, türemiş bir aygıt anahtarı kullanarak bir kayıt grubu hükmüne ait aygıtlar. 

İlk olarak, bir kayıt grubuyla doğrulanan her aygıt için benzersiz bir kayıt kimliği tanımlanır. Kayıt kimliği için geçerli karakterler küçük alfasayısal ve tire ('-') vardır. Bu kayıt kimliği, aygıtı tanımlayan benzersiz bir şey olmalıdır. Örneğin, eski bir aygıt birçok güvenlik özelliğini desteklemeyebilir. Eski aygıtta yalnızca bu aygıtı benzersiz olarak tanımlamak için kullanılabilir bir MAC adresi veya seri numarası olabilir. Bu durumda, bir kayıt kimliği MAC adresi ve seri numarası aşağıdakine benzer oluşturulabilir:

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Bu tam örnek, [simetrik anahtarlar makalesini kullanarak eski aygıtların nasıl sağlanır' da](how-to-legacy-device-symm-key.md) kullanılır.

Aygıt için bir kayıt kimliği tanımlandıktan sonra, kayıt grubunun simetrik anahtarı, türetilmiş bir aygıt anahtarı oluşturmak için kayıt kimliğinin [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) karmasını hesaplamak için kullanılır. Kayıt numarasının karma işlemi aşağıdaki C# kodu ile gerçekleştirilebilir:

```csharp
using System; 
using System.Security.Cryptography; 
using System.Text;  

public static class Utils 
{ 
    public static string ComputeDerivedSymmetricKey(byte[] masterKey, string registrationId) 
    { 
        using (var hmac = new HMACSHA256(masterKey)) 
        { 
            return Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(registrationId))); 
        } 
    } 
} 
```

```csharp
String deviceKey = Utils.ComputeDerivedSymmetricKey(Convert.FromBase64String(masterKey), registrationId);
```

Elde edilen aygıt anahtarı daha sonra attestation için kullanılmak üzere bir SAS belirteci oluşturmak için kullanılır. Bir kayıt grubundaki her aygıtın, benzersiz bir türetilmiş anahtardan oluşturulan bir güvenlik belirteci kullanılarak onay alması gerekir. Kayıt grubu simetrik anahtarı doğrudan attestation için kullanılamaz.

#### <a name="installation-of-the-derived-device-key"></a>Türetilmiş aygıt anahtarının yüklenmesi

İdeal olarak cihaz anahtarları türetilir ve fabrikaya yüklenir. Bu yöntem, grup anahtarının aygıta dağıtılan hiçbir yazılıma dahil edilemeyeceğini garanti eder. Cihaza bir MAC adresi veya seri numarası atandığında, anahtar türetilebilir ve üretici nin saklamayı seçtiği şekilde aygıta enjekte edilebilir.

Her aygıt kayıt kimliğini grup kayıt anahtarı **(K)** ile ilişkilendirerek fabrikada oluşturulan aygıt anahtarları tablosunu gösteren aşağıdaki diyagramı göz önünde bulundurun. 

![Fabrikadan atanan aygıt anahtarları](./media/concepts-symmetric-key-attestation/key-diversification.png)

Her aygıtın kimliği, fabrikaya yüklenen kayıt kimliği ve türetilmiş aygıt anahtarıyla temsil edilir. Aygıt anahtarı hiçbir zaman başka bir konuma kopyalanır ve grup anahtarı hiçbir zaman aygıtta depolanır.

Aygıt anahtarları fabrikada yüklü değilse, aygıt kimliğini güvenli bir şekilde depolamak için bir [donanım güvenlik modülü HSM](concepts-security.md#hardware-security-module) kullanılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Simetrik Anahtar attestation bir anlayışa sahip, daha fazla bilgi edinmek için aşağıdaki makaleleri göz atın:

* [Hızlı Başlangıç: Simetrik anahtarlar ile bir simülasyon cihazı sağlama](quick-create-simulated-device-symm-key.md)
* [Otomatik sağlamadaki kavramlar hakkında bilgi edinin](./concepts-auto-provisioning.md)
* [Otomatik sağlama yı kullanmaya başlayın](./quick-setup-auto-provision.md) 
