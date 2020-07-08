---
title: Azure Media Services hesabı oluşturma
description: Bu öğretici Azure Media Services hesap oluşturma adımlarında size yol gösterir.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79478805"
---
# <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

Azure 'da medya içeriğini şifrelemeyi, kodlamayı, çözümlemeyi, yönetmeyi ve akışını başlatmak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabının bir veya daha fazla depolama hesabıyla ilişkilendirilmesi gerekir.

> [!NOTE]
> Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.

Bu makalede yeni bir Azure Media Services hesabı oluşturmaya yönelik adımlar açıklanmaktadır. Aşağıdaki sekmelerden birini seçin.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Azure portal, hızlı bir şekilde Azure Media Services hesabı oluşturmak için bir yol sağlar. Azure’da medya içeriği depolamanıza, şifrelemenize, kodlamanıza, yönetmenize ve akış yapmanıza imkan tanıyan Media Services’e erişmek için hesabınızı kullanabilirsiniz.

Şu anda [Azure Portal](https://portal.azure.com/) şunları yapmak için kullanabilirsiniz:

* Media Services v3 [canlı olaylarını](live-events-outputs-concept.md)yönetin, 
* v3 [varlıklarını](assets-concept.md)görüntüleme (yönetme), 
* [API 'lere erişme hakkında bilgi alın](access-api-portal.md). 

Diğer tüm yönetim görevleri (örneğin, [dönüşümler ve işler](transforms-jobs-concept.md) ve [içerik koruması](content-protection-overview.md)) için [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK 'lardan](media-services-apis-overview.md#sdks)birini kullanın.

Bu makalede Azure portal kullanılarak Media Services hesabının nasıl oluşturulacağı gösterilmektedir.

### <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

1. [Azure portalda](https://portal.azure.com/) oturum açın.
1. **+ Kaynak**  >  **medyası**oluştur  >  **Media Services**' a tıklayın.
1. **Media Services hesabı oluştur** bölümünde gerekli değerleri girin.
    
    | Name | Açıklama |
    | ---|---|
    |**Hesap Adı**|Yeni Media Services hesabının adını girin. Media Services hesabı adı, boşluk olmadan, tümü sayı ve küçük harften oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.|
    |**Abonelik**|Birden fazla aboneliğiniz varsa, erişiminiz olan Azure abonelikleri listesinden birini seçin.|
    |**Kaynak Grubu**|Yeni veya mevcut kaynağı seçin. Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](../../azure-resource-manager/management/overview.md#resource-groups) daha fazla bilgi edinin.|
    |**Konum**|Media Services hesabınız için medya ve meta veri kayıtlarını depolamak üzere kullanılacak coğrafi bölgeyi seçin. Bu bölge medyanızı işlemek ve akışını sağlamak için kullanılır. Yalnızca Media Services kullanılabilen bölgeler açılır listede görüntülenir. |
    |**Depolama Hesabı**|Media Services hesabınızdan medya içeriğinin blob depolamasını sağlamak için bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da yeni bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.<br/><br/>Bir **birincil** depolama hesabınız olmalıdır ve Media Services hesabınızla Ilişkili birkaç **İkincil** depolama hesabı olabilir. İkincil depolama hesaplarını eklemek için Azure portal kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Media Services hesapları Ile Azure depolama hesapları](storage-account-concept.md).<br/><br/>Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.|
    
1. Hesap dağıtımını ilerleme durumunu görmek için **Panoya sabitle**’yi seçin.
1. Formun alt kısmındaki **Oluştur**’a tıklayın.

    Media Services hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçeriğinizi akışa almak ve [dinamik paketleme](dynamic-packaging-overview.md) ile [dinamik şifrelemeden](content-protection-overview.md)yararlanmak için içerik akışı yapmak Istediğiniz akış uç noktasının **çalışıyor** durumda olması gerekir. 

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

Aşağıdaki komutta, Media Services hesabı için kullanmak istediğiniz Azure abonelik kimliğini sağlayın. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)’e giderek erişimine sahip olduğunuz aboneliklerin listesini görebilirsiniz.

```azurecli
az account set --subscription mySubscriptionId
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu; Azure Media Services hesapları ve ilişkili Depolama hesapları gibi kaynakların dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

`amsResourceGroup`Değeri ile değiştirebilirsiniz.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. Depolama hesaplarının Media Services’ta nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [Depolama hesapları](storage-account-concept.md).

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. Depolama hesapları hakkında daha fazla bilgi edinmek istiyorsanız bkz. [Azure Depolama hesap seçenekleri](../../storage/common/storage-account-options.md). 

Bu örnekte, bir Genel Amaçlı v2, standart LRS hesabı oluşturacağız. Depolama hesaplarıyla denemek istiyorsanız kullanın `--sku Standard_LRS` . Ancak, üretim için bir SKU seçerken `--sku Standard_RAGRS` iş sürekliliği için coğrafi çoğaltma sağlayan göz önünde bulundurmanız gerekir. Daha fazla bilgi için bkz. [depolama hesapları](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest).
 
Aşağıdaki komut, Media Services Hesabı ile ilişkilendirilecek Depolama hesabını oluşturur. Aşağıdaki betikte `storageaccountforams` öğesini kendi değeriniz ile değiştirebilirsiniz. `amsResourceGroup`önceki adımda kaynak grubu için verdiğiniz değerle aynı olmalıdır. Depolama hesabı adının uzunluğu 24 ' ten az olmalıdır.

```azurecli
az storage account create --name storageaccountforams \  
  --kind StorageV2 \
  --sku Standard_LRS \
  -l westus2 \
  -g amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

Aşağıdaki Azure CLI komutu yeni bir Media Services hesabı oluşturur. Aşağıdaki değerleri değiştirebilirsiniz: `amsaccount` `storageaccountforams` (depolama hesabınız için verdiğiniz değerle eşleşmelidir) ve `amsResourceGroup` (kaynak grubu için verdiğiniz değerle eşleşmesi gerekir).  

```azurecli
az ams account create --name amsaccount \
   -g amsResourceGroup --storage-account storageaccountforams \
   -l westus2 
```

### <a name="see-also"></a>Ayrıca bkz.

* [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [Media Services hesaba ikincil depolama iliştirme](https://docs.microsoft.com/cli/azure/ams/account/storage?view=azure-cli-latest#az-ams-account-storage-add)

---

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
