---
title: Azure portalında özel bir VHD'den Windows VM oluşturma
description: Azure portalında bir VHD'den yeni bir Windows VM oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 01/18/2019
ms.author: cynthn
ms.openlocfilehash: 587acd37f465dda7e514b595f8451afe3d259dea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74039138"
---
# <a name="create-a-vm-from-a-vhd-by-using-the-azure-portal"></a>Azure portalını kullanarak Bir VHD'den VM oluşturma

Azure'da sanal makine (VM) oluşturmanın birkaç yolu vardır: 

- Zaten kullanmak için sanal bir sabit disk (VHD) varsa veya kullanmak için mevcut bir VM VHD kopyalamak istiyorsanız, bir işletim sistemi disk olarak yeni VM VHD *ekleyerek* yeni bir VM oluşturabilirsiniz. 

- Silinmiş bir VM'nin VHD'sinden yeni bir VM oluşturabilirsiniz. Örneğin, düzgün çalışmayan bir Azure VM'iniz varsa, VM'yi silebilir ve yeni bir VM oluşturmak için VHD'sini kullanabilirsiniz. Aynı VHD'yi yeniden kullanabilir veya anlık görüntü oluşturup anlık görüntüden yeni yönetilen bir disk oluşturarak VHD'nin bir kopyasını oluşturabilirsiniz. Anlık görüntü oluşturma birkaç adım daha alsa da, orijinal VHD'yi korur ve size bir geri dönüş sağlar.

- Klasik bir VM alın ve Kaynak Yöneticisi dağıtım modelini ve yönetilen diskleri kullanan yeni bir VM oluşturmak için VHD'yi kullanın. En iyi sonuçlar için, anlık görüntü oluşturmadan önce Azure portalındaki klasik VM'yi **durdurun.**
 
- Şirket içi VHD'yi yükleyip yeni bir VM'ye ekleyerek şirket içi Bir VHD'den azure VM oluşturabilirsiniz. VHD'yi bir depolama hesabına yüklemek için PowerShell veya başka bir araç kullanırsınız ve ardından VHD'den yönetilen bir disk oluşturursunuz. Daha fazla bilgi için özel [bir VHD Yükle'ye](create-vm-specialized.md#option-2-upload-a-specialized-vhd)bakın. 

Birden çok VM oluşturmak istiyorsanız özel bir disk kullanmayın. Bunun yerine, daha büyük dağıtımlar için [bir görüntü oluşturun](capture-image-resource.md) ve sonra birden çok [VM oluşturmak için bu görüntüyü kullanın.](create-vm-generalized-managed.md)

Eşzamanlı dağıtım sayısını tek bir anlık görüntüveya VHD'den 20 VM ile sınırlamanızı öneririz. 

## <a name="copy-a-disk"></a>Disk kopyalama

Anlık görüntü oluşturun ve ardından anlık görüntüden bir disk oluşturun. Bu strateji bir geri dönüş olarak orijinal VHD tutmak için izin verir:

1. Sol menüdeki [Azure portalından](https://portal.azure.com) **Tüm hizmetler'i**seçin.
2. Tüm **hizmetler** arama kutusuna, **diskler** girin ve ardından kullanılabilir disklerin listesini görüntülemek için **Diskler'i** seçin.
3. Kullanmak istediğiniz diski seçin. Bu diskin **Disk** sayfası görüntülenir.
4. Üstteki menüden anlık **görüntü oluştur'u**seçin. 
5. Anlık görüntü için bir **Ad** girin.
6. Anlık görüntü için bir **Kaynak grubu** seçin. Varolan bir kaynak grubunu kullanabilir veya yeni bir kaynak oluşturabilirsiniz.
7. **Hesap türü için** **Standart (HDD)** veya **Premium (SSD)** depolama alanını seçin.
8. İşi bittiğinde, anlık görüntü oluşturmak için **Oluştur'u** seçin.
9. Anlık görüntü oluşturulduktan sonra, sol menüde **kaynak oluştur'u** seçin.
10. Arama kutusuna yönetilen **diski** girin ve ardından listeden **Yönetilen Diskler'i** seçin.
11. Yönetilen **Diskler** sayfasında **Oluştur'u**seçin.
12. Disk için bir **Ad** girin.
13. Disk için bir **Kaynak grubu** seçin. Varolan bir kaynak grubunu kullanabilir veya yeni bir kaynak oluşturabilirsiniz. Bu seçim, diskten VM oluşturduğunuz kaynak grubu olarak da kullanılır.
14. **Hesap türü için** **Standart (HDD)** veya **Premium (SSD)** depolama alanını seçin.
15. **Kaynak türünde,** **Anlık Görüntü'ün** seçildiğinden emin olun.
16. Kaynak **anlık görüntü** açılır durumda, kullanmak istediğiniz anlık görüntü'nü seçin.
17. Gerektiğinde diğer ayarlamaları yapın ve ardından diski oluşturmak için **Oluştur'u** seçin.

## <a name="create-a-vm-from-a-disk"></a>Diskten VM oluşturma

Kullanmak istediğiniz yönetilen disk VHD'yi aldıktan sonra portalda VM oluşturabilirsiniz:

1. Sol menüdeki [Azure portalından](https://portal.azure.com) **Tüm hizmetler'i**seçin.
2. Tüm **hizmetler** arama kutusuna, **diskler** girin ve ardından kullanılabilir disklerin listesini görüntülemek için **Diskler'i** seçin.
3. Kullanmak istediğiniz diski seçin. Bu diskin **Disk** sayfası açılır.
4. Genel **Bakış** sayfasında, **DİsK DURUM'un** **eklenmemiş**olarak listelendirildiğından emin olun. Değilse, diski VM'den ayırmanız veya diski boşaltmak için VM'yi silmeniz gerekebilir.
4. Sayfanın üst kısmındaki menüde **VM oluştur'u**seçin.
5. Yeni VM'nin **Temel Bilgileri** sayfasında Sanal **makine adı** girin ve varolan bir Kaynak **grubu** seçin veya yeni bir tane oluşturun.
6. **Boyut**için, **Boyut** sayfasına erişmek için **boyutu değiştir'i** seçin.
7. VM boyutu satırseçin ve sonra **seçin.**
8. **Ağ** sayfasında, portalın tüm yeni kaynakları oluşturmasına izin verebilir veya varolan bir **Sanal ağ** ve Ağ **güvenlik grubu**seçebilirsiniz. Portal her zaman yeni VM için yeni bir ağ arabirimi ve genel IP adresi oluşturur. 
9. **Yönetim** sayfasında, izleme seçeneklerinde herhangi bir değişiklik yapın.
10. Konuk **config** sayfasında, gerektiğinde uzantıları ekleyin.
11. İşi **bittiğinde, Gözden Geçir + oluştur'u**seçin. 
12. VM yapılandırması doğrulamayı geçerse, dağıtımı başlatmak için **Oluştur'u** seçin.


## <a name="next-steps"></a>Sonraki adımlar

PowerShell'i [Azure'a bir VHD yüklemek ve özel bir VM oluşturmak için](create-vm-specialized.md)de kullanabilirsiniz.


