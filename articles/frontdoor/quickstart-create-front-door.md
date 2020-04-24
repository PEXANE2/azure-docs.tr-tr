---
title: 'Hızlı başlangıç: Azure ön kapı hizmeti ile yüksek kullanılabilirliği ayarlama'
description: Bu hızlı başlangıçta, yüksek oranda kullanılabilir ve yüksek performanslı Global Web uygulamanız için Azure ön kapı hizmeti 'nin nasıl kullanılacağı açıklanmaktadır.
services: front-door
documentationcenter: ''
author: sharad4u
editor: ''
ms.assetid: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2020
ms.author: sharadag
ms.openlocfilehash: e7e500f0459c0f5fd4039acf316d9469e1567a09
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82117010"
---
# <a name="quickstart-create-a-front-door-for-a-highly-available-global-web-application"></a>Hızlı başlangıç: Yüksek oranda kullanılabilir global web uygulaması için Front Door oluşturma

Bir Web uygulaması için yüksek kullanılabilirlik ayarlamak üzere Azure portal kullanarak Azure ön kapısını kullanmaya başlayın.

Bu hızlı başlangıçta Azure ön kapısı, farklı Azure bölgelerinde çalışan bir Web uygulamasının iki örneğini havuzlar. Eşit ağırlıklı ve aynı öncelikli arka uçlara dayalı bir ön kapı yapılandırması oluşturursunuz. Bu yapılandırma, trafiği uygulamayı çalıştıran en yakın siteye yönlendirir. Azure ön kapısı, Web uygulamasını sürekli izler. Hizmet, en yakın site kullanılamadığında, kullanılabilir bir sonraki siteye otomatik yük devretme sağlar.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-two-instances-of-a-web-app"></a>Bir Web uygulamasının iki örneğini oluşturma

Bu hızlı başlangıç, farklı Azure bölgelerinde çalışan bir Web uygulamasının iki örneğini gerektirir. Her iki Web uygulaması örneği de *etkin/etkin* modda çalışır, bu nedenle bir tane trafik alabilir. Bu yapılandırma, biri yük devretme görevi gören bir *etkin/bağımsız* yapılandırmadan farklıdır.

Henüz bir Web uygulamanız yoksa, örnek Web uygulamaları ayarlamak için aşağıdaki adımları kullanın.

1. https://portal.azure.com adresinden Azure portalında oturum açın.

1. Giriş sayfasından veya Azure menüsünde **kaynak oluştur**' u seçin.

1. **Web** > **Web uygulaması**' nı seçin.

   ![Azure portalında web uygulaması oluşturma](media/quickstart-create-front-door/create-web-app-for-front-door.png)

1. **Web uygulaması**' nda kullanılacak **aboneliği** seçin.

1. **Kaynak grubu**Için **Yeni oluştur**' u seçin. **Ad** için *FrontDoorQS_rg1* girin ve **Tamam**' ı seçin.

1. **Örnek ayrıntıları**' nın altında, Web uygulamanız için benzersiz bir **ad** girin. Bu örnek *Webappcontoso-1*' i kullanır.

1. Bir **çalışma zamanı yığını**seçin, bu örnekte *.NET Core 2,1 (LTS)*.

1. *Orta ABD*gibi bir bölge seçin.

1. **Windows planı**Için **Yeni oluştur**' u seçin. **Ad** Için *Myappserviceplanmerkezileştirmemizi* girin ve **Tamam**' ı seçin.

1. **SKU ve boyutun** **Standart S1 100 toplam acu, 1,75 GB bellek**olduğundan emin olun.

1. **Gözden geçir + oluştur**' u seçin, **Özeti**gözden geçirin ve ardından **Oluştur**' u seçin. Dağıtımın tamamlanması birkaç dakika sürebilir.

   ![Web uygulaması için Özeti gözden geçirme](media/quickstart-create-front-door/summary-for-web-app-for-front-door.png)

