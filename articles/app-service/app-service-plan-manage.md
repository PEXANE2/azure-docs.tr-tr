---
title: App Service planını yönetme-Azure | Microsoft Docs
description: App Service planını yönetmek için farklı görevleri nasıl gerçekleştireceğinizi öğrenin.
keywords: App Service, Azure App Service, ölçek, App Service planı, değişiklik, oluşturma, yönetme, yönetim
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: c749f090cbb0aeb036a53a01e6cd516617c3186d
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68953872"
---
# <a name="manage-an-app-service-plan-in-azure"></a>Azure 'da App Service planını yönetme

Bir [Azure App Service planı](overview-hosting-plans.md) , bir App Service uygulamasının çalışması için gereken kaynakları sağlar. Bu kılavuzda bir App Service planının nasıl yönetileceği gösterilmektedir.

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

> [!TIP]
> Bir App Service Ortamı varsa, bkz. bir [App Service Ortamı App Service planı oluşturma](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Boş bir App Service planı oluşturabilir veya uygulama oluşturmanın bir parçası olarak bir plan oluşturabilirsiniz.

1. [Azure Portal](https://portal.azure.com) **Yeni** > **Web + mobil**' i seçin ve ardından **Web uygulaması** ' nı veya başka türde bir App Service uygulaması ' nı seçin.

2. Mevcut bir App Service planı seçin veya yeni uygulama için bir plan oluşturun.

   ![Azure portal bir uygulama oluşturun.][createWebApp]

   Bir plan oluşturmak için:

   a. **[+] Yeni oluştur**seçeneğini belirleyin.

      ![App Service planı oluşturun.][createASP] 

   b. **App Service planı**için planın adını girin.

   c. **Konum**için uygun bir konum seçin.

   d. **Fiyatlandırma katmanı**için, hizmet için uygun bir fiyatlandırma katmanı seçin. **Ücretsiz** ve **paylaşılan**gibi daha fazla fiyatlandırma seçeneği görüntülemek için **Tümünü görüntüle** ' yi seçin. Fiyatlandırma katmanını seçtikten sonra **Seç** düğmesine tıklayın.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Bir uygulamayı başka bir App Service plana taşıma

Kaynak planı ve hedef plan _aynı kaynak grubunda ve coğrafi bölgede_olduğu sürece, bir uygulamayı başka bir App Service planına taşıyabilirsiniz.

> [!NOTE]
> Azure her yeni App Service planını, dahili olarak web alanı olarak adlandırılan bir dağıtım birimine dağıtır. Her bölgede birçok webspaces bulunabilir, ancak uygulamanız yalnızca aynı Web alanında oluşturulan planlar arasında hareket edebilir. App Service Ortamı yalıtılmış bir web alanı olduğundan, uygulamalar aynı App Service Ortamı planlar arasında taşınabilir, ancak farklı App Service ortamlarında planlar arasında taşınabilirler.
>
> Bir plan oluştururken istediğiniz Web alanı belirtemezsiniz, ancak bir planın mevcut bir planla aynı Web alanında oluşturulmasını sağlamak mümkündür. Kısaca, aynı kaynak grubu ve bölge birleşimiyle oluşturulan tüm planlar aynı Web alanına dağıtılır. Örneğin, A ve B bölgesi kaynak grubunda bir plan oluşturduysanız, kaynak grubunda daha sonra oluşturduğunuz herhangi bir plan ve B bölgesi aynı Web alanına dağıtılır. Planların oluşturulduktan sonra izlemiyor ' ı taşıyamadığını unutmayın. bu nedenle, başka bir kaynak grubuna taşıyarak planı başka bir planla "aynı Web alanına" taşıyamazsınız.
> 

1. [Azure Portal](https://portal.azure.com), taşımak istediğiniz uygulamaya gidin.

1. Menüsünde **App Service planı** bölümünü bulun.

1. **App Service planı** seçicisini açmak için **App Service planı değiştir** ' i seçin.

   ![App Service planı seçici.][change] 

1. **App Service planı** seçicide bu uygulamayı taşımak için mevcut bir planı seçin.   

**App Service planı seç** sayfası, yalnızca geçerli uygulamanın App Service planı ile aynı kaynak grubunda ve coğrafi bölgede bulunan planları gösterir.

Her planın kendi fiyatlandırma katmanı vardır. Örneğin, bir siteyi **ücretsiz** bir katmandan **Standart** katmana taşımak, kendisine atanan tüm uygulamaların **Standart** katmanın özelliklerini ve kaynaklarını kullanmasına olanak sağlar. Ancak, bir uygulamayı daha yüksek katmanlı bir plandan daha düşük katmanlı bir plana taşımak, artık bazı özelliklere erişiminizin olmadığı anlamına gelir. Uygulamanız hedef planda kullanılamayan bir özellik kullanıyorsa, kullanımda olan hangi özelliğin kullanımda olduğunu gösteren bir hata alırsınız. 

Örneğin, uygulamalarınızdan biri SSL sertifikaları kullanıyorsa şu hata iletisini görebilirsiniz:

`Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`

Bu durumda, uygulamayı hedef plana taşıyabilmeniz için aşağıdakilerden birini yapmanız gerekir:
- Hedef planın fiyatlandırma katmanını **temel** veya daha yüksek olarak ölçeklendirin.
- Uygulamanıza yönelik tüm SSL bağlantılarını kaldırın.

## <a name="move-an-app-to-a-different-region"></a>Uygulamayı farklı bir bölgeye taşıma

Uygulamanızın çalıştırıldığı bölge, içinde bulunduğu App Service planın bölgesidir. Ancak, App Service planının bölgesini değiştiremezsiniz. Uygulamanızı farklı bir bölgede çalıştırmak istiyorsanız, bir alternatif uygulama kopyalama işlemi olur. Kopyalama, uygulamanızın herhangi bir bölgedeki yeni veya mevcut bir App Service planında bir kopyasını oluşturur.

**Kopya uygulamayı** , menünün **geliştirme araçları** bölümünde bulabilirsiniz.

> [!IMPORTANT]
> Kopyalamaya bazı sınırlamalar vardır. [Azure App Service uygulama kopyalama](app-service-web-app-cloning.md)bölümünde bulabilirsiniz.

## <a name="scale-an-app-service-plan"></a>App Service planını ölçeklendirme

App Service planının fiyatlandırma katmanını ölçeklendirmek için bkz. [Azure 'da bir uygulamayı ölçeklendirme](web-sites-scale.md).

Bir uygulamanın örnek sayısını ölçeklendirmek için bkz. [Ölçek örneği sayısı el ile veya otomatik olarak](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>App Service planını silme

Beklenmeyen ücretlerden kaçınmak için, bir App Service planındaki son uygulamayı sildiğinizde App Service planı varsayılan olarak de siler. Bunun yerine planı saklamayı seçerseniz, ücretlendirmemek için planı **boş** katmana değiştirmelisiniz.

> [!IMPORTANT]
> App Service planlar, yapılandırılmış sanal makine örneklerini ayırmaya devam ettiğinden ücretlendirmeye devam etmektedir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure 'da bir uygulamayı ölçeklendirme](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
