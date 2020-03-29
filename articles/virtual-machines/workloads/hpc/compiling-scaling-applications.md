---
title: HPC uygulamalarını ölçekleme - Azure Sanal Makineler | Microsoft Dokümanlar
description: Azure VM'lerde HPC uygulamalarını nasıl ölçeklendireceklerini öğrenin.
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
ms.openlocfilehash: 00d5b86c8cae01d342d55b7ad20ec59c3f7530bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67707833"
---
# <a name="scaling-hpc-applications"></a>HPC uygulamalarını ölçekleme

Azure'daki HPC uygulamalarının en iyi ölçeklendirme ve ölçeklendirme performansı, belirli iş yükü için performans ayarı ve optimizasyon denemeleri gerektirir. Bu bölüm ve VM serisine özel sayfalar, uygulamalarınızı ölçekleme için genel kılavuz sunar.

## <a name="compiling-applications"></a>Uygulamaları derleme

Gerekli olmasa da, uygun optimizasyon bayrakları ile uygulamaları derleme HB ve HC serisi VM'lerde en iyi ölçeklendirme performansı sağlar.

### <a name="amd-optimizing-cc-compiler"></a>AMD Optimize C/C++ Derleyici

AMD Optimize Edici C/C++ Derleyici (AOCC) derleyici sistemi, küresel optimizasyon, vektörelleştirme, yordamlar arası analizler, döngü dönüşümleri ve kod oluşturmayı içeren yüksek düzeyde gelişmiş optimizasyonlar, çoklu iş parçacığı ve işlemci desteği sunar. AOCC derleyici ikilileri, GNU C Library (glibc) sürüm 2.17 ve üzeri olan Linux sistemleri için uygundur. Derleyici paketi c/c++ derleyicisi (clang), fortran derleyicisi (FlANŞ) ve Clang (Dragon Egg) için fortran ön ucundan oluşur.

### <a name="clang"></a>Clang

Clang, ön işleme, ayrıştırma, optimizasyon, kod oluşturma, derleme ve bağlama işleme yi işleyen bir C, C++ve Objective-C derleyicisidir. Clang, `-march=znver1` AMD'nin Zen tabanlı x86 mimarisi için en iyi kod oluşturma ve atonlama sağlamak için bayrağı destekler.

### <a name="flang"></a>FLANŞ

FLANG derleyicisi, AOCC paketine yeni eklenen (Nisan 2018'de eklendi) ve geliştiricilerin indirip test etmesi için ön sürümdedir. Fortran 2008 dayanarak, AMD FlLANG GitHub sürümünühttps://github.com/flangcompiler/flang)genişletir ( . FlANS derleyicisi tüm Clang derleyici seçeneklerini ve flanş'a özel derleyici seçeneklerinin ek sayısını destekler.

### <a name="dragonegg"></a>DragonEgg

DragonEgg BU LLVM projeile GCC optimize ve kod jeneratörleri yerine bir gcc eklentisi. AOCC ile birlikte gelen DragonEgg gcc-4.8.x ile çalışır, x86-32/x86-64 hedefleri için test edilmiş ve başarıyla çeşitli Linux platformlarında kullanılmıştır.

GFortran, GCC GIMPLE ara gösterimini (IR) oluşturan ön işleme, ayrıştırma ve anlamsal analizden sorumlu Fortran programlarının gerçek ön sonudur. DragonEgg gnu eklentisi, GFortran derleme akışı na takılı. GNU eklenti API'sini uygular. Plugin mimarisi ile, DragonEgg derleme farklı aşamalarında sürüş, derleyici sürücü olur.  İndirme ve yükleme yönergelerini takip ettikten sonra Dragon Egg aşağıdakileri kullanarak çağrılabilir: 

```bash
$ gfortran [gFortran flags] 
   -fplugin=/path/AOCC-1.2-Compiler/AOCC-1.2-     
   FortranPlugin/dragonegg.so [plugin optimization flags]     
   -c xyz.f90 $ clang -O3 -lgfortran -o xyz xyz.o $./xyz
```
   
### <a name="pgi-compiler"></a>PGI Derleyici
PGI Topluluk Sürümü ver. 17 AMD EPYC ile çalışmak için teyit edilebedilir. STREAM'in PGI tarafından derlenmiş bir sürümü platformun tam bellek bant genişliği sunar. Yeni Community Edition 18.10 (Kasım 2018) aynı şekilde iyi çalışması gerekir. Aşağıda Intel Derleyicisi ile en iyi şekilde derleyici için örnek CLI:

```bash
pgcc $(OPTIMIZATIONS_PGI) $(STACK) -DSTREAM_ARRAY_SIZE=800000000 stream.c -o stream.pgi
```

### <a name="intel-compiler"></a>Intel Derleyici
Intel Derleyici ver. 18 AMD EPYC ile çalışmak için teyit edilir. Aşağıda Intel Derleyicisi ile en iyi şekilde derleyici için örnek CLI olduğunu.

```bash
icc -o stream.intel stream.c -DSTATIC -DSTREAM_ARRAY_SIZE=800000000 -mcmodel=large -shared-intel -Ofast –qopenmp
```

### <a name="gcc-compiler"></a>GCC Derleyici 
HPC için AMD, GCC derleyicisi 7.3 veya daha yeni yi önerir. RHEL/CentOS 7.4 ile birlikte verilen 4.8.5 gibi eski sürümler önerilmez. GCC 7.3 ve daha yeni, HPL, HPCG ve DGEMM testlerinde önemli ölçüde daha yüksek performans sağlayacaktır.

```bash
gcc $(OPTIMIZATIONS) $(OMP) $(STACK) $(STREAM_PARAMETERS) stream.c -o stream.gcc
```

## <a name="scaling-applications"></a>Ölçeklendirme uygulamaları 

En iyi uygulama ölçekleme verimliliği, performansı ve tutarlılığı için aşağıdaki öneriler geçerlidir:

* Sıralı sabitleme yaklaşımını kullanarak 0-59 çekirdeklere pin işlemleri (otomatik denge yaklaşımının aksine). 
* Numa/Core/HwThread tarafından bağlama varsayılan bağlama daha iyidir.
* Karma paralel uygulamalar (OpenMP+MPI) için CCX başına 4 iş parçacığı ve 1 MPI sıralaması kullanın.
* Saf MPI uygulamaları için, optimum performans için CCX başına 1-4 MPI sıralaması ile denemeler.
* Bellek bant genişliğine karşı aşırı duyarlılığa sahip bazı uygulamalar, CCX başına daha az sayıda çekirdek kullanmaktan yararlanabilir. Bu uygulamalar için, CCX başına 3 veya 2 çekirdek kullanmak bellek bant genişliği çekişmelerini azaltabilir ve daha yüksek gerçek performans veya daha tutarlı ölçeklenebilirlik sağlayabilir. Özellikle, MPI Allreduce bundan yararlanabilir.
* Önemli ölçüde daha büyük ölçekli çalışır için UD veya hibrit RC+UD aktarımları kullanılması önerilir. Birçok MPI kitaplıkları/çalışma zamanı kitaplıkları bunu dahili olarak yapar (UCX veya MVAPICH2 gibi). Büyük ölçekli çalıştırmalar için aktarım yapılandırmalarınızı kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Azure'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
