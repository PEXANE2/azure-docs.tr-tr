---
title: Azure Güvenlik Merkezi 'ni kullanarak mevzuata uyumluluğunuzu geliştirme | Microsoft Docs
description: "Öğretici: Azure Güvenlik Merkezi 'ni kullanarak mevzuata uyumluluğunuzu geliştirmeyi öğrenin."
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/30/2019
ms.author: memildin
ms.openlocfilehash: 20842997c5df81835024a6f458cd863b4e4d78b0
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71202435"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Öğretici: Mevzuat uyumluluğunuzu artırma
---

Azure Güvenlik Merkezi, mevzuat uyumluluk panosunu kullanarak, mevzuat uyumluluk gereksinimlerini karşılamaya yönelik işlemin kolaylaştırılmasına yardımcı olur. Panoda, Güvenlik Merkezi, Azure ortamınızın sürekli değerlendirmelerine bağlı olarak uyumluluk gönderinize yönelik öngörüler sağlar. Güvenlik Merkezi tarafından gerçekleştirilen değerlendirmeler, karma bulut ortamınızdaki risk etkenlerini, en iyi güvenlik uygulamalarına uygun olarak analiz eder. Bu değerlendirmeler, desteklenen bir standartlar kümesinden uyumluluk denetimleriyle eşlenir. Yasal uyumluluk panosunda, ortamınızdaki tüm bu değerlendirmelerin durumunun belirli bir standart veya yönetmeliği bağlamında açık bir şekilde görünümüne sahip olursunuz. Önerilerinizi ve ortamınızdaki risk etmenlerini azaltmanızı sağlayacak şekilde uyumluluğun iyileştirilmesine bakabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

-   Mevzuat uyumluluk panosunu kullanarak mevzuata uyumluluğunuzu değerlendirin

