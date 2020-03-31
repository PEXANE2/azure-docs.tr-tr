---
title: Azure portalını kullanarak Azure'a özel ana bilgisayarları dağıtma
description: Azure portalını kullanarak özel ana bilgisayarlara VM dağıtın.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure
ms.date: 03/10/2020
ms.author: cynthn
ms.openlocfilehash: 195a19ef881f235ad8e42f23b53da9e667ef88d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79086767"
---
# <a name="deploy-vms-to-dedicated-hosts-using-the-portal"></a>Portalı kullanarak özel ana bilgisayarlara VM dağıtma

Bu makalede, sanal makinelerinizi (VM) barındırmak için [Azure'a adanmış](dedicated-hosts.md) bir ana bilgisayar oluşturma konusunda size rehberlik eder. 

[!INCLUDE [virtual-machines-common-dedicated-hosts-portal](../../../includes/virtual-machines-common-dedicated-hosts-portal.md)]

## <a name="create-a-vm"></a>VM oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. Azure Market kaynaklarının listesi üzerindeki arama kutusunda, Canonical tarafından sağlanan **Ubuntu Server 16.04 LTS** işletim sistemini arayıp seçin ve ardından **Oluştur**’u seçin.
1. Temel **Bilgiler** sekmesinde, **Proje ayrıntıları**altında, doğru aboneliğin seçildiğinden emin olun ve kaynak **grubu**olarak *myDedicatedHostsRG'yi* seçin. 
1. **Örnek ayrıntıları** altında, **Sanal makine adı** için *myVM* yazın ve **Konum** için *Doğu ABD*'yi seçin.
1. **Kullanılabilirlik seçeneklerinde** **Kullanılabilirlik bölgesini**seçin, açılır yerden *1'i* seçin.
1. Boyut için **boyutu değiştir'i**seçin. Mevcut boyutlar listesinde, **Standart E2s v3**gibi Esv3 serisinden birini seçin. Kullanılabilir boyutların tümünün görülmesi için filtreyi temizlemeniz gerekebilir.
1. **Yönetici hesabı** altından **SSH ortak anahtarı**'nı seçin, kullanıcı adınızı yazın, sonra ortak anahtarınızı metin kutusuna yapıştırın. Ortak anahtarınızda varsa baştaki ve sondaki tüm boşlukları kaldırın.

    ![Yönetici hesabı](./media/quick-create-portal/administrator-account.png)

1. **Gelen bağlantı noktası kuralları** > altında**Ortak gelen bağlantı noktaları,** **seçili bağlantı noktalarına izin ver'i** seçin ve ardından açılır bağlantı noktasından **SSH (22)** seçeneğini belirleyin. 
1. Sayfanın üst kısmında Gelişmiş **sekmesini** seçin ve **Ana Bilgisayar** bölümünde, **Host grubu** için *myHostGroup'u* ve **Host**için *myHost'u* seçin. 
    ![Ana bilgisayar grubunu ve ana bilgisayar grubunu seçin](./media/dedicated-hosts-portal/advanced.png)
1. Kalan varsayılan ayarları bırakın, ardından sayfanın alt kısmındaki **Gözden geçir + oluştur** düğmesini seçin.
1. Doğrulamanın geçtiği iletiyi gördüğünüzde **Oluştur'u**seçin.

VM'nizin dağıtılması birkaç dakika sürer.

## <a name="add-an-existing-vm"></a>Varolan bir VM ekleme 

Özel bir ana bilgisayara çıkan bir VM ekleyebilirsiniz, ancak VM'nin önce Stop\Deallocated olması gerekir. Bir VM'yi özel bir ana bilgisayara taşımadan önce, VM yapılandırmasının desteklendirildiğinden emin olun:

- VM boyutu, özel ana bilgisayarla aynı boyutta olmalıdır. Örneğin, özel ana bilgisayarınız DSv3 ise, VM boyutu Standard_D4s_v3 olabilir, ancak Standard_A4_v2 olamaz. 
- VM'nin özel ana bilgisayarla aynı bölgede bulunması gerekir.
- VM yakınlık yerleştirme grubunun bir parçası olamaz. VM'yi özel bir ana bilgisayara taşımadan önce yakınlık yerleşim grubundan çıkarın. Daha fazla bilgi için bkz: [VM'yi yakınlık yerleşim grubundan taşıma](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups#move-an-existing-vm-out-of-a-proximity-placement-group)
- VM kullanılabilirlik kümesinde olamaz.
- VM bir kullanılabilirlik bölgesindeyse, ana bilgisayar grubuyla aynı kullanılabilirlik bölgesi olmalıdır. VM ve ana bilgisayar grubu için kullanılabilirlik bölgesi ayarları eşleşmelidir.

VM'yi [portalı](https://portal.azure.com)kullanarak özel bir ana bilgisayara taşıyın.

1. VM için sayfayı açın.
1. **Durdurmak** için Dur\deallocate VM'yi seçin.
1. Sol menüden **Yapılandırma'yı** seçin.
1. Açılan menülerden bir ana bilgisayar grubu ve bir ana bilgisayar seçin.
1. Bittiğinde, sayfanın üst kısmında **Kaydet'i** seçin.
1. VM ana bilgisayara eklendikten sonra sol menüden **Genel Bakış'ı** seçin.
1. Sayfanın üst kısmında VM'yi yeniden başlatmak için **Başlat'ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi [için, Özel ana bilgisayarlarına](dedicated-hosts.md) genel bakış alabakın.

- Bir bölgede maksimum esneklik için hem bölgeleri hem de fay etki alanlarını kullanan örnek şablon [burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)bulunur.

- [Azure CLI'yi](dedicated-hosts-cli.md)kullanarak özel bir ana bilgisayar da dağıtabilirsiniz.



