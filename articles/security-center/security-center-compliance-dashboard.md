---
title: 'Öğretici: Mevzuata uygunluk denetimleri - Azure Güvenlik Merkezi'
description: "Öğretici: Azure Güvenlik Merkezi'ni kullanarak mevzuata uygunluğunuzu nasıl geliştireceğinizi öğrenin."
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
ms.date: 11/12/2019
ms.author: memildin
ms.openlocfilehash: 1a6999c05c0b3dbaf572b376412f666c50c23df7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77604444"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Öğretici: Mevzuata uygunluğunuzu geliştirin
---

Azure Güvenlik Merkezi, **düzenleyici uyumluluk panosunu**kullanarak düzenleyici uyumluluk gereksinimlerini karşılama işlemini kolaylaştırmanıza yardımcı olur. Güvenlik Merkezi, panoda Azure ortamınızın sürekli değerlendirmelerine dayalı olarak uyumluluk duruşunuzla ilgili öngörüler sağlar. Güvenlik Merkezi, karma bulut ortamınızdaki risk faktörlerini güvenlik en iyi uygulamalarına göre analiz eder. Bu değerlendirmeler, desteklenen bir standart kümesinden uyumluluk denetimlerine eşlenir. Mevzuata uygunluk panosunda, ortamınızdaki tüm değerlendirmelerin durumunu belirli bir standart veya yönetmelik bağlamında görebilirsiniz. Önerilere göre hareket ettikçe ve çevrenizdeki risk faktörlerini azalttıkça, uyumluluk duruşunuz iyileşir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

-   Mevzuata uygunluk panosunu kullanarak mevzuata uygunluğunuzu değerlendirin

