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
ms.date: 01/28/2021
ms.author: memildin
ms.openlocfilehash: f8d92ff0835948637761d7d2a98ec95a1c6dfccd
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944246"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Öğretici: Mevzuat uyumluluğunuzu artırma

Azure Güvenlik Merkezi, mevzuat **Uyumluluk panosunu** kullanarak, mevzuat uyumluluk gereksinimlerini karşılamaya yönelik işlemin kolaylaştırılmasına yardımcı olur. 

Güvenlik Merkezi, aboneliklerinize uygulanan standartların denetimlerine ve en iyi uygulamalarına göre risk faktörlerini çözümlemek için karma bulut ortamınızın sürekli değerlendirmelerini gerçekleştirir. Pano, bu standartlarla uyumluluğun durumunu yansıtır. 

Azure aboneliğinde Güvenlik Merkezi 'ni etkinleştirdiğinizde, [Azure Güvenlik kıyaslaması](../security/benchmarks/introduction.md)otomatik olarak atanır. Bu, yaygın olarak kullanılan kıyaslama, [Internet güvenliği (CIS) Için merkezden](https://www.cisecurity.org/benchmark/azure/) ve [ulusal standartlar ve teknolojı Enstitüsü (NIST)](https://www.nist.gov/) ile bulut merkezli güvenliğe odaklanarak yapılar oluşturur.

Mevzuat uyumluluk panosunda, ortamınızdaki tüm değerlendirmelerin durumunu belirli bir standart veya yönetmeliği bağlamında görebilirsiniz. Önerilerle ilgili işlem yaparken ve ortamınızdaki risk etmenlerini azaltmanız durumunda uyumluluk durmanız artar.

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

Yasal uyumluluk panosu, seçilen uyumluluk standartlarınızı, desteklenen gereksinimlerin ilgili güvenlik değerlendirmelerine eşlendiği tüm gereksinimleriyle birlikte gösterir. Bu değerlendirmelerin durumu, standart ile uyumluluğunu yansıtır.

Sizin için gereken standartlar ve yönetmeliklerle uyumlu olan boşlukların dikkatine odaklanmaya yardımcı olması için mevzuat uyumluluk panosunu kullanın. Bu odaklanmış görünüm, dinamik bulut ve karma ortamlarda zaman içinde uyumluluğunuzu sürekli olarak izlemenize de olanak sağlar.

1. Güvenlik Merkezi menüsünde, **mevzuat uyumluluğu**' nı seçin.

    Ekranın üst kısmında, desteklenen uyumluluk düzenlemeleri kümesiyle uyumluluk durumunuz hakkında genel bakış içeren bir panodur. Genel uyumluluk puanınızı ve her bir standart ile ilişkili geçiş ve başarısız değerlendirmelerin sayısını görebilirsiniz.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="Mevzuat uyumluluk panosu":::

1. Uyumluluk standardı için sizin için uygun bir sekme seçin (1). Standart (2) üzerinde hangi aboneliklerin uygulandığını ve bu standart (3) için tüm denetimlerin listesini görürsünüz. İlgili denetimler için, bu denetimle ilişkili geçen ve başarısız değerlendirmelerin ayrıntılarını (4) ve etkilenen kaynakların sayısını (5) görüntüleyebilirsiniz. Bazı denetimler gri renkte. Bu denetimlere kendileriyle ilişkili güvenlik merkezi değerlendirmeleri yok. Bunlarla ilgili gereksinimleri kontrol edin ve kendi ortamınızda kendi ortamınızda değerlendirin. Bunlardan bazıları teknik değil işlem ile ilgili olabilir.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="Belirli bir standartta uyumluluk ayrıntılarını keşfetme":::

1. Belirli bir standart için geçerli uyumluluk durumunuzu Özetle bir PDF raporu oluşturmak için, **raporu indir**' i seçin.

    Rapor, güvenlik merkezi değerlendirme verilerine dayanarak seçili standart için uyumluluk durumunuzu üst düzey bir Özet sağlar ve söz konusu standart denetimlere göre düzenlenmiştir. Rapor ilgili hissedarlarla paylaşılabilir ve iç ve dış denetçiler için kanıt sağlayabilir.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="Uyumluluk raporunu indir":::

## <a name="improve-your-compliance-posture"></a>Uyumluluk durunuzu geliştirme

Yasal uyumluluk panosundaki bilgiler verildiğinde, önerileri doğrudan pano içinde çözerek uyumluluk durunuzu geliştirebilirsiniz.

1.  Söz konusu öneriye ilişkin ayrıntıları görüntülemek için Panoda görünen başarısız değerlendirmelerin herhangi birine tıklayın. Her öneri, sorunu çözmek için izlenmesi gereken bir düzeltme adımları kümesi içerir.

1.  Daha fazla ayrıntı görüntülemek ve söz konusu kaynak için öneriyi çözümlemek üzere belirli bir kaynağı seçebilirsiniz. <br>Örneğin, **Azure CIS 1.1.0** Standard 'da, **sanal makinelere uygulanacak öneri disk şifrelemesi** seçeneğini belirleyebilirsiniz.

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
