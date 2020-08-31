---
title: Azure Event Grid iş ortağı ad alanlarını başka bir bölgeye taşıyın
description: Bu makalede Azure Event Grid iş ortağı ad alanlarını bir bölgeden başka bir bölgeye nasıl taşıyacağınız gösterilmektedir.
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/20/2020
ms.openlocfilehash: 6783db6b9bb1c7d48b308234a179925d6f30e281
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89087211"
---
# <a name="move-azure-event-grid-partner-namespaces-to-another-region"></a>Azure Event Grid iş ortağı ad alanlarını başka bir bölgeye taşıyın
Birkaç nedenden dolayı kaynaklarınızı başka bir bölgeye taşımak isteyebilirsiniz. Örneğin, yeni bir Azure bölgesinden yararlanarak iç ilke ve idare gereksinimlerini veya kapasite planlama gereksinimlerine yanıt olarak tanışın. 

Bu makalede ele alınan üst düzey adımlar aşağıda verilmiştir: 

- **İş ortağı ad alanı** kaynağını bir Azure Resource Manager şablonuna dışarı aktarın. Şablondaki olay kanalı kaynakları için tanımları silin. Bir olay kanalının bir müşteriye ait olan iş ortağı konusunun Azure Resource Manager KIMLIĞINE bir başvurusu olabilir. Bu nedenle, hedef bölgedeki şablonu kullanılarak oluşturulamaz.  
- **İş ortağı ad alanını hedef bölgeye dağıtmak için şablonu kullanın** . Ardından, hedef bölgedeki yeni iş ortağı ad alanında olay kanalları oluşturun. 
- **Taşıma işleminin tamamlanabilmesi**için, kaynak bölgedeki iş ortağı ad alanını silin. 

    > [!NOTE]
    > - Müşteriler doğrudan bir iş ortağı konusu oluşturamadığı için **iş ortağı konularının** Azure Resource Manager bir şablona aktarılması desteklenmez. 
    > - **Iş ortağı kayıtları** genel kaynaklardır (belirli bir bölgeye bağlı değildir), bu nedenle bunları bir bölgeden başka bir bölgeye taşımak geçerli değildir. 

## <a name="prerequisites"></a>Önkoşullar
- Event Grid hizmetinin hedef bölgede kullanılabildiğinden emin olun. [Bölgeye göre kullanılabilen ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all)görüntüleyin.

## <a name="prepare"></a>Hazırlama
Başlamak için iş ortağı ad alanı için bir Kaynak Yöneticisi şablonu dışarı aktarın. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Üstteki arama çubuğunda **Event Grid iş ortakları**yazın ve sonuç listesinden **Event Grid Iş ortağı ad alanları** ' nı seçin. 
3. Kaynak Yöneticisi şablonuna aktarmak istediğiniz **iş ortağı ad alanını** seçin. 
4. **Event Grid Iş ortağı ad alanı** sayfasında sol menüdeki **Ayarlar** ' ın altında **şablonu dışarı aktar** ' ı seçin ve ardından araç çubuğunda **İndir** ' i seçin. 

    :::image type="content" source="./media/move-partner-namespaces-across-regions/download-template.png" alt-text="Şablonu dışarı aktar-> Indir" lightbox="./media/move-partner-namespaces-across-regions/download-template.png":::   
5. Portaldan indirdiğiniz **. zip** dosyasını bulun ve bu dosyayı seçtiğiniz bir klasöre ayıklayın. Bu ZIP dosyası Template ve Parameters JSON dosyalarını içerir. 
1. **template.js** seçtiğiniz bir düzenleyicide açın. 
8. `location` **Konu** kaynağını hedef bölgeye veya konuma güncelleştirin. Konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bir bölgenin kodu, boşluk içermeyen bölge adıdır, örneğin, `West US` eşittir `westus` .

    ```json
    {
        "type": "Microsoft.EventGrid/partnerNamespaces",
        "apiVersion": "2020-04-01-preview",
        "name": "[parameters('partnerNamespace_name')]",
        "location": "westus",
        "properties": {
            "partnerRegistrationFullyQualifiedId": "[parameters('partnerRegistrations_ContosoCorpAccount1_externalid')]"
        }
    }
    ``` 
1. Şablonu **kaydedin** . 

## <a name="recreate"></a>Oluştur 
Hedef bölgede bir iş ortağı ad alanı oluşturmak için şablonu dağıtın. 

1. Azure portal **kaynak oluştur**' u seçin.
2. **Market 'Te ara**' te, **şablon dağıtımı**yazın ve ardından **ENTER**tuşuna basın.
3. **Şablon dağıtımı**seçin.
4. **Oluştur**’u seçin.
5. **Düzenleyicide kendi şablonunuzu oluşturun öğesini**seçin.
6. **Dosya Yükle**' yi seçin ve ardından son bölümde indirdiğiniz dosyanın **template.js** yüklemek için yönergeleri izleyin.
7. Şablonu kaydetmek için **Kaydet** ' i seçin. 
8. **Özel dağıtım** sayfasında, aşağıdaki adımları izleyin: 
    1. Bir Azure **aboneliği**seçin. 
    1. Hedef bölgede var olan bir kaynak grubunu seçin veya bir **kaynak grubu** oluşturun. 
    1. **Konum**için hedef bölgeyi seçin. Var olan bir kaynak grubunu seçtiyseniz, bu ayar salt okunurdur. 
    1. **İş ortağı ad alanı adı**için, yeni iş ortağı ad alanı için bir ad girin. 
    1. İş ortağı kaydının dış KIMLIĞI için, iş ortağı kaydının kaynak KIMLIĞINI şu biçimde girin: `/subscriptions/<Azure subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.EventGrid/partnerRegistrations/<Partner registration name>` .
    1. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** onay kutusunu seçin.     
    1. Dağıtım işlemini başlatmak için **gözden geçir + oluştur** ' u seçin. 
    1. **Gözden geçir + oluştur** sayfasında ayarları gözden geçirin ve **Oluştur**' u seçin. 

## <a name="discard-or-clean-up"></a>Atma veya temizleme
Taşıma işleminin tamamlanabilmesi için kaynak bölgedeki iş ortağı ad alanını silin.  

Baştan başlamak istiyorsanız, hedef bölgedeki iş ortağı ad alanını silin ve bu makalenin [hazırlama](#prepare) ve [yeniden oluşturma](#recreate) bölümlerinde yer alan adımları yineleyin.

Azure portal kullanarak bir iş ortağı ad alanını silmek için:

1. Azure portal en üstündeki ara penceresinde, **Event Grid Iş ad alanları**yazın ve arama sonuçlarından **Event Grid Iş ortağı ad alanlarını** seçin. 
2. Silinecek iş ortağı ad alanını seçin ve araç çubuğundan **Sil** ' i seçin. 
3. İş ortağı ad alanını silmek için silme işlemini **onaylayın** . 

## <a name="next-steps"></a>Sonraki adımlar
Bir Event Grid iş ortağı ad alanını bir bölgeden başka bir bölgeye nasıl taşıyabileceğinizi öğrendiniz. Sistem konularını, özel konuları ve etki alanlarını bölgeler arasında taşımak için aşağıdaki makalelere bakın.

- [Sistem konularını bölgeler arasında taşıyın](move-system-topics-across-regions.md). 
- [Özel konuları bölgeler arasında taşıyın](move-custom-topics-across-regions.md). 
- [Etki alanlarını bölgeler arasında taşıyın](move-domains-across-regions.md).

Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın: [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md).