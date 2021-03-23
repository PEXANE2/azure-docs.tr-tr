---
title: Tehdit zekası verilerini güncelleştirme
description: Tehdit bilgileri veri paketi, her bir IoT sürümü için yeni bir Defender ile veya yayınlar arasında gerekliyse sağlanır.
ms.date: 12/14/2020
ms.topic: how-to
ms.openlocfilehash: 386d59d33c4f9695b8fc5243dab345321cff4a5f
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104784059"
---
# <a name="threat-intelligence-research-and-packages"></a>Tehdit bilgileri araştırması ve paketleri

Microsoft 'taki güvenlik ekipleri, özel ICS tehdit zekasını ve güvenlik açığı araştırmasını çalıştırır. Bu ekipler, MSTIK (Microsoft Threat Intelligence Center), DART (Microsoft algılama ve yanıt ekibi), DCU (dijital Crimes birimi) ve Bölüm 52 (yalnızca ICS 'ye özgü sıfır gün, ters mühendislik kötü amaçlı yazılım, kampanyalar ve duyuru 'ları izleyen IoT/OT/ICS etki alanı uzmanları) içerir.

Takımlar, Microsoft 'a güvenlik algılama, analiz ve yanıt sağlar:

- Bulut altyapısı ve hizmetleri.
- Geleneksel ürün ve cihazlar.
- Dahili kurumsal kaynaklar.

Güvenlik ekipleri şu avantajları elde edebilir:

- Bilinen ve ilgili tehditlere karşı koruma.
- Önceliklendirme ve önceliklendirmenize yardımcı olan Öngörüler.
- Tehditler etkilenmeden önce tam olarak tüm tehditlerin anlaşılmasıdır.
- Daha ilgili, doğru ve işlem yapılabilir veriler.

Bu zeka, Microsoft Platform analizlerini zenginleştirme ve şirketin yönetilen hizmetlerini olay yanıtı ve ihlal araştırmaya yönelik olarak destekler. Tehdit bilgileri paketleri, imzaları (kötü amaçlı yazılım imzaları dahil), Ckları ve diğer güvenlik içeriğini içerir.

IoT portalındaki Azure Defender 'daki **güncelleştirmeler** sayfasından paketleri indirebilirsiniz.

:::image type="content" source="media/how-to-work-with-threat-intelligence-packages/download-screen.png" alt-text="IoT portalı için Azure Defender aracılığıyla güncelleştirmeleri indirin.":::

## <a name="update-threat-intelligence-data"></a>Tehdit zekası verilerini güncelleştirme

Tehdit zekası paketleri her bir IoT sürümü güncelleştirmesi için yeni bir Defender ile veya yayınlar arasında gerekliyse sağlanır.

IoT portalı için Defender 'dan yüklediğiniz paketler, bireysel sensörlerden el ile yüklenebilir. Şirket içi yönetim konsolu Sensörlerinizi yönetirse, tehdit zekaları paketlerini yönetim konsoluna indirebilir ve aynı anda birden çok sensöre gönderebilirsiniz.

Tek bir sensörde bir paketi güncelleştirmek için:

1. IoT **güncelleştirmeleri** Için Azure Defender sayfasına gidin.

2. **Tehdit zekası** paketini indirin ve kaydedin.

3. Algılayıcı konsolunda oturum açın.

4. Yan menüde **sistem ayarları**' nı seçin.

5. **Tehdit zekası verilerini** seçin ve ardından **Güncelleştir**' i seçin.

6. Yeni paketi karşıya yükleyin.

Aynı anda birden çok sensörde bir paketi güncelleştirmek için:

1. IoT **güncelleştirmeleri** Için Azure Defender sayfasına gidin.

2. **Tehdit zekası** paketini indirin ve kaydedin.

3. Yönetim konsolunda oturum açın.

4. Yan menüde **sistem ayarları**' nı seçin.

5. **Algılayıcı altyapısı yapılandırma** bölümünde, güncelleştirilmiş paketleri alması gereken sensörlerini seçin.  

6. **Tehdit zekası verilerini seçin** bölümünde artı işaretini ( **+** ) seçin.

7. Paketi karşıya yükleyin.

## <a name="see-also"></a>Ayrıca bkz.

[Güncelleştirme sürümleri](how-to-manage-sensors-from-the-on-premises-management-console.md#update-versions)
