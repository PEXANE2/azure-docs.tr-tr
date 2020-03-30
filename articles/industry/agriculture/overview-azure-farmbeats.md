---
title: Azure FarmBeats nedir
description: Azure FarmBeats'e genel bakış sağlar
author: uhabiba04
ms.topic: overview
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: b067b18985905b226287f9dd10ad4b937fab6df1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76767964"
---
# <a name="overview-of-azure-farmbeats-preview"></a>Azure FarmBeats'e Genel Bakış (Önizleme)

Azure FarmBeats, Azure Marketi'nde kullanılabilen bir işletmeden işletmeye bir tekliftir. Tarım veri kümelerinin sağlayıcılar arasında toplanmasını sağlar. Azure FarmBeats, erimiş veri kümelerini temel alan yapay zeka (AI) veya makine öğrenimi (ML) modelleri oluşturmanıza olanak tanır. Azure FarmBeats'i kullanarak, tarım işletmeleri veri mühendisliğinin farklılaşmamış ağır kaldırma sıyerine temel katma değer ekibe odaklanabilir.

> [!NOTE]
> Azure FarmBeats şu anda genel önizlemede. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. Azure FarmBeats hizmet düzeyi sözleşmesi olmadan sağlanır. Destek için [Azure FarmBeats Forumu'nu](https://aka.ms/FarmBeatsMSDN ) kullanın.

![Proje Çiftlik Beats](./media/architecture-for-farmbeats/farmbeats-architecture-1.png)

Azure FarmBeats önizlemesi ile şunları yapabilirsiniz:

- Uydu görüntülerine göre bitki örtüsü indeksi ve su indeksinkullanarak çiftlik sağlığını değerlendirin.
- Kaç tane toprak nem sensörü kullanacağıve nereye yerleştirebileceğiniz konusunda öneriler alın.
- Çeşitli satıcılardan sensörler tarafından toplanan zemin verilerini görselleştirerek çiftlik koşullarını izleyin.
- Uydu ve sensör verilerinin birleştirilmesine dayalı toprak nem haritası alın.
- AI/ML modellerini toplanan veri kümelerinin üzerine oluşturarak işlem uygulanabilir öngörüler elde edin.
- Çiftlik sağlığı danışmanlığı sağlayarak dijital tarım çözümünüzü oluşturun veya artırın.

## <a name="datahub"></a>Datahub

Azure FarmBeats Datahub, sağlayıcılar arasında çeşitli tarım veri kümelerinin biraraya getirilmesini, normalleştirilmesini ve bağlamsallaştırılmasını sağlayan bir API katmanıdır. Şunları elde etmek için Azure FarmBeats'i kullanabilirsiniz:
- İki sensör [sağlayıcısıdavis Instruments,](https://www.davisinstruments.com/product/enviromonitor-gateway/) [Teralitik,](https://teralytic.com/) [Pessl Instruments'dan](https://metos.at/) gelen sensör **verileri**
- Avrupa Uzay Ajansı'nın [Sentinel-2](https://sentinel.esa.int/web/sentinel/home) uydu görevinden **uydu görüntüleri**
- Üç drone görüntü sağlayıcıları [senseFly,](https://www.sensefly.com/) [SlantRange,](https://slantrange.com/) [DJI](https://dji.com/) **gelen Drone görüntüleri**

Datahub genişletilebilir bir API platformu olarak tasarlanmıştır. Azure FarmBeats ile entegre olmak için çok daha fazla sağlayıcıyla birlikte çalışıyoruz, böylece çözümünüzü yaparken daha fazla seçeneğiniz var.

## <a name="accelerator"></a>Hızlandırıcı

Azure FarmBeats Hızlandırıcı, Datahub'ın üzerine inşa edilmiş örnek bir web uygulamasıdır. Hızlandırıcı, kullanıcı arabiriminizi ve model geliştirmenizi hızlandırıyor. Azure FarmBeats hızlandırıcısı Azure FarmBeats'in API'lerini kullanır. Sindirilen sensör verilerini grafikler ve model çıktıları olarak harita olarak görselleştirir. Örneğin, hızlandırıcıyı hızlı bir şekilde bir çiftlik oluşturmak ve o çiftlik için bir bitki indeksi haritası veya sensör yerleştirme haritası almak için kullanabilirsiniz.

## <a name="resources"></a>Kaynaklar

Azure FarmBeats ek ücret ödemeden sunulur ve yalnızca kullandığınız Azure kaynakları için ödeme yaptığınızda ödeme yapılır. Teklif hakkında daha fazla şey öğrenmek için aşağıdaki kaynakları kullanabilirsiniz:

- [Azure FarmBeats blogumuzu](https://aka.ms/farmbeatsblog)ziyaret ederek en son Azure FarmBeats haberlerinden haberdar olun.
- [Azure FarmBeats destek forumumuzda](https://aka.ms/farmbeatssupport)bir soru göndererek yardım isteyin.
- [Azure FarmBeats geri bildirim forumumuzda](https://aka.ms/farmbeatsfeedback)bir özellik fikri göndererek veya bir özellik fikri için oy vererek geri bildirim sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure FarmBeats'i yükleme](install-azure-farmbeats.md)
