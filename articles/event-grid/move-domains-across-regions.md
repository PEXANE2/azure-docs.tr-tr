---
title: Azure Event Grid etki alanlarını başka bir bölgeye taşıyın
description: Bu makalede Azure Event Grid etki alanlarını bir bölgeden başka bir bölgeye nasıl taşıyacağınız gösterilmektedir.
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/20/2020
ms.openlocfilehash: fff8638a819511f84f15c52ad0695cdd5759f971
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89087214"
---
# <a name="move-azure-event-grid-domains-to-another-region"></a>Azure Event Grid etki alanlarını başka bir bölgeye taşıyın
Birkaç nedenden dolayı kaynaklarınızı başka bir bölgeye taşımak isteyebilirsiniz. Örneğin, yeni bir Azure bölgesinden yararlanarak iç ilke ve idare gereksinimlerini veya kapasite planlama gereksinimlerine yanıt olarak tanışın. 

Bu makalede ele alınan üst düzey adımlar aşağıda verilmiştir: 

- **Etki alanı** kaynağını bir Azure Resource Manager şablonuna dışarı aktarın. 

    > [!IMPORTANT]
    > Etki alanındaki etki alanı kaynağı ve konuları şablona aktarılmalıdır. Etki alanı konularındaki abonelikler aktarılmaz. 
- **Etki alanını hedef bölgeye dağıtmak için şablonu kullanın** . 
- Hedef bölgede **el ile etki alanı konuları için abonelikler oluşturun** . Etki alanını geçerli bölgedeki bir şablona aktardığınızda, etki alanı konularındaki abonelikler aktarılmaz. Bu nedenle, etki alanı ve etki alanı konuları hedef bölgede oluşturulduktan sonra bunları oluşturun. 
- **Dağıtımı doğrulayın**. Etki alanındaki bir etki alanı konusuna olay gönderin ve abonelikle ilişkili olay işleyicisinin çağrılmakta olduğunu doğrulayın. 
- **Taşıma işleminin tamamlanabilmesi**için kaynak bölgedeki etki alanını silin. 

## <a name="prerequisites"></a>Önkoşullar
- Event Grid hizmetinin hedef bölgede kullanılabildiğinden emin olun. [Bölgeye göre kullanılabilen ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all)görüntüleyin.

## <a name="prepare"></a>Hazırlama
Başlamak için etki alanı için bir Kaynak Yöneticisi şablonu dışarı aktarın. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Arama çubuğuna **Event Grid etki alanları**yazın ve sonuçlar listesinden **Event Grid etki alanları** ' nı seçin. 

    :::image type="content" source="./media/move-domains-across-regions/search-domains.png" alt-text="Event Grid etki alanlarını arayın ve seçin":::
3. Kaynak Yöneticisi şablonuna aktarmak istediğiniz **etki alanını** seçin. 

    :::image type="content" source="./media/move-domains-across-regions/select-domain.png" alt-text="Etki alanını seçin":::   
4. **Event Grid etki alanı** sayfasında, Sol menüdeki **Ayarlar** ' ın altında **şablonu dışarı aktar** ' ı seçin ve ardından araç çubuğunda **İndir** ' i seçin. 

    :::image type="content" source="./media/move-domains-across-regions/export-template-download.png" alt-text="Şablonu dışarı aktar-> Indir" lightbox="./media/move-domains-across-regions/export-template-download.png":::   

    > [!IMPORTANT]
    > Etki alanı ve etki alanı konuları verilir. Etki alanı konuları için abonelikler aktarılmaz. Bu nedenle, etki alanı konularını taşıdıktan sonra etki alanı konuları için abonelikler oluşturmanız gerekir. 
5. Portaldan indirdiğiniz **. zip** dosyasını bulun ve bu dosyayı seçtiğiniz bir klasöre ayıklayın. Bu ZIP dosyası Template ve Parameters JSON dosyalarını içerir. 
1. **template.js** seçtiğiniz bir düzenleyicide açın. 
8. `location` **Etki alanı** kaynağı için hedef bölgeye veya konuma güncelleştirin. Konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bir bölgenin kodu, boşluk içermeyen bölge adıdır, örneğin, `West US` eşittir `westus` .

    ```json
    "type": "Microsoft.EventGrid/domains",
    "apiVersion": "2020-06-01",
    "name": "[parameters('domains_spegriddomain_name')]",
    "location": "westus",
    ```
1. Şablonu **kaydedin** . 

## <a name="recreate"></a>Oluştur 
Hedef bölgede etki alanı ve etki alanı konularını oluşturmak için şablonu dağıtın. 

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
    1. Etki alanı **adı**için etki alanı için yeni bir ad girin. 
    1. **Gözden geçir ve oluştur**’u seçin. 
    
        :::image type="content" source="./media/move-domains-across-regions/deploy-template.png" alt-text="Şablonu dağıt":::        
    1. Şablonun doğrulanması başarılı olduktan sonra, kaynağı dağıtmak için sayfanın alt kısmındaki **Oluştur** ' u seçin. 
    1. Dağıtım başarılı olduktan sonra kaynak grubu sayfasına gitmek için **kaynak grubuna git** ' i seçin. Kaynak grubunda bir etki alanı olduğunu doğrulayın. Etki alanını seçin. Etki alanında etki alanı konuları olduğunu doğrulayın. 

## <a name="discard-or-clean-up"></a>Atma veya temizleme
Taşıma işleminin tamamlanabilmesi için kaynak bölgedeki etki alanını silin.  

Baştan başlamak istiyorsanız, hedef bölgedeki etki alanını silin ve bu makalenin [hazırlama](#prepare) ve [yeniden oluşturma](#recreate) bölümlerinde yer alan adımları yineleyin.

Azure portal kullanarak bir etki alanını silmek için:

1. Azure portal en üstündeki ara penceresinde, **Event Grid etki alanları**yazın ve arama sonuçlarından **Event Grid etki alanları** ' nı seçin. 
2. Silinecek etki alanını seçin ve araç çubuğundan **Sil** ' i seçin. 
3. Onay sayfasında, kaynak grubunun adını girin ve **Sil**' i seçin.  

Azure portal kullanarak etki alanını içeren kaynak grubunu silmek için:

1. Azure portal en üstündeki ara penceresinde, **kaynak grupları**yazın ve arama sonuçlarından **kaynak grupları** ' nı seçin. 
2. Silinecek kaynak grubunu seçin ve araç çubuğundan **Sil** ' i seçin. 
3. Onay sayfasında, kaynak grubunun adını girin ve **Sil**' i seçin.  

## <a name="next-steps"></a>Sonraki adımlar
Bir Event Grid etki alanını bir bölgeden başka bir bölgeye taşımayı öğrendiniz. Sistem konularını, özel konuları ve iş ortağı ad alanlarını bölgeler arasında taşımak için aşağıdaki makalelere bakın.

- [Sistem konularını bölgeler arasında taşıyın](move-system-topics-across-regions.md). 
- [Özel konuları bölgeler arasında taşıyın](move-custom-topics-across-regions.md). 
- [İş ortağı ad alanlarını bölgeler arasında taşıyın](move-partner-namespaces-across-regions.md).

Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın: [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md).