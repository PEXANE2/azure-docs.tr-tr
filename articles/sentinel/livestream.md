---
title: Tehditleri algılamak için Azure Sentinel 'de Livestream 'i kullanma | Microsoft Docs
description: Bu makalede, verileri izlemek için Azure Sentinel 'de Livestream 'in nasıl kullanılacağı açıklanır.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2020
ms.author: yelevin
ms.openlocfilehash: 14928d3c94ced8d1cd0c12e76428be73b68b91d9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84783172"
---
# <a name="use-hunting-livestream-in-azure-sentinel-to-detect-threats"></a>Tehditleri algılamak için Azure Sentinel 'de canlı akışı 'i kullanma

Yeni oluşturulan sorguları olay meydana geldiğinde test etmenize olanak sağlayan etkileşimli oturumlar oluşturmak, bir eşleşme bulunduğunda oturumlardan bildirimler almak ve gerekirse araştırmalar başlatmak için canlı akışı 'i kullanın. Herhangi bir Log Analytics sorgusunu kullanarak hızlı bir şekilde canlı akış oturumu oluşturabilirsiniz.

- **Yeni oluşturulan sorguları olay gerçekleştiğinde sına**
    
    Olaylara etkin olarak uygulanan geçerli kurallara herhangi bir çakışma olmadan sorgu test edebilir ve ayarlayabilirsiniz. Bu yeni sorguların beklendiği gibi çalıştığını doğruladıktan sonra, bir uyarıya oturumu destekleyen bir seçenek belirleyerek bunları özel uyarı kurallarına kolayca yükseltebilirsiniz.

- **Tehditler gerçekleştiğinde bildirim alın**
    
    Tehdit veri akışlarını toplanmış günlük verileriyle karşılaştırabilir ve bir eşleşme gerçekleştiğinde bilgilendirilirsiniz. Tehdit veri akışları, olası veya geçerli tehditlerle ilgili verilerin devam eden akışlardır ve bu nedenle bildirim, kuruluşunuzun potansiyel bir tehdidi olduğunu gösterebilir. Özel bir uyarı kuralını korumanın fazla olması gerekmeden olası bir sorun hakkında bildirim almak istediğinizde özel bir uyarı kuralı yerine bir canlı akışı oturumu oluşturun.

- **Araştırmamaları Başlat**
    
    Konak veya Kullanıcı gibi bir varlıkla ilgili etkin bir araştırma varsa, günlük verilerinde söz konusu varlık üzerinde olduğu gibi belirli (veya herhangi bir) etkinlik görüntüleyebilirsiniz. Bu etkinlik gerçekleştiğinde size bildirilmesini sağlayabilirsiniz.


## <a name="create-a-livestream-session"></a>Canlı akışı oturumu oluşturma

Mevcut bir arama sorgusundan canlı akışı oturumu oluşturabilir veya oturumunuzu sıfırdan oluşturabilirsiniz.

1. Azure Portal, **Sentinel**  >  **tehdit yönetimi**  >  **Arayıcı**' a gidin.

1. Bir hunme sorgusundan canlı akış oturumu oluşturmak için:
    
    1. **Sorgular** sekmesinden kullanılacak arama sorgusunu bulun.
    1. Sorguya sağ tıklayın ve **canlı akışı 'e Ekle**' yi seçin. Örneğin:
    
    > [!div class="mx-imgBorder"]
    > ![Azure Sentinel arama sorgusundan Livestream oturumu oluşturma](./media/livestream/livestream-from-query.png)

1. Sıfırdan bir canlı akışı oturumu oluşturmak için: 
    
    1. **Livestream** sekmesini seçin
    1. **+ Yeni canlı akışı**' e tıklayın.
    
1. **Livestream** bölmesinde:
    
    - Bir sorgudan canlı akışı 'i başlattıysanız, sorguyu gözden geçirin ve yapmak istediğiniz değişiklikleri yapın.
    - Sıfırdan canlı akışı 'i başlattıysanız sorgunuzu oluşturun. 

1. Komut çubuğundan **oynat** ' ı seçin.
    
    Komut çubuğunun altındaki durum çubuğu, canlı akışı oturumunuzun çalışıp çalışmadığını veya duraklatılmadığını gösterir. Aşağıdaki örnekte, oturum çalışıyor:
    
    > [!div class="mx-imgBorder"]
    > ![Azure Sentinel ile canlı akış oturumu oluşturma](./media/livestream/livestream-session.png)

1. Komut çubuğundan **Kaydet** ' i seçin.
    
    **Duraklat**' ı seçmezseniz, Azure Portal oturumunuz kapatılana kadar oturum çalışmaya devam eder.

## <a name="view-your-livestream-sessions"></a>Canlı akışı oturumlarınızı görüntüleyin

1. Azure Portal, **Sentinel**  >  **tehdit yönetimi**için  >  **Hunting**  >  **Livestream** sekmesine gidin.

1. Görüntülemek veya düzenlemek istediğiniz canlı akışı oturumunu seçin. Örneğin:
    
    > [!div class="mx-imgBorder"]
    > ![Azure Sentinel arama sorgusundan canlı akışı oturumu oluşturma](./media/livestream/livestream-tab.png)
    
    Seçtiğiniz canlı akışı oturumunuz açılarak oynatma, duraklatma, düzenleme gibi bir şekilde açılır.

## <a name="receive-notifications-when-new-events-occur"></a>Yeni olaylar gerçekleştiğinde bildirim alın

Yeni olaylar için canlı akışı bildirimleri Azure Portal bildirimleri kullandığından, Azure Portal her kullanışınızda bu bildirimleri görürsünüz. Örneğin:

![Canlı akışı için Azure Portal bildirimi](./media/livestream/notification.png)

**Livestream** bölmesini açmak için bildirimi seçin.
 
## <a name="elevate-a-livestream-session-to-an-alert"></a>Canlı akış oturumunu bir uyarıya yükseltme

İlgili canlı akışı oturumunda komut çubuğundan **uyarıya Yükselt** ' i seçerek, bir canlı akış oturumunu yeni bir uyarıya yükseltebilirsiniz:

> [!div class="mx-imgBorder"]
> ![Canlı akışı oturumunu bir uyarıya yükselt](./media/livestream/elevate-to-alert.png)

Bu eylem, canlı akışı oturumuyla ilişkili sorguyla önceden doldurulan kural oluşturma Sihirbazı ' nı açar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Sentinel 'de canlı akışı 'i nasıl kullanacağınızı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Tehditler için proaktif araya](hunting.md)
- [Otomatik arama kampanyalarını çalıştırmak için not defterlerini kullanın](notebooks.md)
