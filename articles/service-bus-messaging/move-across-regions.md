---
title: Bir Service Bus ad alanını başka bir bölgeye taşıma | Microsoft Docs
description: Bu makalede, bir Azure Service Bus ad alanını geçerli bölgeden başka bir bölgeye nasıl taşıyacağınız gösterilmektedir.
ms.topic: how-to
ms.date: 06/23/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 573ea96437cf6cb76854ffa417fd3ad3fb86138b
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861259"
---
# <a name="move-an-azure-service-bus-namespace-to-another-region"></a>Bir Azure Service Bus ad alanını başka bir bölgeye taşıma
Mevcut Service Bus ad alanınızı bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, test için aynı yapılandırmaya sahip bir ad alanı oluşturmak isteyebilirsiniz. Ayrıca [olağanüstü durum kurtarma planlamasının](service-bus-geo-dr.md)bir parçası olarak başka bir bölgede ikincil bir ad alanı oluşturmak isteyebilirsiniz.

Üst düzey adımlar şunlardır:

1. Geçerli bölgedeki Service Bus ad alanını bir Azure Resource Manager şablonuna dışarı aktarın. 
1. Şablondaki kaynakların konumunu güncelleştirin. Ayrıca, sizin için otomatik olarak oluşturulduğu için varsayılan bir kural oluşturamadığı için varsayılan abonelik filtresini şablondan kaldırın. 
1. Service Bus ad alanını hedef bölgeye dağıtmak için şablonu kullanın. 
1. Konuların ad alanı, kuyrukları, konuları ve aboneliklerinin hedef bölgede oluşturulduğundan emin olmak için dağıtımı doğrulayın. 
1. Tüm iletileri işledikten sonra, kaynak bölgede ad alanını silerek taşımayı doldurun. 

## <a name="prerequisites"></a>Önkoşullar
Hesabınızın kullandığı Azure Service Bus ve özelliklerin hedef bölgede desteklendiğinden emin olun.
 
## <a name="prepare"></a>Hazırlama
Başlamak için Kaynak Yöneticisi şablonu dışarı aktarın. Bu şablon Service Bus ad alanınızı tanımlayan ayarları içerir.

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **Tüm kaynaklar** ' ı seçin ve sonra Service Bus ad alanınızı seçin.
3. > **ayarları**  >  **dışarı aktarma şablonu**' nu seçin.
4. **Şablonu dışarı aktar** sayfasında **İndir** ' i seçin.

    :::image type="content" source="./media/move-across-regions/download-template.png" alt-text="Kaynak Yöneticisi şablonu indir":::
5. Portaldan indirdiğiniz. zip dosyasını bulun ve bu dosyayı seçtiğiniz bir klasöre ayıklayın. Bu ZIP dosyası Template ve Parameters JSON dosyalarını içerir. 
1. Ayıklanan klasörde dosya template.jsaçın. 
1. Özelliği için arama `location` yapın ve değeri bölge veya konum için yeni adla değiştirin. Konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bir bölgenin kodu, boşluk içermeyen bölge adıdır, örneğin, `West US` eşittir `westus` .
1. Şu türdeki kaynakların tanımlarını kaldırın: `Microsoft.ServiceBus/namespaces/topics/subscriptions/rules` . `,`JSON 'ın geçerli kalmasını sağlamak için bu bölümden önce gelen virgül () karakterini kaldırmayı unutmayın.  

    > [!NOTE]
    > Kaynak Yöneticisi şablonu kullanarak bir abonelik için varsayılan bir kural oluşturamazsınız. Varsayılan kural, abonelik hedef bölgede oluşturulduğunda otomatik olarak oluşturulur. 

## <a name="move"></a>Taşı
Hedef bölgede bir Service Bus ad alanı oluşturmak için şablonu dağıtın. 

1. Azure portal **kaynak oluştur**' u seçin.
2. **Market 'Te arama**yapmak için, arama metni için **şablon dağıtımı** yazın, **şablon dağıtımı (özel şablonları kullanarak dağıtın)** öğesini seçin ve ardından **ENTER**tuşuna basın.

    :::image type="content" source="./media/move-across-regions/new-template-deployment.png" alt-text="Yeni şablon dağıtımı":::    
1. **Şablon dağıtımı** sayfasında **Oluştur**' u seçin.

    :::image type="content" source="./media/move-across-regions/template-deployment-create-button.png" alt-text="Yeni şablon dağıtımı-oluştur düğmesi":::        
