---
title: DevTest Labs sanal makineniz için özel eserler oluşturun | Microsoft Dokümanlar
description: Azure DevTest Labs ile kullanmak üzere kendi yapılarınızı nasıl yazarak kullanacağınızı öğrenin.
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: spelluru
ms.openlocfilehash: 69b83590fb9b25c68d231b732b985ba633bb6884
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66399210"
---
# <a name="create-custom-artifacts-for-your-devtest-labs-virtual-machine"></a>DevTest Labs sanal makineniz için özel yapılar oluşturun

Bu makalede açıklanan adımların genel bir özeti için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
>
>

## <a name="overview"></a>Genel Bakış
Bir VM sağlamadan sonra uygulamanızı dağıtmak ve ayarlamak için *yapıları* kullanabilirsiniz. Yapı, bir yapı tanım dosyası ve Git deposundaki bir klasörde depolanan diğer komut dosyası dosyalarından oluşur. Yapı tanım dosyaları JSON ve VM'ye yüklemek istediğinizi belirtmek için kullanabileceğiniz ifadelerden oluşur. Örneğin, bir yapının adını, çalıştırılaca komutu ve komut çalıştırıldığında kullanılabilen parametreleri tanımlayabilirsiniz. Yapı tanım dosyasındaki diğer komut dosyası dosyalarına adıyla başvurabilirsiniz.

## <a name="artifact-definition-file-format"></a>Yapı tanımı dosya biçimi
Aşağıdaki örnek, tanım dosyasının temel yapısını oluşturan bölümleri gösterir:

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2016-11-28/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| Öğe adı | Gerekli mi? | Açıklama |
| --- | --- | --- |
| $schema |Hayır |JSON şema dosyasının yeri. JSON şema dosyası, tanım dosyasının geçerliliğini sınamanıza yardımcı olabilir. |
| başlık |Evet |Laboratuvarda görüntülenen yapıtın adı. |
| açıklama |Evet |Laboratuvarda görüntülenen yapının açıklaması. |
| iconUri |Hayır |Uri simgesi laboratuarda görüntülenir. |
| hedefOsType |Evet |Yapının yüklü olduğu VM işletim sistemi. Desteklenen seçenekler Windows ve Linux'tır. |
| parametreler |Hayır |Yapı yükleme komutu bir makinede çalıştırıldığında sağlanan değerler. Bu, yapınızı özelleştirmenize yardımcı olur. |
| Runcommand |Evet |VM'de çalıştırılan artifakt yükleme komutu. |

### <a name="artifact-parameters"></a>Yapı parametreleri
Tanım dosyasının parametreler bölümünde, bir yapı yı yüklerken kullanıcının hangi değerleri giriş yapabileceğinizi belirtin. Yapı yükleme komutundaki bu değerlere başvurabilirsiniz.

Parametreleri tanımlamak için aşağıdaki yapıyı kullanın:

    "parameters": {
      "<parameterName>": {
        "type": "<type-of-parameter-value>",
        "displayName": "<display-name-of-parameter>",
        "description": "<description-of-parameter>"
      }
    }

| Öğe adı | Gerekli mi? | Açıklama |
| --- | --- | --- |
| type |Evet |Parametre değeri türü. İzin verilen türler için aşağıdaki listeye bakın. |
| displayName |Evet |Laboratuarda bir kullanıcıya görüntülenen parametrenin adı. |
| açıklama |Evet |Laboratuvarda görüntülenen parametrenin açıklaması. |

İzin verilen türleri şunlardır:

* string (herhangi bir geçerli JSON dizesi)
* int (geçerli JSON sondası)
* bool (herhangi bir geçerli JSON Boolean)
* dizi (herhangi bir geçerli JSON dizisi)

## <a name="secrets-as-secure-strings"></a>Güvenli dizeleri olarak Sırlar
Sırları güvenli dizeler olarak bildirin. Burada `parameters` **artifactfile.json** dosyasının bölümünde güvenli bir dize parametresi bildirmek için sözdizimi:

```json

    "securestringParam": {
      "type": "securestring",
      "displayName": "Secure String Parameter",
      "description": "Any text string is allowed, including spaces, and will be presented in UI as masked characters.",
      "allowEmpty": false
    },
```

Artefakt yükleme komutu için ConvertTo-SecureString komutunu kullanarak oluşturulan güvenli dizeyi alan PowerShell komut dosyasını çalıştırın. 

```json
  "runCommand": {
    "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./artifact.ps1 -StringParam ''', parameters('stringParam'), ''' -SecureStringParam (ConvertTo-SecureString ''', parameters('securestringParam'), ''' -AsPlainText -Force) -IntParam ', parameters('intParam'), ' -BoolParam:$', parameters('boolParam'), ' -FileContentsParam ''', parameters('fileContentsParam'), ''' -ExtraLogLines ', parameters('extraLogLines'), ' -ForceFail:$', parameters('forceFail'), '\"')]"
  }
```

