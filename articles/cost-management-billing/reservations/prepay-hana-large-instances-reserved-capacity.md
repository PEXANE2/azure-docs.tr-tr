---
title: Azure rezervasyonuyla SAP HANA Büyük Örnekleri’nden tasarruf etme
description: HANA Büyük Örnekleri rezervasyonu satın almadan önce bilmeniz gereken şeyleri ve satın almanın nasıl yapılacağını öğrenin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: cefb6b2cb2908b9c7311df745417686b52241bab
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286572"
---
# <a name="save-on-sap-hana-large-instances-with-an-azure-reservation"></a>Azure rezervasyonuyla SAP HANA Büyük Örnekleri’nden tasarruf etme

Azure rezervasyonlarını önceden bir veya üç yıllığına satın aldığınızda SAP HANA Büyük Örnekleri (HLI) maliyetlerinde tasarruf edebilirsiniz. Rezervasyon indirimi, satın alınan ayrılmış örnekle eşleşen ve sağlanan HLI SKU'suna uygulanır. Bu makale, rezervasyon satın almadan önce bilmeniz gereken şeyleri ve satın almanın nasıl yapılacağını öğrenmenize yardımcı olur.

Rezervasyon satın alarak bir veya üç yıllığına HLI kullanımını taahhüt etmiş olursunuz. HLI ayrılmış kapasitesi satın almak, SKU ile birlikte gelen işlem ve NFS depolamasını kapsar. Rezervasyona işletim sistemi ya da SAP gibi yazılım lisanslama maliyetleri veya ek depolama maliyetleri dahil değildir. Rezervasyon indirimi, sağlanan SAP HLI'ye otomatik olarak uygulanır. Reservasyon süresi sona erdiğinde sağlanan kaynağınıza kullandıkça öde fiyatları uygulanır.

## <a name="purchase-considerations"></a>Satın alma konusunda dikkat edilmesi gerekenler

HLI SKU'su, ayrılmış kapasite satın alma işlemine geçmeden önce sağlanmalıdır. Rezervasyon ödemesi peşin olarak veya aylık ödemelerle yapılmalıdır. HLI ayrılmış kapasitesine aşağıdaki kısıtlamalar uygulanır:

- Rezervasyon indirimleri yalnızca Kurumsal Anlaşma ve Microsoft Müşteri Sözleşmesi aboneliklerine uygulanır. Diğer abonelikler desteklenmez.
- HLI ayrılmış kapasitesi için örnek boyutu esnekliği desteklenmez. Rezervasyon yalnızca SKU'ya ve bunu satın aldığınız bölgeye uygulanır.
- Self servis iptal ve değiştirme desteklenmez.
- Ayrılmış kapasite kapsamı tek bir kapsamdır; dolayısıyla tek aboneliğe ve kaynak grubuna uygulanır. Satın alınan kapasite başka bir abonelik tarafından kullanılmak üzere güncelleştirilemez.
- HANA ayrılmış kapasitesi için paylaşılan bir rezervasyon kapsamınız olamaz. Rezervasyon kapsamını bölemez, birleştiremez veya güncelleştiremezsiniz.
- Ayrılmış kapasite API çağrılarını kullanarak bir kerede tek bir HLI satın alabilirsiniz. Ek miktarlar satın almak için ek API çağrıları yapın.