-   Önerilerle ilgili eylem gerçekleştirerek uyumluluk durunuzu geliştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide ele alınan özellikler arasında ilerlemek için Güvenlik Merkezi 'nin standart fiyatlandırma katmanına sahip olmanız gerekir. Güvenlik Merkezi Standart 'ı ücretsiz olarak deneyebilirsiniz.
Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](https://docs.microsoft.com/azure/security-center/security-center-get-started) başlıklı hızlı başlangıçta Standart katmanına nasıl yükseltebileceğiniz adım adım açıklanmıştır.

##  <a name="assess-your-regulatory-compliance"></a>Mevzuata uyumluluğunuzu değerlendirin

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını belirlemek üzere kaynaklarınızın yapılandırmasını sürekli olarak değerlendirir. Bu değerlendirmeler, güvenlik Hygiene 'yı geliştirmeye odaklanarak öneriler olarak sunulur. Mevzuat uyumluluk panosunda, desteklenen gereksinimlerin ilgili güvenlik değerlendirmelerine eşlendiği tüm gereksinimleriyle birlikte bir uyumluluk standartları kümesini görüntüleyebilirsiniz. Bu, bu değerlendirmelerin durumuna bağlı olarak, uyumluluğa göre uyumluluk durunuzu görüntülemenize olanak sağlar.

Mevzuat uyumluluk panosu görünümü, sizin için önemli bir standart veya yönetmele uyumlu olan boşlukların dikkatini çekmenize yardımcı olabilir. Bu odaklanmış görünüm, dinamik bulut ve karma ortamlarda zaman içinde uyumluluk puanınızı sürekli olarak izlemenize de olanak sağlar.

>[!NOTE]
> Şu anda desteklenen mevzuata standartları şunlardır: Azure CIS, PCI DSS 3,2, ISO 27001 ve SOC TSP. Ek standartlar geliştikçe panoya yansıtılacaktır.
1.  Güvenlik Merkezi ana menüsünde **ilke &AMP; uyumluluk** altında, **mevzuat uyumluluğu**' nu seçin. <br>
Ekranın üst kısmında, desteklenen uyumluluk düzenlemeleri kümesiyle uyumluluk durumunuz hakkında genel bakış içeren bir pano görürsünüz. Genel uyumluluk puanınızı ve her bir standart ile ilişkili geçiş ve başarısız değerlendirmelerin sayısını görebilirsiniz.

    ![bilgisayar açıklaması yüksek güvenirlik](./media/security-center-compliance-dashboard/compliance-dashboard.png)


2.  Size uygun uyumluluk standardı için bir sekme seçin. Söz konusu standartla ilgili tüm denetimleri göreceksiniz. İlgili denetimler için, söz konusu denetimle ilişkilendirilmiş başarılı ve başarısız değerlendirmelerin ayrıntılarını görüntüleyebilirsiniz. Bazı denetimler gri görüntülenir. Bu denetimlerle ilişkilendirilmiş hiçbir Güvenlik Merkezi değerlendirmesi yoktur. Bunlarla ilgili gereksinimleri çözümlemeniz ve bunları kendi ortamınızda değerlendirmenize gerek vardır. Bunlardan bazıları teknik değil işlem ile ilgili olabilir.

    ![Uyumluluk sekmesi](./media/security-center-compliance-dashboard/compliance-pci.png)

3. Tüm ilgili güvenlik merkezi önerilerinin ve bunlarla ilişkili standartların görünümünü görmek için **Tümü** sekmesini seçin. Bu görünüm belirli bir öneriden etkilenen tüm farklı standartları belirlemek için yararlı olabilir. <br> Bu görünümü, çözmeniz gereken önerilerin önceliklerini belirlemek için kullanabilirsiniz. Örneğin, **aboneliğinizde sahip izinleri olan hesaplar IÇIN MFA 'Yı etkinleştirme** önerisinin birden fazla kaynak üzerinde başarısız olduğunu ve birden çok standartlarla ilişkilendirildiğini görürseniz, bu öneriyi çözmek üzerinde yüksek bir etkisi olur genel uyumluluk puanınız.

    ![Uyumluluk puanı etkisi](./media/security-center-compliance-dashboard/compliance-all-tabs.png)

1. Belirli bir standart için geçerli uyumluluk durumunuzu özetleyen bir PDF raporu oluşturup indirmek için **raporu indir**' e tıklayın.

    Rapor, güvenlik merkezi değerlendirme verilerine dayanarak seçili standart için uyumluluk durumunuzu üst düzey bir Özet sağlar ve söz konusu standart denetimlere göre düzenlenmiştir. Rapor ilgili hissedarlarla paylaşılabilir ve iç ve dış denetçiler için kanıt sağlamaya yönelik olabilir.

    ![İndirme](./media/security-center-compliance-dashboard/download-report.png)

## <a name="improve-your-compliance-posture"></a>Uyumluluk durunuzu geliştirme

Yasal uyumluluk panosundaki bilgiler verildiğinde, önerileri doğrudan pano içinde çözerek uyumluluk durunuzu geliştirebilirsiniz.

1.  Söz konusu öneriye ilişkin ayrıntıları görüntülemek için Panoda görünen başarısız değerlendirmelerin herhangi birine tıklayın. Her öneri, sorunu çözmek için izlenmesi gereken bir düzeltme adımları kümesi içerir.

2.  Daha fazla ayrıntı görüntülemek ve söz konusu kaynak için öneriyi çözümlemek üzere belirli bir kaynağı seçebilirsiniz. <br>Örneğin, **Azure CIS standart** sekmesinde, **depolama hesabına güvenli aktarım gerektir**önerisi ' ne tıklayabilirsiniz.

    ![Uyumluluk önerisi](./media/security-center-compliance-dashboard/compliance-recommendation.png)

3. Öneri bilgilerine tıklayıp sağlıksız bir kaynağı seçtiğinizde, Azure portal içinde **güvenli depolama aktarımını** etkinleştirme deneyimine doğrudan yol açar.<br>Önerileri uygulama hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

    ![Uyumluluk önerisi](./media/security-center-compliance-dashboard/compliance-remediate-recommendation.png)

4.  Önerileri çözümlemek için eylem gerçekleştirdikten sonra uyumluluk puanınız artdığından uyumluluk panosu raporundaki etkiyi görürsünüz.

    > [!NOTE]
    > Değerlendirmeler yaklaşık olarak 12 saatte bir çalıştırılır, dolayısıyla uyumluluk verileriniz üzerindeki etkisini ancak değerlendirmeler çalıştırıldıktan sonra görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, güvenlik merkezi 'nin mevzuat uyumluluk panosunu şu şekilde kullanma hakkında bilgi edindiniz:

-   Sizin için önemli olan standartlara ve düzenlemelere göre uyumluluk gönderinizi görüntüleyin ve izleyin.

-   İlgili önerileri çözümleyerek ve Uyumluluk puanı iyileştirerek uyumluluğu izleyerek uyumluluk durumunuzu geliştirebilirsiniz.

Mevzuat uyumluluk panosu uyumluluk sürecini önemli ölçüde basitleştirebilir ve Azure ve hibrit ortamınız için uyumluluk kanıtlarını toplamak için gereken süreyi önemli ölçüde kesebilir.

Güvenlik Merkezi hakkında daha fazla bilgi için bkz.

-   [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.

-   [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md)-Azure kaynaklarınızın korunmasına yardımcı olmak Için Azure Güvenlik Merkezi 'nde önerilerin nasıl kullanılacağını öğrenin.

-   [Azure Güvenlik Merkezi 'nde güvenli puanınızı iyileştirme](security-center-secure-score.md)-güvenlik açıklarını en iyi şekilde geliştirmek için güvenlik açıklarına ve güvenlik önerilerini önceliklendirmenin nasıl yapılacağını öğrenin.

-   [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
