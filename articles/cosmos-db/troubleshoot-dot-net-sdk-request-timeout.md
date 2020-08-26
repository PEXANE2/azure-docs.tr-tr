---
title: .NET SDK ile Azure Cosmos DB HTTP 408 veya istek zaman aşımı sorunlarını giderme
description: .NET SDK istek zaman aşımı özel durumlarını tanılamayı ve gidermeyi öğrenin.
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 4e7b1fdbcbf85aa4c64a38deeeb03ede9a0e4b87
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871148"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-net-sdk-request-timeout-exceptions"></a>Tanılama ve sorun giderme Azure Cosmos DB .NET SDK isteği zaman aşımı özel durumları
SDK, zaman aşımı sınırı gerçekleşmeden önce isteği tamamlayamadıysa HTTP 408 hatası oluşur.

## <a name="customize-the-timeout-on-the-azure-cosmos-db-net-sdk"></a>Azure Cosmos DB .NET SDK 'sindeki zaman aşımını özelleştirme

SDK, her biri farklı bir kapsama sahip olan zaman aşımlarını denetlemek için iki farklı seçenek içerir.

### <a name="requesttimeout"></a>RequestTimeout

`CosmosClientOptions.RequestTimeout`(Veya `ConnectionPolicy.RequestTimeout` SDK v2 için) yapılandırması, her bir ağ isteğini etkileyen bir zaman aşımı ayarlamanıza olanak sağlar. Bir kullanıcı tarafından başlatılan bir işlem birden çok ağ isteğini kapsayabilir (örneğin, azaltma olabilir). Bu yapılandırma, yeniden deneme sırasında her bir ağ isteği için geçerlidir. Bu zaman aşımı, uçtan uca bir işlem isteği zaman aşımı değildir.

### <a name="cancellationtoken"></a>CancellationToken

SDK 'daki tüm zaman uyumsuz işlemlerin isteğe bağlı bir CancellationToken parametresi vardır. Bu [CancellationToken](https://docs.microsoft.com/dotnet/standard/threading/how-to-listen-for-cancellation-requests-by-polling) parametresi tüm ağ isteklerinde tüm işlem boyunca kullanılır. Ağ istekleri arasında, iptal belirteci işaretli olabilir ve ilgili belirtecin geçerliliği dolmuşsa bir işlem iptal edilir. İptal belirteci, işlem kapsamında yaklaşık bir beklenen zaman aşımını tanımlamak için kullanılmalıdır.

