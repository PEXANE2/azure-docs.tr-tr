---
title: Azure Cosmos DB yetkisiz özel durum sorunlarını giderme
description: Yetkisiz özel durumu tanılama ve çözme
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 872780ff65dabe5a931ae712c0f99b6e4f3e726d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294518"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-unauthorized-exception"></a>Yetkisiz özel durum Azure Cosmos DB tanılama ve sorun giderme

HTTP 401: HTTP isteğinde bulunan MAC imzası, hesaplanan imzayla aynı değil.
Şu 401 hata iletisini aldıysanız: "HTTP isteğinde bulunan MAC imzası, hesaplanan imzayla aynı değil." Bu, aşağıdaki senaryolardan kaynaklanıyor olabilir.

Eski SDK 'lar özel durum, doğru 401 Yetkisiz özel durumu yerine geçersiz bir JSON özel durumu olarak görünebilir. Daha yeni SDK 'lar bu senaryoyu doğru bir şekilde işler ve geçerli bir hata iletisi verir.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, yetkisiz özel durum için bilinen nedenleri ve çözümleri içerir.

### <a name="1-key-was-not-properly-rotated-is-the-most-common-scenario"></a>1. anahtar, en yaygın senaryonun doğru şekilde döndürülmedi.
401 MAC imzası, anahtar döndürme işleminden kısa süre sonra görülür ve herhangi bir değişiklik yaşanmadan durdurulur. 

#### <a name="solution"></a>Çözüm:
Anahtar döndürüldü ve [en iyi yöntemlere](secure-access-to-data.md#key-rotation) uygun değildi. Cosmos DB hesap anahtarı döndürme, Cosmos DB hesap boyutuna bağlı olarak birkaç saniye ile muhtemelen gün arasında bir yere sürebilir.

### <a name="2-the-key-is-misconfigured"></a>2. anahtar yanlış yapılandırılmış 
401 MAC imzası sorunu tutarlı olacak ve bu anahtarı kullanan tüm çağrılar için gerçekleşir

#### <a name="solution"></a>Çözüm:
Anahtar uygulamada hatalı yapılandırılmış ve hesap için yanlış anahtar kullanıyor ya da anahtarın tamamı kopyalanmadı.

### <a name="3-the-application-is-using-the-read-only-keys-for-write-operations"></a>3. uygulama, yazma işlemleri için salt okunurdur anahtarlarını kullanıyor
401 MAC imza sorunu yalnızca create veya Replace gibi yazma işlemleri için oluşuyor, ancak okuma isteği başarılı oldu.

#### <a name="solution"></a>Çözüm:
İşlemin başarılı bir şekilde tamamlanmasına izin vermek için uygulamayı okuma/yazma anahtarı kullanacak şekilde değiştirin.

### <a name="4-race-condition-with-create-container"></a>4. kapsayıcı oluştur ile yarış durumu
401 MAC imzası sorunu, bir kapsayıcı oluşturulduktan sonra kısa bir süre sonra görülür. Bu yalnızca kapsayıcı oluşturma tamamlanana kadar oluşur.

#### <a name="solution"></a>Çözüm:
Kapsayıcı oluşturma sırasında yarış durumu mevcuttur. Uygulama örneği, kapsayıcı oluşturma işlemi tamamlanmadan kapsayıcıya erişmeye çalışıyor. Bu yarış durumu için en yaygın senaryo, uygulamanın çalışıyor olması ve kapsayıcının silinip aynı adla yeniden oluşturulması durumunda olur. SDK, yeni kapsayıcıyı kullanmaya çalışır ancak kapsayıcı oluşturma işlemi devam ettiğinden gerekli anahtarlara sahip değildir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullanırken sorunları [Tanılama ve giderme](troubleshoot-dot-net-sdk.md)
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin