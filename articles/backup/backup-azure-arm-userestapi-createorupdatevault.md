---
title: REST API'yi kullanarak Kurtarma Hizmetleri kasaları oluşturun
description: Bu makalede, REST API'yi kullanarak Azure VM Yedekleme'nin yedekleme ve geri yükleme işlemlerini nasıl yöneteceğimiz öğrenin.
ms.topic: conceptual
ms.date: 08/21/2018
ms.assetid: e54750b4-4518-4262-8f23-ca2f0c7c0439
ms.openlocfilehash: 1901c35d2b4d8bcd02cc064fcfc844e19969e3b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74173410"
---
# <a name="create-azure-recovery-services-vault-using-rest-api"></a>REST API'yi kullanarak Azure Kurtarma Hizmetleri Kasası oluşturma

REST API'yi kullanarak Bir Azure Kurtarma Hizmetleri Kasası oluşturma [adımları, vault REST API](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate) belgelerinde özetlenmiştir. Bu belgeyi "Batı ABD"de "testVault" adı verilen bir kasa oluşturmak için referans olarak kullanalım.

Bir Azure Kurtarma Hizmetleri kasası oluşturmak veya güncelleştirmek için aşağıdaki *PUT* işlemini kullanın.

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}?api-version=2016-06-01
```

## <a name="create-a-request"></a>İstek oluşturma

*PUT* isteğini oluşturmak için `{subscription-id}` parametre gereklidir. Birden çok aboneliğiniz varsa, [bkz.](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest) Parametreile `{resourceGroupName}` `{vaultName}` birlikte kaynaklarınız için bir ve kaynaklar tanımlarsınız. `api-version` Bu makalede `api-version=2016-06-01`kullanır.

Aşağıdaki üstbilgiler gereklidir:

| İstek üst bilgisi   | Açıklama |
|------------------|-----------------|
| *İçerik Türü:*  | Gereklidir. `application/json` olarak ayarlayın. |
| *Yetkilendirme:* | Gereklidir. Geçerli bir `Bearer` [erişim belirtecine](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients) ayarlayın. |

İsteğin nasıl oluşturulacığı hakkında daha fazla bilgi [için, REST API istek/yansıtımının Bileşenleri'](/rest/api/azure/#components-of-a-rest-api-requestresponse)ne bakın.

## <a name="create-the-request-body"></a>İstek gövdesini oluşturma

Bir istek gövdesi oluşturmak için aşağıdaki yaygın tanımlar kullanılır:

|Adı  |Gerekli  |Tür  |Açıklama  |
|---------|---------|---------|---------|
|Etag     |         |   Dize      |  İsteğe bağlı eTag       |
|location     |  true       |Dize         |   Kaynak konumu      |
|properties     |         | [Kasa Özellikleri](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vaultproperties)        |  Kasanın özellikleri       |
|Sku     |         |  [Sku](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#sku)       |    Her Azure kaynağı için benzersiz sistem tanımlayıcısını tanımlar     |
|etiketler     |         | Nesne        |     Kaynak etiketleri    |

PUT URI'de kasa adı ve kaynak grubu adı verildiğini unutmayın. İstek gövdesi konumu tanımlar.

## <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki örnek gövde "Batı ABD"de bir tonoz oluşturmak için kullanılır. Konumu belirtin. SKU her zaman "Standart"tır.

```json
{
  "properties": {},
  "sku": {
    "name": "Standard"
  },
  "location": "West US"
}
```

## <a name="responses"></a>Yanıtlar

Kurtarma Hizmetleri kasası oluşturmak veya güncelleştirmek için işlem için iki başarılı yanıt vardır:

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |   [Kasa](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)      | Tamam        |
|201 Oluşturuldu     | [Kasa](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)        |   Oluşturulan      |

REST API yanıtları hakkında daha fazla bilgi için [yanıt iletisini işleme](/rest/api/azure/#process-the-response-message)ye bakın.

### <a name="example-response"></a>Örnek yanıt

Bir yoğunlaştırılmış *201 Önceki* örnek istek gövdesinden oluşturulan yanıt bir *id* atanmış ve *provisioningState* *Başarılı*olduğunu gösterir:

```json
{
  "location": "westus",
  "name": "testVault",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "id": "/subscriptions/77777777-b0c6-47a2-b37c-d8e65a629c18/resourceGroups/Default-RecoveryServices-ResourceGroup/providers/Microsoft.RecoveryServices/vaults/testVault",
  "type": "Microsoft.RecoveryServices/vaults",
  "sku": {
    "name": "Standard"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Bu kasada bir Azure VM'yi yedeklemek için bir yedekleme ilkesi oluşturun.](backup-azure-arm-userestapi-createorupdatepolicy.md)

Azure REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure Kurtarma Hizmetleri sağlayıcısı REST API](/rest/api/recoveryservices/)
- [Azure REST API’yi kullanmaya başlayın](/rest/api/azure/)
