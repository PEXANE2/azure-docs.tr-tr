---
title: Verileri Azure HPC Önbellek bulut kapsayıcısına taşıma
description: Azure HPC Önbelleği ile kullanılmak üzere Azure Blob depolama alanı doldurma
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: fd21a78d0271f91d334bba5aba748f3770ad38cf
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537942"
---
# <a name="move-data-to-azure-blob-storage"></a>Verileri Azure Blob depolama alanına taşıma

İş akışınız verileri Azure Blob depolama alanına taşımayı içeriyorsa, verimli bir strateji kullandığınızdan emin olun. Verileri depolama hedefi olarak tanımlamadan önce yeni bir Blob kapsayıcısında önceden yükleyebilir veya kapsayıcıyı ekleyip Azure HPC Önbelleği'ni kullanarak verilerinizi kopyalayabilirsiniz.

Bu makalede, Azure HPC Önbelleği ile kullanılmak üzere verileri Blob depolama alanına taşımanın en iyi yolları açıklanmaktadır.

Bu gerçekleri aklınızda bulundurun:

* Azure HPC Önbelleği, Blob depolama alanında verileri düzenlemek için özel bir depolama biçimi kullanır. Bu nedenle, Blob depolama hedefi yeni, boş bir kapsayıcı veya daha önce Azure HPC Önbellek verileri için kullanılan bir Blob kapsayıcısı olmalıdır.

* Birden çok istemci ve paralel işlem kullandığınızda, verileri Azure HPC Önbelleği üzerinden arka uç depolama hedefine kopyalamak daha verimlidir. Bir istemciden basit bir kopyalama komutu verileri yavaşça hareket ettirir.

