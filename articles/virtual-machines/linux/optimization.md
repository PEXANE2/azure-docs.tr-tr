---
title: Azure’da Linux VM’nizi iyileştirme
description: Azure'da en iyi performans için Linux VM'nizi ayarladığınızdan emin olmak için bazı optimizasyon ipuçları öğrenin
author: rickstercdn
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/06/2016
ms.author: rclaus
ms.subservice: disks
ms.openlocfilehash: 87776c14e45ff4bb3cce6661323d74a1315c8ab2
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81757092"
---
# <a name="optimize-your-linux-vm-on-azure"></a>Azure’da Linux VM’nizi iyileştirme
Bir Linux sanal makine (VM) oluşturmak komut satırından veya portaldan kolaydır. Bu öğretici, Microsoft Azure platformunda performansını en iyi duruma getirmek için nasıl ayarladığınızdan emin olabileceğinizi gösterir. Bu konu bir Ubuntu Server VM kullanır, ancak [kendi resimlerinizi şablon olarak](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)kullanarak Linux sanal makine de oluşturabilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar
Bu konu, zaten çalışan bir Azure aboneliğiniz[(ücretsiz deneme kaydolma)](https://azure.microsoft.com/pricing/free-trial/)sahip olduğunuzu ve Azure aboneliğinize zaten bir VM sağlamadığınızı varsayar. [Bir VM oluşturmadan](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)önce en son [Azure CLI'yi](/cli/azure/install-az-cli2) yüklediğinizden ve Azure aboneliğinizde [az oturum açarak](/cli/azure/reference-index) oturum açtığınızdan emin olun.

## <a name="azure-os-disk"></a>Azure İşletim Sistemi Diski
Azure'da bir Linux VM oluşturduktan sonra, onunla ilişkili iki diski vardır. **/dev/sda** işletim sistemi diskiniz, **/dev/sdb** geçici diskinizdir.  Hızlı VM önyükleme süresi için optimize edilmiş ve iş yükleri için iyi bir performans sağlamadığı için işletim sistemi dışında hiçbir şey için ana işletim sistemi diskini **(/dev/sda)** kullanmayın. Verileriniz için kalıcı ve optimize edilmiş depolama alanı elde etmek için VM'nize bir veya daha fazla disk eklemek istiyorsunuz. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Boyut ve Performans hedefleri için Disk ekleme
VM boyutuna bağlı olarak, A Serisi'ne en fazla 16 ek disk, D Serisi'nde 32 disk ve Her biri 32 TB boyutuna kadar olan G-Serisi makinede 64 disk ekleyebilirsiniz. Alanınız ve IOps gereksinimlerinize göre gerektiğinde fazladan disk eklersiniz. Her diskin performans hedefi Standart Depolama için 500 IOps ve Premium Depolama için disk başına 20.000'e kadar IOps'dir.

Önbellek ayarlarının **ReadOnly** veya **None**olarak ayarlandığı Premium Depolama disklerinde en yüksek IOps'leri elde etmek için, Linux'ta dosya sistemini monte ederken **engelleri** devre dışı bırakmalısınız. Premium Depolama destekli diskler bu önbellek ayarları için dayanıklı olduğundan engellere gerek yoktur.

* **ReiserFS**kullanıyorsanız, montaj seçeneğini `barrier=none` kullanarak engelleri devre dışı( engelleri `barrier=flush`etkinleştirmek için kullanın )
* **Ext3/ext4**kullanıyorsanız, montaj seçeneğini `barrier=0` kullanarak bariyerleri devre dışı ( `barrier=1`bariyerleri etkinleştirmek için kullanın )
* **XFS**kullanıyorsanız, montaj seçeneğini `nobarrier` kullanarak engelleri devre dışı( Engelleri etkinleştirmek için seçeneği `barrier`kullanın )

## <a name="unmanaged-storage-account-considerations"></a>Yönetilmeyen depolama hesabı hususları
Azure CLI ile bir VM oluşturduğunuzda varsayılan eylem, Azure Yönetilen Diskler kullanmaktır.  Bu diskler Azure platformu tarafından işlenir ve bunları depolamak için herhangi bir hazırlık veya konum gerektirmez.  Yönetilmeyen diskler bir depolama hesabı gerektirir ve bazı ek performans konuları vardır.  Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../windows/managed-disks-overview.md).  Aşağıdaki bölümde, yalnızca yönetilmeyen diskler kullandığınızda performans konuları özetleniyor.  Yine, varsayılan ve önerilen depolama çözümü yönetilen diskler kullanmaktır.

