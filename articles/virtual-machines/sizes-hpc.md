---
title: Azure VM boyutları - HPC | Microsoft Dokümanlar
description: Azure'da yüksek performanslı bilgi işlem sanal makineleri için kullanılabilen farklı boyutları listeler. VCPUs, veri diskleri ve NIC'lerin sayısı nın yanı sıra bu serideki boyutlar için depolama verime ve ağ bant genişliği hakkındaki bilgileri listeler.
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
ms.openlocfilehash: df22c857571e51bb886ff1d25db185a306999540
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420868"
---
# <a name="high-performance-computing-vm-sizes"></a>Yüksek performanslı bilgi işlem VM boyutları

Azure H serisi sanal makineler (VM'ler), çeşitli gerçek dünya HPC iş yükleri için liderlik sınıfı performans, MPI ölçeklenebilirliği ve maliyet verimliliği sunmak üzere tasarlanmıştır.

[HBv2 serisi](hbv2-series.md) VM'lerde 200 Gb/sn Mellanox HDR InfiniBand, hem HB hem de HC serisi VM'ler 100 Gb/sn Mellanox EDR InfiniBand'e sahiptir. Bu VM türlerinin her biri, optimize edilmiş ve tutarlı RDMA performansı için engelleyici olmayan bir yağ ağacına bağlanır. HBv2 VM'ler Adaptif Yönlendirme'yi ve Dinamik Bağlantılı Taşımayı (Standart RC ve UD taşımalarına ek olarak DCT) destekler. Bu özellikler uygulama performansını, ölçeklenebilirliği ve tutarlılığı artırır ve bunların kullanımı şiddetle önerilir.

[HB serisi](hb-series.md) VM'ler, akışkandin dinamiği, açık sonlu elemanlar çözümlemesi ve hava durumu modellemesi gibi bellek bant genişliğine bağlı uygulamalar için optimize edilmiştir. HB VM'lerde 60 AMD EPYC 7551 işlemci çekirdeği, CPU çekirdeği başına 4 GB RAM ve hiperiş parçacığı yoktur. AMD EPYC platformu 260 GB/sn'den fazla bellek bant genişliği sağlar.

[HC serisi](hc-series.md) VM'ler, örtük sonlu elemananalizi, moleküler dinamikler ve hesaplamalı kimya gibi yoğun hesaplamayla yönlendirilen uygulamalar için optimize edilmiştir. HC VM'lerde 44 Intel Xeon Platinum 8168 işlemci çekirdeği, CPU çekirdeği başına 8 GB RAM ve hiperiş ipliği yoktur. Intel Xeon Platinum platformu, Intel Math Kernel Library gibi yazılım araçlarından oluşan zengin ekosistemini destekler.

[H serisi](h-series.md) VM'ler, yüksek CPU frekansları veya çekirdek gereksinimleri başına büyük bellek tarafından yönlendirilen uygulamalar için optimize edilebiyi iyileştirilir. H serisi VM'ler 8 veya 16 Intel Xeon E5 2667 v3 işlemci çekirdeği, CPU çekirdeği başına 7 veya 14 GB RAM özelliğine sahiptir ve hiper iş parçacığı yoktur. H serisi, tutarlı RDMA performansı için engelleyici olmayan yağ ağacı yapılandırmasında 56 Gb/sn Mellanox FDR InfiniBand'e sahiptir. H serisi VM'ler Intel MPI 5.x ve MS-MPI'yi destekler.

> [!NOTE]
> A8 – A11 VM'lerin 3/2021 tarihinde emekliye ayrılması planlanmaktadır. Daha fazla bilgi için [HPC Geçiş Kılavuzu'na](https://azure.microsoft.com/resources/hpc-migration-guide/)bakın.

## <a name="rdma-capable-instances"></a>RDMA özellikli örnekler

HPC VM boyutlarının çoğu (HBv2, HB, HC, H16r, H16mr, A8 ve A9) uzaktan doğrudan bellek erişimi (RDMA) bağlantısı için bir ağ arabirimine sahiptir. Seçilen [N serisi]https://docs.microsoft.com/azure/virtual-machines/nc-series) ( NC24rs yapılandırmaları (NC24rs_v3, NC24rs_v2 ve NC24r) gibi 'r' ile belirlenen boyutlar da RDMA özelliklidir. Bu arabirim, diğer VM boyutlarında bulunan standart Azure ağ arabirimine ek olarak sunulur.

Bu arayüz, RDMA özellikli örneklerin HBv2 için HDR oranları, HB için EDR, H, H16mr ve RDMA özellikli N serisi sanal makineler için FDR oranları ve A8 ve A9 VM'ler için QDR oranları yla çalışan bir InfiniBand (IB) ağı üzerinden iletişim kurmasına olanak tanır. Bu RDMA yetenekleri, belirli İleti Geçme Arabirimi (MPI) uygulamalarının ölçeklenebilirliğini ve performansını artırabilir. Hız hakkında daha fazla bilgi için bu sayfadaki tablolardaki ayrıntılara bakın.

> [!NOTE]
> Azure HPC'de, InfiniBand için SR-IOV etkin olup olmadıklarına bağlı olarak iki VM sınıfı vardır. Şu anda, InfiniBand etkin VM'ler için SR-IOV şunlardır: HBv2, HB, HC ve NCv3. InfiniBand özellikli VM'lerin geri kalanı SR-IOV etkin değildir.
> IB üzerinden RDMA tüm RDMA özellikli VM'ler için desteklenir.
> IB üzerinden IP yalnızca SR-IOV etkin VM'lerde desteklenir.

- **İşletim sistemi** - Linux çok iyi HPC VMs için, CentOS, RHEL, Ubuntu, SUSE gibi dağıtımları için desteklenir yaygındır. Windows desteği ile ilgili olarak, Windows Server 2016 tüm HPC serisi VM'lerde desteklenir. Windows Server 2012 R2, Windows Server 2012 de olmayan SR-IOV etkin VMs desteklenir.

- **MPI** - SR-IOV, Azure'daki VM boyutlarını (HBv2, HB, HC, NCv3) etkinleştirerek Mellanox OFED ile hemen hemen her türlü MPI lezzetinin kullanılmasına olanak tanır.
SR-IOV etkin olmayan VM'lerde, desteklenen MPI uygulamaları, VM'ler arasında iletişim kurmak için Microsoft Network Direct (ND) arabirimini kullanır. Bu nedenle, yalnızca Microsoft MPI (MS-MPI) 2012 R2 veya sonraki sürümleri ve Intel MPI 5.x sürümleri desteklenir. Intel MPI çalışma zamanı kitaplığın sonraki sürümleri (2017, 2018) Azure RDMA sürücüleri ile uyumlu olabilir veya olmayabilir.

- **InfiniBandDriver<Linux| Windows> VM uzantısı** - RDMA özellikli VM'lerde InfiniBandDriver<Linux| Windows> uzantısı InfiniBand etkinleştirmek için. Linux'ta, InfiniBandDriverLinux VM uzantısı RDMA bağlantısı için Mellanox OFED sürücülerini (SR-IOV VM'lerde) yükler. Windows'da, InfiniBandDriverWindows VM uzantısı RDMA bağlantısı için Windows Network Direct sürücülerini (SR-IOV VM'lerde olmayan) veya Mellanox OFED sürücülerini (SR-IOV VM'lerde) yükler.
A8 ve A9 örneklerinin belirli dağıtımlarında, HpcVmDrivers uzantısı otomatik olarak eklenir. HpcVmDrivers VM uzantısı nın amortismana alındığını unutmayın; güncellenmeyecektir.
VM uzantısını bir VM'ye eklemek için [Azure PowerShell](/powershell/azure/overview) cmdlets'i kullanabilirsiniz. 

  Aşağıdaki komut, *Batı ABD* bölgesinde *myResourceGroup* adlı kaynak grubunda dağıtılan *myVM* adlı mevcut BIR RDMA özellikli VM'de en son sürüm 1.0 InfiniBandDriverWindows uzantısını yükler:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  ```

  Alternatif olarak, VM uzantıları aşağıdaki JSON öğesiyle kolay dağıtım için Azure Kaynak Yöneticisi şablonlarına eklenebilir:

  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverWindows",
  "typeHandlerVersion": "1.0",
  } 
  ```

  Aşağıdaki *komut, myResourceGroup*adlı kaynak grubunda dağıtılan *myVMSS* adlı mevcut bir sanal makine ölçeğinde tüm RDMA özellikli VM'lerde en son sürüm 1.0 InfiniBandDriverWindows uzantısını yükler:

  ```powershell
  $VMSS = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS"
  Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "InfiniBandDriverWindows" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverWindows" -TypeHandlerVersion "1.0"
  Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "MyVMSS" -VirtualMachineScaleSet $VMSS
  Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -InstanceId "*"
  ```

  Daha fazla bilgi için [Sanal makine uzantıları ve özellikleri](./extensions/overview.md)ne bürünme. [Ayrıca, klasik dağıtım modelinde](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/agents-and-extensions-classic)dağıtılan VM uzantılarıyla da çalışabilirsiniz.

- **RDMA ağ adres alanı** - Azure'daki RDMA ağı adres alanını 172.16.0.0/16 olarak ayırır. Bir Azure sanal ağında dağıtılan örneklerde MPI uygulamalarını çalıştırmak için, sanal ağ adresi alanının RDMA ağıyla çakışmadığından emin olun.

## <a name="cluster-configuration-options"></a>Küme yapılandırma seçenekleri

Azure, RDMA ağını kullanarak iletişim kurabilen Windows HPC VM kümeleri oluşturmak için çeşitli seçenekler sunar: 

- **Sanal makineler** - RDMA özellikli HPC VM'leri aynı ölçek kümesinde veya kullanılabilirlik kümesinde dağıtın (Azure Kaynak Yöneticisi dağıtım modelini kullandığınızda). Klasik dağıtım modelini kullanıyorsanız, VM'leri aynı bulut hizmetine dağıtın.

- **Sanal makine ölçek kümeleri** - Sanal makine ölçek kümesinde (VMSS), dağıtımı tek bir yerleşim grubuyla sınırladığınızdan emin olun. Örneğin, Kaynak Yöneticisi şablonunda `singlePlacementGroup` özelliği `true`' ye göre ayarla. Özellik ile `singlePlacementGroup` döndürülebilecek maksimum VMSS boyutunun varsayılan `true` olarak 100 VM'de kapatılmış olduğunu unutmayın. HPC iş ölçeği gereksinimleriniz tek bir VMSS kiracısında 100 VM'den yüksekse, ücretsiz olarak çevrimiçi [müşteri destek isteği açabilir,](../azure-supportability/how-to-create-azure-support-request.md) bir artış talep edebilirsiniz.

- **Sanal makineler arasında MPI** - SANAL makineler (Örneğin MPI iletişimi kullanmak) gerekirse, SANAL Makineler (VM'ler) arasında, Sanal M'lerin aynı sanal makine ölçeği kümesinde veya kullanılabilirlik kümesinde olduğundan emin olun.

- **Azure CycleCloud** - MPI işlerini çalıştırmak için [Azure CycleCloud'da](/azure/cyclecloud/) bir HPC kümesi oluşturun.

- **Azure Toplu İş -** MPI iş yüklerini çalıştırmak için bir [Azure Toplu İş](/azure/batch/) havuzu oluşturun. Azure Toplu İş birimiyle MPI uygulamalarını çalıştırırken işlem yoğun örnekleri kullanmak [için, Azure Toplu İş Birimi'nde İleti Geçme Arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullan'a](../batch/batch-mpi.md)bakın.

- **Microsoft HPC Pack** - [HPC Paketi,](https://docs.microsoft.com/powershell/high-performance-computing/overview) RDMA özellikli Linux VM'lerinde dağıtıldığında Azure RDMA ağını kullanan MS-MPI için bir çalışma zamanı ortamı içerir. Örneğin dağıtımlar, [bkz.](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam)

## <a name="deployment-considerations"></a>Dağıtma konuları

- **Azure aboneliği** – Birkaç işlem yoğun örneği dağıtmak için, istediğiniz kadar öde aboneliği veya diğer satın alma seçeneklerini düşünün. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

- **Fiyatlandırma ve kullanılabilirlik** - Bu VM boyutları yalnızca Standart fiyatlandırma katmanında sunulur. Azure bölgelerinde kullanılabilirlik için [bölgeye göre kullanılabilen Ürünleri](https://azure.microsoft.com/global-infrastructure/services/) denetleyin.

- **Çekirdek kotası** – Azure aboneliğinizdeki çekirdek kotasını varsayılan değerden artırmanız gerekebilir. Aboneliğiniz, H serisi de dahil olmak üzere belirli VM boyutundaki ailelerde dağıtabileceğiniz çekirdek sayısını da sınırlayabilir. Kota artışı talep etmek için ücretsiz [olarak çevrimiçi müşteri destek isteği açın.](../azure-supportability/how-to-create-azure-support-request.md) (Varsayılan sınırlar abonelik kategorinize bağlı olarak değişebilir.)

  > [!NOTE]
  > Büyük ölçekli kapasite gereksinimleriniz varsa Azure Desteği'ne başvurun. Azure kotaları kapasite garantileri değil, kredi limitleridir. Kotanız ne olursa olsun, yalnızca kullandığınız çekirdekler için ücretlendirilirsiniz.
  
- **Sanal ağ** – Bilgi işlem yoğun örnekleri kullanmak için Bir Azure [sanal ağı](https://azure.microsoft.com/documentation/services/virtual-network/) gerekli değildir. Ancak, birçok dağıtım için en az bulut tabanlı bir Azure sanal ağına veya şirket içi kaynaklara erişmeniz gerekiyorsa siteden siteye bağlantıya ihtiyacınız vardır. Gerektiğinde, örnekleri dağıtmak için yeni bir sanal ağ oluşturun. Bir yakınlık grubunda sanal ağa bilgi işlem yoğun VM'ler eklenmesi desteklenmez.

- **Yeniden boyutlandırma** – Özel donanımları nedeniyle, yalnızca aynı boyuttaki aile (H serisi veya işlem yoğun A serisi) içindeki işlem yoğun örnekleri yeniden boyutlandırabilirsiniz. Örneğin, bir H-serisi VM'yi yalnızca bir H serisi boyutundan diğerine yeniden boyutlandırabilirsiniz. Buna ek olarak, işlem yoğun olmayan bir boyuttan işlem yoğun bir boyuta yeniden boyutlandırma desteklenmez.  


## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure için HPC uygulamanızı en iyi duruma alma hakkında daha fazla bilgi edinin ve [HPC İş Yükleri] (https://docs.microsoft.com/azure/virtual-machines/workloads/hpc/overview) 

- Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
