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
ms.date: 02/04/2021
ms.author: memildin
ms.openlocfilehash: 20a464011e5a8d37a6215b222323ca989e02ac04
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550928"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Öğretici: Mevzuat uyumluluğunuzu artırma

Azure Güvenlik Merkezi, mevzuat **Uyumluluk panosunu** kullanarak, mevzuat uyumluluk gereksinimlerini karşılamaya yönelik işlemin kolaylaştırılmasına yardımcı olur. 

Güvenlik Merkezi, aboneliklerinize uygulanan standartların denetimlerine ve en iyi uygulamalarına göre risk faktörlerini analiz etmek için karma bulut ortamınızı sürekli değerlendirir. Pano, bu standartlarla uyumluluğun durumunu yansıtır. 

Azure aboneliğinde Güvenlik Merkezi 'ni etkinleştirdiğinizde, [Azure Güvenlik kıyaslaması](../security/benchmarks/introduction.md)otomatik olarak atanır. Bu, yaygın olarak kullanılan kıyaslama, [Internet güvenliği (CIS) Için merkezden](https://www.cisecurity.org/benchmark/azure/) ve [ulusal standartlar ve teknolojı Enstitüsü (NIST)](https://www.nist.gov/) ile bulut merkezli güvenliğe odaklanarak yapılar oluşturur.

Mevzuat uyumluluk panosu, ortamınızdaki tüm değerlendirmelerin durumunu seçtiğiniz standartlar ve yönetmeliklerinize göre gösterir. Önerilerle ilgili işlem yaparken ve ortamınızdaki risk etmenlerini azaltmanız durumunda uyumluluk durmanız artar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Mevzuat uyumluluk panosunu kullanarak mevzuata uyumluluğunuzu değerlendirin
> * Önerilerle ilgili eylem gerçekleştirerek uyumluluk durunuzu geliştirme
> * Uyumluluk sonrasında yapılan değişiklikler üzerinde uyarı kurulumu
> * Uyumluluk verilerinizi sürekli bir akış olarak ve haftalık anlık görüntüler olarak dışa aktarın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide ele alınan özellikler arasında ilerlemek için:

- [Azure Defender](azure-defender.md) etkin olmalıdır. Azure Defender 'ı 30 gün boyunca ücretsiz olarak deneyebilirsiniz.
- İlke uyumluluk verilerine okuyucu erişimi olan bir hesapla oturum açmanız gerekir (**güvenlik okuyucusu** yeterli değildir). Abonelik için **Genel okuyucu** rolü çalışacaktır. En azından, **kaynak Ilkesi katılımcısı** ve **Güvenlik yönetici** rollerinin atanması gerekir.

##  <a name="assess-your-regulatory-compliance"></a>Mevzuata uyumluluğunuzu değerlendirin

Yasal uyumluluk panosu, seçilen uyumluluk standartlarınızı, desteklenen gereksinimlerin ilgili güvenlik değerlendirmelerine eşlendiği tüm gereksinimleriyle birlikte gösterir. Bu değerlendirmelerin durumu, standart ile uyumluluğunu yansıtır.

Seçtiğiniz standartlar ve yönetmeliklerle uyumlu olan boşlukların dikkatine odaklanmaya yardımcı olması için mevzuat uyumluluk panosunu kullanın. Bu odaklanmış görünüm, dinamik bulut ve karma ortamlarda zaman içinde uyumluluğunuzu sürekli olarak izlemenize de olanak sağlar.

1. Güvenlik Merkezi menüsünde, **mevzuat uyumluluğu**' nı seçin.

    Ekranın üst kısmında, desteklenen uyumluluk düzenlemeleri kümesiyle uyumluluk durumunuz hakkında genel bakış içeren bir panodur. Genel uyumluluk puanınızı ve her bir standart ile ilişkili geçen ve başarısız değerlendirmelerin sayısını görürsünüz.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="Mevzuat uyumluluk panosu" lightbox="./media/security-center-compliance-dashboard/compliance-dashboard.png":::

