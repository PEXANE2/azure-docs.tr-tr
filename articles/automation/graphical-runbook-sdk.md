---
title: Azure Otomasyongrafik runbook SDK'yı kullanma
description: Bu makalede, Azure Otomasyongrafik runbook SDK nasıl kullanılacağı açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 07/20/2018
ms.topic: conceptual
ms.openlocfilehash: 21f6ff8078d5a1db88b2fde33c9063a56b3ee43a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682916"
---
# <a name="use-the-azure-automation-graphical-runbook-sdk"></a>Azure Otomasyongrafik runbook SDK'yı kullanma

[Grafik çalışma kitapları,](automation-graphical-authoring-intro.md) temel Windows PowerShell veya PowerShell İş Akışı kodunun karmaşıklığını yönetmenize yardımcı olur. Microsoft Azure Automation grafik yazarsi SDK, geliştiricilerin Azure Otomasyonu ile kullanılmak üzere grafik runbook'lar oluşturmasına ve bunları yönetmesine olanak tanır. Bu makalede, kodunuzdan grafik çalışma kitabı oluşturmanın temel adımları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Paketi `Microsoft.Azure.Automation.GraphicalRunbook.Model` projenize aktarın.

## <a name="create-a-runbook-object-instance"></a>Runbook nesne örneği oluşturma

Derlemeye `Orchestrator.GraphRunbook.Model` başvurun ve sınıfın `Orchestrator.GraphRunbook.Model.GraphRunbook` bir örneğini oluşturun:

```csharp
using Orchestrator.GraphRunbook.Model;
using Orchestrator.GraphRunbook.Model.ExecutableView;

var runbook = new GraphRunbook();
```

## <a name="add-runbook-parameters"></a>Runbook parametreleri ekleme

Nesneleri anında `Orchestrator.GraphRunbook.Model.Parameter` ayarlayın ve runbook'a ekleyin:

```csharp
runbook.AddParameter(
 new Parameter("YourName") {
  TypeName = typeof(string).FullName,
   DefaultValue = "World",
   Optional = true
 });

runbook.AddParameter(
 new Parameter("AnotherParameter") {
  TypeName = typeof(int).FullName, ...
 });
```

## <a name="add-activities-and-links"></a>Etkinlikler ve bağlantılar ekleme

Uygun türdeki etkinlikleri anında gerçekleştirin ve runbook'a ekleyin:

```csharp
var writeOutputActivityType = new CommandActivityType {
 CommandName = "Write-Output",
  ModuleName = "Microsoft.PowerShell.Utility",
 InputParameterSets = new [] {
  new ParameterSet {
   Name = "Default",
    new [] {
     new Parameter("InputObject"), ...
    }
  },
  ...
 }
};

var outputName = runbook.AddActivity(
 new CommandActivity("Output name", writeOutputActivityType) {
  ParameterSetName = "Default",
   Parameters = new ActivityParameters {
    {
     "InputObject",
     new RunbookParameterValueDescriptor("YourName")
    }
   },
   PositionX = 0,
   PositionY = 0
 });

var initializeRunbookVariable = runbook.AddActivity(
 new WorkflowScriptActivity("Initialize variable") {
  Process = "$a = 0",
   PositionX = 0,
   PositionY = 100
 });
```

Etkinlikler `Orchestrator.GraphRunbook.Model` ad alanında aşağıdaki sınıflar tarafından uygulanır.

|Sınıf  |Etkinlik  |
|---------|---------|
|Komut Etkinliği     | PowerShell komutunu (cmdlet, fonksiyon, vb.) çağırır.        |
|InvokeRunbookEtkinlik     | Başka bir runbook satır lı çağırır.        |
|Bağlantı Aktivitesi     | Gelen tüm dalların tamamlanmasını bekler.        |
|İş AkışıScriptAktivite     | Runbook bağlamında PowerShell veya PowerShell İş Akışı kodu bloğunu (runbook türüne bağlı olarak) yürütür. Bu güçlü bir araçtır, ancak aşırı kullanmayın: Kullanıcı Arabirimi bu komut dosyası bloğunu metin olarak gösterir; yürütme altyapısı sağlanan bloğu bir kara kutu olarak ele alacaktır ve temel sözdizimi denetimi dışında içeriğini çözümlemek için hiçbir girişimde bulunmaz. Tek bir PowerShell komutunu çağırmanız gerekiyorsa, CommandActivity'i tercih edin.        |

