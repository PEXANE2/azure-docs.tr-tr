---
title: Azure Uygulama Öngörülerini Anlık Görüntü Hata Ayıklama'yı Yükseltme
description: .NET uygulamaları için Snapshot Hata Ayıklayıcı'yı Azure Uygulama Hizmetleri'ndeki en son sürüme veya Nuget paketleri aracılığıyla yükseltme
ms.topic: conceptual
author: pharring
ms.author: pharring
ms.date: 03/28/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6991db6055ca8556adff67f194bac0f4aa04cb06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671401"
---
# <a name="upgrading-the-snapshot-debugger"></a>Anlık Görüntü Hata Ayıklama'yı Yükseltme

Verileriniz için mümkün olan en iyi güvenliği sağlamak için Microsoft, belirlenen saldırganlara karşı savunmasız olduğu gösterilen TLS 1.0 ve TLS 1.1'den uzaklaşıyor. Site uzantısının eski bir sürümünü kullanıyorsanız, çalışmaya devam etmek için bir yükseltme gerekir. Bu belge, Anlık Görüntü hata ayıklayıcınızı en son sürüme yükseltmek için gereken adımları özetler. Bir site uzantısı kullanarak Anlık Görüntü Hata Ayıklayıcı'yı etkinleştirip etkinleştirmediğinize veya uygulamanıza eklenen bir SDK/Nuget kullanıp kullanmadığınıza bağlı olarak iki birincil yükseltme yolu vardır. Her iki yükseltme yolu aşağıda açıklanmıştır. 

## <a name="upgrading-the-site-extension"></a>Site uzantısını yükseltme

> [!IMPORTANT]
> Application Insights'ın eski sürümlerinde _Azure Uygulama Hizmeti için Application Insights uzantısı_adı verilen özel bir site uzantısı kullanılmıştır. Geçerli Application Insights deneyimi, önceden yüklenmiş bir site uzantısını gün ışığına çıkarmak için Uygulama Ayarları ayarlayarak etkinleştirilir.
> Sitenizin çalışmasını durdurabilecek çakışmaları önlemek için önce özel site uzantısını silmeniz önemlidir. Aşağıdaki adım 4'e bakın.

Site uzantısını kullanarak Anlık Görüntü hata ayıklama özelliğini etkinleştirdiyseniz, aşağıdaki yordamı kullanarak yükseltme yapabilirsiniz:

1. Azure Portal’da oturum açın.
2. Uygulama Öngörüleri ve Anlık Görüntü hata ayıklama etkin olan kaynağınıza gidin. Örneğin, bir Web Uygulaması için Uygulama Hizmeti kaynağına gidin:

   ![DiagService01 adlı bireysel Uygulama Hizmeti kaynağının ekran görüntüsü](./media/snapshot-debugger-upgrade/app-service-resource.png)

3. Kaynağınıza geçtikten sonra Uzanlar bıçağını tıklatın ve uzantıların listesinin doldurulmasını bekleyin:

   ![Azure Uygulama Hizmeti için Uygulama Öngörüleri uzantısını gösteren Uygulama Hizmeti Uzantılarının ekran görüntüsü yüklendi](./media/snapshot-debugger-upgrade/application-insights-site-extension-to-be-deleted.png)

4. Azure Uygulama _Hizmeti için Application Insights uzantısının_ herhangi bir sürümü yüklenmişse, bu sürümü seçin ve Sil'i tıklatın. Uzantıyı silmek için **Evet'i** onaylayın ve bir sonraki adıma geçmeden önce silmenin tamamlanmasını bekleyin.

   ![Azure Uygulama Hizmeti için Uygulama Öngörüleri uzantısını gösteren Uygulama Hizmeti Uzantılarının ekran görüntüsü vurgulanan Sil düğmesiyle](./media/snapshot-debugger-upgrade/application-insights-site-extension-delete.png)

5. Kaynağınızın Genel Bakış bıçağına gidin ve Uygulama Öngörüleri'ni tıklayın:

   ![Üç düğmenin ekran görüntüsü. Ad Uygulama Öngörüleri ile Merkez düğmesi seçilir](./media/snapshot-debugger-upgrade/application-insights-button.png)

6. Bu Uygulama Hizmeti için Uygulama Öngörüleri bıçağını ilk kez görüntülediyseniz, Uygulama Öngörüleri'ni açmanız istenir. **Uygulama Öngörülerini Aç'ı**seçin.
 
   ![Uygulama Öngörüleri Açma düğmesi vurgulanan Application Insights bıçak için ilk kez deneyim ekran görüntüsü](./media/snapshot-debugger-upgrade/turn-on-application-insights.png)

7. Geçerli Uygulama Öngörüleri ayarları görüntülenir. Ayarlarınızı değiştirme fırsatını fırsatını bulmak istemiyorsanız, ayarlarınızı olduğu gibi bırakabilirsiniz. Bıçağın altındaki **Uygula** düğmesi varsayılan olarak etkinleştirmez ve düğmeyi etkinleştirmek için ayarlardan birini değiştirmeniz gerekir. Gerçek ayarları değiştirmeniz gerekmez, bunun yerine ayarı değiştirebilir ve hemen değiştirebilirsiniz. Profiler ayarını değiştirip **uygula'yı**seçmenizi öneririz.

   ![Uygulama Öngörüleri Uygulama Hizmeti Yapılandırma sayfasının ekran görüntüsü kırmızı ile vurgulanan Uygula düğmesi ile](./media/snapshot-debugger-upgrade/view-application-insights-data.png)

8. **Uygula'yı**tıklattığınızda değişiklikleri onaylamanız istenir.

    > [!NOTE]
    > Site yükseltme işleminin bir parçası olarak yeniden başlatılacaktır.

   ![App Service'in uygulama izleme isteminin ekran görüntüsü. Metin kutusu mesajı görüntüler: "Artık uygulama ayarlarınızda değişiklikler uygulayacağız ve Uygulama Öngörüleri kaynağınızı web uygulamasına bağlamak için araçlarımızı yükleyeceğiz. Bu, siteyi yeniden başlatacaktır. Devam etmek istiyor musun?"](./media/snapshot-debugger-upgrade/apply-monitoring-settings.png)

9. Değişiklikleri uygulamak ve işlemin tamamlanmasını beklemek için **Evet'i** tıklatın.

Site şimdi yükseltildi ve kullanıma hazır.

## <a name="upgrading-snapshot-debugger-using-sdknuget"></a>SDK/Nuget kullanarak Anlık Görüntü Hata Ayıklama Yükseltme

Uygulama sürüm 1.3.1'in `Microsoft.ApplicationInsights.SnapshotCollector` altında bir sürümünü kullanıyorsa, çalışmaya devam etmek için daha yeni bir [sürüme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) yükseltilmesi gerekir.