Bir Blob depolama kapsayıcısına içerik yüklemek için Python tabanlı bir yardımcı program kullanılabilir. Daha fazla bilgi edinmek için [Blob depolamaalanında ön yükleme verilerini](#pre-load-data-in-blob-storage-with-clfsload) okuyun.

Yükleme yardımcı programını kullanmak istemiyorsanız veya varolan bir depolama hedefine içerik eklemek istiyorsanız, [Azure HPC Önbelleği aracılığıyla Verileri](#copy-data-through-the-azure-hpc-cache)Kopyala'daki paralel veri alma ipuçlarını izleyin.

## <a name="pre-load-data-in-blob-storage-with-clfsload"></a>CLFSLoad ile Blob depolamada ön yükleme verileri

Avere CLFSLoad yardımcı programını, depolama hedefi olarak eklemeden önce verileri yeni bir Blob depolama kapsayıcısına kopyalamak için kullanabilirsiniz. Bu yardımcı program tek bir Linux sisteminde çalışır ve verileri Azure HPC Önbelleği için gereken özel biçimde yazar. CLFSLoad önbellek ile kullanılmak üzere bir Blob depolama kapsayıcı doldurmak için en verimli yoludur.

Avere CLFSLoad yardımcı programı, Azure HPC Önbellek ekibinizin isteği üzerine kullanılabilir. Bunun için ekibinize danışın veya yardım istemek için bir [destek bileti](hpc-cache-support-ticket.md) açın.

Bu seçenek yalnızca yeni, boş kapsayıcılarla çalışır. Avere CLFSLoad kullanmadan önce kapsayıcıyı oluşturun.

Azure HPC Önbellek ekibinin isteği üzerine kullanılabilen Avere CLFSLoad dağıtımına ayrıntılı bilgiler dahildir.

Sürecin genel bir özeti:

1. Python sürüm 3.6 veya sonraki sürümlü bir Linux sistemi (VM veya fiziksel) hazırlayın. Python 3.7 daha iyi performans için önerilir.
1. Avere-CLFSLoad yazılımını Linux sistemine yükleyin.
1. Linux komut satırından transferi gerçekleştirin.

Avere CLFSLoad yardımcı programı aşağıdaki bilgilere ihtiyaç duyar:

* Blob depolama kapsayıcınızı içeren depolama hesabı kimliği
* Boş Blob depolama kabının adı
* Yardımcı programın kapsayıcıya yazmasına olanak tanıyan paylaşılan erişim imzası (SAS) belirteci
* Veri kaynağına giden yerel bir yol - kopyalanması gereken verileri içeren yerel bir dizin veya verilerle birlikte monte edilmiş bir uzak sisteme giden yerel bir yol

## <a name="copy-data-through-the-azure-hpc-cache"></a>Azure HPC Önbelleği aracılığıyla verileri kopyalama

Avere CLFSLoad yardımcı programını kullanmak istemiyorsanız veya varolan bir Blob depolama hedefine büyük miktarda veri eklemek istiyorsanız, önbellek ten kopyalayabilirsiniz. Azure HPC Önbelleği aynı anda birden çok istemciye hizmet vermek üzere tasarlanmıştır, bu nedenle önbellek üzerinden veri kopyalamak için birden çok istemciden gelen paralel yazmaları kullanmanız gerekir.

![Çok istemcili, çok iş parçacığı veri hareketini gösteren diyagram: Sol üstte, şirket içi donanım depolama için bir simgenin birden çok ok gelir. Oklar dört istemci makinesini işaret ediyor. Her istemci makineden üç ok Azure HPC Önbelleğini işaret eder. Azure HPC Önbelleğinden birden çok ok Blob depolama alanını işaret eder.](media/hpc-cache-parallel-ingest.png)

Verileri ``cp`` ``copy`` bir depolama sisteminden diğerine aktarmak için genellikle kullandığınız veya komutları, aynı anda yalnızca bir dosyayı kopyalayan tek iş parçacığı işlemleridir. Bu, dosya sunucusunun aynı anda yalnızca bir dosya yutturdonuyor demektir - bu da önbelleğin kaynaklarının boşa harcadığı anlamına gelir.

Bu bölümde, verileri Azure HPC Önbelleği ile Blob depolamaalanına taşımak için çok istemcili, çok iş parçacığı dosya kopyalama sistemi oluşturma stratejileri açıklanmaktadır. Birden çok istemci ve basit kopyalama komutları kullanarak verimli veri kopyalama için kullanılabilecek dosya aktarım kavramlarını ve karar noktalarını açıklar.

Ayrıca yardımcı olabilecek bazı yardımcı programları açıklar. Yardımcı ``msrsync`` program, bir veri kümesini kovalara bölme ve rsync komutlarını kullanma işlemini kısmen otomatikleştirmek için kullanılabilir. Komut ``parallelcp`` dosyası, kaynak dizinini okuyan ve kopyalama komutlarını otomatik olarak veren başka bir yardımcı programdır.

### <a name="strategic-planning"></a>Stratejik planlama

Verileri paralel olarak kopyalamak için bir strateji oluşturuyorken, dosya boyutu, dosya sayısı ve dizin derinliğindeki dengeleri anlamanız gerekir.

* Dosyalar küçük olduğunda, ilgi ölçüsü saniyedeki dosyalardır.
* Dosyalar büyükolduğunda (10MiBi veya daha büyük), ilgi ölçüsü saniyede bayttır.

Her kopyalama işleminde, kopyalama komutunun uzunluğunun zamanlaması ve dosya boyutunun ve dosya sayısının çarpanabinat edilmesiyle ölçülebilen bir işlem hızı ve dosya aktarılabilen bir hız vardır. Oranların nasıl ölçüleceğini açıklamak bu belgenin kapsamı dışındadır, ancak küçük veya büyük dosyalarla mı uğraşacağınız anlaşılması zorunludur.

Azure HPC Önbelleği ile paralel veri alma stratejileri şunlardır:

* El ile kopyalama - Önceden tanımlanmış dosya veya yol kümelerine karşı arka planda aynı anda birden fazla kopya komutu çalıştırarak istemciüzerinde çok iş parçacığı kopyasını el ile oluşturabilirsiniz. Ayrıntılar için [Azure HPC Önbellek veri yutma - el ile kopyalama yöntemini](hpc-cache-ingest-manual.md) okuyun.

* Kısmen otomatik kopyalama ``msrsync``  -  ``msrsync`` ile birden çok paralel ``rsync`` işlem çalıştıran bir sarma yardımcı programıdır. Ayrıntılar için [Azure HPC Önbellek veri yut - msrsync yöntemini](hpc-cache-ingest-msrsync.md)okuyun.

* Komut dosyası ile ``parallelcp`` kopyalama - [Azure HPC Önbellek veri alma](hpc-cache-ingest-parallelcp.md)da paralel bir kopyalama komut dosyası oluşturma ve çalıştırma yı öğrenin - paralel kopyalama komut dosyası yöntemi .

## <a name="next-steps"></a>Sonraki adımlar

Depolama alanınızı ayarladıktan sonra, istemcilerin önbelleği nasıl monte edebileceğini öğrenin.

* [Azure HPC Önbellek sistemine erişin](hpc-cache-mount.md)
