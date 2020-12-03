---
title: Purview Studio 'Yu kullanma
description: Bu kavramsal makalede Azure purview Studio 'Nun nasıl kullanılacağı açıklanmaktadır.
author: nayenama
ms.author: nayenama
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/12/2020
ms.openlocfilehash: 1b2d371153d6612f454e1bf51b78c6b6189a08b2
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96553851"
---
# <a name="use-purview-studio"></a>Purview Studio 'Yu kullanma

Bu makale, Azure purview 'ın bazı ana özelliklerine genel bir bakış sunar.

## <a name="prerequisites"></a>Ön koşullar

* Azure portal 'de etkin bir purview hesabı zaten oluşturulmuş ve kullanıcının purview Studio 'ya erişme izinleri var.

## <a name="launch-purview-account"></a>Purview hesabını başlatın

* Purview hesabınızı başlatmak için, Azure portal ' de, Takiview hesaplarına gidin, başlatmak istediğiniz hesabı seçin ve hesabı başlatın.

   :::image type="content" source="./media/use-purview-studio/launch-from-portal.png" alt-text="Azure purview hesabı kataloğunu başlatmak için seçimin ekran görüntüsü.":::

* Purview hesabını başlatmaya yönelik başka bir yol `https://web.purview.azure.com` da hesabı başlatmak için **Azure Active Directory** ve hesap adı ' nı seçin.

## <a name="home-page"></a>Giriş sayfası

**Giriş** , Azure purview istemcisinin başlangıç sayfasıdır.

 :::image type="content" source="./media/use-purview-studio/purview-homepage.png" alt-text="Giriş sayfasının ekran görüntüsü.":::

Aşağıdaki liste, ana **sayfanın** ana özelliklerini özetler. Listedeki her bir sayı, önceki ekran görüntüsünde vurgulanan bir sayıya karşılık gelir.

1. Kataloğun kolay adı. **Yönetim merkezi** > **hesap bilgileri*' nde Katalog adı ' nı ayarlayabilirsiniz.

2. Katalog Analizi şu sayıyı gösterir:
    - Kullanıcılar, gruplar ve uygulamalar
    - Veri kaynakları
    - Varlıklar
    - Sözlük terimleri

3. Arama kutusu, veri kataloğu genelinde veri varlıkları aramanızı sağlar.

4. Hızlı erişim düğmeleri uygulamanın sık kullanılan işlevlerine erişim sağlar. Sunulan düğmeler, Kullanıcı hesabınıza atanan role göre değişir.

    - *Veri kaynağı Yöneticisi* için hızlı erişim düğmeleri şunlardır: **veri kaynaklarını** ve **bilgi merkezini** kaydetme.
    - *Veri seçicisi* Için düğmeler **Bilgi Merkezi**, **varlıklara gözatıp**, **sözlüğü Yönet** ve **öngörüleri görüntüle**' dir.
    - *Veri okuyucu* için, öne çıkan düğmeler **Bilgi Merkezi**, **varlıklara gözatıp**, **sözlüğü görüntüle** ve **öngörüleri görüntüle**' dir.

5. Sol gezinti çubuğu, uygulamanın ana sayfalarını bulmanıza yardımcı olur. Sunulan düğmeler, Kullanıcı hesabınıza atanan role göre değişir.

    - *Veri kaynağı Yöneticisi* Için düğmeler **ana**, **kaynak** ve **Yönetim Merkezi**' dir.
    - *Veri seçicisi* Için düğmeler **ana**, **Sözlük**, **Öngörüler** ve **Yönetim Merkezi**' dir.
    - *Veri okuyucu* Için düğmeler **ana**, **Sözlük**, **Öngörüler** ve **Yönetim Merkezi**' dir.
  
6. **Son erişilen** sekme, son erişilen veri varlıklarının bir listesini gösterir. Varlıklara erişme hakkında daha fazla bilgi için bkz. [veri kataloğunu arama](how-to-search-catalog.md) ve [varlık türüne göre tarama](how-to-browse-catalog.md#browse-experience).  **Öğelerim** sekmesi, oturum açan kullanıcının sahip olduğu veri varlıklarının bir listesidir.
7. **Faydalı bağlantılar** bölge durumu, belgeler, fiyatlandırma, genel bakış ve takip durumu bağlantılarını içerir
8. Üst gezinti çubuğu sürüm notları/güncelleştirmeler, değişiklik takip hesabı, bildirimler, yardım ve geri bildirim bölümleri hakkında bilgi içerir.

## <a name="knowledge-center"></a>Bilgi Merkezi

Bilgi Merkezi, purview ile ilgili tüm videoları ve öğreticileri bulabileceğiniz yerdir.

## <a name="guided-tours"></a>Kılavuzlu Turlar

Azure purview Studio 'daki her UX, sayfaya genel bakış sağlamak için Kılavuzlu Turlar kullanacaktır. Kılavuzlu tura başlamak için üstteki çubukta **Yardım** ' ı seçin ve **Kılavuzlu Turları** seçin.

:::image type="content" source="./media/use-purview-studio/guided-tour.png" alt-text="Kılavuzlu turun ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Güvenlik sorumlusu ekleme](tutorial-scan-data.md)