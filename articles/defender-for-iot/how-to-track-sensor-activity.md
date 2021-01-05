---
title: Algılayıcı etkinliğini izle
description: Olay zaman çizelgesi, ağınızda algılanan etkinlikleri, uyarılar ve uyarı Yönetim eylemleri, ağ olayları ve Kullanıcı oturum açma ve Kullanıcı silme gibi kullanıcı işlemleri dahil olmak üzere bir zaman çizelgesi gösterir.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/10/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: 3895e01b1fbfcde79ff91bd1eade8d902c33b852
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97843032"
---
# <a name="track-sensor-activity"></a>Algılayıcı etkinliğini izle

## <a name="event-timeline"></a>Olay zaman çizelgesi

Olay zaman çizelgesi, sensörize algıladığı etkinliğin bir zaman çizelgesini gösterir. Örnek:

  - Uyarılar ve uyarı yönetimi eylemleri

  - Ağ olayları

  - Kullanıcı oturumu açma ve Kullanıcı silme gibi kullanıcı işlemleri

Olay zaman çizelgesi, ağda gerçekleşen olayların kronolojik bir görünümünü sağlar. Olay zaman çizelgesi, daha önce ve sonrasında bir saldırı ve olay zincirinin anlaşılmasına ve analiz etmesine yardımcı olur.

> [!NOTE]
> *Yöneticiler* ve *Güvenlik analistleri* bu bölümde açıklanan yordamları gerçekleştirebilir.

Olay günlüklerini görüntülemek için:

- Yan menüden **olay zaman çizelgesi**' ni seçin.

   :::image type="content" source="media/how-to-track-sensor-activity/event-timeline.png" alt-text="Olay zaman çizelgesinde olaylarınızı görüntüleyin.":::

Algılayıcının algıladığı olayları görüntülemenin yanı sıra, el ile olayları zaman çizelgesine ekleyebilirsiniz. Bu işlem, bir dış sistemde olay meydana gelirse ancak ağınızda bir etkisi varsa yararlıdır ve bu olayı zaman çizelgesinin bir parçası olarak sunmak önemlidir.

Olayları el ile eklemek için:

- **Olay oluştur**' u seçin.

Olay günlüğü bilgilerini bir CSV dosyasına aktarmak için:

- **Export** (Dışarı aktar) öğesini seçin.

## <a name="filter-the-event-timeline"></a>Olay zaman çizelgesini filtrele

Zaman çizelgesini, sizi ilgilendiren cihazları ve olayları görüntüleyecek şekilde filtreleyin.

Zaman çizelgesini filtrelemek için:

1. **Gelişmiş filtreler**' i seçin.

   :::image type="content" source="media/how-to-track-sensor-activity/advance-filters.png" alt-text="Olaylarınızı filtrelemek için olaylar gelişmiş filtreler penceresini kullanın.":::

2. Olay filtrelerini aşağıdaki gibi ayarlayın:

   - **Dahil etme adresi**: belirli olay cihazlarını görüntüleme.

   - **Dışlama adresi**: belirli olay cihazlarını gizleyin.

   - **Olay türlerini dahil et**: belirli olay türlerini görüntüle.

   - **Olay türlerini hariç tut**: belirli olay türlerini gizleyin.

   - Cihaz **grubu**: cihaz eşlemesinde tanımlandığı gibi bir cihaz grubu seçin. Yalnızca bu grubun olayları sunulur.

3. Seçili tüm filtreleri temizlemek için **Tümünü Temizle** ' yi seçin.

4. **Yalnızca uyarılar**, **Uyarılar ve bildirimler** veya **tüm olaylar** için arama yapın.

5. Belirli bir tarih seçmek için **tarihi seç** ' i seçin. Gün, saat ve dakika seçin. Seçilen zaman çerçevesinin olayları gösteriliyor.

6.  Kullanıcı işlemi olaylarını dahil etmek veya hariç tutmak için **Kullanıcı işlemlerini** seçin.

7.  Olayla ilgili daha fazla bilgi görüntülemek için oku (**V**) seçin:

    - Uyarının ayrıntılı açıklamasını göstermek için ilgili uyarıları (varsa) seçin.

    - Haritayı haritada görüntülenecek cihaz seçin.

    - İlgili cihazlara göre filtrelemek istiyorsanız **ilgili cihazlara Olayları Filtrele** ' yi seçin.

    - Tüm ağın belirli bir zamanda bir paket yakalamalarını içeren PCAP dosyasını (varsa) indirmek için **PCAP dosyası** ' nı seçin. 
    
      PCAP dosyası, mühendislerin tam olarak olayın nerede olduğunu ve neler olduğunu belirlemesine yardımcı olabilecek teknik bilgiler içerir. PCAP dosyasını, ücretsiz bir uygulama olan Wireshark gibi bir ağ protokolü Çözümleyicisi ile çözümleyebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

[Uyarıları görüntüleme](how-to-view-alerts.md)
