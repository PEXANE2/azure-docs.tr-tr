---
title: Blob Depolama olaylarını Web uç noktasına gönder-şablon
description: BLOB depolama hesabı oluşturmak ve olaylarını abone olmak için Azure Event Grid ve Azure Resource Manager şablonu kullanın. Olayları bir Web kancasına gönder. '
ms.date: 07/07/2020
ms.topic: quickstart
ms.openlocfilehash: e1d266f747671542059e0610cfcdd8cd12594e51
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460448"
---
# <a name="route-blob-storage-events-to-web-endpoint-by-using-an-arm-template"></a>ARM şablonunu kullanarak BLOB Depolama olaylarını Web uç noktasına yönlendirme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede bir BLOB depolama hesabı oluşturmak, söz konusu BLOB depolama olaylarına abone olmak ve sonucu görüntülemek için bir olayı tetiklemek üzere bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Bununla birlikte, bu makaleyi basitleştirmek için olayları iletilerin toplandığı ve görüntülendiği bir web uygulamasına gönderirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-event-grid-subscription-and-storage%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

### <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Blob depolamasındaki olaylara abone olmadan önce olay iletisi için uç noktayı oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu hızlı başlangıcı basitleştirmek için, olay iletilerini görüntüleyin bir [önceden oluşturulmuş web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtırsınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

    [Azure’a dağıtın](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json)
1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

1. Siteyi görürsünüz ancak henüz yayımlanmış olay yoktur.

   ![Yeni siteyi görüntüleme](./media/blob-event-quickstart-portal/view-site.png)

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-event-grid-subscription-and-storage/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-event-grid-subscription-and-storage/azuredeploy.json" range="1-91" highlight="40-85":::

Şablonda iki Azure kaynağı tanımlanmıştır:

* [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageaccounts): bir Azure depolama hesabı oluşturun.
* [**Microsoft. EventGrid/Systemkonular**](/azure/templates/microsoft.eventgrid/systemtopics): depolama hesabı için belirtilen adla bir sistem konusu oluşturun.
* [**Microsoft. EventGrid/systemtopic/Eventabonelikler**](/azure/templates/microsoft.eventgrid/systemtopics/eventsubscriptions): sistem konusu için Azure Event Grid bir abonelik oluşturun.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure 'da oturum açmak ve bir şablonu açmak için aşağıdaki bağlantıyı seçin. Şablon bir anahtar kasası ve gizli dizi oluşturur.

    [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-event-grid-subscription-and-storage%2Fazuredeploy.json)

2. **Uç noktasını**belirtin: Web uygulamanızın URL 'sini sağlayın ve `api/updates` giriş sayfası URL 'sine ekleyin.
3. Şablonu dağıtmak için **satın al** ' ı seçin.

  Azure portal, şablonu dağıtmak için burada kullanılır. Azure PowerShell, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-powershell.md).

> [!NOTE]
> Daha fazla Azure Event Grid [şablon örneği bulabilirsiniz](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid&pageNumber=1&sort=Popular).

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. Göz simgesini seçerek olay verilerini genişletin. Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Web uygulaması aboneliği doğrulamak için kod içerir.

![Abonelik olayını görüntüleme](./media/blob-event-quickstart-portal/view-subscription-event.png)

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim.

Dosya yükleyerek Blob depolaması için bir olay tetiklemiş olursunuz. Dosyanın belirli bir içeriğe sahip olmasına gerek yoktur. Bu makalede testfile.txt adlı bir dosyaya sahip olduğunuz kabul edilmektedir ancak herhangi bir dosyayı kullanabilirsiniz.

Dosyayı Azure Blob depolama alanına yüklediğinizde, Event Grid abone olurken yapılandırdığınız uç noktaya bir ileti gönderir. İleti JSON biçimindedir ve bir veya daha fazla olaya sahip bir dizi içerir. Aşağıdaki örnekte, JSON iletisi bir olay içeren bir dizi içerir. Web uygulamanızı görüntülediğinizde blob oluşturma olayının gönderildiğini göreceksiniz.

![Sonuçları görüntüleme](./media/blob-event-quickstart-portal/view-results.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında [kaynak grubunu silin](../azure-resource-manager/management/delete-resource-group.md?tabs=azure-portal#delete-resource-group
).

## <a name="next-steps"></a>Sonraki adımlar

Azure Resource Manager şablonları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Resource Manager belgeleri](../azure-resource-manager/index.yml)
* [Azure Resource Manager şablonlarda kaynakları tanımlama](/azure/templates/)
* [Azure Hızlı Başlangıç şablonları](https://azure.microsoft.com/resources/templates/)
* [Azure Event Grid şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid).
