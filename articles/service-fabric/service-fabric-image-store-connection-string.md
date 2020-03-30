---
title: Azure Service Fabric resim deposu bağlantı dizesi
description: Kullanımları ve Hizmet Kumaşı kümesine uygulamaları da dahil olmak üzere görüntü deposu bağlantı dizesi hakkında bilgi edinin.
author: alexwun
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: alexwun
ms.openlocfilehash: c3395248188c2a16736cfc8cea262fe163a6944b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645676"
---
# <a name="understand-the-imagestoreconnectionstring-setting"></a>ImageStoreConnectionString ayarını anlama

Bazı belgelerimizde, gerçekten ne anlama geldiğini tanımlamadan bir "ImageStoreConnectionString" parametresinin varlığından kısaca söz ediyoruz. Ve [PowerShell kullanarak Dağıt ve uygulamaları kaldırmak][10]gibi bir makale geçtikten sonra, tüm yaptığınız kopya / hedef kümenin küme manifestosunda gösterildiği gibi değeri yapıştırmak gibi görünüyor. Bu nedenle ayar küme başına yapılandırılabilir olmalıdır, ancak [Azure portalı][11]üzerinden bir küme oluşturduğunuzda, bu ayarı yapılandırma seçeneği yoktur ve her zaman "kumaş:ImageStore". O zaman bu ayarın amacı nedir?

![Küme Manifestosu][img_cm]

Service Fabric birçok farklı ekipler tarafından dahili Microsoft tüketimi için bir platform olarak başladı, bu yüzden bazı yönleri son derece özelleştirilebilir - "Image Store" böyle bir yönüdür. Esasen, Image Store uygulama paketlerini depolamak için takılabilir bir depodur. Uygulamanız kümedeki bir düğüme dağıtıldığında, bu düğüm uygulama paketinizin içeriğini Resim Deposu'ndan indirir. ImageStoreConnectionString, belirli bir küme için doğru Resim Deposu'nu bulmak için hem istemciler hem de düğümler için gerekli tüm bilgileri içeren bir ayardır.

Şu anda Üç olası Görüntü Deposu sağlayıcısı türü vardır ve bunların karşılık gelen bağlantı dizeleri aşağıdaki gibidir:

1. Resim Mağaza Servisi: "kumaş:ImageStore"

2. Dosya Sistemi: "dosya:[dosya sistem yolu]"

3. Azure Depolama: "xstore:DefaultEndpointsProtocol=https; Hesap Adı=[...]; Hesap Anahtarı=[...]; Konteyner=[...]"

Üretimde kullanılan sağlayıcı türü, Service Fabric Explorer'dan görebileceğiniz, kalıcı bir sistem hizmeti olan Image Store Hizmeti'dir. 

![Resim Mağaza Servisi][img_is]

Image Store'u kümenin kendi içinde bir sistem hizmetinde barındırmak, paket deposuiçin dışa bağımlılıkları ortadan kaldırır ve depolama nın yerelliği üzerinde daha fazla kontrol sahibi olmamızı sağlar. Image Store'daki gelecekteki geliştirmelerin, yalnızca olmasa da, önce Image Store sağlayıcısını hedeflemesi olasıdır. İstemci zaten hedef kümeye bağlı olduğundan, Görüntü Deposu Hizmeti sağlayıcısının bağlantı dizesi benzersiz bir bilgiye sahip değildir. İstemcinin yalnızca sistem hizmetini hedefleyen protokollerin kullanılması gerektiğini bilmesi yeterlidir.

Dosya Sistemi sağlayıcısı, geliştirme sırasında yerel tek kutulu kümeler için Görüntü Deposu Hizmeti yerine kümeyi biraz daha hızlı önyükleme için kullanılır. Fark genellikle küçük, ama geliştirme sırasında çoğu millet için yararlı bir optimizasyon's. Diğer depolama sağlayıcısı türleri ile de yerel bir tek kutu küme dağıtmak mümkündür, ancak geliştirme/test iş akışı sağlayıcıdan bağımsız olarak aynı kaldığından, genellikle bunu yapmak için bir neden yoktur. Azure Depolama sağlayıcısı yalnızca Image Store Service sağlayıcısı kullanılmadan önce dağıtılan eski kümelerin eski desteği için bulunur.

Ayrıca, Dosya Sistemi sağlayıcısı veya Azure Depolama sağlayıcısı, bir Görüntü Deposu'nu birden çok küme arasında paylaşma yöntemi olarak kullanılmamalıdır - bu durum küme yapılandırma verilerinin bozulmasına neden olur, çünkü her küme Görüntü'ye çakışan veriler yazabilir Mağazası. Sağlanan uygulama paketlerini birden çok küme arasında paylaşmak için, bunun yerine indirme URI ile herhangi bir harici mağazaya yüklenebilen [sfpkg][12] dosyalarını kullanın.

Yani ImageStoreConnectionString yapılandırılabilir iken, sadece varsayılan ayarı kullanın. Visual Studio aracılığıyla Azure'da yayımlanırken, parametre sizin için otomatik olarak buna göre ayarlanır. Azure'da barındırılan kümelere programatik dağıtım için bağlantı dizesi her zaman "kumaş:ImageStore" olur. Şüphe yemesine rağmen, değeri her zaman [PowerShell](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclustermanifest), [.NET](https://msdn.microsoft.com/library/azure/mt161375.aspx), veya [REST](https://docs.microsoft.com/rest/api/servicefabric/get-a-cluster-manifest)tarafından küme manifestosu alarak doğrulanabilir . Hem şirket içi test hem de üretim kümeleri her zaman Image Store Service sağlayıcısını kullanacak şekilde yapılandırılmalıdır.

### <a name="next-steps"></a>Sonraki adımlar
[PowerShell kullanarak uygulamaları dağıtma ve kaldırma][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-package-apps.md#create-an-sfpkg
