---
title: Azure HPC Önbellek veri yutma - msrsync
description: Azure HPC Önbelleğinde verileri Blob depolama hedefine taşımak için msrsync nasıl kullanılır?
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 4f8863d706d623d613ac156cf202c3b7b12f2ae0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74168432"
---
# <a name="azure-hpc-cache-data-ingest---msrsync-method"></a>Azure HPC Önbellek veri yutma - msrsync yöntemi

Bu makalede, Azure HPC Önbelleği ile kullanılmak üzere verileri bir Azure Blob depolama kapsayıcısına kopyalamak için ``msrsync`` yardımcı programı kullanmak için ayrıntılı yönergeler verem.

Azure HPC Önbelleğiniz için verileri Blob depolama alanına taşıma hakkında daha fazla bilgi edinmek için [verileri Azure Blob depolama alanına taşı'nı](hpc-cache-ingest.md)okuyun.

Araç, ``msrsync`` Verileri Azure HPC Önbelleği için arka uç depolama hedefine taşımak için kullanılabilir. Bu araç, birden çok paralel ``rsync`` işlem çalıştırarak bant genişliği kullanımını en iyi duruma getirmek için tasarlanmıştır. GitHub'dan https://github.com/jbd/msrsyncedinilebilir.

``msrsync``kaynak dizini ayrı "kovalara" ayırır ve ``rsync`` her kovada tek tek işlemleri çalıştırır.

Dört çekirdekli VM kullanılarak yapılan ön testler, 64 proses kullanırken en iyi verimi gösterdi. İşlem ``msrsync`` sayısını ``-p`` 64 olarak ayarlamak için seçeneği kullanın.

Yalnızca ``msrsync`` yerel birimlere ve yerel birimlerden yazabilen not. Komutu vermek için kullanılan iş istasyonundaki yerel bağlar olarak kaynak ve hedefe erişilebilir olmalıdır.

Azure Blob ``msrsync`` depolama alanını Azure HPC Önbelleği yle doldurmak için kullanılacak aşağıdaki yönergeleri izleyin:

1. Yükleyin ``msrsync`` ve önkoşulları``rsync`` (ve Python 2.6 veya sonrası)
1. Kopyalanacak toplam dosya ve dizin sayısını belirleyin.

   Örneğin, bağımsız değişkenlerle ``prime.py`` ```prime.py --directory /path/to/some/directory``` yardımcı programı kullanın <https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py>(indirerek kullanılabilir).

   Kullanmıyorsanız, ``prime.py``GNU ``find`` aracıyla madde sayısını aşağıdaki gibi hesaplayabilirsiniz:

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. İşlem başına madde sayısını belirlemek için madde sayısını 64'e bölün. Komutu ``-f`` çalıştırdığınızda kovaların boyutunu ayarlamak için seçeneği ile bu numarayı kullanın.

1. Dosyaları ``msrsync`` kopyalamak için komutu verme:

   ```bash
   msrsync -P --stats -p64 -f<ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Örneğin, bu komut 64 işlemdeki 11.000 dosyayı /test/kaynak deposundan /mnt/hpccache/depoya taşımak üzere tasarlanmıştır:

   ``mrsync -P --stats -p64 -f170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/hpccache/repository``
