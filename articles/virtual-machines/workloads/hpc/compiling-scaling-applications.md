---
title: HPC uygulamalarını ölçeklendirme-Azure sanal makineleri | Microsoft Docs
description: Azure VM 'lerinde HPC uygulamalarını ölçeklendirmeyi öğrenin.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: 72178b61d7033167ed48a8ddbb661daad6081df7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87020125"
---
# <a name="scaling-hpc-applications"></a>HPC uygulamalarını ölçeklendirme

Azure üzerinde HPC uygulamalarının en iyi ölçeği artırma ve genişleme performansı, belirli bir iş yükü için performans ayarlama ve iyileştirme denemeleri gerektirir. Bu bölüm ve VM dizisine özgü sayfalar, uygulamalarınızın ölçeklendirilmesine yönelik genel rehberlik sunar.

## <a name="compiling-applications"></a>Uygulamaları derleme

Gerekli olmasa da, uygun iyileştirme bayraklarıyla uygulamaları derlemek, HB ve HC Serisi VM 'lerde en iyi genişleme performansını sağlar.

### <a name="amd-optimizing-cc-compiler"></a>AMD Iyileştirmeli C/C++ derleyicisi

AMD Iyileştirmeli C/C++ derleyicisi (AOCC) derleyicisi sistemi, genel iyileştirme, vektörleştirme, işlem tabanlı çözümlemeler, döngü dönüştürmeleri ve kod oluşturma içeren yüksek düzeyde gelişmiş iyileştirmeler, çok iş parçacıklı ve işlemci desteği sunar. AOCC derleyicisi ikili dosyaları, GNU C Kitaplığı (OCBC) sürüm 2,17 ve üzeri olan Linux sistemleri için uygundur. Derleyici paketi bir C/C++ derleyicisi (Clang), bir FORTRAN derleyicisi (FLANG) ve FORTRAN ön ucu ile Clang (Ejegg) oluşur.

### <a name="clang"></a>Clang

Clang, bir C, C++ ve amaç-C derleyicisi, ön işleme, ayrıştırma, iyileştirme, kod oluşturma, derleme ve bağlamayı işleme. Clang, `-march=znver1` AMD 'Nin Zen tabanlı x86 mimarisi için en iyi kod oluşturmayı ve ayarlamayı etkinleştirme bayrağını destekler.

### <a name="flang"></a>FLANG

FLANG derleyicisi, AOCC Suite 'e son eklenen bir ektir (2018 Nisan 'a eklenmiştir) ve şu anda geliştiricilerin indirmesi ve test etmek için önceden yayın sürümündedir. FORTRAN 2008 temelinde, AMD, FLANG 'nin GitHub sürümünü genişletiyor ( https://github.com/flang-compiler/flang) . FLANG derleyicisi, tüm Clang derleyici seçeneklerini ve ek sayıda FLANG özel derleyici seçeneğini destekler.

### <a name="dragonegg"></a>DragonEgg

DragonEgg, GCC 'nin en iyi hale icilerini ve kod oluşturucularını LLVM projesinden değiştiren bir GCC eklentisidir. AOCC ile birlikte gelen DragonEgg, GCC-4.8. x ile birlikte çalışarak, x86-32/x86-64 hedefleri için test edilmiştir ve bu, çeşitli Linux platformlarında başarıyla kullanıldı.

GFortran, GCC GIMPLE ara gösterimini (IR) oluşturan ön işleme, ayrıştırma ve anlam analizinden sorumlu olan FORTRAN programlarının gerçek ön ucunda. DragonEgg, GFortran derleme akışına takmak üzere bir GNU eklentisidir. GNU eklentisi API 'sini uygular. Eklenti mimarisi sayesinde, DragonEgg derleyici sürücüsü haline gelir ve bu da derlemenin farklı aşamalarını kullanır.  İndirme ve yükleme yönergelerinden sonra, aşağıdaki kullanılarak bir Ejpg çağrılabilir: 

```bash
$ gfortran [gFortran flags] 
   -fplugin=/path/AOCC-1.2-Compiler/AOCC-1.2-     
   FortranPlugin/dragonegg.so [plugin optimization flags]     
   -c xyz.f90 $ clang -O3 -lgfortran -o xyz xyz.o $./xyz
```
   
### <a name="pgi-compiler"></a>PGI derleyicisi
PGI Community Edition sürümü. 17, AMD EPYıC ile çalışmayı onayladı. STREAM 'in bir PGI derlenmiş sürümü, platformun tam bellek bant genişliğini sunar. Yeni Community Edition 18,10 (Kasım 2018) de aynı şekilde çalışır. Aşağıda Intel derleyicisi ile en iyi şekilde derleyiciye örnek CLı verilmiştir:

```bash
pgcc $(OPTIMIZATIONS_PGI) $(STACK) -DSTREAM_ARRAY_SIZE=800000000 stream.c -o stream.pgi
```

### <a name="intel-compiler"></a>Intel derleyicisi
Intel derleyicisi ver. 18, AMD EPYıC ile çalışmayı onayladı. Aşağıda Intel derleyicisi ile en iyi şekilde derleyiciye örnek CLı verilmiştir.

```bash
icc -o stream.intel stream.c -DSTATIC -DSTREAM_ARRAY_SIZE=800000000 -mcmodel=large -shared-intel -Ofast –qopenmp
```

### <a name="gcc-compiler"></a>GCC derleyicisi 
HPC için AMD, GCC derleyicisi 7,3 veya daha yeni bir sürümü önerir. RHEL/CentOS 7,4 ile 4.8.5 dahil olmak üzere eski sürümlerin kullanılması önerilmez. GCC 7,3 ve üzeri, HPL, HPCG ve DGEMM testlerinde önemli ölçüde daha yüksek performans sağlar.

```bash
gcc $(OPTIMIZATIONS) $(OMP) $(STACK) $(STREAM_PARAMETERS) stream.c -o stream.gcc
```

## <a name="scaling-applications"></a>Ölçeklendirme uygulamaları 

En iyi uygulama ölçeklendirme verimliliği, performansı ve tutarlılığı için aşağıdaki öneriler geçerlidir:

* Sıralı sabitleme yaklaşımını (bir otomatik dengeleme yaklaşımına karşılık olarak) kullanarak işlem çekirdekleri 0-59. 
* NUMA/çekirdek/HwThread 'e göre bağlama varsayılan bağlamadan daha iyidir.
* Karma paralel uygulamalar (OpenMP + MPı) için, 4 iş parçacığı ve CCX başına 1 MPı derecesi kullanın.
* Saf MPı uygulamaları için en iyi performans için her CCX için 1-4 MPı derecelendirimiyle deneyin.
* Bellek bant genişliğine karşı çok fazla duyarlılık içeren bazı uygulamalar, CCX başına azaltılmış sayıda çekirdek kullanmaktan faydalanabilir. Bu uygulamalar için, CCX başına 3 veya 2 çekirdek kullanılması, bellek bant genişliği çekişmesini azaltabilir ve gerçek zamanlı performansı veya daha tutarlı ölçeklenebilirlik elde edebilir. Özellikle de bu durum bundan faydalanabilir.
* Önemli ölçüde daha büyük ölçek çalıştırmaları için, UD veya hibrit RC + UD taşımalarını kullanmanız önerilir. Birçok MPı Kitaplığı/çalışma zamanı kitaplığı bunu dahili olarak (UCX veya MVAPICH2 gibi) ister. Büyük ölçekli çalıştırmalar için aktarım yapılandırmalarınızı denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da [HPC](/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
