---
title: HTTP uzantısı veri sözleşmesi-Azure
description: Bu makalede, canlı video analizi modülü ile AI veya CV modülünüzün arasında ileti göndermek için HTTP protokolünü kullanma hakkında bilgi edineceksiniz.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: 54037d904bd86120af054f5d2f6a1075f7379004
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691961"
---
# <a name="http-extension-data-contract"></a>HTTP uzantısı veri sözleşmesi

Bu makalede, canlı video analizi modülü ile AI veya CV modülünüzün arasında ileti göndermek için HTTP protokolünü kullanma hakkında bilgi edineceksiniz.

HTTP sözleşmesi aşağıdaki iki bileşen arasında tanımlanır:

* HTTP sunucusu
* IoT Edge modülündeki canlı video analizi HTTP istemcisi olarak davranır

## <a name="request"></a>İstek

Canlı video analizi modülünden HTTP sunucunuza gönderilen istekler aşağıdaki gibi olacaktır:

|Anahtar|Değer|
|---|---|
|POST|`https://hostname/optional-path?optional-query`|
|Kabul Et|Uygulama/JSON,  */*|
|Yetkilendirme|Temel, Özet, taşıyıcı (özel üst bilgi desteği aracılığıyla)|
|İçerik Türü|image/jpeg<br/>image/png<br/>image/bmp<br/>resim/x-ham|
|İçerik uzunluğu Gövde uzunluğu (bayt)|
|User-Agent|Azure Media Services|
|Gövde|Desteklenen içerik türlerinden birinde ikili kodlanmış görüntü baytları.|

### <a name="example"></a>Örnek

```
POST http://localhost:8080/inference HTTP/1.1
Host: localhost:8080
x-ms-client-request-id: d6050cd4-c9f2-42d3-9adc-53ba7e440f17
Content-Type: image/bmp
Content-Length: 519222

(Image Binary Content)
```

## <a name="response"></a>Yanıt

Modülünüzün canlı video analizi modülüne olan yanıtları şu şekilde olmalıdır:

|Anahtar|Değer|
|---|---|
|Durum Kodları|200 Tamam-çıkarım sonuçları bulundu<br/>204 Içerik yok-AI tarafından içerik bulunamadı<br/>400 Hatalı Istek-beklenmiyordu<br/>500 iç sunucu hatası-beklenmiyordu<br/>503 sunucu meşgul-"yeniden deneme" başlığına göre veya durum üst bilgisinde ön ayarlı olmayan varsayılan bir süre temelinde, AMS yeniden kapatılacak.|
|İçerik Türü|uygulama/json|
|İçerik Uzunluğu|Gövde uzunluğu, bayt cinsinden|
|Gövde|Tek "ının" özelliğine sahip JSON nesnesi.|

### <a name="example"></a>Örnek

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 468
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 17 Apr 2020 04:44:01 GMT

{
  "inferences": [
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.9048132 },
        "box": { "l": 0.42681578, "t": 0.47660735, "w": 0.019501392, "h": 0.020954132 }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.8953932 },
        "box": { "l": 0.55083525, "t": 0.4843858, "w": 0.046550274, "h": 0.046502113 }
      }
    }    
  ]
}
```

Yanıtların, aşağıda tanımlanan önceden oluşturulan şemadan sonra geçerli JSON belgeleri kullanılarak döndürülmesi önemle önerilir. Bu, diğer bileşenlerle birlikte çalışabilirliğini ve canlı video analizi modülüne eklenen olası gelecekteki özellikleri daha iyi sağlar.

Modülünüzün içerik türü "Application/JSON" olmayan bir yanıt döndürürse, canlı video analizi iletiyi temel 64 içeriği olarak kodlayıp donuk bir JSON yükü olarak seri hale getirir.

Modülünüzün içerik türü "Application/JSON" olarak bir yanıt döndürürse, ancak JSON şeması aşağıda özetlenen çıkarım meta veri şemasını izlemediği için ileti yükü işlem hattı üzerinden iletilir, ancak birlikte çalışabilirlik azaltılır.

> [!NOTE]
> Modülünüzün herhangi bir sonuç oluşturmuyorsa, boş bir yanıt gövdesi ile HTTP 204 durum kodu (Içerik yok) döndürmelidir. Canlı video analizi bunu boş bir sonuç olarak anlayacak ve olayı ardışık düzen boyunca iletmeyecektir.

## <a name="data-contracts---class-hierarchy"></a>Veri sözleşmeleri-sınıf hiyerarşisi

![sınıf hiyerarşisi](./media/http-data-contract/class-hierarchy.png)

## <a name="next-steps"></a>Sonraki adımlar

[gRPC veri sözleşmesi](grpc-data-contract.md)
