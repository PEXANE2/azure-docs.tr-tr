---
title: Azure Event Hubs adanmış bir kümeyi başka bir bölgeye taşıma | Microsoft Docs
description: Bu makalede, Azure Event Hubs adanmış bir kümeyi geçerli bölgeden başka bir bölgeye nasıl taşıyacağınız gösterilmektedir.
ms.topic: how-to
ms.date: 09/01/2020
ms.openlocfilehash: 94be44ee8f9442a3a65e899d7a58524b2570f194
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89381090"
---
# <a name="move-an-azure-event-hubs-dedicated-cluster-to-another-region"></a>Azure Event Hubs adanmış bir kümeyi başka bir bölgeye taşıma
Bu makalede, var olan bir Event Hubs adanmış küme için Azure Resource Manager şablonunun nasıl dışarı aktarılacağı ve sonra başka bir bölgede aynı yapılandırma ayarlarına sahip bir küme oluşturmak için şablonunun nasıl kullanılacağı gösterilmektedir. 

Event Hubs kümesini içeren Azure Kaynak grubundaki ad alanları ve Olay Hub 'ları gibi başka kaynaklarınız varsa, tüm ilgili kaynakların tek bir adımda yeni bölgeye taşınabilmesi için şablonu kaynak grubu düzeyinde dışarı aktarmak isteyebilirsiniz. Bu makaledeki adımlarda, şablona bir **Event Hubs kümesi** dışarı aktarma işlemi gösterilmektedir. Bir **kaynak grubunu** şablona aktarma adımları benzerdir. 

## <a name="prerequisites"></a>Ön koşullar
Adanmış kümenin hedef bölgede oluşturulabilmek için emin olun. Bulmanın en kolay yolu, [Event Hubs adanmış bir küme oluşturmayı](event-hubs-dedicated-cluster-create-portal.md)denemek için Azure Portal kullanmaktır. Kümeyi oluşturmak için bu süre içinde desteklenen bölgelerin listesini görürsünüz. 

## <a name="prepare"></a>Hazırlama
Başlamak için Kaynak Yöneticisi şablonu dışarı aktarın. Bu şablon Event Hubs adanmış kümenizi tanımlayan ayarları içerir.

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **Tüm kaynaklar** ' ı seçin ve ardından Event Hubs adanmış kümenizi seçin.
3. > **ayarları**  >  **dışarı aktarma şablonu**' nu seçin.
4. **Şablonu dışarı aktar** sayfasında **İndir** ' i seçin.

    :::image type="content" source="./media/move-cluster-across-regions/download-template.png" alt-text="Kaynak Yöneticisi şablonu indir" lightbox="./media/move-cluster-across-regions/download-template.png":::
5. Portaldan indirdiğiniz. zip dosyasını bulun ve bu dosyayı seçtiğiniz bir klasöre ayıklayın.

   Bu ZIP dosyası, şablonu dağıtmak için şablon ve betikleri içeren. JSON dosyalarını içerir.


## <a name="move"></a>Taşı

Hedef bölgede Event Hubs adanmış bir küme oluşturmak için şablonu dağıtın. 


1. Azure portal **kaynak oluştur**' u seçin.
2. **Market 'Te arama**yapın, **şablon dağıtımı**yazın ve **şablon dağıtımı (özel şablonları kullanarak dağıtın)** öğesini seçin.
5. **Düzenleyicide kendi şablonunuzu oluşturun öğesini**seçin.
6. **Dosya Yükle**' yi seçin ve ardından son bölümde indirdiğiniz dosyanın **template.js** yüklemek için yönergeleri izleyin.
1. `location`Özelliğin değerini yeni bölgeyi gösterecek şekilde güncelleştirin. Konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bir bölgenin kodu, boşluk içermeyen bölge adıdır, örneğin, `West US` eşittir `westus` .
1. Şablonu kaydetmek için **Kaydet** ' i seçin. 
1. **Özel dağıtım** sayfasında, aşağıdaki adımları izleyin: 
    1. Bir Azure **aboneliği**seçin. 
    2. Var olan bir **kaynak grubunu** seçin veya bir kaynak grubu oluşturun. 
    3. Hedef **konumu** veya bölgeyi seçin. Var olan bir kaynak grubunu seçtiyseniz, bu ayar salt okunurdur. 
    4. **Ayarlar** bölümünde aşağıdaki adımları uygulayın:    
        1. Yeni **küme adını**girin. 

            :::image type="content" source="./media/move-cluster-across-regions/deploy-template.png" alt-text="Kaynak Yöneticisi şablonu dağıtma":::
    5. Sayfanın alt kısmındaki **gözden geçir + oluştur** ' u seçin. 
    1. **Gözden geçir + oluştur** sayfasında ayarları gözden geçirin ve ardından **Oluştur**' u seçin.  

## <a name="discard-or-clean-up"></a>Atma veya temizleme
Dağıtımdan sonra, baştan başlamak istiyorsanız, **hedef Event Hubs adanmış kümeyi**silebilir ve bu makalenin [hazırlama](#prepare) ve [taşıma](#move) bölümünde açıklanan adımları yineleyebilirsiniz.

Değişiklikleri uygulamak ve bir Event Hubs kümesinin taşınmasını tamamlamak için özgün bölgedeki **Event Hubs kümesini** silin. 

Azure portal kullanarak bir Event Hubs kümesini (kaynak veya hedef) silmek için:

1. Azure portal en üstündeki ara penceresinde **Event Hubs kümeler**yazın ve arama sonuçlarından **Event Hubs kümeler** ' ı seçin. Event Hubs kümeyi bir listede görürsünüz.
2. Silinecek kümeyi seçin ve araç çubuğundan **Sil** ' i seçin. 
3. **Kümeyi Sil** sayfasında, **küme adını**yazarak silme Işlemini onaylayın ve sonra **Sil**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Event Hubs adanmış bir kümeyi bir bölgeden diğerine taşımayı öğrendiniz. 

Bir ad alanını bir bölgeden başka bir bölgeye taşıma yönergeleri için bkz. [Event Hubs ad alanlarını bölgeler arasında taşıma](move-across-regions.md) . 

Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:

- [Kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md)
- [Azure VM’lerini başka bir bölgeye taşıma](../site-recovery/azure-to-azure-tutorial-migrate.md)
