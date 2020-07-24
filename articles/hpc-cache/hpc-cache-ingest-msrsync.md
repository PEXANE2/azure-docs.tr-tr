---
title: Azure HPC önbellek verileri alma-msrsync
description: Azure HPC önbelleğinde bir BLOB depolama hedefine veri taşımak için msrsync kullanma
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 10/30/2019
ms.author: v-erkel
ms.openlocfilehash: 323ecd6a2dd001c3c8df1b2ec15f0ae8402ec70c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87092414"
---
# <a name="azure-hpc-cache-data-ingest---msrsync-method"></a>Azure HPC Cache Data ınest-msrsync yöntemi

Bu makalede, ``msrsync`` Azure HPC Cache ile kullanmak üzere verileri bir Azure Blob depolama kapsayıcısına kopyalamak için yardımcı programı kullanma hakkında ayrıntılı yönergeler sunulmaktadır.

Azure HPC önbelleğiniz için verileri blob depolamaya taşıma hakkında daha fazla bilgi edinmek için [Azure Blob depolama 'ya veri taşıma](hpc-cache-ingest.md)makalesini okuyun.

``msrsync``Araç, verileri Azure HPC önbelleği için bir arka uç depolama hedefine taşımak üzere kullanılabilir. Bu araç birden çok paralel işlem çalıştırarak bant genişliği kullanımını iyileştirmek için tasarlanmıştır ``rsync`` . Bu, tarihinde GitHub 'dan alınabilir https://github.com/jbd/msrsync .

``msrsync``Kaynak dizini ayrı "demetlere" ayırır ve sonra ``rsync`` her bir Bucket üzerinde ayrı süreçler çalıştırır.

Dört çekirdekli bir VM kullanan ön test, 64 işlemleri kullanırken en iyi verimliliği gösteriyordu. ``msrsync`` ``-p`` İşlem sayısını 64 olarak ayarlamak için seçeneğini kullanın.

``msrsync``Yalnızca yerel birimlerden ve bu birimlere yazabilmesini unutmayın. Kaynak ve hedefin, komutu vermek için kullanılan iş istasyonunda yerel takmaları olarak erişilebilir olması gerekir.

Azure ``msrsync`` BLOB depolamayı Azure HPC Cache ile doldurmak için kullanmak üzere aşağıdaki yönergeleri izleyin:

1. Yüklemesi ``msrsync`` ve önkoşulları ( ``rsync`` ve Python 2,6 veya üzeri)
1. Kopyalanacak toplam dosya ve dizin sayısını belirleme.

   Örneğin, yardımcı programını ``prime.py`` bağımsız değişkenlerle ```prime.py --directory /path/to/some/directory``` (indirerek kullanılabilir <https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py> ) kullanın.

   Kullanmıyorsanız ``prime.py`` , GNU aracıyla birlikte öğe sayısını ``find`` aşağıdaki şekilde hesaplayabilirsiniz:

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. İşlem başına öğe sayısını öğrenmek için öğe sayısını 64 göre bölün. ``-f``Komutu çalıştırdığınızda demetlerin boyutunu ayarlama seçeneğiyle bu numarayı kullanın.

1. ``msrsync``Dosyaları kopyalamak için komutu verme:

   ```bash
   msrsync -P --stats -p64 -f<ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Örneğin, bu komut, 11.000 64 dosyalarını/test/source-Repository 'den/mnt/hpccache/Repository dizinine taşımak için tasarlanmıştır:

   ``mrsync -P --stats -p64 -f170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/hpccache/repository``
