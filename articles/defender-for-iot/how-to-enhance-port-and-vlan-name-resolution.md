---
title: Bağlantı noktası ve VLAN ad çözümlemesini geliştirme
description: Algılayıcılarınızın bağlantı noktası ve VLAN adlarını, cihaz çözümlemesine zengin bir şekilde özelleştirin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/13/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 9c976671bccb420ae24d8def7a6574098d86ce6d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98803585"
---
# <a name="enhance-port-and-vlan-name-resolution"></a>Bağlantı noktası ve VLAN ad çözümlemesini geliştirme

Algılayıcılarınızın bağlantı noktası ve VLAN adlarını, cihaz çözümlemesine zengin olarak özelleştirebilirsiniz.

## <a name="customize-port-names"></a>Bağlantı noktası adlarını özelleştirme

IoT için Azure Defender, adları DHCP veya HTTP gibi evrensel olarak ayrılmış bağlantı noktalarına otomatik olarak atar. , IoT için Defender tarafından algılanan diğer bağlantı noktaları için bağlantı noktası adlarını özelleştirebilirsiniz. Örneğin, bu bağlantı noktası olağandışı yüksek etkinlik gösterdiği için, ayrılmış olmayan bir bağlantı noktasına bir ad atayın.

Bu adlar şu durumlarda görünür:

  - Cihaz eşlemesinden cihaz **gruplarını** seçersiniz.

  - Bağlantı noktası bilgilerini sağlayan pencere öğeleri oluşturun.

### <a name="view-custom-port-names-in-the-device-map"></a>Cihaz eşlemesinde özel bağlantı noktası adlarını görüntüleme

Kullanıcılar tarafından tanımlanan bir adı içeren bağlantı noktaları, **bilinen uygulamalar** grubunda cihaz eşlemesinde görüntülenir.

:::image type="content" source="media/how-to-enrich-asset-information/applications-v2.png" alt-text="Bilinen uygulamalar grubunu gösteren cihaz eşlemesinin ekran görüntüsü.":::

### <a name="view-custom-port-names-in-widgets"></a>Pencere öğelerinde özel bağlantı noktası adlarını görüntüleme

Tanımladığınız bağlantı noktası adları, trafiği bağlantı noktasına göre kapsayan pencere öğeleri içinde görüntülenir.

:::image type="content" source="media/how-to-enrich-asset-information/traffic-v2.png" alt-text="Trafiği Kapla.":::

Özel bağlantı noktası adlarını tanımlamak için:

1. **Sistem ayarları** ' nı seçin ve **Standart diğer adlar**' ı seçin.

2. **Bağlantı noktası diğer adı Ekle**' yi seçin.

    :::image type="content" source="media/how-to-enrich-asset-information/edit-aliases.png" alt-text="Bağlantı noktası diğer adı ekleyin.":::

3. Bağlantı noktası numarasını girin, **TCP/UDP**' yi seçin veya **her ikisini birden** seçin ve adını ekleyin.

4. **Kaydet**’i seçin.

## <a name="configure-vlan-names"></a>VLAN adlarını yapılandırma

Cihaz envanter verilerini cihaz VLAN numaraları ve etiketleriyle zenginleştirebilirsiniz. Veri zenginleştirme ' ye ek olarak, VLAN başına cihaz sayısını görüntüleyebilir ve VLAN pencere öğeleri ile bant genişliğini görüntüleyebilirsiniz.

VLAN desteği, 802.1 q 'yi temel alır (VLAN KIMLIĞI 4094 ' e kadar).

VLAN bilgilerini almak için iki yöntem mevcuttur:

- **Otomatik olarak bulunan**: algılayıcı, VLAN 'ları otomatik olarak bulur. Trafikle algılanan VLAN 'Lar VLAN yapılandırma ekranında, veri araştırma raporlarında ve VLAN bilgilerini içeren diğer raporlarda görüntülenir. Kullanılmayan VLAN 'Lar gösterilmez. Bu VLAN 'Ları düzenleyemez veya silemezsiniz. 

  Her VLAN 'a benzersiz bir ad eklemeniz gerekir. Ad eklememeniz durumunda VLAN numarası, VLAN 'ın bildirildiği tüm konumlarda görünür.

- **El ile eklenen**: VLAN 'ları el ile ekleyebilirsiniz. El ile eklenen her VLAN için benzersiz bir ad eklemelisiniz ve bu VLAN 'Ları düzenleyebilir veya silebilirsiniz.

VLAN adları en fazla 50 ASCII karakter içerebilir.

> [!NOTE]
> VLAN adları, algılayıcı ve yönetim konsolu arasında eşitlenmez. Adı yönetim konsolunda de tanımlamanız gerekir.  
Cisco anahtarları için, yayılma yapılandırmasına aşağıdaki satırı ekleyin: `monitor session 1 destination interface XX/XX encapsulation dot1q` . Bu komutta, *xx/xx* bağlantı noktasının adı ve numarasıdır.

VLAN 'Ları yapılandırmak için:

1. Yan menüde **sistem ayarları**' nı seçin.

2. **Sistem ayarları** penceresinde **VLAN**' ı seçin.

    :::image type="content" source="media/how-to-enrich-asset-information/edit-vlan.png" alt-text="VLAN 'larınızı düzenlemek için sistem ayarlarını kullanın.":::

3. Her VLAN KIMLIĞININ yanına benzersiz bir ad ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Zenginleştirilmiş cihaz bilgilerini çeşitli raporlarda görüntüleyin:

- [Bir cihaz envanterinde algılayıcı algılamalarını araştırma](how-to-investigate-sensor-detections-in-a-device-inventory.md)
- [Algılayıcı eğilimleri ve istatistik raporları](how-to-create-trends-and-statistics-reports.md)
- [Algılayıcı veri araştırma sorguları](how-to-create-data-mining-queries.md)
