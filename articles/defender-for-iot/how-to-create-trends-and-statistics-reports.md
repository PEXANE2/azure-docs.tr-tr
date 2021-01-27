---
title: Eğilimler ve istatistik raporları oluşturma
description: IoT eğilimleri ve Istatistik pencere öğeleri için Defender 'ı kullanarak ağ etkinliği, istatistikler ve eğilimler hakkında öngörüler elde edin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 01/24/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 8bfbacc77843c7978bcb1d364bd93f5e09381144
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811730"
---
# <a name="sensor-trends-and-statistics-reports"></a>Algılayıcı eğilimleri ve istatistik raporları

## <a name="about-sensor-trends-and-statistics-reports"></a>Algılayıcı eğilimleri ve istatistik raporları hakkında

Ağ eğilimleri ve istatistikleriyle ilgili bilgi edinmek için pencere öğesi grafikleri ve pasta grafikleri oluşturabilirsiniz. Pencere öğeleri, Kullanıcı tanımlı panolar altında gruplandırılabilir.

> [!NOTE]
> Yönetici ve Güvenlik analistleri, eğilimler ve Istatistik raporları oluşturabilir.

Pano, aşağıdaki bilgi türlerini grafiksel olarak tanımlayan pencere öğeleri içerir:

- Bağlantı noktasına göre trafik
- Kanal bant genişliği
- Toplam bant genişliği
- Etkin TCP bağlantısı
- Cihazlarınız
  - Yeni cihazlar
  - Meşgul cihazlar
  - Satıcıya göre cihazlar
  - İşletim sistemine göre cihazlar
  - Bağlantısı kesilen cihazlar
- Saate göre bağlantı bırakma
- Türe göre olay uyarıları
- Veritabanı tablosu erişimi
- Protokol açıklaması pencere öğeleri
- Ethernet ve IP adresi:
  - CıP hizmetine göre Ethernet ve IP adresi trafiği
  - CıP sınıfına göre Ethernet ve IP adresi trafiği
  - Komuta göre Ethernet ve IP adresi trafiği
- OPC
  - OPC üst yönetim işlemleri
  - OPC Top g/ç işlemleri
- Siemens S7:
  - Denetim işlevine göre S7 trafiği
  - Alt işleve göre S7 trafiği
- SRTP:
  - Hizmet koduna göre SRTP trafiği
  - Güne göre SRTP hataları
- SuiteLink:
  - SuiteLink üst sorgulanan Etiketler
  - SuiteLink sayısal etiket davranışı
- IEC-ASDU 'e göre 60870 trafiği
- İşleve göre DNP3 trafiği
- Hizmete göre MMS trafiği
- İşleve göre Modbus trafiği
- Hizmete göre OPC-UA trafiği

> [!NOTE]
>  Pencere öğelerinin zaman algılayıcısı, algılayıcı zamanına göre ayarlanır.

## <a name="create-reports"></a>Rapor oluşturma

Panoları ve pencere öğelerini görmek için:

Yan menüdeki **eğilim & istatistikleri** ' ni seçin.

:::image type="content" source="media/how-to-generate-reports/investigation-screenshot.png" alt-text="Bir araştırmanın ekran görüntüsü.":::

Varsayılan olarak, sonuçlar son 7 gün içinde algılamalar için görüntülenir. Bu aralığı değiştirmek için filtre araçlarını kullanabilirsiniz. Örneğin, ücretsiz metin arama.

## <a name="see-also"></a>Ayrıca bkz.

[Risk değerlendirmesi raporlaması](how-to-create-risk-assessment-reports.md) 
 [Algılayıcı veri araştırma sorguları](how-to-create-data-mining-queries.md) 
 [Saldırı vektörü raporlaması](how-to-create-attack-vector-reports.md)