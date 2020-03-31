---
title: Olay odaklı makine öğrenimi iş akışları oluşturma
titleSuffix: Azure Machine Learning
description: Etkinlik odaklı çözümleri etkinleştirmek için Azure Machine Learning ile etkinlik ızgarasını nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shipatel
author: shivp950
ms.reviewer: larryfr
ms.date: 03/11/2020
ms.openlocfilehash: fe6125682f669e453100488b7e0afc4c49409588
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79129702"
---
# <a name="create-event-driven-machine-learning-workflows-preview"></a>Olay odaklı makine öğrenimi iş akışları oluşturma (Önizleme)

[Azure Olay Ağı,](https://docs.microsoft.com/azure/event-grid/) Azure Machine Learning etkinliklerini destekler. Çalışma alanı içinde çalışma durumu değiştirildi, çalıştırma tamamlandı, model kaydı, model dağıtımı ve veri kayması algılama gibi olayları abone olabilir ve tüketebilirsiniz.

Etkinlik türleri hakkında daha fazla bilgi için, Olay Ağı ve [Azure Machine Learning olay ızgara şeması](/azure/event-grid/event-schema-machine-learning)ile Azure Machine Learning [tümleştirmesi'ne](concept-event-grid-integration.md) bakın.

Şu gibi yaygın senaryoları etkinleştirmek için Olay Izgarasını kullanın:

* Çalışma hatası ve çalıştırma tamamlandıktan sonra e-posta gönderme
* Model kaydedildikten sonra azure işlevi kullanma
* Azure Machine Learning'den çeşitli uç noktalara olayları akışla aktarın
* Sürüklenme algılandığında bir ML ardışık hattını tetikleme

> [!NOTE] 
> Şu anda, runStatusChanged olayları yalnızca çalışma durumu **başarısız** olduğunda tetiklenir
>

## <a name="prerequisites"></a>Ön koşullar
* Etkinlikler oluşturacağınız Azure Machine Learning çalışma alanına katkıda bulunan veya sahip erişimi.

### <a name="configure-eventgrid-using-the-azure-portal"></a>Azure portalını kullanarak EventGrid'i yapılandırın

1. Azure [portalını](https://portal.azure.com) açın ve Azure Machine Learning çalışma alanınıza gidin.

1. Sol çubuğundan __Etkinlikler'i__ seçin ve ardından **Etkinlik Abonelikleri'ni**seçin. 

    ![select-events-in-workspace.png](./media/how-to-use-event-grid/select-event.png)

1. Tüketilecek olay türünü seçin. Örneğin, aşağıdaki ekran görüntüsü __Model kayıtlı__seçti , Model __dağıtıldı__, __Tamamlandı ,__ ve __Dataset sürüklenme algılandı__:

    ![ekle-olay türü](./media/how-to-use-event-grid/add-event-type-updated.png)

1. Olayı yayınlamak için bitiş noktasını seçin. Aşağıdaki ekran görüntüsünde, __Olay hub'ı__ seçilen bitiş noktasıdır:

    ![select-event işleyicisi](./media/how-to-use-event-grid/select-event-handler.png)

Seçiminizi onayladıktan sonra __Oluştur'u__tıklatın. Yapılandırmadan sonra, bu olaylar bitiş noktanıza itilir.


### <a name="configure-eventgrid-using-the-cli"></a>CLI kullanarak EventGrid'i yapılandır

En son Azure [CLI'sini](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)yükleyebilir veya Azure aboneliğinizin bir parçası olarak sağlanan Azure Bulut Su Larını kullanabilirsiniz.

Olay Izgara uzantısını yüklemek için CLI'den aşağıdaki komutu kullanın:

```azurecli-interactive
az add extension --name eventgrid
```

Aşağıdaki örnek, bir Azure aboneliğinin nasıl seçilebildiğini gösterir ve Azure Machine Learning için yeni bir etkinlik aboneliği oluşturur:

```azurecli-interactive
# Select the Azure subscription that contains the workspace
az account set --subscription "<name or ID of the subscription>"

# Subscribe to the machine learning workspace.
az eventgrid event-subscription create \
  --name {eventGridFilterName} \
  --source-resource-id "/subscriptions/{subId}/resourceGroups/{rgName}/ \providers/Microsoft.MachineLearningServices/workspaces/{wsName}" \
  --endpoint {event handler endpoint} \
  --included-event-types Microsoft.MachineLearningServices.ModelRegistered \
  --subject-begins-with "models/mymodelname"
```

## <a name="filter-events"></a>Filtre Olayları

Etkinliklerinizi ayarlarken, yalnızca belirli olay verilerini tetiklemek için filtreler uygulayabilirsiniz. Aşağıdaki örnekte, çalışma durumu değiştirilen olaylar için, çalıştırma türlerine göre filtre uygulayabilirsiniz. Olay yalnızca ölçütler karşılandığında tetikler. Filtre uygulayabileceğiniz olay verileri hakkında bilgi edinmek için [Azure Machine Learning olay ızgara şemasına](/azure/event-grid/event-schema-machine-learning) bakın. 

1. Azure portalına gidin, yeni bir abonelik veya varolan bir abonelik seçin. 

1. Filtreler sekmesini seçin ve Gelişmiş filtrelere gidin. **Anahtar** ve **Değer'de** filtrelemek istediğiniz özellik türlerini sağlayın. Burada olay yalnızca çalıştırılacak tür bir ardışık hat lar veya ardışık hatlar adımı çalıştırıldığında tetiklenir görebilirsiniz.  

    :::image type="content" source="media/how-to-use-event-grid/select-event-filters.png" alt-text="filtre olayları":::

## <a name="sample-scenarios"></a>Örnek senaryolar

### <a name="use-a-logic-app-to-send-email-alerts"></a>E-posta uyarıları göndermek için Mantık Uygulaması kullanma

Tüm etkinlikleriniz için e-postaları yapılandırmak için [Azure Logic Apps'dan](https://docs.microsoft.com/azure/logic-apps/) yararlanın. Birlikte çalışan ekipler arasında işbirliği ve farkındalığı etkinleştirmek için koşullarla özelleştirin ve alıcıları belirtin.

1. Azure portalında Azure Machine Learning çalışma alanınıza gidin ve sol çubuktan etkinlikler sekmesini seçin. Buradan Mantık __uygulamalarını__seçin. 

    ![select-logic-ap](./media/how-to-use-event-grid/select-logic-ap.png)

1. Logic App UI'de oturum açın ve konu türü olarak Machine Learning hizmetini seçin. 

    ![seçin-konu türü](./media/how-to-use-event-grid/select-topic-type.png)

1. Hangi olay(lar) için bilgilendirileceğini seçin. Örneğin, aşağıdaki ekran görüntüsü __RunCompleted__.

    ![select-event-runcomplete](./media/how-to-use-event-grid/select-event-runcomplete.png)

1. Yukarıdaki bölümde filtreleme yöntemini kullanabilir veya yalnızca olay türlerinin bir alt kümesindeki mantık uygulamasını tetiklemek için filtre ekleyebilirsiniz. Aşağıdaki ekran görüntüsünde __/datadriftID/runs/__ __önek filtresi__ kullanılır.

    ![filtre olayları](./media/how-to-use-event-grid/filtering-events.png)

1. Ardından, bu olayı tüketmek ve e-posta aramak için bir adım ekleyin. Olayları almak için kullanabileceğiniz birkaç farklı posta hesabı vardır. Ayrıca, e-posta uyarısı ne zaman gönderebileceğinizi de yapılandırabilirsiniz.

    ![select-e-posta eylemi](./media/how-to-use-event-grid/select-email-action.png)

1. __E-posta gönder'i__ seçin ve parametreleri doldurun. Konuya, olayları filtrelemede yardımcı olmak için __Olay Türü__ ve __Konu'yu__ ekleyebilirsiniz. İleti gövdesinde çalıştırmalar için çalışma alanı sayfasına bir bağlantı da ekleyebilirsiniz. 

    ![yapılandırma-e-posta-gövde](./media/how-to-use-event-grid/configure-email-body.png)

1. Bu eylemi kaydetmek için sayfanın sol köşesinde **Kis kaydet'i** seçin. Görünen sağ çubuktan, bu eylemin oluşturulmasını onaylayın.

    ![onaylamak-mantık-app-oluşturma](./media/how-to-use-event-grid/confirm-logic-app-create.png)


### <a name="use-a-logic-app-to-trigger-retraining-workflows-when-data-drift-occurs"></a>Veri kayması oluştuğunda yeniden eğitim iş akışlarını tetiklemek için Bir Mantık Uygulaması kullanın

Modeller zaman içinde bayatlar ve içinde çalışmakta olduğu bağlamda yararlı kalmaz. Modeli yeniden eğitmenin zamanının geldiğini söylemenin bir yolu veri sürüklenmesini algılamaktır. 

Bu örnek, yeniden eğitimi tetiklemek için Azure Logic Uygulamasıyla olay ızgarasını nasıl kullanacağımı gösterir. Örnek, bir modelin eğitimi ile hizmet veren veri kümeleri arasında veri kayması meydana geldiğinde bir Azure Veri Fabrikası ardışık hattını tetikler.

Başlamadan önce aşağıdaki eylemleri gerçekleştirin:

* Çalışma alanında [veri sürüklenme sini algılamak]( https://aka.ms/datadrift) için bir veri kümesi monitörü ayarlama
* Yayımlanmış bir [Azure Veri Fabrikası ardışık hattı](https://docs.microsoft.com/azure/data-factory/)oluşturun.

Bu örnekte, dosyaları bir blob deposuna kopyalamak ve yayımlanmış bir Machine Learning ardışık hattını çalıştırmak için basit bir Veri Fabrikası ardışık alanı kullanılır. Bu senaryo hakkında daha fazla bilgi için [Azure Veri Fabrikası'nda Makine Öğrenimi adımLarını](https://docs.microsoft.com/azure/data-factory/transform-data-machine-learning-service) nasıl ayarlayınız

![adf-mlpipeline-sahne](./media/how-to-use-event-grid/adf-mlpipeline-stage.png)

1. Mantık uygulamasını oluşturarak başlayın. [Azure portalına](https://portal.azure.com)gidin, Mantık Uygulamaları'nı arayın ve oluştur'u seçin.

    ![arama-mantık-uygulaması](./media/how-to-use-event-grid/search-for-logic-app.png)

1. İstenen bilgileri doldurun. Deneyimi basitleştirmek için, Azure Veri Fabrikası Ardışık Hattınız ve Azure Machine Learning çalışma alanınızla aynı abonelik ve kaynak grubunu kullanın.

    ![adf için kurulum-mantık-uygulama-](./media/how-to-use-event-grid/set-up-logic-app-for-adf.png)

1. Mantık uygulamasını oluşturduktan sonra, __Olay Izgara kaynak olayı oluştuğunda__seçin. 

    ![select-event-grid-trigger](./media/how-to-use-event-grid/select-event-grid-trigger.png)

1. Giriş yapın ve etkinliğin ayrıntılarını doldurun. Kaynak __Adı'nı__ çalışma alanı adına ayarlayın. Olay __Türünü__ __DatasetDriftDetected__olarak ayarlayın.

    ![giriş ve ekle-olay](./media/how-to-use-event-grid/login-and-add-event.png)

1. Yeni bir adım ekleyin ve __Azure Veri Fabrikası'nı__arayın. __Bir ardışık hat lar çalıştır'ı oluştur'u__seçin. 

    ![create-adfpipeline-run](./media/how-to-use-event-grid/create-adfpipeline-run.png)

1. Oturum açın ve yayınlanacak olan Azure Veri Fabrikası ardışık hattını belirtin.

    ![belirt-adf-boru hattı](./media/how-to-use-event-grid/specify-adf-pipeline.png)

1. Sayfanın sol üst kısmındaki **kaydet** düğmesini kullanarak mantık uygulamasını kaydedin ve oluşturun. Uygulamanızı görüntülemek için [Azure portalındaki](https://portal.azure.com) çalışma alanınıza gidin ve **Etkinlikler'e**tıklayın.

    ![show-logic-app-webhook](./media/how-to-use-event-grid/show-logic-app-webhook.png)

Şimdi veri fabrikası boru hattı sürüklenme meydana geldiğinde tetiklenir. [Yeni çalışma alanı portalında](https://ml.azure.com)veri kayması ve makine öğrenimi boru hattınızdaki ayrıntıları görüntüleyin. 

![çalışma alanı içinde görüntüle](./media/how-to-use-event-grid/view-in-workspace.png)

### <a name="use-azure-functions-to-deploy-a-model-based-on-tags"></a>Etiketlere dayalı bir model dağıtmak için Azure Işlevlerini kullanma

Azure Machine Learning modeli nesnesi, dağıtımları model adı, sürüm, etiket ve özellik gibi döndürme parametreleri içerir. Model kaydı olayı bir bitiş noktası tetikleyebilir ve bu parametrelerin değerini temel alan bir modeli dağıtmak için bir Azure İşlevi kullanabilirsiniz.

Örneğin, depoya [https://github.com/Azure-Samples/MachineLearningSamples-NoCodeDeploymentTriggeredByEventGrid](https://github.com/Azure-Samples/MachineLearningSamples-NoCodeDeploymentTriggeredByEventGrid) bakın ve **okuma dosyasındaki** adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Kullanılabilir etkinlikler hakkında daha fazla bilgi edinmek için [Azure Machine Learning etkinlik şemasına](/azure/event-grid/event-schema-machine-learning) bakın
