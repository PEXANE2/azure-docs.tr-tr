---
title: Verileri bir Azure HPC önbellek bulutu kapsayıcısına taşıma
description: Azure Blob depolamayı Azure HPC Cache ile kullanım için doldurma
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: 07a97b1afa8049ace97f1589393cd76c24f21368
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70775656"
---
# <a name="move-data-to-azure-blob-storage-for-azure-hpc-cache"></a>Azure HPC önbelleği için verileri Azure Blob depolamaya taşıma

İş akışınız Azure Blob depolama alanına veri taşımayı içeriyorsa, verilerinizi Azure HPC önbelleği aracılığıyla kopyalamak için etkili bir strateji kullandığınızdan emin olun.

Bu makalede, Azure HPC Cache ile kullanmak üzere verileri blob depolamaya taşımanın en iyi yolları açıklanmaktadır.

Bu gerçekleri göz önünde bulundurun:

* Azure HPC Cache, blob depolamada verileri düzenlemek için özel bir depolama biçimi kullanır. Bu nedenle, blob Storage hedefinin yeni, boş bir kapsayıcı veya daha önce Azure HPC önbellek verileri için kullanılan bir blob kapsayıcısı olması gerekir. ([Azure Için avere vFXT](https://azure.microsoft.com/services/storage/avere-vfxt/) , bu bulut dosya sistemini de kullanır.)

* Birden çok istemci ve paralel işlem kullandığınızda, verileri Azure HPC önbelleği aracılığıyla kopyalama en iyisidir. Bir istemciden basit bir kopyalama komutu verileri yavaşça taşıyacaktır.

Bir BLOB depolama kapsayıcısına içerik yüklemek için Python tabanlı yardımcı program kullanılabilir. Daha fazla bilgi edinmek için [BLOB depolamada yükleme öncesi verileri](#pre-load-data-in-blob-storage-with-clfsload) okuyun.

Yükleme yardımcı programını kullanmak istemiyorsanız veya var olan bir depolama hedefine içerik eklemek istiyorsanız, [Azure HPC Cache aracılığıyla veri kopyalama](#copy-data-through-the-azure-hpc-cache)içindeki paralel veri alma ipuçları ' nı izleyin. 

## <a name="pre-load-data-in-blob-storage-with-clfsload"></a>CLFSLoad ile blob depolamada verileri önceden yükleme

Bir depolama hedefi olarak eklemeden önce, verileri yeni bir BLOB depolama kapsayıcısına kopyalamak için [avere CLFSLoad](https://aka.ms/avere-clfsload) yardımcı programını kullanabilirsiniz. Bu yardımcı program bir Linux sanal makinesi üzerinde çalışır ve verileri Azure HPC önbelleği için gereken özel biçimde yazar. Bu, önbellek ile kullanmak üzere bir BLOB depolama kapsayıcısını doldurmanın en etkili yoludur.

Bu seçenek yalnızca yeni ve boş kapsayıcılar ile kullanılabilir. Avere CLFSLoad kullanmadan önce kapsayıcıyı oluşturun.

Ayrıntılı bilgiler [avere CLFSLoad Benioku dosyasına](https://github.com/microsoft/Avere-CLFSLoad/blob/master/README.md)dahildir. <!-- caution literal link -->

İşleme genel bir bakış:

1. Python sürüm 3,6 veya sonraki bir Linux sistemi (fiziksel veya VM) hazırlayın. (Python 3,7 daha iyi performans için önerilir.)
1. Linux sistemine avere-CLFSLoad yazılımını yükleyin.
1. Linux komut satırından aktarımı yürütün.

Avere CLFSLoad yardımcı programı aşağıdaki bilgileri gerektirir:

* BLOB depolama kapsayıcınızı içeren depolama hesabı KIMLIĞI
* Boş BLOB depolama kapsayıcısının adı
* Yardımcı programın kapsayıcıya yazmasını sağlayan bir paylaşılan erişim imzası (SAS) belirteci
* Veri kaynağına yönelik yerel bir yol-kopyalanacak verileri içeren yerel bir dizin ya da verilerle bağlı bir uzak sisteme yerel bir yol.

Gereksinimler, [avere CLFSLoad Benioku](https://aka.ms/avere-clfsload)dosyasında ayrıntılı olarak açıklanmıştır.

## <a name="copy-data-through-the-azure-hpc-cache"></a>Azure HPC Cache aracılığıyla veri kopyalama

Avere CLFSLoad yardımcı programını kullanmak istemiyorsanız veya var olan bir BLOB depolama hedefine büyük miktarda veri eklemek istiyorsanız, bunu önbellek aracılığıyla kopyalayabilirsiniz. Azure HPC Cache birden çok istemciye aynı anda hizmeti sunacak şekilde tasarlanmıştır, bu nedenle verileri önbellekten kopyalamak için birden fazla istemciden paralel yazmaları kullanmanız gerekir.

![Çoklu istemci, çok iş parçacıklı veri hareketini gösteren diyagram: Sol üst tarafta, şirket içi donanım depolamada bir simgenin bundan sonra gelen birden çok oku vardır. Oklar dört istemci makineye işaret noktasıdır. Her bir istemci makineden üç ok, Azure HPC önbelleğine doğru işaret noktasıdır. Azure HPC önbelleğinde, birden çok ok blob depolamaya işaret noktasıdır.](media/hpc-cache-parallel-ingest.png) 

Genellikle bir depolama sisteminden diğerine veri aktarmak için kullandığınız ``cp`` Komutlar,tekseferdeyalnızcabirdosyaoluşturantekişparçacıklıişlemlerdir.``copy`` Bu, dosya sunucusunun tek seferde yalnızca bir dosya olduğu anlamına gelir. Bu, kümenin kaynakları için bir atık olur.

Bu bölümde, verileri Azure HPC önbelleğiyle blob depolamaya taşımak için çok istemci, çok iş parçacıklı bir dosya kopyalama sistemi oluşturma stratejileri açıklanmaktadır. Birden çok istemci ve basit kopyalama komutları kullanılarak etkili veri kopyalama için kullanılabilen dosya aktarımı kavramlarını ve karar noktalarını açıklar.

Ayrıca yardımcı olabilecek bazı yardımcı programları da açıklar. ``msrsync`` Yardımcı programı, bir veri kümesini demetlere bölme ve rsync komutlarının kullanımı sürecini kısmen otomatikleştirebilmek için kullanılabilir. ``parallelcp`` Betik, kaynak dizini okuyan ve komutları otomatik olarak kopyalama ile ilgili başka bir yardımcı programdır.

### <a name="strategic-planning"></a>Stratejik planlama

Verileri paralel olarak kopyalamak için bir strateji oluştururken dosya boyutu, dosya sayısı ve Dizin derinliği içindeki avantajları anlamanız gerekir.

* Dosyalar küçük olduğunda, ilgilendiğiniz ölçüm, saniye başına dosya olur.
* Dosyalar büyükse (10 MIBI veya üzeri), ilgilendiğiniz ölçüm bayt/saniye olur.

Her kopyalama işleminin bir işleme hızı ve dosya-aktarım hızı vardır ve bu, kopyalama komutunun uzunluğu ve dosya boyutu ile dosya sayısı düzenleme ile ölçülebilir. Hızların nasıl ölçülmesi, bu belgenin kapsamı dışındadır, ancak küçük veya büyük dosyalarla ilgilenip işlenmeyeceğinizi anlamak için önemlidir.

Azure HPC Cache ile paralel veri alma stratejileri şunları içerir:

* El ile kopyalama-önceden tanımlanmış dosya veya yol kümelerine yönelik olarak, arka planda birden fazla kopyalama komutu çalıştırarak bir istemcide çok iş parçacıklı bir kopyayı el ile oluşturabilirsiniz. Ayrıntılar için [Azure HPC bulut verilerini alma-el ile kopyalama yöntemini](hpc-cache-ingest-manual.md) okuyun.

* İle ``msrsync``  -  ``rsync`` kısmen otomatikleştirilmiş kopyalama, birden çok paralel işlem çalıştıran bir sarmalayıcı yardımcı programıdır. ``msrsync`` Ayrıntılar için [Azure HPC Cache Data ınest-msrsync yöntemini](hpc-cache-ingest-msrsync.md)okuyun.

* İle ``parallelcp`` betikleştirilmiş kopyalama- [Azure HPC Cache Data ınest-Parallel Copy betik yönteminde](hpc-cache-ingest-parallelcp.md)paralel kopyalama betiği oluşturmayı ve çalıştırmayı öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Depolama alanınızı ayarladıktan sonra, istemcilerin önbelleği nasıl bağlayabileceğinizi öğrenin.

* [Azure HPC önbellek sistemine erişin](hpc-cache-mount.md)