-   Öneriler üzerinde harekete geçerek uyumluluk duruşunuzu geliştirin

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide yer alan özelliklere adım atmak için Security Center'ın Standart fiyatlandırma katmanına sahip olmalısınız. Güvenlik Merkezi Standard'ı ücretsiz olarak deneyebilirsiniz.
Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](https://docs.microsoft.com/azure/security-center/security-center-get-started) başlıklı hızlı başlangıçta Standart katmanına nasıl yükseltebileceğiniz adım adım açıklanmıştır.

##  <a name="assess-your-regulatory-compliance"></a>Mevzuata uygunluğunuzu değerlendirin

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını tanımlamak için kaynaklarınızın yapılandırmasını sürekli olarak değerlendirir. Bu değerlendirmeler, güvenlik hijyeninizi iyileştirmeye odaklanan öneriler olarak sunulur. Mevzuata uygunluk panosunda, desteklenen gereksinimlerin geçerli güvenlik değerlendirmelerine eşlendiği tüm gereksinimleriyle uyumluluk standartları kümesini görüntüleyebilirsiniz. Bu, bu değerlendirmelerin durumuna bağlı olarak standart la ilgili uyumluluk duruşunuzu görüntülemenizi sağlar.

Mevzuata uygunluk panosu görünümü, dikkatinizi sizin için önemli olan bir standart veya düzenlemeye uygun olarak boşluklara odaklamanıza yardımcı olabilir. Bu odaklı görünüm, dinamik bulut ve karma ortamlarda zaman içinde uyumluluk puanınızı sürekli olarak izlemenize olanak tanır.

>[!NOTE]
> Güvenlik Merkezi varsayılan olarak aşağıdaki düzenleyici standartları destekler: Azure BDT, PCI DSS 3.2, ISO 27001 ve SOC TSP. 
>
> [Dinamik uyumluluk paketleri (önizleme)](update-regulatory-compliance-packages.md) özelliği, düzenleyici uyumluluk panonuzda gösterilen standartları yeni *dinamik* paketlere yükseltmenize olanak tanır. Ayrıca, yeni uyumluluk paketleri eklemek ve ek standartlarla uyumluluğunuzuzun izlenmesi için de aynı önizleme özelliğini kullanabilirsiniz. 

1.  Güvenlik Merkezi ana menüsünde, **POLİtİkA & UYUMLULUK** altında **Mevzuata uygunluğu**seçin. <br>
Ekranın üst kısmında, desteklenen uyumluluk yönetmelikleri kümesiyle uyumluluk durumunuza genel bir bakış içeren bir pano görürsünüz. Genel uyumluluk puanınızı ve her standartla ilişkili başarısız değerlendirmelere karşı geçen değerlendirme sayısını görebilirsiniz.

    ![bilgisayar açıklaması yüksek güven](./media/security-center-compliance-dashboard/compliance-dashboard.png)

2.  Size uygun uyumluluk standardı için bir sekme seçin. Söz konusu standartla ilgili tüm denetimleri göreceksiniz. İlgili denetimler için, söz konusu denetimle ilişkilendirilmiş başarılı ve başarısız değerlendirmelerin ayrıntılarını görüntüleyebilirsiniz. Bazı denetimler gri renkte. Bu denetimlerde bunlarla ilişkili herhangi bir Güvenlik Merkezi değerlendirmesi yoktur. Bunların gereksinimlerini kontrol edin ve bunları çevrenizde kendi ortamınızda değerlendirin. Bunlardan bazıları süreçle ilgili olabilir ve teknik olmayabilir.

    ![uyumluluk sekmesi](./media/security-center-compliance-dashboard/compliance-pci.png)

1. Belirli bir standart için geçerli uyumluluk durumunuzu özetleyen bir PDF raporu oluşturmak ve indirmek için **Raporu İndir'i**tıklatın.

    Rapor, Güvenlik Merkezi değerlendirme verilerine dayalı olarak seçilen standart için uyumluluk durumunuzun üst düzey bir özetini sağlar ve bu standardın denetimlerine göre düzenlenir. Rapor ilgili paydaşlarla paylaşılabilir ve iç ve dış denetçilere kanıt sağlamaya hizmet edebilir.

    ![indirin](./media/security-center-compliance-dashboard/download-report.png)

## <a name="improve-your-compliance-posture"></a>Uyumluluk duruşunuzu geliştirin

Mevzuata uygunluk panosundaki bilgiler göz önüne alındığında, önerileri doğrudan pano içinde çözerek uyumluluk duruşunuzu geliştirebilirsiniz.

1.  Bu önerinin ayrıntılarını görüntülemek için panoda görünen başarısız değerlendirmelerden herhangi birini tıklatın. Her öneri, sorunu gidermek için izlenmesi gereken bir düzeltme adımları kümesi içerir.

1.  Daha fazla ayrıntı görüntülemek ve bu kaynak için öneriyi gidermek için belirli bir kaynak seçebilirsiniz. <br>Örneğin, Azure **BDT standart** sekmesinde, depolama **hesabına güvenli aktarım yapılmasını gerektir**önerisini tıklatabilirsiniz.

    ![uyumluluk önerisi](./media/security-center-compliance-dashboard/compliance-recommendation.png)

1. Öneri bilgilerini tıklatTığınızda ve sağlıksız bir kaynak seçtiğinizde, doğrudan Azure portalı içinde **güvenli depolama aktarımı** etkinleştirme deneyimine yönlendirir.

    Önerilerin nasıl uygulanacağı hakkında daha fazla bilgi için [bkz.](security-center-recommendations.md)

    ![uyumluluk önerisi](./media/security-center-compliance-dashboard/compliance-remediate-recommendation.png)

1.  Önerileri çözmek için harekete geçtikten sonra, uyumluluk puanınız iyileşir, çünkü uyumluluk panosu raporunda etkisini görürsünüz.

    > [!NOTE]
    > Değerlendirmeler yaklaşık olarak 12 saatte bir çalıştırılır, dolayısıyla uyumluluk verileriniz üzerindeki etkisini ancak değerlendirmeler çalıştırıldıktan sonra görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Güvenlik Merkezi'nin Mevzuata uygunluk panosunun şu şekilde kullanılmasını öğrendiniz:

-   Sizin için önemli olan standartlara ve düzenlemelere göre uyumluluk duruşunuzu görüntüleyin ve izleyin.

-   İlgili önerileri çözerek ve uyumluluk puanının iyileşmesini izleyerek uyumluluk durumunuzu iyileştirin.

Mevzuata uygunluk panosu uyumluluk işlemini büyük ölçüde basitleştirebilir ve Azure ve karma ortamınız için uyumluluk kanıtı toplamak için gereken süreyi önemli ölçüde azaltabilir.

Daha fazla bilgi için bkz:

-   [Mevzuata uygunluk panonuzdaki dinamik uyumluluk paketlerine güncelleştirin (Önizleme)](update-regulatory-compliance-packages.md) - Mevzuata uygunluk panonuzda gösterilen standartları yeni *dinamik* paketlere güncellemenize olanak tanıyan bu önizleme özelliği hakkında bilgi edinin. Ayrıca, yeni uyumluluk paketleri eklemek ve ek standartlarla uyumluluğunuzuzun izlenmesi için aynı önizleme özelliğini de ekleyebilirsiniz. 

-   [Azure Güvenlik Merkezi'nde güvenlik durumu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.

-   [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Azure kaynaklarınızı korumaya yardımcı olmak için Azure Güvenlik Merkezi'nde önerileri nasıl kullanacağınızı öğrenin.

-   [Azure Güvenlik Merkezi'nde Güvenli Puanınızı geliştirin](security-center-secure-score.md) - Güvenlik duruşunuzu en iyi şekilde geliştirmek için güvenlik açıklarına ve güvenlik önerilerine nasıl öncelik verin öğrenin.