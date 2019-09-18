---
title: Azure HPC önbelleği oluşturma
description: Azure HPC önbellek örneği oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: c3d14eaefaa1f317cb061273866ffee83747f12b
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71036853"
---
# <a name="configure-aggregated-namespace"></a>Toplanan ad alanını Yapılandır
<!-- change link in GUI -->

Azure HPC önbelleği, istemcilerin, arka uç depolama sisteminin ayrıntılarını gizleyen bir sanal ad alanı aracılığıyla çeşitli depolama sistemlerine erişmesini sağlar.

Bir depolama hedefi eklediğinizde, istemciye yönelik FilePath 'i ayarlarsınız. İstemci makineler bu FilePath 'i bağlayabilir. Bu yol ile ilişkili depolama hedefini değiştirebilirsiniz. Örneğin, bir donanım depolama sistemini, istemciye yönelik yordamları yeniden yazmaya gerek kalmadan bulut depolama ile değiştirebilirsiniz.

## <a name="aggregated-namespace-example"></a>Toplanmış ad alanı örneği

İstemci makinelerin ihtiyaç duydukları bilgilere rahat bir şekilde ulaşabilmesi için toplanan ad alanınızı planlayın ve yöneticiler ve iş akışı mühendisleri yolları kolayca ayırt edebilir.

Örneğin, Azure Blob 'da depolanan verileri işlemek için bir Azure HPC önbellek örneğinin kullanıldığı bir sistemi düşünün. Analiz, şirket içi veri merkezinde depolanan şablon dosyalarını gerektirir.

Şablon verileri bir veri merkezinde depolanır ve bu iş için gereken bilgiler şu alt dizinlerde saklanır:

    /goldline/templates/acme2017/sku798
    /goldline/templates/acme2017/sku980 

Veri merkezi depolama sistemi bu dışarı aktarmaları kullanıma sunar: 

    /
    /goldline
    /goldline/templates

Çözümlenecek veriler, [Clfsload yardımcı programı](hpc-cache-ingest.md#pre-load-data-in-blob-storage-with-clfsload)kullanılarak "SourceCollection" adlı bir Azure Blob depolama kapsayıcısına kopyalanmış.

Önbellek üzerinden kolay erişime izin vermek için bu sanal ad alanı yollarıyla depolama hedefleri oluşturmayı göz önünde bulundurun:

| Arka uç NFS FilePath veya blob kapsayıcısı | Sanal ad alanı yolu |
|-----------------------------------------|------------------------|
| /Goldline/Templates/acme2017/sku798     | /Templates/sku798      |
| /Goldline/Templates/acme2017/sku980     | /Templates/sku980      |
| SourceCollection                        | /Source               |

NFS kaynak yolları aynı dışarı aktarmanın alt dizinleri olduğundan, aynı depolama hedefinden birden çok ad alanı yolu tanımlamanız gerekecektir. 

| Depolama hedef konak adı  | NFS dışarı aktarma yolu      | Alt dizin yolu | Ad alanı yolu    |
|--------------------------|----------------------|-------------------|-------------------|
| *IP adresi veya ana bilgisayar adı* | /Goldline/Templates  | acme2017/sku798   | /Templates/sku798 |
| *IP adresi veya ana bilgisayar adı* | /Goldline/Templates  | acme2017/sku980   | /Templates/sku980 |

Bir istemci uygulaması, önbelleği bağlayabilir ve toplanan ad alanı fılepaths/kaynak,/Templates/sku798 ve/Templates/sku980dizinine kolayca erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal FileSystem 'ı ayarlamaya karar verdikten sonra, arka uç depolama alanınızı istemciye yönelik sanal dosya yollarına eşlemek için [depolama hedefleri oluşturun](hpc-cache-add-storage.md) .
