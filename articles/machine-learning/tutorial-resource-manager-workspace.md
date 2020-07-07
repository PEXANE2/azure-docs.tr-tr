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
ms.openlocfilehash: 098d82e6521a4a355ac31809937b589f984816f2
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86027180"
---
# <a name="tutorial-deploy-an-azure-machine-learning-workspace-using-an-arm-template"></a>Öğretici: ARM şablonunu kullanarak bir Azure Machine Learning çalışma alanı dağıtma

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğreticide, Azure Resource Manager şablonu kullanarak bir Azure Machine Learning çalışma alanı oluşturma (ARM şablonu) gösterilmektedir. Azure Machine Learning çalışma alanları, tüm makine öğrenme varlıklarınızı temel veri kümelerinden dağıtılan modellere göre düzenler. Çalışma alanları, denemeleri oluşturma, çalıştırma ve gözden geçirme, eğitiminizi yönetme ve dağıtılmış bilgi işlem kaynaklarınızı yönetme ve dağıtılan modelleri izleme ve sürümü oluşturma konusunda iş arkadaşlarınızla işbirliği yapabileceğiniz tek bir konumdur.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure 'A dağıt** düğmesini seçin. Şablon Azure portal açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-machine-learning-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/services/machine-learning/) oluşturun.

* **Yerel ortamınızdan**bu belgedeki CLI komutlarını kullanmak IÇIN [Azure CLI](/cli/azure/install-azure-cli)gerekir.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-machine-learning-create/).

:::code language="json" source="~/quickstart-templates/101-machine-learning-create/azuredeploy.json" range="1-258" highlight="224-254":::

Aşağıdaki kaynaklar şablonda tanımlanmıştır:

* [Microsoft. MachineLearningServices/Workspaces](/azure/templates/microsoft.machinelearningservices/workspaces): BIR Azure ML çalışma alanı oluşturun. Bu şablonda, konum ve ad kullanıcının geçireme veya etkileşimli olarak girebileceği parametrelerdir.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Azure CLı 'dan şablonu kullanmak için oturum açın ve aboneliğinizi seçin (bkz. [Azure CLI Ile oturum açma](/cli/azure/authenticate-azure-cli)). Ardından şunu çalıştırın:

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

1. Oluşturulan kaynak grubu ve Azure ML çalışma alanı adlarının temelini oluşturacak bir proje adı.
1. Dağıtımı yapmak istediğiniz Azure konumu.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Azure ML çalışma alanınızı görmek için:

1. https://portal.azure.com kısmına gidin.
1. Giriş yapın.
1. Yeni oluşturduğunuz çalışma alanını seçin.

Azure Machine Learning giriş sayfasını görürsünüz:

:::image type="content" source="media/tutorial-resource-manager-workspace/workspace-home.png" alt-text="Azure ML çalışma alanının ekran görüntüsü":::

Dağıtımla ilişkili tüm kaynakları görmek için, sol üst taraftaki bağlantıyı çalışma alanı adıyla (ekran görüntüsünde `my_templated_ws` ) tıklatın. Bu bağlantı sizi Azure portal kaynak grubuna götürür. Kaynak grubu adı `{projectName}rg` ve çalışma alanı olarak adlandırılır `{projectName}ws` .

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu çalışma alanını kullanmak istemiyorsanız, silin. Çalışma alanı bir depolama hesabı gibi diğer kaynaklarla ilişkili olduğundan, muhtemelen oluşturduğunuz kaynak grubunun tamamını silmek isteyeceksiniz. **Sil** düğmesine tıklayıp onay yazarak kaynak grubunu Portal kullanarak silebilirsiniz. Ya da, kaynak grubunu CLı 'dan ile silebilirsiniz:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ARM şablonundan bir Azure Machine Learning çalışma alanı oluşturdunuz. Azure Machine Learning araştırmak isterseniz öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Python SDK ile ilk ML denemenizi oluşturmaya başlama](tutorial-1st-experiment-sdk-setup.md)
