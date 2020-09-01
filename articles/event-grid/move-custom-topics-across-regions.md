---
title: Azure Event Grid özel konuları başka bir bölgeye taşıyın
description: Bu makalede, özel konuların Azure Event Grid bir bölgeden başka bir bölgeye nasıl taşınacağı gösterilmektedir.
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/28/2020
ms.openlocfilehash: d0656a4f6ec1c7431cf7111f786b0f1d779166e3
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89145357"
---
# <a name="move-azure-event-grid-custom-topics-to-another-region"></a>Azure Event Grid özel konuları başka bir bölgeye taşıyın
Birkaç nedenden dolayı kaynaklarınızı başka bir bölgeye taşımak isteyebilirsiniz. Örneğin, yeni bir Azure bölgesinden yararlanarak iç ilke ve idare gereksinimlerini veya kapasite planlama gereksinimlerine yanıt olarak tanışın. 

Bu makalede ele alınan üst düzey adımlar aşağıda verilmiştir: 

- **Özel konu** kaynağını bir Azure Resource Manager şablonuna dışarı aktarın. 

    > [!IMPORTANT]
    > Şablona yalnızca özel konu verilir. Konuya yönelik abonelikler aktarılmaz.
- **Özel konuyu hedef bölgeye dağıtmak için şablonu kullanın** . 
- Hedef bölgede **el ile abonelik oluşturun** . Özel konuyu geçerli bölgedeki bir şablona aktardığınızda yalnızca konu verilir. Abonelikler şablona dahil değildir, bu nedenle hedef bölgede özel konu oluşturulduktan sonra bunları el ile oluşturun. 
- **Dağıtımı doğrulayın**. Özel konunun hedef bölgede oluşturulduğunu doğrulayın. 
- **Taşıma işleminin tamamlanabilmesi**için, kaynak bölgeden özel konuyu silin. 

## <a name="prerequisites"></a>Önkoşullar
- Hızlı başlangıcı doldurun [: özel olayları kaynak bölgedeki Web uç noktasına yönlendir](custom-event-quickstart-portal.md) . Bu makaledeki adımları test edebilmeniz için bu adımı uygulayın. 
- Event Grid hizmetinin hedef bölgede kullanılabildiğinden emin olun. [Bölgeye göre kullanılabilen ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all)görüntüleyin.

## <a name="prepare"></a>Hazırlama
Başlamak için, özel konu için bir Kaynak Yöneticisi şablonu dışarı aktarın. 

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Arama çubuğuna **Event Grid konular**yazın ve sonuçlar listesinden **Event Grid konular** ' ı seçin. 

    :::image type="content" source="./media/move-custom-topics-across-regions/search-topics.png" alt-text="Event Grid konuları arayın ve seçin":::
3. Kaynak Yöneticisi şablonuna aktarmak istediğiniz **konuyu** seçin. 

    :::image type="content" source="./media/move-custom-topics-across-regions/select-custom-topic.png" alt-text="Özel konuyu seçin":::   
4. **Event Grid konu** sayfasında sol menüdeki **Ayarlar** ' ın altında **şablonu dışarı aktar** ' ı seçin ve ardından araç çubuğunda **İndir** ' i seçin. 

    :::image type="content" source="./media/move-custom-topics-across-regions/export-template-download.png" alt-text="Şablonu dışarı aktar-> Indir":::   

    > [!IMPORTANT]
    > Yalnızca konu, şablona verilir. Konu için abonelikler aktarılmaz. Bu nedenle, konuyu hedef bölgeye taşıdıktan sonra konu için abonelikler oluşturmanız gerekir. 
5. Portaldan indirdiğiniz **. zip** dosyasını bulun ve bu dosyayı seçtiğiniz bir klasöre ayıklayın. Bu ZIP dosyası Template ve Parameters JSON dosyalarını içerir. 
1. **template.js** seçtiğiniz bir düzenleyicide açın. 
8. `location` **Konu** kaynağını hedef bölgeye veya konuma güncelleştirin. Konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bir bölgenin kodu, boşluk içermeyen bölge adıdır, örneğin, `West US` eşittir `westus` .

    ```json
    "type": "Microsoft.EventGrid/topics",
    "apiVersion": "2020-06-01",
    "name": "[parameters('topics_mytopic0130_name')]",
    "location": "westus"
    ```
1. Şablonu **kaydedin** . 

## <a name="recreate"></a>Oluştur 
Hedef bölgede özel bir konu oluşturmak için şablonu dağıtın. 

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
    1. **Bölge**için hedef bölgeyi seçin. Var olan bir kaynak grubunu seçtiyseniz, bu ayar salt okunurdur. 
    1. **Konu adı**için, konu için yeni bir ad girin. 
    1. Sayfanın alt kısmındaki **gözden geçir + oluştur** ' u seçin. 
    
        :::image type="content" source="./media/move-custom-topics-across-regions/deploy-template.png" alt-text="Özel dağıtım":::
    1. **Gözden geçir + oluştur** sayfasında ayarları gözden geçirin ve **Oluştur**' u seçin. 

## <a name="verify"></a>Doğrulama

1. Dağıtım başarılı olduktan sonra **Kaynağa Git**' i seçin. 

    :::image type="content" source="./media/move-custom-topics-across-regions/navigate-custom-topic.png" alt-text="Kaynağa git":::
1. Özel konu için **Event Grid konu** sayfasını görtığınızdan emin olun.   
1. Konuya olay göndermek için [özel olayları bir Web uç noktasına yönlendir](custom-event-quickstart-portal.md#send-an-event-to-your-topic) ' i izleyin. Web kancası olay işleyicisinin çağrıldığından emin olun. 

## <a name="discard-or-clean-up"></a>Atma veya temizleme
Taşıma işleminin tamamlanabilmesi için kaynak bölgedeki özel konuyu silin.  

Baştan başlamak istiyorsanız, hedef bölgedeki konuyu silin ve bu makalenin [hazırlama](#prepare) ve [yeniden oluşturma](#recreate) bölümlerinde yer alan adımları yineleyin.

Azure portal kullanarak özel bir konuyu silmek için:

1. Azure portal en üstündeki ara penceresinde, **Event Grid konuları**yazın ve arama sonuçlarından **Event Grid konular** ' ı seçin. 
2. Silinecek konuyu seçin ve araç çubuğundan **Sil** ' i seçin. 
3. Onay sayfasında, kaynak grubunun adını girin ve **Sil**' i seçin.  

Azure portal kullanarak özel konuyu içeren kaynak grubunu silmek için:

1. Azure portal en üstündeki ara penceresinde, **kaynak grupları**yazın ve arama sonuçlarından **kaynak grupları** ' nı seçin. 
2. Silinecek kaynak grubunu seçin ve araç çubuğundan **Sil** ' i seçin. 
3. Onay sayfasında, kaynak grubunun adını girin ve **Sil**' i seçin.  

## <a name="next-steps"></a>Sonraki adımlar
Event Grid özel bir konuyu bir bölgeden başka bir bölgeye nasıl taşıyabileceğinizi öğrendiniz. Sistem konularını, etki alanlarını ve iş ortağı ad alanlarını bölgeler arasında taşımak için aşağıdaki makalelere bakın.

- [Sistem konularını bölgeler arasında taşıyın](move-system-topics-across-regions.md). 
- [Etki alanlarını bölgeler arasında taşıyın](move-domains-across-regions.md). 
- [İş ortağı ad alanlarını bölgeler arasında taşıyın](move-partner-namespaces-across-regions.md).

Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın: [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md).