---
title: İç içe geçme şablon ortamlarını Azure DevTest Labs'da dağıtma
description: Azure DevTest Labs ile ortamsağlamak için iç içe yer alan Azure Kaynak Yöneticisi şablonlarını nasıl dağıttığınızı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: e83bc4e77a44f20d55fa3b56bc81aefd1d25bb03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76168833"
---
# <a name="deploy-nested-azure-resource-manager-templates-for-testing-environments"></a>Test ortamları için iç içe olan Azure Kaynak Yöneticisi şablonlarını dağıtma
İç içe dağıtım, ana Kaynak Yöneticisi şablonu içinden diğer Azure Kaynak Yöneticisi şablonlarını yürütmenize olanak tanır. Dağıtımınızı hedeflenen ve amaca özel şablonlar kümesine ayrıştırmak için olanak tanır. Test, yeniden kullanım ve okunabilirlik açısından avantajlar sağlar. Makale [Azure kaynaklarını dağıtırken bağlantılı şablonları kullanma,](../azure-resource-manager/templates/linked-templates.md) bu çözüme birkaç kod örneğiyle iyi bir genel bakış sağlar. Bu makalede, Azure DevTest Labs'a özgü bir örnek verilmektedir. 

## <a name="key-parameters"></a>Anahtar parametreler
Kendi Kaynak Yöneticisi şablonunuzu sıfırdan oluşturabiliyorken, Visual Studio'da şablongeliştirmeyi ve hata ayıklamayı kolaylaştıran [Azure Kaynak Grubu projesini](../azure-resource-manager/templates/create-visual-studio-deployment-project.md) kullanmanızı öneririz. Azuredeploy.json'a iç içe bir dağıtım kaynağı eklediğinizde, Visual Studio şablonu daha esnek hale getirmek için birkaç öğe ekler. Bu öğeler ikincil şablon ve parametreler dosyası, ana şablon dosyası içinde değişken adları ve yeni dosyalar için depolama konumu için iki parametre ile alt klasör içerir. **_artifactsLocation** ve **_artifactsLocationSasToken,** DevTest Labs'ın kullandığı temel parametrelerdir. 

DevTest Labs'ın ortamlarla nasıl çalıştığını bilmiyorsanız, [Azure Kaynak Yöneticisi şablonlarıyla çoklu VM ortamları ve PaaS kaynakları oluşturun'a](devtest-lab-create-environment-from-arm.md)bakın. Şablonlarınız DevTest Labs'daki laboratuvara bağlı depoda depolanır. Bu şablonlarla yeni bir ortam oluşturduğunuzda, dosyalar laboratuardaki bir Azure Depolama kapsayıcısına taşınır. DevTest Labs, iç içe olan dosyaları tanımlayabilmek ve kopyalayabilmek için _artifactsLocation ve _artifactsLocationSasToken parametreleri tanımlar ve alt klasörleri depolama kabına kopyalar. Ardından, otomatik olarak konumu ve Paylaşılan Erişim İmzası (SaS) belirteci parametrelere ekler. 

## <a name="nested-deployment-example"></a>İç içe dağıtım örneği
İç içe geçmenin basit bir örneği aşağıda verilmiştir:

```json

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "_artifactsLocation": {
        "type": "string"
    },
    "_artifactsLocationSasToken": {
        "type": "securestring"
    }},
"variables": {
    "NestOneTemplateFolder": "nestedtemplates",
    "NestOneTemplateFileName": "NestOne.json",
    "NestOneTemplateParametersFileName": "NestOne.parameters.json"},
    "resources": [
    {
        "name": "NestOne",
        "type": "Microsoft.Resources/deployments",
        "apiVersion": "2016-09-01",
        "dependsOn": [ ],
        "properties": {
            "mode": "Incremental",
            "templateLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            },
            "parametersLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateParametersFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            }
        }    
    }],
"outputs": {}
```

Bu şablonu içeren depodaki klasör, **NestOne.json** ve **NestOne.parameters.json**dosyalarını içeren bir alt klasöre `nestedtemplates` sahiptir. **Azuredeploy.json,** URI şablonu için yapılsa konumu, iç içe şablon klasörü, iç içe şablon dosya adı kullanılarak oluşturulur. Benzer şekilde, parametreler için URI, yapıtların konumu, iç içe olan şablon klasörü ve iç içe alınan şablon için parametre dosyası kullanılarak oluşturulur. 

İşte Visual Studio aynı proje yapısının görüntüsü: 

![Visual Studio'da proje yapısı](./media/deploy-nested-template-environments/visual-studio-project-structure.png)

Birincil klasöre ek klasörler ekleyebilirsiniz, ancak tek bir düzeyden daha derin değil. 

## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında ayrıntılar için aşağıdaki makalelere bakın: 

- [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Azure DevTest Labs'da ortak ortamları yapılandırma ve kullanma](devtest-lab-configure-use-public-environments.md)
- [Azure DevTest Labs'da bir ortamı laboratuvarınızın sanal ağına bağlayın](connect-environment-lab-virtual-network.md)