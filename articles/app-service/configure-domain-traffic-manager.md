---
title: DNS adlarını Trafik Yöneticisi ile yapılandırma
description: Yük dengeleme için Trafik Yöneticisi ile tümleşen bir Azure Uygulama Hizmeti uygulaması için özel bir etki alanını nasıl yapılandıracağınızı öğrenin.
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.topic: article
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: f8322c12669e41fc7c9aa88e99f95cf1b26ea87d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944101"
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-with-traffic-manager-integration"></a>Trafik Yöneticisi tümleştirmesiyle Azure Uygulama Hizmeti'nde özel bir etki alanı adı yapılandırma

[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

> [!NOTE]
> Bulut Hizmetleri için bkz: [Azure bulut hizmeti için özel bir etki alanı adı yapılandırma.](../cloud-services/cloud-services-custom-domain-name.md)

Bakiye trafiğini [Azure Uygulama Hizmetine](overview.md)yüklemek için [Azure Trafik Yöneticisi'ni](/azure/traffic-manager/) kullandığınızda, Uygulama Hizmeti uygulamasına trafik yöneticisi bitiş noktası ** \<>.trafficmanager.net**kullanılarak erişilebilir. Kullanıcılarınız için daha tanınabilir bir\.etki alanı adı sağlamak için Uygulama Hizmeti uygulamanızla www contoso.com gibi özel bir etki alanı adı atayabilirsiniz.

Bu makalede, [Trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md)ile entegre edilmiş bir Uygulama Hizmeti uygulamasıyla özel bir etki alanı adı nasıl yapılandırılabildiğiniz gösterilmektedir.

> [!NOTE]
> Trafik Yöneticisi bitiş noktasını kullanarak bir etki alanı adını yapılandırdığınızda yalnızca [CNAME](https://en.wikipedia.org/wiki/CNAME_record) kayıtları desteklenir. A kayıtları desteklenmedığından, contoso.com gibi kök etki alanı eşlemesi de desteklenmez.
> 

## <a name="prepare-the-app"></a>Uygulamayı hazırlama

Özel bir DNS adını Azure Trafik Yöneticisi ile entegre edilmiş bir uygulamayla eşlemek için web uygulamasının [Uygulama Hizmeti planının](https://azure.microsoft.com/pricing/details/app-service/) **Standart** katmanda veya daha yüksek olması gerekir. Bu adımda, App Service uygulamasının desteklenen bir fiyatlandırma katmanında olduğundan emin olursunuz.

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

Azure [portalında,](https://portal.azure.com) **Uygulama Hizmetlerini**arayın ve seçin.

Uygulama **Hizmetleri** sayfasında Azure uygulamanızın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-domain/select-app.png)

Uygulama sayfasının sol navigasyonunda, **Ölçeklendir (Uygulama Hizmeti planı)** seçeneğini belirleyin.

![Ölçeği artır menüsü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Uygulamanın geçerli katmanı mavi kenarlıkla vurgulanmıştır. Uygulamanın **Standart** katmanda veya üzerinde olduğundan emin olun **(Üretim** veya **Yalıtılmış** kategorisindeki herhangi bir katman). Evet ise, **Sayfayı Ölçekle'yi** kapatın ve [CNAME eşlemini oluşturmak](#create-the-cname-mapping)için atlayın.

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

### <a name="scale-up-the-app-service-plan"></a>App Service planının ölçeğini artırma

Uygulamanızı büyütmeniz gerekiyorsa, **Üretim** kategorisindeki fiyatlandırma katmanlarından herhangi birini seçin. Ek seçenekler için **Ek seçeneklere bakın**’a tıklayın.

**Uygula**’ya tıklayın.

## <a name="create-traffic-manager-endpoint"></a>Trafik Yöneticisi bitiş noktası oluşturma

Bitiş Noktaları [Ekle veya Sil'deki](../traffic-manager/traffic-manager-endpoints.md)adımları izleyerek, Uygulama Hizmeti uygulamanızı Trafik Yöneticisi profilinizde bir bitiş noktası olarak ekleyin.

Uygulama Hizmeti uygulamanız desteklenen bir fiyatlandırma katmanına girdiğinde, bitiş noktasını eklediğinizde kullanılabilir Uygulama Hizmeti hedefleri listesinde yer gösterir. Uygulamanız listede yoksa, [uygulamanızın fiyatlandırma katmanını doğrulayın.](#prepare-the-app)

## <a name="create-the-cname-mapping"></a>CNAME eşlemini oluşturma
> [!NOTE]
> Satın aldığınız bir [Uygulama Hizmeti etki alanını](manage-custom-dns-buy-domain.md)yapılandırmak için bu bölümü atlayın ve özel etki alanını [etkinleştir'e](#enable-custom-domain)gidin.
> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Her etki alanı sağlayıcısının özellikleri farklılık gösterse de, özel alan adı **(contoso.com** *gibi)* *ile* uygulamanızla entegre edilmiş Trafik Yöneticisi etki alanı adına **(contoso.trafficmanager.net)** eşbelirlersiniz.

> [!NOTE]
> Bir kayıt zaten kullanılıyorsa ve uygulamalarınızı önceden bağlamanız gerekiyorsa, ek bir CNAME kaydı oluşturabilirsiniz. Örneğin, **www\.contoso.com** önceden uygulamanıza bağlamak **için, awverify.www'den** **contoso.trafficmanager.net'a**bir CNAME kaydı oluşturun. Daha sonra "www" CNAME kaydını değiştirmenize gerek kalmadan uygulamanıza "www\.contoso.com" ekleyebilirsiniz. Daha fazla bilgi için bkz. etkin [bir DNS adını Azure Uygulama Hizmetine Geçir.](manage-custom-dns-migrate-domain.md)

Etki alanı sağlayıcınıza DNS kayıtlarını eklemeyi veya değiştirmeyi bitirdikten sonra değişiklikleri kaydedin.

## <a name="enable-custom-domain"></a>Özel etki alanını etkinleştirme
Alan adınız için kayıtlar yayınlandıktan sonra, özel alan adınızın Uygulama Hizmeti uygulamanıza çözüldüğünü doğrulamak için tarayıcıyı kullanın.

> [!NOTE]
> CNAME'nizin DNS sistemi üzerinden yayılması biraz zaman alabilir. <a href="https://www.digwebinterface.com/">https://www.digwebinterface.com/</a> CNAME'nin kullanılabilir olduğunu doğrulamak için bir hizmet kullanabilirsiniz.
> 
> 

1. Etki alanı çözünürlüğü başarılı olduktan sonra Azure [Portalı'ndaki](https://portal.azure.com) uygulama sayfanıza geri dönmek için
2. Sol gezintiden, **Özel etki alanlarını** > seçin**Ana bilgisayar adı ekleyin.**
4. Daha önce eşlediğiniz özel etki alanı adını yazın ve **Doğrula'yı**seçin.
5. **Hostname kayıt türünün** **\.CNAME (www example.com veya herhangi bir alt etki alanı)** olarak ayarlandıklarına emin olun.

6. App Service uygulaması artık bir Trafik Yöneticisi bitiş noktası ile entegre olduğundan, **CNAME yapılandırması**altında Trafik Yöneticisi etki alanı adını görmeniz gerekir. Seçin ve **özel etki alanı ekle'yi**tıklatın.

    ![Uygulamaya DNS adı ekleme](./media/configure-domain-traffic-manager/enable-traffic-manager-domain.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure App Service'de SSL bağlamasıyla özel DNS adının güvenliğini sağlama](configure-ssl-bindings.md)
