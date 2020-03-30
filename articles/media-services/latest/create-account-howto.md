---
title: Azure Media Services hesabı oluşturma
description: Bu öğretici, bir Azure Medya Hizmetleri hesabı oluşturma adımlarını size iletiyor.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/15/2020
ms.author: juliako
ms.openlocfilehash: 2f1694825319ed8b8682c044e7e2282ed4c43dcd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79478805"
---
# <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

Azure'da medya içeriğini şifrelemeye, kodlamaya, analiz etmeye, yönetmeye ve akışa başlamak için bir Medya Hizmetleri hesabı oluşturmanız gerekir. Medya Hizmetleri hesabının bir veya daha fazla depolama hesabıyla ilişkilendirilmesi gerekir.

> [!NOTE]
> Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.

Bu makalede, yeni bir Azure Medya Hizmetleri hesabı oluşturma adımları açıklanmaktadır. Aşağıdaki sekmelerden birini seçin.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Azure portalı, bir Azure Medya Hizmetleri hesabını hızla oluşturmanın bir yolunu sağlar. Azure’da medya içeriği depolamanıza, şifrelemenize, kodlamanıza, yönetmenize ve akış yapmanıza imkan tanıyan Media Services’e erişmek için hesabınızı kullanabilirsiniz.

Şu anda [Azure portalını](https://portal.azure.com/) şu şekilde kullanabilirsiniz:

* Medya Hizmetleri v3 [Canlı Etkinlikler](live-events-outputs-concept.md)yönetmek , 
* görünümü (yönetilmez) v3 [Varlıklar](assets-concept.md), 
* [API'lara erişim hakkında bilgi alın.](access-api-portal.md) 

Diğer tüm yönetim görevleri için (örneğin, [Dönüşümler ve İşler ve](transforms-jobs-concept.md) [İçerik koruması),](content-protection-overview.md) [REST API,](https://aka.ms/ams-v3-rest-ref) [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.

Bu makalede, Azure portalını kullanarak bir Medya Hizmetleri hesabının nasıl oluşturulutamamolduğu gösterilmektedir.

### <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

1. [Azure portalda](https://portal.azure.com/) oturum açın.
1. **+Kaynak** > **Medya** > **Medya Hizmetleri**Oluştur'u tıklatın.
1. Medya **Hizmetleri Hesabı Oluştur** bölümünde gerekli değerleri girin.
    
    | Adı | Açıklama |
    | ---|---|
    |**Hesap Adı**|Yeni Medya Hizmetleri hesabının adını girin. Media Services hesabı adı, boşluk olmadan, tümü sayı ve küçük harften oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.|
    |**Abonelik**|Birden fazla aboneliğiniz varsa, erişebildiğiniz Azure abonelikleri listesinden birini seçin.|
    |**Kaynak Grubu**|Yeni veya varolan kaynağı seçin. Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](../../azure-resource-manager/management/overview.md#resource-groups) daha fazla bilgi edinin.|
    |**Konum**|Medya Hizmetleri hesabınıziçin ortam ve meta veri kayıtlarını depolamak için kullanılacak coğrafi bölgeyi seçin. Bu bölge medyanızı işlemek ve akışını sağlamak için kullanılır. Yalnızca Media Services kullanılabilen bölgeler açılır listede görüntülenir. |
    |**Depolama Hesabı**|Medya Hizmetleri hesabınızdaki medya içeriğinin blob depolamasını sağlamak için bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da yeni bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.<br/><br/>Bir **Birincil** depolama hesabınız olması gerekir ve Medya Hizmetleri hesabınızla ilişkili herhangi bir sayıda **İkincil** depolama hesabınız olabilir. İkincil depolama hesapları eklemek için Azure portalını kullanabilirsiniz. Daha fazla bilgi için Azure [Medya Hizmetleri hesaplarıiçeren Azure Depolama hesaplarına](storage-account-concept.md)bakın.<br/><br/>Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.|
    
1. Hesap dağıtımını ilerleme durumunu görmek için **Panoya sabitle**’yi seçin.
1. Formun alt kısmındaki **Oluştur**’a tıklayın.

    Media Services hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçeriğinizi akışa başlamak ve [dinamik paketleme](dynamic-packaging-overview.md) ve dinamik [şifrelemeden](content-protection-overview.md)yararlanmak için, içeriği aktarmak istediğiniz akış bitiş **noktasının Çalışan** durumunda olması gerekir. 

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

Aşağıdaki komutta, Media Services hesabı için kullanmak istediğiniz Azure abonelik kimliğini sağlayın. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)’e giderek erişimine sahip olduğunuz aboneliklerin listesini görebilirsiniz.

```azurecli
az account set --subscription mySubscriptionId
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu; Azure Media Services hesapları ve ilişkili Depolama hesapları gibi kaynakların dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Değerinle `amsResourceGroup` değiştirebilirsiniz.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. Depolama hesaplarının Media Services’ta nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [Depolama hesapları](storage-account-concept.md).

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. Depolama hesapları hakkında daha fazla bilgi edinmek istiyorsanız bkz. [Azure Depolama hesap seçenekleri](../../storage/common/storage-account-options.md). 

Bu örnekte, Standart LRS hesabı olan Genel Amaçlı v2'yi oluştururuz. Depolama hesaplarıyla denemeler yapmak istiyorsanız, 'yi kullanın. `--sku Standard_LRS` Ancak, üretim için bir SKU seçerken düşünmelisiniz, `--sku Standard_RAGRS`hangi iş sürekliliği için coğrafi çoğaltma sağlar. Daha fazla bilgi için [depolama hesaplarına](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest)bakın.
 
Aşağıdaki komut, Media Services Hesabı ile ilişkilendirilecek Depolama hesabını oluşturur. Aşağıdaki betikte `storageaccountforams` öğesini kendi değeriniz ile değiştirebilirsiniz. `amsResourceGroup`önceki adımda kaynak grubu için vermiş olduğunuz değeri eşleştirmelidir. Depolama hesabı adı 24'ten az uzunlukta olmalıdır.

```azurecli
az storage account create --name storageaccountforams \  
  --kind StorageV2 \
  --sku Standard_LRS \
  -l westus2 \
  -g amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

Aşağıdaki Azure CLI komutu yeni bir Media Services hesabı oluşturur. Aşağıdaki değerleri değiştirebilirsiniz: `amsaccount` `storageaccountforams` (depolama hesabınız için vermiş olduğunuz değerle eşleşmeli ve `amsResourceGroup` (kaynak grubu için vermiş olduğunuz değerle eşleşmelidir).  

```azurecli
az ams account create --name amsaccount \
   -g amsResourceGroup --storage-account storageaccountforams \
   -l westus2 
```

### <a name="see-also"></a>Ayrıca bkz.

* [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [Medya Hizmetleri hesabına ikincil bir depolama alanı ekleme](https://docs.microsoft.com/cli/azure/ams/account/storage?view=azure-cli-latest#az-ams-account-storage-add)

---

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
