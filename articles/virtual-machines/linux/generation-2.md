---
title: Nesil 2 VM'ler için Azure desteği
description: Nesil 2 VM'ler için Azure desteğine genel bakış
author: ju-shim
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/11/2020
ms.author: jushiman
ms.openlocfilehash: 0767d6c003be8f4e0b28c8d8ce1d48b8f578dd86
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273537"
---
# <a name="support-for-generation-2-vms-on-azure"></a>Azure'da nesil 2 VM desteği

Nesil 2 sanal makineler (VM'ler) desteği artık Azure'da kullanılabilir. Sanal makineyi oluşturduktan sonra oluşturmayı değiştiremezsiniz, bu nedenle bir nesil seçmeden önce bu sayfadaki hususları gözden geçirin.

Generation 2 VM'ler, nesil 1 VM'lerde desteklenmeyen temel özellikleri destekler. Bu özellikler arasında artırılmış bellek, Intel Software Guard Uzantıları (Intel SGX) ve sanallaştırılmış kalıcı bellek (vPMEM) bulunur. Şirket içinde çalışan Generation 2 VM'ler, Azure'da henüz desteklenmeyen bazı özelliklere sahiptir. Daha fazla bilgi için [Özellikler ve yetenekler](#features-and-capabilities) bölümüne bakın.

Generation 2 VM'ler, nesil 1 VM'ler tarafından kullanılan BIOS tabanlı mimari yerine yeni UEFI tabanlı önyükleme mimarisini kullanır. Nesil 1 VM ile karşılaştırıldığında, nesil 2 VM'ler önyükleme ve yükleme sürelerini geliştirmiş olabilir. Nesil 2 VM'lere ve nesil 1 ile nesil 2 arasındaki bazı farklara genel bir bakış için bkz. [Hyper-V'de bir nesil 1 veya 2 sanal makine oluşturmalı mıyım?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)

## <a name="generation-2-vm-sizes"></a>Nesil 2 VM boyutları

Generation 1 VM'ler Azure'daki tüm VM boyutlarıyla desteklenir (Mv2 serisi VM'ler hariç). Azure artık aşağıdaki seçili VM serisi için nesil 2 desteği sunar:

* [B serisi](https://docs.microsoft.com/azure/virtual-machines/linux/b-series-burstable)
* [DC serisi](../dcv2-series.md)
* [DSv2 serisi](../dv2-dsv2-series.md) ve [Dsv3 serisi](../dv3-dsv3-series.md)
* [Esv3 serisi](../ev3-esv3-series.md)
* [Fsv2 serisi](../fsv2-series.md)
* [GS serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-previous-gen#gs-series)
* [HB serisi](../hb-series.md)
* [HC serisi](../hc-series.md)
* [Ls serisi](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-previous-gen#ls-series) ve [Lsv2 serisi](../lsv2-series.md)
* [Mv2 serisi](../mv2-series.md)
* [NCv2 serisi](../ncv2-series.md) ve [NCv3 serisi](../ncv3-series.md)
* [ND serisi](../nd-series.md)
* [NVv3 serisi](../nvv3-series.md)

> [!NOTE]
> Mv2 serisi VM'ler için nesil 2 VM görüntülerinin kullanımı genellikle kullanılabilir, çünkü Mv2 serisi sadece nesil 2 VM görüntülerle çalışır. Nesil 1 VM görüntüleri Mv2 serisi VM'lerde desteklenmez. 

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Azure Marketinde Nesil 2 VM görüntüleri

Nesil 2 VM'ler aşağıdaki Market görüntülerini destekler:

* Windows Server 2019, 2016, 2012 R2, 2012
* Windows 10
* SUSE Linux Kurumsal Sunucu 15 SP1
* SUSE Linux Kurumsal Sunucu 12 SP4
* Ubuntu Server 16.04, 18.04, 19.04, 19.10 
* RHEL 8.1, 8.0, 7.7, 7.6, 7.5, 7.4, 7.0
* Cent İşletim Sistemi 8.1, 8.0, 7.7, 7.6, 7.5, 7.4
* Oracle Linux 7.7, 7.7-CI

## <a name="on-premises-vs-azure-generation-2-vms"></a>Şirket içi ve Azure oluşturma 2 VM'ler

Azure şu anda şirket içi Hyper-V'nin nesil 2 VM'ler için desteklediği bazı özellikleri desteklememektedir.

| Nesil 2 özelliği                | Şirket içi Hyper-V | Azure |
|-------------------------------------|---------------------|-------|
| Güvenli önyükleme                         | :heavy_check_mark:  | :x:   |
| Korumalı VM                         | :heavy_check_mark:  | :x:   |
| vTPM                                | :heavy_check_mark:  | :x:   |
| Sanallaştırma tabanlı güvenlik (VBS) | :heavy_check_mark:  | :x:   |
| VHDX formatı                         | :heavy_check_mark:  | :x:   |

## <a name="features-and-capabilities"></a>Özellikler ve yetenekler

### <a name="generation-1-vs-generation-2-features"></a>Nesil 1 vs. nesil 2 özellikleri

| Özellik | 1. nesil | 2. nesil |
|---------|--------------|--------------|
| Önyükleme             | PCAT         | Uefı |
| Disk denetleyicileri | IDE          | SCSI |
| VM boyutları         | Tüm VM boyutları | Yalnızca premium depolamayı destekleyen VM'ler |

### <a name="generation-1-vs-generation-2-capabilities"></a>Nesil 1 vs. nesil 2 yetenekleri

| Özellik | 1. nesil | 2. nesil |
|------------|--------------|--------------|
| İşletim sistemi disk > 2 TB                    | :x:                | :heavy_check_mark: |
| Özel disk/resim/takas işletim sistemi         | :heavy_check_mark: | :heavy_check_mark: |
| Sanal makine ölçeği seti desteği | :heavy_check_mark: | :heavy_check_mark: |
| Azure Site Recovery               | :heavy_check_mark: | :heavy_check_mark: |
| Yedekleme/geri yükleme                    | :heavy_check_mark: | :heavy_check_mark: |
| Paylaşılan resim galerisi              | :heavy_check_mark: | :heavy_check_mark: |
| Azure disk şifreleme             | :heavy_check_mark: | :x:                |

## <a name="creating-a-generation-2-vm"></a>Nesil 2 VM oluşturma

### <a name="marketplace-image"></a>Pazar yeri görüntüsü

Azure portalında veya Azure CLI'de, UEFI önyüklemesini destekleyen bir Market görüntüsünden nesil 2 VM'ler oluşturabilirsiniz.

#### <a name="azure-portal"></a>Azure portal

Aşağıda, Azure portalında nesil 2 (Gen2) VM oluşturma adımları verilmiştir.

1. https://portal.azure.com adresinden Azure portalında oturum açın.
1. **Kaynak oluştur**’u seçin.
1. Soldaki Azure Marketi'nden **Tümlerini Gör'e** tıklayın.
1. Gen2'yi destekleyen bir resim seçin.
1. **Oluştur'u**tıklatın.
1. **Gelişmiş** sekmesinde, **VM oluşturma** bölümünün altında **Gen 2** seçeneğini belirleyin.
1. Temel **Bilgiler** sekmesinde, **Örnek Altında ayrıntılar,** **Boyut'a** gidin ve VM boyutu bıçakla **seç'i** açın.
1. Desteklenen [nesil 2 VM](#generation-2-vm-sizes)seçin.
1. [VM'yi](quick-create-portal.md) oluşturmayı bitirmek için Azure portal oluşturma akışını gözden geçirin.

![Gen 1 veya Gen 2 VM'yi seçin](./media/generation-2/gen1-gen2-select.png)

#### <a name="powershell"></a>PowerShell

PowerShell'i, nesil 1 veya nesil 2 SKU'ya doğrudan atıfta bulunarak bir VM oluşturmak için de kullanabilirsiniz.

Örneğin, teklifteki SNU'ların listesini almak için aşağıdaki PowerShell `WindowsServer` cmdlet'i kullanın.

```powershell
Get-AzVMImageSku -Location westus2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```

Alternatif olarak, **Publisher**tarafından listelenen kullanılabilir nesil 2 resimlerini görmek için Azure CLI'yi kullanabilirsiniz.

```azurecli
az vm image list --publisher Canonical --sku gen2 --output table --all
```

Windows Server 2012 işletim sistemi olarak bir VM oluşturuyorsanız, buna benzeyen nesil 1 (BIOS) veya nesil 2 (UEFI) VM SKU'yu seçersiniz:

```powershell
2012-Datacenter
2012-datacenter-gensecond
```

Desteklenen Market görüntülerinin geçerli bir listesi için [Özellikler ve yetenekler](#features-and-capabilities) bölümüne bakın.

### <a name="managed-image-or-managed-disk"></a>Yönetilen görüntü veya yönetilen disk

Yönetilen bir görüntüden veya yönetilen diskten, nesil 1 VM oluşturduğunuz şekilde bir nesil 2 VM oluşturabilirsiniz.

### <a name="virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri

Ayrıca, sanal makine ölçek kümelerini kullanarak nesil 2 VM'ler oluşturabilirsiniz. Azure CLI'de, nesil 2 VM'ler oluşturmak için Azure ölçeği kümelerini kullanın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

* **Nesil 2 VM'ler tüm Azure bölgelerinde kullanılabilir mi?**  
    Evet. Ancak tüm [nesil 2 VM boyutları](#generation-2-vm-sizes) her bölgede mevcut değildir. Nesil 2 VM'nin kullanılabilirliği VM boyutunun kullanılabilirliğine bağlıdır.

* **Nesil 1 ve nesil 2 VM arasında bir fiyat farkı var mı?**  
    Hayır.

* **Şirket içi nesil 2 VM'imden bir .vhd dosyam var. Bu .vhd dosyasını Azure'da bir nesil 2 VM oluşturmak için kullanabilir miyim?**
  Evet, nesil 2 .vhd dosyanızı Azure'a getirebilir ve bunu nesil 2 VM oluşturmak için kullanabilirsiniz. Bunu yapmak için aşağıdaki adımları kullanın:
    1. .vhd'yi VM'nizi oluşturmak istediğiniz bölgedeki bir depolama hesabına yükleyin.
    1. .vhd dosyasından yönetilen bir disk oluşturun. Hyper-V Generation özelliğini V2 olarak ayarlayın. Aşağıdaki PowerShell komutları yönetilen disk oluştururken Hyper-V Generation özelliğini ayarlar.

        ```powershell
        $sourceUri = 'https://xyzstorage.blob.core.windows.net/vhd/abcd.vhd'. #<Provide location to your uploaded .vhd file>
        $osDiskName = 'gen2Diskfrmgenvhd'  #<Provide a name for your disk>
        $diskconfig = New-AzDiskConfig -Location '<location>' -DiskSizeGB 127 -AccountType Standard_LRS -OsType Windows -HyperVGeneration "V2" -SourceUri $sourceUri -CreateOption 'Import'
        New-AzDisk -DiskName $osDiskName -ResourceGroupName '<Your Resource Group>' -Disk $diskconfig
        ```

    1. Disk kullanılabilir olduğunda, bu diski ekleyerek bir VM oluşturun. Oluşturulan VM bir nesil 2 VM olacaktır.
    Nesil 2 VM oluşturulduğunda, isteğe bağlı olarak bu VM'nin görüntüsünü genelleştirebilirsiniz. Görüntüyü genelleştirerek birden çok VM oluşturmak için kullanabilirsiniz.

* **İşletim sistemi disk boyutunu nasıl artırırım?**  
  2 TB'den büyük işletim sistemi diskleri nesilden nesile yeni 2 VM'dir. Varsayılan olarak, işletim sistemi diskleri nesil 2 VM'ler için 2 TB'den daha küçüktür. Disk boyutunu önerilen maksimum 4 TB'ye kadar artırabilirsiniz. İşletim sistemi disk boyutunu artırmak için Azure CLI veya Azure portalını kullanın. Diskleri programlı olarak genişletme hakkında bilgi [için](expand-disks.md)bkz.

  Azure portalından işletim sistemi disk boyutunu artırmak için:

  1. Azure portalında VM özellikleri sayfasına gidin.
  1. VM'yi kapatmak ve anlaşma yapmak için **Durdur** düğmesini seçin.
  1. **Diskler** bölümünde, artırmak istediğiniz işletim sistemi diskini seçin.
  1. **Diskler** bölümünde **Yapılandırma'yı**seçin ve **Boyutu** istediğiniz değere güncelleştirin.
  1. VM özellikleri sayfasına geri dön ve VM'yi **başlat.**

  2 TB'den büyük işletim sistemi diskleri için bir uyarı görebilirsiniz. Uyarı nesil 2 VM için geçerli değildir. Ancak, 4 TB'den büyük işletim sistemi disk boyutları *önerilmez.*

* **Nesil 2 VM'ler hızlandırılmış ağ oluşturmayı destekliyor mu?**  
    Evet. Daha fazla bilgi için bkz: [Hızlandırılmış ağ içeren bir VM oluşturun.](../../virtual-network/create-vm-accelerated-networking-cli.md)

* **VHDX nesil 2'de desteklendi mi?**  
    Hayır, nesil 2 VM'ler yalnızca VHD'yi destekler.

* **Nesil 2 VM'ler Azure Ultra Disk Depolama'yı destekliyor mu?**  
    Evet.

* **Bir VM'yi nesil 1'den nesil 2'ye geçirebilir miyim?**  
    Hayır, oluşturduktan sonra VM'nin neslini değiştiremezsiniz. VM nesilleri arasında geçiş yapmanız gerekiyorsa, farklı nesilde yeni bir VM oluşturun.

* **Gen2 VM oluşturmaya çalıştığımda vm boyutum boyut seçicide neden etkinleştirilmez?**

    Bu, aşağıdakileri yaparak çözülebilir:

    1. **VM oluşturma** özelliğinin **Gelişmiş** sekmesinde **Gen 2** olarak ayarlı olduğundan doğrulayın.
    1. [Gen2 VM'leri destekleyen bir VM boyutu aradığınızı](#generation-2-vm-sizes)doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Hyper-V'de nesil 2 sanal makineler](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)hakkında bilgi edinin.
