---
title: Veri kümesi Power BI çalışma alanı koleksiyonlarından rapor oluşturma
description: Power BI çalışma alanı koleksiyonu raporları, artık kendi uygulamanızdaki bir veri kümesinden oluşturulabilir.
services: power-bi-workspace-collections
ms.service: power-bi-embedded
author: rkarlin
ms.author: rkarlin
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.openlocfilehash: bcc6044d0f0f5270f81a619e4d1ad71ea35cc170
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75427067"
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-workspace-collections"></a>Power BI çalışma alanı koleksiyonlarında bir veri kümesinden yeni bir rapor oluşturma

Power BI çalışma alanı koleksiyonu raporları, artık kendi uygulamanızdaki bir veri kümesinden oluşturulabilir.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Kimlik doğrulama yöntemi, rapor gömmekle benzerdir. Bir veri kümesine özgü erişim belirteçlerini temel alır. PowerBI.com için kullanılan belirteçler Azure Active Directory (AAD) tarafından verilir. Power BI çalışma alanı koleksiyon belirteçleri kendi uygulamanız tarafından verilir.

Gömülü bir rapor oluştururken, verilen belirteçler belirli bir veri kümesi içindir. Her birinin benzersiz bir belirteci olduğundan emin olmak için belirteçlerin aynı öğedeki ekleme URL 'siyle ilişkilendirilmesi gerekir. Gömülü bir rapor oluşturmak için, *veri kümesi. Read ve Workspace. Report. Create* Scopes, erişim belirtecinde belirtilmelidir.

## <a name="create-access-token-needed-to-create-new-report"></a>Yeni rapor oluşturmak için gereken erişim belirteci oluşturma

Power BI çalışma alanı koleksiyonları, HMAC imzalı JSON Web belirteçleri olan bir ekleme belirteci kullanır. Belirteçler, Power BI çalışma alanı koleksiyonunuzdan erişim anahtarıyla imzalanır. Ekleme belirteçleri, bir uygulamaya eklemek üzere bir rapora salt okuma erişimi sağlamak için varsayılan olarak kullanılır. Ekleme belirteçleri belirli bir rapor için verilir ve bir ekleme URL 'siyle ilişkilendirilmelidir.

Belirteçleri imzalamak/şifrelemek için erişim anahtarları kullanıldığından, erişim belirteçleri sunucuda oluşturulmalıdır. Erişim belirteci oluşturma hakkında daha fazla bilgi için bkz. [Power BI çalışma alanı koleksiyonlarında kimlik doğrulama ve yetkilendirme](app-token-flow.md). [Createreportembedtoken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN) metodunu da gözden geçirebilirsiniz. Bu, Power BI için .NET SDK 'Yı kullanarak nasıl görüneceğine ilişkin bir örnektir.

Bu örnekte, üzerinde yeni rapor oluşturmak istediğimiz veri kümesi KIMLIĞI sunuyoruz. Ayrıca, *DataSet. Read ve Workspace. Report. Create*için kapsamları eklememiz gerekir.

*Powerbıtoken sınıfı* [Power BI Core Nugut paketini](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)yüklemenizi gerektirir.

**NuGet paket yüklemesi**

```powershell
Install-Package Microsoft.PowerBI.Core
```

**C#kodudur**

```csharp
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a>Yeni bir boş rapor oluştur

Yeni bir rapor oluşturmak için, oluşturma yapılandırması sağlanmalıdır. Bu, rapor oluşturmak istediğimiz olan erişim belirtecini, embedURL ve DatasetId 'yi içermelidir. Bu, NuGet [Power BI JavaScript paketini](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)yüklemenizi gerektirir. EmbedUrl yalnızca https://embedded.powerbi.com/appTokenReportEmbed olacaktır.

> [!NOTE]
> Test işlevselliği için [JavaScript rapor ekleme örneğini](https://microsoft.github.io/PowerBI-JavaScript/demo/) kullanabilirsiniz. Ayrıca, kullanılabilir farklı işlemler için kod örnekleri de sağlar.

**NuGet paket yüklemesi**

```powershell
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript kodu**

```html
<div id="reportContainer"></div>

<script>
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
</script>
```

*PowerBI. createReport ()* çağrısı, *div* öğesi içinde düzenleme modunda boş bir tuval oluşturur.

![Yeni boş rapor](media/create-report-from-dataset/create-new-report.png)

## <a name="save-new-reports"></a>Yeni raporları Kaydet

Rapor, **farklı kaydet** işlemi çağrılana kadar oluşturulmaz. Bu, Dosya menüsünden veya JavaScript 'ten yapılabilir.

```javascript
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> Yeni bir rapor yalnızca **Save as** çağrıldıktan sonra oluşturulur. Kaydettikten sonra tuval, raporu değil, veri kümesini düzenleme modunda göstermeye devam eder. Yeni raporu başka bir raporla yaptığınız gibi yeniden yüklemeniz gerekir.

![Dosya menüsü-farklı kaydet](media/create-report-from-dataset/save-new-report.png)

## <a name="load-the-new-report"></a>Yeni raporu yükle

Yeni raporla etkileşime geçmek için, uygulamayı düzenli bir raporu gömerşekilde gömmeniz gerekir, yani yeni bir belirteç özel olarak yeni rapor için verilmelidir ve sonra ekleme yöntemi çağırın.

```html
<div id="reportContainer"></div>
<script>
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
</script>
```

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a>"Kaydedildi" olayını kullanarak yeni bir raporun kaydedilmesini ve yüklenmesini otomatikleştirin

"Farklı kaydet" işlemini otomatik hale getirmek ve sonra yeni raporu yüklemek için "kaydedildi" olayını kullanabilirsiniz. Bu olay, Kaydet işlemi tamamlandığında tetiklenir ve yeni REPORTID, rapor adı, eski REPORTID (varsa) ve işlem saveAs veya Kaydet ' i içeren bir JSON nesnesi döndürür.

```json
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

İşlemi otomatik hale getirmek için "kaydedilmiş" olayını dinleyebilir, yeni REPORTID 'yi alabilir, yeni belirteci oluşturabilir ve yeni raporu bu raporla birlikte ekleyebilirsiniz.

```html
<div id="reportContainer"></div>
<script>
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints to Log window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that the application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide the wanted scopes*/);
        
        
    var embedConfiguration = {
        accessToken: newToken ,
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: newReportId,
    };

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
       
   // report.off removes a given event handler if it exists.
   report.off("saved");
    });
</script>
```

## <a name="see-also"></a>Ayrıca bkz.

[Bir örnek ile kullanmaya başlama](get-started-sample.md)  
[Raporları kaydetme](save-reports.md)  
[Rapor ekleme](embed-report.md)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Çekirdek NuGut paketini Power BI](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[JavaScript paketini Power BI](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](https://community.powerbi.com/)
