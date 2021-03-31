---
title: Azure Cosmos DB HTTP 408 sorunlarını giderin veya Java v4 SDK ile zaman aşımı sorunları isteyin
description: Java v4 SDK 'Sı ile Java SDK 'Sı istek zaman aşımı özel durumlarını tanılamayı ve gidermeyi öğrenin.
author: kushagrathapar
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.date: 10/28/2020
ms.author: kuthapar
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: a805300ac62d0627c9b06188c9764a6887947afe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94411295"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-java-v4-sdk-request-timeout-exceptions"></a>Java v4 SDK isteği zaman aşımı özel durumları Azure Cosmos DB tanılama ve sorun giderme
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

SDK, zaman aşımı sınırı gerçekleşmeden önce isteği tamamlayamadıysa HTTP 408 hatası oluşur.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Aşağıdaki liste, istek zaman aşımı özel durumları için bilinen nedenleri ve çözümleri içerir.

### <a name="existing-issues"></a>Mevcut sorunlar
İstekleri daha uzun süreli veya daha sık zaman aşımına uğramış olarak gördüğünüzü, lütfen Java v4 SDK 'sını en son sürüme yükseltin. NOTE: sürüm 4.7.0 ve üstünü kullanmanız önemle önerilir. Daha fazla ayrıntı için [Java v4 SDK sürüm notlarını](sql-api-sdk-java-v4.md) kullanıma alın.

### <a name="high-cpu-utilization"></a>Yüksek CPU kullanımı
Yüksek CPU kullanımı en yaygın durumdur. En iyi gecikme için CPU kullanımı yaklaşık %40 olmalıdır. Maksimum (ortalama değil) CPU kullanımını izlemek için kullanılan Aralık olarak 10 saniye kullanın. CPU ani artışları, tek bir sorgu için birden çok bağlantı olabileceği, bölümler arası sorgularla daha yaygındır.

#### <a name="solution"></a>Çözüm:
SDK 'Yı kullanan istemci uygulamasının ölçeği yukarı veya aşağı yerleştirilmelidir.

### <a name="connection-throttling"></a>Bağlantı azaltma
Bir konak makinesindeki bağlantı sınırının veya Azure SNAT (PAT) bağlantı noktası tükenmesi nedeniyle bağlantı azaltma gerçekleşecektir.

### <a name="connection-limit-on-a-host-machine"></a>Bir konak makinesinde bağlantı sınırı
Red hat gibi bazı Linux sistemleri, Toplam açık dosya sayısı için üst sınıra sahiptir. Linux 'daki yuvalar dosya olarak uygulanır, bu nedenle bu sayı toplam bağlantı sayısını da sınırlar. Aşağıdaki komutu çalıştırın.

```bash
ulimit -a
```

#### <a name="solution"></a>Çözüm:
"Nofile" olarak tanımlanan en fazla izin verilen açık dosya sayısı en az 10.000 veya daha fazla olmalıdır. Daha fazla bilgi için bkz. Java SDK v4 [Performans ipuçları](performance-tips-java-sdk-v4-sql.md)Azure Cosmos DB.

### <a name="socket-or-port-availability-might-be-low"></a>Yuva veya bağlantı noktası kullanılabilirliği düşük olabilir
Azure 'da çalışırken, Java SDK 'sını kullanan istemciler Azure SNAT (PAT) bağlantı noktası tükenmesi ile aynı olabilir.

#### <a name="solution-1"></a>1. Çözüm:
Azure VM 'lerinde çalıştırıyorsanız, [SNAT bağlantı noktası Tükenme Kılavuzu](troubleshoot-java-sdk-v4-sql.md#snat)' nu izleyin.

#### <a name="solution-2"></a>2. Çözüm:
Azure App Service kullanıyorsanız, [bağlantı hataları sorun giderme kılavuzunu](../app-service/troubleshoot-intermittent-outbound-connection-errors.md#cause) izleyin ve [App Service tanılamayı kullanın](https://azure.github.io/AppService/2018/03/01/Deep-Dive-into-TCP-Connections-in-App-Service-Diagnostics.html).

#### <a name="solution-3"></a>Çözüm 3:
Azure Işlevleri üzerinde çalıştırıyorsanız, tüm ilgili hizmetler (Azure Cosmos DB dahil) için tek veya statik istemcileri sürdürmek için [Azure işlevleri önerisi](../azure-functions/manage-connections.md#static-clients) ' nin takip ettiğini doğrulayın. İşlev Uygulaması barındırmanın türüne ve boyutuna göre [hizmet sınırlarını](../azure-functions/functions-scale.md#service-limits) denetleyin.

#### <a name="solution-4"></a>Çözüm 4:
Bir HTTP proxy kullanıyorsanız, SDK 'da yapılandırılan bağlantı sayısını destekleyediğinden emin olun `GatewayConnectionConfig` . Aksi takdirde, bağlantı sorunlarıyla karşılaşırsınız.

### <a name="create-multiple-client-instances"></a>Birden çok istemci örneği oluşturma
Birden çok istemci örneği oluşturmak bağlantı çakışması ve zaman aşımı sorunlarına yol açabilir.

#### <a name="solution-1"></a>1. Çözüm:
[Performans ipuçlarını](performance-tips-java-sdk-v4-sql.md#sdk-usage)izleyin ve bir uygulamanın tamamında tek bir CosmosClient örneği kullanın.

#### <a name="solution-2"></a>2. Çözüm:
Tek bir uygulamada olması mümkün değilse, CosmosClient içinde bu API aracılığıyla birden çok Cosmos Istemcisi genelinde bağlantı paylaşımını kullanmanızı öneririz `connectionSharingAcrossClientsEnabled(true)` . Birden çok Cosmos hesabına etkileşim kuran aynı JVM 'de birden çok Cosmos Istemcisinin örneği varsa, bunu etkinleştirmek Cosmos Istemcisinin örnekleri arasında mümkünse doğrudan modda bağlantı paylaşımına izin verir. Bu seçeneği ayarlarken, ilk örneklenmiş istemcinin bağlantı yapılandırması (örn. yuva zaman aşımı yapılandırması, boşta kalma zaman aşımı yapılandırması) diğer tüm istemci örnekleri için kullanılacak şekilde aklınızda olun.

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
* Java v4 SDK Azure Cosmos DB kullandığınızda sorunları [tanılayın ve sorun giderin](troubleshoot-java-sdk-v4-sql.md) .
* [Java v4](performance-tips-java-sdk-v4-sql.md)için performans yönergeleri hakkında bilgi edinin.
