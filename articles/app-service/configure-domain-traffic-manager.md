---
title: DNS adlarını Traffic Manager ile yapılandırma
description: Yük Dengeleme için Traffic Manager ile tümleşen Azure App Service bir uygulama için özel bir etki alanı yapılandırmayı öğrenin.
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.topic: article
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: f8322c12669e41fc7c9aa88e99f95cf1b26ea87d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78944101"
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-with-traffic-manager-integration"></a>Traffic Manager tümleştirmeyle Azure App Service bir özel etki alanı adı yapılandırma

[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

> [!NOTE]
> Cloud Services için bkz. [Azure bulut hizmeti için özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name.md).

[Azure App Service](overview.md)trafiğini dengelemek için [Azure Traffic Manager](/azure/traffic-manager/) kullandığınızda App Service uygulamasına ** \<Traffic-Manager-Endpoint>. trafficmanager.net**kullanılarak erişilebilir. Kullanıcılarınız için daha tanınabilir bir etki alanı adı sağlamak üzere App Service\.uygulamasına sahip www contoso.com gibi özel bir etki alanı adı atayabilirsiniz.

Bu makalede, [Traffic Manager](../traffic-manager/traffic-manager-overview.md)ile tümleştirilmiş App Service bir uygulamayla özel bir etki alanı adının nasıl yapılandırılacağı gösterilir.

> [!NOTE]
> Traffic Manager uç noktasını kullanarak bir etki alanı adı yapılandırdığınızda yalnızca [CNAME](https://en.wikipedia.org/wiki/CNAME_record) kayıtları desteklenir. Bir kayıt desteklenmediğinden, contoso.com gibi bir kök etki alanı eşlemesi de desteklenmez.
> 

## <a name="prepare-the-app"></a>Uygulamayı hazırlama

Özel bir DNS adını Azure Traffic Manager ile tümleştirilmiş bir uygulamayla eşlemek için, Web uygulamasının [App Service planı](https://azure.microsoft.com/pricing/details/app-service/) **Standart** katmanda veya daha yüksek olmalıdır. Bu adımda, App Service uygulamasının desteklenen bir fiyatlandırma katmanında olduğundan emin olursunuz.

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

[Azure Portal](https://portal.azure.com), **uygulama hizmetleri**' ni arayıp seçin.

**Uygulama hizmetleri** sayfasında, Azure uygulamanızın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-domain/select-app.png)

Uygulama sayfasının sol gezinti bölmesinde **Ölçek yukarı (App Service planı)** öğesini seçin.

![Ölçeği artır menüsü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Uygulamanın geçerli katmanı mavi kenarlıkla vurgulanmıştır. Uygulamanın **Standart** katmanda veya yukarısında ( **üretimde** veya **yalıtılmış** kategoride herhangi bir katman) olduğundan emin olun. Yanıt Evet ise, **Ölçek artırma** sayfasını kapatın ve [CNAME eşlemesini oluşturmak](#create-the-cname-mapping)için atlayın.

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

### <a name="scale-up-the-app-service-plan"></a>App Service planının ölçeğini artırma

Uygulamanızı ölçeklendirmeniz gerekiyorsa, **Üretim** kategorisindeki fiyatlandırma katmanlarından birini seçin. Ek seçenekler için **Ek seçeneklere bakın**’a tıklayın.

**Uygula**’ya tıklayın.

## <a name="create-traffic-manager-endpoint"></a>Traffic Manager uç noktası oluşturma

[Uç nokta ekleme veya silme](../traffic-manager/traffic-manager-endpoints.md)bölümündeki adımları izleyerek, App Service uygulamanızı Traffic Manager profilinize bir uç nokta olarak ekleyin.

App Service uygulamanız desteklenen bir fiyatlandırma katmanındaysa, uç noktayı eklediğinizde kullanılabilir App Service hedefleri listesinde görünür. Uygulamanız listede yoksa [uygulamanızın fiyatlandırma katmanını doğrulayın](#prepare-the-app).

## <a name="create-the-cname-mapping"></a>CNAME eşlemesini oluşturma
> [!NOTE]
> [Satın aldığınız App Service bir etki alanını](manage-custom-dns-buy-domain.md)yapılandırmak için bu bölümü atlayın ve [özel etki alanını etkinleştir](#enable-custom-domain)' e gidin.
> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Her etki alanı sağlayıcının özellikleri farklılık gösterdiğinden, özel etki alanı adından (örneğin, **contoso.com** *) uygulamanızla* tümleştirilmiş Traffic Manager etki alanı adına (**contoso.trafficmanager.net** *) eşleyebilirsiniz.*

> [!NOTE]
> Bir kayıt zaten kullanımda ise ve uygulamalarınızı buna preemptively bağlamanız gerekiyorsa, ek bir CNAME kaydı oluşturabilirsiniz. Örneğin, preemptively için **\.www contoso.com** 'yi uygulamanıza bağlamak için **awverify. www** öğesinden **contoso.trafficmanager.net**'e bir CNAME kaydı oluşturun. "Www" CNAME kaydını değiştirmeye\.gerek kalmadan uygulamanıza "www contoso.com" ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure App Service için etkin BIR DNS adı geçirme](manage-custom-dns-migrate-domain.md).

Etki alanı sağlayıcınızda DNS kayıtlarını ekleme veya değiştirme işlemi tamamlandıktan sonra, değişiklikleri kaydedin.

## <a name="enable-custom-domain"></a>Özel etki alanını etkinleştir
Etki alanı adınızın kayıtları dağıtıldıktan sonra, özel etki alanı adınızın App Service uygulamanıza çözümlendiğini doğrulamak için tarayıcıyı kullanın.

> [!NOTE]
> CNAME 'nizin DNS sistemine yayılması biraz zaman alabilir. CNAME 'in kullanılabildiğini doğrulamak için gibi <a href="https://www.digwebinterface.com/">https://www.digwebinterface.com/</a> bir hizmet kullanabilirsiniz.
> 
> 

1. Etki alanı çözümlemesi başarılı olduktan sonra [Azure portalında](https://portal.azure.com) uygulama sayfanıza geri dönün
2. Sol gezinti bölmesinde **özel etki alanı** > **adı Ekle**' yi seçin.
4. Daha önce eşlenen özel etki alanı adını yazın ve **Doğrula**' yı seçin.
5. **Ana bilgisayar adı kayıt türünün** **CNAME (www\.example.com veya herhangi bir alt etki alanı)** olarak ayarlandığından emin olun.

6. App Service uygulama artık bir Traffic Manager uç noktasıyla tümleştirildiği için **CNAME yapılandırması**altında Traffic Manager etki alanı adını görmeniz gerekir. Seçin ve **özel etki alanı Ekle**' ye tıklayın.

    ![Uygulamaya DNS adı ekleme](./media/configure-domain-traffic-manager/enable-traffic-manager-domain.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure App Service'de SSL bağlamasıyla özel DNS adının güvenliğini sağlama](configure-ssl-bindings.md)