Yönetilmeyen disklere sahip bir VM oluşturursanız, yakın yakınlığı sağlamak ve ağ gecikmesini en aza indirmek için VM'niz ile aynı bölgede bulunan depolama hesaplarından diskler taktığınızdan emin olun.  Her Standart depolama hesabı en fazla 20k IOps'ye ve 500 TB boyut kapasitesine sahiptir.  Bu sınır, hem işletim sistemi diski hem de oluşturduğunuz tüm veri diskleri de dahil olmak üzere yaklaşık 40 ağır kullanılan diskle çalışır. Premium Depolama hesapları için Maksimum IOps sınırı yoktur, ancak 32 TB boyut sınırı vardır. 

Yüksek IOps iş yükleri ile uğraşırken ve diskleriniz için Standart Depolama'yı seçtiğinizde, Standart Depolama hesapları için 20.000 IOps sınırına çarpmadığınızdan emin olmak için diskleri birden çok depolama hesabına bölmeniz gerekebilir. VM'niz, en iyi yapılandırmanızı elde etmek için farklı depolama hesapları ve depolama hesabı türleri arasında bir disk karışımı içerebilir.
 

## <a name="your-vm-temporary-drive"></a>VM Geçici sürücünüz
Varsayılan olarak bir VM oluşturduğunuzda, Azure size bir işletim sistemi diski **(/dev/sda)** ve geçici bir disk **(/dev/sdb)** sağlar.  Eklediğiniz tüm ek diskler **/dev/sdc**, **/dev/sdd**, **/dev/sde** ve benzeri olarak gösterilmektedir. Geçici diskinizdeki tüm veriler **(/dev/sdb)** dayanıklı değildir ve VM Yeniden Boyutlandırma, yeniden dağıtım veya bakım gibi belirli olaylar VM'nizin yeniden başlatılmasını zorlarsa kaybedilebilir.  Geçici diskinizin boyutu ve türü, dağıtım sırasında seçtiğiniz VM boyutuyla ilişkilidir. Tüm premium boyutu VM 'ler (DS, G ve DS_V2 serisi) geçici sürücü 48k IOps'a kadar ek performans için yerel bir SSD tarafından yedeklenir. 

## <a name="linux-swap-partition"></a>Linux Swap Bölümü
Azure VM'niz bir Ubuntu veya CoreOS görüntüsüne aitse, cloud-init'e bulut config göndermek için CustomData'yı kullanabilirsiniz. Bulut init kullanan [özel bir Linux görüntüsü yüklediyseniz,](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bulut init'i kullanarak takas bölümlerini de yapılandırmış sınız.

Ubuntu Cloud Images'da, takas bölümüyapılandırmak için bulut init'i kullanmanız gerekir. Daha fazla bilgi için [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions)bölümüne bakın.

Bulut init desteği olmayan görüntüler için, Azure Marketi'nden dağıtılan VM görüntülerinde işletim sistemiyle entegre edilmiş bir VM Linux Aracısı vardır. Bu aracı, VM'nin çeşitli Azure hizmetleriyle etkileşim kurmasına olanak tanır. Azure Marketi'nden standart bir resim dağıtdığınızı varsayarsak, Linux takas dosya ayarlarınızı doğru yapılandırmak için aşağıdakileri yapmanız gerekir:

**/etc/waagent.conf** dosyasındaki iki girişi bulun ve değiştirin. Özel bir takas dosyasının varlığını ve takas dosyasının boyutunu denetlerler. Doğrulamak için gereken parametreler `ResourceDisk.EnableSwap` şunlardır ve`ResourceDisk.SwapSizeMB` 

Düzgün şekilde etkinleştirilen bir disk ve monte edilmiş takas dosyasını etkinleştirmek için parametrelerin aşağıdaki ayarlara sahip olduğundan emin olun:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={mb boyutu ihtiyaçlarınızı karşılamak için} 

