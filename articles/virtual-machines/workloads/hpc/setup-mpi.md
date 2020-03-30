---
title: HPC için İleti Geçiş Arabirimi Ayarlama - Azure Sanal Makineler | Microsoft Dokümanlar
description: Azure'da HPC için MPI'yi nasıl ayarlayabilirsiniz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023999"
---
# <a name="set-up-message-passing-interface-for-hpc"></a>HPC için İleti Geçiş Arabirimi Ayarlama

İleti Geçme Arabirimi (MPI) iş yükleri, geleneksel HPC iş yüklerinin önemli bir parçasıdır. Azure'daki SR-IOV özellikli VM boyutları, neredeyse her türlü MPI tadında kullanılmasına olanak tanır. 

VM'lerde MPI işleri çalıştırmak, bir kiracı arasında bölüm anahtarlarının (p tuşları) ayarlanmasını gerektirir. P tuşu değerlerini belirlemeye ilişkin ayrıntılar için [Bölüm tuşlarını Keşfet](#discover-partition-keys) bölümündeki adımları izleyin.

## <a name="ucx"></a>UCX

[UCX](https://github.com/openucx/ucx) IB en iyi performansı sunar ve MPICH ve OpenMPI ile çalışır.

```bash
wget https://github.com/openucx/ucx/releases/download/v1.4.0/ucx-1.4.0.tar.gz
tar -xvf ucx-1.4.0.tar.gz
cd ucx-1.4.0
./configure --prefix=<ucx-install-path>
make -j 8 && make install
```

## <a name="openmpi"></a>OpenMPI

UcX'i daha önce açıklandığı gibi yükleyin.

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

OpenMPI çalıştırın.

```bash
<ompi-install-path>/bin/mpirun -np 2 --map-by node --hostfile ~/hostfile -mca pml ucx --mca btl ^vader,tcp,openib -x UCX_NET_DEVICES=mlx5_0:1  -x UCX_IB_PKEY=0x0003  ./osu_latency
```

Yukarıda belirtildiği gibi bölüm anahtarınızı kontrol edin.

## <a name="mpich"></a>MPICH

UcX'i daha önce açıklandığı gibi yükleyin.

MPICH oluşturun.

```bash
wget https://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz
tar -xvf mpich-3.3.tar.gz
cd mpich-3.3
./configure --with-ucx=<ucx-install-path> --prefix=<mpich-install-path> --with-device=ch4:ucx
make -j 8 && make install
```

MPICH çalıştırıyor.

```bash
<mpich-install-path>/bin/mpiexec -n 2 -hostfile ~/hostfile -env UCX_IB_PKEY=0x0003 -bind-to hwthread ./osu_latency
```

Yukarıda belirtildiği gibi bölüm anahtarınızı kontrol edin.

## <a name="mvapich2"></a>MVAPICH2

MVAPICH2 oluşturun.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/mv2/mvapich2-2.3.tar.gz
tar -xv mvapich2-2.3.tar.gz
cd mvapich2-2.3
./configure --prefix=<mvapich2-install-path>
make -j 8 && make install
```

Çalışan MVAPICH2.

```bash
<mvapich2-install-path>/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=48 ./osu_latency
```

## <a name="platform-mpi-community-edition"></a>Platform MPI Topluluk Sürümü

Platform MPI için gerekli paketleri yükleyin.

```bash
sudo yum install libstdc++.i686
sudo yum install glibc.i686
Download platform MPI at https://www.ibm.com/developerworks/downloads/im/mpi/index.html 
sudo ./platform_mpi-09.01.04.03r-ce.bin
```

Yükleme işlemini izleyin.

## <a name="intel-mpi"></a>Intel MPI

[Intel MPI'yi karşıdan yükleyin.](https://software.intel.com/mpi-library/choose-download)

sürüme bağlı olarak I_MPI_FABRICS ortam değişkenini değiştirin. Intel MPI 2018 `I_MPI_FABRICS=shm:ofa` için, kullanın `I_MPI_FABRICS=shm:ofi`ve 2019 için.

İşlem sabitleme varsayılan olarak 15, 30 ve 60 PPN için doğru çalışır.

## <a name="osu-mpi-benchmarks"></a>OSU MPI Kriterleri

[OSU MPI Kriterleri](http://mvapich.cse.ohio-state.edu/benchmarks/) ve untar indirin.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/osu-micro-benchmarks-5.5.tar.gz
tar –xvf osu-micro-benchmarks-5.5.tar.gz
cd osu-micro-benchmarks-5.5
```

Belirli bir MPI kitaplığını kullanarak Kıyaslama Oluşturma:

```bash
CC=<mpi-install-path/bin/mpicc>CXX=<mpi-install-path/bin/mpicxx> ./configure 
make
```

MPI Kıyaslamaları `mpi/` klasörün altındadır.


## <a name="discover-partition-keys"></a>Bölüm tuşlarını keşfedin

Aynı kiracı (Kullanılabilirlik Kümesi veya VM Ölçek Kümesi) içindeki diğer VM'lerle iletişim kurmak için bölüm tuşlarını (p tuşu) keşfedin.

```bash
/sys/class/infiniband/mlx5_0/ports/1/pkeys/0
/sys/class/infiniband/mlx5_0/ports/1/pkeys/1
```

İkisinden daha büyüğü MPI ile kullanılması gereken kiracı anahtarıdır. Örnek: Aşağıdaki p tuşları ise, 0x800b MPI ile kullanılmalıdır.

```bash
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/0
0x800b
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/1
0x7fff
```

Varsayılan (0x7fff) bölüm anahtarı dışındaki bölümü kullanın. UCX, p-tuşunun MSB'sinin temizlenmesini gerektirir. Örneğin, 0x800b için 0x000b olarak UCX_IB_PKEY ayarlayın.

Ayrıca, kiracı (AVSet veya VMSS) olduğu sürece, PKEY'lerin aynı kaldığını unutmayın. Düğümler eklense/silinse bile bu durum geçerlidir. Yeni kiracılar farklı PKEYs olsun.


## <a name="set-up-user-limits-for-mpi"></a>MPI için kullanıcı sınırları ayarlama

MPI için kullanıcı sınırları ayarlayın.

```bash
cat << EOF | sudo tee -a /etc/security/limits.conf
*               hard    memlock         unlimited
*               soft    memlock         unlimited
*               hard    nofile          65535
*               soft    nofile          65535
EOF
```


## <a name="set-up-ssh-keys-for-mpi"></a>MPI için SSH tuşlarını ayarlama

Bunu gerektiren MPI türleri için SSH tuşlarını ayarlayın.

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

Yukarıdaki sözdizimi paylaşılan bir ev dizini varsayar, else .ssh dizini her düğüme kopyalanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
