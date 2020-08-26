---
title: Azure Cosmos DB bir "Istek üst bilgisi çok büyük" iletisini veya 400 Hatalı isteği sorun giderme
description: İstek üst bilgisini tanılamayı ve gidermeyi öğrenin çok büyük özel durum.
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: a4d8a919d1881f61e490f135cc8fb1659c64cbd3
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871131"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-header-too-large-message"></a>Tanılama ve sorun giderme Azure Cosmos DB "Istek üst bilgisi çok büyük" iletisi
"Istek üst bilgisi çok büyük" iletisi bir HTTP hata kodu 400 ile oluşturulur. İstek üstbilgisinin boyutu, izin verilen en büyük boyutu aşarsa, bu hata oluşur. SDK 'nın en son sürümünü kullanmanızı öneririz. En az sürüm 3. x veya 2. x kullanın, çünkü bu sürümler özel durum iletisine üst bilgi boyutu izleme ekler.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Oturum veya devamlılık belirteci çok büyükse "Istek üst bilgisi çok büyük" iletisi oluşur. Aşağıdaki bölümlerde sorunun nedeni ve çözümü her kategoride açıklanır.

### <a name="session-token-is-too-large"></a>Oturum belirteci çok büyük

#### <a name="cause"></a>Neden:
400 hatalı bir istek, oturum belirteci çok büyük olduğu için büyük olasılıkla oluşur. Aşağıdaki deyimler true ise, oturum belirteci çok büyük olur:

* Hata, bir devamlılık belirteci olmadığı oluşturma, okuma ve güncelleştirme gibi nokta işlemlerinde oluşur.
* Uygulamada herhangi bir değişiklik yapılmadan özel durum başlatıldı. Kapsayıcıda bölüm sayısı arttıkça oturum belirteci artar. Veri miktarı arttıkça veya üretilen iş arttırılırsa bölüm sayısı artar.

#### <a name="temporary-mitigation"></a>Geçici risk azaltma: 
Tüm oturum belirteçlerini sıfırlamak için istemci uygulamanızı yeniden başlatın. Sonuç olarak, oturum belirteci soruna neden olan önceki boyuta geri büyür. Bu sorunu tamamen önlemek için, sonraki bölümde çözümü kullanın.

#### <a name="solution"></a>Çözüm:
1. [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) veya [.net v2](performance-tips.md) performans ipuçları makalelerindeki yönergeleri izleyin. Doğrudan bağlantı modunu, Iletim Denetim Protokolü (TCP) ile kullanmak için uygulamayı dönüştürün. TCP protokolüne sahip doğrudan bağlantı modunun, HTTP protokolü gibi üst bilgi boyutu kısıtlaması yoktur, bu nedenle bu sorunu önler. Hizmet birlikte çalışabilirliği kullanılamadığında sorgu işlemleri için bir düzelme sahip olan SDK 'nın en son sürümünü kullandığınızdan emin olun.
1. TCP protokolüne sahip doğrudan bağlantı modu, iş yükünüz için bir seçenek değilse, [istemci tutarlılığı düzeyini](how-to-manage-consistency.md)değiştirerek bu ayarı azaltabilirsiniz. Oturum belirteci yalnızca Azure Cosmos DB için varsayılan tutarlılık düzeyi olan oturum tutarlılığı için kullanılır. Diğer tutarlılık düzeyleri oturum belirtecini kullanmaz.

### <a name="continuation-token-is-too-large"></a>Devamlılık belirteci çok büyük

#### <a name="cause"></a>Neden:
400 hatalı istek, devamlılık belirteci çok büyük büyüdüyse veya farklı sorgularda farklı devamlılık belirteci boyutları varsa, devamlılık belirtecinin kullanıldığı sorgu işlemlerinde meydana gelir.
    
#### <a name="solution"></a>Çözüm:
1. [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) veya [.net v2](performance-tips.md) performans ipuçları makalelerindeki yönergeleri izleyin. Uygulamayı, TCP protokolüyle doğrudan bağlantı modunu kullanacak şekilde dönüştürün. TCP protokolüne sahip doğrudan bağlantı modunun, HTTP protokolü gibi üst bilgi boyutu kısıtlaması yoktur, bu nedenle bu sorunu önler. 
1. TCP protokolüne sahip doğrudan bağlantı modu iş yükünüz için bir seçenek değilse, `ResponseContinuationTokenLimitInKb` seçeneğini ayarlayın. Bu seçeneği, `FeedOptions` v2 'de veya v3 'de bulabilirsiniz `QueryRequestOptions` .

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-dot-net-sdk.md) .
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md)için performans yönergeleri hakkında bilgi edinin.
