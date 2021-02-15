---
title: Uyarılardaki bilgileri görüntüleme
description: Ayrıntıları gözden geçirmek için Uyarılar penceresinden bir uyarı seçin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/03/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 5877db6b3bc7366f28e679882a2c784e6828b1c1
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100523592"
---
# <a name="view-information-in-alerts"></a>Uyarılardaki bilgileri görüntüleme

Uyarı ayrıntılarını gözden geçirmek için **Uyarılar** penceresinden bir uyarı seçin. Ayrıntılar aşağıdaki bilgileri içerir:

- Uyarı meta verileri

- Trafik, cihazlar ve olay hakkında bilgi

- Cihaz eşlemesindeki bağlı cihazların bağlantıları

- Güvenlik analistleri ve yöneticiler tarafından tanımlanan açıklamalar

- Olayı araştırmak için öneriler

## <a name="alert-metadata"></a>Uyarı meta verileri

Uyarı ayrıntıları aşağıdaki uyarı meta verilerini içerir:

  - Uyarı KIMLIĞI

  - Uyarıyı tetikleyen ilke altyapısı

  - Uyarının tetiklendiği tarih ve saat

:::image type="content" source="media/how-to-work-with-alerts-sensor/internet-connectivity-detection-unauthorized.png" alt-text="Yetkisiz internet bağlantısı algılandı.":::

## <a name="information-about-devices-traffic-and-the-event"></a>Cihazlar, trafik ve olay hakkında bilgi

Uyarı iletisi hakkında bilgi sağlar:

  - Algılanan cihazlar.

  - Cihazlar arasında protokol ve işlev kodu gibi algılanan trafik.

  - Etkinliğin etkilerine ilişkin Öngörüler.

Bu bilgileri, uyarı olayını yönetme konusunda karar verirken kullanabilirsiniz.

## <a name="links-to-connected-devices-in-the-device-map"></a>Cihaz eşlemesindeki bağlı cihazların bağlantıları

Algılanan cihazlara bağlı cihazlar hakkında daha fazla bilgi edinmek için, uyarıdaki bir cihaz görüntüsünü seçip haritadaki bağlı cihazları görüntüleyebilirsiniz.

:::image type="content" source="media/how-to-work-with-alerts-sensor/rcp-operation-failed.png" alt-text="RCP işlemi başarısız oldu.":::

:::image type="content" source="media/how-to-work-with-alerts-sensor/devices-screen-populated.png" alt-text="Cihazların ekran görüntüsü.":::

Eşleme, seçtiğiniz cihaza ve diğer cihazlara bağlı olarak filtreler. Haritada Ayrıca, uyarılarda algılanan cihazların **Hızlı özellikler** iletişim kutusu görüntülenir.

## <a name="comments-defined-by-security-analysts-and-administrators"></a>Güvenlik analistleri ve yöneticiler tarafından tanımlanan açıklamalar 

Uyarılar, önceden tanımlanmış yorumların bir listesini içerebilir. Örneğin, Yorumlar gerçekleştirilecek azaltma eylemleri veya olay hakkında iletişim kurmak için kişilerin adları için yönergeler olabilir.

Bir uyarı olayını yönetirken, uyarı araştırmak için gereken olay durumunu veya adımları en iyi şekilde yansıtan yorumu veya açıklamaları seçebilirsiniz.

Seçili Yorumlar Uyarı iletisine kaydedilir. Açıklamalarla çalışma, bir uyarı olayının araştırılması sırasında bireyler ve takımlar arasındaki iletişimi geliştirir. Sonuç olarak, açıklamalar olay yanıt süresini hızlandırabilir.

Bir yönetici veya güvenlik analistinin açıklamaları önceden tanımlar. Seçilen Yorumlar, iletme kurallarında tanımlanan iş ortağı sistemlerine iletilmez.

Bir uyarıdaki bilgileri gözden geçirdikten sonra, uyarı olayını yönetme konusunda size rehberlik etmek için çeşitli adli adımları gerçekleştirebilirsiniz. Örneğin:

- Son cihaz etkinliğini çözümleyin (veri araştırma raporu). 

- Aynı anda gerçekleşen diğer olayları (olay zaman çizelgesi) çözümleyin. 

- Kapsamlı olay trafiğini (PCAP dosyası) çözümleyin.

## <a name="pcap-files"></a>PCAP dosyaları

Bazı durumlarda, uyarı iletisinden bir PCAP dosyasına erişebilirsiniz. Bu, ilişkili ağ trafiği hakkında daha ayrıntılı bilgi edinmek istiyorsanız yararlı olabilir.

Bir PCAP dosyası indirmek için :::image type="content" source="media/how-to-work-with-alerts-sensor/download-pcap.png" alt-text="İndir simgesini seçin."::: **Uyarı ayrıntıları** iletişim kutusunun sağ üst kısmında.

## <a name="recommendations-for-investigating-an-event"></a>Bir olayı araştırmak için öneriler 

Bir uyarının **öneri** alanı, bir olayı daha iyi anlamanıza yardımcı olabilecek bilgileri görüntüler. Uyarı olayını yönetmeden veya cihaz ya da ağ üzerinde işlem yapmadan önce bu bilgileri gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Uyarı iş akışlarını hızlandırma](how-to-accelerate-alert-incident-response.md)

[Uyarı olayını yönetme](how-to-manage-the-alert-event.md)
