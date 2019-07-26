---
title: REST API ile Azure kurumsal kayıt faturalandırma verilerini gözden geçirin | Microsoft Docs
description: Kurumsal kayıt faturalandırma bilgilerini gözden geçirmek için Azure REST API 'Lerini nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: 25d9b48696dc2a83ea0ba77c1be2c7aad7627fff
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443142"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>REST API 'Lerini kullanarak kurumsal kayıt faturalandırmasını gözden geçirme

Azure Raporlama API 'Leri, Azure maliyetlerinizi gözden geçirmenize ve yönetmenize yardımcı olur.

Bu makalede, Azure REST API 'Lerini kullanarak faturalandırma hesapları, departmanı veya kurumsal anlaşma (EA) kayıt hesaplarıyla ilişkili faturalandırma bilgilerini almayı öğreneceksiniz. 

## <a name="individual-account-billing"></a>Bireysel hesap Faturalaması

Bir departmandaki hesapların kullanım ayrıntılarını almak için:

```http
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

`{billingAccountId}` Parametresi gereklidir ve hesabın kimliğini içermelidir.

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gerekli. Olarak `application/json`ayarlayın.|  
|*Authorization:*|Gerekli. Geçerli `Bearer` bir [API anahtarı](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based)olarak ayarlayın. |  

Bu örnek, geçerli faturalandırma döngüsünün ayrıntılarını döndüren zaman uyumlu bir çağrıyı gösterir. Performans nedenleriyle, zaman uyumlu çağrılar geçen aya ilişkin bilgileri döndürür.  Ayrıca, 36 ay boyunca verileri döndürmek için [API 'yi zaman uyumsuz](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) olarak çağırabilirsiniz.


## <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam), hesabın ayrıntılı maliyetlerinin bir listesini içeren başarılı bir yanıt için döndürülür.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/BillingAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        ...
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        ...
      }
    }
  ]
}
```  

Bu örnek kısaltılmıştır; Her yanıt alanı ve hata işleme hakkında açıklayıcı bir açıklama için bkz. [bir faturalandırma hesabı için kullanım ayrıntısı alma](/rest/api/consumption/usagedetails/list#billingaccountusagedetailslist) .

## <a name="department-billing"></a>Bölüm Faturalaması 

Bir departmandaki tüm hesaplar için toplanan kullanım ayrıntılarını alın. 

```http
GET https://management.azure.com/providers/Microsoft.Billing/departments/{departmentId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

`{departmentId}` Parametresi gereklidir ve kayıt hesabındaki departmanın kimliğini içermelidir.

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gerekli. Olarak `application/json`ayarlayın.|  
|*Authorization:*|Gerekli. Geçerli `Bearer` bir [API anahtarı](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based)olarak ayarlayın. |  

Bu örnek, geçerli faturalandırma döngüsünün ayrıntılarını döndüren zaman uyumlu bir çağrıyı gösterir. Performans nedenleriyle, zaman uyumlu çağrılar geçen aya ilişkin bilgileri döndürür.  Ayrıca, 36 ay boyunca verileri döndürmek için [API 'yi zaman uyumsuz](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) olarak çağırabilirsiniz.

### <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam), departman için belirli bir fatura dönemi ve fatura KIMLIĞI için ayrıntılı kullanım bilgilerinin ve maliyetlerinin listesini içeren başarılı bir yanıt için döndürülür.


Aşağıdaki örnek, bölümünün `1234`REST API çıkışını gösterir.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/providers/Microsoft.Billing/Departments/1234/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        ...
      }
    }
  ]
}
```  

Bu örnek kısaltılmıştır; Her yanıt alanı ve hata işleme hakkında açıklayıcı bir açıklama için bkz. [bir departman için kullanım ayrıntısı alma](/rest/api/consumption/usagedetails/list#departmentusagedetailslist) .

## <a name="enrollment-account-billing"></a>Kayıt hesabı faturalandırması

Kayıt hesabı için toplanan kullanım ayrıntılarını alın.

```http
GET GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/{enrollmentAccountId}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

`{enrollmentAccountId}` Parametresi gereklidir ve kayıt hesabının kimliğini içermelidir.

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gerekli. Olarak `application/json`ayarlayın.|  
|*Authorization:*|Gerekli. Geçerli `Bearer` bir [API anahtarı](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based)olarak ayarlayın. |  

Bu örnek, geçerli faturalandırma döngüsünün ayrıntılarını döndüren zaman uyumlu bir çağrıyı gösterir. Performans nedenleriyle, zaman uyumlu çağrılar geçen aya ilişkin bilgileri döndürür.  Ayrıca, 36 ay boyunca verileri döndürmek için [API 'yi zaman uyumsuz](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) olarak çağırabilirsiniz.

### <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam), departman için belirli bir fatura dönemi ve fatura KIMLIĞI için ayrıntılı kullanım bilgilerinin ve maliyetlerinin listesini içeren başarılı bir yanıt için döndürülür.

Aşağıdaki örnek, kurumsal kayıt `1234`için REST API çıkışını gösterir.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/providers/Microsoft.Billing/EnrollmentAccounts/1234/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        ....
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        ...
      }
    }
  ]
}
``` 

Bu örnek kısaltılmıştır; Her yanıt alanı ve hata işleme hakkında açıklayıcı bir açıklama için bkz. [bir kayıt hesabı için kullanım ayrıntısı alma](/rest/api/consumption/usagedetails/list#enrollmentaccountusagedetailslist) .

## <a name="next-steps"></a>Sonraki adımlar 
- [Kurumsal raporlamaya genel bakış](https://docs.microsoft.com/azure/billing/billing-enterprise-api) konusunu gözden geçirme
- [Kurumsal faturalandırma REST API](https://docs.microsoft.com/rest/api/billing/) araştırın   
- [Azure REST API kullanmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
