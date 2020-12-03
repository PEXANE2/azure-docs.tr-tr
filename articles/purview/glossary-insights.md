---
title: Purview öngörülerini kullanarak verilerinize ilişkin sözlük raporu
description: Bu nasıl yapılır kılavuzunda, verileriniz üzerinde purview Insights sözlüğü raporlama 'nın nasıl görüntüleneceği ve kullanılacağı açıklanmaktadır.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/20/2020
ms.openlocfilehash: 9f7d81423c75c3e1a51f5b5d5f37c54307488eb3
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96554058"
---
# <a name="glossary-insights-on-your-data-in-azure-purview"></a>Azure purview 'daki verilerinize ilişkin sözlük öngörüleri

Bu nasıl yapılır kılavuzunda verilerinize yönelik olarak nasıl erişim, görüntüleme ve filtreleme öngörüleri raporlarının nasıl yapılacağı açıklanmaktadır.

Bu nasıl yapılır kılavuzunda şunları yapmayı öğreneceksiniz:

> - Purview hesabınızdaki Öngörüler 'e gidin
> - Verilerinizin bir kuşbakışı görünümünü alın

## <a name="prerequisites"></a>Ön koşullar

Purview Insights 'ı kullanmaya başlamadan önce aşağıdaki adımları tamamladığınızdan emin olun:

- Azure kaynaklarınızı ayarlayın ve hesabı verilerle doldurun

- Kaynak türünde bir taramayı ayarlama ve tamamlamayı Temizleme

- Sözlük ayarlama ve sözlük koşullarına varlık iliştirme

Daha fazla bilgi için bkz. [Azure purview 'ta veri kaynaklarını yönetme (Önizleme)](manage-data-sources.md).

## <a name="use-purview-glossary-insights"></a>Purview sözlük öngörülerini kullanma

Azure purview ' de, Sözlük terimleri oluşturabilir ve bunları varlıklara ekleyebilirsiniz. Daha sonra sözlük dağıtımını sözlük öngörülerine göre görüntüleyebilirsiniz. Bu, sözlüğizin durumlarını varlıklara bağlı koşullara göre gösterir. Ayrıca, roller için kullanıcı sayısına göre durum ve dağıtım koşullarını size bildirir.

**Sözlük öngörülerini görüntülemek için:**

1. Azure portal **Azure purview** [örneği ekranına](https://aka.ms/purviewportal) gidin ve purview hesabınızı seçin.

1. **Genel bakış** sayfasında, **Başlarken** bölümünde, **purview hesabını Başlat** kutucuğunu seçin.

   :::image type="content" source="./media/glossary-insights/portal-access.png" alt-text="Azure portal Takiview 'ı başlatın":::

1. Takip görünümü **giriş** sayfasında, Öngörüler alanına erişmek için **öngörüleri görüntüle** kutucuğunu seçin **Insights** :::image type="icon" source="media/glossary-insights/ico-insights.png" border="false"::: .

   :::image type="content" source="./media/glossary-insights/view-insights.png" alt-text="Azure portal öngörülerinizi görüntüleyin":::

1. **Öngörüler** :::image type="icon" source="media/glossary-insights/ico-insights.png" border="false"::: alanında **terimler** ' i seçerek purview **Sözlük öngörüleri** raporunu görüntüleyin.

**Sözlük öngörüleri** sayfasında aşağıdaki bölgeler görüntülenir:
1. Sözlük koşullarını ve Katalog kullanıcılarını göstermek için **üst düzey KPI** 'ler

2. **En üst Sözlük terimleri ve varlık sayısı** , bunlara eklenen varlıklar ile birlikte 5 sözlük koşullarını gösterir. Diğer tüm varlıklar, grafikteki "diğer" kategorisinde hesaba katılmaz.

3. **Terim koşullarına göre** terimler sözlüğü, sözlük koşullarının "taslak", "onaylanan", "uyarı" ve "süresi dolma" gibi durum dağılımını gösterir. 

1. Bir durum ile grafiğin dilimine gelin veya tıklayın ve bu duruma sahip koşulların sayısını unutmayın.

1. **Rollerin kullanıcı sayısına göre dağıtılması** , rollerdeki rol başına kullanıcı sayısına göre rollerin dağılımını gösterir.

   :::image type="content" source="./media/glossary-insights/glossary-insights1.png" alt-text="Sözlük öngörülerini görüntüle":::

## <a name="next-steps"></a>Sonraki adımlar

[Varlık öngörüleri](./asset-insights.md) aracılığıyla Azure purview Insight Reports hakkında daha fazla bilgi edinin