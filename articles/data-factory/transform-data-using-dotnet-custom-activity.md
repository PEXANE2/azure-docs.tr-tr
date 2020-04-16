---
title: Bir ardışık alanda özel etkinlikler kullanma
description: Azure Veri Fabrikası ardışık bir ardışık alanda özel etkinlikler oluşturmayı ve bunları nasıl kullanacağınızı öğrenin.
services: data-factory
ms.service: data-factory
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/26/2018
ms.openlocfilehash: 74e381a9ad32acdaa8cbb719824d74ca6d339f30
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418958"
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Bir Azure Data Factory işlem hattında özel etkinlikler kullanma

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-use-custom-activities.md)
> * [Geçerli sürüm](transform-data-using-dotnet-custom-activity.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir Azure Veri Fabrikası ardışık hattında kullanabileceğiniz iki tür etkinlik vardır.

- Desteklenen [kaynak ve lavabo veri depoları](copy-activity-overview.md#supported-data-stores-and-formats)arasında veri taşımak için veri hareketi [faaliyetleri.](copy-activity-overview.md)
- Azure HDInsight, Azure Toplu İş ve Azure Machine Learning gibi bilgi işlem hizmetlerini kullanarak verileri dönüştürmek için [veri dönüştürme etkinlikleri.](transform-data.md)

Verileri Veri Fabrikası'nın desteklemediği bir veri deposuna/veri deposundan taşımak veya Verileri Veri Fabrikası tarafından desteklenmeyen bir şekilde dönüştürmek/işlemek için, kendi veri hareketiniz veya dönüştürme mantığınızla bir **Özel etkinlik** oluşturabilir ve etkinliği bir ardışık ardışık işlemde kullanabilirsiniz. Özel etkinlik, özelleştirilmiş kod mantığınızı Azure **Toplu Sanal** Makineler havuzunda çalıştırır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure Toplu İş hizmetinde yeniyseniz aşağıdaki makalelere bakın:

* Azure Toplu İşlem hizmetine genel bakış için [Azure Toplu İşlem temelleri.](../batch/batch-technical-overview.md)
* Azure portalını kullanarak Azure Toplu Hesabı oluşturmak için bir Azure Toplu İş hesabı (veya) [Azure portalı](../batch/batch-account-create-portal.md) oluşturmak için [Yeni-AzBatchAccount](/powershell/module/az.batch/New-azBatchAccount) cmdlet. Cmdlet'i kullanma yla ilgili ayrıntılı talimatlar için Azure Toplu İş Hesabı makalesini yönetmek için [PowerShell'i](https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) kullanma makalesine bakın.
* Azure Toplu İş havuzu oluşturmak için [Yeni-AzBatchPool](/powershell/module/az.batch/New-AzBatchPool) cmdlet.

## <a name="azure-batch-linked-service"></a>Azure Toplu İşlem bağlantılı hizmet

Aşağıdaki JSON örnek bir Azure Toplu İş bağlantılı hizmeti tanımlar. Ayrıntılar için Bkz. [Azure Veri Fabrikası tarafından desteklenen Bilgi İşlem ortamları](compute-linked-services.md)

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

 Azure Toplu İş bağlantılı hizmet hakkında daha fazla bilgi edinmek için [Bkz. Bilgi İşlem bağlantılı hizmetler](compute-linked-services.md) makalesine bakın.

## <a name="custom-activity"></a>Özel etkinlik

Aşağıdaki JSON snippet basit bir Özel Etkinlik ile bir boru hattı tanımlar. Etkinlik tanımının Azure Toplu İş Bağlantısı hizmetine bir başvurusu vardır.

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

Bu örnekte, helloworld.exe, resourceLinkedService'te kullanılan Azure Depolama hesabının customactv2/helloworld klasöründe depolanan özel bir uygulamadır. Özel etkinlik, Azure Toplu İş'te yürütülecek bu özel uygulamayı gönderir. Komutu, Azure Toplu Birleştirme Havuzu düğümlerinin hedef İşlem Sistemi'nde yürütülebilecek tercih edilen uygulamalarla değiştirebilirsiniz.

Aşağıdaki tabloda, bu faaliyete özgü özelliklerin adları ve açıklamaları açıklanmaktadır.

| Özellik              | Açıklama                              | Gerekli |
| :-------------------- | :--------------------------------------- | :------- |
| ad                  | Boru hattındaki etkinliğin adı     | Evet      |
| açıklama           | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| type                  | Özel etkinlik için etkinlik türü **Özeldir.** | Evet      |
| linkedServiceName     | Azure Toplu İş'e Bağlı Hizmet. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın.  | Evet      |
| command               | Yürütülecek özel uygulamanın komutu. Uygulama Azure Toplu İş Havuzu Düğümü'nde zaten kullanılabilirse, kaynak LinkedService ve folderPath atlanabilir. Örneğin, Windows Toplu İş Havuzu `cmd /c dir`düğümü tarafından yerel olarak desteklenen komutu belirtebilirsiniz. | Evet      |
| kaynakLinkedService | Azure Depolama Bağlantılı Hizmet, özel uygulamanın depolandığı Depolama hesabına | &#42; yok       |
| folderPath            | Özel uygulama klasörüne ve tüm bağımlılıklarına giden yol<br/><br/>Alt klasörlerde depolanan bağımlılıklarınız varsa - yani *folderPath* altında hiyerarşik bir klasör yapısında - dosyalar Azure Toplu İş'e kopyalandığında klasör yapısı şu anda düzleştirilmiş durumda. Diğer bir diğer anda, tüm dosyalar alt klasörü olmayan tek bir klasöre kopyalanır. Bu davranışı n için dosyaları sıkıştırmayı, sıkıştırılmış dosyayı kopyalamayı ve ardından istediğiniz konumda özel kodla açmayı düşünün. | &#42; yok       |
| referansNesneler      | Varolan Bağlantılı Hizmetler ve Veri Kümeleri dizisi. Başvurulan Bağlantılı Hizmetler ve Veri kümeleri JSON formatında özel uygulamaya aktarılır, böylece özel kodunuz Veri Fabrikası'nın kaynaklarına başvurulabilir | Hayır       |
| genişletilmiş Özellikler    | Özel kodunuz ek özelliklere başvurulabilmek için JSON formatında özel uygulamaya geçirilebilen kullanıcı tanımlı özellikler | Hayır       |
| retentionTimeInDays | Özel etkinlik için gönderilen dosyaların bekletme süresi. Varsayılan değer 30 gündür. | Hayır |

&#42; Özellikleri `resourceLinkedService` `folderPath` ve her ikisi de belirtilmelidir veya her ikisi de atlanmalıdır.

> [!NOTE]
> Bağlantılı hizmetleri Özel Etkinlik'te referans Olarak Nesneleri geçiriyorsanız, Azure Key Vault özellikli bağlantılı bir hizmeti geçirmek (güvenli dizeleri içermediğinden) ve kimlik bilgilerini doğrudan Anahtar Kasa'dan koddan gizli adı kullanarak getirmek iyi bir güvenlik uygulamasıdır. [Burada](https://github.com/nabhishek/customactivity_sample/tree/linkedservice) AKV etkin bağlantılı hizmete başvuran, kimlik bilgilerini Key Vault'tan alan ve koddaki depolama alanına erişen bir örnek bulabilirsiniz.

## <a name="custom-activity-permissions"></a>Özel etkinlik izinleri

Özel etkinlik, Azure Toplu Otomatik Kullanıcı hesabını görev kapsamıyla (varsayılan otomatik kullanıcı belirtimi) *yönetici olmayan erişime* ayarlar. Otomatik kullanıcı hesabının izin düzeyini değiştiremezsiniz. Daha fazla bilgi için bkz: [Toplu Iş' deki kullanıcı hesapları altındaki görevleri çalıştır | Otomatik kullanıcı hesapları.](../batch/batch-user-accounts.md#auto-user-accounts)

## <a name="executing-commands"></a>Yürütme komutları

Özel Etkinlik'i kullanarak bir komutu doğrudan yürütebilirsiniz. Aşağıdaki örnek, hedef Azure Toplu İşlem Havuzu düğümlerinde "yankı merhaba world" komutunu çalıştırır ve çıktıyı stdout'a yazdırır.

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

## <a name="passing-objects-and-properties"></a>Geçen nesneler ve özellikler

Bu örnek, Veri Fabrikası nesnelerini ve kullanıcı tanımlı özellikleri özel uygulamanıza geçirmek için referans Nesneleri ve genişletilmiş Özellikleri nasıl kullanabileceğinizi gösterir.

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

Etkinlik yürütüldüğünde, referenceObjects ve extendedProperties, SampleApp.exe'nin aynı yürütme klasörüne dağıtılan aşağıdaki dosyalarda depolanır:

- `activity.json`

  Genişletilmiş Özellikleri ve özel etkinliğin özelliklerini depolar.

- `linkedServices.json`

  ReferenceObjects özelliğinde tanımlanan bir dizi Bağlantılı Hizmet depolar.

- `datasets.json`

  ReferenceObjects özelliğinde tanımlanan bir dizi Veri kümesi depolar.

Örnek kod aşağıdaki ÖrnekApp.exe'nin JSON dosyalarından gerekli bilgilere nasıl erişebileceğini gösterir:

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

## <a name="retrieve-execution-outputs"></a>Yürütme çıktılarını alma

Aşağıdaki PowerShell komutunu kullanarak bir ardışık hat lar hattı çalıştırabilirsiniz:

```powershell
$runId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName
```

Ardışık iş aktarım ı çalışırken, yürütme çıktısını aşağıdaki komutları kullanarak denetleyebilirsiniz:

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

Özel uygulamanızın **stdout** ve **stderr** görevin bir GUID ile Azure Toplu İş Bağlantılı Hizmet oluştururken tanımladığınız Azure Depolama Bağlantılı Hizmeti **adfjobs** kapsayıcısına kaydedilir. Aşağıdaki snippet gösterildiği gibi Etkinlik Çalıştır çıktısından ayrıntılı yolu alabilirsiniz:

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

Downstream etkinliklerinde stdout.txt içeriğini tüketmek istiyorsanız, "activity('MyCustomActivity').output.outputs[0]"\@ifadesinde stdout.txt dosyasına giden yolu alabilirsiniz.

> [!IMPORTANT]
> - Activity.json, linkedServices.json ve datasets.json Toplu İşlem görevinin çalışma zamanı klasöründe depolanır. Bu örnekte, activity.json, linkedServices.json ve datasets.json `"https://adfv2storage.blob.core.windows.net/adfjobs/\<GUID>/runtime/"` şekilde depolanır. Gerekirse, ayrı ayrı temizlemek gerekir.
> - Self-Hosted Integration Runtime'ı kullanan Bağlantılı Hizmetler için, anahtarlar veya parolalar gibi hassas bilgiler, müşteri tanımlı özel ağ ortamında kimlik bilgisi nin kalmasını sağlamak için Self-Hosted Integration Runtime tarafından şifrelenir. Bu şekilde özel uygulama kodunuz tarafından başvurulduğunda bazı hassas alanlar eksik olabilir. Gerekirse Bağlantılı Hizmet başvurusu kullanmak yerine genişletilmiş Özellikler'de SecureString'i kullanın.

## <a name="pass-outputs-to-another-activity"></a>Çıktıları başka bir faaliyete geçirin

Özel Etkinlik'te kodunuzdaki özel değerleri Azure Veri Fabrikası'na geri gönderebilirsiniz. Bunu uygulamanızdan yazarak `outputs.json` yapabilirsiniz. Veri Fabrikası içeriğini kopyalar `outputs.json` ve `customOutput` özelliğin değeri olarak Faaliyet Çıktısı'na ekler. (Boyut sınırı 2MB'dır.) Akış aşağı etkinliklerinin `outputs.json` içeriğini tüketmek istiyorsanız, ifadeyi `@activity('<MyCustomActivity>').output.customOutput`kullanarak değeri elde edebilirsiniz.

## <a name="retrieve-securestring-outputs"></a>SecureString çıktılarını alın

Bu makaledeki bazı örneklerde gösterildiği gibi, *SecureString*türü olarak atanan hassas özellik değerleri, Veri Fabrikası kullanıcı arabirimindeki İzleme sekmesinde maskelenir.  Gerçek ardışık yürütme, ancak, bir *SecureString* özelliği düz `activity.json` metin olarak dosya içinde JSON olarak seriolarak. Örneğin:

```json
"extendedProperties": {
  "connectionString": {
    "type": "SecureString",
    "value": "aSampleSecureString"
  }
}
```

Bu serileştirme gerçekten güvenli değildir ve güvenli olması amaçlanmamıştır. Amaç, İzleme sekmesindeki değeri gizlemek için Veri Fabrikası'na ipucu vermektir.

Özel bir etkinlikten *SecureString* türü özelliklerine `activity.json` erişmek için, ''niz ile aynı klasöre yerleştirilen dosyayı okuyun. EXE, JSON deserialize ve sonra JSON özelliğine erişmek (genişletilmiş Properties => [propertyName] => değeri).

## <a name="compare-v2-custom-activity-and-version-1-custom-dotnet-activity"></a><a name="compare-v2-v1"></a>v2 Özel Etkinliği ve sürüm 1 (Özel) DotNet Etkinliğini Karşılaştır

Azure Veri Fabrikası sürüm 1'de, `Execute` `IDotNetActivity` arabirim yöntemini uygulayan bir sınıfa sahip bir .NET Sınıf Kitaplığı projesi oluşturarak (Özel) DotNet Etkinliği uygularsınız. Bir (Özel) DotNet Etkinliğinin JSON yükündeki Bağlantılı Hizmetler, Veri Kümeleri ve Genişletilmiş Özellikler, güçlü bir şekilde yazılan nesneler olarak yürütme yöntemine aktarılır. Sürüm 1 davranışı hakkında ayrıntılı bilgi için sürüm [1'deki (Custom) DotNet'e](v1/data-factory-use-custom-activities.md)bakın. Bu uygulama nedeniyle, sürüm 1 DotNet Etkinlik kodunun .NET Framework 4.5.2'yi hedeflemesi gerekiyor. Sürüm 1 DotNet Etkinliği de Windows tabanlı Azure Toplu Havuz düğümleri üzerinde yürütülmesi gerekir.

Azure Veri Fabrikası V2 Özel Etkinliği'nde bir .NET arabirimi uygulamanız gerekmez. Artık komutları, komutları ve yürütülebilir olarak derlenen kendi özel kodunuzu doğrudan çalıştırabilirsiniz. Bu uygulamayı yapılandırmak için `Command` özelliği `folderPath` özellik ile birlikte belirtirsiniz. Özel Etkinlik, yürütülebilir ve bağımlılıklarını `folderpath` yükler ve komutu sizin için yürütür.

Bağlantılı Hizmetler, Veri Kümeleri (başvuru Nesneleri olarak tanımlanır) ve Bir Veri Fabrikası v2 Özel Etkinliğinin JSON yükünde tanımlanan Genişletilmiş Özellikler,JSON dosyaları olarak çalıştırılabilir tarafından erişilebilir. Yukarıdaki SampleApp.exe kod örneğinde gösterildiği gibi bir JSON serializer kullanarak gerekli özelliklere erişebilirsiniz.

Veri Fabrikası V2 Özel Etkinliği'nde tanıtılan değişikliklerle, özel kod mantığınızı tercih ettiğiniz dilde yazabilir ve Azure Toplu İşlemi tarafından desteklenen Windows ve Linux İşlem Sistemleri'nde çalıştırabilirsiniz.

Aşağıdaki tabloda Veri Fabrikası V2 Özel Etkinliği ile Veri Fabrikası sürüm 1 (Özel) DotNet Etkinliği arasındaki farklar açıklanmaktadır:

|Farklılıklar      | Özel Etkinlik      | sürüm 1 (Özel) DotNet Etkinliği      |
| ---- | ---- | ---- |
|Özel mantık nasıl tanımlanır?      |Çalıştırılabilir bir sağlayarak      |.NET DLL uygulayarak      |
|Özel mantığın yürütme ortamı      |Windows veya Linux      |Windows (.NET Framework 4.5.2)      |
|Komut dosyalarını yürütme      |Komut dosyalarının doğrudan yürütülmesini destekler (örneğin Windows VM'de "cmd /c yankı merhaba dünyası" )      |.NET DLL'de uygulama gerektirir      |
|Gerekli veri kümesi      |İsteğe bağlı      |Faaliyetleri zincirlemek ve bilgi aktarmak için gerekli      |
|Bilgileri etkinlikten özel mantığa geçirin      |ReferenceObjects (LinkedServices ve Datasets) ve ExtendedProperties (özel özellikler) aracılığıyla      |ExtendedProperties (özel özellikler), Giriş ve Çıktı Veri Kümeleri aracılığıyla      |
|Özel mantıkla bilgi alma      |Parses activity.json, linkedServices.json ve datasets.json yürütülebilir aynı klasörde depolanır      |.NET SDK üzerinden (.NET Çerçeve 4.5.2)      |
|Günlüğe kaydetme      |Doğrudan STDOUT'a yazar      |.NET DLL'de Logger Uygulaması      |

Sürüm 1 (Özel) DotNet Etkinliği için yazılmış varolan .NET kodunuz varsa, Özel Etkinlik'in geçerli sürümüyle çalışabilmek için kodunuzu değiştirmeniz gerekir. Bu üst düzey yönergeleri izleyerek kodunuzu güncelleştirin:

  - Projeyi .NET Sınıf Kitaplığı'ndan Konsol Uygulamasına değiştirin.
  - Uygulamanızı yöntemle `Main` başlatın. `IDotNetActivity` Arabirimin `Execute` yöntemi artık gerekli değildir.
  - Bağlantılı Hizmetleri, Veri Kümelerini ve Etkinliği bir JSON serileştiricisiyle okuyun ve ayrıştırın ve güçlü bir şekilde yazılan nesnelerle değil. Gerekli özelliklerin değerlerini ana özel kod mantığınıza geçirin. Örnek olarak önceki SampleApp.exe koduna bakın.
  - Logger nesnesi artık desteklenmeyecek. Çalıştırılabilir çıktı konsola yazdırılabilir ve stdout.txt kaydedilir.
  - Microsoft.Azure.Management.DataFactorys NuGet paketi artık gerekli değildir.
  - Kodunuzu derle, çalıştırılabilir ve bağımlılıklarını Azure Depolama'ya yükleyin `folderPath` ve özellikteki yolu tanımlayın.

Veri Fabrikası sürüm 1 makalede açıklanan uçtan uca DLL ve boru hattı örneğinin [Azure Veri Fabrikası ardışık hattındaki özel etkinlikleri](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) nasıl kullanabileceğine ilişkin tam bir örnek için bkz. [Data Factory Custom Activity sample](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/ADFv2CustomActivitySample)

## <a name="auto-scaling-of-azure-batch"></a>Azure Toplu İş'in otomatik ölçekleme

**Otomatik ölçeklendirme** özelliğine sahip bir Azure Toplu İş havuzu da oluşturabilirsiniz. Örneğin, 0 özel VM'ler içeren masmavi bir toplu iş havuzu ve bekleyen görev sayısını temel alan otomatik ölçeklendirme formülü oluşturabilirsiniz.

Buradaki örnek formül aşağıdaki davranışı elde eder: Havuz başlangıçta oluşturulduğunda, 1 VM ile başlar. $PendingTasks ölçümü, çalışan + etkin (sıralanmış) durumdaki görev sayısını tanımlar. Formül, son 180 saniyeiçinde bekleyen görevlerin ortalama sayısını bulur ve TargetDedicated'ı buna göre ayarlar. TargetDedicated'In asla 25 VM'yi geçmemesini sağlar. Böylece, yeni görevler gönderildikçe, havuz otomatik olarak büyür ve görevler tamamlandıkça, VM'ler birer birer ücretsiz hale gelir ve otomatik ölçekleme bu VM'leri küçültür. başlangıçNumberOfVMs ve maxNumberofVMs ihtiyaçlarınıza göre ayarlanabilir.

Otomatik ölçekformülü:

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Ayrıntılar için [bir Azure Toplu İş havuzunda otomatik olarak hesap düğümlerini ölçeklendirin.](../batch/batch-automatic-scaling.md)

Havuz varsayılan [otomatik ÖlçeklendirmeAralığı](https://msdn.microsoft.com/library/azure/dn820173.aspx)kullanıyorsa, Toplu İşlem hizmetinin özel etkinliği çalıştırmadan önce VM'yi hazırlaması 15-30 dakika sürebilir. Havuz farklı bir otomatik ÖlçeklendirmeInterval kullanıyorsa, Toplu Işlem hizmeti otomatik ÖlçeklendirmeAralığı + 10 dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka şekillerde nasıl dönüştüreceklerini açıklayan aşağıdaki makalelere bakın:

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Kovan aktivitesi](transform-data-using-hadoop-hive.md)
* [Domuz aktivitesi](transform-data-using-hadoop-pig.md)
* [MapAz etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop Akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Kıvılcım etkinliği](transform-data-using-spark.md)
* [Makine Öğrenimi Toplu Yürütme Etkinliği](transform-data-using-machine-learning.md)
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
