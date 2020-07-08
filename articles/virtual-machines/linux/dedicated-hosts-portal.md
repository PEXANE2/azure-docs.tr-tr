---
title: Azure adanmış Konakları Azure portal kullanarak dağıtma
description: Azure portal kullanarak VM 'Leri adanmış ana bilgisayarlara dağıtın.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure
ms.date: 03/10/2020
ms.author: cynthn
ms.openlocfilehash: 195a19ef881f235ad8e42f23b53da9e667ef88d0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79086767"
---
# <a name="deploy-vms-to-dedicated-hosts-using-the-portal"></a>Portalı kullanarak VM 'Leri adanmış konaklara dağıtma

Bu makalede, sanal makinelerinizi (VM 'Ler) barındırmak için Azure [adanmış ana bilgisayar](dedicated-hosts.md) oluşturma konusunda size kılavuzluk eder. 

[!INCLUDE [virtual-machines-common-dedicated-hosts-portal](../../../includes/virtual-machines-common-dedicated-hosts-portal.md)]

## <a name="create-a-vm"></a>VM oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. Azure Market kaynaklarının listesi üzerindeki arama kutusunda, Canonical tarafından sağlanan **Ubuntu Server 16.04 LTS** işletim sistemini arayıp seçin ve ardından **Oluştur**’u seçin.
1. **Temel bilgiler** sekmesinde, **proje ayrıntıları**' nın altında, doğru aboneliğin seçildiğinden emin olun ve ardından **kaynak grubu**olarak, *myayrılmış hostsrg* ' yi seçin. 
1. **Örnek ayrıntıları** altında, **Sanal makine adı** için *myVM* yazın ve **Konum** için *Doğu ABD*'yi seçin.
1. **Kullanılabilirlik seçenekleri** ' nde **kullanılabilirlik alanı**' nı seçin, açılan listeden *1* ' i seçin.
1. Boyut için **boyutu Değiştir**' i seçin. Kullanılabilir boyutlar listesinde, **Standart E2s v3**gibi Esv3 serilerinden birini seçin. Tüm kullanılabilir boyutları görmek için filtreyi temizlemeniz gerekebilir.
1. **Yönetici hesabı** altından **SSH ortak anahtarı**'nı seçin, kullanıcı adınızı yazın, sonra ortak anahtarınızı metin kutusuna yapıştırın. Ortak anahtarınızda varsa baştaki ve sondaki tüm boşlukları kaldırın.

    ![Yönetici hesabı](./media/quick-create-portal/administrator-account.png)

1. **Gelen bağlantı noktası kuralları**  >  **Genel gelen bağlantı noktaları**altında **Seçili bağlantı noktalarına izin ver** ' i seçin ve ardından açılır listeden **SSH (22)** öğesini seçin. 
1. Sayfanın üst kısmında **Gelişmiş** sekmesini seçin ve **konak** bölümünde konak **grubu** için *Myhostgroup* ve **konak**için *myhost* ' u seçin. 
    ![Konak grubu ve konak seçin](./media/dedicated-hosts-portal/advanced.png)
1. Kalan varsayılan ayarları bırakın, ardından sayfanın alt kısmındaki **Gözden geçir + oluştur** düğmesini seçin.
1. Doğrulamanın geçtiğini belirten iletiyi gördüğünüzde **Oluştur**' u seçin.

VM'nizin dağıtılması birkaç dakika sürer.

## <a name="add-an-existing-vm"></a>Var olan bir VM 'yi ekleme 

Bir çıkış VM 'sini ayrılmış bir konağa ekleyebilirsiniz, ancak önce VM 'nin Stop\satıcılarla locatedolması gerekir. Bir VM 'yi adanmış bir konağa taşımadan önce, VM yapılandırmasının desteklendiğinden emin olun:

- VM boyutu, ayrılmış konakla aynı büyüklükte bir aile içinde olmalıdır. Örneğin, adanmış ana bilgisayarınız DSv3 ise sanal makine boyutu Standard_D4s_v3 olabilir, ancak bir Standard_A4_v2 olamaz. 
- VM 'nin adanmış konakla aynı bölgede bulunması gerekir.
- VM, bir yakınlık yerleşimi grubunun parçası olamaz. Ayrılmış bir konağa taşımadan önce VM 'yi yakınlık yerleşimi grubundan kaldırın. Daha fazla bilgi için bkz. [bir VM 'yi bir yakınlık yerleşimi grubundan taşıma](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups#move-an-existing-vm-out-of-a-proximity-placement-group)
- VM bir kullanılabilirlik kümesinde olamaz.
- VM bir kullanılabilirlik bölgeindeyse, konak grubuyla aynı Kullanılabilirlik bölgesi olması gerekir. VM ve konak grubu için kullanılabilirlik bölgesi ayarlarının eşleşmesi gerekir.

[Portalı](https://portal.azure.com)kullanarak VM 'yi adanmış bir konağa taşıyın.

1. VM için sayfayı açın.
1. VM 'yi serbest bırakmak için **Durdur** ' u seçin.
1. Sol menüden **yapılandırma** ' yı seçin.
1. Açılır menülerden bir konak grubu ve konak seçin.
1. İşiniz bittiğinde sayfanın en üstündeki **Kaydet** ' i seçin.
1. VM konağa eklendikten sonra, sol menüden **genel bakış** ' ı seçin.
1. Sanal makineyi yeniden başlatmak için sayfanın üst kısmındaki **Başlat** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için bkz. [adanmış ana bilgisayarlara](dedicated-hosts.md) genel bakış.

- [Burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md), bir bölgedeki maksimum dayanıklılık için hem bölge hem de hata etki alanı kullanan örnek şablon vardır.

- Ayrıca, [Azure CLI](dedicated-hosts-cli.md)kullanarak adanmış bir konak dağıtabilirsiniz.



