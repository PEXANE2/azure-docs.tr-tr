---
title: Azure HPC önbelleği oluşturma (Önizleme)
description: Azure HPC önbellek örneği oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: 677d42dfa1c468417f18ba4222cb0d5fd3ebb189
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71180980"
---
# <a name="plan-the-aggregated-namespace"></a>Toplanan ad alanını planlayın

Azure HPC Cache (Önizleme), istemcilerin, arka uç depolama sisteminin ayrıntılarını gizleyen bir sanal ad alanı aracılığıyla çeşitli depolama sistemlerine erişmesini sağlar.

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
