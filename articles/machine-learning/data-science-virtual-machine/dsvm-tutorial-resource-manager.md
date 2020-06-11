---
title: "Hızlı başlangıç: Veri Bilimi VM'si Kaynak Yöneticisi şablonu oluşturma"
titleSuffix: Azure Data Science Virtual Machine
description: Bu hızlı başlangıçta, bir Veri Bilimi Sanal Makinesi hızlı bir şekilde dağıtmak için Azure Resource Manager şablonu kullanacaksınız
services: machine-learning
author: lobrien
ms.author: laobri
ms.custom: subject-armqs
ms.date: 06/10/2020
ms.service: machine-learning
ms.subservice: data-science-vm
ms.topic: quickstart
ms.openlocfilehash: 7e3e45d96839be06deec238deccdc9873492d68f
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84660256"
---
# <a name="quickstart-create-an-ubuntu-data-science-virtual-machine-using-a-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Ubuntu Veri Bilimi Sanal Makinesi oluşturma
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu kullanarak Ubuntu 18,04 Veri Bilimi Sanal Makinesi oluşturma gösterilmektedir. Veri bilimi sanal makineleri, bir veri bilimi ve makine öğrenimi çerçeveleri ve araçları paketiyle önceden yüklenmiş bulut tabanlı sanal makinelerdir. GPU destekli işlem kaynaklarına dağıtıldığında, tüm araçlar ve kitaplıklar GPU 'YU kullanacak şekilde yapılandırılır. 

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun

* **Yerel ortamınızdan**bu belgedeki CLI komutlarını kullanmak IÇIN [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) gerekir

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-vm-ubuntu-DSVM-GPU-or-CPU/). Bu makale için tam şablon burada görüntülenemeyecek kadar uzun. Tüm şablonu görüntülemek için bkz. [azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-ubuntu-DSVM-GPU-or-CPU/azuredeploy.json). DSVM 'nin özelliklerini tanımlayan bölüm burada gösterilmektedir:

:::code language="json" source="~/quickstart-templates/101-vm-ubuntu-DSVM-GPU-or-CPU/azuredeploy.json" range="235-276":::

Aşağıdaki kaynaklar şablonda tanımlanmıştır:

* [Microsoft. COMPUTE/virtualMachines](/azure/templates/microsoft.compute/virtualmachines): bulut tabanlı bir sanal makine oluşturun. Bu şablonda, sanal makine Ubuntu 18,04 çalıştıran bir Veri Bilimi Sanal Makinesi olarak yapılandırılır.

### <a name="deploy-the-template"></a>Şablonu dağıtma 

Azure CLı 'dan şablonu kullanmak için oturum açın ve aboneliğinizi seçin (bkz. [Azure CLI Ile oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)). Ardından şunu çalıştırın:

```azurecli-interactive
read -p "Enter the name of the resource group to create:" resourceGroupName &&
read -p "Enter the Azure location (e.g., centralus):" location &&
read -p "Enter the authentication type (must be 'password' or 'sshPublicKey') :" authenticationType &&
read -p "Enter the login name for the administrator account (may not be 'admin'):" adminUsername &&
read -p "Enter administrator account secure string (value of password or ssh public key):" adminPasswordOrKey &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-ubuntu-DSVM-GPU-or-CPU/azuredeploy.json" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri $templateUri --parameters adminUsername=$adminUsername authenticationType=$authenticationType adminPasswordOrKey=$adminPasswordOrKey && 
echo "Press [ENTER] to continue ..." &&
read
```

Yukarıdaki komutu çalıştırdığınızda şunu girin:

1. DSVM ve ilişkili kaynakları içerecek şekilde oluşturmak istediğiniz kaynak grubunun adı. 
1. Dağıtımı yapmak istediğiniz Azure konumu
1. Kullanmak istediğiniz kimlik doğrulaması türü (dizeyi girin `password` veya `sshPublicKey` )
1. Yönetici hesabının oturum açma adı (Bu değer olmayabilir `admin` )
1. Hesap için parola veya SSH ortak anahtarının değeri

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Veri Bilimi Sanal Makinesi görmek için:

1. Şuraya gidin: https://portal.azure.com 
1. Oturum açın 
1. Az önce oluşturduğunuz kaynak grubunu seçin

Kaynak grubunun bilgilerini görürsünüz: 

:::image type="content" source="media/dsvm-tutorial-resource-manager/resource-group-home.png" alt-text="DSVM içeren temel bir kaynak grubunun ekran görüntüsü":::

Bilgi sayfasına gitmek için sanal makine kaynağına tıklayın. Burada, bağlantı ayrıntıları dahil olmak üzere VM hakkındaki bilgileri bulabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu sanal makineyi kullanmak istemiyorsanız, silin. DSVM, depolama hesabı gibi diğer kaynaklarla ilişkilendirildiğinden, muhtemelen oluşturduğunuz kaynak grubunun tamamını silmek isteyeceksiniz. "Sil" düğmesine tıklayarak ve onayladıktan sonra, portalı kullanarak kaynak grubunu silebilirsiniz. Ya da, kaynak grubunu CLı 'dan ile silebilirsiniz: 

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Resource Manager şablonundan Veri Bilimi Sanal Makinesi oluşturdunuz. 

> [!div class="nextstepaction"]
> [Örnek programlar & ML izlenecek yolları](dsvm-samples-and-walkthroughs.md)
