---
title: DHCP oluşturma ve yönetme
description: Bu makalede, Azure VMware çözümünde DHCP 'nin nasıl yönetileceği açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: cb74ed4474cc14081e59142f2f60915fccd47559
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92461065"
---
# <a name="how-to-create-and-manage-dhcp-in-azure-vmware-solution"></a>Azure VMware çözümünde DHCP oluşturma ve yönetme

NSX-T, özel bulutunuz için DHCP 'yi yapılandırma olanağı sağlar. DHCP sunucunuzu barındırmak için NSX-T kullanırsanız, bkz. [DHCP sunucusu oluşturma](#create-dhcp-server). Aksi takdirde, ağınızda bir üçüncü taraf dış DHCP sunucusuna sahipseniz bkz. [DHCP geçiş hizmeti oluşturma](#create-dhcp-relay-service).

## <a name="create-dhcp-server"></a>DHCP sunucusu oluştur

NSX-T üzerinde bir DHCP sunucusu yapılandırmak için aşağıdaki adımları kullanın.

1. NSX Yöneticisi ' nde **ağ** sekmesine gidin ve **DHCP**' yi seçin. 
1. **Sunucu Ekle** ' yi seçin ve ardından sunucu adını ve IP adresini belirtin. 
1. **Kaydet**’i seçin.

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="DHCP sunucusu Ekle" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>DHCP sunucusunu katman 1 ağ geçidine bağlayın.

1. **Katman 1 ağ geçitlerini**, listeden ağ geçidini seçin ve ardından **Düzenle**' yi seçin.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="DHCP sunucusu Ekle" border="true":::

1. Bir alt ağ eklemek için bir **IP ayırma kümesi** seçin.

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="DHCP sunucusu Ekle" border="true":::

1. **Tür**Için **DHCP yerel sunucusu** ' nu seçin. 
1. **DHCP sunucusu** IÇIN **varsayılan DHCP** ' yi seçin ve ardından **Kaydet**' i seçin.


1. **Katman 1 ağ geçidi** penceresinde **Kaydet**' i seçin. 
1. Son olarak **düzenlemeleri kapat** ' ı seçin.

### <a name="add-a-network-segment"></a>Ağ kesimi ekleme

DHCP sunucunuzu oluşturduktan sonra bu sunucuya ağ kesimleri eklemeniz gerekir.

1. NSX-T ' de **ağ** sekmesini seçin ve **bağlantı**altında **segmentler** ' i seçin. 
1. **Segment Ekle** ' yi seçin ve segmenti ve Katman 1 ağ geçidine bağlantıyı adlandırın. 
1. Yeni bir alt ağ yapılandırmak için **alt ağları ayarla** ' yı seçin. 

   :::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="DHCP sunucusu Ekle" border="true":::

1. **Alt ağları ayarla** penceresinde, **alt ağ ekle**' yi seçin. 
1. Ağ geçidi IP adresini ve DHCP aralığını girin ve **Ekle** ' yi ve ardından **Uygula** ' yı seçin.

1. Yeni ağ kesimini eklemek için **Kaydet** ' i seçin.

## <a name="create-dhcp-relay-service"></a>DHCP geçiş hizmeti oluştur

1. **Ağ** sekmesini seçin ve **IP yönetimi**altında **DHCP**' yi seçin. 
1. **Sunucu Ekle**' yi seçin. 
1. **Sunucu türü** Için DHCP geçişi ' ni seçin ve geçiş sunucusu için sunucu adını ve IP adresini girin. 
1. **Kaydet**’i seçin.

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="DHCP sunucusu Ekle" border="true":::

1. **Bağlantı**altında **Katman 1 ağ geçitleri** ' ni seçin. 
1. Katman 1 ağ geçidinde dikey üç nokta simgesini seçin ve **Düzenle**' yi seçin.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="DHCP sunucusu Ekle" border="true":::

1. IP adresi ayırmayı tanımlamak için bir **IP ayırma kümesi** seçin.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="DHCP sunucusu Ekle" border="true":::

1. **Tür**Için **DHCP geçiş sunucusu** ' nu seçin.
1. **DHCP geçişi IÇIN DHCP**geçiş sunucunuzu seçin. 
1. **Kaydet**’i seçin.


## <a name="specify-a-dhcp-range-ip-on-a-segment"></a>Kesimde bir DHCP aralığı IP 'si belirtme

> [!NOTE]
> Bu yapılandırma, DHCP Istemci kesimindeki DHCP geçiş işlevini gerçekleştirmek için gereklidir. 

1. **Bağlantı**altında **segmentler**' i seçin. 
1. Dikey üç noktayı seçin ve **Düzenle**' yi seçin. 

   >[!TIP]
   >Yeni bir segment eklemek istiyorsanız, **segment Ekle**' yi seçin.

1. Segment hakkında ayrıntılar ekleyin. 
1. Alt ağı eklemek veya değiştirmek için **alt ağları ayarla** altındaki değeri seçin.

   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="DHCP sunucusu Ekle" border="true":::

1. Dikey üç noktayı seçin ve **Düzenle**' yi seçin. Yeni bir alt ağ oluşturmanız gerekiyorsa, bir ağ geçidi oluşturmak ve bir DHCP aralığı yapılandırmak için **alt ağ ekle** ' yi seçin. 
1. IP havuzu aralığını girin ve **Uygula**' yı seçin ve ardından **Kaydet** ' i seçin.

   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="DHCP sunucusu Ekle" border="true":::

   Bir DHCP sunucusu havuzu, segmente atanır.

   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="DHCP sunucusu Ekle" border="true":::
