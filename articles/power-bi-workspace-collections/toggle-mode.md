---
title: Raporlar için görünüm ve düzenleme modu arasında geçiş yap
description: Power BI çalışma alanı koleksiyonları içindeki raporlarınız için görünüm ve düzenleme modu arasında geçiş yapmayı öğrenin.
services: power-bi-workspace-collections
ms.service: power-bi-embedded
author: rkarlin
ms.author: rkarlin
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.openlocfilehash: b2696560b5d5013fe337b51ec61cbfac9e512610
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75357673"
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-workspace-collections"></a>Power BI çalışma alanı koleksiyonlarında raporların görünüm ve düzenleme modu arasında geçiş yap

Power BI çalışma alanı koleksiyonları içindeki raporlarınız için görünüm ve düzenleme modu arasında geçiş yapmayı öğrenin.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

## <a name="creating-an-access-token"></a>Erişim belirteci oluşturma

Bir raporu görüntüleme ve düzenleme olanağı sunan bir erişim belirteci oluşturmanız gerekir. Bir raporu düzenlemek ve kaydetmek için, **Report. ReadWrite** belirteci iznine sahip olmanız gerekir. Daha fazla bilgi için bkz. [Power BI çalışma alanı koleksiyonlarında kimlik doğrulama ve yetkilendirme](app-token-flow.md).

> [!NOTE]
> Bu, var olan bir rapordaki değişiklikleri düzenlemenizi ve kaydetmenizi sağlar. **Farklı kaydet**destekleme işlevi de isterseniz, ek izinler sağlamanız gerekir. Daha fazla bilgi için bkz. [kapsamlar](app-token-flow.md#scopes).

```csharp
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a>Yapılandırma Ekle

Düzenleme modundayken Kaydet düğmesini görmek için izinleri ve bir viewMode sağlamanız gerekir. Daha fazla bilgi için bkz. [yapılandırma ayrıntılarını ekleme](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

Örneğin, JavaScript içinde:

```html
   <div id="reportContainer"></div>

    <script>
    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used to describe the what and how to embed.
    // This object is used when calling powerbi.embed.
    // This also includes settings and options such as filters.
    // You can find more information at https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details.
    var config= {
        type: 'report',
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        id:  '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
        permissions: models.Permissions.ReadWrite /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.View,
        settings: {
            filterPaneEnabled: true,
            navContentPaneEnabled: true
        }
    };

    // Get a reference to the embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed the report and display it within the div container.
    var report = powerbi.embed(reportContainer, config);
    </script>
```

Bu, raporu, görünümler olarak ayarlanan **ViewMode** 'a göre görüntüleme moduna eklemeyi gösterir **. ViewMode. View**.

## <a name="view-mode"></a>Görünüm modu

Düzenleme modundaysanız, görünüm moduna geçmek için aşağıdaki JavaScript 'ı kullanabilirsiniz.

```javascript
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to view mode.
report.switchMode("view");

```

## <a name="edit-mode"></a>Düzenleme modu

Görüntüleme modundaysanız, düzenleme moduna geçmek için aşağıdaki JavaScript 'ı kullanabilirsiniz.

```javascript
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to edit mode.
report.switchMode("edit");

```

## <a name="see-also"></a>Ayrıca bkz.

[Bir örnek ile kullanmaya başlama](get-started-sample.md)  
[Rapor ekleme](embed-report.md)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[PowerBI-CSharp git deposu](https://github.com/Microsoft/PowerBI-CSharp)  
[PowerBI-node git deposu](https://github.com/Microsoft/PowerBI-Node)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](https://community.powerbi.com/)