Tam örnek artifactfile.json ve artifact.ps1 (PowerShell komut dosyası) [için, GitHub bu örnek](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes)bakın.

Dikkat edilmesi gereken bir diğer önemli nokta, kullanıcı hata ayıklama için yakalanan çıktı olarak konsola sırlarını günlüğe kaydetmemektir. 

## <a name="artifact-expressions-and-functions"></a>Yapı ifadeleri ve işlevleri
Yapı yükleme komutunu oluşturmak için ifadeleri ve işlevleri kullanabilirsiniz.
İfadeler parantez ([ ve ]) ile çevrilidir ve yapı yüklendiğinde değerlendirilir. İfadeler JSON dize değerinde herhangi bir yerde görünebilir. İfadeler her zaman başka bir JSON değeri döndürer. Bir parantez ([) ile başlayan bir edebi dize kullanmanız gerekiyorsa, iki parantez ([[[).
Genellikle, bir değer oluşturmak için işlevli ifadeler kullanırsınız. JavaScript'te olduğu gibi işlev çağrıları **da functionName (arg1, arg2, arg3)** olarak biçimlendirilir.

Aşağıdaki liste ortak işlevleri gösterir:

* **parametreler(parameterName)**: Yapı komutu çalıştırıldığında sağlanan bir parametre değerini verir.
* **concat(arg1, arg2, arg3,.....)**: Birden çok dize değerlerini birleştirir. Bu işlev çeşitli bağımsız değişkenler alabilir.

Aşağıdaki örnek, bir değer oluşturmak için ifadelerin ve işlevlerin nasıl kullanılacağını gösterir:

    runCommand": {
        "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Özel bir yapı oluşturma

1. Bir JSON düzenleyicisi yükleyin. Artifakı tanım dosyalarıyla çalışmak için bir JSON düzenleyicisine ihtiyacınız var. Windows, Linux ve OS X için kullanılabilen [Visual Studio Code'u](https://code.visualstudio.com/)kullanmanızı öneririz.
2. Örnek bir artifactfile.json tanım dosyası alın. [GitHub depomuzda](https://github.com/Azure/azure-devtestlab)DevTest Labs ekibi tarafından oluşturulan yapılara göz atın. Kendi yapılarınızı oluşturmanıza yardımcı olabilecek zengin bir yapıt kütüphanesi oluşturduk. Bir yapı tanım dosyası indirin ve kendi yapılarınızı oluşturmak için dosyada değişiklik yapın.
3. IntelliSense'i kullanın. Yapı tanım dosyası oluşturmak için kullanabileceğiniz geçerli öğeleri görmek için IntelliSense'i kullanın. Ayrıca, bir öğenin değerleri için farklı seçenekleri de görebilirsiniz. Örneğin, **targetOsType** öğesini sizler lediğinizde, IntelliSense Size Windows veya Linux için iki seçenek gösterir.
4. Yapıyı DevTest Labs veya [kendi Git deponuz](devtest-lab-add-artifact-repo.md) [için herkese açık Git deposunda](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) saklayın. Genel depoda, başkaları tarafından paylaşılan ve doğrudan kullanabileceğiniz yapıları görüntüleyebilir veya gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.
   
   1. Her yapı için ayrı bir dizin oluşturun. Dizin adı, yapı adı ile aynı olmalıdır.
   2. Yapı tanım dosyasını (artifactfile.json) oluşturduğunuz dizinde depolayın.
   3. Yapı yükleme komutundan başvurulan komut dosyalarını depolayın.
      
      Yapı klasörünün nasıl görünebileceğine bir örnek aşağıda verilmiştir:
      
      ![Artefakt klasörü örneği](./media/devtest-lab-artifact-author/git-repo.png)
5. Yapıları depolamak için kendi deponuzu kullanıyorsanız, makaledeki yönergeleri izleyerek depoyu laboratuvara ekleyin: [Yapılar ve şablonlar için git deposu ekleyin.](devtest-lab-add-artifact-repo.md)

## <a name="related-articles"></a>İlgili makaleler:
* [DevTest Labs'da yapı hataları nasıl tanılanır?](devtest-lab-troubleshoot-artifact-failure.md)
* [DevTest Labs'da Kaynak Yöneticisi şablonu kullanarak vm'ye varolan bir Active Directory etki alanına katılma](https://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Sonraki adımlar
* [Bir git artifon deposunu laboratuvara](devtest-lab-add-artifact-repo.md)nasıl ekleyeceğinizi öğrenin.
