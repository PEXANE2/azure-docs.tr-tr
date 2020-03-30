---
title: Tehditleri algılamak için Azure Sentinel'de Livestream avlamayı kullanın| Microsoft Dokümanlar
description: Bu makalede, verileri izlemek için Azure Sentinel'de Livestream avcılık nasıl kullanılacağı açıklanmaktadır.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2019
ms.author: yelevin
ms.openlocfilehash: b392644e504fa8187e637278bef8718c9c2caa3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77582135"
---
# <a name="use-hunting-livestream-in-azure-sentinel-to-detect-threats"></a>Tehditleri algılamak için Azure Sentinel'de canlı avlanmayı kullanma

> [!IMPORTANT]
> Azure Sentinel'de canlı yayını avlamak şu anda genel önizlemede dir ve giderek kiracılara yayılıyor.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.


Olaylar meydana geldikçe yeni oluşturulan sorguları test etmenize, eşleşme bulunduğunda oturumlardan bildirimler almanıza ve gerekirse soruşturma başlatmanıza izin veren etkileşimli oturumlar oluşturmak için canlı yayını avlamayı kullanın. Herhangi bir Log Analytics sorgusukullanarak hızlı bir şekilde canlı akış oturumu oluşturabilirsiniz.

- **Olaylar oluştukça yeni oluşturulan sorguları test edin**
    
    Olaylara etkin olarak uygulanan geçerli kurallarla çakışma olmadan sorguları sınayabilir ve ayarlayabilirsiniz. Bu yeni sorguların beklendiği gibi çalıştığını onayladıktan sonra, oturumu bir uyarıya yükselten bir seçenek seçerek bunları özel uyarı kurallarına yükseltmek kolaydır.

- **Tehditler oluştuğunda bilgilendirilme**
    
    Tehdit veri akışlarını toplanan günlük verileriyle karşılaştırabilir ve bir eşleşme oluştuğunda bilgilendirebilirsiniz. Tehdit veri akışları, olası veya geçerli tehditlerle ilgili devam eden veri akışlarıdır, bu nedenle bildirim kuruluşunuz için potansiyel bir tehdit gösterebilir. Özel uyarı kuralını koruma nın genel giderleri olmadan olası bir sorundan haberdar olmak istediğinizde, özel uyarı kuralı yerine canlı akış oturumu oluşturun.

- **Soruşturma başlatma**
    
    Ana bilgisayar veya kullanıcı gibi bir varlığı içeren etkin bir araştırma varsa, günlük verilerindeki belirli (veya herhangi bir) etkinliği, söz konusu varlıkta oluştuğu gibi görüntüleyebilirsiniz. Bu etkinlik gerçekleştiğinde size bildirilebilir.


## <a name="create-a-livestream-session"></a>Canlı akış oturumu oluşturma

Varolan bir av sorgusundan canlı akış oturumu oluşturabilir veya oturumunuzu sıfırdan oluşturabilirsiniz.

1. Azure portalında **Sentinel** > **Threat yönetimi** > **Avı'na**gidin.

2. Bir av sorgusundan canlı akış oturumu oluşturmak için:
    
    1. **Sorgular** sekmesinden, kullanılacak av sorgusunu bulun.
    2. Sorguyu sağ tıklatın ve **canlı akışa ekle'yi**seçin. Örnek:
    
    > [!div class="mx-imgBorder"]
    > ![Azure Sentinel av sorgusundan Livestream oturumu oluşturma](./media/livestream/livestream-from-query.png)

3. Sıfırdan canlı yayın oturumu oluşturmak için: 
    
    1. **Livestream** sekmesini seçin
    2. **Canlı yayına git'i**seçin.
    
4. **Livestream** bölmesinde:
    
    - Bir sorgudan canlı akışı başlattıysanız, sorguyu gözden geçirin ve yapmak istediğiniz değişiklikleri yapın.
    - Canlı yayını sıfırdan başlattıysanız, sorgunuzu oluşturun. 

5. Komut çubuğundan **Oynat'ı** seçin.
    
    Komut çubuğunun altındaki durum çubuğu, canlı akış oturumunuzun çalışıp çalışmadığını veya duraklatılmış olup olmadığını gösterir. Aşağıdaki örnekte, oturum çalışıyor:
    
    > [!div class="mx-imgBorder"]
    > ![Azure Sentinel avından canlı akış oturumu oluşturma](./media/livestream/livestream-session.png)

6. Komut çubuğundan **Kaydet'i** seçin.
    
    **Duraklat'ı**seçmediğiniz sürece, oturum Azure portalından oturumunuz bitene kadar çalışır.

## <a name="view-your-livestream-sessions"></a>Canlı akış oturumlarınızı görüntüleyin

1. Azure portalında **Sentinel** > **Threat yönetimi** > **Avcılık** > **Livestream** sekmesine gidin.

2. Görüntülemek veya görüntülemek istediğiniz canlı akış oturumunu seçin. Örnek:
    
    > [!div class="mx-imgBorder"]
    > ![Azure Sentinel av sorgusundan canlı akış oturumu oluşturma](./media/livestream/livestream-tab.png)
    
    Seçili canlı akış oturumunuz oynamanız, duraklatmanız, düzenlemeniz ve benzeri için açılır.

## <a name="receive-notifications-when-new-events-occur"></a>Yeni olaylar oluştuğunda bildirimleri alma

Yeni etkinlikler için canlı akış bildirimleri Azure portal bildirimlerini kullandığından, Azure portalını her kullandığınızda bu bildirimleri görürsünüz. Örnek:

![Canlı akış için Azure portalı bildirimi](./media/livestream/notification.png)

**Livestream** bölmesini açmak için bildirimi seçin.
 
## <a name="elevate-a-livestream-session-to-an-alert"></a>Canlı akış oturumunu uyarıya yükseltme

İlgili canlı akış oturumunda komut çubuğundan **uyarmak için Yükseltme'yi** seçerek canlı akış oturumunu yeni bir uyarıya yükseltebilirsiniz:

> [!div class="mx-imgBorder"]
> ![Canlı akış oturumunu bir uyarıya yükseltme](./media/livestream/elevate-to-alert.png)

Bu eylem, canlı akış oturumu ile ilişkili sorgu ile önceden doldurulur kural oluşturma sihirbazı açar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Sentinel'de canlı avlanmayı nasıl kullanacağınızı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Proaktif tehditler için avı](hunting.md)
- [Otomatik avlanma kampanyaları yürütmek için dizüstü bilgisayarları kullanın](notebooks.md)
