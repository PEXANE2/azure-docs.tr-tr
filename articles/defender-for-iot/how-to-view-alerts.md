---
title: Uyarıları görüntüleme
description: Çeşitli kategorilere göre uyarıları görüntüleyin ve ilgilendiğiniz uyarıları bulmanıza yardımcı olması için arama özelliklerini kullanın.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/02/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 4803691a82a97cd2be5fa3beafd4419010e7a9c9
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97842982"
---
# <a name="view-alerts"></a>Uyarıları görüntüleme

Bu makalede, sensöriz tarafından tetiklenen uyarıların nasıl görüntüleneceği ve bunları uyarı araçlarıyla yönetme işlemlerinin nasıl yapılacağı açıklanır.

Arşivlenmiş veya sabitlenmiş uyarılar gibi çeşitli kategorilere göre uyarıları görüntüleyebilirsiniz. Ya da bir IP veya MAC adresine dayalı uyarılar gibi ilgi uyarılarını arayabilirsiniz.  

Ayrıca, algılayıcı panosundan uyarıları görüntüleyebilirsiniz.

Uyarıları görüntülemek için:

- Yan menüden **Uyarılar** ' ı seçin. **Uyarılar** penceresi, sensörlerinizin algıladığı uyarıları görüntüler.

  :::image type="content" source="media/how-to-work-with-alerts-sensor/alerts-screen.png" alt-text="Uyarılar ekranının görünümü.":::

## <a name="view-alerts-by-category"></a>Uyarıları kategoriye göre görüntüle

Uyarıları **Uyarı** ana görünümündeki çeşitli kategorilere göre görüntüleyebilirsiniz. Ayrıntıları gözden geçirmek ve olayı yönetmek için bir uyarı seçin.

| Türe göre sırala | Açıklama |
|--|--|
| **Önemli uyarılar** | Uyarılar önem derecesine göre sıralanır. |
| **Sabitlenmiş uyarılar** | Kullanıcının daha fazla araştırma için sabitlendiğini uyarır. Sabitlenmiş uyarılar arşivlenmez ve sabitlenmiş klasörde 14 gün boyunca depolanır. |
| **Son Uyarılar** | Zamana göre sıralanan uyarılar. |
| **Onaylanan uyarılar** | Onaylanan ve işlenmemiş olan veya kapalı ve kapalı olan uyarılar. |
| **Arşivlenmiş uyarılar** | Sistemin otomatik olarak arşivlendiği uyarılar. Yalnızca Yönetici Kullanıcı bunlara erişebilir. |

## <a name="search-for-alerts-of-interest"></a>İlgilendiğiniz uyarıları arayın

**Uyarılar** ana görünümü, ilgilendiğiniz uyarıları bulmanıza yardımcı olmak için çeşitli arama özellikleri sağlar.

:::image type="content" source="media/how-to-work-with-alerts-sensor/main-alerts-view.png" alt-text="Uyarılar öğrenme ekran görüntüsü.":::

### <a name="text-search"></a>Metin arama 

Uyarıları metin, sayı veya karakter ile aramak için **ücretsiz arama** seçeneğini kullanın.

Aramak için:

- **Ücretsiz arama** alanına gerekli metni yazın ve klavyenizde Enter tuşuna basın.

Aramayı temizlemek için:

- **Ücretsiz arama** alanındaki metni silin ve klavyenizde Enter tuşuna basın.

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

## <a name="alert-window-options"></a>Uyarı penceresi seçenekleri

Uyarı iletileri aşağıdaki eylemleri sağlar:

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/acknowledge-an-alert-icon.png" border="false":::Uyarı kabul etmek için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/unacknowledge-an-alert-icon.png" border="false":::Bir uyarının kabul kaldırılması için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/pin-an-alert-icon.png" border="false":::Bir uyarıyı sabitlemek için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/unpin-an-alert-icon.png" border="false":::Bir uyarıyı serbest bırakmak için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/acknowledge-all-alerts-icon.png" border="false":::Tüm uyarıları kabul etmek için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/learn-and-acknowledge-all-alerts.png" border="false":::Tüm uyarıları öğrenmek ve onaylamak için seçin.

- :::image type="icon" source="media/how-to-work-with-alerts-sensor/export-to-csv.png" border="false":::Uyarı listesini BIR CSV dosyasına aktarmak ve dışarı aktar seçeneğini belirlemek için seçin. Normal dışa aktarma-CSV seçeneği için **Uyarı dışarı aktarma** ' yı seçin. Ya da CSV dosyasındaki bir uyarıyla ilgili ek bilgiler için ayrı satırlar ekleme olasılığa yönelik **genişletilmiş uyarı dışarı aktarma** ' yı seçin.

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