Dağıtımınız tamamlandıktan sonra ikinci bir Web uygulaması oluşturun. Aynı yordamı, aşağıdaki değerler dışında aynı değerlerle kullanın:

| Ayar          | Değer     |
| ---              | ---  |
| **Kaynak grubu**   | **Yeni** ' yi seçin ve *FrontDoorQS_rg2* girin |
| **Adı**             | Web uygulamanız için benzersiz bir ad girin, bu örnekte *Webappcontoso-2*  |
| **Geli**           | Farklı bir bölge, bu örnekte *Orta Güney ABD* |
| **App Service plan** > **Windows planı**         | **Yeni** ' yi seçin ve *Myappserviceplangüneydoğu US*' i girip **Tamam** ' ı seçin. |

## <a name="create-a-front-door-for-your-application"></a>Uygulamanız için Front Door oluşturma

İki Web Apps sunucusu arasındaki en düşük gecikme süresine göre Kullanıcı trafiğini yönlendirmek için Azure ön kapısını yapılandırın. Başlamak için, Azure ön kapısının ön uç konağını ekleyin.

1. Giriş sayfasından veya Azure menüsünde **kaynak oluştur**' u seçin. **Ağ** > **ön kapısını**seçin.

1. **Ön kapı oluştur**' da bir **abonelik**seçin.

1. **Kaynak grubu**Için, **Yeni**' yi seçin ve ardından *FrontDoorQS_rg0* girin ve **Tamam**' ı seçin.  Bunun yerine, var olan bir kaynak grubunu kullanabilirsiniz.

1. Bir kaynak grubu oluşturduysanız, bir **kaynak grubu konumu**seçin ve ardından **İleri: yapılandırma**' yı seçin.

1. **Ön uç konak Ekle**' yi açmak **+** için ön **uçlar/etki alanları**' nı seçin.

1. **Ana bilgisayar adı**için, genel olarak benzersiz bir ana bilgisayar adı girin. Bu örnek *contoso-ön uç*kullanır. **Add (Ekle)** seçeneğini belirleyin.

   ![Azure ön kapısı için bir ön uç konağı ekleme](media/quickstart-create-front-door/add-frontend-host-for-front-door.png)

Sonra, iki Web uygulaması içeren bir arka uç havuzu oluşturun.

1. Hala **ön kapı oluşturun**, **arka uç havuzlarında**, **+** **arka uç Havuzu Ekle**' yi seçin.

1. **Ad**Için *Mybackendpool*yazın.

1. **Arka uç Ekle**' yi seçin. **Arka uç ana bilgisayar türü**için *App Service*seçin.

1. Aboneliğinizi seçin ve ardından **arka uç ana bilgisayar adından**oluşturduğunuz ilk Web uygulamasını seçin. Bu örnekte, Web uygulaması *Webappcontoso-1*idi. **Add (Ekle)** seçeneğini belirleyin.

1. **Arka uç Ekle** ' yi seçin. **Arka uç ana bilgisayar türü**için *App Service*seçin.

1. Aboneliğinizi yeniden seçin ve **arka uç ana bilgisayar adından**oluşturduğunuz ikinci Web uygulamasını seçin. **Add (Ekle)** seçeneğini belirleyin.

   ![Ön kapıya bir arka uç konağı ekleme](media/quickstart-create-front-door/add-backend-host-to-pool-for-front-door.png)

Son olarak, bir yönlendirme kuralı ekleyin. Bir yönlendirme kuralı, ön uç konunuzu arka uç havuzuna eşler. Kural, için `contoso-frontend.azurefd.net` bir Isteği **Mybackendpool**öğesine iletir.

1. Hala **ön kapı oluştur**' da, **yönlendirme kuralları**' nda bir **+** yönlendirme kuralı yapılandırmak için öğesini seçin.

