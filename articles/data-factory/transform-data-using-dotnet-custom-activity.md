---
title: İşlem hattında özel etkinlikler kullanma
description: Özel etkinlikler oluşturmayı ve bunları bir Azure Data Factory işlem hattında kullanmayı öğrenin.
services: data-factory
ms.service: data-factory
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/26/2018
ms.openlocfilehash: 4913152125b0fafd74db575f835d53fa992b075e
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79260585"
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Bir Azure Data Factory işlem hattında özel etkinlikler kullanma

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-use-custom-activities.md)
> * [Geçerli sürüm](transform-data-using-dotnet-custom-activity.md)

Azure Data Factory ardışık düzeninde kullanabileceğiniz iki tür etkinlik vardır.

- Verileri [, desteklenen kaynak ve havuz veri depoları](copy-activity-overview.md#supported-data-stores-and-formats)arasında taşımak için [veri taşıma etkinlikleri](copy-activity-overview.md) .
- Azure HDInsight, Azure Batch ve Azure Machine Learning gibi işlem hizmetlerini kullanarak verileri dönüştürmek için [veri dönüştürme etkinlikleri](transform-data.md) .

Data Factory desteklemediği bir veri deposuna/veritabanından veri taşımak veya Data Factory tarafından desteklenmeyen bir şekilde veri dönüştürmek veya işlemek için, kendi veri taşıma veya dönüştürme mantığınızla **özel bir etkinlik** oluşturabilir ve aktiviteyi bir işlem hattında kullanabilirsiniz. Özel etkinlik, özelleştirilmiş kod mantığınızı bir sanal makineler **Azure Batch** havuzunda çalıştırır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure Batch Service ' i yeni biliyorsanız aşağıdaki makalelere bakın:

* Azure Batch hizmetine genel bakış hakkında [temel bilgiler Azure Batch](../batch/batch-technical-overview.md) .
* Azure Batch bir hesap oluşturmak için [New-AzBatchAccount](/powershell/module/az.batch/New-azBatchAccount) cmdlet 'i, Azure portal kullanarak Azure Batch hesabı oluşturmak için [Azure Portal](../batch/batch-account-create-portal.md) . Cmdlet 'ini kullanma hakkında ayrıntılı yönergeler için [Azure Batch hesabını yönetmek üzere PowerShell kullanma](https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) makalesine bakın.
* Azure Batch havuzu oluşturmak için [New-AzBatchPool](/powershell/module/az.batch/New-AzBatchPool) cmdlet 'i.

## <a name="azure-batch-linked-service"></a>Bağlı hizmet Azure Batch

Aşağıdaki JSON bir örnek Azure Batch bağlı hizmetini tanımlar. Ayrıntılar için bkz. [Azure Data Factory tarafından desteklenen işlem ortamları](compute-linked-services.md)

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "batchaccount",
            "accessKey": {
                "type": "SecureString",
                "value": "access key"
            },
            "batchUri": "https://batchaccount.region.batch.azure.com",
            "poolName": "poolname",
            "linkedServiceName": {
                "referenceName": "StorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }
}
```

 Azure Batch bağlı hizmet hakkında daha fazla bilgi edinmek için bkz. [işlem bağlı hizmetleri](compute-linked-services.md) makalesi.

## <a name="custom-activity"></a>Özel etkinlik

Aşağıdaki JSON kod parçacığı, basit bir özel etkinliğe sahip bir işlem hattı tanımlar. Etkinlik tanımının Azure Batch bağlı hizmetine bir başvurusu vardır.

```json
{
  "name": "MyCustomActivityPipeline",
  "properties": {
    "description": "Custom activity sample",
    "activities": [{
      "type": "Custom",
      "name": "MyCustomActivity",
      "linkedServiceName": {
        "referenceName": "AzureBatchLinkedService",
        "type": "LinkedServiceReference"
      },
      "typeProperties": {
        "command": "helloworld.exe",
        "folderPath": "customactv2/helloworld",
        "resourceLinkedService": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        }
      }
    }]
  }
}
```

Bu örnekte, HelloWorld. exe, Resourcelınkedservice 'te kullanılan Azure Storage hesabının customactv2/HelloWorld klasöründe depolanan özel bir uygulamadır. Özel etkinlik, Azure Batch yürütülmek üzere bu özel uygulamayı gönderir. Komutunu, Azure Batch havuz düğümlerinin hedef Işletim sisteminde yürütülebilecek herhangi bir tercih edilen uygulamayla değiştirebilirsiniz.

Aşağıdaki tabloda, bu etkinliğe özgü özelliklerin adları ve açıklamaları açıklanmaktadır.

| Özellik              | Açıklama                              | Gerekli |
| :-------------------- | :--------------------------------------- | :------- |
| ad                  | İşlem hattındaki etkinliğin adı     | Yes      |
| açıklama           | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| type                  | Özel etkinlik için etkinlik türü **Custom**olur. | Yes      |
| linkedServiceName     | Azure Batch bağlı hizmet. Bu bağlı hizmet hakkında bilgi edinmek için bkz. [işlem bağlı hizmetleri](compute-linked-services.md) makalesi.  | Yes      |
| command               | Yürütülecek özel uygulamanın komutu. Uygulama Azure Batch havuzu düğümünde zaten kullanılabiliyorsa, Resourcelınkedservice ve folderPath atlanabilir. Örneğin, Windows Batch havuzu düğümü tarafından yerel olarak desteklenen `cmd /c dir`için komutu belirtebilirsiniz. | Yes      |
| Resourcelınkedservice | Özel uygulamanın depolandığı depolama hesabına Azure Storage bağlı hizmeti | Eşleşen&#42;       |
| folderPath            | Özel uygulamanın klasörünün yolu ve tüm bağımlılıkları<br/><br/>Alt klasörlerde depolanan bağımlılıklarınız varsa-diğer bir deyişle, *FolderPath* altındaki hiyerarşik bir klasör yapısında, dosyalar Azure Batch kopyalanırken klasör yapısı şu anda düzleştirilir. Diğer bir deyişle, tüm dosyalar alt klasörleri olmayan tek bir klasöre kopyalanır. Bu davranışa geçici bir çözüm bulmak için, dosyaları sıkıştırmayı, sıkıştırılmış dosyayı kopyalamayı ve sonra istenen konumdaki özel kodla bir daha fazla ping işlemi yapmayı göz önünde bulundurun. | Eşleşen&#42;       |
| referenceObjects      | Mevcut bağlı hizmetlerin ve veri kümelerinin dizisi. Başvurulan bağlı hizmetler ve veri kümeleri, JSON biçiminde özel uygulamaya geçirilir, böylece özel kodunuzun Data Factory kaynaklarına başvurabilir. | Hayır       |
| extendedProperties    | Özel kodunuzun ek özelliklere başvurabilmesi için JSON biçiminde özel uygulamaya geçirilebilecek Kullanıcı tanımlı özellikler | Hayır       |
| retentionTimeInDays | Özel etkinlik için gönderilen dosyalar için bekletme süresi. Varsayılan değer 30 gündür. | Hayır |

&#42;`resourceLinkedService` ve `folderPath` özelliklerinin ikisi de belirtilmelidir ya da her ikisi de atlanmalıdır.

> [!NOTE]
> Bağlı hizmetleri özel etkinlikte referenceObjects olarak geçirirseniz, Azure Key Vault etkinleştirilmiş bir bağlı hizmeti geçirmek (güvenli dizeler içermediğinden) ve doğrudan anahtardan gizli bir ad kullanarak kimlik bilgilerini getirmek iyi bir güvenlik uygulamasıdır. Koddan kasa. [Burada](https://github.com/nabhishek/customactivity_sample/tree/linkedservice) , Akv özellikli bağlı hizmete başvuran bir örnek bulabilir, Key Vault kimlik bilgilerini alır ve sonra koddaki depolamaya erişir.

## <a name="custom-activity-permissions"></a>Özel etkinlik izinleri

Özel etkinlik Azure Batch otomatik Kullanıcı hesabını *görev kapsamıyla yönetici olmayan erişime* (varsayılan Otomatik Kullanıcı belirtimi) ayarlar. Otomatik Kullanıcı hesabının izin düzeyini değiştiremezsiniz. Daha fazla bilgi için bkz. [Batch 'de Kullanıcı hesapları altında görevleri çalıştırma | Otomatik Kullanıcı hesapları](../batch/batch-user-accounts.md#auto-user-accounts).

## <a name="executing-commands"></a>Komutları yürütme

Özel etkinlik kullanarak bir komutu doğrudan çalıştırabilirsiniz. Aşağıdaki örnek, hedef Azure Batch havuz düğümlerinde "Echo Hello World" komutunu çalıştırır ve çıktıyı stdout ' a yazdırır.

```json
{
  "name": "MyCustomActivity",
  "properties": {
    "description": "Custom activity sample",
    "activities": [{
      "type": "Custom",
      "name": "MyCustomActivity",
      "linkedServiceName": {
        "referenceName": "AzureBatchLinkedService",
        "type": "LinkedServiceReference"
      },
      "typeProperties": {
        "command": "cmd /c echo hello world"
      }
    }]
  }
}
```

## <a name="passing-objects-and-properties"></a>Nesneleri ve özellikleri geçirme

Bu örnek, özel uygulamanıza Data Factory nesneleri ve Kullanıcı tanımlı özellikleri geçirmek için referenceObjects ve extendedProperties 'i nasıl kullanabileceğinizi gösterir.

```json
{
  "name": "MyCustomActivityPipeline",
  "properties": {
    "description": "Custom activity sample",
    "activities": [{
      "type": "Custom",
      "name": "MyCustomActivity",
      "linkedServiceName": {
        "referenceName": "AzureBatchLinkedService",
        "type": "LinkedServiceReference"
      },
      "typeProperties": {
        "command": "SampleApp.exe",
        "folderPath": "customactv2/SampleApp",
        "resourceLinkedService": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        },
        "referenceObjects": {
          "linkedServices": [{
            "referenceName": "AzureBatchLinkedService",
            "type": "LinkedServiceReference"
          }]
        },
        "extendedProperties": {          
          "connectionString": {
            "type": "SecureString",
            "value": "aSampleSecureString"
          },
          "PropertyBagPropertyName1": "PropertyBagValue1",
          "propertyBagPropertyName2": "PropertyBagValue2",
          "dateTime1": "2015-04-12T12:13:14Z"
        }
      }
    }]
  }
}
```

Etkinlik yürütüldüğünde, referenceObjects ve extendedProperties, SampleApp. exe ' nin aynı yürütme klasörüne dağıtılan aşağıdaki dosyalarda saklanır:

- `activity.json`

  Özel etkinliğin Extendedözelliklerini ve özelliklerini depolar.

- `linkedServices.json`

  ReferenceObjects özelliğinde tanımlanan bağlantılı hizmetlerden oluşan bir diziyi depolar.

- `datasets.json`

  ReferenceObjects özelliğinde tanımlanan bir veri kümesi dizisini depolar.

Aşağıdaki örnek kod, SampleApp. exe ' nin JSON dosyalarından gerekli bilgilere nasıl erişebileceğini göstermektedir:

```csharp
using Newtonsoft.Json;
using System;
using System.IO;

