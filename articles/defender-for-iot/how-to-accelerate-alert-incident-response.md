---
title: Uyarı iş akışlarını hızlandırma
description: Uyarı ve olay iş akışlarını geliştirme.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/02/2020
ms.service: azure
ms.topic: how-to
ms.openlocfilehash: 14d7a0de1cd29b8c07f90c759a4d423d7186fdb9
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97841731"
---
# <a name="accelerate-alert-workflows"></a>Uyarı iş akışlarını hızlandırma

Bu makalede, IoT için Azure Defender 'daki uyarı açıklamalarını, uyarı gruplarını ve özel uyarı kurallarını kullanarak uyarı iş akışlarının nasıl hızlandırılacağını açıklanmaktadır.  Bu araçlar size yardımcı olur:

- Ağınızda algılanan büyük hacimde uyarı olaylarını çözümleyin ve yönetin.

- Belirli ağ etkinliğini sabitleme ve işleme.

## <a name="accelerate-incident-workflows-by-using-alert-comments"></a>Uyarı açıklamalarını kullanarak olay iş akışlarını hızlandırma

Bir uyarı olayının araştırılması sırasında bireyler ve takımlar arasındaki iletişimi geliştirmek için uyarı açıklamalarıyla çalışın.

:::image type="content" source="media/how-to-work-with-alerts-sensor/suspicion-of malicious-activity-screen.png" alt-text="Kötü amaçlı etkinliği gösteren ekran görüntüsü.":::

Bunları geliştirmek için uyarı açıklamalarını kullanın:

- **Iş akışı adımları**: uyarı azaltma adımları sağlayın.

- **Iş akışı izleme**: adımların alındığını bildirin.

- **Iş akışı Kılavuzu**: olay hakkında öneriler, Öngörüler veya Uyarılar sağlayın.

:::image type="content" source="media/how-to-work-with-alerts-sensor/alert-comment-screen.png" alt-text="Uyarı açıklamalarını gösteren ekran görüntüsü.":::

Kullanılabilir seçeneklerin listesi her bir uyarıda görünür. Kullanıcılar bir veya birkaç ileti seçebilir.

Uyarı açıklamaları eklemek için:

1. Yan menüde **sistem ayarları**' nı seçin.

2. **Sistem ayarı** penceresinde **Uyarı açıklamaları**' nı seçin.

3. Yorum **Ekle** kutusuna açıklama metnini girin. En fazla 50 karakter kullanın. Virgüller izin verilmez.

4. **Ekle**’yi seçin.

## <a name="accelerate-incident-workflows-by-using-alert-groups"></a>Uyarı gruplarını kullanarak olay iş akışlarını hızlandırma

Uyarı grupları, SOC ekiplerin SıEM çözümlerinde uyarıları görüntülemesine ve filtrelemesine olanak tanır ve ardından bu uyarıları kurumsal güvenlik ilkelerine ve iş önceliklerine göre yönetebilir. Örneğin, yeni algılamalar hakkındaki uyarılar bir bulma grubunda düzenlenir. Bu grup, yeni cihazları, yeni VLAN 'Ları, Yeni Kullanıcı hesaplarını, yeni MAC adreslerini ve daha fazlasını algılamayla ilgili uyarıları içerir.

Uyarı grupları, aşağıdaki iş ortağı çözümleri için iletme kuralları oluşturduğunuzda uygulanır:

  - Syslog sunucuları

  - QRadar

  - ArcSight

:::image type="content" source="media/how-to-work-with-alerts-sensor/create-forwarding-rule.png" alt-text="İletme kuralı oluşturma ekran görüntüsü.":::

İlgili uyarı grubu, iş ortağı çıkış çözümlerinde görüntülenir. 

### <a name="requirements"></a>Gereksinimler

Uyarı grubu, desteklenen iş ortağı çözümlerinde aşağıdaki öneklerle görüntülenir:

  - QRadar, Arcgörüş, syslog CEF, syslog LEEF için **kedi**

  - Syslog metin iletileri için **Uyarı grubu**

  - Syslog nesneleri için **alert_group**