1. **Kural Ekle**' de, **ad**için *locationrule*yazın. Varsayılan değerleri kabul edin ve sonra yönlendirme kuralını eklemek için **Ekle** ' yi seçin.

   >[!WARNING]
   > Ön kapıdaki her bir ön uç konağın, kendisiyle ilişkilendirilmiş bir yönlendirme kuralına`\*`sahip olduğundan emin **olmanız gerekir** . Diğer bir deyişle, tüm yönlendirme kurallarınızın tamamında, varsayılan yolda (`\*`) tanımlanan ön uç konaklarınızın her biri için en az bir yönlendirme kuralı olmalıdır. Bunun başarısız olması, Son Kullanıcı trafiğinizin doğru şekilde yönlendirilmemesi sonucunu verebilir.

1. **Gözden geçir + oluştur**' u ve ardından **Oluştur**' u seçin.

   ![Yapılandırılmış Azure ön kapısı](media/quickstart-create-front-door/configuration-of-front-door.png)

## <a name="view-azure-front-door-in-action"></a>Azure ön kapısını görüntüleme işlemi

Ön kapı oluşturduktan sonra yapılandırmanın küresel olarak dağıtılması birkaç dakika sürer. Tamamlandıktan sonra oluşturduğunuz ön uç konağına erişin. Bir tarayıcıda adresine gidin `contoso-frontend.azurefd.net`. İsteğiniz, arka uç havuzundaki belirtilen sunuculardan sizin için en yakın sunucuya otomatik olarak yönlendirilir.

Bu uygulamaları bu hızlı başlangıçta oluşturduysanız bir bilgi sayfası görürsünüz.

Hızlı genel yük devretmeyi işlem sırasında test etmek için aşağıdaki adımları deneyin:

1. Yukarıda açıklandığı gibi bir tarayıcı açın ve ön uç adresine gidin: `contoso-frontend.azurefd.net`.

1. Azure portal, *uygulama hizmetleri*' ni arayıp seçin. Bu örnekte Web uygulamalarınızdan birini bulmak için aşağı kaydırın, **Webappcontoso-1** .

1. Web uygulamanızı seçin ve ardından **Durdur**' u ve ardından doğrulamak için **Evet** ' i seçin.

1. Tarayıcınızı yenileyin. Aynı bilgi sayfasını görmeniz gerekir.

   >[!TIP]
   >Bu eylemler için biraz gecikme vardır. Yeniden yenilemeniz gerekebilir.

1. Diğer Web uygulamasını bulun ve bunu da durdurun.

1. Tarayıcınızı yenileyin. Bu kez bir hata iletisi görmeniz gerekir.

   ![Her iki Web uygulaması örneği de durduruldu](media/quickstart-create-front-door/service-has-been-stopped.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde, oluşturduğunuz tüm öğeleri kaldırabilirsiniz. Kaynak grubunun silinmesi, içeriğini de siler. Bu ön kapıyı kullanmayı düşünmüyorsanız gereksiz ücretlerden kaçınmak için kaynakları kaldırmanız gerekir.

1. Azure portal, **kaynak gruplarını**arayıp seçin ya da Azure Portal menüsünden **kaynak grupları** ' nı seçin.

1. **FrontDoorQS_rg0**gibi bir kaynak grubu bulmak için filtre uygulayın veya aşağı kaydırın.

1. Kaynak grubunu seçin ve **kaynak grubunu sil**' i seçin.

   >[!WARNING]
   >Bu eyleme geri döndürülemez.

1. Doğrulanacak kaynak grubu adını yazın ve ardından **Sil**' i seçin.

Diğer iki grup için yordamı tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Ön kapıya özel bir etki alanı ekleme hakkında bilgi edinmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Özel etki alanı ekleme](front-door-custom-domain.md)

Yönlendirme trafiği hakkında daha fazla bilgi için bkz. [ön kapı yönlendirme yöntemleri](front-door-routing-methods.md).
