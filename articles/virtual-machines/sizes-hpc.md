---
title: Azure VM boyutları-HPC | Microsoft Docs
description: Azure 'da yüksek performanslı bilgi işlem sanal makineleri için kullanılabilen farklı boyutları listeler. Bu serideki boyutlarda vCPU sayısı, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: amverma
ms.reviewer: jonbeck
ms.openlocfilehash: a5b0eb0c8bd56a28889945480e95b6413dc9b4dd
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77660028"
---
# <a name="high-performance-compute-vm-sizes"></a>Yüksek performanslı işlem VM boyutları

Azure H serisi sanal makineler (VM 'Ler), çeşitli gerçek dünyada HPC iş yükleri için liderlik sınıfı performans, MPı ölçeklenebilirliği ve maliyet verimliliği sunmak üzere tasarlanmıştır.

[HBv2 serisi](hbv2-series.md) VM 'Ler özelliği 200 GB/sn Mellanox HDR InfiniBand, her ikisi de HB ve HC Serisi VM 'Ler özelliği 100 GB/sn Mellanox EDR InfiniBand. Bu sanal makine türlerinin her biri, iyileştirilmiş ve tutarlı RDMA performansı için engelleyici olmayan bir FAT ağacına bağlanır. HBv2 VM 'Leri, uyarlamalı yönlendirmeyi ve dinamik bağlı taşımayı (Standart RC ve UD aktarımlarında ek olarak) destekler. Bu özellikler uygulama performansı, ölçeklenebilirliği ve tutarlılığı geliştirir ve bunların kullanımı kesinlikle önerilir.

[HB Serisi](hb-series.md) Sanal makineler, akıcı Dynamics, açık sınırlı öğe analizi ve hava durumu modelleme gibi bellek bant genişliğine göre çalışan uygulamalar için iyileştirilmiştir. HB VM 'Ler özelliği 60 AMD EPIC 7551 işlemci çekirdekleri, CPU çekirdeği başına 4 GB RAM ve hiper iş parçacığı yok. AMD EPYıC platformu 260 GB/sn 'den fazla bellek bant genişliği sağlar.

[HC Serisi](hc-series.md) VM 'Ler, örtük sınırlı öğe analizi, molesel Dynamics ve hesaplama Chemistry gibi yoğun hesaplama tarafından yönetilen uygulamalar için iyileştirilmiştir. HC VM 'Ler özelliği 44 Intel Xeon Platinum 8168 işlemci çekirdekleri, CPU çekirdeği başına 8 GB RAM ve hiper iş parçacığı yok. Intel Xeon Platinum platformu Intel Math çekirdek kitaplığı gibi yazılım araçlarının zengin ekosistemini destekler.

[H serisi](h-series.md) VM 'Ler, yüksek CPU sıklıklarca veya çekirdek gereksinimlerine göre büyük bellek kullanan uygulamalar için iyileştirilmiştir. H serisi VM 'Ler özelliği 8 veya 16 Intel Xeon E5 2667 v3 işlemci çekirdeği, 7 veya CPU çekirdeği başına 14 GB RAM ve hiper iş parçacığı yok. Uyumlu RDMA performansı için engelleyici olmayan bir FAT ağacı yapılandırmasında 56 GB/sn Mellanox FDR InfiniBand içindeki H Serisi özellikleri. H serisi VM 'Ler Intel MPı 5. x ve MS-MPı 'yi destekler.

## <a name="deployment-considerations"></a>Dağıtma konuları

- **Azure aboneliği** : birkaç işlem yoğunluğu yoğun örneği dağıtmak için, Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerine göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

- **Fiyatlandırma ve kullanılabilirlik** -bu VM boyutları yalnızca standart fiyatlandırma katmanında sunulur. Azure bölgelerinde kullanılabilirlik için [bölgeye göre kullanılabilir ürünleri](https://azure.microsoft.com/global-infrastructure/services/) denetleyin.
- **Çekirdek kotası** – Azure aboneliğinizdeki çekirdek kotasını varsayılan değerden artırmanız gerekebilir. Aboneliğiniz Ayrıca, H serisi de dahil olmak üzere belirli VM boyutu ailelerinde dağıtabileceğiniz çekirdek sayısını sınırlayabilir. Kota artışı istemek için, ücretsiz [bir çevrimiçi müşteri destek isteği açın](../azure-supportability/how-to-create-azure-support-request.md) . (Varsayılan sınırlar, abonelik kategorime bağlı olarak değişebilir.)

  > [!NOTE]
  > Büyük ölçekli kapasite gereksinimleriniz varsa Azure desteği 'ne başvurun. Azure kotaları, kapasite garantisi değil kredi limitlerdir. Kotasından bağımsız olarak yalnızca kullandığınız çekirdekler için ücretlendirilirsiniz.
  
- **Sanal ağ** : yoğun işlem yoğunluklu örnekleri kullanmak Için bir Azure [sanal ağı](https://azure.microsoft.com/documentation/services/virtual-network/) gerekli değildir. Ancak, birçok dağıtım için, şirket içi kaynaklara erişmeniz gerekiyorsa, en az bir bulut tabanlı Azure sanal ağı veya siteden siteye bağlantı gerekir. Gerektiğinde, örnekleri dağıtmak için yeni bir sanal ağ oluşturun. Benzeşim grubundaki bir sanal ağa işlem yoğunluklu VM 'Lerin eklenmesi desteklenmez.
- **Yeniden boyutlandırma** – kendi özel donanımları nedeniyle, yalnızca aynı büyüklükte aile içindeki işlem yoğunluğu örnekleri yeniden boyutlandırabilirsiniz (H serisi veya Işlem yoğunluklu A serisi). Örneğin, bir h serisi VM 'yi yalnızca bir H serisi boyutundan diğerine yeniden boyutlandırabilirsiniz. Ayrıca, işlem yoğunluğu olmayan bir boyuttan işlem yoğunluğu yoğun bir boyut olarak yeniden boyutlandırılması desteklenmez.  
## <a name="rdma-capable-instances"></a>RDMA özellikli örnekler

Yoğun işlem yoğunluğu olan örneklerin (A8, A9, H16r, H16mr, HB ve HC) bir alt kümesi, uzak doğrudan bellek erişimi (RDMA) bağlantısı için bir ağ arabirimi özelliğidir. ' R ' ile ayrılmış, NC24rs konfigürasyonları (NC24rs_v2 ve NC24rs_v3) gibi seçili N serisi boyutlar da RDMA özellikli. Bu arabirim, diğer VM boyutlarının kullanabildiği standart Azure ağ arabirimine ek niteliğindedir.

Bu arabirim, RDMA özellikli örneklerin bir InfiniBand (ıB) ağı üzerinden iletişim kurmasını, HB, HC, H16r, H16mr ve RDMA özellikli N serisi sanal makineler için de EDR ücretleri ve A8 ve A9 sanal makineler için QDR ücretleri ile çalışma olanağı sağlar. Bu RDMA özellikleri, bazı Ileti geçirme arabirimi (MPı) uygulamalarının ölçeklenebilirliğini ve performansını artırabilir. Hız hakkında daha fazla bilgi için bu sayfadaki tablolardaki ayrıntılara bakın.

> [!NOTE]
> Azure 'da, ıB üzerinden IP yalnızca SR-ıOV etkinleştirilmiş VM 'lerde desteklenir (InfiniBand için SR-ıOV, şu anda HB ve HC). RDMA üzerinden RDMA, tüm RDMA özellikli örnekler için desteklenir.
- **İşletim sistemi** -Windows Server 2016 yukarıdaki HPC serisi VM 'lerde. Windows Server 2012 R2, Windows Server 2012, SR-ıOV olmayan VM 'Lerde de desteklenir (Bu nedenle HB ve HC hariç).

- **MPI** -Azure 'daki SR-ıOV etkin VM boyutları (HB, HC), her türlü MPI 'ın Mellanox ile kullanılmasına izin verir.
SR-ıOV olmayan VM 'lerde desteklenen MPı uygulamaları, örnekler arasında iletişim kurmak için Microsoft ağ doğrudan (ND) arabirimini kullanır. Bu nedenle, yalnızca Microsoft MPı (MS-MPı) 2012 R2 veya üzeri ve Intel MPı 5. x sürümleri desteklenir. Intel MPı çalışma zamanı kitaplığı 'nın sonraki sürümleri (2017, 2018), Azure RDMA sürücüleriyle uyumlu olmayabilir veya olmayabilir.

- **INFINIBANDDRIVERWINDOWS VM Uzantısı** -RDMA özellikli VM 'Lerde, InfiniBand 'i etkinleştirmek Için ınfinibanddriverwindows uzantısını ekleyin. Bu Windows sanal makine uzantısı, RDMA bağlantısı için Windows ağ doğrudan sürücülerini (SR-ıOV olmayan VM 'Lere) veya (SR-ıOV VM 'lerinde) Mellanox sürücüleri (SR-ıOV sanal makinelerinde) kurar.
A8 ve A9 örneklerinin bazı dağıtımlarında, HpcVmDrivers uzantısı otomatik olarak eklenir. HpcVmDrivers VM uzantısının kullanım dışı olduğunu unutmayın; güncellenmeyecektir. VM uzantısını bir VM 'ye eklemek için [Azure PowerShell](/powershell/azure/overview) cmdlet 'lerini kullanabilirsiniz. 

  Aşağıdaki komut, *Batı ABD* bölgesinde *myresourcegroup* adlı kaynak grubunda dağıtılan *MYVM* adlı mevcut bir RDMA özellikli sanal makineye en son 1,0 ınfinibanddriverwindows uzantısını yükleme:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  ```

  Alternatif olarak, VM uzantıları, aşağıdaki JSON öğesiyle kolay dağıtım için Azure Resource Manager şablonlarına dahil edilebilir:

  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverWindows",
  "typeHandlerVersion": "1.0",
  } 
  ```

  Aşağıdaki komut, *Myresourcegroup*adlı kaynak grubunda dağıtılan *myvmss* adlı mevcut bir VM Ölçek kümesindeki tüm RDMA özellikli vm 'Lere en son 1,0 ınfinibanddriverwindows uzantısını yükleme:

  ```powershell
  $VMSS = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS"
  Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "MyVMSS" -VirtualMachineScaleSet $VMSS
  Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -InstanceId "*"
  ```

  Daha fazla bilgi için bkz. [sanal makine uzantıları ve özellikleri](./extensions/overview.md). [Klasik dağıtım modelinde](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/agents-and-extensions-classic)dağıtılan VM 'ler için uzantılara de çalışabilirsiniz.

- **RDMA ağ adresi alanı** -Azure 'daki RDMA ağı, 172.16.0.0/16 adres alanını ayırır. MPı uygulamalarını bir Azure sanal ağında dağıtılan örneklerde çalıştırmak için, sanal ağ adres alanının RDMA ağıyla çakışmadığından emin olun.

## <a name="cluster-configuration-options"></a>Küme yapılandırma seçenekleri

Azure, RDMA ağını kullanarak iletişim kurabilen Windows HPC VM kümeleri oluşturmak için çeşitli seçenekler sunar; örneğin: 

- **Sanal makineler** -RDMA özellikli HPC VM 'lerini aynı Kullanılabilirlik kümesinde dağıtın (Azure Resource Manager dağıtım modelini kullandığınızda). Klasik dağıtım modelini kullanıyorsanız, VM 'Leri aynı bulut hizmetinde dağıtın. 

- **Sanal Makine Ölçek Kümeleri** -sanal makine ölçek kümesinde dağıtımı tek bir yerleştirme grubuyla sınırlandırtığınızdan emin olun. Örneğin, bir Kaynak Yöneticisi şablonunda, `singlePlacementGroup` özelliğini `true`olarak ayarlayın. 

- **Sanal makineler arasında MPI** -sanal makineler (VM 'ler) arasında gerekliyse MPI iletişimi varsa, VM 'lerin aynı Kullanılabilirlik kümesinde veya sanal makinede aynı ölçek kümesinde olduğundan emin olun.

- **Azure CycleCloud** -Windows düğümlerinde MPI işlerini çalıştırmak Için [Azure CYCLECLOUD](/azure/cyclecloud/) 'te bir HPC kümesi oluşturun.

- **Azure Batch** -Windows Server işlem düğümlerinde MPI iş yüklerini çalıştırmak için bir [Azure Batch](/azure/batch/) havuzu oluşturun. Daha fazla bilgi için bkz. [Batch havuzlarında RDMA özellikli veya GPU özellikli örnekler kullanma](../batch/batch-pool-compute-intensive-sizes.md). Toplu iş üzerinde kapsayıcı tabanlı iş yüklerini çalıştırmak için [Batch Shipbahçe](https://github.com/Azure/batch-shipyard) projesine de bakın.

- **MICROSOFT HPC pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) , RDMA özellikli Linux sanal makinelerinde dağıtıldığında Azure RDMA ağını kullanan MS-MPI için bir çalışma zamanı ortamı içerir. Örneğin, dağıtımlar için bkz. [MPI uygulamalarını çalıştırmak IÇIN HPC Pack Ile LINUX RDMA kümesi ayarlama](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- Windows Server 'da HPC Pack ile işlem yoğunluğu kullanan örnekleri kullanmak için denetim listeleri için bkz. [MPI uygulamalarını çalıştırmak IÇIN HPC Pack Ile LINUX RDMA kümesi ayarlama](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).

- MPı uygulamalarını Azure Batch ile çalıştırırken yoğun işlem yoğunluklu örnekler kullanmak için bkz. [Azure Batch 'de Ileti geçirme arabirimi (MPı) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma](../batch/batch-mpi.md).

- Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.