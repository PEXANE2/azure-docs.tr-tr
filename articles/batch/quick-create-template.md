---
title: Azure hızlı başlangıç-Batch hesabı oluşturma-Azure Resource Manager şablonu
description: Azure CLI ile bir Batch işi çalıştırmayı hızlı bir şekilde öğrenin. Komut satırından veya betiklerden Azure kaynaklarını oluşturun ve yönetin.
ms.topic: quickstart
ms.date: 08/17/2020
ms.custom: subject-armqs
ms.openlocfilehash: c59e8891678d9fc1af7c4fe78e4ee2d267ec99b4
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88511397"
---
# <a name="quickstart-create-a-batch-account-by-using-arm-template"></a>Hızlı başlangıç: ARM şablonunu kullanarak Batch hesabı oluşturma

İşlem kaynakları (işlem düğümleri havuzları) ve Batch işleri oluşturmak için bir Batch hesabına ihtiyacınız vardır. Bir Azure Depolama hesabını Batch hesabınıza bağlayabilirsiniz. Bu, uygulamaları dağıtmak ve gerçek hayatta çok sayıda iş yükü için giriş ve çıkış verilerini depolamak için yararlıdır. Bu hızlı başlangıçta, depolama dahil olmak üzere bir Batch hesabı oluşturmak için bir Azure Resource Manager şablonunun (ARM şablonu) nasıl kullanılacağı gösterilmektedir. Bu hızlı başlangıcı tamamladıktan sonra, Batch hizmetinin temel kavramlarını anlayacak ve Batch’i daha büyük ölçekte daha gerçekçi iş yükleri ile denemeye hazır olacaksınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-batchaccount-with-storage%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Etkin bir Azure aboneliğiniz olmalıdır.

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-batchaccount-with-storage/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-batchaccount-with-storage/azuredeploy.json" range="1-80" highlight="36-69":::

Şablonda iki Azure kaynağı tanımlanmıştır:

- [Microsoft. Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts): bir depolama hesabı oluşturur.
- [Microsoft.Batch/batchAccounts](/azure/templates/microsoft.batch/batchaccounts): bir Batch hesabı oluşturur.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir Azure Batch hesabı ve depolama hesabı oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-batchaccount-with-storage%2Fazuredeploy.json)

1. Aşağıdaki değerleri seçin veya girin.

   ![Kaynak Yöneticisi şablonu, Batch hesabı oluşturma, Portal dağıtma](media/quick-create-template/batch-template.png)

   - **Abonelik**: Bir Azure aboneliği seçin.
   - **Kaynak grubu**: **Yeni oluştur**' u seçin, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam**' a tıklayın.
   - **Konum**: Bir konum seçin. Örneğin **Orta ABD**.
   - **Batch hesap adı**: varsayılan değeri bırakın.
   - **Storage Accountsku 'su**: bir depolama hesabı türü seçin. Örneğin, **Standard_LRS**.
   - **Konum**: kaynakların kaynak grubuyla aynı konumda olması için varsayılan olarak bırakın.
   - Yukarıdaki hüküm ve koşullar durumunu kabul ediyorum: öğesini **seçin**.

1. **Satın al**'ı seçin.

Birkaç dakika sonra Batch hesabının başarıyla oluşturulduğunu belirten bir bildirim görmeniz gerekir.

Bu örnekte, Azure portal şablonu dağıtmak için kullanılır. Azure portal ek olarak, Azure PowerShell, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Oluşturduğunuz kaynak grubuna giderek Azure portal dağıtımı doğrulayabilirsiniz. **Genel bakış** ekranında Batch hesabının ve depolama hesabının mevcut olduğundan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki [öğreticilerle](./tutorial-parallel-dotnet.md)çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmak isteyebilirsiniz. Ya da artık gerekmiyorsa, [kaynak grubunu silebilirsiniz](../azure-resource-manager/management/delete-resource-group.md?tabs=azure-portal#delete-resource-group), bu, Batch hesabını ve oluşturduğunuz depolama hesabını da silecektir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Batch hesabı ve bir depolama hesabı oluşturdunuz. Azure Batch hakkında daha fazla bilgi için Azure Batch öğreticilerine devam edin.

> [!div class="nextstepaction"]
> [Azure Batch öğreticileri](./tutorial-parallel-dotnet.md)
