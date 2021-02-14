---
title: NSX-T ağ kesimi ekleme
description: Azure VMware çözümü için NSX-T ağ kesimi ekleme adımları.
ms.topic: include
ms.date: 11/09/2020
ms.openlocfilehash: 7db45650588d37c39e7d156fa189b3ff7da2239f
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100514971"
---
<!-- Used in manage-dhcp.md and tutorial-nsx-t-network-segment.md -->

1. NSX-T Yöneticisi 'nde **ağ**  >  **kesimleri**' ni seçin ve **segment Ekle**' yi seçin. 

   :::image type="content" source="../media/nsxt/nsxt-segments-overview.png" alt-text="Yeni bir segmentin nasıl ekleneceğini gösteren ekran görüntüsü":::

1. **Segment Ekle** ' yi seçin ve segment için bir ad girin.

1. **Bağlı ağ geçidi** olarak katman 1 ağ geçidini (TNTxx-T1) seçin ve **türü** esnek olarak bırakın.

1. Önceden yapılandırılmış yer paylaşımı **taşıma bölgesini** (tntxx-KAPLAMASı-TZ) seçin ve ardından **alt ağları ayarla**' yı seçin. 

   :::image type="content" source="../media/nsxt/nsxt-create-segment-specs.png" alt-text="Segment adı, bağlı ağ geçidi ve tür ve taşıma bölgesini ayarlayın, sonra alt ağ ayarla ' yı seçin.":::

1. Ağ geçidinin IP adresini girin ve ardından **Ekle**' yi seçin. 

   >[!IMPORTANT]
   >IP adresinin çakışmayan bir RFC1918 Address bloğunda olması gerekir ve bu, yeni kesimdeki VM 'Lerle bağlantı sağlar.

   :::image type="content" source="../media/nsxt/nsxt-create-segment-gateway.png" alt-text="Yeni segment için ağ geçidinin IP adresini ayarlayın ve ardından Ekle ' yi seçin.":::

1. **Uygula** ' yı ve ardından **Kaydet**' i seçin.

1. Segmenti yapılandırmaya devam etme seçeneğini reddetmek için **Hayır** ' ı seçin. 

   :::image type="content" source="../media/nsxt/nsxt-create-segment-continue-no.png" alt-text="Yeni oluşturulan ağ segmentini, Hayır seçeneğini belirleyerek daha fazla yapılandırmaya reddedin.":::

1. Yeni ağ kesiminin varlığını onaylayın. Bu örnekte, **LS01** yeni ağ kesimdir.

   1. NSX-T Yöneticisi 'nde **ağ**  >  **kesimleri**' ni seçin. 

      :::image type="content" source="../media/nsxt/nsxt-new-segment-overview-2.png" alt-text="Yeni ağ kesiminin NSX-T ' de mevcut olduğunu doğrulayın.":::

   1. VCenter 'da, **ağ > SDDC-Datacenter**' ı seçin.

      :::image type="content" source="../media/nsxt/vcenter-with-ls01-2.png" alt-text="Yeni ağ segmentinin vCenter 'da bulunduğunu onaylayın.":::