1. Uyumluluk standardı için sizin için uygun bir sekme seçin (1). Standart (2) üzerinde hangi aboneliklerin uygulandığını ve bu standart (3) için tüm denetimlerin listesini görürsünüz. İlgili denetimler için, bu denetimle ilişkili geçen ve başarısız değerlendirmelerin ayrıntılarını (4) ve etkilenen kaynakların sayısını (5) görüntüleyebilirsiniz. Bazı denetimler gri renkte. Bu denetimlere kendileriyle ilişkili güvenlik merkezi değerlendirmeleri yok. Bunlarla ilgili gereksinimleri kontrol edin ve kendi ortamınızda kendi ortamınızda değerlendirin. Bunlardan bazıları teknik değil işlem ile ilgili olabilir.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="Belirli bir standartta uyumluluk ayrıntılarını keşfetme":::

1. Belirli bir standart için geçerli uyumluluk durumunuzu Özetle bir PDF raporu oluşturmak için, **raporu indir**' i seçin.

    Rapor, güvenlik merkezi değerlendirme verilerine dayanarak seçili standart için uyumluluk durumunuzu üst düzey bir Özet sağlar ve söz konusu standart denetimlere göre düzenlenmiştir. Rapor ilgili hissedarlarla paylaşılabilir ve iç ve dış denetçiler için kanıt sağlayabilir.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="Uyumluluk raporunu indir":::

## <a name="improve-your-compliance-posture"></a>Uyumluluk durunuzu geliştirme

Yasal uyumluluk panosundaki bilgileri kullanarak, önerileri doğrudan pano içinde çözerek uyumluluk durunuzu geliştirebilirsiniz.

1.  Söz konusu öneriye ilişkin ayrıntıları görüntülemek için Panoda görünen başarısız değerlendirmelerin herhangi birine tıklayın. Her öneri, sorunu çözmek için izlenmesi gereken bir düzeltme adımları kümesi içerir.

1.  Daha fazla ayrıntı görüntülemek ve söz konusu kaynak için öneriyi çözümlemek üzere belirli bir kaynağı seçin. <br>Örneğin, **Azure CIS 1.1.0** Standard 'da, **sanal makinelere uygulanacak öneri disk şifrelemesi**' ni seçin.

    :::image type="content" source="./media/security-center-compliance-dashboard/sample-recommendation.png" alt-text="Standart Müşteri adaylarından öneri seçme, doğrudan öneri ayrıntıları sayfasına":::

1. Bu örnekte, öneri ayrıntıları sayfasında **eylemi al** ' ı seçtiğinizde, Azure Portal Azure sanal makine sayfalarına geldiğinizde **güvenlik** sekmesinden şifrelemeyi etkinleştirebilirsiniz:

    :::image type="content" source="./media/security-center-compliance-dashboard/encrypting-vm-disks.png" alt-text="Öneri ayrıntıları sayfasında eylem Al düğmesi düzeltme seçeneklerine yol açar":::

    Önerilerin nasıl uygulanacağı hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

1.  Önerileri çözümlemek için eylem gerçekleştirdikten sonra uyumluluk puanınız artdığından uyumluluk panosu raporundaki etkiyi görürsünüz.

    > [!NOTE]
    > Değerlendirmeler yaklaşık 12 saatte bir çalışır, bu nedenle yalnızca ilgili değerlendirmenin bir sonraki çalıştırıldıktan sonra uyumluluk verilerinize etkisini görürsünüz.


## <a name="export-your-compliance-status-data"></a>Uyumluluk durumu verilerinizi dışarı aktarma

Ortamınızdaki diğer izleme araçlarıyla uyumluluk durumunuzu izlemek isterseniz, güvenlik merkezi bunu basit hale getirmek için bir dışarı aktarma mekanizması içerir. Azure Olay Hub 'ına veya bir Log Analytics çalışma alanına seçim verileri göndermek için **sürekli dışarı aktarmayı** yapılandırın.

