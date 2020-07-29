---
title: H serisi-Azure sanal makineleri
description: H serisi VM 'Ler için belirtim.
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: jushiman
ms.openlocfilehash: 34f89656fc56b27edafeff0e60874a00139266dd
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87268960"
---
# <a name="h-series"></a>H Serisi

H serisi VM 'Ler, yüksek CPU sıklıklarca veya çekirdek gereksinimlerine göre büyük bellek kullanan uygulamalar için iyileştirilmiştir. H serisi VM 'Ler özelliği 8 veya 16 Intel Xeon E5 2667 v3 işlemci çekirdekleri, CPU çekirdeği başına en fazla 14 GB RAM ve hiper iş parçacığı yok. Uyumlu RDMA performansı için engelleyici olmayan bir FAT ağacı yapılandırmasında 56 GB/sn Mellanox FDR InfiniBand içindeki H Serisi özellikleri. H serisi VM 'Ler Intel MPı 5. x ve MS-MPı 'yi destekler.

ACU: 290-300

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek bant genişliği GB/sn | Taban CPU sıklığı (GHz) | Tüm çekirdekler sıklığı (GHz, tepe) | Tek çekirdekli sıklık (GHz, tepe) | RDMA performansı (GB/sn) | MPı desteği | Geçici depolama (GB) | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | En fazla Ethernet NIC |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8   | 8  | Intel Xeon E5 2667 v3 | 56 | 40 | 3.2 | 3.3 | 3,6 | - | Intel 5. x, MS-MPı | 1000 | 32 | 32x500 | 2 |
| Standard_H16  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3,6 | - | Intel 5. x, MS-MPı | 2000 | 64 | 64x500 | 4 |
| Standard_H8m  | 8  | Intel Xeon E5 2667 v3 | 112 | 40 | 3.2 | 3.3 | 3,6 | - | Intel 5. x, MS-MPı | 1000 | 32 | 32x500 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3,6 | - | Intel 5. x, MS-MPı | 2000 | 64 | 64x500 | 4 |
| Standard_H16r <sup>1</sup>  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3,6 | 56 | Intel 5. x, MS-MPı | 2000 | 64 | 64x500 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3,6 | 56 | Intel 5. x, MS-MPı | 2000 | 64 | 64x500 | 4 |

<sup>1</sup> MPI uygulamaları için, adanmış RDMA arka uç ağı, FDR InfiniBand ağı tarafından etkinleştirilir.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]


## <a name="supported-os-images-linux"></a>Desteklenen işletim sistemi görüntüleri (Linux)
 
Azure Marketi 'nde RDMA bağlantısını destekleyen birçok Linux dağıtımı vardır:
  
* **CentOS tabanlı HPC** -SR-IOV etkin olmayan VM 'Ler, CentOS tabanlı sürüm 6,5 HPC veya sonraki bir sürüm olan 7,5 ' e kadar uygundur. H serisi VM 'Ler için 7,1 sürümlerinin 7,5 olması önerilir. RDMA sürücüleri ve Intel MPı 5,1 VM 'ye yüklenir.
  SR-ıOV VM 'lerinde, CentOS-HPC 7,6, RDMA sürücüleri ve çeşitli MPı paketleri yüklüyken en iyi duruma getirilmiş ve önceden yüklenmiş olarak sunulur.
  Diğer RHEL/CentOS VM görüntüleri için, InfiniBand 'yi etkinleştirmek üzere ınfinıbandlinux uzantısını ekleyin. Bu Linux VM uzantısı, RDMA bağlantısı için (SR-ıOV VM 'lerde), Mellanox temelli sürücüleri (SR-ıOV) Aşağıdaki PowerShell cmdlet 'i, mevcut bir RDMA özellikli sanal makineye ınfinibanddriverlinux uzantısının en son sürümünü (sürüm 1,0) yüklüyor. RDMA özellikli VM, *Myvm* olarak adlandırılır ve *Batı ABD* bölgesindeki *myresourcegroup* adlı kaynak grubunda aşağıdaki gibi dağıtılır:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  ```
  Alternatif olarak, VM uzantıları aşağıdaki JSON öğesiyle kolay dağıtım için Azure Resource Manager şablonlarına dahil edilebilir:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverLinux",
  "typeHandlerVersion": "1.0",
  } 
  ```
  
  Aşağıdaki komut, *Myresourcegroup*adlı kaynak grubunda dağıtılan *myvmss* adlı mevcut bir sanal makıne ölçek kümesindeki tüm RDMA özellikli vm 'Lere en son 1,0 ınfinibanddriverlinux uzantısını yükleme:
  ```powershell
  $VMSS = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS"
  Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "MyVMSS" -VirtualMachineScaleSet $VMSS
  Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -InstanceId "*"
  ```
  
  > [!NOTE]
  > CentOS tabanlı HPC görüntülerinde, çekirdek güncelleştirmeleri, **yıum** yapılandırma dosyasında devre dışıdır. Bunun nedeni, Linux RDMA sürücülerinin bir RPM paketi olarak dağıtılmaktadır ve çekirdek güncelleştirilirse sürücü güncelleştirmeleri çalışmayabilir.
  >
  

* **SUSE Linux Enterprise Server** , HPC için SLES 12 SP3, HPC için SLES 12 SP3 (Premium), HPC için SLES 12 SP1, HPC için SLES 12 SP1, HPC (Premium), SLES 12 SP4 ve SLES 15. RDMA sürücüleri yüklenir ve Intel MPı paketleri sanal makineye dağıtılır. Aşağıdaki komutu çalıştırarak MPı 'yi çalıştırın:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
  
* **Ubuntu** -Ubuntu Server 16,04 LTS, 18,04 LTS. VM 'de RDMA sürücülerini yapılandırın ve Intel MPı 'yi indirmek için Intel ile kaydolun:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../includes/virtual-machines-common-ubuntu-rdma.md)]  

  InfiniBand 'yi etkinleştirme hakkında daha fazla ayrıntı için MPı 'yi [ayarlama bölümüne bakın](./workloads/hpc/enable-infiniband.md).

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
