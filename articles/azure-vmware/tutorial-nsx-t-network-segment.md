---
title: Öğretici-Azure VMware çözümünde NSX-T ağ kesimi oluşturma
description: VCenter 'da VM 'Ler için kullanılan NSX-T ağ kesimlerini oluşturmayı öğrenin
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: f88f186d2af10bcc114d64920a3ac489ef7be54f
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367678"
---
# <a name="tutorial-create-an-nsx-t-network-segment-in-azure-vmware-solution"></a>Öğretici: Azure VMware çözümünde NSX-T ağ kesimi oluşturma

VCenter 'da oluşturulan sanal makineler (VM) NSX-T içinde oluşturulan ağ kesimlerine yerleştirilir ve vCenter 'da görünür.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağ kesimleri eklemek için NSX-T Yöneticisi 'nde gezin
> * Yeni bir ağ kesimi Ekle
> * VCenter 'daki yeni ağ kesimini gözlemleyin

## <a name="prerequisites"></a>Ön koşullar

VCenter ve NSX-T Manager yönetim arabirimlerine erişimi olan bir Azure VMware çözümü özel buluta sahip olmanız gerekir. Daha fazla bilgi için bkz. [ağ Iletişimini yapılandırma](tutorial-configure-networking.md) öğreticisi.

## <a name="provision-a-network-segment-in-nsx-t"></a>NSX-T içinde bir ağ kesimi sağlama

1. Özel bulutunuz için vCenter 'da, **SDDC-Datacenter > ağları** ' nı seçin ve henüz bir ağ olmadığını unutmayın.

   :::image type="content" source="media/nsxt/vcenter-without-ls01.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. Özel bulutunuz için NSX-T Yöneticisi ' nde **ağ**' ı seçin.

   :::image type="content" source="media/nsxt/nsxt-network-overview.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. **Segmentleri**seçin.

   :::image type="content" source="media/nsxt/nsxt-select-segments.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. NSX-T segmentlerine Genel Bakış sayfasında, **segment Ekle**' yi seçin. Özel bulut sağlama kapsamında üç segment oluşturulur ve VM 'Ler için kullanılamaz.  Bu amaçla yeni bir ağ kesimi eklemeniz gerekir.

   :::image type="content" source="media/nsxt/nsxt-segments-overview.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. Segmenti adlandırın, **bağlı ağ geçidi**olarak önceden yapılandırılmış Katman1 ağ geçidini (tntxx-T1) seçin, **türü** esnek olarak bırakın, önceden yapılandırılmış olan kaplama **taşıma bölgesini** (TNTXX-kaplaması-TZ) seçin ve ardından alt ağları ayarla ' yı seçin. Bu bölümdeki diğer tüm ayarlar ve **bağlantı noktaları** ve **kesim profilleri** varsayılan olarak yapılandırma olarak kalır.

   :::image type="content" source="media/nsxt/nsxt-create-segment-specs.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. Yeni segment için ağ geçidinin IP adresini ayarlayın ve ardından **Ekle**' yi seçin. Kullandığınız IP adresinin çakışmayan bir RFC1918 adres bloğunda olması gerekir. Bu, yeni kesimdeki VM 'lere bağlanabilmenizi sağlar.

   :::image type="content" source="media/nsxt/nsxt-create-segment-gateway.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. **Uygula** ' yı seçerek yeni ağ kesimini uygulayın ve ardından **Kaydet**' i seçerek yapılandırmayı kaydedin.

   :::image type="content" source="media/nsxt/nsxt-create-segment-apply.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

   :::image type="content" source="media/nsxt/nsxt-create-segment-save.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. Yeni ağ kesimi şimdi oluşturulmuştur ve **Hayır**seçeneğini belirleyerek segmenti yapılandırmaya devam etme seçeneğini reddedebilirsiniz.

   :::image type="content" source="media/nsxt/nsxt-create-segment-continue-no.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. **Ağ > segmentleri** seçerek ve yeni segmentin listede olduğunu (Bu durumda "LS01") seçerek NSX-T ' de yeni ağ kesiminin mevcut olduğunu onaylayın.

   :::image type="content" source="media/nsxt/nsxt-new-segment-overview-2.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

1. **Ağ > SDDC-Datacenter** ' ı seçerek ve yeni segmentin listede olduğunu (Bu durumda "LS01") seçerek yeni ağ kesiminin vCenter 'da mevcut olduğunu doğrulayın.

   :::image type="content" source="media/nsxt/vcenter-with-ls01-2.png" alt-text="Özel bulutunuz için vCenter ' da SDDC-Datacenter > ağları ' nı seçin ve henüz ağ olmadığını unutmayın.":::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, vCenter 'daki VM 'Ler için kullanılan NSX-T ağ kesimlerini oluşturdunuz. Artık [Azure VMware çözümünde VM dağıtmak için bir Içerik kitaplığı oluşturabilirsiniz](deploy-vm-content-library.md). Ayrıca, bu öğreticide oluşturduğunuz ağda bir VM sağlayabilirsiniz.

Aksi takdirde, Azure VMware çözümünde özel bir buluta nasıl ExpressRoute Global Reach eşleme oluşturulacağını öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Şirket içi ortamlar için özel bir buluta](tutorial-expressroute-global-reach-private-cloud.md)

<!-- LINKS - external-->

<!-- LINKS - internal -->
