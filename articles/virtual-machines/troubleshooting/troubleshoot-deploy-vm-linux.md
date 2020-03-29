---
title: Azure'da Linux sanal makine sorunlarını dağıtma sorunu | Microsoft Dokümanlar
description: Azure Kaynak Yöneticisi dağıtım modelinde Linux sanal makine sorunlarını dağıtma sorunu giderin.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 5a8a58a3935b7cd5efb8565f7e3278ccaae4f4de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921445"
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>Azure'da Linux sanal makine dağıtma sorunlarını giderme

Azure'da sanal makine (VM) dağıtım sorunlarını gidermek için, sık karşılaşılan hatalar ve çözümler için [en önemli sorunları](#top-issues) gözden geçirin.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.

## <a name="top-issues"></a>En önemli sorunlar
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a>Küme istenen VM boyutunu destekleyemez
\<özellikleri destekTopicIds="123456789" kaynakEtiketler="windows" productPesIds="1234, 5678" />
- Daha küçük bir VM boyutu kullanarak isteği yeniden deneyin.
- İstenen VM'nin boyutu değiştirilemiyorsa:
    - Kullanılabilirlik kümesindeki tüm VM'leri durdurun. Kaynak grubunuz > **Kaynak grupları** > **Kaynakları'nı** tıklatın sanal **makineniz** > > sanal makineniz > iniz > **Durdurun.**
    - Tüm VM'ler durduktan sonra, VM'yi istenilen boyutta oluşturun.
    - Önce yeni VM'yi başlatın ve ardından durdurulan VM'lerin her birini seçin ve Başlat'ı tıklatın.


## <a name="the-cluster-does-not-have-free-resources"></a>Kümenin boş kaynakları yok
\<özellikleri destekTopicIds="123456789" kaynakEtiketler="windows" productPesIds="1234, 5678" />
- İsteği daha sonra yeniden deneyin.
- Yeni VM farklı bir kullanılabilirlik kümesinin parçası olabilirse
    - Farklı bir kullanılabilirlik kümesinde (aynı bölgede) bir VM oluşturun.
    - Yeni VM'yi aynı sanal ağa ekleyin.

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Visual studio Enterprise (BizSpark) için aylık kredimi nasıl etkinleştirebilirim?

Aylık kredinizi etkinleştirmek için bu [makaleye](https://azure.microsoft.com/offers/ms-azr-0064p/)bakın.

## <a name="why-can-i-not-install-the-gpu-driver-for-an-ubuntu-nv-vm"></a>Neden bir Ubuntu NV VM için GPU sürücüsü yükleyemezsiniz?

Şu anda Linux GPU desteği yalnızca Ubuntu Server 16.04 LTS çalıştıran Azure NC VM'lerde kullanılabilir. Daha fazla bilgi için bkz: [Linux çalıştıran N serisi VM'ler için GPU sürücüleri ayarlayın.](../linux/n-series-driver-setup.md)

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>Sürücülerim Linux N-Serisi VM için eksik

Linux tabanlı VM'ler için sürücüler [burada](../linux/n-series-driver-setup.md)bulunmaktadır. 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>N-Serisi VM'imde GPU örneği bulamıyorum

Windows Server 2016 veya Windows Server 2012 R2 çalıştıran Azure N serisi VM'lerin GPU özelliklerinden yararlanmak için dağıtımdan sonra her VM'ye NVIDIA grafik sürücüleri yüklemeniz gerekir. [Windows VM'ler](../windows/n-series-driver-setup.md) ve [Linux VM'leri](../linux/n-series-driver-setup.md)için sürücü kurulum bilgileri mevcuttur.

## <a name="is-n-series-vms-available-in-my-region"></a>N Serisi VM'ler bölgemde kullanılabilir mi?

[Bölge tablosuna göre mevcut Ürünler'den](https://azure.microsoft.com/regions/services)kullanılabilirliği ve fiyatlandırmayı [buradan](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series)kontrol edebilirsiniz.

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>VM'imi yeniden boyutlandırırken istediğim VM Size ailesini göremiyorum.

Bir VM çalışırken, fiziksel bir sunucuya dağıtılır. Azure bölgelerindeki fiziksel sunucular, ortak fiziksel donanım kümeleri halinde gruplandırılır. VM'nin farklı donanım kümelerine taşınmasını gerektiren bir VM'yi yeniden boyutlandırmak, VM'yi dağıtmak için hangi dağıtım modelinin kullanıldığına bağlı olarak farklıdır.

- Klasik dağıtım modelinde dağıtılan VM'ler, VM'leri başka bir boyut ailesinde bir boyuta değiştirmek için bulut hizmeti dağıtımının kaldırılması ve yeniden dağıtılması gerekir.

- Kaynak Yöneticisi dağıtım modelinde dağıtılan VM'ler, kullanılabilirlik kümesindeki herhangi bir VM'nin boyutunu değiştirmeden önce kullanılabilirlik kümesindeki tüm VM'leri durdurmanız gerekir.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Kullanılabilirlik Kümesi'nde dağıtılırken listelenen VM boyutu desteklenmez.

Kullanılabilirlik kümesinin kümesinde desteklenen bir boyut seçin. İhtiyacınız olduğunu düşündüğünüz en büyük VM boyutunu seçmek için bir kullanılabilirlik kümesi oluştururken ve bu durumu kullanılabilirlik kümesine ilk dağıtımınız olması önerilir.

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Azure'da hangi Linux dağıtımları/sürümleri desteklenir?

Listeyi Linux'ta [Azure Onaylı Dağıtımlar'da](../linux/endorsed-distros.md)bulabilirsiniz.

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Kullanılabilirlik kümesine varolan bir Klasik VM ekleyebilir miyim?

Evet. Yeni veya varolan kullanılabilirlik kümesine varolan bir klasik VM ekleyebilirsiniz. Daha fazla bilgi için bkz. [Varolan sanal makineyi kullanılabilirlik kümesine ekle.](/previous-versions/azure/virtual-machines/windows/classic/configure-availability-classic#addmachine)

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz.

Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
