---
title: Azure Marketi için Microsoft Azure barındırılan VM'yi yapılandırın
description: Azure'da barındırılan bir VM'yi nasıl boyutlandırır, güncelleştirecek ve genelleştireceğimi açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: f20beced7d977668d12c06375ceb8a2554c6d335
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273435"
---
# <a name="configure-the-azure-hosted-vm"></a>Azure barındırılan VM'yi yapılandırma

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin hareketli yönetimine İş Ortağı Merkezi'ne başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Sanal Makine teknik varlıklarınızı oluştur'daki](https://aka.ms/AzureVMTechAsset) yönergeleri izleyin.

Bu makalede, Azure'da barındırılan sanal bir makinenin (VM) nasıl boyutlandırılabildiğini, güncelleştirilmeye ve genelleştirilene açıklanmaktadır.  Bu adımlar, VM'nizi Azure Marketi'nden dağıtılacak şekilde hazırlamak için gereklidir.

## <a name="sizing-the-vhds"></a>VHD'lerin boyutlandırılması

<!--TD: Check if the following assertion is true. I didn't understand the original content. -->
Bir işletim sistemiyle (ve isteğe bağlı olarak ek hizmetlerle) önceden yapılandırılmış VM'lerden birini seçtiyseniz, Sanal [makine SKUs sekmesinde](./cpp-skus-tab.md)açıklandığı gibi standart bir Azure VM boyutu seçtiniz demektir.  Çözümünüzü önceden yapılandırılmış bir işletim sistemiyle başlatmak önerilen yaklaşımdır.  Ancak, bir işletim sistemi'ni el ile yüklüyorsanız, birincil VHD'nizi VM görüntünüzde boyutlandırmanız gerekir:

- Windows için, işletim sistemi VHD 127-128 GB sabit formatlı VHD olarak oluşturulmalıdır. 
- Linux için, bu VHD 30-50 GB sabit formatlı VHD olarak oluşturulmalıdır.

Fiziksel boyutu 127-128 GB'dan küçükse, VHD seyrek olmalıdır. Sağlanan temel Windows ve SQL Server görüntüleri zaten bu gereksinimleri karşıladığından, elde edilen VHD'nin biçimini veya boyutunu değiştirmez. 

Veri diskleri 1 TB kadar büyük olabilir. Boyutlarına karar verirken, müşterilerin dağıtım sırasında görüntü içindeki VHD'leri yeniden boyutlandıramayacağını unutmayın. Veri diski VHD'leri sabit biçimli VHD'ler olarak oluşturulmalıdır. Onlar da seyrek olmalıdır. Veri diskleri başlangıçta boş olabilir veya veri içerebilir.


## <a name="install-the-most-current-updates"></a>En güncel güncelleştirmeleri yükleme

İşletim sistemi VM'lerinin temel görüntüleri, yayımlandığı tarihe kadar en son güncelleştirmeleri içerir. Oluşturduğunuz vhd işletim sistemini yayımlamadan önce, işletim sistemini ve yüklü tüm hizmetleri en son güvenlik ve bakım yamaları ile güncelleştirdiğinizden emin olun.

Windows Server 2016 **için Güncelleştirmeleri Denetle** komutunu çalıştırın.  Aksi takdirde, Windows'un eski sürümlerinde [Windows Update aracılığıyla güncelleştirmeyi nasıl elde edin.](https://support.microsoft.com/help/3067639/how-to-get-an-update-through-windows-update)  Windows güncelleştirmesi en son kritik ve önemli güvenlik güncelleştirmelerini otomatik olarak yükler.

Linux dağıtımları için güncelleştirmeler genellikle bir komut satırı aracı veya grafik yardımcı programı aracılığıyla indirilir ve yüklenir.  Örneğin, Ubuntu Linux işletim sistemi güncelleştirmek için [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) komutu ve [Update Manager](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) aracını sağlar.


## <a name="perform-additional-security-checks"></a>Ek güvenlik denetimleri gerçekleştirin

Azure Marketi'ndeki çözüm görselleriniz için yüksek düzeyde güvenlik sağlamalısınız.  Aşağıdaki makale, bu hedefte size yardımcı olacak güvenlik yapılandırmaları ve yordamlarının bir denetim listesini sağlar: [Azure Marketi Görselleri için Güvenlik Önerileri.](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images)  Bu önerilerden bazıları Linux tabanlı görüntülere özgüdir, ancak çoğu herhangi bir VM görüntüsü için geçerlidir. 


## <a name="perform-custom-configuration-and-scheduled-tasks"></a>Özel yapılandırma ve zamanlanmış görevleri gerçekleştirme

Ek yapılandırma gerekiyorsa, önerilen yaklaşım, dağıtıldıktan sonra VM'de son değişiklikleri yapmak için başlangıçta çalışan zamanlanmış bir görev kullanmaktır.  Ayrıca aşağıdaki önerileri göz önünde bulundurun:
- Bir kez çalıştırılabilen bir görevse, görevin başarıyla tamamlandıktan sonra kendisini silmesi önerilir.
- Yapılandırmalar C veya D dışındaki sürücülere dayanmamalıdır, çünkü yalnızca her zaman var olması garanti edilen bu iki sürücü. Sürücü C işletim sistemi diski, D sürücüsü ise geçici yerel disktir.

Linux özelleştirmeleri hakkında daha fazla bilgi için, [Linux için Sanal makine uzantıları ve özellikleri](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux)ne bakın.


## <a name="generalize-the-image"></a>Görüntüyü genelleştirin

Azure Marketi'ndeki tüm görüntüler genel bir şekilde yeniden kullanılabilir olmalıdır. Bu yeniden kullanılabilirliği elde etmek için, işletim sistemi VHD *genelleştirilmelidir*, bir VM tüm örnek özel tanımlayıcıları ve yazılım sürücüleri kaldırır bir işlem.

### <a name="windows"></a>Windows

Windows işletim sistemi diskleri [sysprep aracı](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)ile genelleştirilmiştir. Daha sonra işletim sistemi güncelleştirmeveya yeniden yapılandırma, sysprep yeniden gerekir. 

> [!WARNING]
>  Güncelleştirmeler otomatik olarak çalışabilir, sysprep çalıştırdıktan sonra, dağıtılana kadar VM'yi kapatmanız gerekir.  Bu kapatma, sonraki güncelleştirmelerin VHD IŞLETIM Sistemi'nde veya yüklü hizmetlerde örne özel değişiklikler yapmasını önler.

Sysprep çalıştırma hakkında daha fazla bilgi için, [VHD genelleme adımları'na](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep) bakın

### <a name="linux"></a>Linux

Aşağıdaki iki aşamalı işlem bir Linux VM genelleştirir ve ayrı bir VM olarak yeniden dağıtir. Bu iki adım sadece sürecin temelleridir. Bu iki adım ve neden yapılması gerektiği hakkında daha fazla bilgi için, [sanal bir makine veya VHD bir görüntü oluşturmak için nasıl](../../../virtual-machines/linux/capture-image.md)bakın. Azure Marketi teklifiniz için VHD oluşturmak amacıyla, "Yakalanan görüntüden Bir VM oluştur" bölümüne ulaştığınızda durabilirsiniz.

#### <a name="remove-the-azure-linux-agent"></a>Azure Linux aracısını kaldırma
1.  Bir SSH istemcisi kullanarak Linux VM'nize bağlanın.
2.  SSH penceresinde aşağıdaki komutu yazın: <br/>
    `sudo waagent -deprovision+user`
3.  Devam `y` etmek için yazın. (Bu onay `-force` adımıönlemek önceki komuta parametre ekleyebilirsiniz.)
4.  Komut tamamlandıktan sonra, `exit` SSH istemcisini kapatmak için yazın.

<!-- TD: I need to add meat and/or references to the following steps -->
#### <a name="capture-the-image"></a>Görüntüyü Yakalama
1.  Azure portalına gidin, kaynak grubunuzu (RG) seçin ve VM'yi ayırın.
2.  VHD'niz artık genelleştirilmiştir ve bu VHD'yi kullanarak yeni bir VM oluşturabilirsiniz.


## <a name="create-one-or-more-copies"></a>Bir veya daha fazla kopya oluşturma

VM kopyalarını oluşturmak genellikle yedekleme, test, özelleştirilmiş arıza veya yük dengeleme, bir çözümün farklı yapılandırmaları sunmak ve benzeri için yararlıdır. Birincil VHD'yi nasıl kopyalayıp indireceklerine ilişkin bilgi için, yönetilmeyen bir klon yapmak için bkz:

- Linux VM: [Azure'dan Linux VHD İndir](../../../virtual-machines/linux/download-vhd.md)
- Windows VM: [Azure'dan Windows VHD İndir](../../../virtual-machines/windows/download-vhd.md)


## <a name="next-steps"></a>Sonraki adımlar

VM'niz genelleştirildikten, ayrıldıktan ve VM'nin bir görüntüsünü oluşturduktan sonra sanal [bir sabit diskten sanal bir makine dağıtmaya](./cpp-deploy-vm-vhd.md)hazırsınız.
