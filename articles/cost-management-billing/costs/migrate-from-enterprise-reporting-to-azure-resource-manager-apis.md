---
title: Kurumsal Raporlamadan Azure Resource Manager API’lerine geçiş
description: Bu makale Raporlama API'leri ile Azure Resource Manager API'leri arasındaki farkları, Azure Resource Manager API'lerine geçtiğinizde neler bekleyebileceğinizi ve yeni Azure Resource Manager API'leriyle sağlanan yeni özellikleri anlamanıza yardımcı olur.
author: bandersmsft
ms.reviewer: adwise
ms.service: cost-management-billing
ms.subservice: common
ms.topic: reference
ms.date: 03/10/2021
ms.author: banders
ms.openlocfilehash: 24fbf52c8fd0338537862a54e15e8a249541a701
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102635799"
---
# <a name="migrate-from-enterprise-reporting-to-azure-resource-manager-apis"></a>Kurumsal Raporlamadan Azure Resource Manager API’lerine geçiş

Bu makale Maliyet Yönetimi için Azure Resource Manager API'lerine geçmek üzere [Kurumsal Müşteriler için Azure Raporlama API'lerini](../manage/enterprise-api.md) kullanarak özel çözümler oluşturan geliştiricilere yardımcı olur. Daha yeni Azure Resource Manager API'leri için Hizmet Sorumlusu desteği şimdi genel kullanıma sunuldu. Azure Resource Manager API'leri etkin geliştirme aşamasındadır. Eski Kurumsal Müşteriler Azure Raporlama API'lerini kullanmak yerine yeni API'lere geçmeyi düşünün. Eski API'ler kullanım dışı bırakılma aşamasındadır. Bu makale Raporlama API'leri ile Azure Resource Manager API'leri arasındaki farkları, Azure Resource Manager API'lerine geçtiğinizde neler bekleyebileceğinizi ve yeni Azure Resource Manager API'leriyle sağlanan yeni özellikleri anlamanıza yardımcı olur.

## <a name="api-differences"></a>API farklılıkları

Aşağıdaki bilgilerde eski Kurumsal Müşteriler için Raporlama API'leri ile yeni Azure Resource Manager API'leri arasındaki farklar açıklanır.

