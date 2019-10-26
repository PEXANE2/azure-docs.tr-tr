---
title: .NET uygulamaları için Azure Application Insights Snapshot Debugger yükseltmesi | Microsoft Docs
description: Snapshot Debugger Azure Uygulama Hizmetleri veya NuGet paketleri aracılığıyla en son sürüme yükseltme
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: MarioHewardt
ms.author: marioh
ms.date: 03/28/2019
ms.reviewer: mbullwin
ms.openlocfilehash: e2b21b7cbb6b04da0c93e73c0cacb8a05c338bde
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72899846"
---
# <a name="upgrading-the-snapshot-debugger"></a>Snapshot Debugger yükseltiliyor

Verileriniz için mümkün olan en iyi güvenliği sağlamak üzere Microsoft, belirlenen saldırganlar tarafından açık olarak gösterilen TLS 1,0 ve TLS 1,1 ' den uzaklaşmakta. Site uzantısının daha eski bir sürümünü kullanıyorsanız, çalışmaya devam etmek için yükseltmenin yapılması gerekir. Bu belgede, anlık görüntü hata ayıklayıcıyı en son sürüme yükseltmek için gereken adımlar özetlenmektedir. Snapshot Debugger bir site uzantısı kullanarak veya uygulamanıza eklenmiş bir SDK/NuGet kullandıysanız, bu iki birincil yükseltme yolu vardır. Her iki yükseltme yolu da aşağıda ele alınmıştır. 

## <a name="upgrading-the-site-extension"></a>Site uzantısını yükseltme

Site uzantısını kullanarak anlık görüntü hata ayıklayıcısını etkinleştirdiyseniz, aşağıdaki yordamı kullanarak kolayca yükseltebilirsiniz:

1. Azure Portal’da oturum açın.
2. Application Insights ve anlık görüntü hata ayıklayıcısı etkin olan kaynağınız için gidin. Örneğin, bir Web uygulaması için App Service kaynağına gidin:

   ![DiagService01 adlı tek bir App Service kaynağının ekran görüntüsü](./media/snapshot-debugger-upgrade/app-service-resource.png)

3. Kaynak üzerinde gezindikten sonra genel bakış dikey penceresinde Application Insights ' ye tıklayın:

   ![Üç düğme ekran görüntüsü. Application Insights adlı Merkez düğmesi seçili](./media/snapshot-debugger-upgrade/application-insights-button.png)

4. Geçerli ayarlarla yeni bir dikey pencere açılır. Ayarlarınızı değiştirme fırsatı almak istemediğiniz sürece, bunları olduğu gibi bırakabilirsiniz. Dikey pencerenin alt kısmındaki **Uygula** düğmesi varsayılan olarak etkinleştirilmemiştir ve bu düğmeyi etkinleştirmek için ayarlardan birine geçiş yapmanız gerekir. Gerçek ayarları değiştirmeniz gerekmez, bunun yerine ayarı değiştirebilir ve sonra hemen değiştirebilirsiniz. Profil Oluşturucu ayarını ve ardından **Uygula**' yı seçmeniz önerilir.

   ![Application Insights App Service yapılandırma sayfasının kırmızı renkle vurgulanmış şekilde ekran görüntüsü](./media/snapshot-debugger-upgrade/view-application-insights-data.png)

5. **Uygula**' ya tıkladığınızda, değişiklikleri onaylamanız istenir.

    > [!NOTE]
    > Site, yükseltme işleminin bir parçası olarak yeniden başlatılacak.

   ![App Service izleme isteminin ekran görüntüsü. Metin kutusu şu iletiyi görüntüler: "Şimdi uygulama ayarlarınıza değişiklikler uygulayacağız ve Application Insights kaynağınızı Web uygulamasına bağlamak için araçlarımızı yükleyeceğiz. Bu, siteyi yeniden başlatacak. Devam etmek istiyor musunuz? "](./media/snapshot-debugger-upgrade/apply-monitoring-settings.png)

6. Değişiklikleri uygulamak için **Evet** ' i tıklatın. İşlem sırasında değişikliklerin uygulandığını gösteren bir bildirim görüntülenir:

   ![Değişiklikleri Uygula-sağ üst köşede görüntülenen uzantıları güncelleştirme iletisi ekran görüntüsü](./media/snapshot-debugger-upgrade/updating-extensions.png)

İşlem tamamlandığında, **"değişiklikler uygulandı"** bildirimi görüntülenir.

   ![Değişikliklerin uygulandığını belirten ileti ekran görüntüsü](./media/snapshot-debugger-upgrade/changes-are-applied.png)

Site artık yükseltildi ve kullanıma hazır.

## <a name="upgrading-snapshot-debugger-using-sdknuget"></a>SDK/NuGet kullanarak Snapshot Debugger yükseltme

Uygulama, 1.3.1 sürümünün `Microsoft.ApplicationInsights.SnapshotCollector` bir sürümünü kullanıyorsa, çalışmaya devam etmek için [daha yeni bir sürüme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) yükseltilmesi gerekir.
