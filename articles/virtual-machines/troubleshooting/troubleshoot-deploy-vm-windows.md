---
title: Azure'da Windows sanal makine sorunlarını dağıtma sorunu | Microsoft Dokümanlar
description: Azure Kaynak Yöneticisi dağıtım modelinde Windows sanal makine sorunlarını dağıtma sorunu giderin.
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
ms.openlocfilehash: cdbaeb5a97beba342bc471e75d1b07be0d0141ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921420"
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>Azure'da Windows sanal makinesi dağıtma sorunlarını giderme

Azure'da sanal makine (VM) dağıtım sorunlarını gidermek için, sık karşılaşılan hatalar ve çözümler için [en önemli sorunları](#top-issues) gözden geçirin.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.

## <a name="top-issues"></a>En önemli sorunlar
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

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

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a>Windows istemcisi görüntüsünü Azure'da nasıl kullanabilir ve dağıtabilirim?

Uygun bir Visual Studio (eski MSDN) aboneliğiniz varsa, geliştirme/test senaryoları için Azure'da Windows 7, Windows 8 veya Windows 10'u kullanabilirsiniz. Bu [makalede,](../windows/client-images.md) Azure'da Windows istemcisini çalıştırmak için uygunluk gereksinimleri ve Azure Galerisi görüntülerinin kullanımı özetlenmiştir.

## <a name="how-can-i-deploy-a-virtual-machine-using-the-hybrid-use-benefit-hub"></a>Hibrit Kullanım Avantajı'nı (HUB) kullanarak sanal bir makineyi nasıl dağıtabilirim?

Azure Karma Kullanım Avantajı ile Windows sanal makinelerini dağıtmanın birkaç farklı yolu vardır.

Kurumsal Sözleşme aboneliği için:

• Azure Karma Kullanım Avantajı ile önceden yapılandırılmış belirli Market görüntülerinden VM'leri dağıtın.

Kurumsal anlaşma için:

• Özel bir VM yükleyin ve Kaynak Yöneticisi şablonu veya Azure PowerShell kullanarak dağıtın.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

 - [Azure Karma Kullanım Avantajına genel bakış](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [İndirilebilir SSS](https://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - [Windows Server ve Windows İstemci için Azure Karma Kullanım Avantajı.](../windows/hybrid-use-benefit-licensing.md)

 - [Azure'da Karma Kullanım Avantajını nasıl kullanabilirim?](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Visual studio Enterprise (BizSpark) için aylık kredimi nasıl etkinleştirebilirim?

Aylık kredinizi etkinleştirmek için bu [makaleye](https://azure.microsoft.com/offers/ms-azr-0064p/)bakın.

## <a name="how-to-add-enterprise-devtest-to-my-enterprise-agreement-ea-to-get-access-to-window-client-images"></a>Pencere istemci görüntülerine erişmek için Kurumsal Sözleşmeme (EA) Kurumsal Dev/Test nasıl eklenir?

Kurumsal Geliştirme/Test teklifine dayalı abonelik oluşturma olanağı, bir Kurumsal Yönetici tarafından izin verilen Hesap Sahipleri ile sınırlıdır. Hesap Sahibi, Azure Hesap Portalı üzerinden abonelikler oluşturur ve ardından ortak yönetici olarak etkin Visual Studio aboneleri eklemelidir. Böylece geliştirme ve test için gerekli kaynakları yönetebilir ve kullanabilirler. Daha fazla bilgi için bkz. [Kurumsal Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a>Sürücülerim Windows N Serisi VM'im için eksik

Windows tabanlı VM'ler için sürücüler [burada](../windows/n-series-driver-setup.md)bulunur.

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>N-Serisi VM'imde GPU örneği bulamıyorum

Windows Server 2016 veya Windows Server 2012 R2 çalıştıran Azure N serisi VM'lerin GPU özelliklerinden yararlanmak için dağıtımdan sonra her VM'ye NVIDIA grafik sürücüleri yüklemeniz gerekir. [Windows VM'ler](../windows/n-series-driver-setup.md) ve [Linux VM'leri](../linux/n-series-driver-setup.md)için sürücü kurulum bilgileri mevcuttur.

## <a name="is-n-series-vms-available-in-my-region"></a>N Serisi VM'ler bölgemde kullanılabilir mi?

[Bölge tablosuna göre mevcut Ürünler'den](https://azure.microsoft.com/regions/services)kullanılabilirliği ve fiyatlandırmayı [buradan](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series)kontrol edebilirsiniz.

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-to-i-get-them"></a>Azure'da hangi istemci resimlerini kullanabilir ve dağıtabilirim ve bunları nasıl alabilirim?

Uygun bir Visual Studio (eski MSDN) aboneliğiniz olması koşuluyla geliştirme/test senaryoları için Azure'da Windows 7, Windows 8 veya Windows 10'u kullanabilirsiniz. 

- Windows 10 görüntüleri Azure Galerisi'nden [uygun geliştirme/test teklifleri](../windows/client-images.md#eligible-offers)içinde edinilebilir. 
- Herhangi bir teklif türü içindeki Visual Studio aboneleri de [yeterli şekilde hazırlayabilir ve](../windows/prepare-for-upload-vhd-image.md) 64 bit Windows 7, Windows 8 veya Windows 10 görüntüsü oluşturabilir ve ardından [Azure'a yükleyebilir.](../windows/upload-generalized-managed.md) Kullanım, aktif Visual Studio aboneleri tarafından yapılan geliştirme/test ile sınırlı kalır.

Bu [makalede,](../windows/client-images.md) Azure'da Windows istemcisini çalıştırmak ve Azure Galerisi görüntülerinin kullanımı için uygunluk gereksinimleri özetlenmiştir.

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>VM'imi yeniden boyutlandırırken istediğim VM Size ailesini göremiyorum.

Bir VM çalışırken, fiziksel bir sunucuya dağıtılır. Azure bölgelerindeki fiziksel sunucular, ortak fiziksel donanım kümeleri halinde gruplandırılır. VM'nin farklı donanım kümelerine taşınmasını gerektiren bir VM'yi yeniden boyutlandırmak, VM'yi dağıtmak için hangi dağıtım modelinin kullanıldığına bağlı olarak farklıdır.

- Klasik dağıtım modelinde dağıtılan VM'ler, VM'leri başka bir boyut ailesinde bir boyuta değiştirmek için bulut hizmeti dağıtımının kaldırılması ve yeniden dağıtılması gerekir.

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

- Kaynak Yöneticisi dağıtım modelinde dağıtılan VM'ler, kullanılabilirlik kümesindeki herhangi bir VM'nin boyutunu değiştirmeden önce kullanılabilirlik kümesindeki tüm VM'leri durdurmanız gerekir.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Kullanılabilirlik Kümesi'nde dağıtılırken listelenen VM boyutu desteklenmez.

Kullanılabilirlik kümesinin kümesinde desteklenen bir boyut seçin. İhtiyacınız olduğunu düşündüğünüz en büyük VM boyutunu seçmek için bir kullanılabilirlik kümesi oluştururken ve bu durumu kullanılabilirlik kümesine ilk dağıtımınız olması önerilir.

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Kullanılabilirlik kümesine varolan bir Klasik VM ekleyebilir miyim?

Evet. Yeni veya varolan kullanılabilirlik kümesine varolan bir klasik VM ekleyebilirsiniz. Daha fazla bilgi için bkz. [Varolan sanal makineyi kullanılabilirlik kümesine ekle.](/previous-versions/azure/virtual-machines/windows/classic/configure-availability-classic#addmachine)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz.

Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
