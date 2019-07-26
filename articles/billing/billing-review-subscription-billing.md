---
title: Azure Abonelik Faturalandırma verilerini REST API inceleyin | Microsoft Docs
description: Abonelik Faturalandırma ayrıntılarını gözden geçirmek için Azure REST API 'Lerini nasıl kullanacağınızı öğrenin.
services: billing
documentationcenter: na
author: lleonard-msft
manager: ''
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: banders
ms.openlocfilehash: 8cfa429b18fb282f5c1f85d2fd1637704653b855
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443044"
---
# <a name="review-subscription-billing-using-rest-apis"></a>REST API 'Leri kullanarak abonelik faturalandırmasını gözden geçirme

Azure Raporlama API 'Leri, Azure maliyetlerinizi gözden geçirmenize ve yönetmenize yardımcı olur.

Filtreler, sonuçları gereksinimlerinize uyacak şekilde özelleştirmenize yardımcı olur.

Burada, belirli bir tarih aralığı için Abonelik Faturalandırma ayrıntılarını döndürmek üzere bir REST API kullanacağınızı öğrenirsiniz.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json
Authorization: Bearer
```

## <a name="build-the-request"></a>Derleme isteği

`{subscriptionID}` Parametresi gereklidir ve hedef aboneliği tanımlar.

Parametresi gereklidir ve geçerli bir [fatura dönemini](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods)belirtir. `{billingPeriod}`

`${startDate}` Ve`${endDate}` parametreleri bu örnek için gereklidir, ancak uç nokta için isteğe bağlıdır. Tarih aralığını, YYYY-AA-GG biçiminde dizeler olarak belirtir (örnekler: `'20180501'` ve `'20180615'`).

Aşağıdaki üstbilgiler gereklidir:

|İstek üst bilgisi|Açıklama|
|--------------------|-----------------|
|*Content-Type:*|Gerekli. Olarak `application/json`ayarlayın.|
|*Authorization:*|Gerekli. Geçerli `Bearer` bir [erişim belirtecine](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients)ayarlayın. |

## <a name="response"></a>Yanıt

Durum kodu 200 (Tamam), hesabınız için ayrıntılı maliyetlerin bir listesini içeren başarılı bir yanıt için döndürülür.

``` json
{
  "value": [
    {
      "id": "/subscriptions/{$subscriptionID}/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/{$detailsID}",
      "name": "{$detailsID}",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}",
        "invoiceId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/invoices/${invoiceID}",
        "usageStart": "${startDate}}",
        "usageEnd": "${endDate}",
        "currency": "USD",
        "usageQuantity": "${usageQuantity}",
        "billableQuantity": "${billableQuantity}",
        "pretaxCost": "${cost}",
        "meterId": "${meterID}",
        "meterDetails": "${meterDetails}"
      }
    }
  ],
  "nextLink": "${nextLinkURL}"
}
```

**Değerindeki** her öğe, bir hizmetin kullanımıyla ilgili ayrıntıları temsil eder:

|Response özelliği|Açıklama|
|----------------|----------|
|**subscriptionGuid** | Aboneliğin genel benzersiz KIMLIĞI. |
|**Başlangıç** | Kullanım başlangıç tarihi. |
|**endDate** | Kullanım bitiş tarihi. |
|**useageQuantity** | Kullanılan miktar. |
|**billableQuantity** | Miktarı fiili olarak faturalandırılır. |
|**pretaxCost** | Maliyet faturalandırılır ve ilgili vergiler. |
|**meterDetails** | Kullanım hakkında ayrıntılı bilgi. |
|**nextLink**| Ayarlandığında, Ayrıntılar için bir sonraki "sayfa" URL 'SI belirtir. Sayfa son bir olduğunda boştur. |

Bu örnek kısaltılmıştır; Her yanıt alanının tüm açıklaması için bkz. [kullanım ayrıntılarını listeleme](https://docs.microsoft.com/rest/api/consumption/usagedetails/list#usagedetailslistforbillingperiod) .

Diğer durum kodları hata koşullarını gösterir. Bu durumlarda, yanıt nesnesi isteğin neden başarısız olduğunu açıklar.

``` json
{
  "error": [
    {
      "code": "Error type.",
      "message": "Error response describing why the operation failed."
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Kurumsal raporlamaya genel bakış](https://docs.microsoft.com/azure/billing/billing-enterprise-api) konusunu gözden geçirme
- [Kurumsal faturalandırma REST API](https://docs.microsoft.com/rest/api/billing/) araştırın
- [Azure REST API kullanmaya başlama](https://docs.microsoft.com/rest/api/azure/)