Azure Olay Hub 'ına veya Log Analytics çalışma alanına sürekli veri aktarma kullanın:

- **Sürekli bir akışta** tüm yasal uyumluluk verilerini dışarı aktar:

    :::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-stream.png" alt-text="Yasal bir uyumluluk verileri akışını sürekli dışa aktarma" lightbox="media/security-center-compliance-dashboard/export-compliance-data-stream.png":::

- Yasal uyumluluk verilerinizin **haftalık anlık görüntülerini** dışarı aktarın:

    :::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-snapshot.png" alt-text="Mevzuat uyumluluk verilerinin haftalık anlık görüntüsünü sürekli dışa aktarma" lightbox="media/security-center-compliance-dashboard/export-compliance-data-snapshot.png":::

Ayrıca, uyum verilerinizin **PDF/CSV raporunu** doğrudan mevzuata uyumluluk panosundan dışarı aktarabilirsiniz:

:::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-report.png" alt-text="Yasal uyumluluk verilerinizi PDF veya CSV raporu olarak dışa aktarma" lightbox="media/security-center-compliance-dashboard/export-compliance-data-report.png":::

[Güvenlik Merkezi verilerini sürekli dışa aktarma](continuous-export.md)hakkında daha fazla bilgi edinin.


## <a name="run-workflow-automations-when-there-are-changes-to-your-compliance"></a>Uyumluluğa değişiklik yapıldığında iş akışı otomatiklemeleri çalıştırın

Güvenlik Merkezi 'nin iş akışı Otomasyonu özelliği, mevzuat uyumluluk değerlendirmelerinden biri değiştiğinde Logic Apps tetikleyebilir.

Örneğin, bir uyumluluk değerlendirmesi başarısız olduğunda güvenlik merkezi 'nin belirli bir kullanıcıyı e-posta ile postasını isteyebilirsiniz. Öncelikle Logic App ( [Azure Logic Apps](../logic-apps/logic-apps-overview.md)kullanarak) oluşturmanız ve ardından tetikleyiciyi [Güvenlik Merkezi tetikleyicilerine otomatik hale](workflow-automation.md)getirme bölümünde açıklandığı gibi yeni bir iş akışı otomasyonunda ayarlamanız gerekir.

:::image type="content" source="media/release-notes/regulatory-compliance-triggers-workflow-automation.png" alt-text="Bir iş akışı Otomasyonu tetiklemek için mevzuat uyumluluk değerlendirmelerinde yapılan değişiklikleri kullanma" lightbox="media/release-notes/regulatory-compliance-triggers-workflow-automation.png":::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, güvenlik merkezi 'nin mevzuat uyumluluk panosunu şu şekilde kullanma hakkında bilgi edindiniz:

- Sizin için önemli olan standartlar ve yönetmeliklerle ilgili uyumluluk durunuzu görüntüleyin ve izleyin.
- İlgili önerileri çözümleyerek ve Uyumluluk puanı iyileştirerek uyumluluğu izleyerek uyumluluk durumunuzu geliştirebilirsiniz.

Mevzuat uyumluluk panosu uyumluluk sürecini büyük ölçüde basitleştirebilir ve Azure, hibrit ve çok bulut ortamınız için uyumluluk kanıtını toplamak için gereken süreyi önemli ölçüde kesebilir.

Daha fazla bilgi edinmek için bu ilgili sayfalara bakın:

- [Yasal uyumluluk panonuzda standartlar kümesini özelleştirme](update-regulatory-compliance-packages.md) -yasal uyumluluk panonuzda hangi standartların görüneceğini seçme hakkında bilgi edinin. 
- [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
- [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -Azure kaynaklarınızın korunmasına yardımcı olmak Için Azure Güvenlik Merkezi 'nde önerilerin nasıl kullanılacağını öğrenin.