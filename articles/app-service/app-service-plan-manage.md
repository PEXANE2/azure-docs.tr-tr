---
title: Uygulama Hizmeti planını yönet
description: Bir Uygulama Hizmeti planını yönetmek için oluşturma, taşıma, ölçeklendirme ve silme gibi farklı görevleri nasıl gerçekleştireceklerini öğrenin.
keywords: uygulama hizmeti, azure uygulama hizmeti, ölçek, uygulama hizmet planı, değiştirme, oluşturma, yönetme, yönetme
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.topic: article
ms.date: 10/24/2019
ms.custom: seodec18
ms.openlocfilehash: d40f5db65ce9ca90ae978bac4491bdebccc2a328
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811718"
---
# <a name="manage-an-app-service-plan-in-azure"></a>Azure'da Uygulama Hizmeti planını yönetme

[Azure Uygulama Hizmeti planı,](overview-hosting-plans.md) bir Uygulama Hizmeti uygulamasının çalışması için gereken kaynakları sağlar. Bu kılavuz, Bir Uygulama Hizmeti planının nasıl yönetilengösterilebildiğini gösterir.

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

> [!TIP]
> Uygulama Hizmeti Ortamınız varsa, [bkz.](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Boş bir Uygulama Hizmeti planı oluşturabilir veya uygulama oluşturmanın bir parçası olarak bir plan oluşturabilirsiniz.

1. Azure [portalında](https://portal.azure.com) **kaynak oluştur'u**seçin.

   ![Azure portalında bir kaynak oluşturun.][createResource] 

1. **Yeni** > **Web Uygulaması'nı** veya başka bir Uygulama hizmeti uygulamasını seçin.

   ![Azure portalında bir uygulama oluşturun.][createWebApp] 

2. Uygulama Hizmeti planını yapılandırmadan önce **Örnek Ayrıntılar** bölümünü yapılandırın. **Yayımlama** ve **İşletim Sistemleri** gibi ayarlar, Uygulama Hizmeti planınızın kullanılabilir fiyatlandırma katmanlarını değiştirebilir. **Bölge,** Uygulama Hizmeti planınızın nerede oluşturulacağını belirler. 
   
3. Uygulama **Hizmet Planı** bölümünde, varolan bir planı seçin veya **yeni oluştur'u**seçerek bir plan oluşturun.

   ![Bir Uygulama Hizmeti planı oluşturun.][createASP] 

4. Bir plan oluştururken, yeni planın fiyatlandırma katmanını seçebilirsiniz. **Sku ve boyutunda,** fiyatlandırma katmanını değiştirmek için **boyutu değiştir'i** seçin. 

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Bir uygulamayı başka bir Uygulama Hizmeti planına taşıma

Kaynak planı ve hedef plan _aynı kaynak grubunda ve coğrafi bölgede_olduğu sürece, bir uygulamayı başka bir Uygulama Hizmeti planına taşıyabilirsiniz.

> [!NOTE]
> Azure, her yeni Uygulama Hizmeti planını dahili olarak web alanı olarak adlandırılan bir dağıtım birimine dağıtır. Her bölgenin birçok web alanı olabilir, ancak uygulamanız yalnızca aynı web alanında oluşturulan planlar arasında hareket edebilir. Uygulama Hizmet Ortamı yalıtılmış bir web alanıdır, bu nedenle uygulamalar aynı Uygulama Hizmeti Ortamındaki planlar arasında taşınabilir, ancak farklı Uygulama Hizmeti Ortamlarında planlar arasında taşınamayabilir.
>
> Plan oluştururken istediğiniz web alanını belirtemezsiniz, ancak bir planın varolan bir planla aynı web alanında oluşturulduğundan emin olabilirsiniz. Kısacası, aynı kaynak grubu ve bölge birleşimi ile oluşturulan tüm planlar aynı web alanına dağıtılır. Örneğin, kaynak grubu A ve B bölgesinde bir plan oluşturduysanız, daha sonra kaynak grubunda oluşturduğunuz tüm planlar A ve b bölgesinde aynı web alanına dağıtılır. Planların oluşturulduktan sonra web alanlarını taşıyamayacaklarını, bu nedenle bir planı başka bir kaynak grubuna taşıyarak başka bir planla "aynı web alanına" taşıyamayacağınızı unutmayın.
> 

1. Azure [portalında,](https://portal.azure.com) **Uygulama hizmetlerini** arayın ve seçin ve taşımak istediğiniz uygulamayı seçin.

2. Sol menüden **Uygulama Hizmeti planını değiştir'i**seçin.

3. Uygulama **Hizmeti planı** açılır düşüşünde, uygulamayı taşımak için varolan bir planı seçin. Açılır sonuç yalnızca geçerli Uygulama Hizmeti planıyla aynı kaynak grubunda ve coğrafi bölgede olan planları gösterir. Böyle bir plan yoksa, varsayılan olarak bir plan oluşturmanıza olanak tanır. Ayrıca **yeni oluştur**seçeneğini seçerek el ile yeni bir plan oluşturabilirsiniz.

4. Bir plan oluşturursanız, yeni planın fiyatlandırma katmanını seçebilirsiniz. **Fiyatlandırma**Katmanı'nda, değiştirmek için varolan katmanı seçin. 
   
   > [!IMPORTANT]
   > Bir uygulamayı daha üst katmanlı bir plandan **D1'den** **F1'e**kadar daha düşük katmanlı bir plana taşıyorsanız, uygulama hedef plandaki bazı özellikleri kaybedebilir. Örneğin, uygulamanız TLS/SSL sertifikaları kullanıyorsa, şu hata iletisi görebilirsiniz:
   >
   > `Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`

5. Tamamlandığında **Tamam'ı**seçin.
   
   ![Uygulama Hizmeti planı seçici.][change] 

## <a name="move-an-app-to-a-different-region"></a>Uygulamayı farklı bir bölgeye taşıma

Uygulamanızın çalıştığı bölge, Uygulama Hizmeti planının bulunduğu bölgedir. Ancak, Bir Uygulama Hizmeti planının bölgesini değiştiremezsiniz. Uygulamanızı farklı bir bölgede çalıştırmak istiyorsanız, bir alternatif uygulama klonlamadır. Klonlama, uygulamanızın bir kopyasını herhangi bir bölgedeki yeni veya mevcut Bir Uygulama Hizmeti planında yapar.

**Klon Uygulamasını** menüdeki **Geliştirme Araçları** bölümünde bulabilirsiniz.

> [!IMPORTANT]
> Klonlamanın bazı sınırlamaları vardır. Azure App Service [App klonlama](app-service-web-app-cloning.md)da bunlar hakkında okuyabilirsiniz.

## <a name="scale-an-app-service-plan"></a>Uygulama Hizmeti planını ölçeklendirin

Bir Uygulama Hizmeti planının fiyatlandırma katmanını ölçeklendirmek için [bkz.](manage-scale-up.md)

Bir uygulamanın örnek sayısını ölçeklendirmek için, [Bkz. Ölçek örneği sayısı el ile veya otomatik olarak.](../monitoring-and-diagnostics/insights-how-to-scale.md)

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Uygulama Hizmeti planını silme

Beklenmeyen ücretleri önlemek için, Bir Uygulama Hizmeti planındaki son uygulamayı sildiğinizde, App Service varsayılan olarak planı da siler. Bunun yerine planı tutmayı seçerseniz, ücretlendirilmemek için planı **Ücretsiz** katman olarak değiştirmelisiniz.

> [!IMPORTANT]
> Yapılandırılan VM örneklerini ayırmaya devam ettikleri için, kendileriyle ilişkili hiçbir uygulaması olmayan Uygulama Hizmeti planları yine de ücrete tabidir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure’da uygulamanın ölçeğini artırma](manage-scale-up.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[createResource]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-a-resource.png
