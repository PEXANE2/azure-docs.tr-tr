---
title: 2\. nesil VM 'Ler için Azure Desteği (Önizleme) | Microsoft Docs
description: 2\. nesil sanal makineler için Azure desteğine genel bakış
services: virtual-machines-windows
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 09/10/2019
ms.author: lahugh
ms.openlocfilehash: 5e342418dc6cc9ed0a3bbbfaad42801d5ffe9e9d
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70900248"
---
# <a name="support-for-generation-2-vms-preview-on-azure"></a>Azure 'da 2. nesil VM 'Ler (Önizleme) desteği

> [!IMPORTANT]
> 2\. nesil VM 'Ler için Azure desteği şu anda önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

2\. nesil sanal makineler (VM 'Ler) için destek artık Azure 'da önizlemede sunulmaktadır. Oluşturduktan sonra bir sanal makinenin neslini değiştiremezsiniz, bu nedenle bir oluşturma seçmeden önce bu sayfadaki konuları gözden geçirin. 

2\. nesil VM 'ler, 1. nesil VM 'lerde desteklenmeyen önemli özellikleri destekler. Bu özellikler, artan bellek, Intel Software Guard uzantıları (Intel SGX) ve sanallaştırılmış kalıcı bellek (vPMEM) içerir. 2\. nesil VM 'Lerde, henüz Azure 'da desteklenmeyen bazı özellikler de vardır. Daha fazla bilgi için [Özellikler ve yetenekler](#features-and-capabilities) bölümüne bakın.

2\. nesil sanal makineler, 1. nesil VM 'Ler tarafından kullanılan BIOS tabanlı mimaride değil, yeni UEFı tabanlı önyükleme mimarisini kullanır. 1\. nesil VM 'Lerle karşılaştırıldığında 2. nesil VM 'Ler geliştirilmiş önyükleme ve yükleme süreleriyle aynı olabilir. 2\. nesil VM 'Lere genel bakış ve 1. nesil ve 2. nesil arasındaki farklılıklar için bkz. [Hyper-V ' d e 1. nesil veya 2 sanal makine oluşturmalıyım?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>2\. nesil VM boyutları

1\. nesil sanal makineler, Azure 'daki tüm VM boyutları tarafından desteklenir. Azure artık aşağıdaki seçili VM serileri için Önizleme 2. nesil destek sunuyor:

* [B serisi](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable)
* [DC Serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dc-series)
* [Dsv2-Series](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv2-series) ve [Dsv3 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)
* [Esv3 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#esv3-series)
* [Fsv2 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-compute#fsv2-series-1)
* [GS serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#gs-series)
* [HB Serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#hb-series)
* [HC Serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#hc-series)
* [LS-Serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#ls-series) ve [Lsv2 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-storage#lsv2-series)
* [Mv2 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#mv2-series)
* [NCv2-Series](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#ncv2-series) ve [NCv3 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#ncv3-series)
* [ND serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#nd-series)
* [NVv2 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#nvv3-series--1)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Azure Marketi 'nde 2. nesil VM görüntüleri

2\. nesil VM 'Ler aşağıdaki Market görüntülerini destekler:

* Windows Server 2019 Datacenter
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Veri Merkezi
* Windows Server 2012 Datacenter
* SUSE Linux Enterprise Server 15 SP1
* SUSE Linux Enterprise Server 12 SP4

## <a name="on-premises-vs-azure-generation-2-vms"></a>Şirket içi vs. Azure 2. nesil VM 'Ler

Azure, 2. nesil VM 'Ler için şirket içi Hyper-V ' d i destekleyen bazı özellikleri desteklememektedir.

| 2\. nesil özelliği                | Şirket içi Hyper-V | Azure |
|-------------------------------------|---------------------|-------|
| Güvenli önyükleme                         | :heavy_check_mark:  | sayı   |
| Korumalı VM                         | :heavy_check_mark:  | sayı   |
| vTPM                                | :heavy_check_mark:  | sayı   |
| Sanallaştırma tabanlı güvenlik (VBS) | :heavy_check_mark:  | sayı   |
| VHDX biçimi                         | :heavy_check_mark:  | sayı   |

## <a name="features-and-capabilities"></a>Özellikler ve yetenekler

### <a name="generation-1-vs-generation-2-features"></a>1\. nesil ve 2. nesil Özellikler

| Özellik | 1\. nesil | 2\. nesil |
|---------|--------------|--------------|
| Önyükleme             | PCAT                      | UEFI                               |
| Disk denetleyicileri | IDE                       | SCSI                               |
| VM boyutları         | Tüm VM boyutları | Yalnızca Premium depolamayı destekleyen VM 'Ler |

### <a name="generation-1-vs-generation-2-capabilities"></a>1\. nesil ve 2. nesil yetenekler

| Özellik | 1\. nesil | 2\. nesil |
|------------|--------------|--------------|
| İşletim sistemi diski > 2 TB                    | sayı                        | :heavy_check_mark: |
| Özel disk/görüntü/takas OS         | :heavy_check_mark:         | :heavy_check_mark: |
| Sanal makine ölçek kümesi desteği | :heavy_check_mark:         | :heavy_check_mark: |
| ASR/yedekleme                        | :heavy_check_mark:         | sayı                |
| Paylaşılan görüntü galerisi              | :heavy_check_mark:         | sayı                |
| Azure disk şifrelemesi             | :heavy_check_mark:         | sayı                |

## <a name="creating-a-generation-2-vm"></a>2\. nesil VM oluşturma

### <a name="marketplace-image"></a>Market görüntüsü

Azure portal veya Azure CLı 'de, UEFı önyüklemesini destekleyen bir market görüntüsünden 2. nesil VM 'Ler oluşturabilirsiniz.

`windowsserver-gen2preview` Teklif yalnızca Windows 2. nesil görüntüleri içerir. Bu paketleme 1. kuşak ve 2. nesil görüntüler arasındaki karışıklığı önler. 2\. nesil VM oluşturmak için bu tekliften **görüntüler** ' i seçin ve sanal makineyi oluşturmak için standart süreci izleyin.

Market şu anda aşağıdaki Windows nesil 2 görüntülerini sunmaktadır:

* 2019-Datacenter-Gen2
* 2016-Datacenter-Gen2
* 2012-R2-Datacenter-Gen2
* 2012-Datacenter-Gen2

Desteklenen Market görüntülerinin geçerli listesi için [Özellikler ve yetenekler](#features-and-capabilities) bölümüne bakın.

### <a name="managed-image-or-managed-disk"></a>Yönetilen görüntü veya yönetilen disk

Yönetilen bir görüntüden veya yönetilen diskten, 1. nesil bir VM oluşturduğunuz şekilde bir 2. nesil VM oluşturabilirsiniz.

### <a name="virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri

Ayrıca, sanal makine ölçek kümelerini kullanarak 2. nesil VM 'Ler oluşturabilirsiniz. Azure CLı 'da 2. nesil VM 'Ler oluşturmak için Azure Ölçek Kümeleri ' ni kullanın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

* **2. nesil VM 'Ler tüm Azure bölgelerinde kullanılabilir mi?**  
    Evet. Her bölgede [2. nesıl VM boyutları](#generation-2-vm-sizes) kullanılamaz. 2\. nesil VM 'nin kullanılabilirliği, VM boyutunun kullanılabilirliğine bağlıdır.

* **1. nesil ve 2. nesil VM 'Ler arasında bir fiyat farkı var mı?**  
   Hayır.

* **Şirket içi nesil 2 VM 'den bir. vhd dosyası var. Azure 'da 2. nesil VM oluşturmak için bu. vhd dosyasını kullanabilir miyim?**
  Evet, 2. nesil. vhd dosyanızı Azure 'a getirebilir ve bunu kullanarak 2. nesil bir VM oluşturabilirsiniz. Bunu yapmak için aşağıdaki adımları kullanın:
    1. . Vhd 'yi, sanal makineyi oluşturmak istediğiniz bölgedeki bir depolama hesabına yükleyin.
    1. . Vhd dosyasından yönetilen disk oluşturun. HyperV oluşturma özelliğini v2 olarak ayarlayın. Aşağıdaki PowerShell komutları, yönetilen disk oluştururken HyperV oluşturma özelliğini ayarlar.

        ```powershell
        $sourceUri = 'https://xyzstorage.blob.core.windows.net/vhd/abcd.vhd'. #<Provide location to your uploaded .vhd file>
        $osDiskName = 'gen2Diskfrmgenvhd'  #<Provide a name for your disk>
        $diskconfig = New-AzDiskConfig -Location '<location>' -DiskSizeGB 127 -AccountType Standard_LRS -OsType Windows -HyperVGeneration "V2" -SourceUri $sourceUri -CreateOption 'Import'
        New-AzDisk -DiskName $osDiskName -ResourceGroupName '<Your Resource Group>' -Disk $diskconfig
        ```

    1. Disk kullanılabilir olduğunda, bu diski ekleyerek bir VM oluşturun. Oluşturulan VM bir 2. nesil VM olacaktır.
    2\. nesil VM oluşturulduğunda, bu VM 'nin görüntüsünü isteğe bağlı olarak genelleştirmenize de sağlayabilirsiniz. Görüntüyü genelleştirerek birden çok VM oluşturmak için kullanabilirsiniz.

* **Nasıl yaparım? işletim sistemi disk boyutu artsın mı?**  
  2 TB 'den büyük işletim sistemi diskleri 2. nesil VM 'lere yenidir. Varsayılan olarak, 1. nesil VM 'Ler için işletim sistemi diskleri 2 TB 'den daha küçüktür. Disk boyutunu önerilen en fazla 4 TB 'a kadar artırabilirsiniz. İşletim sistemi disk boyutunu artırmak için Azure CLı veya Azure portal kullanın. Diskleri program aracılığıyla genişletme hakkında daha fazla bilgi için bkz. [diski yeniden boyutlandırma](expand-os-disk.md).

  İşletim sistemi diskinin boyutunu Azure portal artırmak için:

  1. Azure portal VM özellikleri sayfasına gidin.
  1. VM 'yi kapatmak ve serbest bırakmak için **Durdur** düğmesini seçin.
  1. **Diskler** bölümünde, yükseltmek istediğiniz işletim sistemi diskini seçin.
  1. **Diskler** bölümünde **yapılandırma**' yı seçin ve **boyutu** istediğiniz değerle güncelleştirin.
  1. VM özellikleri sayfasına dönün ve VM 'yi **başlatın** .
  
  2 TB 'den büyük işletim sistemi diskleri için bir uyarı görebilirsiniz. Uyarı 2. nesil VM 'lere uygulanmaz. Ancak, 4 TB 'tan büyük işletim sistemi disk boyutları *önerilmez.*

* **2. nesil VM 'Ler hızlandırılmış ağı destekliyor mu?**  
    Evet. Daha fazla bilgi için bkz. [hızlandırılmış ağ Ile VM oluşturma](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **VHDX 2. nesil üzerinde destekleniyor mu?**  
    Hayır, 2. nesil VM 'Ler yalnızca VHD 'YI destekler.

* **2. nesil VM 'Ler Azure Ultra Disk Depolama 'yi destekliyor mu?**  
    Evet.

* **1. nesil bir VM 'yi 2. nesil 'e geçirebilir miyim?**  
    Hayır, bir VM 'yi oluşturduktan sonra oluşturmayı değiştiremezsiniz. VM nesilleri arasında geçiş yapmanız gerekiyorsa, farklı nesil yeni bir VM oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* [Hyper-V ' d a 2. nesil sanal makineler](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)hakkında bilgi edinin.

* [BIR VHD](prepare-for-upload-vhd-image.md) 'yi şirket Içi sistemlerden Azure 'a yüklemek için hazırlama hakkında bilgi edinin.
