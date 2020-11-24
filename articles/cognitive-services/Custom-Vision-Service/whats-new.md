---
title: Özel Görüntü İşleme yenilikler nelerdir?
titleSuffix: Azure Cognitive Services
description: Bu makale Özel Görüntü İşleme hakkındaki haberleri içerir.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 11/23/2020
ms.author: pafarley
ms.openlocfilehash: 57f3cf0cb15243d054da0111366f3a1dc0fb5349
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95739766"
---
# <a name="whats-new-in-custom-vision"></a>Özel Görüntü İşleme yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Bu öğeler sürüm notları, videolar, blog gönderileri ve diğer bilgi türleri olabilir. Hizmetle güncel kalmasını sağlamak için bu sayfaya yer işareti ekleyin.


## <a name="october-2020"></a>Ekim 2020 

### <a name="custom-base-model"></a>Özel temel model

- Bazı uygulamalarda çok sayıda ek eğitim verisi vardır ancak modellerini ayrı olarak hassas ayarlamalar gerekir; Bu, küçük farklılıklar içeren farklı kaynaklardaki görüntülerin daha iyi performansa neden olur. Bu durumda, ilk modeli, büyük bir eğitim verileri hacmiyle her zamanki gibi eğitebilirsiniz. Ardından, istek gövdesinde _CustomBaseModelInfo_ ile 3,4 genel önizleme API 'Sindeki **traınproject** 'i çağırın. Kaynak proje ve aşağı akış hedef projesinde benzer görüntü özellikleri varsa, daha iyi performans sağlayabilirsiniz. 

### <a name="new-domain-information"></a>Yeni etki alanı bilgileri

- Özel Görüntü İşleme 3,4 genel önizleme API 'sindeki **Getdomains** tarafından döndürülen etki alanı bilgileri artık desteklenen verilebilir platformları, model mimarisinin kısa bir açıklamasını ve Compact etki alanlarının modelinin boyutunu içerir.

### <a name="training-divergence-feedback"></a>Eğitim farklılaşma geri bildirimi

- Özel Görüntü İşleme Özel Görüntü İşleme 3,4 genel önizleme API 'SI artık **Getıteration** çağrısından **TrainingErrorDetails** döndürüyor. Başarısız yinelemelerde, bu hatanın eğitim ağırlıklı gösterimi tarafından kaynaklanıp kaynaklanmadığını ortaya koyar. Bu, daha fazla ve daha yüksek kaliteli eğitim verileriyle giderilebilir.

## <a name="july-2020"></a>Temmuz 2020

### <a name="azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi

* Özel Görüntü İşleme, Azure kaynaklarına bireysel erişimi yönetmeye yönelik bir yetkilendirme sistemi olan Azure rol tabanlı erişim denetimi 'ni (Azure RBAC) destekler. Özel Görüntü İşleme projelerinize erişimi yönetmeyi öğrenmek için bkz. [Azure rol tabanlı erişim denetimi](./role-based-access-control.md).

### <a name="subset-training"></a>Alt küme eğitimi

* Bir nesne algılama projesi eğitimi yaparken, isteğe bağlı olarak yalnızca uygulanan etiketlerin bir alt kümesini eğitebilirsiniz. Yalnızca belirli etiketlerden yeterince birini uygulamadıysanız bunu yapmak isteyebilirsiniz, ancak bu, başkalarının yeterli olması durumunda. Daha fazla bilgi edinmek için C# veya Python için [istemci kitaplığı hızlı](./quickstarts/object-detection.md) başlangıcını izleyin.

### <a name="azure-storage-notifications"></a>Azure depolama bildirimleri

* Proje Eğitimi/dışarı aktarma etkinliğinin ve yayımlanan modellerin yedek kopyalarının anında bildirimlerini almak için Özel Görüntü İşleme projenizi bir Azure Blob depolama kuyruğu ile tümleştirebilirsiniz. Bu özellik, uzun işlemler çalışırken hizmetin sonuçları sürekli olarak yoklanmaması için yararlıdır. Bunun yerine, depolama kuyruğu bildirimlerini iş akışınız ile tümleştirebilirsiniz. Daha fazla bilgi edinmek için [Depolama Tümleştirme](./storage-integration.md) kılavuzuna bakın.

### <a name="copy-and-move-projects"></a>Projeleri kopyalama ve taşıma

* Artık, projeleri bir Özel Görüntü İşleme hesabından başkalarına kopyalayabilirsiniz. Bir projeyi geliştirmeden üretim ortamına taşımak veya daha fazla veri güvenliği için bir projeyi farklı bir Azure bölgesindeki bir hesaba yedeklemek isteyebilirsiniz. Daha fazla bilgi edinmek için [projeleri kopyalama ve taşıma](./copy-move-projects.md) kılavuzuna bakın.

## <a name="september-2019"></a>Eylül 2019

### <a name="suggested-tags"></a>Önerilen Etiketler

* [Özel görüntü işleme Web sitesindeki](https://www.customvision.ai/) akıllı etiketleyici Aracı, eğitim görüntüleriniz için önerilen Etiketler oluşturur. Bu, bir Özel Görüntü İşleme modeline eğitim yaparken çok sayıda görüntüyü daha hızlı etiketlemenize olanak sağlar. Bu özelliği kullanma hakkında yönergeler için bkz. [Önerilen Etiketler](./suggested-tags.md).

## <a name="cognitive-service-updates"></a>Bilişsel hizmet güncelleştirmeleri

[Bilişsel hizmetler için Azure Update duyuruları](https://azure.microsoft.com/updates/?product=cognitive-services)