> [!NOTE]
> `CancellationToken`Parametresi, kitaplığın [geçersiz duruma neden](https://devblogs.microsoft.com/premier-developer/recommended-patterns-for-cancellationtoken/)olmadığı durumlarda iptal işlemini denetlebileceği bir mekanizmadır. İşlem, İptalde tanımlanan zaman varsa tam olarak iptal etmeyebilir. Bunun yerine, zaman geçtikten sonra bunun güvenli olduğunu iptal eder.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, istek zaman aşımı özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="high-cpu-utilization"></a>Yüksek CPU kullanımı
Yüksek CPU kullanımı en yaygın durumdur. En iyi gecikme için CPU kullanımı yaklaşık %40 olmalıdır. Maksimum (ortalama değil) CPU kullanımını izlemek için kullanılan Aralık olarak 10 saniye kullanın. CPU ani artışları, tek bir sorgu için birden çok bağlantı olabileceği, bölümler arası sorgularla daha yaygındır.

#### <a name="solution"></a>Çözüm:
SDK 'Yı kullanan istemci uygulamasının ölçeği yukarı veya aşağı yerleştirilmelidir.

### <a name="socket-or-port-availability-might-be-low"></a>Yuva veya bağlantı noktası kullanılabilirliği düşük olabilir
Azure 'da çalışırken, .NET SDK kullanan istemciler Azure SNAT (PAT) bağlantı noktası tükenmesi ile aynı olabilir.

#### <a name="solution-1"></a>1. Çözüm:
Azure VM 'lerinde çalıştırıyorsanız, [SNAT bağlantı noktası Tükenme Kılavuzu](troubleshoot-dot-net-sdk.md#snat)' nu izleyin.

#### <a name="solution-2"></a>2. Çözüm:
Azure App Service kullanıyorsanız, [bağlantı hataları sorun giderme kılavuzunu](../app-service/troubleshoot-intermittent-outbound-connection-errors.md#cause) izleyin ve [App Service tanılamayı kullanın](https://azure.github.io/AppService/2018/03/01/Deep-Dive-into-TCP-Connections-in-App-Service-Diagnostics.html).

#### <a name="solution-3"></a>Çözüm 3:
Azure Işlevleri üzerinde çalıştırıyorsanız, tüm ilgili hizmetler (Azure Cosmos DB dahil) için tek veya statik istemcileri sürdürmek için [Azure işlevleri önerisi](../azure-functions/manage-connections.md#static-clients) ' nin takip ettiğini doğrulayın. İşlev Uygulaması barındırmanın türüne ve boyutuna göre [hizmet sınırlarını](../azure-functions/functions-scale.md#service-limits) denetleyin.

#### <a name="solution-4"></a>Çözüm 4:
Bir HTTP proxy kullanıyorsanız, SDK 'da yapılandırılan bağlantı sayısını destekleyediğinden emin olun `ConnectionPolicy` . Aksi takdirde, bağlantı sorunlarıyla karşılaşırsınız.

### <a name="create-multiple-client-instances"></a>Birden çok istemci örneği oluşturma
Birden çok istemci örneği oluşturmak bağlantı çakışması ve zaman aşımı sorunlarına yol açabilir.

#### <a name="solution"></a>Çözüm:
[Performans ipuçlarını](performance-tips-dotnet-sdk-v3-sql.md#sdk-usage)izleyin ve bir işlemin tamamında tek bir CosmosClient örneği kullanın.

### <a name="hot-partition-key"></a>Sık erişimli bölüm anahtarı
Azure Cosmos DB, genel olarak sağlanan üretilen işi fiziksel bölümler arasında eşit olarak dağıtır. Bir sıcak bölüm olduğunda, fiziksel bir bölümdeki bir veya daha fazla mantıksal bölüm anahtarı, tüm fiziksel bölümün Istek birimi (RU/s) olarak tüketiyor. Aynı zamanda, diğer fiziksel bölümlerdeki RU/s kullanılmıyor. Bir belirti olarak, tüketilen toplam RU/sn, veritabanı veya kapsayıcıdaki genel olarak sağlanan RU/s değerinden daha az olacaktır, ancak etkin mantıksal bölüm anahtarına karşı isteklerde daraltma (429s) görmeye devam edersiniz. İş yükünün sık erişimli bir bölümle karşılaşıyor olup olmadığını görmek için [NORMALLEŞTIRILMIŞ ru tüketim ölçümünü](monitor-normalized-request-units.md) kullanın. 

#### <a name="solution"></a>Çözüm:
İstek birimini ve depolamayı eşit bir şekilde dağıtan iyi bir bölüm anahtarı seçin. [Bölüm anahtarınızı değiştirme](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/)hakkında bilgi edinin.

### <a name="high-degree-of-concurrency"></a>Yüksek oranda eşzamanlılık
Uygulama yüksek düzeyde eşzamanlılık yapıyor ve bu, kanalda çekişmeye yol açabilir.

#### <a name="solution"></a>Çözüm:
SDK 'Yı kullanan istemci uygulamasının ölçeği yukarı veya aşağı yerleştirilmelidir.

### <a name="large-requests-or-responses"></a>Büyük istekler veya yanıtlar
Büyük istekler veya yanıtlar, görece düşük eşzamanlılık derecesine sahip olan kanal ve exacerbate çekişmesi üzerinde satır başı engellemeye yol açabilir.

#### <a name="solution"></a>Çözüm:
SDK 'Yı kullanan istemci uygulamasının ölçeği yukarı veya aşağı yerleştirilmelidir.

### <a name="failure-rate-is-within-the-azure-cosmos-db-sla"></a>Başarısızlık oranı Azure Cosmos DB SLA içinde
Uygulama geçici sorunları işleyebilmelidir ve gerektiğinde yeniden deneyebilir. Tüm 408 özel durumlar, oluşturma yollarındaki hizmetin öğeyi oluşturup oluşturmadığından emin olmak imkansız olduğu için yeniden denenmez. Oluşturma için aynı öğenin tekrar gönderilmesi çakışma özel durumuna neden olur. Kullanıcı uygulamaları iş mantığı, çakışmaları işlemek için özel mantık içerebilir ve bu da bir oluşturma yeniden denenmesinden sonra var olan bir öğenin belirsizliğin kesintiye uğramasını sağlar.

### <a name="failure-rate-violates-the-azure-cosmos-db-sla"></a>Hata oranı Azure Cosmos DB SLA 'sını ihlal ediyor
[Azure desteği](https://aka.ms/azure-support)'ne başvurun.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-dot-net-sdk.md) .
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md)için performans yönergeleri hakkında bilgi edinin.
