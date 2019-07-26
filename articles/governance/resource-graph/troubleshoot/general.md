---
title: Sık karşılaşılan hataları giderme
description: Azure Kaynak Grafmasıyla Azure kaynaklarını sorgulama sorunlarını giderme hakkında bilgi edinin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/24/2019
ms.topic: troubleshooting
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 511d170f90e8ed34b00a3960d084223ec73d99dd
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480559"
---
# <a name="troubleshoot-errors-using-azure-resource-graph"></a>Azure Kaynak Grafiği 'ni kullanarak hatalarda sorun giderme

Azure Kaynak Grafiği ile Azure kaynaklarını sorgularken hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hatalar ve bunların nasıl çözümleneceği açıklanmaktadır.

## <a name="finding-error-details"></a>Hata ayrıntılarını bulma

Birçok hata, Azure Kaynak Graf ile bir sorgu çalıştırılırken bir sorunun sonucudur. Sorgu başarısız olduğunda, SDK başarısız sorguyla ilgili ayrıntıları sağlar. Bu bilgiler sorunu, düzeltilmesi ve sonraki bir sorgunun başarılı olması için gösterir.

## <a name="general-errors"></a>Genel hatalar

### <a name="toomanysubscription"></a>Senaryon Çok fazla abonelik

#### <a name="issue"></a>Sorun

[Azure ışıklı](../../../lighthouse/overview.md)abonelikler de dahil olmak üzere 1000 ' den fazla aboneliğe erişimi olan müşteriler, Azure Resource Graph 'a yönelik tek bir çağrıda verileri tüm aboneliklerde getiremez.

#### <a name="cause"></a>Nedeni

Azure CLı ve PowerShell yalnızca ilk 1000 aboneliğini Azure Kaynak grafiğine ilet. Azure Kaynak Grafiği REST API, üzerinde sorgu gerçekleştirmek için en fazla abonelik sayısını kabul eder.

#### <a name="resolution"></a>Çözüm

Aboneliklerin bir alt kümesiyle 1000 abonelik sınırının altında kalmak için toplu istek istekleri. Çözüm, PowerShell 'deki **abonelik** parametresini kullanıyor.

```azurepowershell-interactive
# Replace this query with your own
$query = 'project type'

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

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
- [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.