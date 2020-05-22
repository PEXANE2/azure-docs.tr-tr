---
title: Öğretici-Azure ML çalışma alanı oluşturma-Kaynak Yöneticisi şablonu
description: Bu öğreticide, makine öğrenimi için hızlı bir şekilde Azure çalışma alanı dağıtmak üzere bir Azure Resource Manager şablonu kullanacaksınız
services: machine-learning
author: lobrien
ms.author: laobri
ms.custom: subject-armqs
ms.date: 05/26/2020
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.openlocfilehash: b6eac711ed6c420330b036cb6094c841508cee9f
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83791582"
---
# <a name="tutorial-deploy-an-azure-machine-learning-workspace-using-a-resource-manager-template"></a>Öğretici: Kaynak Yöneticisi şablonu kullanarak bir Azure Machine Learning çalışma alanı dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğreticide, bir Azure Resource Manager şablonu kullanarak bir Azure Machine Learning çalışma alanı oluşturma gösterilmektedir. Azure Machine Learning çalışma alanları, tüm makine öğrenme varlıklarınızı temel veri kümelerinden dağıtılan modellere göre düzenler. Çalışma alanları, denemeleri oluşturma, çalıştırma ve gözden geçirme, eğitiminizi yönetme ve dağıtılmış bilgi işlem kaynaklarınızı yönetme ve dağıtılan modelleri izleme ve sürümü oluşturma konusunda iş arkadaşlarınızla işbirliği yapabileceğiniz tek bir konumdur.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun

* **Yerel ortamınızdan**bu belgedeki CLI komutlarını kullanmak IÇIN [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) gerekir

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-machine-learning-create/).

:::code language="json" source="~/quickstart-templates/101-machine-learning-create/azuredeploy.json":::

Aşağıdaki kaynaklar şablonda tanımlanmıştır:

* [Microsoft. MachineLearningServices/Workspaces](/azure/templates/microsoft.machinelearningservices/workspaces): BIR Azure ML çalışma alanı oluşturun. Bu şablonda, konum ve ad kullanıcının geçireme veya etkileşimli olarak girebileceği parametrelerdir.

### <a name="deploy-the-template"></a>Şablonu dağıtma 

Azure CLı 'dan şablonu kullanmak için oturum açın ve aboneliğinizi seçin (bkz. [Azure CLI Ile oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)). Ardından şunu çalıştırın:

```azurecli-interactive
read -p "Enter a project name that is used for generating resource names:" projectName &&
read -p "Enter the location (i.e. centralus):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-machine-learning-create/azuredeploy.json" &&
resourceGroupName="${projectName}rg" &&
workspaceName="${projectName}ws" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri $templateUri --parameters workspaceName=$workspaceName location=$location && 
echo "Press [ENTER] to continue ..." &&
read
```

Yukarıdaki komutu çalıştırdığınızda şunu girin:

1. Oluşturulan kaynak grubu ve Azure ML çalışma alanı adlarının temelini oluşturacak bir proje adı
1. Dağıtımı yapmak istediğiniz Azure konumu

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Azure ML çalışma alanınızı görmek için:

1. Şuraya gidin: https://portal.azure.com 
1. Oturum açın 
1. Yeni oluşturduğunuz çalışma alanını seçin

Azure Machine Learning giriş sayfasını görürsünüz: 

:::image type="content" source="media/tutorial-resource-manager-workspace/workspace-home.png" alt-text="Azure ML çalışma alanının ekran görüntüsü":::

Dağıtımla ilişkili tüm kaynakları görmek için, sol üst taraftaki bağlantıyı çalışma alanı adıyla (ekran görüntüsünde `my_templated_ws` ) tıklatın. Bu bağlantı sizi Azure portal kaynak grubuna götürür. Kaynak grubu adı `{projectName}rg` ve çalışma alanı olarak adlandırılır `{projectName}ws` .

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu çalışma alanını kullanmak istemiyorsanız, silin. Çalışma alanı bir depolama hesabı gibi diğer kaynaklarla ilişkili olduğundan, muhtemelen oluşturduğunuz kaynak grubunun tamamını silmek isteyeceksiniz. "Sil" düğmesine tıklayarak ve onayladıktan sonra, portalı kullanarak kaynak grubunu silebilirsiniz. Ya da, kaynak grubunu CLı 'dan ile silebilirsiniz: 

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Resource Manager şablonundan bir Azure Machine Learning çalışma alanı oluşturdunuz. Azure Machine Learning araştırmak isterseniz öğreticiye devam edin. 

> [!div class="nextstepaction"]
> [Öğretici: Python SDK ile ilk ML denemenizi oluşturmaya başlama](tutorial-1st-experiment-sdk-setup.md)
