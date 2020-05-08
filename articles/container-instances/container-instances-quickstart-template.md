---
title: Hızlı başlangıç-coğrafi olarak çoğaltılan kayıt defteri Kaynak Yöneticisi şablonu oluşturma
description: Bu hızlı başlangıçta, yalıtılmış bir Azure Container örneğinde çalışan kapsayıcılı bir Web uygulamasını hızlıca dağıtmak için bir Azure Resource Manager şablonu kullanırsınız.
services: azure-resource-manager
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 04/30/2020
ms.openlocfilehash: f9f789d2f2c6c21e38f241b445c72b330c689a8d
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82930429"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-a-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure 'da kapsayıcı örneği dağıtma

Azure 'da sunucusuz Docker kapsayıcılarını basitlik ve hızla çalıştırmak için Azure Container Instances kullanın. Azure Kubernetes hizmeti gibi tam kapsayıcı düzenleme platformu gerekmiyorsa, bir uygulamayı isteğe bağlı olarak bir kapsayıcı örneğine dağıtın.

Bu hızlı başlangıçta, yalıtılmış bir Docker kapsayıcısını dağıtmak ve Web uygulamasını genel bir IP adresi ile kullanılabilir hale getirmek için bir Azure Resource Manager şablonu kullanırsınız. 

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Yok.

## <a name="create-a-container-instance"></a>Kapsayıcı örneği oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-linuxcontainer-public-ip).

[!code-json[<Azure Resource Manager template create geo-replicated registry>](~/quickstart-templates/101-aci-linuxcontainer-public-ip/azuredeploy.json)]

Aşağıdaki kaynak şablonda tanımlanmıştır:

* **[Microsoft. Containerınstance/containerGroups](/azure/templates/microsoft.containerinstance/containergroups)**: bir Azure Container grubu oluşturun. Bu şablon, tek bir kapsayıcı örneğinden oluşan bir grubu tanımlar.

Daha fazla Azure Container Instances şablon örneği [hızlı başlangıç şablonu galerisinde](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Containerinstance&pageNumber=1&sort=Popular)bulunabilir.

### <a name="deploy-the-template"></a>Şablonu dağıtma

 1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon, başka bir konumda bir kayıt defteri ve bir çoğaltma oluşturur.

    [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-aci-linuxcontainer-public-ip%2Fazuredeploy.json)

 2. Aşağıdaki değerleri seçin veya girin.

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni oluştur**' u seçin, kaynak grubu için benzersiz bir ad girin ve **Tamam**' ı seçin.
    * **Konum**: kaynak grubu için bir konum seçin. Örnek: **Orta ABD**.
    * **Ad**: örnek için oluşturulan adı kabul edin veya bir ad girin.
    * **Görüntü**: varsayılan görüntü adını kabul edin. Bu örnek Linux görüntüsü, statik bir HTML sayfasına hizmet veren Node. js ' de yazılmış küçük bir Web uygulamasını paketler. 

    Kalan özellikler için varsayılan değerleri kabul edin.

    Hüküm ve koşulları gözden geçirin. Kabul ediyorsanız, **yukarıda belirtilen hüküm ve koşulları kabul**ediyorum ' u seçin.

    ![Şablon Özellikleri](media/container-instances-quickstart-template/template-properties.png)

 3. Örnek başarıyla oluşturulduktan sonra bir bildirim alırsınız:

    ![Portal bildirimi](media/container-instances-quickstart-template/deployment-notification.png)

 Azure portal, şablonu dağıtmak için kullanılır. Azure portal ek olarak, Azure PowerShell, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-cli.md).

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Kapsayıcı örneğinin özelliklerini gözden geçirmek için Azure portal veya [Azure CLI](container-instances-quickstart.md) gibi bir araç kullanın.

1. Portalda Container Instances araması yapın ve oluşturduğunuz kapsayıcı örneğini seçin.

1. **Genel bakış** sayfasında, örneğin **durumunu** ve **IP adresini**aklınızda edin.

    ![Örneğe genel bakış](media/container-instances-quickstart-template/aci-overview.png)

2. Durumu *çalışmaya*başladıktan sonra tarayıcınızda IP adresine gidin. 

    ![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor](media/container-instances-quickstart-template/view-application-running-in-an-azure-container-instance.png)

### <a name="view-container-logs"></a>Kapsayıcı günlüklerini görüntüleme

Kapsayıcı örneğinin günlüklerini görüntülemek, kapsayıcınızın veya çalıştırdığı uygulamanın sorunlarını gidermede yardımcı olur.

Kapsayıcının günlüklerini görüntülemek için, **Ayarlar**altında **kapsayıcılar** > **Günlükler**' i seçin. Uygulamayı tarayıcınızda görüntülediğinizde HTTP GET isteğinin oluşturulduğunu görmeniz gerekir.

![Azure portalında kapsayıcı günlükleri](media/container-instances-quickstart-template/aci-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcı ile işiniz bittiğinde, kapsayıcı örneği için **genel bakış** sayfasında **Sil**' i seçin. Sorulduğunda silme işlemini onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel Microsoft görüntüsünden bir Azure Kapsayıcı örneği oluşturdunuz. Kapsayıcı görüntüsünü oluşturup özel bir Azure kapsayıcı kayıt defterinden dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Öğretici: Azure Container Instances dağıtım için bir kapsayıcı görüntüsü oluşturma](./container-instances-tutorial-prepare-app.md)

Şablon oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için, bkz.:

> [!div class="nextstepaction"]
> [Öğretici: ilk Azure Resource Manager şablonunuzu oluşturma ve dağıtma](../azure-resource-manager/templates/template-tutorial-create-first-template.md)