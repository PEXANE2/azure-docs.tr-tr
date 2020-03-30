---
title: JavaScript uygulamaları için kaynak harita desteği - Azure Monitor Uygulama Öngörüleri
description: Uygulama Öngörüleri'ni kullanarak kaynak haritalarını kendi depolama hesabınız olan Blob kapsayıcınıza nasıl yükleyebilirsiniz öğrenin.
ms.topic: conceptual
author: markwolff
ms.author: marwolff
ms.date: 03/04/2020
ms.openlocfilehash: 4b452b31338760a8f53eed54420319101836bc00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474892"
---
# <a name="source-map-support-for-javascript-applications"></a>JavaScript uygulamaları için kaynak harita desteği

Application Insights, kaynak haritaların kendi Depolama Hesabı Blob Konteynerinize yüklenmesini destekler.
Kaynak eşlemler, işlem ayrıntıları sayfasının sonunda bulunan arama yığınlarını en aza indirgemeyen ler için kullanılabilir. [JavaScript SDK][ApplicationInsights-JS] veya [Node.js SDK][ApplicationInsights-Node.js] tarafından gönderilen herhangi bir istisna kaynak haritalar ile unminified olabilir.

![Bir Depolama Hesabı'na bağlanarak Çağrı Yığınını Unminify](./media/source-map-support/details-unminify.gif)

## <a name="create-a-new-storage-account-and-blob-container"></a>Yeni bir depolama hesabı ve Blob kapsayıcısı oluşturma

Zaten varolan bir depolama hesabınız veya blob kapsayıcınız varsa, bu adımı atlayabilirsiniz.

1. [Yeni depolama hesabı oluşturma][create storage account]
2. Depolama hesabınızda [bir blob kapsayıcısı oluşturun.][create blob container] Kaynak haritalarınızın herkese açık olmamasını `Private`sağlamak için "Genel erişim düzeyini" ayarladığından emin olun.

> [!div class="mx-imgBorder"]
>![Konteyner erişim düzeyiniz Özel olarak ayarlanmalıdır](./media/source-map-support/container-access-level.png)

## <a name="push-your-source-maps-to-your-blob-container"></a>Kaynak haritalarınızı Blob kabınıza itin

Kaynak haritalarınızı otomatik olarak yapılandırılmış Blob kapsayıcısına yükecek şekilde yapılandırarak sürekli dağıtım ardışık ardınızı depolama hesabınızla entegre etmelisiniz. Kaynak haritalarınızı Blob kapsayıcısındaki bir alt klasöre yüklememelisiniz; şu anda kaynak harita yalnızca kök klasöründen getirilecektir.

### <a name="upload-source-maps-via-azure-pipelines-recommended"></a>Kaynak haritalarını Azure Ardışık Hatları ile yükleme (önerilir)

Uygulamanızı sürekli olarak oluşturmak ve dağıtmak için Azure Ardışık Hatları kullanıyorsanız, kaynak haritalarınızı otomatik olarak yüklemek için ardınıza bir [Azure Dosya Kopyalama][azure file copy] görevi ekleyin.

> [!div class="mx-imgBorder"]
> ![Kaynak haritalarınızı Azure Blob Depolamasına yüklemek için Ardınıza Azure Dosya Kopyalama görevi ekleyin](./media/source-map-support/azure-file-copy.png)

## <a name="configure-your-application-insights-resource-with-a-source-map-storage-account"></a>Uygulama Öngörüleri kaynağınızı Kaynak Harita depolama hesabıyla yapılandırın

### <a name="from-the-end-to-end-transaction-details-page"></a>Uçtan uca işlem ayrıntıları sayfasından

Uçtan uca işlem ayrıntıları sekmesinden, *Unminify'i* tıklatabilirsiniz ve kaynağınız yapılandırılmamışsa yapılandırmaemri görüntülenir.

1. Portalda, minified bir özel durum ayrıntılarını görüntüleyin.
2. *Unminify'e* tıklayın
3. Kaynağınız yapılandırılmamışsa, yapılandırmanızı isteyen bir ileti görüntülenir.

### <a name="from-the-properties-page"></a>Özellikler sayfasından

Uygulama Öngörüleri Kaynağınıza bağlı depolama hesabını veya Blob kapsayıcısını yapılandırmak veya değiştirmek istiyorsanız, bunu Application Insights kaynağının *Özellikler* sekmesini görüntüleyerek yapabilirsiniz.

1. Application Insights kaynağınızın *Özellikler* sekmesine gidin.
2. Kaynak *haritası blob konteyner değiştir'e*tıklayın.
3. Kaynak haritalı kapsayıcı olarak farklı bir Blob kapsayıcıseçin.
4. `Apply` öğesine tıklayın.

> [!div class="mx-imgBorder"]
> ![Özellikler Blade'e giderek seçtiğiniz Azure Blob Konteynerini yeniden yapılandırın](./media/source-map-support/reconfigure.png)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="required-role-based-access-control-rbac-settings-on-your-blob-container"></a>Blob kapsayıcınızda gerekli rol tabanlı erişim denetimi (RBAC) ayarları

Portal'da bu özelliği kullanan herhangi bir kullanıcı en azından Blob kapsayıcınıza [Depolama Blob Veri Okuyucusu][storage blob data reader] olarak atanmalıdır. Bu rolü, bu özellik aracılığıyla kaynak eşlemleri kullanacak olan herkese atamanız gerekir.

> [!NOTE]
> Kapsayıcının nasıl oluşturulduğuna bağlı olarak, bu işlem size veya ekibinize otomatik olarak atanmamış olabilir.

### <a name="source-map-not-found"></a>Kaynak harita bulunamadı

1. İlgili kaynak haritasının doğru blob kapsayıcısına yüklendiğini doğrulayın
2. Kaynak harita dosyasının adını eşlenere eşlenere sahip `.map`JavaScript dosyasından alan ve suffixed ile doğrulayın.
    - Örneğin, `/static/js/main.4e2ca5fa.chunk.js` adlı blob için arayacaktır`main.4e2ca5fa.chunk.js.map`
3. Herhangi bir hata günlüğe kaydedilip kaydedilmemekte olup olmadığını görmek için tarayıcınızın konsolunu kontrol edin. Bunu herhangi bir destek biletine ekleyin.

## <a name="next-steps"></a>Sonraki Adımlar

* [Azure Dosya Kopyalama görevi](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-file-copy?view=azure-devops)


<!-- Remote URLs -->
[create storage account]: https://docs.microsoft.com/azure/storage/common/storage-account-create?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal
[create blob container]: https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal
[storage blob data reader]: https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader
[ApplicationInsights-JS]: https://github.com/microsoft/applicationinsights-js
[ApplicationInsights-Node.js]: https://github.com/microsoft/applicationinsights-node.js
[azure file copy]: https://aka.ms/azurefilecopyreadme