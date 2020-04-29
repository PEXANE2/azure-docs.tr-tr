---
title: Azure Otomasyonu grafik runbook SDK 'sını kullanma
description: Bu makalede, Azure Otomasyonu grafik runbook SDK 'sının nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 07/20/2018
ms.topic: conceptual
ms.openlocfilehash: 0058c0a0cedf2ea3f6c32f8f8368cca5b8dc6e3c
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509015"
---
# <a name="use-the-azure-automation-graphical-runbook-sdk"></a>Azure Otomasyonu grafik runbook SDK 'sını kullanma

[Grafik runbook 'ları](automation-graphical-authoring-intro.md) , temeldeki Windows PowerShell veya PowerShell iş akışı kodunun karmaşıklıklarını yönetmeye yardımcı olur. Microsoft Azure Automation grafik yazma SDK 'Sı, geliştiricilerin Azure Otomasyonu ile kullanılmak üzere grafik runbook 'ları oluşturmasını ve düzenlemesini sağlar. Bu makalede, kodunuzun içinden bir grafik runbook 'u oluşturma konusunda temel adımlar açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

`Microsoft.Azure.Management.Automation.GraphicalRunbook.Model` Paketi projenize aktarın.

## <a name="create-a-runbook-object-instance"></a>Runbook nesne örneği oluşturma

`Orchestrator.GraphRunbook.Model` Derlemeye başvurun ve `Orchestrator.GraphRunbook.Model.GraphRunbook` sınıfın bir örneğini oluşturun:

```csharp
using Orchestrator.GraphRunbook.Model;
using Orchestrator.GraphRunbook.Model.ExecutableView;

var runbook = new GraphRunbook();
```

## <a name="add-runbook-parameters"></a>Runbook parametreleri Ekle

Nesneleri `Orchestrator.GraphRunbook.Model.Parameter` örnekleyin ve Runbook 'a ekleyin:

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

## <a name="add-activities-and-links"></a>Etkinlik ve bağlantı ekleme

Uygun türdeki etkinlikleri oluşturun ve bunları runbook 'a ekleyin:

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

Etkinlikler, `Orchestrator.GraphRunbook.Model` ad alanındaki aşağıdaki sınıflar tarafından uygulanır.

|Sınıf  |Etkinlik  |
|---------|---------|
|CommandActivity     | Bir PowerShell komutunu (cmdlet, işlev, vb.) çağırır.        |
|InvokeRunbookActivity     | Başka bir runbook 'u satır içi olarak çağırır.        |
|JunctionActivity     | Tüm gelen dalların bitmesini bekler.        |
|WorkflowScriptActivity     | Runbook bağlamında bir PowerShell veya PowerShell Iş akışı kodu bloğu yürütür (runbook türüne bağlı olarak). Bu güçlü bir araçtır, ancak bunu aşırı kullanmayın: UI Bu betik bloğunu metin olarak gösterir; yürütme altyapısı, belirtilen bloğu bir siyah kutu olarak değerlendirir ve temel bir sözdizimi denetimi dışında, içeriğini çözümlemeye çalışır hale getirir. Yalnızca tek bir PowerShell komutu çağırmanız gerekiyorsa CommandActivity ' ı tercih edin.        |

> [!NOTE]
> Kendi etkinliklerinizi, belirtilen sınıflardan türemeyin. Azure Otomasyonu, runbook 'ları özel etkinlik türleriyle kullanamaz.

Doğrudan değer değil `CommandActivity` , `InvokeRunbookActivity` değer tanımlayıcıları olarak ve parametreleri sağlamalısınız. Değer tanımlayıcıları, gerçek parametre değerlerinin nasıl oluşturulacağını belirtir. Aşağıdaki değer tanımlayıcıları Şu anda verilmiştir:


|CI  |Tanım  |
|---------|---------|
|ConstantValueDescriptor     | Sabit kodlanmış sabit değer anlamına gelir.        |
|RunbookParameterValueDescriptor     | Bir runbook parametresini ada göre gösterir.        |
|Activitızputvaluedescriptor     | , Bir etkinliğin başka bir etkinlik tarafından oluşturulan verilere "abone ol" yapmasına izin veren bir yukarı akış etkinlik çıktısına başvurur.        |
|AutomationVariableValueDescriptor     | Bir Otomasyon değişken varlığını ada göre gösterir.         |
|AutomationCredentialValueDescriptor     | Bir Otomasyon sertifika varlığını ada göre gösterir.        |
|AutomationConnectionValueDescriptor     | Bir Otomasyon bağlantı varlığını ada göre gösterir.        |
|PowerShellExpressionValueDescriptor     | Etkinlik çağırmadan hemen önce değerlendirilecek olan serbest biçimli bir PowerShell ifadesi belirtir.  <br/>Bu güçlü bir araçtır, ancak bunu aşırı kullanmayın: UI bu ifadeyi metin olarak gösterir; yürütme altyapısı, belirtilen bloğu bir siyah kutu olarak değerlendirir ve temel bir sözdizimi denetimi dışında, içeriğini çözümlemeye çalışır hale getirir. Mümkün olduğunda, daha belirgin değer tanımlayıcıları tercih edin.      |

> [!NOTE]
> Kendi değer tanımlayıcılarınızı, belirtilen sınıflardan türemeyin. Azure Otomasyonu, runbook 'ları özel değer tanımlayıcı türleriyle kullanamaz.

Etkinlikleri bağlayan bağlantıların örneğini oluşturun ve bunları runbook 'a ekleyin:

```csharp
runbook.AddLink(new Link(activityA, activityB, LinkType.Sequence));

runbook.AddLink(
 new Link(activityB, activityC, LinkType.Pipeline) {
  Condition = string.Format("$ActivityOutput['{0}'] -gt 0", activityB.Name)
 });
```

## <a name="save-the-runbook-to-a-file"></a>Runbook 'u bir dosyaya kaydetme

Bir `Orchestrator.GraphRunbook.Model.Serialization.RunbookSerializer` runbook 'u bir dizeye seri hale getirmek için kullanın:

```csharp
var serialized = RunbookSerializer.Serialize(runbook);
```

Bu dizeyi **. graphrunbook** uzantılı bir dosyaya kaydedebilirsiniz. İlgili runbook, Azure Otomasyonu 'na aktarılabilir.
Serileştirilmiş biçim gelecekteki sürümlerinde değişebilir `Orchestrator.GraphRunbook.Model.dll`. Geriye dönük uyumluluk: daha eski bir sürümü ile serileştirilmiş tüm runbook `Orchestrator.GraphRunbook.Model.dll` , daha yeni bir sürüm tarafından seri durumdan çıkarılamaz. İleri uyumluluk garantisi garanti edilmez: daha yeni bir sürüme sahip bir runbook, eski sürümler tarafından seri hale getirilemez.

## <a name="next-steps"></a>Sonraki adımlar

Azure Otomasyonu 'nda grafik runbook 'ları hakkında daha fazla bilgi için bkz. [grafik yazma tanıtımı](automation-graphical-authoring-intro.md).