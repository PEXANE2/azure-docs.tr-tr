---
title: Azure Resource Manager (ARM) şablonu kullanarak belirli güvenlik uyarıları için Güvenlik Otomasyonu oluşturma
description: Bir Azure Resource Manager (ARM) şablonu kullanarak belirli güvenlik merkezi uyarıları tarafından tetiklenecek bir Logic App tetiklemeye yönelik bir Azure Güvenlik Merkezi Otomasyonu oluşturmayı öğrenin
services: azure-resource-manager
author: memildin
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: memildin
ms.date: 08/20/2020
ms.openlocfilehash: ecef318978194ac3773c54e2d9c960781086de65
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691874"
---
# <a name="quickstart-create-an-automatic-response-to-a-specific-security-alert-using-an-azure-resource-manager-template-arm-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak belirli bir güvenlik uyarısına otomatik yanıt oluşturma (ARM şablonu)

Bu hızlı başlangıçta, Azure Güvenlik Merkezi tarafından belirli güvenlik uyarıları alındığında bir mantıksal uygulamayı tetikleyen bir iş akışı otomasyonu oluşturmak için bir Azure Resource Manager şablonunun (ARM şablonu) nasıl kullanılacağı açıklanmaktadır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-securitycenter-create-automation-for-alertnamecontains%2fazuredeploy.json)


## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Azure Güvenlik Merkezi 'nin iş akışı Otomasyonu özelliği ile çalışmak için gerekli rollerin ve izinlerin bir listesi için bkz. [Workflow Automation](workflow-automation.md).


## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-securitycenter-create-automation-for-alertnamecontains/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-securitycenter-create-automation-for-alertnamecontains/azuredeploy.json":::

### <a name="relevant-resources"></a>İlgili kaynaklar

- [**Microsoft. Security/Automations**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-securitycenter-create-automation-for-alertnamecontains/#microsoftsecurity-resource-provider): belirli bir dize Içeren bir Azure Güvenlik Merkezi uyarısını aldıktan sonra mantıksal uygulamayı tetikleyecek Otomasyon.
- [**Microsoft. Logic/iş akışları**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-securitycenter-create-automation-for-alertnamecontains/#microsoftlogic-resource-provider): boş bir Triggerable Logic App.

Diğer Güvenlik Merkezi hızlı başlangıç şablonları için, bu [topluluğa katkıda bulunulan şablonlar](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Security)' a bakın.

## <a name="deploy-the-template"></a>Şablonu dağıtma

  - **PowerShell**:
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <resource-group-name> -Location <resource-group-location> #use this command when you need to create a new resource group for your deployment
    New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-securitycenter-create-automation-for-alertnamecontains/azuredeploy.json
    ```

  - **CLI**:
    ```azurecli-interactive
    az group create --name <resource-group-name> --location <resource-group-location> #use this command when you need to create a new resource group for your deployment
    az group deployment create --resource-group <my-resource-group> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-securitycenter-create-automation-for-alertnamecontains/azuredeploy.json
    ```

  - **Portal**:

    [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-securitycenter-create-automation-for-alertnamecontains%2fazuredeploy.json)

    Bu dağıtım seçeneği hakkında daha fazla bilgi edinmek için bkz. [GitHub deposundan şablon dağıtmak için dağıtım düğmesi kullanma](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-to-azure-button).

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

İş akışı otomasyonunun dağıtıldığını denetlemek için Azure portal kullanın. 

1. [Azure Portal](https://portal.azure.com), **Güvenlik Merkezi**'ni açın.
1. Üst menü çubuğundan filtre simgesini seçin ve yeni iş akışı otomasyonunu dağıttığınız belirli bir abonelik seçin.
1. Güvenlik Merkezi 'nin kenar çubuğundan, **iş akışı Otomasyonu 'nu** açın ve yeni Otomasyonunuzu denetleyin.

    :::image type="content" source="./media/quickstart-automation-alert/validating-template-run.png" alt-text="Görüntü alt metni." lightbox="./media/quickstart-automation-alert/validating-template-run.png":::

    >[!TIP]
    > Aboneliğinizde çok sayıda iş akışı Otomasyonu varsa, **ada göre filtrele** seçeneğini kullanın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, Azure portal kullanarak iş akışı otomasyonunu silin.

1. [Azure Portal](https://portal.azure.com), **Güvenlik Merkezi**'ni açın.
1. Üst menü çubuğundan filtre simgesini seçin ve yeni iş akışı otomasyonunu dağıttığınız belirli bir abonelik seçin.
1. Güvenlik Merkezi 'nin kenar çubuğundan, **iş akışı Otomasyonu 'nu** açın ve silinecek Otomasyonu bulun.

    :::image type="content" source="./media/quickstart-automation-alert/deleting-workflow-automation.png" alt-text="Görüntü alt metni." lightbox="./media/quickstart-automation-alert/deleting-workflow-automation.png":::

1. Silinecek öğe onay kutusunu seçin.
1. Araç çubuğundan Sil ' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Şablon oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için, bkz.:

> [!div class="nextstepaction"]
> [Öğretici: ilk ARM şablonunuzu oluşturma ve dağıtma](/azure/azure-resource-manager/templates/template-tutorial-create-first-template)