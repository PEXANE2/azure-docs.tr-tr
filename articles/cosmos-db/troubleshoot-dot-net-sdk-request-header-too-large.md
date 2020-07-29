---
title: Azure Cosmos DB istek üst bilgisi çok büyük veya 400 hatalı istek ile sorun giderme
description: İstek üst bilgisini tanılama ve çözme çok büyük özel durum
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 942af52c43a1d4526a42f20df40abf3f510e0ee6
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294672"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-request-header-too-large-message"></a>Tanılama ve sorun giderme Azure Cosmos DB istek üst bilgisi çok büyük ileti
İstek üst bilgisi, HTTP hata kodu 400 ile çok büyük bir ileti oluşturuldu. Bu hata, istek üst bilgisinin boyutu çok büyükse ve izin verilen en büyük boyutu aşmışsa oluşur. SDK 'nın en son sürümünü kullanmanızı öneririz. Bu sürümler özel durum iletisine üst bilgi boyutu izleme eklemesini sağladığından, en az 3. x veya 2. x sürümünü kullandığınızdan emin olun.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Oturum veya devamlılık belirteci çok büyükse istek üst bilgisi çok büyük bir ileti ortaya çıkar. Aşağıdaki bölümlerde, her kategoride soruna neden olan sorun ve çözüm açıklanır.

### <a name="1-session-token-too-large"></a>1. oturum belirteci çok büyük

#### <a name="cause"></a>Neden:
400 hatalı istek, büyük olasılıkla oturum belirtecinin büyük olması nedeniyle oluşur. Aşağıdaki deyimler true ise, oturum belirtecinin çok büyük olduğunu onaylar.

* Hata, oluşturma, okuma, güncelleştirme ve vb. gibi bir devamlılık belirteci olmayan nokta işleminde oluşur.
* Uygulamada herhangi bir değişiklik yapılmadan özel durum başlatıldı. Kapsayıcıda bulunan bölüm sayısı arttıkça oturum belirteci de artar. Bölüm sayısı arttıkça veri miktarı artar veya aktarım hızı artar.

#### <a name="temporary-mitigation"></a>Geçici risk azaltma: 
Tüm oturum belirteçlerini sıfırlamak için istemci uygulamanızı yeniden başlatın. Oturum belirteci sonunda soruna neden olan önceki boyuta geri dönüş yapılır. Bu nedenle, bu sorunu tamamen önlemek için sonraki bölümde çözümü kullanın.

#### <a name="solution"></a>Çözüm:
1. [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) veya [.net v2](performance-tips.md) performans ipuçları makalesindeki YÖNERGELERI izleyin ve uygulamayı TCP protokolüyle doğrudan bağlantı modunu kullanacak şekilde dönüştürün. TCP protokolüyle doğrudan mod, HTTP protokolü gibi üst bilgi boyutu kısıtlamasına sahip değildir, bu nedenle bu sorunu önler. Hizmet birlikte çalışabilirliği kullanılabilir olmadığında sorgu işlemleri için bir düzelme sahip olan SDK 'nın en son sürümünü kullandığınızdan emin olun.
2. TCP protokolüyle doğrudan bağlantı modu, iş yükünüz için bir seçenek değilse, [istemci tutarlılığı düzeyini](how-to-manage-consistency.md)değiştirerek bunu azaltın. Oturum belirteci yalnızca Azure Cosmos DB için varsayılan tutarlılık düzeyi olan oturum tutarlılığı için kullanılır. Diğer tutarlılık düzeyleri oturum belirtecini kullanmaz.

### <a name="2-continuation-token-too-large"></a>2. devamlılık belirteci çok büyük

#### <a name="cause"></a>Neden:
400 hatalı istek, devamlılık belirtecinin kullanıldığı sorgu işlemlerinde meydana gelir. Devamlılık belirteci çok büyük büyüdüyse veya farklı sorgularda farklı devamlılık belirteci boyutları varsa.
    
#### <a name="solution"></a>Çözüm:
1. [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) veya [.net v2](performance-tips.md) performans ipuçları makalesindeki YÖNERGELERI izleyin ve uygulamayı TCP protokolüyle doğrudan bağlantı modunu kullanacak şekilde dönüştürün. TCP protokolüyle doğrudan mod, HTTP protokolü gibi üst bilgi boyutu kısıtlamasına sahip değildir, bu nedenle bu sorunu önler. 
3. TCP protokolüyle doğrudan bağlantı modu, iş yükünüz için bir seçenek değilse, seçeneğini ayarlamayı deneyin `ResponseContinuationTokenLimitInKb` . Bu seçeneği, `FeedOptions` for v2 veya içinde `QueryRequestOptions` v3 bölümünde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullanırken sorunları [Tanılama ve giderme](troubleshoot-dot-net-sdk.md)
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin
