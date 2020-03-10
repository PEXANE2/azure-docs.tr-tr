---
title: Azure Güvenlik Merkezi 'nde güvenli puan | Microsoft Docs
description: " Azure Güvenlik Merkezi 'ndeki güvenli puanı kullanarak güvenlik önerilerinizi önceliklendirin. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/15/2019
ms.author: memildin
ms.openlocfilehash: 15a94a9724fac29d36f6bb88ee4810b3bc7ca607
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78394630"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde güvenli puanınızı geliştirme

> [!NOTE]
> Önizlemede kullanılabilen gelişmiş bir güvenli puan vardır. Gelişmiş güvenli puan sonunda, son olarak var olan güvenli puanı değiştirilir, ancak geçiş kolaylığı için yan yana çalışmaya devam ederler.
>
> Gelişmiş güvenli puanların avantajları hakkında ayrıntılı bilgi için [buraya](secure-score-security-controls.md)bakın.
>
> Önizlemede bir bölüm almak için Azure portalını açın, Azure Güvenlik Merkezi 'ni başlatın ve güvenli puan ' i seçin. Buradan, sayfanın en üstünde yeni güvenli puan deneyimini sunan bir başlık görürsünüz. Alternatif olarak, [buraya](https://aka.ms/ascnewscore)tıklayın.

Bu sayede birçok hizmetin güvenlik avantajları sunarak, iş yükünüzü güvenli hale getirmek ve bu avantajlardan yararlanmak için öncelikle ne yapmanız gereken adımları bilmek zordur. Güvenli puan, güvenlik önerilerinizi gözden geçirir ve bunları sizin için önceliklendirir. bu sayede öncelikle hangi önerilerin gerçekleştirileceğini bilirsiniz. Bu, araştırmanın önceliklerini belirleyebilmeniz için en önemli güvenlik açıklarını bulmanıza yardımcı olur. Güvenli puan, iş yükü güvenlik duruşunuzu değerlendirmenize yardımcı olan bir araçtır.

## <a name="secure-score-calculation"></a>Güvenli puan hesaplaması

Güvenlik Merkezi, bir güvenlik analistinin çalışmasına, güvenlik önerilerinizi gözden geçirmeye ve her öneriye ne kadar önemli olduğunu belirlemede gelişmiş algoritmalar uygulamasına olanak sağlar.
Azure Güvenlik Merkezi, etkin önerilerinizi sürekli olarak inceler ve bunlara göre güvenli puanınızı hesaplar. bir önerinin puanı, iş yükünüzü en iyi şekilde etkileyecek önem derecesini ve en iyi güvenlik uygulamalarını elde eder.

Ayrıca Güvenlik Merkezi, size genel olarak **güvenli bir puan**sağlar. 

**Genel güvenli puan** , tüm öneri Puanlarınızın birikmesi olur. Seçtiğiniz seçeneğe bağlı olarak abonelikleriniz veya yönetim gruplarınız genelinde genel güvenli puanınızı görüntüleyebilirsiniz. Puan, seçilen aboneliğe ve bu aboneliklerdeki etkin önerilere göre değişir.

En çok güvenli puanınızı hangi önerilerin etkilediğini denetlemek için, güvenlik merkezi panosunda en iyi üç önemli öneriyi görüntüleyebilir veya öneriler listesi dikey penceresinde önerileri, **güvenli puan etkisi** sütununu kullanarak sıralayabilirsiniz.

Genel güvenli puanınızı görüntülemek için:

1. Azure panosunda **Güvenlik Merkezi** ' ne ve ardından **güvenli skor**' e tıklayın.

2. En üstte, güvenli puanı vurgular ' a bakabilirsiniz:
   - **Genel güvenli puan** , seçilen abonelik başına düşen ilke başına puan sayısını temsil eder
   - **Kategoriye göre güvenli puan** , en çok dikkat gerektiren kaynakları gösterir
   - **Güvenli puan etkisinden en önemli öneriler** , uygulamanızı uygularsanız en çok güvenli puanı iyileştirecek önerilerin bir listesini sağlar.
 
   ![Güvenli puan](./media/security-center-secure-score/secure-score-dashboard.png)

3. Aşağıdaki tabloda, aboneliklerinizin her birini ve her biri için genel güvenli puanı görebilirsiniz.

   > [!NOTE]
   > Her aboneliğin güvenli Puanlama toplamı, genel güvenli puana eşit değildir. Güvenli puan, aboneliklerinizde güvenli puanlar toplamı değil, sağlıklı kaynaklarınız ve öneri başına toplam kaynaklarınızın oranına göre hesaplanır. 
   >
4. Güvenli puanınızı iyileştirmek için düzeltebileceğiniz bu aboneliğe yönelik önerileri görmek için **öneriyi görüntüle** ' ye tıklayın.
4. Öneriler listesinde, her öneri için, **güvenli puan etkisini**temsil eden bir sütun olduğunu görebilirsiniz. Bu sayı, önerileri izlerseniz genel güvenli puanınızın ne kadar iyileştirebileceğini temsil eder. Örneğin, aşağıdaki ekranda, **kapsayıcı güvenlik yapılandırmalarında güvenlik açıklarını düzeltmeniz**durumunda, güvenli puanınız 35 noktayla artacaktır.

   ![Güvenli puan](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>Bireysel güvenli puan

Ayrıca, bireysel güvenli puanları görüntülemek için, bunları bireysel öneri dikey penceresinde bulabilirsiniz.  

**Öneri güvenli puanı** , sağlıklı kaynaklarınızın ve toplam kaynaklarınızın oranına göre hesaplanır. Sağlıklı kaynakların sayısı toplam kaynak sayısına eşitse, 50 önerisinin en yüksek güvenlik puanı elde edersiniz. Güvenli puanınızı en fazla puanı daha yakınına getirmek için, önerileri izleyerek sağlıksız kaynakları düzeltemedi.

Öneri **etkisi** , öneri adımlarını uyguladığınızda güvenli puanınızın ne kadar İyileşeceğimizi size sağlar. Örneğin, güvenli puanınız 42 ise ve **öneri etkisi** + 3 ise, öneriye göre özetlenen adımları gerçekleştirmek 45 için puanınızı geliştirme.

Öneri, düzeltme adımları alınmadığı takdirde iş yükünüzün hangi tehditlere maruz olduğunu gösterir.

![tek öneri güvenli puanı](./media/security-center-secure-score/indiv-recommendation-secure-score.png)




## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi 'nde **güvenli puanı** kullanarak güvenlik duruşunuzu nasıl iyileştireceğiniz gösterilmektedir. Güvenlik Merkezi hakkında daha fazla bilgi için bkz.

* [Azure Güvenlik Merkezi hakkında SSS](faq-general.md)-hizmet ve güvenli Puanlama hakkında sık sorulan soruları bulun.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Güvenli puan-gelişmiş](secure-score-security-controls.md)--Şu anda önizleme aşamasında olan gelişmiş güvenli puanların avantajları hakkında bilgi edinin.