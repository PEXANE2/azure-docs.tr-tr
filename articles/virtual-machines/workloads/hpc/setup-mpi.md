---
title: HPC-Azure sanal makineleri için Ileti geçirme arabirimini ayarlama | Microsoft Docs
description: Azure 'da HPC için MPı ayarlamayı öğrenin.
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
ms.openlocfilehash: 469e926932ffa11ef9f2a262b78a587ba435549e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77023999"
---
# <a name="set-up-message-passing-interface-for-hpc"></a>HPC için Ileti geçirme arabirimini ayarlama

İleti geçirme arabirimi (MPı) iş yükleri geleneksel HPC iş yüklerinin önemli bir parçasıdır. Azure 'daki SR-ıOV özellikli VM boyutları, neredeyse her türlü MPı kullanılmasına izin verir. 

VM 'lerde MPı işlerinin çalıştırılması, kiracı genelinde bölüm anahtarlarının (p-anahtarları) ayarlanmasını gerektirir. P anahtar değerlerini belirlemeye ilişkin ayrıntılar için [bölüm anahtarlarını bul](#discover-partition-keys) bölümündeki adımları izleyin.

## <a name="ucx"></a>UCX

[UCX](https://github.com/openucx/ucx) , IB üzerinde en iyi performansı sunar ve Mpich ve OpenMPI ile birlikte çalışabilir.

```bash
wget https://github.com/openucx/ucx/releases/download/v1.4.0/ucx-1.4.0.tar.gz
tar -xvf ucx-1.4.0.tar.gz
cd ucx-1.4.0
./configure --prefix=<ucx-install-path>
make -j 8 && make install
```

## <a name="openmpi"></a>OpenMPI

Daha önce açıklanan UCX 'i yükler.

```bash
sudo yum install –y openmpi
```

OpenMPI oluşturun.

```bash
wget https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.0.tar.gz
tar -xvf openmpi-4.0.0.tar.gz
cd openmpi-4.0.0
./configure --with-ucx=<ucx-install-path> --prefix=<ompi-install-path>
make -j 8 && make install
```

OpenMPI 'yi çalıştırın.

```bash
<ompi-install-path>/bin/mpirun -np 2 --map-by node --hostfile ~/hostfile -mca pml ucx --mca btl ^vader,tcp,openib -x UCX_NET_DEVICES=mlx5_0:1  -x UCX_IB_PKEY=0x0003  ./osu_latency
```

Yukarıdaki bölümde belirtilen bölüm anahtarınızı denetleyin.

## <a name="mpich"></a>MPICH

Daha önce açıklanan UCX 'i yükler.

MPICH oluşturun.

```bash
wget https://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz
tar -xvf mpich-3.3.tar.gz
cd mpich-3.3
./configure --with-ucx=<ucx-install-path> --prefix=<mpich-install-path> --with-device=ch4:ucx
make -j 8 && make install
```

MPICH çalıştırma.

```bash
<mpich-install-path>/bin/mpiexec -n 2 -hostfile ~/hostfile -env UCX_IB_PKEY=0x0003 -bind-to hwthread ./osu_latency
```

Yukarıdaki bölümde belirtilen bölüm anahtarınızı denetleyin.

## <a name="mvapich2"></a>MVAPICH2

Derleme MVAPICH2.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/mv2/mvapich2-2.3.tar.gz
tar -xv mvapich2-2.3.tar.gz
cd mvapich2-2.3
./configure --prefix=<mvapich2-install-path>
make -j 8 && make install
```

MVAPICH2 çalıştırılıyor.

```bash
<mvapich2-install-path>/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=48 ./osu_latency
```

## <a name="platform-mpi-community-edition"></a>Platform MPı Community sürümü

Platform MPı için gerekli paketleri yükler.

```bash
sudo yum install libstdc++.i686
sudo yum install glibc.i686
Download platform MPI at https://www.ibm.com/developerworks/downloads/im/mpi/index.html 
sudo ./platform_mpi-09.01.04.03r-ce.bin
```

Yükleme işlemini izleyin.

## <a name="intel-mpi"></a>Intel MPı

[Intel MPI 'Yi indirin](https://software.intel.com/mpi-library/choose-download).

Sürüme bağlı olarak I_MPI_FABRICS ortam değişkenini değiştirin. Intel MPı 2018 için `I_MPI_FABRICS=shm:ofa` kullanın ve 2019 için `I_MPI_FABRICS=shm:ofi`kullanın.

İşlem sabitleme, varsayılan olarak 15, 30 ve 60 PPN için doğru şekilde çalışmaktadır.

## <a name="osu-mpi-benchmarks"></a>OSU MPı değerlendirmeleri

[OSU MPı değerlendirmeleri](http://mvapich.cse.ohio-state.edu/benchmarks/) ve UNIK 'yi indirin.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/osu-micro-benchmarks-5.5.tar.gz
tar –xvf osu-micro-benchmarks-5.5.tar.gz
cd osu-micro-benchmarks-5.5
```

Belirli bir MPı kitaplığı kullanarak kıyaslamalar oluşturun:

```bash
CC=<mpi-install-path/bin/mpicc>CXX=<mpi-install-path/bin/mpicxx> ./configure 
make
```

MPı değerlendirmeleri `mpi/` klasörü altındadır.


## <a name="discover-partition-keys"></a>Bölüm anahtarlarını bul

Aynı kiracı içindeki diğer VM 'lerle iletişim için bölüm anahtarlarını (p-Keys) bulun (kullanılabilirlik kümesi veya VM Ölçek kümesi).

```bash
/sys/class/infiniband/mlx5_0/ports/1/pkeys/0
/sys/class/infiniband/mlx5_0/ports/1/pkeys/1
```

İkinin büyük olması MPı ile kullanılması gereken kiracı anahtarıdır. Örnek: aşağıda p anahtarları kullanılıyorsa, 0x800b MPı ile kullanılmalıdır.

```bash
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/0
0x800b
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/1
0x7fff
```

Varsayılan bölümü (0x7FFF) bölüm anahtarını kullanın. UCX, p-anahtarının işaretsiz olmasını gerektirir. Örneğin, 0x800b için 0x000b olarak UCX_IB_PKEY ayarlayın.

Ayrıca, kiracı (AVSet veya VMSS) olduğu sürece PKEYs 'in de aynı kaldığı unutulmamalıdır. Bu, düğümler eklendiğinde/silindiğinde bile geçerlidir. Yeni kiracılar farklı PKEYs 'ler alır.


## <a name="set-up-user-limits-for-mpi"></a>MPı için Kullanıcı sınırlarını ayarla

MPı için Kullanıcı sınırlarını ayarlayın.

```bash
cat << EOF | sudo tee -a /etc/security/limits.conf
*               hard    memlock         unlimited
*               soft    memlock         unlimited
*               hard    nofile          65535
*               soft    nofile          65535
EOF
```


## <a name="set-up-ssh-keys-for-mpi"></a>MPı için SSH anahtarlarını ayarlama

Gerekli MPı türleri için SSH anahtarlarını ayarlayın.

```bash
ssh-keygen -f /home/$USER/.ssh/id_rsa -t rsa -N ''
cat << EOF > /home/$USER/.ssh/config
Host *
    StrictHostKeyChecking no
EOF
cat /home/$USER/.ssh/id_rsa.pub >> /home/$USER/.ssh/authorized_keys
chmod 600 /home/$USER/.ssh/authorized_keys
chmod 644 /home/$USER/.ssh/config
```

Yukarıdaki sözdizimi paylaşılan bir giriş dizinini varsayar, başka bir deyişle, her bir düğüme tek bir SSH dizini kopyalanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
