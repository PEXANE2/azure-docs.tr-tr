---
title: Azure Akış Analizi CI/CD NuGet paketini kullanma
description: Bu makalede, sürekli bir tümleştirme ve dağıtım işlemi ayarlamak için Azure Akış Analizi CI/CD NuGet paketinin nasıl kullanılacağı açıklanmaktadır.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: 11e68aaa7c70d4f888c0009bc28d9bb90f431f3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75354452"
---
# <a name="use-the-azure-stream-analytics-cicd-nuget-package-for-integration-and-development"></a>Tümleştirme ve geliştirme için Azure Akış Analizi CI/CD NuGet paketini kullanın 
Bu makalede, sürekli bir tümleştirme ve dağıtım işlemi ayarlamak için Azure Akış Analizi CI/CD NuGet paketinin nasıl kullanılacağı açıklanmaktadır.

MSBuild desteği almak için Visual Studio için [Stream Analytics araçlarının](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio) 2.3.0000.0 veya üzeri sürümünü kullanın.

Bir NuGet paketi kullanılabilir: [Microsoft.Azure.Stream Analytics.CICD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/). [Stream Analytics Visual Studio projelerinin](stream-analytics-vs-tools.md)sürekli tümleştirme ve dağıtım işlemini destekleyen MSBuild, yerel çalıştırma ve dağıtım araçlarını sağlar. 
> [!NOTE]
> NuGet paketi yalnızca Visual Studio için Stream Analytics Tools'un 2.3.0000.0 veya üzeri sürümünde kullanılabilir. Visual Studio araçlarının önceki sürümlerinde oluşturulan projelerinizin varsa, bunları 2.3.0000.0 veya üzeri sürümle açın ve kaydedin. Sonra yeni yetenekler etkinleştirilir. 

Daha fazla bilgi için [Visual Studio için Stream Analytics araçlarına](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio)bakın.

## <a name="msbuild"></a>MSBuild
Standart Visual Studio MSBuild deneyimi gibi, bir proje oluşturmak için iki seçeneğiniz vardır. Projeyi sağ tıklatıp sonra **Oluştur'u**seçebilirsiniz. Komut satırından NuGet paketinde **MSBuild'i** de kullanabilirsiniz.
```
./build/msbuild /t:build [Your Project Full Path] /p:CompilerTaskAssemblyFile=Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll  /p:ASATargetsFilePath="[NuGet Package Local Path]\build\StreamAnalytics.targets"

```

Bir Akış Analizi Visual Studio projesi başarıyla oluşturduğunda, **depo gözü/[Hata Ayıklama/Perakende Satış]/Dağıt** klasörü altında aşağıdaki iki Azure Kaynak Yöneticisi şablon dosyası oluşturur: 

*  Kaynak Yöneticisi şablon dosyası

       [ProjectName].JobTemplate.json 

*  Kaynak Yöneticisi parametreleri dosyası

       [ProjectName].JobTemplate.parameters.json   

parameters.json dosyasındaki varsayılan parametreler Visual Studio projenizdeki ayarlardan dır. Başka bir ortama dağıtmak istiyorsanız, parametreleri buna göre değiştirin.

> [!NOTE]
> Tüm kimlik bilgileri için varsayılan değerler null olarak ayarlanır. Buluta dağıtılmadan önce değerleri ayarlamanız **gerekir.**

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
[Kaynak Yöneticisi şablon dosyası ve Azure PowerShell ile](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)nasıl dağıtılanın caiz olduğu hakkında daha fazla bilgi edinin. [Kaynak Yöneticisi şablonunda bir nesnenin parametre olarak](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters)nasıl kullanılacağı hakkında daha fazla bilgi edinin.

Azure Veri Gölü Deposu Gen1 için Yönetilen Kimlik'i çıktı olarak kullanmak için, Azure'a dağıtmadan önce PowerShell'i kullanarak hizmet ilkesine Erişim sağlamanız gerekir. [Kaynak Yöneticisi şablonu yla Yönetilen Kimlik ile ADLS Gen1'in nasıl dağıtılancaya kadar dağıtılabildiğini](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment)öğrenin.


## <a name="command-line-tool"></a>Komut satırı aracı

### <a name="build-the-project"></a>Projeyi derleme
NuGet paketinde **SA.exe**adında bir komut satırı aracı vardır. Sürekli tümleştirme ve sürekli teslimat işleminizde kullanabileceğiniz rasgele bir makinede proje oluşturma ve yerel testleri destekler. 

Dağıtım dosyaları varsayılan olarak geçerli dizinin altına yerleştirilir. Aşağıdaki -OutputPath parametresini kullanarak çıktı yolunu belirtebilirsiniz:

```
./tools/SA.exe build -Project [Your Project Full Path] [-OutputPath <outputPath>] 
```

### <a name="test-the-script-locally"></a>Komut dosyasını yerel olarak test edin

Projeniz Visual Studio'da yerel giriş dosyalarını belirtmişse, *localrun* komutunu kullanarak otomatik bir komut dosyası testi çalıştırabilirsiniz. Çıktı sonucu geçerli dizinin altına yerleştirilir.
 
```
localrun -Project [ProjectFullPath]
```

### <a name="generate-a-job-definition-file-to-use-with-the-stream-analytics-powershell-api"></a>Stream Analytics PowerShell API ile kullanmak üzere bir iş tanımı dosyası oluşturma

*Kol* komutu, yapı girişi olarak oluşturulan iş şablonu ve iş şablonu parametre dosyalarını alır. Daha sonra bunları Stream Analytics PowerShell API ile kullanılabilecek bir iş tanımı JSON dosyasında birleştirir.

```powershell
arm -JobTemplate <templateFilePath> -JobParameterFile <jobParameterFilePath> [-OutputFile <asaArmFilePath>]
```
Örnek:
```powershell
./tools/SA.exe arm -JobTemplate "ProjectA.JobTemplate.json" -JobParameterFile "ProjectA.JobTemplate.parameters.json" -OutputFile "JobDefinition.json" 
```



## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio'da Bir Azure Akışı Analizi bulut işi oluşturun](stream-analytics-quick-create-vs.md)
* [Visual Studio ile Test Stream Analytics sorgularını yerel olarak sorgulayın](stream-analytics-vs-tools-local-run.md)
* [Visual Studio ile Azure Akış Analizi işlerini keşfedin](stream-analytics-vs-tools.md)
