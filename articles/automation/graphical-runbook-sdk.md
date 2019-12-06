---
title: Azure Otomasyonu Grpahical runbook SDK 'sına genel bakış
description: Bu makalede, Azure Otomasyonu grafik runbook SDK 'sının nasıl kullanılacağı açıklanmaktadır
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 07/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a06c190931fdd0f49132f815b153c08ece68c9f3
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74849556"
---
# <a name="use-the-azure-automation-graphical-runbook-sdk"></a>Azure Otomasyonu grafik runbook SDK 'sını kullanma

[Grafik runbook](automation-graphical-authoring-intro.md) 'ları, temeldeki Windows PowerShell veya PowerShell iş akışı kodunun karmaşıklıklarını yönetmeye yardımcı olan runbook 'lardır. Microsoft Azure Automation grafik yazma SDK 'Sı, geliştiricilerin Azure Automation hizmeti ile kullanılmak üzere grafik runbook 'Ları oluşturmasını ve düzenlemesini sağlar. Aşağıdaki kod parçacıkları, kodunuzda bir grafik runbook oluşturmanın temel akışını gösterir.

## <a name="pre-requisites"></a>Önkoşullar

Başlamak için `Microsoft.Azure.Automation.GraphicalRunbook.Model` paketini projenize aktarın.

## <a name="create-a-runbook-object-instance"></a>Runbook nesne örneği oluşturma

`Orchestrator.GraphRunbook.Model` derlemesine başvurun ve `Orchestrator.GraphRunbook.Model.GraphRunbook` sınıfının bir örneğini oluşturun:

```csharp
using Orchestrator.GraphRunbook.Model;
using Orchestrator.GraphRunbook.Model.ExecutableView;

var runbook = new GraphRunbook();
```

## <a name="add-runbook-parameters"></a>Runbook parametreleri Ekle

`Orchestrator.GraphRunbook.Model.Parameter` nesnelerinin örneğini oluşturun ve bunları runbook 'a ekleyin:

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

Etkinlikler `Orchestrator.GraphRunbook.Model` ad alanında aşağıdaki sınıflar tarafından uygulanır:

|Sınıf  |Etkinlik  |
|---------|---------|
|CommandActivity     | Bir PowerShell komutunu (cmdlet, işlev, vb.) çağırır.        |
|InvokeRunbookActivity     | Başka bir runbook 'u satır içi olarak çağırır.        |
|JunctionActivity     | Tüm gelen dalların bitmesini bekler.        |
|WorkflowScriptActivity     | Runbook bağlamında bir PowerShell veya PowerShell Iş akışı kodu bloğu yürütür (runbook türüne bağlı olarak). Bu güçlü bir araçtır, ancak bunu aşırı kullanmayın: UI Bu betik bloğunu metin olarak gösterir; yürütme altyapısı, belirtilen bloğu bir siyah kutu olarak değerlendirir ve temel bir sözdizimi denetimi dışında, içeriğini çözümlemeye çalışır hale getirir. Yalnızca tek bir PowerShell komutu çağırmanız gerekiyorsa CommandActivity ' ı tercih edin.        |

> [!NOTE]
> Kendi etkinliklerinizi, belirtilen sınıflardan türemeyin: Azure Otomasyonu, runbook 'ları özel etkinlik türleriyle kullanamaz.

CommandActivity ve InvokeRunbookActivity parametrelerinin doğrudan değer değil değer tanımlayıcıları olarak sağlanması gerekir. Değer tanımlayıcıları, gerçek parametre değerlerinin nasıl üretilmesi gerektiğini belirtir. Aşağıdaki değer tanımlayıcıları Şu anda verilmiştir:


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
> Kendi değer tanımlayıcılarınızı, belirtilen sınıflardan türemeyin: Azure Otomasyonu, runbook 'ları özel değer tanımlayıcı türleriyle kullanamaz.

Etkinlikleri bağlayan bağlantıların örneğini oluşturun ve bunları runbook 'a ekleyin:

```csharp
runbook.AddLink(new Link(activityA, activityB, LinkType.Sequence));

runbook.AddLink(
 new Link(activityB, activityC, LinkType.Pipeline) {
  Condition = string.Format("$ActivityOutput['{0}'] -gt 0", activityB.Name)
 });
```

## <a name="save-the-runbook-to-a-file"></a>Runbook 'u bir dosyaya kaydetme

Bir runbook 'u bir dizeye seri hale getirmek için `Orchestrator.GraphRunbook.Model.Serialization.RunbookSerializer` kullanın:

```csharp
var serialized = RunbookSerializer.Serialize(runbook);
```

Bu dize **. graphrunbook** uzantılı bir dosyaya kaydedilebilir ve bu dosya Azure Otomasyonu 'na aktarılabilir.
Serileştirilmiş biçim `Orchestrator.GraphRunbook.Model.dll`gelecek sürümlerinde değişebilir. Geriye dönük uyumluluk: daha eski bir `Orchestrator.GraphRunbook.Model.dll` seri hale getirilen runbook, daha yeni bir sürüm tarafından seri durumdan çıkarılamaz. İleri uyumluluk garantisi garanti edilmez: daha yeni bir sürüme sahip bir runbook, eski sürümler tarafından seri hale getirilemez.

## <a name="next-steps"></a>Sonraki adımlar

Azure Otomasyonu 'nda grafik runbook 'Ları hakkında daha fazla bilgi için bkz. [grafik yazma tanıtımı](automation-graphical-authoring-intro.md)

