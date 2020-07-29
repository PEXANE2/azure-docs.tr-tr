---
title: .NET SDK ile Azure Cosmos DB HTTP 408 veya istek zaman aşımı sorunlarını giderme
description: .NET SDK isteği zaman aşımı özel durumunu tanılama ve çözme
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 29b0c6237ae04ea5da9ec496498fc7c20890b173
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294632"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-net-sdk-request-timeout"></a>Tanılama ve sorun giderme Azure Cosmos DB .NET SDK isteği zaman aşımı
HTTP 408 hatası, zaman aşımı sınırı gerçekleşmeden önce SDK isteği tamamlayamadıysa oluşur.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, istek zaman aşımı özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="1-high-cpu-utilization-most-common-case"></a>1. yüksek CPU kullanımı (en yaygın durum)
En iyi gecikme için CPU kullanımının yaklaşık %40 olması önerilir. En yüksek (ortalama değil) CPU kullanımını izlemek için kullanılan Aralık olarak 10 saniye kullanmanız önerilir. CPU artışlarının tek bir sorgu için birden çok bağlantı olabileceği çapraz bölüm sorgularıyla daha yaygın hale gelir.

#### <a name="solution"></a>Çözüm:
SDK 'Yı kullanan istemci uygulamasının ölçeği yukarı/aşağı yerleştirilmelidir.

### <a name="2-socket--port-availability-might-be-low"></a>2. yuva/bağlantı noktası kullanılabilirliği düşük olabilir
Azure 'da çalışırken, .NET SDK kullanan istemciler Azure SNAT (PAT) bağlantı noktası tükenmesi ile aynı olabilir.

#### <a name="solution-1"></a>1. Çözüm:
[SNAT bağlantı noktası Tükenme kılavuzunu](troubleshoot-dot-net-sdk.md#snat)izleyin.

#### <a name="solution-2"></a>2. Çözüm:
Bir HTTP proxy kullanıyorsanız, SDK 'da yapılandırılan bağlantı sayısını destekleyediğinden emin olun `ConnectionPolicy` .
Aksi halde bağlantı sorunlarıyla karşılaşın.

### <a name="3-creating-multiple-client-instances"></a>3. birden çok istemci örneği oluşturma
Birden çok istemci örneği oluşturmak bağlantı çakışması ve zaman aşımı sorunlarına yol açabilir.

#### <a name="solution"></a>Çözüm:
[Performans ipuçlarını](performance-tips-dotnet-sdk-v3-sql.md#sdk-usage)izleyin ve bir işlemin tamamında tek bir CosmosClient örneği kullanın.

### <a name="4-hot-partition-key"></a>4. sık erişimli bölüm anahtarı
Azure Cosmos DB, genel olarak sağlanan üretilen işi fiziksel bölümler arasında eşit olarak dağıtır. Bir sıcak bölüm olduğunda, fiziksel bölümdeki bir veya daha fazla mantıksal bölüm, tüm fiziksel bölümlerin RU/sn 'sini tüketirken diğer fiziksel bölümlerdeki RU/s kullanılmıyor olur. Bir belirti olarak, tüketilen toplam RU/sn, veritabanı veya kapsayıcıdaki genel olarak sağlanan RU/s değerinden daha az olacaktır, ancak etkin mantıksal bölüm anahtarına karşı isteklerde daraltma (429s) görmeye devam edersiniz. İş yükünün sık erişimli bir bölümle karşılaşıyor olup olmadığını görmek için [NORMALLEŞTIRILMIŞ ru tüketim ölçümünü](monitor-normalized-request-units.md) kullanın. 

#### <a name="solution"></a>Çözüm:
İstek birimini ve depolamayı eşit bir şekilde dağıtan iyi bir bölüm anahtarı seçin. [Bölüm anahtarınızı değiştirme](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/)hakkında bilgi edinin.

### <a name="5-high-degree-of-concurrency"></a>5. yüksek oranda eşzamanlılık
Uygulama yüksek düzeyde eşzamanlılık yapıyor ve bu, kanalda çekişmeye yol açabilir

#### <a name="solution"></a>Çözüm:
SDK 'Yı kullanan istemci uygulamasının ölçeği yukarı/aşağı yerleştirilmelidir.

### <a name="6-large-requests-andor-responses"></a>6. büyük istek ve/veya Yanıt
Büyük istekler veya yanıtlar, görece düşük eşzamanlılık derecesine sahip olan kanal ve exacerbate çekişmesi üzerinde satır başı engellemeye yol açabilir.

#### <a name="solution"></a>Çözüm:
SDK 'Yı kullanan istemci uygulamasının ölçeği yukarı/aşağı yerleştirilmelidir.

### <a name="7-failure-rate-is-within-cosmos-db-sla"></a>7. başarısızlık oranı Cosmos DB SLA içinde
Uygulama geçici sorunları işleyebilmelidir ve gerektiğinde yeniden deneyebilir. 408 özel durumlar, oluşturma yollarındaki hizmetin öğeyi oluşturup oluşturmadığı veya olmadığı hakkında bilgi sahibi olmadığı için yeniden denenmez. Oluşturma için aynı öğenin tekrar gönderilmesi çakışma özel durumuna neden olur. Kullanıcı uygulamaları iş mantığı, çakışmaları işlemek için özel mantık içerebilir, bu da bir oluşturma yeniden denenmesinden sonra var olan bir öğenin belirsizliğin kesintiye uğramasını sağlar.

### <a name="8-failure-rate-is-violating-the-cosmos-db-sla"></a>8. hata oranı Cosmos DB SLA 'sını ihlal ediyor
Lütfen Azure desteğine başvurun.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullanırken sorunları [Tanılama ve giderme](troubleshoot-dot-net-sdk.md)
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin