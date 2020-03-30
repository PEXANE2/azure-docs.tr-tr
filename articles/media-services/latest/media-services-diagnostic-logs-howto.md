---
title: Azure Monitor üzerinden Medya Hizmetleri tanı lama günlüklerini izleyin | Microsoft Dokümanlar
description: Bu makalede, Azure Monitor üzerinden tanılama günlüklerinin nasıl yönlendirilip görüntülenenebildiğini gösterin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 4d4587c701a054828fc34785e2ae680fef47625d
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382928"
---
# <a name="monitor-media-services-diagnostic-logs"></a>Medya Hizmetleri tanı günlüklerini izleyin

[Azure Monitor,](../../azure-monitor/overview.md) uygulamalarınızın nasıl performans gösterdiğini anlamanıza yardımcı olan ölçümleri ve tanılama günlüklerini izlemenize olanak tanır. Bu özelliğin ayrıntılı açıklaması ve Azure Medya Hizmetleri ölçümlerini ve tanılama günlüklerini neden kullanmak istediğinizi görmek için [Bkz.](media-services-metrics-diagnostic-logs.md)

Bu makalede, verileri depolama hesabına nasıl yönlendirip sonra da verileri nasıl görüntüleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)
- [Monitör Medya Hizmetleri ölçümlerini ve tanılama günlüklerini gözden geçirin.](media-services-metrics-diagnostic-logs.md)

## <a name="route-data-to-the-storage-account-using-the-portal"></a>Portalı kullanarak verileri depolama hesabına yönlendirin

1. https://portal.azure.com adresinden Azure portalında oturum açın.
1. Medya Hizmetleri hesabınıza gidin ve **Monitor**altında **Tanılama Ayarları'na** tıklayın. Burada, aboneliğinizde Azure İzleyici ile izleme verileri oluşturan tüm kaynakların bir listesini görürsünüz.

    ![Tanılama ayarları bölümü](media/media-services-diagnostic-logs/logs01.png)

1. **Tanı ayarını ekle'yi**tıklatın.

   Kaynak tanılama ayarı, belirli bir kaynaktan *hangi* izleme verilerinin yönlendirilmesi gerektiğine ve bu izleme verilerinin *nereye* gideceğine ilişkin bir tanımdır.

1. Görüntülenen bölümde, ayarınıza bir **ad** verin ve **Bir depolama hesabında arşivle** kutusunu işaretleyin.

    Günlükgöndermek istediğiniz depolama hesabını seçin ve **Tamam**tuşuna basın.
1. **Günlük** ve **Ölçüm** altındaki tüm kutuları işaretleyin. Kaynak türüne bağlı olarak, bu seçeneklerden yalnızca birini kullanabilirsiniz. Bu onay kutuları, seçtiğiniz hedefe (bu örnekte bir depolama hesabına) ilgili kaynak türü için kullanılabilen günlük ve ölçüm verileri kategorilerinden hangilerinin gönderildiğini denetler.

   ![Tanılama ayarları bölümü](media/media-services-diagnostic-logs/logs02.png)
1. **Bekletme (gün)** kaydırıcısını 30’a ayarlayın. Bu kaydırıcı, depolama hesabında izleme verilerinin tutulacağı gün sayısını ayarlar. Azure İzleyici, belirtilen gün sayısından daha eski verileri otomatik olarak siler. Bekletme günü sayısının sıfır olması verileri süresiz olarak depolar.
1. **Kaydet**'e tıklayın.

Kaynağınızdaki izleme verileri artık depolama hesabına akar.

## <a name="route-data-to-the-storage-account-using-the-azure-cli"></a>Azure CLI'yi kullanarak verileri depolama hesabına yönlendirin

Tanılama günlüklerinin bir Depolama Hesabında depolanmasını etkinleştirmek `az monitor diagnostic-settings` için aşağıdaki Azure CLI komutunu çalıştırAbilirsiniz:

```azurecli-interactive
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Örnek:

```azurecli-interactive
az monitor diagnostic-settings create --name amsv3diagnostic \
    --storage-account storageaccountforams  \
    --resource "/subscriptions/00000000-0000-0000-0000-0000000000/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount" \
    --resource-group "amsResourceGroup" \
    --logs '[{"category": "KeyDeliveryRequests",  "enabled": true, "retentionPolicy": {"days": 3, "enabled": true }}]'
```

## <a name="view-data-in-the-storage-account-using-the-portal"></a>Portalı kullanarak depolama hesabındaki verileri görüntüleme

Yukarıdaki adımları izlediyseniz, veriler depolama hesabınıza akmaya başlamıştır.

Olayın depolama hesabında görünmesi için beş dakikaya kadar beklemeniz gerekebilir.

1. Portalda, sol gezinti çubuğundaki **Depolama Hesapları** bölümüne gidin.
1. Önceki bölümde oluşturduğunuz depolama hesabını belirleyin ve tıklayın.
1. **Blobs**tıklayın, sonra konteyner etiketli **insights-logs-keydeliveryrequests**. Bu, içinde günlüklerin olduğu konteyner. İzleme verileri kaynak kimliğine, sonra tarih ve saate göre kapsayıcılara bölünerek ayrılır.
1. Kaynak kimliği, tarih ve saat için kapsayıcılara tıklayarak PT1H.json dosyasına gidin. PT1H.json dosyasına ve **İndir**’e tıklayın.

 Artık depolama hesabında depolanmış JSON olayını görüntüleyebilirsiniz.

### <a name="examples-of-pt1hjson"></a>PT1H.json örnekleri

#### <a name="clear-key-delivery-log"></a>Anahtar teslim günlüğünü temizle

```json
{
  "time": "2019-05-21T00:07:33.2820450Z",
  "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/amsResourceGroup/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/AMSACCOUNT",
  "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
  "operationVersion": "1.0",
  "category": "KeyDeliveryRequests",
  "resultType": "Succeeded",
  "resultSignature": "OK",
  "durationMs": 253,
  "identity": {
    "authorization": {
      "issuer": "myIssuer",
      "audience": "myAudience"
    },
    "claims": {
      "urn:microsoft:azure:mediaservices:contentkeyidentifier": "e4276e1d-c012-40b1-80d0-ac15808b9277",
      "nbf": "1558396914",
      "exp": "1558400814",
      "iss": "myIssuer",
      "aud": "myAudience"
    }
  },
  "level": "Informational",
  "location": "westus2",
  "properties": {
    "requestId": "fb5c2b3a-bffa-4434-9c6f-73d689649add",
    "keyType": "Clear",
    "keyId": "e4276e1d-c012-40b1-80d0-ac15808b9277",
    "policyName": "SharedContentKeyPolicyUsedByAllAssets",
    "tokenType": "JWT",
    "statusMessage": "OK"
  }
}
```

#### <a name="widevine-encrypted-key-delivery-log"></a>Widevine şifreli anahtar teslim günlüğü

```json
{
  "time": "2019-05-20T23:15:22.7088747Z",
  "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/amsResourceGroup/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/AMSACCOUNT",
  "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
  "operationVersion": "1.0",
  "category": "KeyDeliveryRequests",
  "resultType": "Succeeded",
  "resultSignature": "OK",
  "durationMs": 69,
  "identity": {
    "authorization": {
      "issuer": "myIssuer",
      "audience": "myAudience"
    },
    "claims": {
      "urn:microsoft:azure:mediaservices:contentkeyidentifier": "0092d23a-0a42-4c5f-838e-6d1bbc6346f8",
      "nbf": "1558392430",
      "exp": "1558396330",
      "iss": "myIssuer",
      "aud": "myAudience"
    }
  },
  "level": "Informational",
  "location": "westus2",
  "properties": {
    "requestId": "49613dd2-16aa-4595-a6e0-4e68beae6d37",
    "keyType": "Widevine",
    "keyId": "0092d23a-0a42-4c5f-838e-6d1bbc6346f8",
    "policyName": "DRMContentKeyPolicy",
    "tokenType": "JWT",
    "statusMessage": "OK"
  }
}
```

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Monitör Ölçümleri](../../azure-monitor/platform/data-platform.md)
* [Azure Monitör Tanılama günlükleri](../../azure-monitor/platform/platform-logs-overview.md)
* [Azure kaynaklarınızdan günlük verileri toplama ve kullanma](../../azure-monitor/platform/platform-logs-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

[İzleyici ölçümleri](media-services-metrics-howto.md)
