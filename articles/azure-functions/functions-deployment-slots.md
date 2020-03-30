---
title: Azure İşlevler dağıtım yuvaları
description: Azure İşlevleriyle dağıtım yuvaları oluşturmayı ve kullanmayı öğrenin
author: craigshoemaker
ms.topic: reference
ms.date: 08/12/2019
ms.author: cshoe
ms.openlocfilehash: 0e8c93ea6d5c2b525ccbea2af900f100afcc3d93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75769226"
---
# <a name="azure-functions-deployment-slots"></a>Azure İşlevler dağıtım yuvaları

Azure İşlevler dağıtım yuvaları, işlev uygulamanızın "yuvalar" adı verilen farklı örnekleri çalıştırmasına olanak tanır. Yuvalar, genel kullanıma açık bir bitiş noktası aracılığıyla ortaya çıkarılan farklı ortamlardır. Bir uygulama örneği her zaman üretim yuvasına eşlenir ve isteğe bağlı olarak yuvaya atanan örnekleri değiştirebilirsiniz. Apps Hizmeti planı altında çalışan işlev uygulamaları birden çok yuvaya sahip olabilir, tüketim planında ise yalnızca bir yuvaya izin verilir.

Aşağıdakiler, işlevlerin takas yuvalarından nasıl etkilendiğini yansıtır:

- Trafik yönlendirmesi sorunsuzdur; takas nedeniyle hiçbir istek düşürülmez.
- Bir işlev değiştirme sırasında çalışıyorsa, yürütme devam eder ve sonraki tetikleyiciler değiştirilen uygulama örneğine yönlendirilir.

> [!NOTE]
> Yuvalar şu anda Linux Tüketim planı için kullanılamıyor.

## <a name="why-use-slots"></a>Neden yuva kullanıyorsun?

Dağıtım yuvalarını kullanmanın birçok avantajı vardır. Aşağıdaki senaryolar yuvaları için ortak kullanımları açıklar:

- **Farklı amaçlar için farklı ortamlar**: Farklı yuvalar kullanmak, üretime veya evreleme yuvasına geçmeden önce uygulama örneklerini ayırt etme fırsatı verir.
- **Prewarming**: Doğrudan üretim yerine bir yuvaya dağıtım, uygulamanın yayına girmeden önce ısınmasını sağlar. Ayrıca, yuva kullanmak HTTP tarafından tetiklenen iş yüklerinin gecikme süresini azaltır. Örnekler dağıtımdan önce ısıtılır ve bu da yeni dağıtılan işlevlerin soğuk başlangıcını azaltır.
- **Kolay geri dönüşler**: Üretimle bir takastan sonra, daha önce sahnelenen bir uygulamanın yuvası artık önceki üretim uygulamasına sahip. Üretim yuvasına değiştirilen değişiklikler beklediğiniz gibi değilse, "bilinen son iyi örneğini" geri almak için takası hemen tersine çevirebilirsiniz.

## <a name="swap-operations"></a>Takas işlemleri

Bir takas sırasında, bir yuva kaynak ve diğer hedef olarak kabul edilir. Kaynak yuvası, hedef yuvaya uygulanan uygulama örneğine sahiptir. Aşağıdaki adımlar, hedef yuvasının takas sırasında kapalı kalma süresi yaşamamasını sağlar:

