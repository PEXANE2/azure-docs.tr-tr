---
title: DHCP oluşturma ve yönetme
description: Bu makalede, Azure VMware çözümünde DHCP 'nin nasıl yönetileceği açıklanmaktadır.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: ba7c3983f55ed729c77d2f24d3ef311a00522148
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90069569"
---
# <a name="how-to-create-and-manage-dhcp-in-azure-vmware-solution"></a>Azure VMWare çözümünde DHCP oluşturma ve yönetme

NSX-T, özel bulutunuz için DHCP 'yi yapılandırma olanağı sağlar. DHCP sunucunuzu barındırmak için NSX-T kullanmayı planlıyorsanız, bkz. [DHCP sunucusu oluşturma](#create-dhcp-server). Aksi takdirde, ağınızda üçüncü taraf dış DHCP sunucunuz varsa ve istekleri bu DHCP sunucusuna geçirmek istiyorsanız, bkz. [DHCP geçiş hizmeti oluşturma](#create-dhcp-relay-service).

## <a name="create-dhcp-server"></a>DHCP sunucusu oluştur

NSX-T üzerinde bir DHCP sunucusu yapılandırmak için aşağıdaki adımları kullanın.

NSX yöneticisinden **ağ** sekmesine gidin ve **IP yönetimi**altında **DHCP** ' yi seçin. **Sunucu Ekle** düğmesini seçin. Ardından sunucu adını ve sunucu IP adresini belirtin. İşiniz bittiğinde **Kaydet**' i seçin.

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="DHCP sunucusu Ekle" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>DHCP sunucusunu katman 1 ağ geçidine bağlayın.

1. **Katman 1 ağ geçitlerini**seçin, ağ geçidini seçin ve **Düzenle** ' yi seçin.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="kullanılacak ağ geçidini seçin" border="true":::

1. **IP ayırma kümesi yok** seçeneğini belirleyerek bir alt ağ ekleyin

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="alt ağ ekleme" border="true":::

1. Sonraki ekranda, **tür** açılan listesinden **DHCP yerel sunucusu** ' nu seçin. **DHCP sunucusu**IÇIN **varsayılan DHCP** ' yi seçin ve **Kaydet**' i seçin.

   :::image type="content" source="./media/manage-dhcp/set-ip-address-management.png" alt-text="DHCP sunucusu seçeneklerini belirleyin" border="true":::

1. **Katman 1 ağ geçidi** penceresinde **Kaydet**' i seçin. Bir sonraki ekranda **değişiklikler**' i görürsünüz, son olarak **düzenlemeleri kapat** ' ı seçin.

### <a name="add-a-network-segment"></a>Ağ kesimi ekleme

DHCP sunucunuzu oluşturduktan sonra bu sunucuya ağ kesimleri eklemeniz gerekir.

1. NSX-T ' de **ağ** sekmesini seçin ve **bağlantı**altında **segmentler** ' i seçin. **Segment Ekle**' yi seçin. Segmenti ve bağlantıyı katman 1 ağ geçidine göre adlandırın. Ardından, yeni bir alt ağ yapılandırmak için **alt ağları ayarla** ' yı seçin. 

   :::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="Yeni bir ağ kesimi Ekle" border="true":::

1. **Alt ağları ayarla** penceresinde, **alt ağ ekle**' yi seçin. Ağ geçidi IP adresini ve DHCP aralığını girin ve **Ekle** ' yi ve ardından **Uygula** ' yı seçin.

   :::image type="content" source="./media/manage-dhcp/add-subnet-segment.png" alt-text="ağ kesimi Ekle" border="true":::

1. Tamamlandığında, ağ kesimi eklemeyi gerçekleştirmek için **Kaydet** ' i seçin.

   :::image type="content" source="./media/manage-dhcp/segments-complete.png" alt-text="segmentler Tamam" border="true":::

## <a name="create-dhcp-relay-service"></a>DHCP geçiş hizmeti oluştur

1. NSX-T penceresinde **ağ** sekmesini seçin ve **IP yönetimi**altında **DHCP**' yi seçin. **Sunucu Ekle**' yi seçin. **Sunucu türü** Için DHCP geçişi ' ni seçin ve geçiş sunucusu için sunucu adını ve IP adresini girin. Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="DHCP geçiş sunucusu oluştur" border="true":::

1. **Bağlantı**altında **Katman 1 ağ geçitleri** ' ni seçin. Katman 1 ağ geçidinde dikey üç nokta simgesini seçin ve **Düzenle**' yi seçin.

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="Katman 1 ağ geçidini Düzenle" border="true":::

1. IP adresi ayırmayı tanımlamak için bir **IP ayırma kümesi** seçin.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="IP adresi ayırmayı Düzenle" border="true":::

1. İletişim kutusunda, **türü**Için **DHCP geçiş sunucusu**' nu seçin. **DHCP geçişi** açılan menüsünde, DHCP geçiş sunucunuzu seçin. İşiniz bittiğinde **Kaydet** ' i seçin

   :::image type="content" source="./media/manage-dhcp/set-ip-address-management-relay.png" alt-text="IP adresi yönetimini ayarla" border="true":::

## <a name="specify-a-dhcp-range-ip-on-segment"></a>Kesimde bir DHCP aralığı IP 'si belirtin

> [!NOTE]
> Bu yapılandırma, DHCP Istemci kesimindeki DHCP geçiş işlevini gerçekleştirmek için gereklidir. 

1. **Bağlantı**altında **segmentler**' i seçin. Dikey üç noktayı seçin ve **Düzenle**' yi seçin. Bunun yerine yeni bir segment eklemek isterseniz, yeni bir segment oluşturmak için **segment Ekle** ' yi seçebilirsiniz.

   :::image type="content" source="./media/manage-dhcp/edit-segments.png" alt-text="ağ alt ağını düzenleme" border="true":::

1. Segment hakkında ayrıntılar ekleyin. Alt **ağlar** altındaki değeri seçin veya alt ağları eklemek veya değiştirmek Için **alt ağları ayarlayın** .

   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="ağ kesimleri" border="true":::

1. Dikey üç noktayı seçin ve **Düzenle**' yi seçin. Yeni bir alt ağ oluşturmanız gerekiyorsa, bir ağ geçidi oluşturmak ve bir DHCP aralığı yapılandırmak için **alt ağ ekle** ' yi seçin. IP havuzu aralığını girin ve **Uygula**' yı seçin ve ardından **Kaydet** ' i seçin.

   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="alt ağları Düzenle" border="true":::

1. Artık kesime bir DHCP sunucusu havuzu atandı.

   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="Segmente atanan DHCP sunucusu havuzu" border="true":::
