---
title: Azure Işlevleri dağıtım Yuvaları
description: Azure Işlevleri ile dağıtım yuvaları oluşturmayı ve kullanmayı öğrenin
author: craigshoemaker
ms.topic: reference
ms.date: 04/15/2020
ms.author: cshoe
ms.openlocfilehash: 7cfbd533921ba4d1757e7415a3bb8f70aeb71251
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83122604"
---
# <a name="azure-functions-deployment-slots"></a>Azure Işlevleri dağıtım Yuvaları

Azure Işlevleri dağıtım yuvaları, işlev uygulamanızın "yuvalar" adlı farklı örnekleri çalıştırmasına izin verir. Yuvalar, genel kullanıma açık bir uç nokta aracılığıyla kullanıma sunulan farklı ortamlardır. Bir uygulama örneği her zaman üretim yuvasına eşlenir ve isteğe bağlı bir yuvaya atanmış örnekleri takas edebilirsiniz. Uygulamalar hizmet planı altında çalışan işlev uygulamalarının birden çok yuvası olabilir, ancak tüketim planı kapsamında yalnızca bir yuva kullanılabilir.

Aşağıdaki, işlevlerin takas yuvaları tarafından nasıl etkilendiğini yansıtır:

- Trafik yeniden yönlendirmesi sorunsuz; bir değiştirme nedeniyle hiçbir istek atılamaz.
- Bir işlev değiştirme sırasında çalışıyorsa, yürütme devam eder ve sonraki Tetikleyiciler, takas edilen uygulama örneğine yönlendirilir.

> [!NOTE]
> Yuvalar Şu anda Linux tüketim planı için kullanılamaz.

## <a name="why-use-slots"></a>Yuvalar neden kullanılmalıdır?

Dağıtım yuvalarını kullanmanın bazı avantajları vardır. Aşağıdaki senaryolar yuvalar için ortak kullanımları anlatmaktadır:

- Farklı **amaçlar Için farklı ortamlar**: farklı yuvaların kullanılması, üretim veya hazırlama yuvasına geçmeden önce uygulama örneklerini ayırt etme fırsatına sahip olmanızı sağlar.
- **Ön ısıtma**: doğrudan üretime dağıtmak yerine bir yuvaya dağıtım, uygulamanın canlı olmadan önce daha sıcak olmasına olanak sağlar. Ayrıca, yuva kullanımı, HTTP ile tetiklenen iş yükleri için gecikme süresini azaltır. Örnekler dağıtımdan önce azaltılır, bu da yeni dağıtılan işlevlerin soğuk başlangıcını azaltır.
- **Kolay azalma**: bir değiştirme işleminden sonra, daha önce hazırlanmış bir uygulamaya sahip yuva artık önceki üretim uygulamasına sahiptir. Üretim yuvasında takas edilen değişiklikler beklenmediği sürece, "son bilinen iyi örnek örneğinizi" geri almak için değiştirmeyi hemen ters çevirebilirsiniz.

## <a name="swap-operations"></a>Değiştirme işlemleri

Bir değiştirme sırasında, bir yuva kaynak ve diğer hedef olarak değerlendirilir. Kaynak yuva, hedef yuvaya uygulanan uygulamanın örneğine sahiptir. Aşağıdaki adımlar, hedef yuvanın bir değiştirme sırasında kapalı kalma süresi yaşamasını güvence altına almamalıdır:

