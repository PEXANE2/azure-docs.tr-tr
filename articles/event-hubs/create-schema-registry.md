---
title: Azure Event Hubs şeması kayıt defteri oluşturma
description: Bu makalede, Azure Event Hubs ad alanında nasıl şema kayıt defteri oluşturacağınız gösterilmektedir.
ms.topic: how-to
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: 90556e0843cda94dc79330321d027f8d28eb7d20
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91652202"
---
# <a name="create-an-azure-event-hubs-schema-registry-preview"></a>Azure Event Hubs şeması kayıt defteri oluşturma (Önizleme)
Bu makalede, Azure Event Hubs tarafından barındırılan bir şema kayıt defterinde şemalarla nasıl şema grubu oluşturacağınız gösterilmektedir. Azure Event Hubs 'in şema kayıt defteri özelliğine genel bir bakış için, bkz. [Event Hubs Azure şema kayıt defteri](schema-registry-overview.md).

> [!NOTE]
> - **Şema kayıt defteri** özelliği şu anda **önizlemededir**ve üretim iş yükleri için önerilmez.
> - Özelliği, **temel** katmanda değil, yalnızca **Standart** ve **adanmış** katmanlarda kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
[Event Hubs ad alanı oluşturun](event-hubs-create.md#create-an-event-hubs-namespace). Mevcut bir ad alanını da kullanabilirsiniz. 

## <a name="create-a-schema-group"></a>Şema grubu oluşturma
1. **Event Hubs ad alanı** sayfasına gidin. 
1. Sol taraftaki menüde **şema kayıt defteri** ' ni seçin. Bir şema grubu oluşturmak için araç çubuğunda **+ şema grubu** ' nu seçin. 

    :::image type="content" source="./media/create-schema-registry/namespace-page.png" alt-text="Şema kayıt defteri sayfası":::
1. **Şema grubu oluştur** sayfasında, şu adımları uygulayın:
    1. Şema grubu için bir **ad** girin.
    1. **Serileştirme türü**için, şema grubundaki tüm şemalar için geçerli olan bir serileştirme biçimi seçin. Şu anda, **avro** desteklenen tek türdür, bu nedenle **avro**' yi seçin. 
    1. Gruptaki tüm şemalar için bir **Uyumluluk modu** seçin. **Avro**için, ileri ve geri uyumluluk modları desteklenir. 
    1. Ardından, şema grubunu oluşturmak için **Oluştur** ' u seçin. 
1. Şema grupları listesinde şema **grubunun** adını seçin.

    :::image type="content" source="./media/create-schema-registry/select-schema-group.png" alt-text="Şema kayıt defteri sayfası":::    
1. Grubun **şema grubu** sayfasını görürsünüz.

    :::image type="content" source="./media/create-schema-registry/schema-group-page.png" alt-text="Şema kayıt defteri sayfası":::
    

## <a name="add-a-schema-to-the-schema-group"></a>Şema grubuna bir şema ekleyin
Bu bölümde, Azure portal kullanarak şema grubuna bir şema eklersiniz. 

1. **Şema grubu** sayfasında, araç çubuğunda **+ şema** ' ı seçin. 
1. **Şema oluştur** sayfasında, şu adımları uygulayın:
    1. Şema için bir **ad** girin.
    1. Metin kutusuna aşağıdaki **Şemayı** girin. Şemaya sahip dosya ' yı da seçebilirsiniz.
    
        ```json
        {
            "type": "record",
            "name": "AvroUser",
            "namespace": "com.azure.schemaregistry.samples",
            "fields": [
                {
                    "name": "name",
                    "type": "string"
                },
                {
                    "name": "favoriteNumber",
                    "type": "int"
                }
            ]
        }
        ```
    1. **Oluştur**’u seçin. 
1. Şemalar listesinden **Şemayı** seçin. 

    :::image type="content" source="./media/create-schema-registry/select-schema.png" alt-text="Şema kayıt defteri sayfası":::
1. Şema için **şemaya genel bakış** sayfasını görürsünüz. 

    :::image type="content" source="./media/create-schema-registry/schema-overview-page.png" alt-text="Şema kayıt defteri sayfası":::    
1. Bir şemanın birden çok sürümü varsa, bunları **sürümler** açılan listesinde görürsünüz. Bu sürüm şemasına geçiş yapmak için bir sürüm seçin. 

## <a name="create-a-new-version-of-schema"></a>Yeni şema sürümü oluştur

1. Metin kutusunda şemayı güncelleştirin ve **Doğrula**' yı seçin. Aşağıdaki örnekte, şemaya yeni bir alan `id` eklenmiştir. 

    :::image type="content" source="./media/create-schema-registry/update-schema.png" alt-text="Şema kayıt defteri sayfası":::    
    
1. Doğrulama durumunu ve değişiklikleri gözden geçirin ve **Kaydet**' i seçin. 

    :::image type="content" source="./media/create-schema-registry/compare-save-schema.png" alt-text="Şema kayıt defteri sayfası":::     
1. `2` **Şemaya genel bakış** sayfasındaki **Sürüm** için seçili olduğunu görürsünüz. 

    :::image type="content" source="./media/create-schema-registry/new-version.png" alt-text="Şema kayıt defteri sayfası":::    
1. `1`Şemanın 1. sürümünü görmek için seçin. 

    :::image type="content" source="./media/create-schema-registry/select-version.png" alt-text="Şema kayıt defteri sayfası":::    


## <a name="next-steps"></a>Sonraki adımlar
Şema kayıt defteri hakkında daha fazla bilgi için bkz. [Event Hubs Azure şema kayıt defteri](schema-registry-overview.md).

