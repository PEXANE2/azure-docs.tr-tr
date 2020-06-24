---
title: DevTest Labs sanal makineniz için özel yapılar oluşturun | Microsoft Docs
description: Azure DevTest Labs ile kullanmak için kendi yapılarınızı nasıl yazacağınızı öğrenin.
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
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84896602"
---
# <a name="create-custom-artifacts-for-your-devtest-labs-virtual-machine"></a>DevTest Labs sanal makineniz için özel yapılar oluşturma

Bu makalede açıklanan adımlara genel bir bakış için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
>
>

## <a name="overview"></a>Genel Bakış
Bir VM sağlamadıktan sonra, uygulamanızı dağıtmak ve ayarlamak için *yapıtları* kullanabilirsiniz. Yapıt bir yapıt tanım dosyasından ve git deposundaki bir klasörde depolanan diğer betik dosyalarından oluşur. Yapıt tanımı dosyaları, bir VM 'ye ne yüklemek istediğinizi belirtmek için kullanabileceğiniz JSON ve ifadelerden oluşur. Örneğin, bir yapıt adı, çalıştırılacak bir komut ve komut çalıştırıldığında kullanılabilir parametreler tanımlayabilirsiniz. Yapıt tanım dosyası içindeki diğer betik dosyalarına adı ile başvurabilirsiniz.

## <a name="artifact-definition-file-format"></a>Yapıt tanım dosyası biçimi
Aşağıdaki örnek, bir tanım dosyasının temel yapısını oluşturan bölümleri gösterir:

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

| Öğe adı | Gerekli mi? | Description |
| --- | --- | --- |
| $schema |No |JSON Şema dosyasının konumu. JSON şema dosyası, tanım dosyasının geçerliliğini test etmenize yardımcı olabilir. |
| başlık |Yes |Laboratuvarda görünen yapıt adı. |
| açıklama |Yes |Laboratuvarda görünen yapının açıklaması. |
| ıconuri |No |Laboratuvarda görünen simgenin URI 'SI. |
| targetOsType |Yes |Yapıtın yüklendiği VM 'nin işletim sistemi. Desteklenen seçenekler Windows ve Linux. |
| parametreler |No |Bir makinede yapıt install komutu çalıştırıldığında belirtilen değerler. Bu, yapıtı özelleştirmenize yardımcı olur. |
| Eylemine |Yes |Bir VM üzerinde yürütülen yapıt install komutu. |

### <a name="artifact-parameters"></a>Yapıt parametreleri
Tanım dosyasının parametreler bölümünde, bir kullanıcının bir yapıtı yüklediklerinde hangi değerlere giriş yapabilir olduğunu belirtin. Yapıt install komutunda bu değerlere başvurabilirsiniz.

Parametreleri tanımlamak için aşağıdaki yapıyı kullanın:

    "parameters": {
      "<parameterName>": {
        "type": "<type-of-parameter-value>",
        "displayName": "<display-name-of-parameter>",
        "description": "<description-of-parameter>"
      }
    }

| Öğe adı | Gerekli mi? | Description |
| --- | --- | --- |
| tür |Yes |Parametre değerinin türü. İzin verilen türler için aşağıdaki listeye bakın. |
| displayName |Yes |Laboratuvardaki bir kullanıcıya görüntülenen parametrenin adı. |
| açıklama |Yes |Laboratuvarda görüntülenen parametrenin açıklaması. |

İzin verilen türler şunlardır:

* dize (geçerli JSON dizesi)
* int (herhangi bir geçerli JSON tamsayı)
* bool (herhangi bir geçerli JSON Boole değeri)
* dizi (geçerli JSON dizisi)

## <a name="secrets-as-secure-strings"></a>Güvenli dizeler olarak gizli diziler
Gizli dizeleri güvenli dizeler olarak bildirin. `parameters`Dosyadaki **artifactfile.js** bölümünde güvenli bir dize parametresi bildirmek için sözdizimi aşağıda verilmiştir:

```json

    "securestringParam": {
      "type": "securestring",
      "displayName": "Secure String Parameter",
      "description": "Any text string is allowed, including spaces, and will be presented in UI as masked characters.",
      "allowEmpty": false
    },
```

Yapıt install komutu için, ConvertTo-SecureString komutu kullanılarak oluşturulan güvenli dizeyi alan PowerShell betiğini çalıştırın. 

```json
  "runCommand": {
    "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./artifact.ps1 -StringParam ''', parameters('stringParam'), ''' -SecureStringParam (ConvertTo-SecureString ''', parameters('securestringParam'), ''' -AsPlainText -Force) -IntParam ', parameters('intParam'), ' -BoolParam:$', parameters('boolParam'), ' -FileContentsParam ''', parameters('fileContentsParam'), ''' -ExtraLogLines ', parameters('extraLogLines'), ' -ForceFail:$', parameters('forceFail'), '\"')]"
  }
```

