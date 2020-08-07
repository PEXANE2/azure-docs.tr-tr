---
title: .NET SDK ile Azure Cosmos DB HTTP 408 veya istek zaman aşımı sorunlarını giderme
description: .NET SDK isteği zaman aşımı özel durumunu tanılama ve çözme
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: a0469feed391025f8dd50a7f8b11b96265b0df29
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87987418"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-net-sdk-request-timeout"></a>Tanılama ve sorun giderme Azure Cosmos DB .NET SDK isteği zaman aşımı
HTTP 408 hatası, zaman aşımı sınırı gerçekleşmeden önce SDK isteği tamamlayamadıysa oluşur.

## <a name="customizing-the-timeout-on-the-azure-cosmos-net-sdk"></a>Azure Cosmos .NET SDK 'sında zaman aşımını özelleştirme

SDK, her biri farklı bir kapsama sahip olan zaman aşımlarını denetlemek için iki farklı seçenek içerir.

### <a name="requesttimeout"></a>RequestTimeout

`CosmosClientOptions.RequestTimeout`(Veya `ConnectionPolicy.RequestTimeout` SDK v2 için) yapılandırması, her bir ağ isteğini etkileyen bir zaman aşımı ayarlamanıza olanak sağlar.  Bir kullanıcı tarafından başlatılan bir işlem birden çok ağ isteğini kapsayabilir (örneğin, azaltma olabilir) ve bu yapılandırma yeniden denemeden her bir ağ isteği için geçerlidir. Bu bir uçtan uca işlem isteği zaman aşımı değildir.

### <a name="cancellationtoken"></a>CancellationToken

SDK 'daki tüm zaman uyumsuz işlemlerin isteğe bağlı bir CancellationToken parametresi vardır. Bu [CancellationToken](https://docs.microsoft.com/dotnet/standard/threading/how-to-listen-for-cancellation-requests-by-polling) tüm ağ isteklerinde tüm işlem boyunca kullanılır. Ağ istekleri arasında, CancellationToken denetlenebilir ve ilgili belirtecin geçerliliği dolmuşsa bir işlem iptal edilir. CancellationToken, işlem kapsamında yaklaşık bir beklenen zaman aşımını tanımlamak için kullanılmalıdır.

> [!NOTE]
> CancellationToken, kitaplığın [geçersiz duruma neden](https://devblogs.microsoft.com/premier-developer/recommended-patterns-for-cancellationtoken/)olmadığı durumlarda iptal işlemini denetlebileceği bir mekanizmadır. İşlem, İptalde tanımlanan zaman varsa tam olarak iptal etmeyebilir, ancak bunun yerine, zaman geçtikten sonra bunu yapmak güvenli olduğunda iptal edilir.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, istek zaman aşımı özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="1-high-cpu-utilization-most-common-case"></a>1. yüksek CPU kullanımı (en yaygın durum)
En iyi gecikme için CPU kullanımının yaklaşık %40 olması önerilir. En yüksek (ortalama değil) CPU kullanımını izlemek için kullanılan Aralık olarak 10 saniye kullanmanız önerilir. CPU artışlarının tek bir sorgu için birden çok bağlantı olabileceği çapraz bölüm sorgularıyla daha yaygın hale gelir.

#### <a name="solution"></a>Çözüm:
SDK 'Yı kullanan istemci uygulamasının ölçeği yukarı/aşağı yerleştirilmelidir.

### <a name="2-socket--port-availability-might-be-low"></a>2. yuva/bağlantı noktası kullanılabilirliği düşük olabilir
Azure 'da çalışırken, .NET SDK kullanan istemciler Azure SNAT (PAT) bağlantı noktası tükenmesi ile aynı olabilir.

#### <a name="solution-1"></a>1. Çözüm:
Azure VM 'lerinde çalıştırıyorsanız, [SNAT bağlantı noktası Tükenme Kılavuzu](troubleshoot-dot-net-sdk.md#snat)' nu izleyin.

#### <a name="solution-2"></a>2. Çözüm:
Azure App Service üzerinde çalıştırıyorsanız, [bağlantı hataları sorun giderme kılavuzunu](../app-service/troubleshoot-intermittent-outbound-connection-errors.md#cause) izleyin ve [App Service tanılamayı kullanın](https://azure.github.io/AppService/2018/03/01/Deep-Dive-into-TCP-Connections-in-App-Service-Diagnostics.html).

#### <a name="solution-3"></a>Çözüm 3:
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