1. **Ayarları Uygula:** Hedef yuvadan alınan ayarlar, kaynak yuvasının tüm örneklerine uygulanır. Örneğin, üretim ayarları hazırlama örneğine uygulanır. Uygulanan ayarlar aşağıdaki kategorileri içerir:
    - [Yuvaya özgü](#manage-settings) uygulama ayarları ve bağlantı dizeleri (varsa)
    - [Sürekli dağıtım](../app-service/deploy-continuous-deployment.md) ayarları (etkinse)
    - [App Service kimlik doğrulama](../app-service/overview-authentication-authorization.md) ayarları (etkinse)

1. **Yeniden başlatmalar ve kullanılabilirlik Için bekleyin:** Takas, kaynak yuvadaki her örnek için yeniden başlatma işleminin tamamlanmasını ve istekler için kullanılabilir olmasını bekler. Herhangi bir örnek yeniden başlatılamazsa, değiştirme işlemi kaynak yuvada tüm değişiklikleri geri alır ve işlemi sonlandırır.

1. **Yönlendirmeyi güncelleştir:** Kaynak yuvasındaki tüm örnekler başarıyla iyileştirilirken, iki yuva yönlendirme kurallarını değiştirerek takası tamamlar. Bu adımdan sonra, hedef yuva (örneğin, üretim yuvası), kaynak yuvada daha önce çarpımış olan uygulamaya sahiptir.

1. **Işlemi Yinele:** Artık kaynak yuvasının hedef yuvada daha önce takas öncesi uygulamasına sahip olduğuna göre, tüm ayarları uygulayarak ve kaynak yuva için örnekleri yeniden başlatarak aynı işlemi doldurun.

Aşağıdaki noktaları göz önünde bulundurun:

- Değiştirme işleminin herhangi bir noktasında, takas edilen uygulamaların başlatılması kaynak yuvada gerçekleşir. Kaynak yuva hazırlanırken, değiştirme işleminin başarılı veya başarısız olmasına bakılmaksızın hedef yuva çevrimiçi kalır.

- Bir hazırlama yuvasını üretim yuvası ile değiştirmek için, üretim yuvasının *her zaman* hedef yuva olduğundan emin olun. Bu şekilde, değiştirme işlemi üretim uygulamanızı etkilemez.

- *Değiştirme işlemine başlamadan önce*, olay kaynakları ve bağlamalarla ilgili ayarların [dağıtım yuvası ayarları](#manage-settings) olarak yapılandırılması gerekir. Bu süreyi "yapışkan" olarak işaretlemek, olayların ve çıktıların doğru örneğe yönlendirilmesini sağlar.

## <a name="manage-settings"></a>Ayarları yönetme

[!INCLUDE [app-service-deployment-slots-settings](../../includes/app-service-deployment-slots-settings.md)]

### <a name="create-a-deployment-setting"></a>Dağıtım ayarı oluşturma

Ayarları bir dağıtım ayarı olarak işaretleyebilirsiniz. Bu, "yapışkan" haline gelir. Yapışkan ayar, uygulama örneğiyle takas etmez.

Bir yuvada bir dağıtım ayarı oluşturursanız, bir değiştirme işleminde yer alan diğer tüm yuvalara aynı ayarı oluşturmanız emin olun. Bu şekilde, bir ayarın değeri değişmezse, ayar adları yuvalar arasında tutarlı kalır. Bu ad tutarlılığı, kodunuzun tek bir yuvada tanımlanmış ancak başka bir yuvada tanımlanmış bir ayara erişmeyi denememesini sağlar.

Bir dağıtım ayarı oluşturmak için aşağıdaki adımları kullanın:

1. İşlev uygulamasındaki **dağıtım yuvaları** ' na gidin ve ardından yuva adını seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-navigate-slots.png" alt-text="Azure portal yuvaları bulun." border="true":::

1. **Yapılandırma**' yı seçin ve ardından geçerli yuvaya eklemek istediğiniz ayar adını seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-configure-deployment-slot.png" alt-text="Azure portal bir yuva için uygulama ayarını yapılandırın." border="true":::

1. **Dağıtım yuvası ayarı**' nı seçin ve ardından **Tamam**' ı seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-deployment-slot-setting.png" alt-text="Dağıtım yuvası ayarını yapılandırın." border="true":::

1. Ayar bölümü kaybolduktan sonra değişiklikleri tutmak için **Kaydet** ' i seçin

    :::image type="content" source="./media/functions-deployment-slots/functions-save-deployment-slot-setting.png" alt-text="Dağıtım yuvası ayarını kaydedin." border="true":::

## <a name="deployment"></a>Dağıtım

Yuva oluşturduğunuzda yuvalar boştur. Uygulamanızı bir yuvaya dağıtmak için [Desteklenen Dağıtım teknolojilerinden](./functions-deployment-technologies.md) herhangi birini kullanabilirsiniz.

## <a name="scaling"></a>Ölçeklendirme

Tüm yuvalar, üretim yuvasında aynı çalışan sayısına göre ölçeklenir.

- Tüketim planları için yuva, işlev uygulaması ölçeklendirilen şekilde ölçeklendirilir.
- App Service planlar için, uygulama sabit bir çalışan sayısına göre ölçeklendirilir. Yuvalar, uygulama planıyla aynı sayıda çalışan üzerinde çalışır.

## <a name="add-a-slot"></a>Yuva ekleme

[CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-create) aracılığıyla veya Portal aracılığıyla bir yuva ekleyebilirsiniz. Aşağıdaki adımlarda portalda nasıl yeni bir yuva oluşturacağınız gösterilmektedir:

1. İşlev uygulamanıza gidin.

1. **Dağıtım yuvaları**' nı seçin ve **+ yuva Ekle**' yi seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-deployment-slots-add.png" alt-text="Azure Işlevleri dağıtım yuvası ekleyin." border="true":::

1. Yuvanın adını yazın ve **Ekle**' yi seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-deployment-slots-add-name.png" alt-text="Azure Işlevleri dağıtım yuvasını adlandırın." border="true":::

## <a name="swap-slots"></a>Takas Yuvaları

Yuvaları [CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-swap) aracılığıyla veya Portal üzerinden takas edebilirsiniz. Aşağıdaki adımlarda, portalda yuvaların nasıl takas yapılacağı gösterilmektedir:

1. İşlev uygulamasına gidin.
1. **Dağıtım yuvaları**' nı seçin ve ardından **Değiştir**' i seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-swap-deployment-slot.png" alt-text="Dağıtım yuvasını değiştirin." border="true":::

1. Değiştirme için yapılandırma ayarlarını doğrulayın ve **değiştirme** 'yi seçin
    
    :::image type="content" source="./media/functions-deployment-slots/azure-functions-deployment-slots-swap-config.png" alt-text="Dağıtım yuvasını değiştirin." border="true":::

Değiştirme işlemi yürütülürken işlem biraz zaman alabilir.

## <a name="roll-back-a-swap"></a>Değiştirme geri alma

Bir değiştirme bir hatayla sonuçlanarak veya yalnızca bir değiştirmeyi "geri almak" istiyorsanız, ilk duruma geri alabilirsiniz. Önceden takas durumuna geri dönmek için, değiştirmeyi tersine çevirmek için başka bir takas yapın.

## <a name="remove-a-slot"></a>Yuva kaldırma

Bir yuvayı [CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-delete) aracılığıyla veya Portal üzerinden kaldırabilirsiniz. Aşağıdaki adımlarda portalda bir yuvanın nasıl kaldırılacağı gösterilmektedir:

1. İşlev uygulamasındaki **dağıtım yuvaları** ' na gidin ve ardından yuva adını seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-navigate-slots.png" alt-text="Azure portal yuvaları bulun." border="true":::

1. **Sil**’i seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-delete-deployment-slot.png" alt-text="Azure portal dağıtım yuvasını silin." border="true":::

1. Silmek istediğiniz dağıtım yuvasının adını yazın ve ardından **Sil**' i seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-delete-deployment-slot-details.png" alt-text="Azure portal dağıtım yuvasını silin." border="true":::

1. Silme onayı bölmesini kapatın.

    :::image type="content" source="./media/functions-deployment-slots/functions-deployment-slot-deleted.png" alt-text="Dağıtım yuvası silme onayı." border="true":::

## <a name="automate-slot-management"></a>Yuva yönetimini otomatikleştirin

[Azure CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest)'yı kullanarak bir yuva için aşağıdaki işlemleri otomatikleştirebilirsiniz:

- [oluşturma](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-create)
- [silmeli](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-delete)
- [Listele](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-list)
- [Kur](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-swap)
- [Otomatik takas](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-auto-swap)

## <a name="change-app-service-plan"></a>App Service planını değiştir

App Service planı altında çalışan bir işlev uygulamasıyla, bir yuva için temel alınan App Service planını değiştirebilirsiniz.

> [!NOTE]
> Tüketim planı kapsamındaki bir yuvanın App Service planını değiştiremezsiniz.

Bir yuvanın App Service planını değiştirmek için aşağıdaki adımları kullanın:

1. İşlev uygulamasındaki **dağıtım yuvaları** ' na gidin ve ardından yuva adını seçin.

    :::image type="content" source="./media/functions-deployment-slots/functions-navigate-slots.png" alt-text="Azure portal yuvaları bulun." border="true":::

1. **App Service planı**altında **App Service planı değiştir**' i seçin.

1. Yükseltmek istediğiniz planı seçin veya yeni bir plan oluşturun.

    :::image type="content" source="./media/functions-deployment-slots/azure-functions-deployment-slots-change-app-service-apply.png" alt-text="Azure portal App Service planını değiştirin." border="true":::

1. **Tamam**’ı seçin.

## <a name="limitations"></a>Sınırlamalar

Azure Işlevleri dağıtım yuvaları aşağıdaki sınırlamalara sahiptir:

- Bir uygulama için kullanılabilen yuvaların sayısı plana bağlıdır. Tüketim planına yalnızca bir dağıtım yuvası izin verilir. App Service plan kapsamında çalışan uygulamalar için ek yuvalar vardır.
- Bir `AzureWebJobsSecretStorageType` uygulama ayarı değerine eşit olan uygulamalar için bir yuva sıfırlama anahtarlarını değiştirme `files` .
- Yuvalar, Linux tüketim planı için kullanılamaz.

## <a name="support-levels"></a>Destek düzeyleri

Dağıtım yuvaları için iki düzey destek vardır:

- **Genel kullanılabilirlik (GA)**: üretim kullanımı için tam olarak desteklenir ve onaylanır.
- **Önizleme**: henüz desteklenmiyor, ancak gelecekte GA durumuna ulaşması bekleniyor.

| İşletim sistemi/barındırma planı           | Destek düzeyi     |
| ------------------------- | -------------------- |
| Windows tüketimi       | Genel kullanılabilirlik |
| Windows Premium           | Genel kullanılabilirlik  |
| Windows ayrılmış         | Genel kullanılabilirlik |
| Linux tüketimi         | Desteklenmeyen          |
| Linux Premium             | Genel kullanılabilirlik  |
| Linux adanmış           | Genel kullanılabilirlik |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Işlevlerinde dağıtım teknolojileri](./functions-deployment-technologies.md)