Azure portalında veya [REST API](https://docs.microsoft.com/rest/api/reserved-vm-instances/reservationorder/purchase)'yi kullanarak ayrılmış kapasite satın alabilirsiniz.

## <a name="buy-a-hana-large-instance-reservation"></a>HANA Büyük Örnekleri rezervasyonu satın alma

Aşağıdaki bilgileri kullanarak [Rezervasyon Siparişi REST API'leriyle](https://docs.microsoft.com/rest/api/reserved-vm-instances/reservationorder/purchase) bir HLI rezervasyonu satın alabilirsiniz.

### <a name="get-the-reservation-order-and-price"></a>Rezervasyon siparişini ve fiyatını alma

İlk olarak, [Hesaplanan Fiyat](https://docs.microsoft.com/rest/api/reserved-vm-instances/reservationorder/calculate) API'sini kullanarak sağlanan HANA Büyük Örnekleri SKU'su için rezervasyon siparişini ve fiyatını alın.

Aşağıdaki örnekte PowerShell ile REST API çağrıları yapmak için [armclient](https://github.com/projectkudu/ARMClient) kullanılır. Rezervasyon siparişi ile Hesaplanan Fiyat API'si isteğinin ve istek gövdesinin nasıl görünmesi gerektiği aşağıda gösterilmiştir:

```azurepowershell-interactive
armclient post /providers/Microsoft.Capacity/calculatePrice?api-version=2018-06-01 "{
    'sku': {
        'name': 'SAP_HANA_On_Azure_S224om'
    },
    'location': 'eastus',
    'properties': {
        'reservedResourceType': 'SapHana',
        'billingScopeId': '/subscriptions/11111111-1111-1111-111111111111',
        'term': 'P1Y',
        'quantity': '1',
        'displayName': 'testreservation_S224om',
        'appliedScopes': ['/subscriptions/11111111-1111-1111-111111111111'],
        'appliedScopeType': 'Single',
       'instanceFlexibility': 'NotSupported'
    }
}"
```

Veri alanları ve bu alanların açıklamaları hakkında daha fazla bilgi için bkz. [HLI rezervasyon alanları](#hli-reservation-fields).

Aşağıdaki örnek yanıt, size döndürülecek olan yanıta benzer. `quoteId` için döndürülen değere dikkat edin.

```
{
  "properties": {
    "currencyCode": "USD",
    "netTotal": 313219.0,
    "taxTotal": 0.0,
    "isTaxIncluded": false,
    "grandTotal": 313219.0,
    "purchaseRequest": {
      "sku": {
        "name": "SAP_HANA_On_Azure_S224om"
      },
      "location": "eastus",
      "properties": {
        "billingScopeId": "/subscriptions/11111111-1111-1111-111111111111",
        "term": "P1Y",
        "billingPlan": "Upfront",
        "quantity": 1,
        "displayName": "testreservation_S224om",
        "appliedScopes": [
          "/subscriptions/11111111-1111-1111-111111111111"
        ],
        "appliedScopeType": "Single",
        "reservedResourceType": "SapHana",
        "instanceFlexibility": "NotSupported"
      }
    },
    "quoteId": "d0fd3a890795",
    "isBillingPartnerManaged": true,
    "reservationOrderId": "22222222-2222-2222-2222-222222222222",
    "skuTitle": "SAP HANA on Azure Large Instances - S224om - US East",
    "skuDescription": "SAP HANA on Azure Large Instances, S224om",
    "pricingCurrencyTotal": {
      "currencyCode": "USD",
      "amount": 313219.0
    }
  }
}
```

### <a name="make-your-purchase"></a>Satın alma işleminizi yapma

Döndürülen `quoteId` değerini ve önceki [Rezervasyon siparişini ve fiyatını alma](#get-the-reservation-order-and-price) bölümünde aldığınız `reservationOrderId` değerini kullanarak satın alma işleminizi yapın.

İstek örneği:

```azurepowershell-interactive
armclient put /providers/Microsoft.Capacity/reservationOrders/22222222-2222-2222-2222-222222222222?api-version=2018-06-01 "{
    'sku': {
        'name': 'SAP_HANA_On_Azure_S224om'
    },
    'location': 'eastus',
    'properties': {
        'reservedResourceType': 'SapHana',
        'billingScopeId': '/subscriptions/11111111-1111-1111-111111111111',
        'term': 'P1Y',
        'quantity': '1',
        'displayName': ' testreservation_S224om',
        'appliedScopes': ['/subscriptions/11111111-1111-1111-111111111111/resourcegroups/123'],
        'appliedScopeType': 'Single',
       'instanceFlexibility': 'NotSupported',
       'renew': true,
       'quoteId': 'd0fd3a890795'
    }
}"
```

Yanıt örneği. Sipariş başarıyla verilirse `provisioningState` değeri `creating` olmalıdır.

```
{
  "id": "/providers/microsoft.capacity/reservationOrders/22222222-2222-2222-2222-222222222222",
  "type": "Microsoft.Capacity/reservationOrders",
  "name": "22222222-2222-2222-2222-222222222222",
  "etag": 1,
  "properties": {
    "displayName": "testreservation_S224om",
    "requestDateTime": "2020-07-14T05:42:34.3528353Z",
    "term": "P1Y",
    "provisioningState": "Creating",
    "reservations": [
      {
        "sku": {
          "name": "SAP_HANA_On_Azure_S224om"
        },
        "id": "/providers/microsoft.capacity/reservationOrders22222222-2222-2222-2222-222222222222/reservations/33333333-3333-3333-3333-3333333333333",
        "type": "Microsoft.Capacity/reservationOrders/reservations",
        "name": "22222222-2222-2222-2222-222222222222/33333333-3333-3333-3333-3333333333333",
        "etag": 1,
        "location": "eastus”
        "properties": {
          "appliedScopes": [
            "/subscriptions/11111111-1111-1111-111111111111/resourcegroups/123"
          ],
          "appliedScopeType": "Single",
          "quantity": 1,
          "provisioningState": "Creating",
          "displayName": " testreservation_S224om",
          "effectiveDateTime": "2020-07-14T05:42:34.3528353Z",
          "lastUpdatedDateTime": "2020-07-14T05:42:34.3528353Z",
          "reservedResourceType": "SapHana",
          "instanceFlexibility": "NotSupported",
          "skuDescription": "SAP HANA on Azure Large Instances – S224om - US East",
          "renew": true
        }
      }
    ],
    "originalQuantity": 1,
    "billingPlan": "Upfront"
  }
}
```

### <a name="verify-purchase-status-success"></a>Satın alma durumunun başarısını doğrulama

Satın alma siparişinin durumunu görmek için Rezervasyon siparişi GET isteğini çalıştırın. `provisioningState` değeri `Succeeded` olmalıdır.

```azurepowershell-interactive
armclient get /providers/microsoft.capacity/reservationOrders/22222222-2222-2222-2222-222222222222?api-version=2018-06-01
```

Yanıt, aşağıdaki örneğe benzemelidir.

```
{
  "id": "/providers/microsoft.capacity/reservationOrders/44444444-4444-4444-4444-444444444444",
  "type": "Microsoft.Capacity/reservationOrders",
  "name": "22222222-2222-2222-2222-222222222222 ",
  "etag": 8,
  "properties": {
    "displayName": "testreservation_S224om",
    "requestDateTime": "2020-07-14T05:42:34.3528353Z",
    "createdDateTime": "2020-07-14T05:44:47.157579Z",
    "expiryDate": "2021-07-14",
    "term": "P1Y",
    "provisioningState": "Succeeded",
    "reservations": [
      {
        "id": "/providers/microsoft.capacity/reservationOrders/22222222-2222-2222-2222-222222222222/reservations/33333333-3333-3333-3333-3333333333333"
      }
    ],
    "originalQuantity": 1,
    "billingPlan": "Upfront"
  }
}
```

## <a name="hli-reservation-fields"></a>HLI rezervasyon alanları

Aşağıdaki bilgilerde çeşitli rezervasyon alanlarının anlamı açıklanmıştır.

  **SKU** HLI SKU'sunun adı. Şuna benzer: `SAP_HANA_On_Azure_<SKUname>`.

  **Konum** Kullanılabilir HLI bölgeleri. Kullanılabilir bölgeler için bkz. [Azure üzerinde SAP HANA (Büyük Örnekler) için SKU'lar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-available-skus). Konum dizesinin biçimini almak için [konum alma API'si çağrısını](https://docs.microsoft.com/rest/api/resources/subscriptions/listlocations#locationlistresult) kullanın.

  **Ayrılmış Kaynak türü** `SapHana`

  **Abonelik** Rezervasyonun ödemesini yapmak için kullanılan abonelik. Rezervasyon maliyetleri, aboneliğin ödeme yöntemiyle ücretlendirilir. Abonelik türü bir kurumsal anlaşma (teklif numarası: MS-AZR-0017P veya MS-AZR-0148P) ya da Microsoft Müşteri Sözleşmesi. Ücretler, parasal taahhüt bakiyesinden düşülür veya varsa fazla kullanım olarak ücretlendirilir.

  **Kapsam** Rezervasyonun kapsamı tek bir kapsam olmalıdır.

  **Dönem** Bir yıl veya üç yıl. `P1Y` veya `P3Y` değerine benzer olmalıdır.

  **Miktar** Rezervasyon için satın alınan örneklerin sayısı. Satın alınacak miktar bir kerede tek bir HLI'dir. Ek rezervasyonlar için API çağrısını ilgili alanlarla tekrarlayın.

## <a name="troubleshoot-errors"></a>Sorun giderme hataları

Rezervasyon satın alma işlemini yaparken aşağıdaki örneğe benzer bir hata alabilirsiniz. Bunun olası nedeni HLI'nin satın alma için sağlanmamış olmasıdır. Böyle bir durumda, rezervasyon satın alma işlemini yapmayı denemeden önce sağlanan bir HLI elde etmek için Microsoft hesap ekibinizle bağlantı kurun.

```
{
  "error": {
    "code": "BadRequest",
    "message": "Capacity check or quota check failed. Please select a different subscription or 
location. You can also go to https://aka.ms/corequotaincrease to learn about quota increase."
  }
} 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Postman ve cURL ile Azure REST API'lerini çağırma](https://docs.microsoft.com/rest/api/azure/#how-to-call-azure-rest-apis-with-postman).
- Kullanılabilir SKU listesi ve bölgeler için bkz. [Azure üzerinde SAP HANA (Büyük Örnekler) için SKU'lar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-available-skus).