> [!NOTE]
> Verilen derslerden kendi etkinliklerinizi türetetmeyin. Azure Otomasyonu, özel etkinlik türlerine sahip runbook'ları kullanamaz.

Doğrudan değerler `CommandActivity` `InvokeRunbookActivity` değil, değer tanımlayıcıları olarak ve parametreleri sağlamanız gerekir. Değer tanımlayıcıları, gerçek parametre değerlerinin nasıl üretilebildiğini belirtir. Aşağıdaki değer tanımlayıcıları şu anda sağlanmaktadır:


|Tanımlayıcısı  |Tanım  |
|---------|---------|
|ConstantValueDescriptor     | Sabit kodlanmış sabit değeri ifade eder.        |
|RunbookParameterValueDescriptor     | Runbook parametresini ada göre ifade eder.        |
|ActivityOutputValueDescriptor     | Bir etkinliğin başka bir etkinlik tarafından üretilen verilere "abone" olmasını sağlayan bir yukarı akış etkinliği çıktısını ifade eder.        |
|OtomasyonVariableValueDescriptor     | Bir Otomasyon Değişkeni kıymetini ada göre ifade eder.         |
|OtomasyonCredentialValueDescriptor     | Bir Otomasyon Sertifikası kıymetini ada göre ifade eder.        |
|OtomasyonBağlantıDeğer Tanımlayıcı     | Bir Otomasyon Bağlantısı kıymetini ada göre ifade eder.        |
|PowerShellExpressionValueDescriptor     | Etkinliği aramadan hemen önce değerlendirilecek bir serbest biçim PowerShell ifadesi belirtir.  <br/>Bu güçlü bir araçtır, ancak aşırı kullanmayın: Kullanıcı Arabirimi bu ifadeyi metin olarak gösterir; yürütme altyapısı sağlanan bloğu bir kara kutu olarak ele alacaktır ve temel sözdizimi denetimi dışında içeriğini çözümlemek için hiçbir girişimde bulunmaz. Mümkün olduğunda, daha spesifik değer tanımlayıcılarını tercih edin.      |

> [!NOTE]
> Sağlanan sınıflardan kendi değer tanımlayıcılarınızı türetetmeyin. Azure Otomasyonu, özel değer tanımlayıcı türlerine sahip runbook'ları kullanamaz.

Bağlantıları bağlama etkinliklerini anında ayarlayın ve bunları runbook'a ekleyin:

```csharp
runbook.AddLink(new Link(activityA, activityB, LinkType.Sequence));

runbook.AddLink(
 new Link(activityB, activityC, LinkType.Pipeline) {
  Condition = string.Format("$ActivityOutput['{0}'] -gt 0", activityB.Name)
 });
```

## <a name="save-the-runbook-to-a-file"></a>Runbook'u bir dosyaya kaydetme

Runbook'u dize seri hale getirmek için kullanın: `Orchestrator.GraphRunbook.Model.Serialization.RunbookSerializer`

```csharp
var serialized = RunbookSerializer.Serialize(runbook);
```

Bu dizeyi **.graphrunbook** uzantılı bir dosyaya kaydedebilirsiniz. İlgili runbook Azure Otomasyonu'na aktarılabilir.
Serileştirilmiş biçim, `Orchestrator.GraphRunbook.Model.dll`'nin gelecekteki sürümlerinde değişebilir. Biz geriye dönük uyumluluk söz veriyorum: herhangi bir `Orchestrator.GraphRunbook.Model.dll` runbook eski bir sürümü ile seri leştirilmiş herhangi bir yeni sürümü tarafından deserialized olabilir. İleriye dönük uyumluluk garanti edilmez: daha yeni bir sürümle seri hale getirilen bir runbook eski sürümler tarafından deserialize edilemeyebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Otomasyonu'nda grafik runbook'lar hakkında daha fazla bilgi edinmek için [bkz.](automation-graphical-authoring-intro.md)