---
title: JavaScript uygulamaları için kaynak eşleme desteği-Azure Izleyici Application Insights
description: Application Insights kullanarak, kendi depolama hesabı blobu kapsayıcınıza kaynak haritaları yüklemeyi öğrenin.
ms.topic: conceptual
author: markwolff
ms.author: marwolff
ms.date: 03/04/2020
ms.openlocfilehash: 4b452b31338760a8f53eed54420319101836bc00
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79474892"
---
# <a name="source-map-support-for-javascript-applications"></a>JavaScript uygulamaları için kaynak eşleme desteği

Application Insights, kaynak eşlemelerinin kendi depolama hesabı blobu kapsayıcınıza yüklenmesini destekler.
Kaynak haritaları, uçtan uca işlem ayrıntıları sayfasında bulunan çağrı yığınlarını kaldırmak için kullanılabilir. [JavaScript SDK 'sı][ApplicationInsights-JS] veya [Node. js SDK 'sı][ApplicationInsights-Node.js] tarafından gönderilen herhangi bir özel durum, kaynak eşlemeleriyle korumasız olabilir.

![Bir depolama hesabıyla bağlantı kurarak çağrı yığınını geri alma](./media/source-map-support/details-unminify.gif)

## <a name="create-a-new-storage-account-and-blob-container"></a>Yeni bir depolama hesabı ve BLOB kapsayıcısı oluşturun

Zaten mevcut bir depolama hesabınız veya blob kapsayıcınız varsa, bu adımı atlayabilirsiniz.

1. [Yeni depolama hesabı oluşturma][create storage account]
2. Depolama hesabınızda [bir blob kapsayıcısı oluşturun][create blob container] . Kaynak Haritalarınızın genel olarak erişilebilir olmamasını sağlamak için "genel `Private`erişim düzeyi" seçeneğini olarak ayarladığınızdan emin olun.

> [!div class="mx-imgBorder"]
>![Kapsayıcı erişim düzeyiniz özel olarak ayarlanmalıdır](./media/source-map-support/container-access-level.png)

## <a name="push-your-source-maps-to-your-blob-container"></a>Kaynak haritalarınızı blob kapsayıcınıza gönderin

Kaynak haritalarınızı yapılandırılan blob kapsayıcısına otomatik olarak yükleyecek şekilde yapılandırarak, sürekli dağıtım işlem hattınızı depolama hesabınızla tümleştirmelisiniz. Kaynak haritalarınızı blob kapsayıcısındaki bir alt klasöre yüklememelisiniz; Şu anda kaynak eşleme yalnızca kök klasörden alınacaktır.

### <a name="upload-source-maps-via-azure-pipelines-recommended"></a>Kaynak haritalarını Azure Pipelines aracılığıyla karşıya yükle (önerilir)

Uygulamanızı sürekli olarak derlemek ve dağıtmak için Azure Pipelines kullanıyorsanız, kaynak haritalarınızı otomatik olarak yüklemek için işlem hattınıza bir [Azure dosya kopyalama][azure file copy] görevi ekleyin.

> [!div class="mx-imgBorder"]
> ![Kaynak haritalarınızı Azure Blob depolamaya yüklemek için işlem hattınıza bir Azure dosya kopyalama görevi ekleyin](./media/source-map-support/azure-file-copy.png)

## <a name="configure-your-application-insights-resource-with-a-source-map-storage-account"></a>Kaynak eşleme depolama hesabıyla Application Insights kaynağınızı yapılandırma

### <a name="from-the-end-to-end-transaction-details-page"></a>Uçtan uca işlem ayrıntıları sayfasından

Uçtan uca işlem ayrıntıları sekmesinden, en *azal* ' a tıklayabilirsiniz ve kaynağınızın yapılandırılmamış olması halinde yapılandırmak için bir istem görüntülenir.

1. Portalda, mini kullanılan bir özel durumun ayrıntılarını görüntüleyin.
2. En *azal* seçeneğine tıklayın
3. Kaynağınız yapılandırılmamışsa, yapılandırmak isteyip istemediğinizi soran bir ileti görüntülenir.

### <a name="from-the-properties-page"></a>Özellikler sayfasından

Application Insights kaynağınız ile bağlantılı depolama hesabı veya blob kapsayıcısını yapılandırmak veya değiştirmek isterseniz, bunu Application Insights kaynağının *Özellikler* sekmesini görüntüleyerek yapabilirsiniz.

1. Application Insights kaynağınızın *Özellikler* sekmesine gidin.
2. *Kaynak eşleme blobu kapsayıcısını Değiştir*' e tıklayın.
3. Kaynak haritaları Kapsayıcınız olarak farklı bir blob kapsayıcısı seçin.
4. `Apply` öğesine tıklayın.

> [!div class="mx-imgBorder"]
> ![Özellikler dikey penceresine giderek seçtiğiniz Azure Blob kapsayıcınızı yeniden yapılandırın](./media/source-map-support/reconfigure.png)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="required-role-based-access-control-rbac-settings-on-your-blob-container"></a>Blob kapsayıcıda gerekli rol tabanlı erişim denetimi (RBAC) ayarları

Bu özelliği kullanan portaldaki herhangi bir kullanıcının blob kapsayıcısına en az bir [Depolama Blobu veri okuyucusu][storage blob data reader] olarak atanması gerekir. Bu rolü, bu özellik aracılığıyla kaynak haritaları kullanacak başka bir kişiye atamanız gerekir.

> [!NOTE]
> Kapsayıcının nasıl oluşturulduğuna bağlı olarak, bu otomatik olarak size veya ekibinize atanmamış olabilir.

### <a name="source-map-not-found"></a>Kaynak eşlemesi bulunamadı

1. Karşılık gelen kaynak eşlemesinin doğru blob kapsayıcısına yüklendiğini doğrulayın
2. Kaynak eşleme dosyasının eşlendiği JavaScript dosyasından sonra adlandırılmış olduğunu doğrulayın `.map`.
    - Örneğin, `/static/js/main.4e2ca5fa.chunk.js` adlı blobu arama yapılacak`main.4e2ca5fa.chunk.js.map`
3. Günlüğe yazılan herhangi bir hata olup olmadığını görmek için tarayıcınızın konsoluna bakın. Bunu herhangi bir destek biletinde ekleyin.

## <a name="next-steps"></a>Sonraki Adımlar

* [Azure dosya kopyalama görevi](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-file-copy?view=azure-devops)


<!-- Remote URLs -->
[create storage account]: https://docs.microsoft.com/azure/storage/common/storage-account-create?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal
[create blob container]: https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal
[storage blob data reader]: https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader
[ApplicationInsights-JS]: https://github.com/microsoft/applicationinsights-js
[ApplicationInsights-Node.js]: https://github.com/microsoft/applicationinsights-node.js
[azure file copy]: https://aka.ms/azurefilecopyreadme