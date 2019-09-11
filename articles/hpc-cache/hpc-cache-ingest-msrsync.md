---
title: Azure HPC önbellek verileri alma-msrsync
description: Azure HPC önbelleğinde bir BLOB depolama hedefine veri taşımak için msrsync kullanma
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: v-erkell
ms.openlocfilehash: d49c2ba45b125f8e42ea5d10dcf3dcd68558a52c
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70775643"
---
# <a name="azure-hpc-cache-data-ingest---msrsync-method"></a>Azure HPC Cache Data ınest-msrsync yöntemi

Bu makalede, ``msrsync`` Azure HPC Cache ile kullanmak üzere verileri bir Azure Blob depolama kapsayıcısına kopyalamak için yardımcı programı kullanma hakkında ayrıntılı yönergeler sunulmaktadır.

Azure HPC önbelleğiniz için verileri blob depolamaya taşıma hakkında daha fazla bilgi edinmek için Azure [HPC Cache Için Azure Blob depolama 'ya veri taşıma](hpc-cache-ingest.md)makalesini okuyun.

``msrsync`` Araç, verileri Azure HPC önbelleği için bir arka uç depolama hedefine taşımak üzere kullanılabilir. Bu araç birden çok paralel ``rsync`` işlem çalıştırarak bant genişliği kullanımını iyileştirmek için tasarlanmıştır. Bu, tarihinde https://github.com/jbd/msrsync GitHub 'dan alınabilir.

``msrsync``Kaynak dizini ayrı "demetlere" ayırır ve sonra her bir Bucket üzerinde ``rsync`` ayrı süreçler çalıştırır.

Dört çekirdekli bir VM kullanan ön test, 64 işlemleri kullanırken en iyi verimliliği gösteriyordu. İşlem sayısını 64 ``-p`` olarak ayarlamak için seçeneğinikullanın.``msrsync``

Yalnızca yerel ``msrsync`` birimlerden ve bu birimlere yazabilmesini unutmayın. Kaynak ve hedefin, komutu vermek için kullanılan iş istasyonunda yerel takmaları olarak erişilebilir olması gerekir.

Azure Blob depolamayı Azure HPC ``msrsync`` Cache ile doldurmak için kullanmak üzere aşağıdaki yönergeleri izleyin:

1. Yüklemesi ``msrsync`` ve önkoşulları (``rsync`` ve Python 2,6 veya üzeri)
1. Kopyalanacak toplam dosya ve dizin sayısını belirleme.

   Örneğin, yardımcı programını ``prime.py`` bağımsız değişkenlerle ```prime.py --directory /path/to/some/directory``` (indirerek <https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py>kullanılabilir) kullanın.

   Kullanmıyorsanız, GNU ``find`` aracıyla birlikte öğe sayısını aşağıdaki şekilde hesaplayabilirsiniz: ``prime.py``

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. İşlem başına öğe sayısını öğrenmek için öğe sayısını 64 göre bölün. Komutu çalıştırdığınızda demetlerin boyutunu ``-f`` ayarlama seçeneğiyle bu numarayı kullanın.

1. Dosyaları kopyalamak için komutu verme: ``msrsync``

   ```bash
   msrsync -P --stats -p64 -f<ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Örneğin, bu komut, 11.000 64 dosyalarını/test/source-Repository 'den/mnt/hpccache/Repository dizinine taşımak için tasarlanmıştır:

   ``mrsync -P --stats -p64 -f170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/hpccache/repository``
