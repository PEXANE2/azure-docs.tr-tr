---
title: Azure Hizmet Kumaş Kafesi örneklerini bul
description: Burada kullanılabilir Hizmet Kumaş Örgü örnek uygulamaların bir dizin's. Bu örneklerdeki kaynak kodu, Hizmet Kumaşı Kaynak Modeli'ni kullanarak belirli bir senaryonun nasıl elde edilebildiğini gösterir.
ms.date: 12/03/2018
ms.topic: conceptual
ms.openlocfilehash: c944ddc26f2e2d099cf9552acb8287c363d0c768
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75461915"
---
# <a name="find-service-fabric-mesh-samples"></a>Servis Kumaş Örgü Örnekleri Bul

Bu tablo, kullanılabilir Hizmet Kumaş Örgü örnek uygulamaları özetliyor. Bu örneklerdeki kaynak kodu, Hizmet Kumaşı Kaynak Modeli'ni kullanarak belirli bir senaryonun nasıl elde edilebildiğini gösterir.

Şablonları doğrudan Azure'a dağıtma hakkında daha fazla bilgi için [örnek şablon GitHub sayfasına bakın.](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/README.md)

|Örnek Şablon|Senaryo Açıklaması|Kaynak Kodu|Geliştirici Araçları|
|------------|--------------------|----------|----------------------|
| [Merhaba Dünya Uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/helloworld) | Statik web sayfası bir kapsayıcıda barındırılan. Linux için nginx kullanır, Windows IIS için | [Kaynak Kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) | Hiçbir gereksinim yok |
| [Azure Dosya Birimleri için Sayaç Uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.md) | Azure Dosyaları tabanlı birimi kapsayıcının içine monte ederek durumu depolayın. <br><br> **Not:** Bu şablon, zaten sağlanan [Talimatlar](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share) için bir Azure Dosyaları dosya paylaşımı gerektirir | [Kaynak Kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Görsel Stüdyo Örgü Takım |
| [TodoListApp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/todolist) | DNS tabanlı çözünürlüğü kullanan ön uç ve arka uç hizmeti içeren bir uygulama oluşturun. [Burada](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-tutorial-create-dotnetcore) bir öğretici olarak kullanılır | [Kaynak Kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/todolistapp) | Görsel Stüdyo Örgü Takım |
| [Görsel Nesneler Uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/visualobjects) | Bir uygulama içindeki mikro hizmetleri ölçeklendirin ve yükseltin. | [Kaynak Kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) |  Görsel Stüdyo Örgü Takım |
| [Oylama Uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/voting) | DNS tabanlı çözünürlüğü kullanan ön uç ve arka uç hizmeti içeren bir uygulama oluşturma | [Kaynak Kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) | Windows sürümü için Visual Studio Mesh Tooling, VS Kodu / dotnet CLI Linux sürümü için kullanılabilir |
| [Servis Kumaşı Güvenilir Hacimler Için Sayaç Uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.sfreliablevolume.md)| Servis Kumaş ı Güvenilir Disk tabanlı hacmi konteynerin içine monte ederek durumu depolayın.| [Kaynak Kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Görsel Stüdyo Örgü Takım |