| **Kullanım** | **Kurumsal Anlaşma API'leri** | **Azure Resource Manager API'leri** |
| --- | --- | --- |
| Kimlik doğrulama | Kurumsal Anlaşma (EA) portalında sağlanan API Anahtarı | Kullanıcı belirteçlerini veya Hizmet Sorumlularını kullanarak Azure Active Directory (Azure AD) Kimlik Doğrulaması. Hizmet Sorumluları, API Anahtarlarının yerini alır. |
| Kapsamlar ve İzinler | Tüm istekler Kayıt kapsamındadır. API Anahtarı izin atamaları, tüm Kaydın, Departmanın veya belirli bir Hesabın verilerinin döndürüleceğini belirler. Kullanıcı kimlik doğrulaması yoktur. | Kullanıcılara veya Hizmet Sorumlularına Kayıt, Departman veya Hesap kapsamında erişim atanır. |
| URI Uç Noktası | https://consumption.azure.com | https://management.azure.com |
| Geliştirme Durumu | Bakım modunda. Kullanım dışı bırakılma aşamasında. | Etkin bir şekilde geliştiriliyor |
| Kullanılabilir API’ler | Şu anda kullanıma sunulanla sınırlı | Her EA API'sinin yerini alacak eşdeğer API'ler vardır. <p> Ayrıca aşağıdaki gibi ek [Maliyet Yönetimi API'leri](/rest/api/cost-management/) de sağlanır: <p> <ul><li>Bütçeler</li><li>Uyarılar<li>Dışarı Aktarmalar</li></ul> |

## <a name="migration-checklist"></a>Geçiş denetim listesi

- [Azure Resource Manager REST API'leri](/rest/api/azure) hakkında bilgi edinin.
- [Yeni Azure Resource Manager API'lerine EA API eşlemesi](#ea-api-mapping-to-new-azure-resource-manager-apis) sayfasında hangi EA API'lerini kullandığınızı saptayın ve hangi Azure Resource Manager API'lerine geçileceğine bakın.
- Azure Resource Manager API'leri için hizmet yetkilendirmesini ve kimlik doğrulamasını yapılandırın
  - Daha önce Azure Resource Manager API'lerini kullanmadıysanız [istemci uygulamanızı Azure AD'ye kaydedin](/rest/api/azure/#register-your-client-application-with-azure-ad). Kayıt sizin için API'leri çağırmak üzere bir hizmet sorumlusu oluşturur.
  - Aşağıda açıklandığı gibi gerekli kapsamlara hizmet sorumlusu erişimi atayın.
  - Programlarınızın kodunu Hizmet Sorumlunuz için [Azure AD kimlik doğrulamasını](/rest/api/azure/#create-the-request) kullanacak şekilde güncelleştirin.
- API'leri test edin ve ardından programlarınızın kodunu güncelleştirip EA API çağrılarını Azure Resource Manager API çağrılarıyla değiştirin.
- Hata işleme süreçlerini yeni hata kodlarını kullanacak şekilde güncelleştirin. Dikkate alınması gereken bazı noktalar şunlardır:
  - Azure Resource Manager API'lerinin 60 saniyelik bir zaman aşımı süresi vardır.
  - Azure Resource Manager API'lerinin hız sınırlaması vardır. Hızlar aşılırsa sonuçta 429 bant genişliği azaltma hatasıyla karşılaşılır. Çözümlerinizi oluştururken kısa bir süreye çok fazla API çağrısı ayarlamamaya dikkat edin.
- Azure Resource Manager aracılığıyla sağlanan diğer Maliyet Yönetimi API'lerini gözden geçirin ve daha sonra kullanmak üzere değerlendirin. Daha fazla bilgi için bkz. [Ek Maliyet Yönetimi API'lerini kullanma](#use-additional-cost-management-apis).

## <a name="assign-service-principal-access-to-azure-resource-manager-apis"></a>Azure Resource Manager API'lerine Hizmet Sorumlusu erişimi atama

Azure Resource Manager API'lerini program aracılığıyla çağırmak için Hizmet Sorumlunuzu oluşturduktan sonra, Azure Resource Manager üzerinde yetkilendirmek ve istekler yürütmesini sağlamak için Hizmet Sorumlusuna düzgün izinleri atamanız gerekir. Farklı senaryolar için iki izin çerçevesi vardır.

### <a name="azure-billing-hierarchy-access"></a>Azure Faturalama Hiyerarşisi Erişimi

Kurumsal faturalandırma hesabınız, departmanlar veya kayıt hesabı kapsamlarınızla hizmet sorumlusu izinleri atamak için bkz. [Azure Kurumsal Anlaşma hizmet sorumlusu adlarına rol atama](../manage/assign-roles-azure-service-principals.md).

### <a name="azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi

Yeni Hizmet Sorumlusu desteği yönetim grupları, abonelikler ve kaynak grupları gibi Azure'a özgü kapsamları da içerecek şekilde genişletilmiştir. Söz konusu kapsamlara Hizmet Sorumlusu izinlerini doğrudan [Azure portalda](../../active-directory/develop/howto-create-service-principal-portal.md#assign-a-role-to-the-application) veya [Azure PowerShell](../../active-directory/develop/howto-authenticate-service-principal-powershell.md#assign-the-application-to-a-role) kullanarak atayabilirsiniz.

## <a name="ea-api-mapping-to-new-azure-resource-manager-apis"></a>Yeni Azure Resource Manager API'lerine EA API eşlemesi

Aşağıdaki tablodan yararlanarak şu anda kullanmakta olduğunuz EA API'lerini ve bunlar yerine kullanılacak Azure Resource Manager API'lerini belirleyin.

| **Senaryo** | **EA API'leri** | **Azure Resource Manager API'leri** |
| --- | --- | --- |
| Bakiye Özeti | [/balancesummary](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) |[Microsoft.Consumption/balances](/rest/api/consumption/balances/getbybillingaccount) |
| Fiyat Listesi | [/pricesheet](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) | [Microsoft.Consumption/pricesheets/default](/rest/api/consumption/pricesheet) – üzerinde anlaşılan fiyatlar için kullanın <p> [Perakende Fiyatları API'si](/rest/api/cost-management/retail-prices/azure-retail-prices) – perakende fiyatları için kullanın |
| Ayrılmış Örnek Ayrıntıları | [/reservationdetails](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) | [Microsoft.CostManagement/generateReservationDetailsReport](/rest/api/cost-management/generatereservationdetailsreport) |
| Ayrılmış Örnek Özeti | [/reservationsummaries](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) | [Microsoft.Consumption/reservationSummaries](/rest/api/consumption/reservationssummaries/list#reservationsummariesdailywithbillingaccountid) |
| Ayrılmış Örnek Önerileri | [/SharedReservationRecommendations](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation)<p>[/SingleReservationRecommendations](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) | [Microsoft.Consumption/reservationRecommendations](/rest/api/consumption/reservationrecommendations/list) |
| Ayrılmış Örnek Ücretleri | [/reservationcharges](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges) | [Microsoft.Consumption/reservationTransactions](/rest/api/consumption/reservationtransactions/list) |

## <a name="migration-details-by-api"></a>API'ye göre geçiş ayrıntıları

Aşağıdaki bölümlerde eski API isteği örnekleriyle bunların yerini alacak API örnekleri gösterilir.

### <a name="balance-summary-api"></a>Bakiye Özeti API'si

Yeni Bakiye Özeti API'sini çağırırken aşağıdaki istek URI'lerini kullanın. billingAccountId olarak kayıt numaranız kullanılmalıdır.

#### <a name="supported-requests"></a>Desteklenen istekler

[Kayıt için Get komutu](/rest/api/consumption/balances/getbybillingaccount)

```json
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.Consumption/balances?api-version=2019-10-01
```

### <a name="response-body-changes"></a>Yanıt gövdesi değişiklikleri

_Eski yanıt gövdesi_:

```json
{
        "id": "enrollments/100/billingperiods/201507/balancesummaries",
          "billingPeriodId": 201507,
          "currencyCode": "USD",
          "beginningBalance": 0,
          "endingBalance": 1.1,
          "newPurchases": 1,
          "adjustments": 1.1,
          "utilized": 1.1,
          "serviceOverage": 1,
          "chargesBilledSeparately": 1,
          "totalOverage": 1,
          "totalUsage": 1.1,
          "azureMarketplaceServiceCharges": 1,
          "newPurchasesDetails": [
            {
              "name": "",
              "value": 1
            }
          ],
          "adjustmentDetails": [
            {
              "name": "Promo Credit",
              "value": 1.1
            },
            {
              "name": "SIE Credit",
              "value": 1.0
            }
          ]
    }

```

_Yeni yanıt gövdesi_:

Aynı veriler şimdi yeni API yanıtının `properties` alanında sağlanır. Bazı alan adlarında küçük yazım değişiklikleri olabilir.

```json
{
  "id": "/providers/Microsoft.Billing/billingAccounts/123456/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/balances/balanceId1",
  "name": "balanceId1",
  "type": "Microsoft.Consumption/balances",
  "properties": {
    "currency": "USD  ",
    "beginningBalance": 3396469.19,
    "endingBalance": 2922371.02,
    "newPurchases": 0,
    "adjustments": 0,
    "utilized": 474098.17,
    "serviceOverage": 0,
    "chargesBilledSeparately": 0,
    "totalOverage": 0,
    "totalUsage": 474098.17,
    "azureMarketplaceServiceCharges": 609.82,
    "billingFrequency": "Month",
    "priceHidden": false,
    "newPurchasesDetails": [
      {
        "name": "Promo Purchase",
        "value": 1
      }
    ],
    "adjustmentDetails": [
      {
        "name": "Promo Credit",
        "value": 1.1
      },
      {
        "name": "SIE Credit",
        "value": 1
      }
    ]
  }
}

```

### <a name="price-sheet"></a>Fiyat Listesi

Fiyat Listesi API'sini çağırırken aşağıdaki istek URI'lerini kullanın.

#### <a name="supported-requests"></a>Desteklenen istekler

 API'yi çağırmak için aşağıdaki kapsamları kullanabilirsiniz:

- Kayıt: `providers/Microsoft.Billing/billingAccounts/{billingAccountId}`
- Abonelik: `subscriptions/{subscriptionId}`

[_Geçerli Faturalama Dönemi için Get komutu_](/rest/api/consumption/pricesheet/get)


```json
https://management.azure.com/{scope}/providers/Microsoft.Consumption/pricesheets/default?api-version=2019-10-01
```

[_Belirtilen Faturalama Dönemi için Get komutu_](/rest/api/consumption/pricesheet/getbybillingperiod)

```json
https://management.azure.com/{scope}/providers/Microsoft.Billing/billingPeriods/{billingPeriodName}/providers/Microsoft.Consumption/pricesheets/default?api-version=2019-10-01
```

#### <a name="response-body-changes"></a>Yanıt gövdesi değişiklikleri

_Eski yanıt_:

```json
      [
        {
              "id": "enrollments/57354989/billingperiods/201601/products/343/pricesheets",
              "billingPeriodId": "201704",
            "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
              "meterName": "A1 VM",
              "unitOfMeasure": "100 Hours",
              "includedQuantity": 0,
              "partNumber": "N7H-00015",
              "unitPrice": 0.00,
              "currencyCode": "USD"
        },
        {
              "id": "enrollments/57354989/billingperiods/201601/products/2884/pricesheets",
              "billingPeriodId": "201404",
            "meterId": "dc210ecb-97e8-4522-8134-5385494233c0",
              "meterName": "Locally Redundant Storage Premium Storage - Snapshots - AU East",
              "unitOfMeasure": "100 GB",
              "includedQuantity": 0,
              "partNumber": "N9H-00402",
              "unitPrice": 0.00,
              "currencyCode": "USD"
        },
        ...
    ]

```

_Yeni yanıt_:

Eski veriler şimdi yeni API yanıtının `pricesheets` alanındadır. Ölçüm ayrıntılarına ilişkin bilgiler de sağlanır.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/pricesheets/default",
  "name": "default",
  "type": "Microsoft.Consumption/pricesheets",
  "properties": {
    "nextLink": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/microsoft.consumption/pricesheets/default?api-version=2018-01-31&$skiptoken=AQAAAA%3D%3D&$expand=properties/pricesheets/meterDetails",
    "pricesheets": [
      {
        "billingPeriodId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702",
        "meterId": "00000000-0000-0000-0000-000000000000",
        "unitOfMeasure": "100 Hours",
        "includedQuantity": 100,
        "partNumber": "XX-11110",
        "unitPrice": 0.00000,
        "currencyCode": "EUR",
        "offerId": "OfferId 1",
        "meterDetails": {
          "meterName": "Data Transfer Out (GB)",
          "meterCategory": "Networking",
          "unit": "GB",
          "meterLocation": "Zone 2",
          "totalIncludedQuantity": 0,
          "pretaxStandardRate": 0.000
        }
      }
    ]
  }
}

```

### <a name="reserved-instance-usage-details"></a>Ayrılmış örnek kullanımı ayrıntıları

Microsoft şu zaman uyumluluk temelinde Rezervasyon Ayrıntıları API'leri üzerinde çalışma yapmamaktadır. Geçiş kapsamında daha yeni SPN destekli zaman uyumsuz API çağrısı desenine geçmenizi öneririz. Zaman uyumsuz istekler büyük miktarlardaki verileri daha iyi işler ve zaman aşımı hatalarını azaltır.

#### <a name="supported-requests"></a>Desteklenen istekler

Yeni Zaman Uyumsuz Rezervasyon Ayrıntıları API'lerini çağırırken aşağıdaki istek URI'lerini kullanın. `billingAccountId` olarak kayıt numaranız kullanılmalıdır. API'yi aşağıdaki kapsamlarla çağırabilirsiniz:

- Kayıt: `providers/Microsoft.Billing/billingAccounts/{billingAccountId}`

#### <a name="sample-request-to-generate-a-reservation-details-report"></a>Rezervasyon ayrıntıları raporu oluşturmak için istek örneği

```json
POST 
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.CostManagement/generateReservationDetailsReport?startDate={startDate}&endDate={endDate}&api-version=2019-11-01

```

#### <a name="sample-request-to-poll-report-generation-status"></a>Rapor oluşturma durumunu yoklamak için istek örneği

```json
GET
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.CostManagement/reservationDetailsOperationResults/{operationId}?api-version=2019-11-01

```

#### <a name="sample-poll-response"></a>Yoklama yanıtı örneği

```json
{
  "status": "Completed",
  "properties": {
    "reportUrl": "https://storage.blob.core.windows.net/details/20200911/00000000-0000-0000-0000-000000000000?sv=2016-05-31&sr=b&sig=jep8HT2aphfUkyERRZa5LRfd9RPzjXbzB%2F9TNiQ",
    "validUntil": "2020-09-12T02:56:55.5021869Z"
  }
}

```

#### <a name="response-body-changes"></a>Yanıt gövdesi değişiklikleri

Eski zaman uyumsuz tabanlı Rezervasyon Ayrıntıları API'sinin yanıtı aşağıda verilmiştir.

_Eski yanıt_:

```json
{
    "reservationOrderId": "00000000-0000-0000-0000-000000000000",
    "reservationId": "00000000-0000-0000-0000-000000000000",
    "usageDate": "2018-02-01T00:00:00",
    "skuName": "Standard_F2s",
    "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/resourvegroup1/providers/microsoft.compute/virtualmachines/VM1",
    "totalReservedQuantity": 18.000000000000000,
    "reservedHours": 432.000000000000000,
    "usedHours": 400.000000000000000
}

```

_Yeni yanıt_:

Yeni API sizin için bir CSV dosyası oluşturur. Aşağıdaki dosya alanlarına bakın.

| **Eski Özellik** | **Yeni Özellik** | **Notlar** |
| --- | --- | --- |
|  | InstanceFlexibilityGroup | Örnek esnekliği için yeni özellik. |
|  | InstanceFlexibilityRatio | Örnek esnekliği için yeni özellik. |
| instanceId | InstanceName |  |
|  | Tür | Bu yeni bir özelliktir. Değerleri: `None`, `Reservation` veya `IncludedQuantity`. |
| reservationId | ReservationId |  |
| reservationOrderId | ReservationOrderId |  |
| reservedHours | ReservedHours |  |
| skuName | SkuName |  |
| totalReservedQuantity | TotalReservedQuantity |  |
| usageDate | UsageDate |  |
| usedHours | UsedHours |  |

### <a name="reserved-instance-usage-summary"></a>Ayrılmış Örnek Kullanımı Özeti

Yeni Rezervasyon Özetleri API'sini çağırmak için aşağıdaki istek URI'lerini kullanın.

#### <a name="supported-requests"></a>Desteklenen istekler

 API'yi aşağıdaki kapsamlarla çağırın:

- Kayıt: `providers/Microsoft.Billing/billingAccounts/{billingAccountId}`

[_Günlük Rezervasyon Özeti için Get komutu_](/rest/api/consumption/reservationssummaries/list#reservationsummariesdailywithbillingaccountid)

```json
https://management.azure.com/{scope}/Microsoft.Consumption/reservationSummaries?grain=daily&$filter=properties/usageDate ge 2017-10-01 AND properties/usageDate le 2017-11-20&api-version=2019-10-01
```

[_Aylık Rezervasyon Özeti için Get komutu_](/rest/api/consumption/reservationssummaries/list#reservationsummariesmonthlywithbillingaccountid)

```json
https://management.azure.com/{scope}/Microsoft.Consumption/reservationSummaries?grain=daily&$filter=properties/usageDate ge 2017-10-01 AND properties/usageDate le 2017-11-20&api-version=2019-10-01
```

#### <a name="response-body-changes"></a>Yanıt gövdesi değişiklikleri

_Eski yanıt_:

```json
[
     {
        "reservationOrderId": "00000000-0000-0000-0000-000000000000",
        "reservationId": "00000000-0000-0000-0000-000000000000",
        "skuName": "Standard_F1s",
        "reservedHours": 24,
        "usageDate": "2018-05-01T00:00:00",
        "usedHours": 23,
        "minUtilizationPercentage": 0,
        "avgUtilizationPercentage": 95.83,
        "maxUtilizationPercentage": 100
    }
]

```

_Yeni yanıt_:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/12345/providers/Microsoft.Consumption/reservationSummaries/reservationSummaries_Id1",
      "name": "reservationSummaries_Id1",
      "type": "Microsoft.Consumption/reservationSummaries",
      "tags": null,
      "properties": {
        "reservationOrderId": "00000000-0000-0000-0000-000000000000",
        "reservationId": "00000000-0000-0000-0000-000000000000",
        "skuName": "Standard_B1s",
        "reservedHours": 720,
        "usageDate": "2018-09-01T00:00:00-07:00",
        "usedHours": 0,
        "minUtilizationPercentage": 0,
        "avgUtilizationPercentage": 0,
        "maxUtilizationPercentage": 0
      }
    }
  ]
}

```

### <a name="reserved-instance-recommendations"></a>Ayrılmış örnek önerileri

Yeni Rezervasyon Önerileri API'sini çağırmak için aşağıdaki istek URI'lerini kullanın.

#### <a name="supported-requests"></a>Desteklenen istekler

 API'yi aşağıdaki kapsamlarla çağırın:

- Kayıt: `providers/Microsoft.Billing/billingAccounts/{billingAccountId}`
- Abonelik: `subscriptions/{subscriptionId}`
- Kaynak Grupları: `subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}`

[_Öneriler için Get komutu_](/rest/api/consumption/reservationrecommendations/list)

Bu API aracılığıyla hem paylaşılan hem de tek kapsam önerileri sağlanır. Ayrıca kapsam üzerinde isteğe bağlı API parametresiyle filtre uygulayabilirsiniz.

```json
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/123456/providers/Microsoft.Consumption/reservationRecommendations?api-version=2019-10-01
```

#### <a name="response-body-changes"></a>Yanıt gövdesi değişiklikleri

Paylaşılan ve Tek kapsam önerileri tek API'de bir araya getirilmiştir.

_Eski yanıt_:

```json
[{
    "subscriptionId": "1111111-1111-1111-1111-111111111111",
    "lookBackPeriod": "Last7Days",
    "meterId": "2e3c2132-1398-43d2-ad45-1d77f6574933",
    "skuName": "Standard_DS1_v2",
    "term": "P1Y",
    "region": "westus",
    "costWithNoRI": 186.27634908960002,
    "recommendedQuantity": 9,
    "totalCostWithRI": 143.12931642978083,
    "netSavings": 43.147032659819189,
    "firstUsageDate": "2018-02-19T00:00:00"
}
]

```

_Yeni yanıt_:

```json
{
  "value": [
    {
      "id": "billingAccount/123456/providers/Microsoft.Consumption/reservationRecommendations/00000000-0000-0000-0000-000000000000",
      "name": "00000000-0000-0000-0000-000000000000",
      "type": "Microsoft.Consumption/reservationRecommendations",
      "location": "westus",
      "sku": "Standard_DS1_v2",
      "kind": "legacy",
      "properties": {
        "meterId": "00000000-0000-0000-0000-000000000000",
        "term": "P1Y",
        "costWithNoReservedInstances": 12.0785105,
        "recommendedQuantity": 1,
        "totalCostWithReservedInstances": 11.4899644807748,
        "netSavings": 0.588546019225182,
        "firstUsageDate": "2019-07-07T00:00:00-07:00",
        "scope": "Shared",
        "lookBackPeriod": "Last7Days",
        "instanceFlexibilityRatio": 1,
        "instanceFlexibilityGroup": "DSv2 Series",
        "normalizedSize": "Standard_DS1_v2",
        "recommendedQuantityNormalized": 1,
        "skuProperties": [
          {
            "name": "Cores",
            "value": "1"
          },
          {
            "name": "Ram",
            "value": "1"
          }
        ]
      }
    },
   ]
}

```

### <a name="reserved-instance-charges"></a>Ayrılmış örnek ücretleri

Yeni Ayrılmış Örnek Ücretleri API'sini çağırmak için aşağıdaki istek URI'lerini kullanın.

#### <a name="supported-requests"></a>Desteklenen istekler

[_Tarih Aralığına Göre Rezervasyon Ücretleri için Get komutu_](/rest/api/consumption/reservationtransactions/list)

```json
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.Consumption/reservationTransactions?$filter=properties/eventDate+ge+2020-05-20+AND+properties/eventDate+le+2020-05-30&api-version=2019-10-01
```

#### <a name="response-body-changes"></a>Yanıt gövdesi değişiklikleri

_Eski yanıt_:

```json
[
    {
        "purchasingEnrollment": "string",
        "armSkuName": "Standard_F1s",
        "term": "P1Y",
        "region": "eastus",
        "PurchasingsubscriptionGuid": "00000000-0000-0000-0000-000000000000",
        "PurchasingsubscriptionName": "string",
        "accountName": "string",
        "accountOwnerEmail": "string",
        "departmentName": "string",
        "costCenter": "",
        "currentEnrollment": "string",
        "eventDate": "string",
        "reservationOrderId": "00000000-0000-0000-0000-000000000000",
        "description": "Standard_F1s eastus 1 Year",
        "eventType": "Purchase",
        "quantity": int,
        "amount": double,
        "currency": "string",
        "reservationOrderName": "string"
    }
]

```
_Yeni yanıt_:

```json
{
  "value": [
    {
      "id": "/billingAccounts/123456/providers/Microsoft.Consumption/reservationtransactions/201909091919",
      "name": "201909091919",
      "type": "Microsoft.Consumption/reservationTransactions",
      "tags": {},
      "properties": {
        "eventDate": "2019-09-09T19:19:04Z",
        "reservationOrderId": "00000000-0000-0000-0000-000000000000",
        "description": "Standard_DS1_v2 westus 1 Year",
        "eventType": "Cancel",
        "quantity": 1,
        "amount": -21,
        "currency": "USD",
        "reservationOrderName": "Transaction-DS1_v2",
        "purchasingEnrollment": "123456",
        "armSkuName": "Standard_DS1_v2",
        "term": "P1Y",
        "region": "westus",
        "purchasingSubscriptionGuid": "11111111-1111-1111-1111-11111111111",
        "purchasingSubscriptionName": "Infrastructure Subscription",
        "accountName": "Microsoft Infrastructure",
        "accountOwnerEmail": "admin@microsoft.com",
        "departmentName": "Unassigned",
        "costCenter": "",
        "currentEnrollment": "123456",
        "billingFrequency": "recurring"
      }
    },
  ]
}

```

## <a name="use-additional-cost-management-apis"></a>Ek Maliyet Yönetimi API'lerini kullanma

Mevcut raporlama senaryolarınızda Azure Resource Manager API'leri geçtikten sonra birçok başka API'yi de kullanabilirsiniz. API'ler Azure Resource Manager aracılığıyla da kullanılabilir ve Hizmet Sorumlusu tabanlı kimlik doğrulaması kullanılarak otomatikleştirilebilir. Burada kullanabileceğiniz yeni özelliklerin kısa bir özeti verilmiştir.

- [Bütçeler](/rest/api/consumption/budgets/createorupdate): Maliyetlerinizi proaktif olarak izlemek, ilgili paydaşları uyarmak ve eşik ihlallerinde gerçekleştirilecek eylemleri otomatikleştirmek amacıyla eşikleri ayarlamak için kullanın.

- [Uyarılar](/rest/api/cost-management/alerts): Bütçe uyarıları, fatura uyarıları, kredi uyarıları ve kota uyarıları gibi daha birçok uyarı bilgisini görüntülemek için kullanın.

- [Dışarı Aktarmalar](/rest/api/cost-management/exports): Ücretlerinizin kendi seçtiğiniz bir Azure Depolama hesabına yinelenen veri dışarı aktarmalarını zamanlamak için kullanın. Bu, Azure'da büyük bir varlığı olan ve verilerini analiz edip kendi dahili sistemlerinde kullanmak isteyen müşteriler için önerilen çözümdür.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Resource Manager REST API'leri](/rest/api/azure) hakkında bilgi edinin.
- Gerekirse [Yeni Azure Resource Manager API'lerine EA API eşlemesi](#ea-api-mapping-to-new-azure-resource-manager-apis) sayfasında hangi EA API'lerini kullandığınızı saptayın ve hangi Azure Resource Manager API'lerine geçileceğine bakın.
- Daha önce Azure Resource Manager API'lerini kullanmadıysanız [istemci uygulamanızı Azure AD'ye kaydedin](/rest/api/azure/#register-your-client-application-with-azure-ad).
- Gerekirse programlarınızın kodunu Hizmet Sorumlunuz için [Azure AD kimlik doğrulamasını](/rest/api/azure/#create-the-request) kullanacak şekilde güncelleştirin.