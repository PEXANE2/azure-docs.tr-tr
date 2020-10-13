---
title: Azure Sentinel Watchlists kullanma
description: Bu makalede, Azure Sentinel Watchlists araştırma tehditleri, iş verilerini içeri aktarma, izin verilenler listeleri oluşturma ve zengin olay verileri kullanma açıklanmaktadır.
services: sentinel
author: yelevin
ms.author: yelevin
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.custom: mvc
ms.date: 09/06/2020
ms.openlocfilehash: 25252b73f25a96f85d5e2cf1d68b76f9eaa3ca75
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91979909"
---
# <a name="use-azure-sentinel-watchlists"></a>Azure Sentinel Watchlists kullanma

Azure Sentinel Watchlists, Azure Sentinel ortamınızdaki olaylarla bağıntı için dış veri kaynaklarından veri toplamayı etkinleştirir. Oluşturulduktan sonra, arama, algılama kuralları, tehdit araması ve yanıt PlayBook 'lerinizde Watchlists kullanabilirsiniz. Watchlists, Azure Sentinel çalışma alanınızda ad-değer çiftleri olarak depolanır ve en iyi sorgu performansı ve düşük gecikme süresi için önbelleğe alınır.

Watchlists kullanmaya yönelik yaygın senaryolar şunlardır:

- IP adreslerinin, dosya karmalarının ve CSV dosyalarındaki diğer verilerin hızlı bir şekilde içeri aktarılması ile **tehditleri araştırma** ve olaylara yanıt verme. İçeri aktarıldıktan sonra, listem ad-değer çiftlerini, uyarı kuralları, tehdit arayışları, çalışma kitapları, Not defterleri ve genel sorgularda birleşimler ve filtreler için kullanabilirsiniz.

- **İş verilerini** bir Watchlists olarak içeri aktarma. Örneğin, ayrıcalıklı sistem erişimi veya sonlandırılmış çalışanlar içeren kullanıcı listelerini içeri aktarın ve ardından bu kullanıcıların ağda oturum açmasını algılamak veya engellemek için kullanılan izin verme ve reddetme listeleri oluşturmak için listem kullanın.

- **Uyarı durumunu azaltma**. Normalde uyarıyı tetikleyen görevleri gerçekleştiren yetkili IP adreslerinden kullanıcılar gibi kullanıcı grubundan gelen uyarıları bastırmak için izin verilenler listeleri oluşturun ve zararsız olayların uyarı haline gelmesini önler.

- **Olay verilerini zenginleştirme**. Dış veri kaynaklarından türetilmiş ad-değer birleşimleriyle olay verilerinizi zenginleştirmek için Watchlists kullanın.

## <a name="create-a-new-watchlist"></a>Yeni bir listem oluşturma

1. Azure Portal **Azure Sentinel**  >  **Configuration**  >  **listem** ' e gidin ve **Yeni Ekle**' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Yeni listem](./media/watchlists/sentinel-watchlist-new.png)

1. **Genel** sayfasında, listem için ad, açıklama ve diğer ad belirtin ve ardından **İleri**' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![listem genel sayfası](./media/watchlists/sentinel-watchlist-general.png)

1. **Kaynak** sayfasında, veri kümesi türünü seçin, bir dosyayı karşıya yükleyin ve ardından **İleri**' yi seçin.

    :::image type="content" source="./media/watchlists/sentinel-watchlist-source.png" alt-text="listem kaynak sayfası" lightbox="./media/watchlists/sentinel-watchlist-source.png":::


1. Bilgileri gözden geçirin, doğru olduğunu doğrulayın ve ardından **Oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![listem İnceleme sayfası](./media/watchlists/sentinel-watchlist-review.png)

    Listem oluşturulduktan sonra bir bildirim görüntülenir.

    :::image type="content" source="./media/watchlists/sentinel-watchlist-complete.png" alt-text="listem kaynak sayfası" lightbox="./media/watchlists/sentinel-watchlist-complete.png":::


## <a name="use-watchlists-in-queries"></a>Sorgularda Watchlists kullanma

1. Azure Portal **Azure Sentinel**  >  **Configuration**  >  **listem**' e gidin, kullanmak istediğiniz listem seçin ve sonra **Log Analytics görüntüle**' yi seçin.

    :::image type="content" source="./media/watchlists/sentinel-watchlist-queries-list.png" alt-text="listem kaynak sayfası" lightbox="./media/watchlists/sentinel-watchlist-queries-list.png":::


1. Listem içindeki öğeler, sorgunuz için otomatik olarak ayıklanır ve **sonuçlar** sekmesinde görüntülenir. Aşağıdaki örnekte, **ServerName** ve **IPAddress** alanlarının ayıklanmasına ilişkin sonuçlar gösterilmektedir.

    > [!NOTE]
    > Sorgularınızdaki zaman damgası hem sorgu Kullanıcı arabiriminde hem de zamanlanmış uyarılarda yok sayılır.

    :::image type="content" source="./media/watchlists/sentinel-watchlist-queries-fields.png" alt-text="listem kaynak sayfası" lightbox="./media/watchlists/sentinel-watchlist-queries-fields.png":::
    
## <a name="use-watchlists-in-analytics-rules"></a>Analiz kurallarında Watchlists kullanma

Watchlists 'i analiz kurallarında kullanmak için Azure Portal **Azure Sentinel**  >  **yapılandırma**  >  **Analizi**' ne gidin ve sorgudaki işlevi kullanarak bir kural oluşturun `_GetWatchlist('<watchlist>')` .

:::image type="content" source="./media/watchlists/sentinel-watchlist-analytics-rule.png" alt-text="listem kaynak sayfası" lightbox="./media/watchlists/sentinel-watchlist-analytics-rule.png":::


## <a name="view-list-of-watchlists-aliases"></a>Watchlists diğer ad listesini görüntüle

Listem diğer adların bir listesini almak için Azure Portal **Azure Sentinel**  >  **genel**  >  **Günlükler**' e gidin ve aşağıdaki sorguyu çalıştırın: `_GetWatchlistAlias` . Diğer adların listesini **sonuçlar** sekmesinde görebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Watchlists Listele](./media/watchlists/sentinel-watchlist-alias.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel 'de Watchlists kullanarak verileri zenginleştirme ve araştırmamaları geliştirme hakkında öğrenirsiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .

