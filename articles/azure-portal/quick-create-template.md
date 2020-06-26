---
title: Azure Resource Manager şablonu kullanarak Azure portal panosu oluşturma
description: Azure Resource Manager şablonu kullanarak Azure portal panosu oluşturmayı öğrenin.
author: mgblythe
ms.service: azure-portal
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: mblythe
ms.date: 06/15/2020
ms.openlocfilehash: bd68778eb3f0ca9c2af456cdb88cdcc9fe5862dc
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85393904"
---
# <a name="quickstart-create-a-dashboard-in-the-azure-portal-by-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak Azure portal bir pano oluşturma

Azure portal bir Pano, bulut kaynaklarınızın odaklanmış ve düzenlenmiş bir görünümüdür. Bu hızlı başlangıç, bir pano oluşturmak için Kaynak Yöneticisi şablonu dağıtma işlemine odaklanmaktadır. Pano, bir sanal makinenin (VM) ve bazı statik bilgi ve bağlantıların performansını gösterir.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bu hızlı başlangıç bölümünde oluşturduğunuz Pano, mevcut bir VM gerektirir. Bu adımları izleyerek bir VM oluşturun.

1. Azure portal Cloud Shell ' ni seçin.

    ![Azure portal şeritten Cloud Shell ' i seçin](media/quick-create-template/cloud-shell.png)

1. Aşağıdaki komutu kopyalayın ve bir kaynak grubu oluşturmak için komut istemine girin.

    ```powershell
    New-AzResourceGroup -Name SimpleWinVmResourceGroup -Location EastUS
    ```

    ![Komut istemine bir komut kopyalama](media/quick-create-template/command-prompt.png)

1. Aşağıdaki komutu kopyalayın ve kaynak grubunda bir VM oluşturmak için komut istemine girin.

    ```powershell
    New-AzVm `
        -ResourceGroupName "SimpleWinVmResourceGroup" `
        -Name "SimpleWinVm" `
        -Location "East US" 
    ```

1. VM için bir Kullanıcı adı ve parola girin. Bu yeni bir Kullanıcı adı ve paroladır; Örneğin, Azure 'da oturum açmak için kullandığınız hesap değildir. Daha fazla bilgi için bkz. [Kullanıcı adı gereksinimleri](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm) ve [Parola gereksinimleri](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

    VM dağıtımı artık başlar ve genellikle birkaç dakika sürer. Dağıtım tamamlandıktan sonra bir sonraki bölüme geçin.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-azure-portal-dashboard/). Bu makalenin şablonu burada görüntülenemeyecek kadar uzun. Şablonu görüntülemek için bkz. [azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-azure-portal-dashboard/azuredeploy.json). [Microsoft. Portal/panolar](/azure/templates/microsoft.portal/dashboards) -şablonda bir Azure kaynağı tanımlanmıştır. Azure Portal bir pano oluşturun.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın.

    [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azure-portal-dashboard%2Fazuredeploy.json)

1. Aşağıdaki değerleri seçin veya girin ve ardından **gözden geçir + oluştur**' u seçin.

    ![Şablon Kaynak Yöneticisi, pano oluşturun, portalı dağıtın](media/quick-create-template/create-dashboard-using-template-portal.png)

    Belirtilmediği takdirde, Panoyu oluşturmak için varsayılan değerleri kullanın.

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Simplewinvmresourcegroup**öğesini seçin.
    * **Konum**: **Doğu ABD**seçin.
    * **Sanal makine adı**: **simplewinvm**girin.
    * **Sanal makine kaynak grubu**: **Simplewinvmresourcegroup**girin.

1. **Oluştur** veya **satın al**' ı seçin. Pano başarıyla dağıtıldıktan sonra bir bildirim alırsınız:

    ![Kaynak Yöneticisi şablonu, pano oluşturun, Portal bildirimi dağıtın](media/quick-create-template/resource-manager-template-portal-deployment-notification.png)

Azure portal şablonu dağıtmak için kullanıldı. Azure portalının yanı sıra Azure PowerShell, Azure CLI ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Panonun başarıyla oluşturulduğunu ve VM 'den verileri göremediklerini denetleyin.

1. Azure portal, **Pano**' yı seçin.

    ![Panoya gezinti Azure portal](media/quick-create-template/navigate-to-dashboards.png)

1. Pano sayfasında, **basıt VM panosu**' nu seçin.

    ![Basit VM panosuna git](media/quick-create-template/select-simple-vm-dashboard.png)

1. ARM şablonunun oluşturduğu panoyu gözden geçirin. İçeriğin bazılarının statik olduğunu, ancak başlangıcında oluşturduğunuz sanal makinenin performansını gösteren grafikler de olduğunu görebilirsiniz.

    ![Basit VM panosunu gözden geçirme](media/quick-create-template/review-simple-vm-dashboard.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

VM 'yi ve ilişkili panoyu kaldırmak istiyorsanız, bunları içeren kaynak grubunu silin.

1. Azure portal, **Simplewinvmresourcegroup**öğesini arayın ve arama sonuçlarında bunu seçin.

1. **Simplewınvmresourcegroup** sayfasında, **kaynak grubunu sil**' i seçin, onaylamak için kaynak grubunun adını girin ve **Sil**' i seçin.

    ![Kaynak grubunu silme](media/quick-create-template/delete-resource-group.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure portal panolar hakkında daha fazla bilgi için bkz.:

> [!div class="nextstepaction"]
> [Azure portalında pano oluşturma ve paylaşma](azure-portal-dashboards.md)