1. **Ayarları uygulayın:** Hedef yuvadaki ayarlar kaynak yuvanın tüm örneklerine uygulanır. Örneğin, üretim ayarları evreleme örneğine uygulanır. Uygulanan ayarlar aşağıdaki kategorileri içerir:
    - [Yuvaya özel](#manage-settings) uygulama ayarları ve bağlantı dizeleri (varsa)
    - [Sürekli dağıtım](../app-service/deploy-continuous-deployment.md) ayarları (etkinse)
    - [Uygulama Hizmeti kimlik doğrulama](../app-service/overview-authentication-authorization.md) ayarları (etkinse)

1. **Yeniden başlatma ve kullanılabilirlik için bekleyin:** Takas, kaynak yuvasındaki her örneğin yeniden başlatılmasını ve istekler için kullanılabilir olmasını bekler. Herhangi bir örnek yeniden başlatılamazsa, takas işlemi kaynak yuvasındaki tüm değişiklikleri geri değiştirir ve işlemi durdurur.

1. **Güncelleme yönlendirmesi:** Kaynak yuvasındaki tüm örnekler başarıyla ısıtılırsa, iki yuva yönlendirme kurallarını değiştirerek takası tamamlar. Bu adımdan sonra, hedef yuvası (örneğin, üretim yuvası) daha önce kaynak yuvasında ısınmış bir uygulamaya sahiptir.

1. **Yineleme işlemi:** Artık kaynak yuvasında daha önce hedef yuvasında ön takas uygulaması bulunduğuna göre, tüm ayarları uygulayarak ve kaynak yuvası için örnekleri yeniden başlatarak aynı işlemi gerçekleştirin.

Aşağıdaki noktaları göz önünde bulundurun:

- Takas işleminin herhangi bir noktasında, değiştirilen uygulamaların başlatılması kaynak yuvasında gerçekleşir. Kaynak yuvası hazırlanırken, takas başarılı olsun veya başarısız olurken hedef yuvası çevrimiçi kalır.

- Bir evreleme yuvasını üretim yuvasıyla değiştirmek için, üretim yuvasının *her zaman* hedef yuvası olduğundan emin olun. Bu şekilde, takas işlemi üretim uygulamanızı etkilemez.

- Bir *takas başlatmadan önce*olay kaynakları ve bağlamalarla ilgili [ayarların dağıtım yuvası ayarları](#manage-settings) olarak yapılandırılması gerekir. Bunları önceden "yapışkan" olarak işaretlemek, olayların ve çıktıların uygun örne yönlendirilmesini sağlar.

## <a name="manage-settings"></a>Ayarları yönetme

[!INCLUDE [app-service-deployment-slots-settings](../../includes/app-service-deployment-slots-settings.md)]

### <a name="create-a-deployment-setting"></a>Dağıtım ayarı oluşturma

Ayarları, "yapışkan" yapan bir dağıtım ayarı olarak işaretleyebilirsiniz. Yapışkan bir ayar uygulama örneği ile değiştirmez.

Bir yuvada bir dağıtım ayarı oluşturursanız, takasta yer alan diğer yuvalarda benzersiz bir değere sahip aynı ayarı oluşturduğunuzdan emin olun. Bu şekilde, bir ayarın değeri değişmezken, ayar adları yuvalar arasında tutarlı kalır. Bu ad tutarlılığı, kodunuzun bir yuvada tanımlanan ancak başka bir yuvada tanımlanmamış bir ayara erişmeye çalışmamasını sağlar.

Dağıtım ayarı oluşturmak için aşağıdaki adımları kullanın:

- İşlev uygulamasında *Ki Yuvalara* gidin
- Yuva adına tıklayın
- *Genel Ayarlar> Platform Özellikleri*altında **Yapılandırma'ya** tıklayın
- Geçerli yuvaya yapıştırmak istediğiniz ayar adını tıklatın
- Dağıtım **yuvası ayar** onay kutusunu tıklatın
- **Tamam'ı** tıklatın
- Ayar bıçağı kaybolduktan sonra, değişiklikleri korumak için **Kaydet'i** tıklatın

![Dağıtım Yuvası Ayarı](./media/functions-deployment-slots/azure-functions-deployment-slots-deployment-setting.png)

## <a name="deployment"></a>Dağıtım

Yuva oluşturduğunuzda yuvalar boştur. Uygulamanızı bir yuvaya dağıtmak için [desteklenen dağıtım teknolojilerinden](./functions-deployment-technologies.md) herhangi birini kullanabilirsiniz.

## <a name="scaling"></a>Ölçeklendirme

Tüm yuvalar, üretim yuvasıyla aynı sayıda işçiye ölçeklendirilir.

- Tüketim planları için, işlev uygulaması ölçeklendikçe yuva ölçeklendirilir.
- Uygulama Hizmeti planları için uygulama, sabit sayıda çalışana ölçeklenir. Yuvalar, uygulama planıyla aynı sayıda çalışanla çalışır.

## <a name="add-a-slot"></a>Yuva ekleme

[CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-create) üzerinden veya portal aracılığıyla bir yuva ekleyebilirsiniz. Aşağıdaki adımlar, portalda nasıl yeni bir yuva oluşturulacak larını gösterir:

1. Fonksiyon uygulamanıza gidin ve *Yuvaların*yanındaki **artı işaretine** tıklayın.

    ![Azure İşlevler dağıtım yuvası ekleme](./media/functions-deployment-slots/azure-functions-deployment-slots-add.png)

1. Textbox'a bir ad girin ve **Oluştur** düğmesine basın.

    ![Azure İşlevler dağıtım yuvasına ad](./media/functions-deployment-slots/azure-functions-deployment-slots-add-name.png)

## <a name="swap-slots"></a>Takas yuvaları

[CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-swap) üzerinden veya portal üzerinden yuvaları değiştirebilirsiniz. Aşağıdaki adımlar, portaldaki yuvaların nasıl değiştirilebildiğini gösterir:

1. İşlev uygulamasına gidin
1. Takas etmek istediğiniz kaynak yuvası adını tıklatın
1. Genel *Bakış* sekmesinden, **Azure** ![İşlerini Değiştir düğmesini tıklatın](./media/functions-deployment-slots/azure-functions-deployment-slots-swap.png)
1. Takasınız için yapılandırma ayarlarını doğrulayın ve **Takas** ![Azure İşlevleri dağıtım yuvasını tıklatın](./media/functions-deployment-slots/azure-functions-deployment-slots-swap-config.png)

Takas işlemi yürütülerken işlem bir an sürebilir.

## <a name="roll-back-a-swap"></a>Bir değiş tokuşu geri alma

Bir takas bir hataya neden olursa veya yalnızca bir değiş tokuşu "geri almak" istiyorsanız, ilk duruma geri dönebilirsiniz. Önceden değiştirilen duruma dönmek için, takası tersine çevirmek için başka bir takas yapın.

## <a name="remove-a-slot"></a>Yuvayı kaldırma

[CLI](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-delete) veya portal aracılığıyla bir yuva kaldırabilirsiniz. Aşağıdaki adımlar, portaldaki bir yuvanın nasıl kaldırılış yapılacağını gösterir:

1. İşlev uygulamasına Genel Bakış'ta gezinin

1. **Sil** düğmesine tıklayın

    ![Azure İşlevler dağıtım yuvası ekleme](./media/functions-deployment-slots/azure-functions-deployment-slots-delete.png)

## <a name="automate-slot-management"></a>Yuva yönetimini otomatikleştirin

Azure [CLI'yi](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest)kullanarak, bir yuva için aşağıdaki eylemleri otomatikleştirebilirsiniz:

- [oluşturmaya](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-create)
- [Silmek](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-delete)
- [list](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-list)
- [Takas](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-swap)
- [otomatik takas](https://docs.microsoft.com/cli/azure/functionapp/deployment/slot?view=azure-cli-latest#az-functionapp-deployment-slot-auto-swap)

## <a name="change-app-service-plan"></a>Uygulama Hizmeti planını değiştir

Bir Uygulama Hizmeti planı altında çalışan bir işlev uygulamasıyla, bir yuva için temel Uygulama Hizmeti planını değiştirme seçeneğiniz vardır.

> [!NOTE]
> Tüketim planı kapsamında bir yuvanın Uygulama Hizmeti planını değiştiremezsiniz.

Bir yuvanın Uygulama Hizmeti planını değiştirmek için aşağıdaki adımları kullanın:

1. Yuvaya gidin

1. *Platform Özellikleri*altında Tüm **Ayarlar'ı** tıklatın

    ![Uygulama hizmet planını değiştirme](./media/functions-deployment-slots/azure-functions-deployment-slots-change-app-service-settings.png)

1. Uygulama **Hizmeti planına** tıklayın

1. Yeni bir Uygulama Hizmeti planı seçin veya yeni bir plan oluşturun

1. **Tamam'ı** tıklatın

    ![Uygulama hizmet planını değiştirme](./media/functions-deployment-slots/azure-functions-deployment-slots-change-app-service-select.png)


## <a name="limitations"></a>Sınırlamalar

Azure İşlevler dağıtım yuvaları aşağıdaki sınırlamalara sahiptir:

- Bir uygulamaiçin kullanılabilen yuva sayısı plana bağlıdır. Tüketim planına yalnızca bir dağıtım yuvası na izin verilir. Uygulama Hizmeti planı kapsamında çalışan uygulamalar için ek yuvalar mevcuttur.
- Yuva değiştirme, `AzureWebJobsSecretStorageType` uygulama ayarı `files`ile eşit olan uygulamalar için anahtarları sıfırlar.
- Yuvalar Linux Tüketim planı için kullanılamaz.

## <a name="support-levels"></a>Destek düzeyleri

Dağıtım yuvaları için iki destek düzeyi vardır:

- **Genel kullanılabilirlik (GA)**: Üretim kullanımı için tam olarak desteklenmiş ve onaylanmıştır.
- **Önizleme**: Henüz desteklenmedi, ancak gelecekte GA durumuna ulaşması bekleniyor.

| Os/Hosting planı           | Destek düzeyi     |
| ------------------------- | -------------------- |
| Windows Tüketimi       | Genel kullanılabilirlik |
| Windows Premium           | Genel kullanılabilirlik  |
| Windows Özel         | Genel kullanılabilirlik |
| Linux Tüketimi         | Desteklenmeyen          |
| Linux Premium             | Genel kullanılabilirlik  |
| Linux Adanmış           | Genel kullanılabilirlik |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İşlevlerinde Dağıtım teknolojileri](./functions-deployment-technologies.md)