Tüm örnek artifactfile.jsve artifact.ps1 (PowerShell betiği) için [GitHub 'da bu örneğe](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes)bakın.

Bir diğer önemli nokta, Kullanıcı hata ayıklaması için çıktı yakalandığından gizli dizileri konsola günlüğe kaydetmek değildir. 

## <a name="artifact-expressions-and-functions"></a>Yapıt ifadeleri ve işlevleri
Yapıt install komutunu oluşturmak için ifadeleri ve işlevleri kullanabilirsiniz.
İfadeler köşeli ayraçlar ([ve]) ile çevrelenmiş ve yapıt yüklendiği zaman değerlendirilir. İfadeler, JSON dize değerinde herhangi bir yerde görünebilir. İfadeler her zaman başka bir JSON değeri döndürür. Köşeli ayraç ([) ile başlayan bir sabit dize kullanmanız gerekiyorsa, iki köşeli ayraç ([[) kullanmanız gerekir.
Genellikle, bir değer oluşturmak için işlevleri işlevlerle birlikte kullanırsınız. JavaScript içinde olduğu gibi, işlev çağrıları **fonksiyonadı (arg1, arg2, arg3)** olarak biçimlendirilir.

Aşağıdaki listede yaygın işlevler gösterilmektedir:

* **Parametreler (ParameterName)**: yapıt komutu çalıştırıldığında belirtilen bir parametre değeri döndürür.
* **Concat (arg1, arg2, arg3,.....)**: birden çok dize değerini birleştirir. Bu işlev, çeşitli bağımsız değişkenler alabilir.

Aşağıdaki örnek, bir değer oluşturmak için ifadelerin ve işlevlerin nasıl kullanılacağını gösterir:

    runCommand": {
        "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Özel yapıt oluşturma

1. JSON Düzenleyicisi yükler. Yapıt tanım dosyalarıyla çalışmak için bir JSON düzenleyicisine ihtiyacınız vardır. Windows, Linux ve OS X için kullanılabilen [Visual Studio Code](https://code.visualstudio.com/)kullanmanızı öneririz.
2. Tanım dosyasında bir örnek artifactfile.jsalın. [GitHub deponuzda](https://github.com/Azure/azure-devtestlab)DevTest Labs ekibi tarafından oluşturulan yapıtları inceleyin. Kendi yapılarınızı oluşturmanıza yardımcı olabilecek zengin bir yapıt kitaplığı oluşturduk. Kendi yapıtlarınızı oluşturmak için bir yapıt tanım dosyası indirin ve üzerinde değişiklikler yapın.
3. IntelliSense 'in kullanımını yapın. Bir yapıt tanım dosyası oluşturmak için kullanabileceğiniz geçerli öğeleri görmek için IntelliSense kullanın. Ayrıca, bir öğenin değerleri için farklı seçenekleri görebilirsiniz. Örneğin, **Targetostype** öğesini düzenlediğinizde, IntelliSense Windows veya Linux için size iki seçenek gösterir.
4. Yapıtı DevTest Labs veya [kendi git deponuzdaki](devtest-lab-add-artifact-repo.md) [genel git deposunda](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) depolayın. Ortak depoda, başkaları tarafından paylaşılan yapıtları, doğrudan kullanabileceğiniz veya gereksinimlerinize uyacak şekilde özelleştirebilmeniz için görüntüleyebilirsiniz.
   
   1. Her yapıt için ayrı bir dizin oluşturun. Dizin adı yapıt adı ile aynı olmalıdır.
   2. Yapıt tanım dosyasını (artifactfile.jsüzerinde) oluşturduğunuz dizinde depolayın.
   3. Yapıt install komutuyla başvurulan betikleri depolayın.
      
      Yapıt klasörünün nasıl görünebileceğini gösteren bir örnek aşağıda verilmiştir:
      
      ![Yapıt klasörü örneği](./media/devtest-lab-artifact-author/git-repo.png)
5. Yapıtları depolamak için kendi deponuzu kullanıyorsanız, Şu makaledeki yönergeleri izleyerek depoyu laboratuvara ekleyin: [yapılar ve şablonlar Için Git deposu ekleme](devtest-lab-add-artifact-repo.md).

## <a name="related-articles"></a>İlgili makaleler:
* [DevTest Labs 'de yapıt başarısızlıklarını tanılama](devtest-lab-troubleshoot-artifact-failure.md)
* [DevTest Labs 'de bir Kaynak Yöneticisi şablonu kullanarak bir VM 'yi mevcut bir Active Directory etki alanına katma](https://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Sonraki adımlar
* [Laboratuvara git yapıt deposu eklemeyi](devtest-lab-add-artifact-repo.md)öğrenin.
