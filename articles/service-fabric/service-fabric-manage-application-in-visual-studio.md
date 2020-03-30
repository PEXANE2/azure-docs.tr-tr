---
title: Visual Studio'da uygulamaları yönetme
description: Azure Hizmet Kumaşı uygulama ve hizmetlerinizi oluşturmak, geliştirmek, paketlemek, dağıtmak ve hata ayıklamak için Visual Studio'yu kullanın.
author: mikkelhegn
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: mikhegn
ms.openlocfilehash: d6734f5da0fb7e5c9052b26b55b2d90b068bdbbf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614341"
---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Service Fabric uygulamalarınızı yazmayı ve yönetmeyi kolaylaştırmak için Visual Studio'yu kullanın
Azure Hizmet Kumaşı uygulamalarınızı ve hizmetlerinizi Visual Studio üzerinden yönetebilirsiniz. [Geliştirme ortamınızı ayarladıktan](service-fabric-get-started.md)sonra, Visual Studio'yu Service Fabric uygulamaları oluşturmak, hizmet eklemek veya yerel geliştirme kümenizde uygulamaları paketlemek, kaydetmek ve dağıtmak için kullanabilirsiniz.

## <a name="deploy-your-service-fabric-application"></a>Service Fabric uygulamanızı dağıtın
Varsayılan olarak, bir uygulamayı dağıtmak aşağıdaki adımları tek bir basit işlemde birleştirir:

1. Uygulama paketinin oluşturulması
2. Uygulama paketini resim deposuna yükleme
3. Başvuru türünü kaydetme
4. Çalışan uygulama örneklerini kaldırma
5. Uygulama örneği oluşturma

Visual Studio'da **F5** tuşuna basarak uygulamanızı dağır ve hata ayıklamayı tüm uygulama örneklerine bağlar. Bir uygulamayı hata ayıklamadan dağıtmak için **Ctrl+F5'i** kullanabilir veya yayımlama profilini kullanarak yerel veya uzak bir kümede yayımlayabilirsiniz.

### <a name="application-debug-mode"></a>Uygulama Hata Ayıklama Modu
Visual Studio, Hata Ayıklama'nın bir parçası olarak Visual Studios'un Uygulama dağıtımını nasıl işlemesini istediğinizi kontrol eden **Uygulama Hata Ayıklama Modu**adlı bir özellik sağlar.

#### <a name="to-set-the-application-debug-mode-property"></a>Uygulama Hata Ayıklama Modu özelliğini ayarlamak için
1. Service Fabric uygulama projesinin (*.sfproj) kısayol menüsünde **Özellikler'i** seçin (veya **F4** tuşuna basın).
2. **Özellikler** penceresinde, Uygulama **Hata Ayıklama Modu** özelliğini ayarlayın.

![Uygulama Hata Ayıklama Modu Özelliğini Ayarlama][debugmodeproperty]

#### <a name="application-debug-modes"></a>Uygulama Hata Ayıklama Modları

1. **Yenileme Uygulaması** Bu mod, kodunuzu hızlı bir şekilde değiştirmenizi ve hata ayıklamanızı sağlar ve hata ayıklama sırasında statik web dosyalarını düzenlemeyi destekler. Bu mod yalnızca yerel geliştirme kümeniz 1-Düğüm modundaysa çalışır. Bu varsayılan Uygulama Hata Ayıklama Modu'dur.
2. **Uygulama kaldır,** hata ayıklama oturumu sona erdiğinde uygulamanın kaldırılmasına neden olur.
3. **Otomatik Yükseltme** Hata ayıklama oturumu sona erdiğinde uygulama çalıştırmaya devam ediyor. Bir sonraki hata ayıklama oturumu dağıtımı yükseltme olarak ele alacaktır. Yükseltme işlemi, önceki hata ayıklama oturumunda girdiğiniz tüm verileri korur.
4. **Uygulamayı Koru** Hata ayıklama oturumu sona erdiğinde uygulama kümede çalışmaya devam eder. Bir sonraki hata ayıklama oturumunun başında, uygulama kaldırılır.

**Otomatik Yükseltme** için veriler Hizmet Kumaş uygulama yükseltme yetenekleri uygulanarak korunur. Uygulamaları yükseltme ve gerçek bir ortamda yükseltmeyi nasıl gerçekleştirebileceğinhakkında daha fazla bilgi için [Service Fabric uygulama yükseltmesi'ne](service-fabric-application-upgrade.md)bakın.

## <a name="add-a-service-to-your-service-fabric-application"></a>Service Fabric uygulamanıza hizmet ekleme
Uygulamanızın işlevselliğini genişletmek için yeni hizmetler ekleyebilirsiniz. Hizmetin uygulama paketinize dahil olduğundan emin olmak için, hizmeti **Yeni Kumaş Hizmeti...** menü öğesi ne ekleyin.

![Yeni bir Service Fabric hizmeti ekleme][newservice]

Uygulamanıza eklemek için bir Hizmet Kumaşı proje türünü seçin ve hizmet için bir ad belirtin.  Bkz. Hangi hizmet türünü kullanacağınıza karar vermenize yardımcı olacak [hizmetinizle ilgili bir çerçeve seçme.](service-fabric-choose-framework.md)

![Uygulamanıza eklemek için bir Service Fabric servis proje türü seçin][addserviceproject]

Yeni hizmet, çözümünüzüze ve mevcut uygulama paketinize eklenir. Hizmet başvuruları ve varsayılan hizmet örneği uygulama bildirimine eklenarak hizmetin oluşturulmasına ve uygulamayı bir sonraki dağıtımında başlatılmasına neden olur.

![Yeni hizmet başvuru bildiriminize eklenir][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Servis Kumaşı uygulamanızı paketle
Uygulamayı ve hizmetlerini bir kümeye dağıtmak için bir uygulama paketi oluşturmanız gerekir.  Paket, uygulama bildirimini, hizmet bildirimlerini ve diğer gerekli dosyaları belirli bir düzende düzenler.  Visual Studio, 'pkg' dizininde, uygulama projesinin klasöründe paketi kurar ve yönetir.  **Uygulama** bağlamı menüsünden **Paket'i** tıklatmak uygulama paketini oluşturur veya güncelleştirir.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Bulut Gezgini'ni kullanarak uygulamaları ve uygulama türlerini kaldırma
**Görünüm** menüsünden başlatabileceğiniz Cloud Explorer'ı kullanarak Visual Studio içinden temel küme yönetimi işlemlerini gerçekleştirebilirsiniz. Örneğin, yerel veya uzak kümelerde uygulamaları ve hükümsüz uygulama türlerini silebilirsiniz.

![Uygulamayı kaldırma][removeapplication]

> [!TIP]
> Daha zengin bir küme yönetimi işlevi [için, Service Fabric Explorer ile kümenizi Görselleştirme'ye](service-fabric-visualizing-your-cluster.md)bakın.
>
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Servis Kumaş ı uygulama modeli](service-fabric-application-model.md)
* [Hizmet Kumaş uygulama dağıtım](service-fabric-deploy-remove-applications.md)
* [Birden çok ortam için uygulama parametrelerini yönetme](service-fabric-manage-multiple-environment-app-configuration.md)
* [Servis Kumaşı uygulamanızın hata ayıklanması](service-fabric-debugging-your-application.md)
* [Service Fabric Explorer'ı kullanarak kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png
