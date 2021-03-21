---
title: Purview öngörülerini kullanarak verilerinize ilişkin sözlük raporu
description: Bu nasıl yapılır kılavuzunda, verileriniz üzerinde purview Insights sözlüğü raporlama 'nın nasıl görüntüleneceği ve kullanılacağı açıklanmaktadır.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/20/2020
ms.openlocfilehash: eb1d59ae41b04be60dec90aaee4b2305b6d39ca6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095859"
---
# <a name="glossary-insights-on-your-data-in-azure-purview"></a>Azure purview 'daki verilerinize ilişkin sözlük öngörüleri

Bu nasıl yapılır kılavuzunda verilerinize yönelik olarak nasıl erişim, görüntüleme ve filtreleme öngörüleri raporlarının nasıl yapılacağı açıklanmaktadır.

Bu nasıl yapılır kılavuzunda şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> - Purview hesabınızdaki Öngörüler 'e gidin
> - Verilerinizin bir kuşbakışı görünümünü alın

## <a name="prerequisites"></a>Önkoşullar

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

1. Takip görünümü **giriş** sayfasında, Öngörüler alanına erişmek için **öngörüleri görüntüle** kutucuğunu seçin  :::image type="icon" source="media/glossary-insights/ico-insights.png" border="false"::: .

   :::image type="content" source="./media/glossary-insights/view-insights.png" alt-text="Azure portal öngörülerinizi görüntüleyin":::

1. **Öngörüler** :::image type="icon" source="media/glossary-insights/ico-insights.png" border="false"::: alanında **terimler** ' i seçerek purview **Sözlük öngörüleri** raporunu görüntüleyin.

**Sözlük öngörüleri** , kuruluşunuz için iyi tanımlanmış bir sözlük sağlamak üzere iş kullanıcısı ve değerli bilgiler sağlar.

1. Rapor, ***purview hesabınızdaki* _. *._varlıklar olmadan onaylanan_ toplam koşulları, _ *ve _*_kullanım dışı koşulları olan varlıkları_** gösteren **üst düzey KPI** 'ler ile başlar. Bu değerlerin her biri, Sözlüğlerinizin sistem durumunu belirlemenize yardımcı olur.

   :::image type="content" source="./media/glossary-insights/glossary-kpi.png" alt-text="Sözlük öngörülerini görüntüle KPI"::: 


2. **Koşullar bölümünün anlık görüntüsü** (yukarıda görüntülenir), varlıklar ve varlık olmayan hüküm ve koşullarla ilgili şartlar için **_taslak_*_, _*_Onaylandı_*_, _*_Alert_*_ ve _*_süresi dolduğunda_ ,** dönem durumunu gösterir.

3. Çeşitli durum ve **_Steki_*_ ve _*_uzmanlar_** hakkında daha fazla ayrıntı içeren terim adlarını görmek için **daha fazla görüntüle** ' ye tıklayın. 

   :::image type="content" source="./media/glossary-insights/glossary-view-more.png" alt-text="Varlıklar ile ve olmayan terimlerin anlık görüntüsü":::  

4. "Daha fazla bilgi" ' ye tıkladığınızda, "**varlıklar Ile onaylanan koşullar**" için "daha fazla bilgi" ye tıkladığınızda, içgörüler, ekli koşullara sahip varlıklar listesine daha fazla gidebileceğiniz _ *Sözlük** terim ayrıntısı sayfasına gitmenizi sağlar. 

   :::image type="content" source="./media/glossary-insights/navigate-to-glossary-detail.png" alt-text="Sözlükte Öngörüler"::: 

4. Sözlük öngörüleri sayfasında, eksik **koşulların** bir dağıtımını eksik bilgi türüne göre görüntüleyin. Grafik, eksik **_tanım_*_, _*_eksik uzman_*_, _* eksik _Steward_*_ ve _* içinde _birden fazla alanı eksik_** olan terimlerin sayısını gösterir.

1. Eksik bilgileri olan terimleri görüntülemek için, ***View more** _ from _ * eksik terimler * * öğesine tıklayın. Eksik bilgileri girmek ve sözlük teriminin tamamlandığından emin olmak için, sözlük terimi ayrıntı sayfasına gidebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Sözlük aracılığıyla sözlük](./how-to-create-import-export-glossary.md) terimi oluşturma hakkında daha fazla bilgi edinin