1. **Özel dağıtım** sayfasında, **düzenleyicide kendi şablonunuzu oluştur**' u seçin.

    :::image type="content" source="./media/move-across-regions/build-template-link.png" alt-text="Düzenleyicide kendi şablonunuzu oluşturun-bağlantı":::            
1. **Şablonu Düzenle** sayfasında, araç çubuğunda **Dosya Yükle** ' yi seçin ve ardından son bölümde indirdiğiniz dosyanın **template.js** yüklemek için yönergeleri izleyin.

    :::image type="content" source="./media/move-across-regions/select-template.png" alt-text="Şablon seç":::                
1. Şablonu kaydetmek için **Kaydet** ' i seçin. 

    :::image type="content" source="./media/move-across-regions/save-template.png" alt-text="Şablonu Kaydet":::                    
1. **Özel dağıtım** sayfasında, aşağıdaki adımları izleyin: 
    1. Bir Azure **aboneliği**seçin. 
    2. Var olan bir **kaynak grubunu** seçin veya bir kaynak grubu oluşturun. 
    3. Hedef **konumu** veya bölgeyi seçin. Var olan bir kaynak grubunu seçtiyseniz, bu ayar salt okunurdur. 
    4. **Ad alanı için**yeni bir ad girin.
    1. **Gözden geçir ve oluştur**’u seçin. 

        :::image type="content" source="./media/move-across-regions/deploy-template.png" alt-text="Kaynak Yöneticisi şablonu dağıtma":::
    1. **Gözden geçir + oluştur** sayfasında sayfanın alt kısmında **Oluştur** ' u seçin. 
    
## <a name="verify"></a>Doğrulama
1. Dağıtım başarılı olduktan sonra **kaynak grubuna git**' i seçin.

    :::image type="content" source="./media/move-across-regions/resource-group-navigation-link.png" alt-text="Kaynak grubu bağlantısına git":::    
1. **Kaynak grubu** sayfasında Service Bus ad alanını seçin. 

    :::image type="content" source="./media/move-across-regions/select-namespace.png" alt-text="Service Bus ad alanını seçin":::    
1. **Service Bus ad alanı** sayfasında, kaynak bölgeden kuyrukları, konuları ve abonelikleri gördiğinizi doğrulayın. 
    1. Sağ bölmenin altındaki ad alanındaki **kuyrukları** görürsünüz.         
    
        :::image type="content" source="./media/move-across-regions/queue-namespace.png" alt-text="Ad alanındaki kuyruklar":::
    2. Ad alanındaki konuları görmek için **konular** sekmesine geçin
    
        :::image type="content" source="./media/move-across-regions/topics-namespace.png" alt-text="Ad alanındaki konular":::
    3. Aboneliklerin oluşturulduğunu doğrulamak için konu başlığını seçin. 

        :::image type="content" source="./media/move-across-regions/topic-subscriptions.png" alt-text="Konu abonelikleri":::      
    
    

## <a name="discard-or-clean-up"></a>Atma veya temizleme
Dağıtımdan sonra, baştan başlamak istiyorsanız, **hedef Service Bus ad alanını**silebilir ve bu makalenin [hazırlama](#prepare) ve [taşıma](#move) bölümünde açıklanan adımları yineleyebilirsiniz.

Değişiklikleri uygulamak ve bir Service Bus ad alanını taşımayı tamamlamak için, **kaynak Service Bus ad alanını**silin. Ad alanını silmeden önce tüm iletileri işlediğinizden emin olun. 

Azure portal kullanarak bir Service Bus ad alanını (kaynak veya hedef) silmek için:

1. Azure portal en üstündeki ara penceresinde **Service Bus**yazın ve arama sonuçlarından **Service Bus** ' yı seçin. Service Bus ad alanlarını bir listede görürsünüz.
2. Silinecek hedef ad alanını seçin ve araç çubuğundan **Sil** ' i seçin. 

    ![Ad alanını sil-düğme](./media/move-across-regions/delete-namespace-button.png)
3. **Kaynakları Sil** sayfasında, seçilen kaynakları doğrulayın ve **Evet**yazarak silme Işlemini onaylayın ve sonra **Sil**' i seçin. 

    Diğer seçenek de Service Bus ad alanına sahip kaynak grubunu silmektir. **Kaynak grubu** sayfasında, araç çubuğunda **kaynak grubunu sil** ' i seçin ve sonra silme işlemini onaylayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Service Bus ad alanını bir bölgeden diğerine taşıdı ve kaynak kaynakları temizledi.  Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:

- [Kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md)
