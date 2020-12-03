---
title: 'Öğretici: mevzuat uyumluluk denetimleri-Azure Güvenlik Merkezi'
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
ms.date: 10/11/2020
ms.author: memildin
ms.openlocfilehash: bbc36dbb2a17d379d31a9a235898500aea36247d
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96533919"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Öğretici: Mevzuat uyumluluğunuzu artırma

Azure Güvenlik Merkezi, mevzuat **Uyumluluk panosunu** kullanarak, mevzuat uyumluluk gereksinimlerini karşılamaya yönelik işlemin kolaylaştırılmasına yardımcı olur. Panoda, Güvenlik Merkezi, Azure ortamınızın sürekli değerlendirmelerine bağlı olarak uyumluluk gönderinize yönelik öngörüler sağlar. Güvenlik Merkezi, karma bulut ortamınızdaki risk faktörlerini en iyi güvenlik uygulamalarına göre analiz eder. Bu değerlendirmeler, desteklenen bir standartlar kümesinden uyumluluk denetimleriyle eşlenir. Mevzuat uyumluluk panosunda, ortamınızdaki tüm değerlendirmelerin durumunu belirli bir standart veya yönetmeliği bağlamında görebilirsiniz. Önerilerle ilgili işlem yaparken ve ortamınızdaki risk etmenlerini azaltmanız durumunda uyumluluk durmanız artar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Mevzuat uyumluluk panosunu kullanarak mevzuata uyumluluğunuzu değerlendirin
> * Önerilerle ilgili eylem gerçekleştirerek uyumluluk durunuzu geliştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide ele alınan özellikler arasında ilerlemek için:

- [Azure Defender](azure-defender.md) etkin olmalıdır. Azure Defender 'ı 30 gün boyunca ücretsiz olarak deneyebilirsiniz.
- İlke uyumluluk verilerine okuyucu erişimi olan bir hesapla oturum açmanız gerekir (**güvenlik okuyucusu** yeterli değildir). Abonelik için **Genel okuyucu** rolü çalışacaktır. En azından, **kaynak Ilkesi katılımcısı** ve **Güvenlik yönetici** rollerinin atanması gerekir.

##  <a name="assess-your-regulatory-compliance"></a>Mevzuata uyumluluğunuzu değerlendirin

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını belirlemek üzere kaynaklarınızın yapılandırmasını sürekli olarak değerlendirir. Bu değerlendirmeler, güvenlik Hygiene 'yı geliştirmeye odaklanarak öneriler olarak sunulur. Mevzuat uyumluluk panosunda, desteklenen gereksinimlerin ilgili güvenlik değerlendirmelerine eşlendiği tüm gereksinimleriyle birlikte bir uyumluluk standartları kümesini görüntüleyebilirsiniz. Bu, bu değerlendirmelerin durumuna bağlı olarak, uyumluluğa göre uyumluluk durunuzu görüntülemenize olanak sağlar.

Mevzuat uyumluluk panosu görünümü, sizin için önemli bir standart veya yönetmele uyumlu olan boşlukların dikkatini çekmenize yardımcı olabilir. Bu odaklanmış görünüm, dinamik bulut ve karma ortamlarda zaman içinde uyumluluk puanınızı sürekli olarak izlemenize de olanak sağlar.

>[!NOTE]
> Varsayılan olarak, güvenlik merkezi şu düzenleme standartlarını destekler: Azure CIS, PCI DSS 3,2, ISO 27001 ve SOC TSP. 
>
> [Dinamik uyumluluk paketleri (Önizleme)](update-regulatory-compliance-packages.md) özelliği, mevzuat uyumluluk panonuzda gösterilen standartları yeni *dinamik* paketlere yükseltmenize olanak tanır. Ayrıca, yeni uyumluluk paketleri eklemek ve ek standartlarla uyumluluğunuzu izlemek için aynı önizleme özelliğini de kullanabilirsiniz. 

1. Güvenlik Merkezi menüsünde, **mevzuat uyumluluğu**' nı seçin. <br>
Ekranın üst kısmında, desteklenen uyumluluk düzenlemeleri kümesiyle uyumluluk durumunuz hakkında genel bakış içeren bir pano görürsünüz. Genel uyumluluk puanınızı ve her bir standart ile ilişkili geçiş ve başarısız değerlendirmelerin sayısını görebilirsiniz.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="Mevzuat uyumluluk panosu":::

