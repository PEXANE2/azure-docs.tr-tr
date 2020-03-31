---
title: Azure Güvenlik Merkezi'nde Güvenli Puan | Microsoft Dokümanlar
description: " Azure Güvenlik Merkezi'nde Güvenli Puan'ı kullanarak güvenlik önerilerinize öncelik verin. "
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
ms.openlocfilehash: 30405ce5cc875144fcd1cf83d4a3f883a0304989
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415763"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenli puanınızı artırın

> [!NOTE]
> Bu makale, güvenli skorun önceki sürümü hakkındadır. Bu güvenli puan deneyimi hala UI kullanılabilir ama zaman içinde aşamalı olacaktır. İki güvenli skor deneyimi, daha sorunsuz bir geçiş sağlamak için yan yana çalışıyor.
>
> Yeni güvenli puanın ayrıntıları için [buraya](secure-score-security-controls.md)bakın.
>

Güvenlik avantajları sunan bu kadar çok hizmetle, iş yükünüzü güvence altına almak ve sertleştirmek için ilk olarak hangi adımları atacağınız genellikle zordur. Güvenli Puan, güvenlik önerilerinizi gözden geçirir ve sizin için önceliklerini belirler, böylece ilk önce hangi önerileri gerçekleştireceklerinizi bilirsiniz. Bu, en ciddi güvenlik açıklarını bulmanıza yardımcı olur, böylece araştırmaya öncelik verirsiniz. Güvenli Puan, iş yükü güvenlik duruşunuzu değerlendirmenize yardımcı olan bir araçtır.

## <a name="secure-score-calculation"></a>Güvenli Puan hesaplaması

Güvenlik Merkezi, güvenlik önerilerinizi gözden geçirerek ve her önerinin ne kadar önemli olduğunu belirlemek için gelişmiş algoritmalar uygulayarak bir güvenlik analistinin çalışmasını taklit eder.
Azure Güvenlik merkezi etkin önerilerinizi sürekli olarak gözden geçirir ve Güvenli Puanınızı bunlara göre hesaplar, bir önerinin puanı, iş yükü güvenliğinizi en çok etkileyecek önem derecesi ve güvenlik en iyi uygulamalarından türetilir.

Güvenlik Merkezi de **Genel Güvenli Puan**sağlar. 

**Genel Güvenli Puan,** tüm tavsiye puanlarınızın bir birikimidir. Seçtiğiniz şeye bağlı olarak, genel Güvenli Puanınızı abonelikleriniz veya yönetim gruplarınız arasında görüntüleyebilirsiniz. Puan, seçilen abonelik ve bu abonelikler üzerindeki etkin önerilere bağlı olarak değişir.

Hangi önerilerin Güvenli Puanınızı en çok etkilediğini kontrol etmek için, Güvenlik Merkezi panosundaki en etkili üç öneriyi görüntüleyebilir veya **Güvenli Puan etki** sütununu kullanarak öneriler listesindeki önerileri sıralayabilirsiniz.

Genel Güvenli Puanınızı görüntülemek için:

1. Azure panosunda **Güvenlik Merkezi'ni** tıklatın ve ardından **Güvenli Puan'ı**tıklatın.

2. En üstte Güvenli Skor vurgularını görebilirsiniz:
   - **Genel Güvenli Puan,** seçili abonelik başına ilkeler başına puanı temsil eder
   - **Kategoriye göre Güvenli Puan,** hangi kaynakların en çok ilgiye ihtiyacı olduğunu gösterir
   - **Secure Score etkisiyle en iyi öneriler,** bunları uygularsanız Güvenli Puanınızı en çok artıracak önerilerin bir listesini sağlar.
 
   ![Güvenlik Puanı](./media/security-center-secure-score/secure-score-dashboard.png)

3. Aşağıdaki tabloda aboneliklerinizin her birini ve her biri için genel Güvenli Puanı görebilirsiniz.

   > [!NOTE]
   > Her aboneliğin Güvenli Puanı'nın toplamı, genel Güvenli Puan'a eşit değildir. Güvenli Puan, aboneliklerinizdeki Güvenli Puanların toplamı değil, sağlıklı kaynaklarınız ve öneri başına toplam kaynaklarınız arasındaki orana dayalı bir hesaplamadır. 
   >
4. Güvenli Puanınızı geliştirmek için düzeltebileceğiniz bu abonelik için önerileri görmek için **Önerileri Görüntüle'yi** tıklatın.
4. Öneriler listesinde, her öneri için **Güvenli Puan etkisini**temsil eden bir sütun olduğunu görebilirsiniz. Bu sayı, önerileri uygularsanız genel Güvenli Puanınızın ne kadar artacağını gösterir. Örneğin, aşağıdaki ekranda, kapsayıcı **güvenlik yapılandırmalarında güvenlik açıklarını**düzeltiyorsanız, Güvenli Puanınız 35 puan artacaktır.

   ![Güvenlik Puanı](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>Bireysel Güvenli Puan

Buna ek olarak, tek tek Güvenli Puanları görüntülemek için, bunları tek tek tavsiye bıçağıiçinde bulabilirsiniz.  

**Tavsiye Güvenli Puan,** sağlıklı kaynaklarınız ve toplam kaynaklarınız arasındaki orana dayalı bir hesaplamadır. Sağlıklı kaynak sayısı toplam kaynak sayısına eşitse, tavsiye önerisinin en yüksek Güvenli Puanı 50'yi alırsınız. Secure Score'unuzu maksimum skora yaklaştırmayı denemek için, önerileri izleyerek sağlıksız kaynakları düzeltin.

**Öneri etkisi,** öneri adımlarını uygularsanız Güvenli Puanınızın ne kadar iyileştiğini bilmenizi sağlar. Örneğin, Güvenli Puanınız 42 ve **Tavsiye etkisi** +3 ise, öneride belirtilen adımları gerçekleştirmek puanınızı 45'e yükseltin.

Öneri, düzeltme adımları atılırsa iş yükünüzün hangi tehditlere maruz kaldığını gösterir.

![bireysel tavsiye güvenli puan](./media/security-center-secure-score/indiv-recommendation-secure-score.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi'nde **Güvenli Puan'ı** kullanarak güvenlik duruşunuzu nasıl iyileştireceğiniz gösterilebistir. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için bkz:

* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