Değişikliği yaptıktan sonra, bu değişiklikleri yansıtmak için waagent'ı yeniden başlatmanız veya Linux VM'nizi yeniden başlatmanız gerekir.  Değişikliklerin uygulandığını ve boş alanı görüntülemek için komutu `free` kullandığınızda bir takas dosyası oluşturulduğunu biliyorsunuz. Aşağıdaki **örnekte, waagent.conf** dosyasının değiştirilmesi sonucunda oluşturulan 512MB takas dosyası vardır:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>Premium Depolama için G/Ç zamanlama algoritması
2.6.18 Linux çekirdeği ile varsayılan G/Ç zamanlama algoritması Son Başvuru Tarihinden CFQ'ya (Tamamen adil kuyruk algoritması) değiştirildi. Rasgele erişim G/Ç desenleri için CFQ ile Son Tarih arasındaki performans farklarında ihmal edilebilir bir fark vardır.  Disk G/Ç deseninin ağırlıklı olarak sıralı olduğu SSD tabanlı diskler için, NOOP veya Deadline algoritmasına geri geçiş daha iyi G/Ç performansı elde edebilir.

### <a name="view-the-current-io-scheduler"></a>Geçerli G/Ç zamanlayıcısını görüntüleme
Aşağıdaki komutu kullanın:  

```bash
cat /sys/block/sda/queue/scheduler
```

Geçerli zamanlayıcıyı gösteren çıktıyı takip etmek.  

```bash
noop [deadline] cfq
```

### <a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>G/Ç zamanlama algoritmasının geçerli aygıtını (/dev/sda) değiştirme
Aşağıdaki komutları kullanın:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Bu ayarı **tek başına /dev/sda** için uygulamak yararlı değildir. Sıralı G/Ç'nin G/Ç desenine hakim olduğu tüm veri disklerinde ayarlayın.  

**Grub.cfg'nin** başarıyla yeniden inşa edildiğini ve varsayılan zamanlayıcının NOOP olarak güncelleştirildiğini belirten aşağıdaki çıktıyı görmeniz gerekir.  

```bash
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.13.0-34-generic
Found initrd image: /boot/initrd.img-3.13.0-34-generic
Found linux image: /boot/vmlinuz-3.13.0-32-generic
Found initrd image: /boot/initrd.img-3.13.0-32-generic
Found memtest86+ image: /memtest86+.elf
Found memtest86+ image: /memtest86+.bin
done
```

Red Hat dağıtım ailesi için yalnızca aşağıdaki komuta ihtiyacınız var:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

## <a name="using-software-raid-to-achieve-higher-iops"></a>Daha yüksek I/Ops elde etmek için Yazılım RAID'i kullanma
İş yüklerinizin tek bir diskin sağlayabileceğinden daha fazla IOps gerektiriyorsa, birden çok diskten oluşan bir yazılım RAID yapılandırması kullanmanız gerekir. Azure zaten yerel kumaş katmanında disk esnekliği gerçekleştirdiğinden, RAID-0 şeritleme yapılandırmasından en yüksek performans düzeyini elde edeyim.  Azure ortamında diskleri sağlama ve oluşturma ve sürücüleri bölmeden, biçimlendirmeden ve takmadan önce Bunları Linux VM'nize takın.  Azure'daki Linux VM'nizde bir yazılım RAID kurulumu yapılandırma hakkında daha fazla ayrıntı, Linux belgesinde **[Yapılandırma Yazılımı RAID'de](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** bulunabilir.

Geleneksel RAID yapılandırmasına alternatif olarak, bir dizi fiziksel diski tek bir çizgili mantıksal depolama birimine yapılandırmak için Mantıksal Birim Yöneticisi'ni (LVM) yüklemeyi de seçebilirsiniz. Bu yapılandırmada, okuma ve yazma, ses grubunda bulunan birden çok diske (RAID0'a benzer) dağıtılır. Performans nedenleriyle, mantıksal birimlerinizi şeritlemek isteyebilirsiniz, böylece okuma ve yazma tüm bağlı veri disklerinizi kullanır.  Azure'daki Linux VM'nizde çizgili mantıksal birim yapılandırma hakkında daha fazla ayrıntı, Azure belgesinde **[bir Linux VM'de Yapılandırma LVM'sinde](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** bulunabilir.

## <a name="next-steps"></a>Sonraki Adımlar
Tüm optimizasyon tartışmalarında olduğu gibi, değişikliğin etkisini ölçmek için her değişiklikten önce ve sonra testler gerçekleştirmeniz gerektiğini unutmayın.  Optimizasyon, ortamınızdaki farklı makinelerde farklı sonuçlar alabilen adım adım bir işlemdir.  Bir yapılandırma için çalışan şey diğerleri için çalışmayabilir.

Ek kaynaklara bazı yararlı bağlantılar:

* [Azure Linux Aracısı Kullanım Kılavuzu](../extensions/agent-linux.md)
* [Linux'ta Yazılım RAID'i yapılandırın](configure-raid.md)