Bu alanlar, uyarı grubu adını göstermek için iş ortağı çözümünde yapılandırılmalıdır. Bir uyarı grubuyla ilişkili bir uyarı yoksa, iş ortağı çözümündeki alanı **na** görüntülenir.

### <a name="default-alert-groups"></a>Varsayılan uyarı grupları

Aşağıdaki uyarı grupları otomatik olarak tanımlanmıştır:
|  |  |  |
|--|--|--|
| Olağan dışı iletişim davranışı | Özel uyarılar | Uzaktan erişim |
| Olağan dışı HTTP iletişim davranışı | Bulma | Yeniden başlatma ve durdurma komutları |
| Kimlik doğrulaması | Üretici yazılımı değişikliği | Tarama |
| Yetkisiz iletişim davranışı | Geçersiz komutlar | Algılayıcı trafiği |
| Bant genişliği bozukluklar | İnternet erişimi | Kötü amaçlı yazılımın şüphesi |
| Arabellek taşması | İşlem sorunları | Kötü amaçlı şüphesi etkinliği |
| Komut sorunları | İşletim sorunları |  |
| Yapılandırma değişiklikleri | Program |  |

Uyarı grupları önceden tanımlanmıştır. Uyarı gruplarıyla ilişkili uyarılar ve özel uyarı grupları oluşturma hakkında daha fazla bilgi için [Microsoft desteği](https://support.microsoft.com/supportforbusiness/productselection?sapId=82c88f35-1b8e-f274-ec11-c6efdd6dd099)başvurun.

## <a name="customize-alert-rules"></a>Uyarı kurallarını özelleştirme

Tek tek sensörların algılamadığı bilgileri temel alarak özel uyarı kuralları ekleyebilirsiniz. Örneğin, bir algılayıcı kaynak IP, hedef IP veya komuta (bir protokol içinde) göre bir uyarı tetiklemesine yönlendiren bir kural tanımlayın. Algılayıcı kuralda tanımlanan trafiği algıladığında bir uyarı veya olay oluşturulur.

Uyarı iletisi Kullanıcı tanımlı bir kuralın uyarıyı tetiklediğini belirtir.

:::image type="content" source="media/how-to-work-with-alerts-sensor/customized-alerts-screen.png" alt-text="Özelleştirilmiş uyarıları gösteren ekran görüntüsü.":::

Özel bir uyarı kuralı oluşturmak için:

1. Bir sensörin yan menüsünde **özel uyarılar** ' ı seçin.
1. **+** Bir kural oluşturmak için artı işaretini () seçin.

   :::image type="content" source="media/how-to-work-with-alerts-sensor/user-defined-rule.png" alt-text="Kullanıcı tanımlı bir kural gösteren ekran görüntüsü.":::

1. Bir kural adı tanımlayın.
1. **Kategoriler** bölmesinden bir kategori veya protokol seçin.
1. Belirli bir kaynak ve hedef IP veya MAC adresi tanımlayın veya herhangi bir adres seçin.
1. Koşul ekleyin. Koşulların ve özelliklerinin bir listesi her kategori için benzersizdir. Her uyarı için birden fazla koşul seçebilirsiniz.
1. Kuralın bir **alarm** veya **olay** tetikleyip tetiklemeyeceğini belirtir.
1. Uyarıya önem düzeyi atayın.
1. Uyarının PCAP dosyası dahil edileceğini belirtin.
1. **Kaydet**’i seçin.

Kural, temel kural parametrelerini, kuralın en son tetiklendiği zamanı ve daha fazlasını gözden geçirebileceğiniz **özelleştirilmiş uyarı kuralları** listesine eklenir. Ayrıca, kuralı listeden etkinleştirebilir ve devre dışı bırakabilirsiniz.

:::image type="content" source="media/how-to-work-with-alerts-sensor/customized-alerts-screen.png" alt-text="Kullanıcı tarafından eklenen özelleştirilmiş kuralın ekran görüntüsü.":::

### <a name="see-also"></a>Ayrıca bkz.

[Uyarılarda belirtilen bilgileri görüntüle](how-to-view-information-provided-in-alerts.md)

[Uyarı olayını yönetme](how-to-manage-the-alert-event.md)
