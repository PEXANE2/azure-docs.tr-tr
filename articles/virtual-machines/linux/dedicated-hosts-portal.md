---
title: Azure adanmış Konakları Azure portal kullanarak dağıtma | Microsoft Docs
description: Azure portal kullanarak VM 'Leri adanmış ana bilgisayarlara dağıtın.
services: virtual-machines-linux
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/25/2019
ms.author: cynthn
ms.openlocfilehash: 03d8b5353a2dc9840c8231978243e7f94a5bff56
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700398"
---
# <a name="preview-deploy-vms-to-dedicated-hosts-using-the-portal"></a>Önizleme: Portalı kullanarak VM 'Leri adanmış konaklara dağıtma

Bu makalede, sanal makinelerinizi (VM 'Ler) barındırmak için Azure [adanmış ana bilgisayar](dedicated-hosts.md) oluşturma konusunda size kılavuzluk eder. 

[!INCLUDE [virtual-machines-common-dedicated-hosts-portal](../../../includes/virtual-machines-common-dedicated-hosts-portal.md)]

## <a name="create-a-vm"></a>VM oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. Azure Market kaynaklarının listesi üzerindeki arama kutusunda, Canonical tarafından sağlanan **Ubuntu Server 16.04 LTS** işletim sistemini arayıp seçin ve ardından **Oluştur**’u seçin.
1. **Temel bilgiler** sekmesinde, **proje ayrıntıları**' nın altında, doğru aboneliğin seçildiğinden emin olun ve ardından **kaynak grubu**olarak, *myayrılmış hostsrg* ' yi seçin. 
1. **Örnek ayrıntıları** altında, **Sanal makine adı** için *myVM* yazın ve **Konum** için *East US*'yi seçin.
1. **Kullanılabilirlik seçenekleri** ' nde **kullanılabilirlik alanı**' nı seçin, açılan listeden *1* ' i seçin.
1. Boyut için **boyutu Değiştir**' i seçin. Kullanılabilir boyutlar listesinde, **Standart E2s v3**gibi Esv3 serilerinden birini seçin. Tüm kullanılabilir boyutları görmek için filtreyi temizlemeniz gerekebilir.
1. **Yönetici hesabı** altından **SSH ortak anahtarı**'nı seçin, kullanıcı adınızı yazın, sonra ortak anahtarınızı metin kutusuna yapıştırın. Ortak anahtarınızda varsa baştaki ve sondaki tüm boşlukları kaldırın.

    ![Yönetici hesabı](./media/quick-create-portal/administrator-account.png)

1. **Gelen bağlantı noktası kuralları** > **Genel gelen bağlantı noktaları**altında **Seçili bağlantı noktalarına izin ver** ' i seçin ve ardından açılır listeden **SSH (22)** öğesini seçin. 
1. Sayfanın üst kısmında **Gelişmiş** sekmesini seçin ve **konak** bölümünde konak **grubu** için *Myhostgroup* ve **konak**için *myhost* ' u seçin. 
    ![Konak grubu ve konak seçin](./media/dedicated-hosts-portal/advanced.png)
1. Kalan varsayılan ayarları bırakın, ardından sayfanın alt kısmındaki **Gözden geçir + oluştur** düğmesini seçin.
1. Doğrulamanın geçtiğini belirten iletiyi gördüğünüzde **Oluştur**' u seçin.

VM'nizin dağıtılması birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için bkz. [adanmış ana bilgisayarlara](dedicated-hosts.md) genel bakış.

- [Burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md), bir bölgedeki maksimum dayanıklılık için hem bölge hem de hata etki alanı kullanan örnek şablon vardır.

- Ayrıca, [Azure CLI](dedicated-hosts-cli.md)kullanarak adanmış bir konak dağıtabilirsiniz.



