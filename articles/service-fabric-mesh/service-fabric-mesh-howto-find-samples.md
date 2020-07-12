---
title: Azure Service Fabric kafes örneklerini bulma
description: Kullanılabilir Service Fabric kafes örnek uygulamalarının bir dizini aşağıda verilmiştir. Bu örneklerdeki kaynak kodu, Service Fabric kaynak modeli kullanılarak belirli bir senaryonun nasıl elde alınacağını gösterir.
ms.date: 12/03/2018
ms.topic: conceptual
ms.openlocfilehash: 64d9e4e112f26f83eedd02b034c8191c6f677618
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261283"
---
# <a name="find-service-fabric-mesh-samples"></a>Service Fabric ağ örneklerini bulma

Bu tabloda, kullanılabilir Service Fabric kafes örnek uygulamaları özetlenmektedir. Bu örneklerdeki kaynak kodu, Service Fabric kaynak modeli kullanılarak belirli bir senaryonun nasıl elde alınacağını gösterir.

Şablonları doğrudan Azure 'a dağıtma hakkında daha fazla bilgi için bkz [. örnek şablon GitHub sayfası.](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/README.md)

|Örnek şablon|Senaryo Açıklaması|Kaynak kodu|Geliştirici Araçları|
|------------|--------------------|----------|----------------------|
| [Merhaba Dünya uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/helloworld) | Kapsayıcıda barındırılan statik Web sayfası. Linux için Windows IIS için NGINX kullanır | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) | Gereksinim yok |
| [Azure dosya birimleri için sayaç uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.md) | Kapsayıcının içindeki Azure dosya tabanlı birimi bağlayarak durumu saklayın. <br><br> **Note:** Bu şablon, zaten sağlanmış [yönergeler](../storage/files/storage-how-to-create-file-share.md) Için bir Azure dosyaları dosya paylaşımında gereklidir | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Visual Studio kafes araçları |
| [TodoListApp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/todolist) | DNS tabanlı çözüm kullanan ön uç ve arka uç hizmetiyle bir uygulama oluşturun. [Burada](./service-fabric-mesh-tutorial-create-dotnetcore.md) öğretici olarak kullanılır | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/todolistapp) | Visual Studio kafes araçları |
| [Görsel nesneler uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/visualobjects) | Bir uygulama içindeki mikro hizmetleri ölçeklendirin ve yükseltin. | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) |  Visual Studio kafes araçları |
| [Oylama uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/voting) | DNS tabanlı çözüm kullanan ön uç ve arka uç hizmetiyle uygulama oluşturma | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) | Windows sürümü için Visual Studio kafes araçları, VS Code/DotNet CLı, Linux sürümü için kullanılabilir |
| [Service Fabric güvenilir birimler için sayaç uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.sfreliablevolume.md)| Kapsayıcının içinde güvenilir disk tabanlı birim Service Fabric bağlayarak durumu saklayın.| [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Visual Studio kafes araçları |
