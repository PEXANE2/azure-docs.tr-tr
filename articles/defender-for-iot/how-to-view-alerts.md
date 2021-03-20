---
title: Uyarıları görüntüleme
description: Çeşitli kategorilere göre uyarıları görüntüleyin ve ilgilendiğiniz uyarıları bulmanıza yardımcı olması için arama özelliklerini kullanır.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/02/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 6dc2a9683a48f20816adc8ce0ee0c1e8dc57b287
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100523677"
---
# <a name="view-alerts"></a>Uyarıları görüntüleme

Bu makalede, sensöriz tarafından tetiklenen uyarıların nasıl görüntüleneceği ve bunları uyarı araçlarıyla yönetme işlemlerinin nasıl yapılacağı açıklanır.

Arşivlenmiş veya sabitlenmiş uyarılar gibi çeşitli kategorilere göre uyarıları görüntüleyebilirsiniz. Ayrıca, bir IP veya MAC adresine dayalı uyarılar gibi ilgilendiğiniz uyarıları arayabilirsiniz.  

Ayrıca, algılayıcı panosundan uyarıları görüntüleyebilirsiniz.

Uyarıları görüntülemek için:

- Yan menüden **Uyarılar** ' ı seçin. Uyarılar penceresi, sensörlerinizin algıladığı uyarıları görüntüler.

  :::image type="content" source="media/how-to-work-with-alerts-sensor/alerts-screen.png" alt-text="Uyarılar ekranının görünümü.":::

## <a name="view-alerts-by-category"></a>Uyarıları kategoriye göre görüntüle

Uyarıları **Uyarı** ana görünümündeki çeşitli kategorilere göre görüntüleyebilirsiniz. Ayrıntıları gözden geçirmek ve olayı yönetmek için bir uyarı seçin.

| Türe göre sırala | Description |
|--|--|
| **Önemli uyarılar** | Uyarılar önem derecesine göre sıralanır. |
| **Sabitlenmiş uyarılar** | Kullanıcının daha fazla araştırma için sabitlendiğini uyarır. Sabitlenmiş uyarılar arşivlenmez ve sabitlenmiş klasörde 14 gün boyunca depolanır. |
| **Son Uyarılar** | Zamana göre sıralanan uyarılar. |
| **Onaylanan uyarılar** | Onaylanan ve işlenmemiş olan veya kapalı ve kapalı olan uyarılar. |
| **Arşivlenmiş uyarılar** | Sistemin otomatik olarak arşivlendiği uyarılar. Yalnızca Yönetici Kullanıcı bunlara erişebilir. |

## <a name="search-for-alerts-of-interest"></a>İlgilendiğiniz uyarıları arayın

Uyarılar ana görünümü, ilgilendiğiniz uyarıları bulmanıza yardımcı olmak için çeşitli arama özellikleri sağlar.

:::image type="content" source="media/how-to-work-with-alerts-sensor/main-alerts-view.png" alt-text="Uyarılar öğrenme ekran görüntüsü.":::

### <a name="text-search"></a>Metin arama

Uyarıları metin, sayı veya karakter ile aramak için ücretsiz arama seçeneğini kullanın.

Aramak için:

- Ücretsiz arama alanına gerekli metni yazın ve klavyenizde Enter tuşuna basın.

Aramayı temizlemek için:

- Ücretsiz arama alanındaki metni silin ve klavyenizde Enter tuşuna basın.

### <a name="device-group-or-device-ip-address-search"></a>Cihaz grubu veya cihaz IP adresi arama

Belirli IP adreslerine veya cihaz gruplarına başvuran uyarıları arayın. Cihaz grupları cihaz eşlemesinde oluşturulur.

Gelişmiş filtreleri kullanmak için:

1. **Uyarılar** ana görünümünden **Gelişmiş filtreler** ' i seçin.

2. Bir cihaz grubu veya cihaz seçin.

3. **Onayla** seçeneğini belirleyin.

4. Aramayı temizlemek için **Tümünü Temizle**' yi seçin.

### <a name="security-versus-operational-alert-search"></a>Güvenlik ve işlemsel uyarı arama

İşlem ve güvenlik uyarılarını görüntüleme arasında geçiş yap:

- **Güvenlik** uyarıları olası kötü amaçlı yazılım trafiğini, ağ bozukluklerini, ilke ihlallerini ve protokol ihlallerini temsil eder.

- **İşletimsel** uyarılar ağ bozukluileri, ilke ihlalleri ve protokol ihlallerini temsil eder.

Seçeneklerden hiçbiri seçili olmadığında tüm uyarılar görüntülenir.

:::image type="content" source="media/how-to-work-with-alerts-sensor/alerts-security.png" alt-text="Uyarılar ekranında Güvenlik.":::

## <a name="alert-page-options"></a>Uyarı sayfası seçenekleri

Uyarı iletileri aşağıdaki eylemleri sağlar:

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/acknowledge-an-alert-icon.png" border="false":::Uyarı kabul etmek için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/unacknowledge-an-alert-icon.png" border="false":::Bir uyarının kabul kaldırılması için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/pin-an-alert-icon.png" border="false":::Bir uyarıyı sabitlemek için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/unpin-an-alert-icon.png" border="false":::Bir uyarıyı serbest bırakmak için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/acknowledge-all-alerts-icon.png" border="false":::Tüm uyarıları kabul etmek için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/learn-and-acknowledge-all-alerts.png" border="false":::Tüm uyarıları öğrenmek ve onaylamak için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/export-to-csv.png" border="false":::Uyarı bilgilerini bir. csv dosyasına dışarı aktarmak için seçin. Uyarı bilgilerini birden çok cihazı kaplayan her uyarı için ayrı satırlarda dışarı aktarmak için **genişletilmiş uyarı dışarı aktarma** seçeneğini kullanın.

## <a name="alert-pop-up-window-options"></a>Uyarı açılır penceresi seçenekleri

- Bir :::image type="icon" source="media/how-to-work-with-alerts-sensor/export-to-pdf.png" border="false"::: Uyarı raporunu PDF dosyası olarak indirmek için simgeyi seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/download-pcap.png" border="false":::PCAP dosyasını indirmek için simgeyi seçin. Dosya, ücretsiz ağ protokolü Çözümleyicisi olan Wireshark ile görüntülenebilir.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/download-filtered-pcap.png" border="false":::Yalnızca ilgili uyarı paketlerini içeren filtrelenmiş BIR PCAP dosyasını indirmek için seçin, böylece çıkış dosyası boyutunu azaltır ve daha odaklanmış bir analize izin verilir. Dosyayı Wireshark kullanarak görüntüleyebilirsiniz.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/show-alert-in-timeline.png" border="false":::Uyarıyı olay zaman çizelgesinde göstermek için simgeyi seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/pin-an-alert-icon.png" border="false":::Uyarıyı sabitlemek için simgeyi seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/unpin-an-alert-icon.png" border="false":::Uyarının serbest olması için simgeyi seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/learn-icon.png" border="false":::Trafiği onaylamak için seçin (yalnızca Güvenlik analistleri ve Yöneticiler).

- Cihaz eşlemesinde görüntülenecek bir cihaz seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Uyarı olayını yönetme](how-to-manage-the-alert-event.md)

[Uyarı iş akışlarını hızlandırma](how-to-accelerate-alert-incident-response.md)
