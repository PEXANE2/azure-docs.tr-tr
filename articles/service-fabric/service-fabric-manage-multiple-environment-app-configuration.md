---
title: Birden çok ortam için uygulamaları yönetme
description: Azure Hizmet Kumaşı uygulamaları, bir makineden binlerce makineye kadar değişen kümeler üzerinde çalıştırılabilir. Bazı durumlarda, uygulamanızı bu çeşitli ortamlar için farklı şekilde yapılandırmak isteyebilirsiniz. Bu makalede, ortam başına farklı uygulama parametrelerinin nasıl tanımlanılıgı ele asıymaktadır.
author: mikkelhegn
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 0bca690fd585b288f15cbab21c1c951474390318
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78196988"
---
# <a name="manage-applications-for-multiple-environments"></a>Birden çok ortam için uygulamaları yönetme

Azure Hizmet Kumaş kümeleri, bir ila binlerce makinenin her yerinde kümeler oluşturmanıza olanak tanır. Çoğu durumda, uygulamanızı birden çok küme yapılandırmasına dağıtmak zorunda kalırsınız: yerel geliştirme kümeniz, paylaşılan geliştirme kümeniz ve üretim kümeniz. Bu kümelerin tümü, kodunuzun çalışması gereken farklı ortamlar olarak kabul edilir. Uygulama ikilileri bu geniş spektrumda değişiklik yapılmadan çalıştırılabilir, ancak genellikle uygulamayı farklı şekilde yapılandırmak istersiniz.

İki basit örnek göz önünde bulundurun:
  - hizmetiniz tanımlı bir bağlantı noktasında dinler, ancak bu bağlantı noktasının ortamlar arasında farklı olması gerekir
  - ortamlar arasında bir veritabanı için farklı bağlama kimlik bilgileri sağlamanız gerekir

## <a name="specifying-configuration"></a>Yapılandırmayı belirtme

Sağladığınız yapılandırma iki kategoriye ayrılabilir:

- Hizmetlerinizin çalışma şekline uygulanan yapılandırma
  - Örneğin, bir bitiş noktasının bağlantı noktası numarası veya bir hizmetin örnek sayısı
  - Bu yapılandırma uygulama veya hizmet bildirimi dosyasında belirtilir
- Uygulama kodunuz için geçerli yapılandırma
  - Örneğin, bir veritabanı için bağlayıcı bilgiler
  - Bu yapılandırma yapılandırma dosyaları veya ortam değişkenleri aracılığıyla sağlanabilir

> [!NOTE]
> Uygulama ve hizmet bildirimi dosya destek parametrelerindeki tüm öznitelikler.
> Bu gibi durumlarda, dağıtım iş akışınız bir parçası olarak dizeleri değiştirme güvenmek zorunda. Azure DevOps'lerde Belirteçleri Değiştir gibi https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens bir uzantı kullanabilirsiniz: veya Jenkins'te değerleri değiştirmek için bir komut dosyası görevi çalıştırabilirsiniz.
>

## <a name="specifying-parameters-during-application-creation"></a>Uygulama oluşturma sırasında parametreleri belirtme

Hizmet Kumaşı'nda adlandırılmış bir uygulama örnekleri oluştururken, parametreleri geçirme seçeneğiniz vardır. Bunu yapma şekliniz, uygulama örneğini nasıl oluşturduğunuza bağlıdır.

  - PowerShell'de [`New-ServiceFabricApplication`](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet uygulama parametrelerini karma olarak alır.
  - Sfctl kullanarak, [`sfctl application create`](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-create) komut parametreleri JSON dizesi olarak alır. install.sh komut dosyası sfctl kullanır.
  - Visual Studio, uygulama projesindeki Parametreler klasöründe bir dizi parametre dosyası sağlar. Bu parametre dosyaları Visual Studio'dan yayımlanırken, Azure DevOps Hizmetleri veya Azure DevOps Server kullanılarak kullanılır. Visual Studio'da parametre dosyaları Deploy-FabricApplication.ps1 komut dosyasına aktarılıyor.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler, burada açıklanan bazı kavramların nasıl kullanılacağını gösterir:

- [Service Fabric'teki hizmetler için ortam değişkenleri nasıl belirtilir?](service-fabric-how-to-specify-environment-variables.md)
- [Hizmet Kumaşı'ndaki parametreleri kullanarak bir hizmetin bağlantı noktası numarası nasıl belirtilir?](service-fabric-how-to-specify-port-number-using-parameters.md)
- [Yapılandırma dosyalarını parametreleme](service-fabric-how-to-parameterize-configuration-files.md)

- [Ortam değişkeni başvurusu](service-fabric-environment-variables-reference.md)
