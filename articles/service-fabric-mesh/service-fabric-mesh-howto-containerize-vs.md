---
title: Mevcut bir .NET uygulamasını Service Fabric Mesh için kapsayıcılı hale getirme
description: Tam .NET çerçevesini kullanan ASP.NET ve Konsol projelerine Hizmet Kumaş Kafesi konteyner düzenleme desteği ekleyin.
author: dkkapur
ms.author: dekapur
ms.date: 11/08/2018
ms.topic: conceptual
ms.openlocfilehash: d67ea5bb7df5910ec87e69adf3c414c303bf0182
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75462039"
---
# <a name="containerize-an-existing-net-app-for-service-fabric-mesh"></a>Mevcut bir .NET uygulamasını Service Fabric Mesh için kapsayıcılı hale getirme

Bu makalede, mevcut bir .NET uygulamasına Service Fabric Mesh konteyner düzenleme desteği nasıl ekleyeceğinizi gösterilmektedir.

Visual Studio 2017'de, tam .NET çerçevesini kullanan ASP.NET ve Konsol projelerine kapsayıcıizasyon desteği ekleyebilirsiniz.

> [!NOTE]
> .NET **Core** projeleri şu anda desteklenmemektedir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturabilirsiniz.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

* [Geliştirme ortamınızı ayarladığınızdan](service-fabric-mesh-howto-setup-developer-environment-sdk.md)emin olun. Buna Service Fabric çalışma zamanı, SDK, Docker, Visual Studio 2017'yi yüklemek ve yerel bir küme oluşturmak dahildir.

## <a name="open-an-existing-net-app"></a>Varolan bir .NET uygulamasını açma

Konteyner düzenleme desteği eklemek istediğiniz uygulamayı açın.

Bir örnek denemek isterseniz, [eShop](https://github.com/MikkelHegn/ContainersSFLab) kod örneğini kullanabilirsiniz. Bu makalenin geri kalanı, bu adımları kendi projenize uygulayabilirsiniz rağmen, bu projeyi kullandığımızı varsayar.

**eShop** projesinin bir kopyasını alın:

```git
git clone https://github.com/MikkelHegn/ContainersSFLab.git
```

Bir kez indirdi, Visual Studio 2017 açık **ContainersSFLab\eShopLegacyWebFormsSolution\eShopLegacyWebForms.sln**.

## <a name="add-container-support"></a>Konteyner desteği ekle
 
Service Fabric Mesh araçlarını kullanarak varolan bir ASP.NET veya Konsol projesine konteyner düzenleme desteği ekleyin:

Visual Studio çözüm gezgininde, proje adını sağ tıklatın (örnekte, **eShopLegacyWebForms)** ve ardından**Konteyner Orchestrator Desteği** **Ekle'yi** > seçin.
**Kapsayıcı Düzenek Desteği Ekle** iletişim kutusu görüntülenir.

![Visual Studio konteyner orchestrator iletişim kutusu ekleyin](./media/service-fabric-mesh-howto-containerize-vs/add-container-orchestration-support.png)

Açılan dan **Servis Kumaş Kafesi'ni** seçin ve ardından **Tamam'ı**tıklatın.

Araç daha sonra Docker'ın yüklü olduğunu doğrular, projenize bir Dockerfile ekler ve projeniz için bir docker görüntüsü çeker.  
Çözümünüze Bir Servis Kumaş ı uygulama projesi eklenir. Mesh yayımlama profillerinizi ve yapılandırma dosyalarınızı içerir. Projenin adı proje adınız ile aynıdır, 'Uygulama' sonuna kadar concatenated ile, örneğin, **eShopLegacyWebFormsApplication**. 

Yeni Mesh projesinde dikkat edilmesi gereken iki klasör görürsünüz:
- Ağ gibi ek Mesh kaynaklarını açıklayan YAML dosyaları içeren **Uygulama Kaynakları.**
- **Uygulamanızın** dağıtıldığında nasıl çalışması gerektiğini açıklayan bir service.yaml dosyası içeren Hizmet Kaynakları.

Uygulamanıza konteyner düzenleme desteği eklendikten sonra, yerel Service Fabric Mesh kümenizde .NET uygulamanızı hata ayıklamak için **F5** tuşuna basabilirsiniz. Burada bir Service Fabric Mesh küme üzerinde çalışan eShop ASP.NET uygulaması: 

![eShop uygulaması](./media/service-fabric-mesh-howto-containerize-vs/eshop-running.png)

Artık uygulamayı Azure Hizmet Kumaş Kafesi'nde yayınlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric Mesh'e uygulama nın nasıl yayınlayacağına bakın: [Öğretici- Servis Kumaş Kafesi uygulamasını dağıtın](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)