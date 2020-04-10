---
title: Uygulama Öngörüleri için açıklama yayımla | Microsoft Dokümanlar
description: Uygulama Öngörüleri'ndeki metrik kaşif grafiklerinize dağıtım veya yapı işaretçileri ekleyin.
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: 0ad773ca6a7102ac718d43dfbbf6a4f834e681a0
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010755"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Uygulama Öngörüleri'nde metrik grafiklerdeki ek açıklamalar

Ek açıklamalar, yeni bir yapıyı veya diğer önemli olayları nereye dağıttığınızı gösterir. Ek açıklamalar, değişikliklerinizin uygulamanızın performansı üzerinde herhangi bir etkisi olup olmadığını görmenizi kolaylaştırır. [Azure Ardışık Hatları](https://docs.microsoft.com/azure/devops/pipelines/tasks/) oluşturma sistemi tarafından otomatik olarak oluşturulabilirler. PowerShell'den oluşturarak istediğiniz etkinliği işaretlemek için ek açıklamalar da oluşturabilirsiniz.

## <a name="release-annotations-with-azure-pipelines-build"></a>Azure Pipelines yapısıyla açıklama açıklamaları

Sürüm ek açıklamaları, Azure DevOps'un bulut tabanlı Azure Bulut Hatları hizmetinin bir özelliğidir.

### <a name="install-the-annotations-extension-one-time"></a>Ek Açıklamalar uzantısını yükleme (bir kez)

Sürüm ek açıklamaları oluşturabilmek için Visual Studio Marketplace'te bulunan birçok Azure DevOps uzantısından birini yüklemeniz gerekir.

1. [Azure DevOps](https://azure.microsoft.com/services/devops/) projenizde oturum açın.
   
1. Visual Studio Marketplace [Yayın Ek Açıklamaları uzantısı](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) sayfasında, Azure DevOps kuruluşunuzu seçin ve ardından uzantıyı Azure DevOps kuruluşunuza eklemek için **Yükle'yi** seçin.
   
   ![Bir Azure DevOps kuruluşunu seçin ve ardından Yükle'yi seçin.](./media/annotations/1-install.png)
   
Azure DevOps kuruluşunuz için uzantıyı yalnızca bir kez yüklemeniz gerekir. Artık kuruluşunuzdaki herhangi bir proje için sürüm ek açıklamalarını yapılandırabilirsiniz.

### <a name="configure-release-annotations"></a>Sürüm ek açıklamalarını yapılandırma

Azure Ardışık Hatları sürüm şablonlarınızın her biri için ayrı bir API anahtarı oluşturun.

1. [Azure portalında](https://portal.azure.com) oturum açın ve uygulamanızı izleyen Application Insights kaynağını açın. Veya yoksa, yeni bir [Application Insights kaynağı oluşturun.](../../azure-monitor/app/app-insights-overview.md)
   
1. **API Access sekmesini** açın ve **Application Insights ID'yi**kopyalayın.
   
   ![API Access altında, Uygulama Kimliğini kopyalayın.](./media/annotations/2-app-id.png)

1. Ayrı bir tarayıcı penceresinde, Azure Ardışık Hatları dağıtımlarınızı yöneten sürüm şablonunu açın veya oluşturun.
   
1. **Görev Ekle'yi**seçin ve ardından menüden **Uygulama Öngörüleri Yayın Açıklama ek ini** seçin.
   
   ![Görev Ekle'yi seçin ve Uygulama Öngörüleri Yayın Ek Açıklamasını seçin.](./media/annotations/3-add-task.png)

   > [!NOTE]
   > Sürüm Ek Açıklama görevi şu anda yalnızca Windows tabanlı aracıları destekler; Linux, macOS veya diğer türde aracılarda çalışmaz.
   
1. **Uygulama Kimliği**altında, **API Access** sekmesinden kopyaladığınız Uygulama Öngörüleri Kimliğini yapıştırın.
   
   ![Uygulama Öngörüleri Kimliğini Yapıştır](./media/annotations/4-paste-app-id.png)
   
1. Application Insights **API Access** penceresinde **API Anahtarı Oluştur'u**seçin. 
   
   ![API Access sekmesinde API Anahtarı Oluştur'u seçin.](./media/annotations/5-create-api-key.png)
   
1. **API oluştur anahtar** penceresinde, açıklama yazın, **ek açıklamalar yaz'ı**seçin ve ardından **Oluştur anahtarını**seçin. Yeni anahtarı kopyalayın.
   
   ![API oluştur anahtar penceresinde, açıklama yazın, ek açıklamalar yaz'ı seçin ve sonra Oluştur anahtarını seçin.](./media/annotations/6-create-api-key.png)
   
1. Serbest bırakma şablonu penceresinde, **Değişkenler** sekmesinde, yeni API anahtarı için değişken tanımı oluşturmak için **Ekle'yi** seçin.

1. **Ad**altında `ApiKey`, enter ve **Value**altında, **API Access** sekmesinden kopyalanmış API anahtarını yapıştırın.
   
   ![Azure DevOps Değişkenleri sekmesinde Ekle'yi seçin, değişken ApiKey'i adlandırın ve API anahtarını Değer'in altına yapıştırın.](./media/annotations/7-paste-api-key.png)
   
1. Şablonu kaydetmek için ana sürüm şablonu penceresinde **Kaydet'i** seçin.

## <a name="view-annotations"></a>Ek açıklamaları görüntüle


   > [!NOTE]
   > Sürüm ek açıklamaları şu anda Uygulama Öngörüleri Ölçümleri bölmesinde kullanılamıyor

Şimdi, yeni bir sürüm dağıtmak için sürüm şablonunu her kullandığınızda, Uygulama Öngörüleri'ne bir ek açıklama gönderilir. Ek açıklamalar aşağıdaki konumlarda görüntülenebilir:

Sürüm ek açıklamalarını el ile oluşturabilme özelliğine de sahip olduğunuz kullanım bölmesi:

![Saat belirli bir süre içinde görüntülenen kullanıcı ziyaretlerinin sayısıyla bar grafiğinin ekran görüntüsü. Sürüm ek açıklamaları, grafiğin üzerinde, bir sürümün oluştuğu zamanı gösteren yeşil onay işaretleri olarak görünür](./media/annotations/usage-pane.png)

Görselleştirmenin x ekseni boyunca zamanı gösterdiği günlük tabanlı çalışma kitabı sorgusunda.

![Çalışma kitapları bölmesinin ekran görüntüsü ve zaman serisi günlük tabanlı sorgu ve ek açıklamalar görüntülendi](./media/annotations/workbooks-annotations.png)

Çalışma kitabınızdaki ek açıklamaları etkinleştirmek için **Gelişmiş Ayarlar'a** gidin ve **ek açıklamaları göster'i**seçin.

![Gelişmiş Ayarlar menüsünün sözcüklerle birlikte ekran görüntüsü, etkinleştirmek için ayarın yanındaki onay işaretiyle vurgulanan ek açıklamaları gösterir.](./media/annotations/workbook-show-annotations.png)

İstekçi, kaynak denetim dalı, sürüm ardışık hattı ve ortam dahil olmak üzere sürümle ilgili ayrıntıları açmak için herhangi bir ek açıklama işaretçisini seçin.

## <a name="create-custom-annotations-from-powershell"></a>PowerShell'den özel ek açıklamalar oluşturun
Azure DevOps'leri kullanmadan istediğiniz işlemden ek açıklamalar oluşturmak için GitHub'dan [CreateReleaseAnnotation](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1) PowerShell komut dosyasını kullanabilirsiniz. 

1. [CreateReleaseAnnotation.ps1'in](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)yerel bir kopyasını oluşturun.
   
1. Application Insights ID'nizi almak ve Application Insights **API Access** sekmenizden bir API anahtarı oluşturmak için önceki yordamdaki adımları kullanın.
   
1. Açı parantezli yer tutucuları değerlerinizle değiştirerek PowerShell komut dosyasını aşağıdaki kodla çağırın. İsteğe `-releaseProperties` bağlı. 
   
   ```powershell
   
        .\CreateReleaseAnnotation.ps1 `
         -applicationId "<applicationId>" `
         -apiKey "<apiKey>" `
         -releaseName "<releaseName>" `
         -releaseProperties @{
             "ReleaseDescription"="<a description>";
             "TriggerBy"="<Your name>" }
   ```

Örneğin, geçmiş için ek açıklamalar oluşturmak için komut dosyasını değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [İş öğeleri oluşturma](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [PowerShell ile Automation](../../azure-monitor/app/powershell.md)
