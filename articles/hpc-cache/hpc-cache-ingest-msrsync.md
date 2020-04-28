---
title: Azure HPC önbellek verileri alma-msrsync
description: Azure HPC önbelleğinde bir BLOB depolama hedefine veri taşımak için msrsync kullanma
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 2e0442b6aa1404ae5f57445179979496faa09863
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82194984"
---
# <a name="azure-hpc-cache-data-ingest---msrsync-method"></a>Azure HPC Cache Data ınest-msrsync yöntemi

Bu makalede, ``msrsync`` Azure HPC Cache ile kullanmak üzere verileri bir Azure Blob depolama kapsayıcısına kopyalamak için yardımcı programı kullanma hakkında ayrıntılı yönergeler sunulmaktadır.

Azure HPC önbelleğiniz için verileri blob depolamaya taşıma hakkında daha fazla bilgi edinmek için [Azure Blob depolama 'ya veri taşıma](hpc-cache-ingest.md)makalesini okuyun.

``msrsync`` Araç, VERILERI Azure HPC önbelleği için bir arka uç depolama hedefine taşımak üzere kullanılabilir. Bu araç birden çok paralel ``rsync`` işlem çalıştırarak bant genişliği kullanımını iyileştirmek için tasarlanmıştır. Bu, tarihinde https://github.com/jbd/msrsyncGitHub 'dan alınabilir.

``msrsync``Kaynak dizini ayrı "demetlere" ayırır ve sonra her bir Bucket üzerinde ``rsync`` ayrı süreçler çalıştırır.

Dört çekirdekli bir VM kullanan ön test, 64 işlemleri kullanırken en iyi verimliliği gösteriyordu. İşlem sayısını ``msrsync`` 64 ``-p`` olarak ayarlamak için seçeneğini kullanın.

Yalnızca yerel ``msrsync`` birimlerden ve bu birimlere yazabilmesini unutmayın. Kaynak ve hedefin, komutu vermek için kullanılan iş istasyonunda yerel takmaları olarak erişilebilir olması gerekir.

Azure Blob depolamayı Azure HPC ``msrsync`` Cache ile doldurmak için kullanmak üzere aşağıdaki yönergeleri izleyin:

1. Yüklemesi ``msrsync`` ve önkoşulları (``rsync`` ve Python 2,6 veya üzeri)
1. Kopyalanacak toplam dosya ve dizin sayısını belirleme.

   Örneğin, yardımcı programını ``prime.py`` bağımsız değişkenlerle ```prime.py --directory /path/to/some/directory``` (indirerek <https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py>kullanılabilir) kullanın.

   Kullanmıyorsanız ``prime.py``, GNU ``find`` aracıyla birlikte öğe sayısını aşağıdaki şekilde hesaplayabilirsiniz:

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. İşlem başına öğe sayısını öğrenmek için öğe sayısını 64 göre bölün. Komutu çalıştırdığınızda demetlerin boyutunu ``-f`` ayarlama seçeneğiyle bu numarayı kullanın.

1. Dosyaları kopyalamak ``msrsync`` için komutu verme:

   ```bash
   msrsync -P --stats -p64 -f<ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Örneğin, bu komut, 11.000 64 dosyalarını/test/source-Repository 'den/mnt/hpccache/Repository dizinine taşımak için tasarlanmıştır:

   ``mrsync -P --stats -p64 -f170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/hpccache/repository``
