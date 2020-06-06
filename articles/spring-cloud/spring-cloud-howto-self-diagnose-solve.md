---
title: Azure Spring Cloud 'da sorunları kendi kendine tanılama ve çözme
description: Azure Spring Cloud 'daki sorunları kendi kendine tanılamayı ve çözmeyi öğrenin.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 05/29/2020
ms.openlocfilehash: 581d77a5568b9533d85ff476b63001c8a237bc4e
ms.sourcegitcommit: ba8df8424d73c8c4ac43602678dae4273af8b336
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84457210"
---
# <a name="how-to-self-diagnose-and-solve-problems-in-azure-spring-cloud"></a>Azure Spring Cloud 'da sorunları kendi kendine tanılama ve çözme
Azure yay bulutu Tanılaması, uygulamanızın sorunlarını gidermenize yardımcı olacak etkileşimli bir deneyimdir. Yapılandırma gerekmez. Sorunları bulduğunuzda, Azure yay Bulut Tanılaması, nelerin yanlış olduğunu gösterir ve sorunu gidermeye ve sorunu çözmeye yardımcı olan bilgilere kılavuzluk eder.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Dağıtılmış bir Azure yay bulut hizmeti örneği. Kullanmaya başlamak için [Azure CLI aracılığıyla uygulama dağıtmaya yönelik hızlı başlangıç hızımızı](spring-cloud-quickstart-launch-app-cli.md) izleyin.
* Hizmet örneğiniz için en az bir uygulama zaten oluşturulmuş.

## <a name="navigate-to-the-diagnostics-page"></a>Tanılama sayfasına gidin
1. Azure Portal’da oturum açın.
2. Azure yay bulutuna **genel bakış** sayfasına gidin.
3. Sayfanın sol tarafındaki menüden **Tanıla ve çöz sorunlarını** açın.

 ![Tanıla, Çöz iletişim kutusu](media/spring-cloud-diagnose/diagnose-solve-dialog.png)

 ## <a name="search-logged-issues"></a>Günlüğe kaydedilen sorunları ara
Bir sorunu bulmak için, bir anahtar sözcük yazarak veya çözüm grubu ' na tıklayarak bu kategoride araştırma yapabilirsiniz.

 ![Arama sorunları](media/spring-cloud-diagnose/search-detectors.png)

Yapılandırma **sunucusu durum denetimi**, **yapılandırma sunucusu sistem**durumu veya **yapılandırma sunucusu güncelleştirme geçmişi** seçimi çeşitli sonuçlar görüntüler.

![Sorun seçenekleri](media/spring-cloud-diagnose/detectors-options.png)

Hedef algılayıcısının bulun ve yürütmek için tıklayın. Algılayıcısının yürütülmesi gerçekleştirildikten sonra tanılama özeti gösterilir. Algılayıcı listesine geri dönmek için tanılama ayrıntılarını denetlemek üzere **tam raporu görüntüle** ' yi veya **kutucuk menüsü düğmesini göster** ' i seçin.

 ![Özet tanılama](media/spring-cloud-diagnose/summary-diagnostics.png)

Tanılama ayrıntıları sayfasında, tanılama zaman aralığını denetleyici ile sağ üst köşedeki şekilde değiştirebilirsiniz. Daha fazla ölçüm veya günlük görmek için her bir tanılamayı değiştirin. Ölçümler ve Günlükler için 15 dakikalık bir gecikme olabilir.

 ![Tanılama ayrıntıları](media/spring-cloud-diagnose/diagnostics-details.png)

Bazı sonuçlar ilgili belgeleri içerir.

 ![İlgili ayrıntılar](media/spring-cloud-diagnose/related-details.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Uyarıları ve eylem gruplarını kullanarak Spring Cloud kaynaklarını izleme](spring-cloud-tutorial-alerts-action-groups.md)
* [Azure Spring Cloud Hizmeti için güvenlik denetimleri](spring-cloud-concept-security-controls.md)