---
title: Sık karşılaşılan hataları giderme
description: Azure kaynaklarının Azure Kaynak Grafiği ile sorgulanması sırasında çeşitli SDK'larla ilgili sorunları nasıl gidereceklerini öğrenin.
ms.date: 10/18/2019
ms.topic: troubleshooting
ms.openlocfilehash: f881db4f75bcee8c13221717596442ac29a4b1ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74303906"
---
# <a name="troubleshoot-errors-using-azure-resource-graph"></a>Azure Kaynak Grafiği'ni kullanarak hataları giderme

Azure kaynaklarınızı Azure Kaynak Grafiği ile sorgularken hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hatalar ve bunları nasıl çözeceğiniaçıklanır.

## <a name="finding-error-details"></a>Hata ayrıntılarını bulma

Hataların çoğu, Azure Kaynak Grafiği ile sorgu çalıştırırken bir sorunun sonucudur. Bir sorgu başarısız olduğunda, SDK başarısız sorgu hakkında ayrıntılı bilgi sağlar. Bu bilgiler, sorunu giderilebilmek ve daha sonraki bir sorgunun başarılı olması için gösterir.

## <a name="general-errors"></a>Genel hatalar

### <a name="scenario-too-many-subscriptions"></a><a name="toomanysubscription"></a>Senaryo: Çok fazla abonelik

#### <a name="issue"></a>Sorun

[Azure Deniz Feneri](../../../lighthouse/overview.md)ile kiracılar arası abonelikler de dahil olmak üzere 1000'den fazla aboneye erişimi olan müşteriler, Azure Kaynak Grafiği'ni tek bir çağrıda tüm abonelikler üzerinden veri alamaz.

#### <a name="cause"></a>Nedeni

Azure CLI ve PowerShell, Azure Kaynak Grafiği'ne yalnızca ilk 1000 aboneliği iletin. Azure Kaynak Grafiği için REST API'si, sorguyu gerçekleştirmek için en fazla sayıda abonelik kabul eder.

#### <a name="resolution"></a>Çözüm

1000 abonelik sınırının altında kalmak için abonelik alt kümesine sahip sorgu için toplu iş istekleri. Çözüm PowerShell'deki **Abonelik** parametresini kullanmaktır.

```azurepowershell-interactive
# Replace this query with your own
$query = 'Resources | project type'

# Fetch the full array of subscription IDs
$subscriptions = Get-AzSubscription
$subscriptionIds = $subscriptions.Id

# Create a counter, set the batch size, and prepare a variable for the results
$counter = [PSCustomObject] @{ Value = 0 }
$batchSize = 1000
$response = @()

# Group the subscriptions into batches
$subscriptionsBatch = $subscriptionIds | Group -Property { [math]::Floor($counter.Value++ / $batchSize) }

# Run the query for each batch
foreach ($batch in $subscriptionsBatch){ $response += Search-AzGraph -Query $query -Subscription $batch.Group }

# View the completed results of the query on all subscriptions
$response
```

### <a name="scenario-unsupported-content-type-rest-header"></a><a name="rest-contenttype"></a>Senaryo: Desteklenmeyen İçerik Türü REST üstbilgisi

#### <a name="issue"></a>Sorun

Azure Kaynak Grafiği REST API'sini sorgulayan _müşteriler, 500_ (Dahili Sunucu Hatası) yanıtını iade alır.

#### <a name="cause"></a>Nedeni

Azure Kaynak Grafiği REST API `Content-Type` yalnızca bir **uygulama/json'u**destekler. Rest API tarafından desteklenmeyen **metin/düz**varsayılan bazı REST araçları veya aracıları.

#### <a name="resolution"></a>Çözüm

Azure Kaynak Grafiği'ni sorgulamak için kullandığınız aracın veya aracının, `Content-Type` **UYGULAMA/json**için yapılandırılan REST API üstbilgisini olduğunu doğrulayın.

### <a name="scenario-no-read-permission-to-all-subscriptions-in-list"></a><a name="rest-403"></a>Senaryo: Listedeki tüm abonelikler için okuma izni yok

#### <a name="issue"></a>Sorun

Azure Kaynak Grafiği sorgusuyla abonelik listesini açıkça geçen müşteriler _403_ (Yasak) yanıt alır.

#### <a name="cause"></a>Nedeni

Müşterinin sağlanan tüm abonelikler için okuma izni yoksa, uygun güvenlik hakları olmadığı için istek reddedilir.

#### <a name="resolution"></a>Çözüm

Sorguyu çalıştıran müşterinin en az erişim e-erişimini okuduğu abonelik listesine en az bir abonelik ekleyin. Daha fazla bilgi için [Azure Kaynak Grafiği'ndeki İzinler'e](../overview.md#permissions-in-azure-resource-graph)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
- [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.