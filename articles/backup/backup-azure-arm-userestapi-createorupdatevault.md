---
title: 'Azure Backup: REST API kullanarak kurtarma hizmetleri kasaları oluşturma'
description: Bu makalede, REST API kullanarak Azure VM yedeklemesi 'nin yedekleme ve geri yükleme işlemlerini yönetmeyi öğrenin.
ms.reviewer: pullabhk
author: dcurwin
manager: carmonm
keywords: REST API; Azure VM yedeklemesi; Azure VM geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: dacurwin
ms.assetid: e54750b4-4518-4262-8f23-ca2f0c7c0439
ms.openlocfilehash: 7c9d165f623367d1c888b90f76e96ce8fa2e8f89
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747556"
---
# <a name="create-azure-recovery-services-vault-using-rest-api"></a>REST API kullanarak Azure kurtarma hizmetleri Kasası oluşturma

REST API kullanarak bir Azure kurtarma hizmetleri Kasası oluşturma adımları [kasa oluşturma REST API](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate) belgelerinde özetlenmiştir. "Batı ABD" içinde "Testkasası" adlı bir kasa oluşturmak için bu belgeyi bir başvuru olarak kullanmamıza izin verin.

Bir Azure kurtarma hizmetleri Kasası oluşturmak veya güncelleştirmek için aşağıdaki *PUT* işlemini kullanın.

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}?api-version=2016-06-01
```

## <a name="create-a-request"></a>İstek oluştur

*PUT* isteğini oluşturmak için `{subscription-id}` parametresi gereklidir. Birden çok aboneliğiniz varsa bkz. [birden çok abonelikle çalışma](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). Kaynaklarınızın `{resourceGroupName}` ve `{vaultName}`, `api-version` parametresiyle birlikte tanımlarsınız. Bu makale `api-version=2016-06-01`kullanır.

Aşağıdaki üstbilgiler gereklidir:

| İstek üst bilgisi   | Açıklama |
|------------------|-----------------|
| *Content-Type:*  | Gereklidir. `application/json` olarak ayarlayın. |
| *Yetkilendirme:* | Gereklidir. Geçerli bir `Bearer` [erişim belirtecine](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients) ayarlayın. |

İsteğin nasıl oluşturulacağı hakkında daha fazla bilgi için, bkz. [bir REST API isteği/yanıtı bileşenleri](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>İstek gövdesini oluşturma

Aşağıdaki ortak tanımlar bir istek gövdesi oluşturmak için kullanılır:

|Ad  |Gerekli  |Tür  |Açıklama  |
|---------|---------|---------|---------|
|Özelliği     |         |   Dize      |  İsteğe bağlı eTag       |
|location     |  doğru       |Dize         |   Kaynak konumu      |
|properties     |         | [VaultProperties](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vaultproperties)        |  Kasanın özellikleri       |
|İsteyin     |         |  [Sku](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#sku)       |    Her Azure kaynağı için benzersiz sistem tanımlayıcısını tanımlar     |
|etiketler     |         | Nesne        |     Kaynak etiketleri    |

Kasa adı ve kaynak grubu adının PUT URI 'sinde sağlandığını unutmayın. İstek gövdesi konumu tanımlar.

## <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki örnek gövde, "Batı ABD" içinde bir kasa oluşturmak için kullanılır. Konumu belirtin. SKU her zaman "standart" dır.

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

Bir kurtarma hizmetleri Kasası oluşturma veya güncelleştirme işlemi için iki başarılı yanıt vardır:

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |   [Kasa](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)      | Tamam        |
|201 oluşturuldu     | [Kasa](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vault)        |   Oluşturulan      |

REST API yanıtları hakkında daha fazla bilgi için bkz. [Yanıt Iletisini işleme](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Örnek yanıt

Önceki örnek istek gövdesinden sıkıştırılmış *201 tarafından oluşturulan* bir yanıt, bir *kimlik* atandığını ve *provisioningstate* *başarılı*olduğunu gösterir:

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

[Bu kasada bir Azure VM yedeklemesi için bir yedekleme Ilkesi oluşturun](backup-azure-arm-userestapi-createorupdatepolicy.md).

Azure REST API 'Leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure kurtarma hizmetleri sağlayıcısı REST API](/rest/api/recoveryservices/)
- [Azure REST API’yi kullanmaya başlayın](/rest/api/azure/)