namespace SampleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            //From Extend Properties
            dynamic activity = JsonConvert.DeserializeObject(File.ReadAllText("activity.json"));
            Console.WriteLine(activity.typeProperties.extendedProperties.connectionString.value);

            // From LinkedServices
            dynamic linkedServices = JsonConvert.DeserializeObject(File.ReadAllText("linkedServices.json"));
            Console.WriteLine(linkedServices[0].properties.typeProperties.accountName);
        }
    }
}
```

## <a name="retrieve-execution-outputs"></a>Yürütme çıkışlarını alma

Aşağıdaki PowerShell komutunu kullanarak bir işlem hattı çalıştırması başlatabilirsiniz:

```powershell
$runId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName
```

İşlem hattı çalışırken, aşağıdaki komutları kullanarak yürütme çıkışını kontrol edebilirsiniz:

```powershell
while ($True) {
    $result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

    if(!$result) {
        Write-Host "Waiting for pipeline to start..." -foregroundcolor "Yellow"
    }
    elseif (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
        Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
    }
    else {
        Write-Host "Pipeline '"$pipelineName"' run finished. Result:" -foregroundcolor "Yellow"
        $result
        break
    }
    ($result | Format-List | Out-String)
    Start-Sleep -Seconds 15
}

Write-Host "Activity `Output` section:" -foregroundcolor "Yellow"
$result.Output -join "`r`n"

Write-Host "Activity `Error` section:" -foregroundcolor "Yellow"
$result.Error -join "`r`n"
```

Özel uygulamanızın **stdout** ve **stderr** 'ı, görevin bir GUID 'ı ile Azure Batch bağlantılı hizmet oluştururken tanımladığınız Azure Storage bağlı hizmetindeki **adfjobs** kapsayıcısına kaydedilir. Aşağıdaki kod parçacığında gösterildiği gibi, etkinlik çalıştırma çıktısından ayrıntılı yolu alabilirsiniz:

```
Pipeline ' MyCustomActivity' run finished. Result:

ResourceGroupName : resourcegroupname
DataFactoryName   : datafactoryname
ActivityName      : MyCustomActivity
PipelineRunId     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
PipelineName      : MyCustomActivity
Input             : {command}
Output            : {exitcode, outputs, effectiveIntegrationRuntime}
LinkedServiceName :
ActivityRunStart  : 10/5/2017 3:33:06 PM
ActivityRunEnd    : 10/5/2017 3:33:28 PM
DurationInMs      : 21203
Status            : Succeeded
Error             : {errorCode, message, failureType, target}

Activity Output section:
"exitcode": 0
"outputs": [
  "https://<container>.blob.core.windows.net/adfjobs/<GUID>/output/stdout.txt",
  "https://<container>.blob.core.windows.net/adfjobs/<GUID>/output/stderr.txt"
]
"effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
Activity Error section:
"errorCode": ""
"message": ""
"failureType": ""
"target": "MyCustomActivity"
```

StdOut. txt içeriğini aşağı akış etkinliklerinde kullanmak istiyorsanız, "\@Activity (' MyCustomActivity ') ifadesindeki stdout. txt dosyasının yolunu alabilirsiniz. Output. çıktılar [0]".

> [!IMPORTANT]
> - Activity. JSON, linkedServices. JSON ve DataSet. JSON, Batch görevinin Runtime klasöründe saklanır. Bu örnek için Activity. JSON, linkedServices. JSON ve DataSet. JSON `"https://adfv2storage.blob.core.windows.net/adfjobs/\<GUID>/runtime/"` yolunda depolanır. Gerekirse, bunları ayrı olarak temizlemeniz gerekir.
> - Şirket içinde barındırılan Integration Runtime kullanan bağlı hizmetler için, anahtar veya parola gibi hassas bilgiler, müşteri tarafından tanımlanan özel ağ ortamında kimlik bilgilerinin kalmasını sağlamak üzere şirket içinde barındırılan Integration Runtime tarafından şifrelenir. Özel uygulama kodunuz tarafından bu şekilde başvuruluyorsa bazı hassas alanlar eksik olabilir. Gerekirse, bağlantılı hizmet başvurusunu kullanmak yerine, SecureString 'i extendedProperties içinde kullanın.

## <a name="pass-outputs-to-another-activity"></a>Çıkışları başka bir etkinliğe geçirme

Özel bir etkinlikte kodunuzdaki özel değerleri Azure Data Factory geri gönderebilirsiniz. Bunu uygulamanızdaki `outputs.json` yazarak yapabilirsiniz. Data Factory, `outputs.json` içeriğini kopyalar ve `customOutput` özelliğinin değeri olarak etkinlik çıktısına ekler. (Boyut sınırı 2MB 'dir.) Aşağı akış etkinliklerindeki `outputs.json` içeriğini kullanmak istiyorsanız, `@activity('<MyCustomActivity>').output.customOutput`ifadesini kullanarak değeri alabilirsiniz.

## <a name="retrieve-securestring-outputs"></a>SecureString çıkışlarını al

Bu makaledeki bazı örneklerde gösterildiği gibi *SecureString*türü olarak atanan hassas özellik değerleri, Data Factory Kullanıcı arabirimindeki İzleme sekmesinde maskelenir.  Ancak gerçek işlem hattı yürütmesinde, bir *SecureString* özelliği `activity.json` dosyasında düz metın olarak JSON olarak serileştirilir. Örnek:

```json
"extendedProperties": {
  "connectionString": {
    "type": "SecureString",
    "value": "aSampleSecureString"
  }
}
```

Bu serileştirme gerçekten güvenli değildir ve güvenli hale getirilmesi amaçlanmamıştır. Amaç, Izleme sekmesindeki değeri maskelemek için Data Factory ipucu.

Özel etkinlikten *SecureString* türünün özelliklerine erişmek için, ile aynı klasöre yerleştirilmiş `activity.json` dosyasını okuyun. EXE, JSON serisini kaldırıp JSON özelliğine (extendedProperties = > [propertyName] = > value) erişin.

## <a name="compare-v2-v1"></a>V2 özel etkinliğini ve sürüm 1 (özel) DotNet etkinliğini karşılaştırın

Azure Data Factory sürüm 1 ' de, `IDotNetActivity` arabiriminin `Execute` yöntemini uygulayan bir sınıf ile .NET sınıf kitaplığı projesi oluşturarak (özel) DotNet etkinliği uygulayabilirsiniz. Bir (özel) DotNet etkinliğinin JSON yükünde bağlı hizmetler, veri kümeleri ve genişletilmiş özellikler, yürütme yöntemine türü kesin belirlenmiş nesneler olarak geçirilir. Sürüm 1 davranışı hakkındaki ayrıntılar için bkz. [Sürüm 1 ' de (özel) DotNet](v1/data-factory-use-custom-activities.md). Bu uygulama nedeniyle, sürüm 1 DotNet etkinlik kodunuzun .NET Framework 4.5.2 hedeflemesi gerekir. Sürüm 1 DotNet etkinliğinin de Windows tabanlı Azure Batch havuzu düğümlerinde yürütülmesi gerekir.

Azure Data Factory v2 özel etkinliğinde, .NET arabirimi uygulamanız gerekmez. Artık yürütülebilir dosya olarak derlenen komutları, betikleri ve kendi özel kodunuzu doğrudan çalıştırabilirsiniz. Bu uygulamayı yapılandırmak için, `folderPath` özelliği ile birlikte `Command` özelliğini belirtin. Özel etkinlik, `folderpath` için çalıştırılabiliri ve bağımlılıklarını yükler ve komutu sizin için yürütür.

Bağlı hizmetler, veri kümeleri (referenceObjects 'te tanımlanmıştır) ve bir Data Factory v2 özel etkinliğinin JSON yükünde tanımlanan genişletilmiş özelliklere ve çalıştırılabilir dosya tarafından JSON dosyaları olarak erişilebilir. Yukarıdaki SampleApp. exe kod örneğinde gösterildiği gibi, bir JSON serileştirici kullanarak gerekli özelliklere erişebilirsiniz.

Data Factory v2 özel etkinliğinde tanıtılan değişikliklerle, özel kod mantığınızı tercih ettiğiniz dilde yazabilir ve Azure Batch tarafından desteklenen Windows ve Linux Işletim sistemlerinde yürütebilirsiniz.

Aşağıdaki tabloda Data Factory v2 özel etkinliği ve Data Factory sürüm 1 (özel) DotNet etkinliği arasındaki farklar açıklanmaktadır:

|Fark      | Özel Etkinlik      | sürüm 1 (özel) DotNet etkinliği      |
| ---- | ---- | ---- |
|Özel mantık nasıl tanımlanır      |Yürütülebilir bir dosya sağlayarak      |.NET DLL uygulayarak      |
|Özel mantığın yürütme ortamı      |Windows veya Linux      |Windows (.NET Framework 4.5.2)      |
|Betikler yürütülüyor      |, Betikleri doğrudan yürütmeyi destekler (örneğin, Windows VM 'de "cmd/c echo Hello World")      |.NET DLL 'de uygulama gerektirir      |
|Veri kümesi gerekli      |İsteğe bağlı      |Etkinlikleri zincirlemek ve bilgi geçirmek için gereklidir      |
|Etkinlikten özel mantığa bilgi geçirin      |ReferenceObjects (LinkedServices ve DataSet) ve ExtendedProperties aracılığıyla (özel özellikler)      |ExtendedProperties (özel özellikler), girdi ve çıktı veri kümeleri aracılığıyla      |
|Özel mantığdaki bilgileri alma      |Yürütülebilir dosyanın aynı klasöründe depolanan Activity. JSON, linkedServices. JSON ve DataSet. JSON öğesini ayrıştırır      |.NET SDK aracılığıyla (.NET Frame 4.5.2)      |
|Günlüğe kaydetme      |Doğrudan STDOUT 'a yazar      |.NET DLL 'de günlükçü uygulama      |

Sürüm 1 (özel) DotNet etkinliği için yazılmış bir .NET kodunuz varsa, özel etkinliğin geçerli sürümüyle çalışması için kodunuzu değiştirmeniz gerekir. Bu üst düzey yönergeleri izleyerek kodunuzu güncelleştirin:

  - Projeyi bir .NET sınıf kitaplığından konsol uygulamasına değiştirin.
  - `Main` yöntemiyle uygulamanızı başlatın. `IDotNetActivity` arabiriminin `Execute` yöntemi artık gerekli değildir.
  - Bağlı hizmetleri, veri kümelerini ve etkinlikleri bir JSON serileştiriciyle okuyup ayrıştırın ve türü kesin belirlenmiş nesneler olarak kullanmayın. Gerekli özelliklerin değerlerini ana özel kod mantığınıza geçirin. Örnek olarak yukarıdaki SampleApp. exe koduna bakın.
  - Günlükçü nesnesi artık desteklenmiyor. Yürütülebilir bir dosyanın çıktısı konsola yazdırılabilir ve stdout. txt dosyasına kaydedilir.
  - Microsoft. Azure. Management. DataFactory NuGet paketi artık gerekli değildir.
  - Kodunuzu derleyin, yürütülebilir dosyayı ve bağımlılıklarını Azure depolama 'ya yükleyin ve `folderPath` özelliğindeki yolu tanımlayın.

Data Factory sürüm 1 makalesinde açıklanan uçtan uca DLL ve işlem hattı örneğinin tam bir örneği için bir Azure Data Factory işlem hattında [özel etkinlikler kullanma](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) Data Factory özel etkinlik olarak yeniden yazılabilir, bkz. [Data Factory özel etkinlik örneği](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/ADFv2CustomActivitySample).

## <a name="auto-scaling-of-azure-batch"></a>Azure Batch otomatik ölçeklendiriliyor

Ayrıca, **Otomatik ölçeklendirme** özelliği ile bir Azure Batch havuzu da oluşturabilirsiniz. Örneğin, 0 adanmış VM ile bir Azure Batch havuzu ve bekleyen görevlerin sayısına göre bir otomatik ölçeklendirme formülü oluşturabilirsiniz.

Buradaki örnek formül aşağıdaki davranışa ulaşır: havuz başlangıçta oluşturulduğunda 1 VM ile başlar. $PendingTasks ölçümü, çalışan + etkin (sıraya alınmış) durumundaki görevlerin sayısını tanımlar. Formül, son 180 saniye içinde bekleyen görevlerin ortalama sayısını bulur ve Targetadanmış 'yi uygun şekilde ayarlar. Targetadanmış, 25 sanal makine dışında hiçbir şekilde geçmeyeceğinden emin olmanızı sağlar. Bu nedenle, yeni görevler gönderildiğinde havuz otomatik olarak büyür ve görevler tamamlandıkça, VM 'Ler tek bir kez serbest olur ve otomatik ölçeklendirme bu VM 'Leri küçültür. startingNumberOfVMs ve Maxnumberofvm 'Ler gereksinimlerinize göre ayarlanabilir.

Otomatik ölçeklendirme formülü:

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Ayrıntılar için bkz. [bir Azure Batch havuzundaki işlem düğümlerini otomatik olarak ölçeklendirme](../batch/batch-automatic-scaling.md) .

Havuz varsayılan [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx)kullanıyorsa, Batch hizmeti özel etkinliği ÇALıŞTıRMADAN önce VM 'yi hazırlamak için 15-30 dakika sürebilir. Havuz farklı bir autoScaleEvaluationInterval kullanıyorsa, Batch hizmeti autoScaleEvaluationInterval + 10 dakika alabilir.

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka yollarla nasıl dönüştürebileceğinizi açıklayan aşağıdaki makalelere bakın:

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akışı etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