1. Uyumluluk standardı için sizin için uygun bir sekme seçin (1). Standart (2) üzerinde hangi aboneliklerin uygulandığını ve bu standart (3) için tüm denetimlerin listesini görürsünüz. İlgili denetimler için, bu denetimle ilişkili geçen ve başarısız değerlendirmelerin ayrıntılarını (4) ve etkilenen kaynakların sayısını (5) görüntüleyebilirsiniz. Bazı denetimler gri renkte. Bu denetimlere kendileriyle ilişkili güvenlik merkezi değerlendirmeleri yok. Bunlarla ilgili gereksinimleri kontrol edin ve kendi ortamınızda kendi ortamınızda değerlendirin. Bunlardan bazıları teknik değil işlem ile ilgili olabilir.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="Belirli bir standartta uyumluluk ayrıntılarını keşfetme":::

1. Belirli bir standart için geçerli uyumluluk durumunuzu özetleyen bir PDF raporu oluşturup indirmek için **raporu indir**' e tıklayın.

    Rapor, güvenlik merkezi değerlendirme verilerine dayanarak seçili standart için uyumluluk durumunuzu üst düzey bir Özet sağlar ve söz konusu standart denetimlere göre düzenlenmiştir. Rapor ilgili hissedarlarla paylaşılabilir ve iç ve dış denetçiler için kanıt sağlayabilir.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="Uyumluluk raporunu indir":::

## <a name="improve-your-compliance-posture"></a>Uyumluluk durunuzu geliştirme

Yasal uyumluluk panosundaki bilgiler verildiğinde, önerileri doğrudan pano içinde çözerek uyumluluk durunuzu geliştirebilirsiniz.

1.  Söz konusu öneriye ilişkin ayrıntıları görüntülemek için Panoda görünen başarısız değerlendirmelerin herhangi birine tıklayın. Her öneri, sorunu çözmek için izlenmesi gereken bir düzeltme adımları kümesi içerir.

1.  Daha fazla ayrıntı görüntülemek ve söz konusu kaynak için öneriyi çözümlemek üzere belirli bir kaynağı seçebilirsiniz. <br>Örneğin, **Azure CIS 1.1.0 (yeni) standardında**, **sanal makinelere uygulanacak öneri disk şifrelemesini** seçebilirsiniz.

    :::image type="content" source="./media/security-center-compliance-dashboard/sample-recommendation.png" alt-text="Standart Müşteri adaylarından öneri seçme, doğrudan öneri ayrıntıları sayfasına":::

1. Bu örnekte, öneri ayrıntıları sayfasından **eylem al** ' ı seçtiğinizde, Azure Portal Azure sanal makine sayfalarına geldiğinizde **güvenlik** sekmesini açabilir ve şifrelemeyi etkinleştirebilirsiniz:

    :::image type="content" source="./media/security-center-compliance-dashboard/encrypting-vm-disks.png" alt-text="Öneri ayrıntıları sayfasında eylem Al düğmesi düzeltme seçeneklerine yol açar":::

    Önerilerin nasıl uygulanacağı hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

1.  Önerileri çözümlemek için eylem gerçekleştirdikten sonra uyumluluk puanınız artdığından uyumluluk panosu raporundaki etkiyi görürsünüz.

    > [!NOTE]
    > Değerlendirmeler yaklaşık 12 saatte bir çalışır, bu nedenle yalnızca ilgili değerlendirmenin bir sonraki çalıştırıldıktan sonra uyumluluk verilerinize etkisini görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, güvenlik merkezi 'nin mevzuat uyumluluk panosunu şu şekilde kullanma hakkında bilgi edindiniz:

-   Sizin için önemli olan standartlara ve düzenlemelere göre uyumluluk gönderinizi görüntüleyin ve izleyin.
-   İlgili önerileri çözümleyerek ve Uyumluluk puanı iyileştirerek uyumluluğu izleyerek uyumluluk durumunuzu geliştirebilirsiniz.

Mevzuat uyumluluk panosu uyumluluk sürecini önemli ölçüde basitleştirebilir ve Azure ve hibrit ortamınız için uyumluluk kanıtlarını toplamak için gereken süreyi önemli ölçüde kesebilir.

Daha fazla bilgi edinmek için bu ilgili makalelere bakın:

-   [Yasal uyumluluk panonuzda (Önizleme) dinamik uyumluluk paketlerine güncelleştirme](update-regulatory-compliance-packages.md) -yasal uyumluluk panonuzda gösterilen standartları yeni *dinamik* paketlere güncelleştirmenizi sağlayan bu önizleme özelliğini öğrenin. Ayrıca, yeni uyumluluk paketleri eklemek ve ek standartlarla uyumluluğunuzu izlemek için aynı önizleme özelliğini de kullanabilirsiniz. 
-   [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
-   [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -Azure kaynaklarınızın korunmasına yardımcı olmak Için Azure Güvenlik Merkezi 'nde önerilerin nasıl kullanılacağını öğrenin.
-   [Azure Güvenlik Merkezi 'Nde güvenli puanınızı geliştirme](secure-score-security-controls.md) -güvenlik açıklarına ve güvenlik önerilerinin en iyi şekilde iyileştireceğinizi öğrenin.
