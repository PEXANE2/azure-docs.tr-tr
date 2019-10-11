---
title: Azure HPC önbellek Önizleme verileri alma-msrsync
description: Azure HPC önbelleğinde bir BLOB depolama hedefine veri taşımak için msrsync kullanma
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: rohogue
ms.openlocfilehash: 6eac6c367be42021a4654f85c8f4ec980c9f6925
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255289"
---
# <a name="azure-hpc-cache-preview-data-ingest---msrsync-method"></a>Azure HPC Cache (Önizleme) Data ınest-msrsync yöntemi

Bu makalede, Azure HPC Cache ile kullanmak üzere verileri bir Azure Blob depolama kapsayıcısına kopyalamak için ``msrsync`` yardımcı programını kullanmaya yönelik ayrıntılı yönergeler sunulmaktadır.

Azure HPC önbelleğiniz için verileri blob depolamaya taşıma hakkında daha fazla bilgi edinmek için Azure [HPC Cache Için Azure Blob depolama 'ya veri taşıma](hpc-cache-ingest.md)makalesini okuyun.

@No__t-0 aracı, verileri Azure HPC önbelleği için bir arka uç depolama hedefine taşımak üzere kullanılabilir. Bu araç birden çok paralel ``rsync`` işlemi çalıştırarak bant genişliği kullanımını iyileştirmek üzere tasarlanmıştır. GitHub 'dan https://github.com/jbd/msrsync ' da kullanılabilir.

``msrsync``, kaynak dizinini ayrı "demetlere" ayırır ve sonra her bir demet üzerinde tek ``rsync`` işlem çalıştırır.

Dört çekirdekli bir VM kullanan ön test, 64 işlemleri kullanırken en iyi verimliliği gösteriyordu. İşlem sayısını 64 olarak ayarlamak için ``-p`` ``msrsync`` seçeneğini kullanın.

@No__t-0 ' ın yalnızca yerel birimlerden ve ' a yazabildiğini unutmayın. Kaynak ve hedefin, komutu vermek için kullanılan iş istasyonunda yerel takmaları olarak erişilebilir olması gerekir.

Azure Blob depolamayı Azure HPC Cache ile doldurmak için ``msrsync`` kullanmak üzere şu yönergeleri izleyin:

1. @No__t-0 ve önkoşullarını (``rsync`` ve Python 2,6 veya üzeri) yükler
1. Kopyalanacak toplam dosya ve dizin sayısını belirleme.

   Örneğin, ```prime.py --directory /path/to/some/directory``` (<https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py> ' yi indirerek kullanılabilir) bağımsız değişkenlerle ``prime.py`` yardımcı programını kullanın.

   @No__t-0 ' ı kullanmıyorsanız, GNU ``find`` aracıyla birlikte öğe sayısını aşağıdaki şekilde hesaplayabilirsiniz:

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. İşlem başına öğe sayısını öğrenmek için öğe sayısını 64 göre bölün. Komutu çalıştırdığınızda demetlerin boyutunu ayarlamak için bu sayıyı ``-f`` seçeneğiyle kullanın.

1. Dosyaları kopyalamak için ``msrsync`` komutunu verin:

   ```bash
   msrsync -P --stats -p64 -f<ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Örneğin, bu komut, 11.000 64 dosyalarını/test/source-Repository 'den/mnt/hpccache/Repository dizinine taşımak için tasarlanmıştır:

   ``mrsync -P --stats -p64 -f170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/hpccache/repository``
