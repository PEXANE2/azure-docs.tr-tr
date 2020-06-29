---
title: Azure DevTest Labs içinde iç içe şablon ortamları dağıtma
description: Azure DevTest Labs sahip ortamlar sağlamak üzere iç içe Azure Resource Manager şablonlarını dağıtmayı öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 39002e286fafd4f813333a14ed86256a517897e9
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85481349"
---
# <a name="deploy-nested-azure-resource-manager-templates-for-testing-environments"></a>Ortamları test etmek için iç içe Azure Resource Manager şablonları dağıtma
İç içe dağıtım, diğer Azure Resource Manager şablonlarını Ana Kaynak Yöneticisi şablonu içinden yürütmelerine olanak tanır. Bu, dağıtımınızı hedeflenen ve amaca özgü bir dizi şekilde parçalara ayırmayı sağlar. Test, yeniden kullanım ve okunabilirlik açısından avantajlar sağlar. [Azure kaynaklarını dağıtmaya yönelik bağlı şablonları kullanma](../azure-resource-manager/templates/linked-templates.md) makalesinde, bu çözüme birçok kod örneği ile ilgili iyi bir genel bakış sağlanır. Bu makale, Azure DevTest Labs özgü bir örnek sağlar. 

## <a name="key-parameters"></a>Anahtar parametreleri
Sıfırdan kendi Kaynak Yöneticisi şablonunuzu oluşturabileceğiniz gibi, Visual Studio 'da [Azure Kaynak grubu projesi](../azure-resource-manager/templates/create-visual-studio-deployment-project.md) kullanmanızı öneririz, bu da şablonları geliştirmeyi ve hata ayıklamanızı kolaylaştırır. azuredeploy.jsiçin iç içe geçmiş dağıtım kaynağı eklediğinizde, Visual Studio şablonu daha esnek hale getirmek için birkaç öğe ekler. Bu öğeler ikincil şablon ve parametreler dosyasının bulunduğu alt klasörü, ana şablon dosyası içindeki değişken adlarını ve yeni dosyalar için depolama konumu için iki parametreyi içerir. **_ArtifactsLocation** ve **_ArtifactsLocationSasToken** , DevTest Labs tarafından kullanılan anahtar parametrelerdir. 

DevTest Labs 'nin ortamlarla nasıl çalıştığı konusunda bilgi sahibi değilseniz, bkz. [Azure Resource Manager şablonlarıyla çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md). Şablonlarınız DevTest Labs içindeki laboratuvarla bağlantılı depoda depolanır. Bu şablonlar ile yeni bir ortam oluşturduğunuzda, dosyalar laboratuvardaki bir Azure depolama kapsayıcısına taşınır. İç içe geçmiş dosyaları tanımlayabilmek ve kopyalayabilmek için DevTest Labs _artifactsLocation ve _artifactsLocationSasToken parametrelerini tanımlar ve alt klasörleri depolama kapsayıcısına kopyalar. Ardından, konum ve paylaşılan erişim Imzası (SaS) belirtecini parametrelere otomatik olarak ekler. 

## <a name="nested-deployment-example"></a>İç içe dağıtım örneği
İç içe dağıtımın basit bir örneği aşağıda verilmiştir:

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

Bu şablonu içeren depodaki klasörün bir alt klasörü vardır `nestedtemplates` **NestOne.js** ve **NestOne.parameters.js**. **Üzerindeazuredeploy.js**, şablon URI 'si yapıt konumu, iç içe şablon klasörü, iç içe şablon dosya adı kullanılarak oluşturulmuştur. Benzer şekilde, parametreler için URI, iç içe geçmiş şablon için yapıt konumu, iç içe şablon klasörü ve parametre dosyası kullanılarak oluşturulmuştur. 

Visual Studio 'da aynı proje yapısının görüntüsü aşağıda verilmiştir: 

![Visual Studio 'da proje yapısı](./media/deploy-nested-template-environments/visual-studio-project-structure.png)

Birincil klasöre başka klasörler ekleyebilirsiniz, ancak tek bir düzeyden daha derin olamaz. 

## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkındaki ayrıntılar için aşağıdaki makalelere bakın: 

- [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Azure DevTest Labs ortak ortamları yapılandırma ve kullanma](devtest-lab-configure-use-public-environments.md)
- [Azure DevTest Labs ortamında bir ortamı laboratuvarınızın sanal ağına bağlama](connect-environment-lab-virtual-network.md)