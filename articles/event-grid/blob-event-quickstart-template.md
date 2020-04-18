---
title: Blob depolama olaylarını web bitiş noktasına gönderme - şablon
description: Blob depolama hesabı oluşturmak ve etkinliklerine abone olmak için Azure Olay Ağıtını ve Azure Kaynak Yöneticisi şablonunu kullanın. Olayları bir Webhook'a gönderin.
services: event-grid
keywords: ''
author: spelluru
ms.author: spelluru
ms.date: 02/27/2020
ms.topic: quickstart
ms.service: event-grid
ms.custom: subject-armqs
ms.openlocfilehash: 86dc7a4ed05ceae5c7a641ffef23bd75ec48ceea
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605541"
---
# <a name="route-blob-storage-events-to-web-endpoint-by-using-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak Blob depolama olaylarını web bitiş noktasına yönlendirin

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, blob depolama hesabı oluşturmak, bu blob depolama için olaylara abone olmak ve sonucu görüntülemek için bir olay tetiklemek için bir **Azure Kaynak Yöneticisi şablonu** kullanın. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Bununla birlikte, bu makaleyi basitleştirmek için olayları iletilerin toplandığı ve görüntülendiği bir web uygulamasına gönderirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

### <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Blob depolamasındaki olaylara abone olmadan önce olay iletisi için uç noktayı oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu hızlı başlangıcı basitleştirmek için, olay iletilerini görüntüleyin bir [önceden oluşturulmuş web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtırsınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

    [Azure’a dağıtma](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json)
1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

1. Siteyi görürsünüz ancak henüz yayımlanmış olay yoktur.

   ![Yeni siteyi görüntüleme](./media/blob-event-quickstart-portal/view-site.png)

## <a name="create-a-storage-account-with-an-event-grid-subscription"></a>Olay Izgara aboneliği yle depolama hesabı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)

[!code-json[<Azure Resource Manager template create Blob storage Event Grid subscription>](~/quickstart-templates/101-event-grid-subscription-and-storage/azuredeploy.json)]

Şablonda iki Azure kaynağı tanımlanır:

* [**Microsoft.Storage/storageHesapları:**](/azure/templates/microsoft.storage/storageaccounts)bir Azure Depolama hesabı oluşturun.
* [**"Microsoft.Storage/storageAccounts/providers/eventSubscriptions**](/azure/templates/microsoft.eventgrid/eventsubscriptions): depolama hesabı için bir Azure Olay Ağı aboneliği oluşturun.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure'da oturum açmak ve şablon açmak için aşağıdaki bağlantıyı seçin. Şablon bir anahtar kasa ve bir sır oluşturur.

    [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-event-grid-subscription-and-storage%2Fazuredeploy.json)

2. Bitiş **noktasını**belirtin: web uygulamanızın URL'sini sağlayın ve ana sayfa URL'sine ekleyin. `api/updates`
3. Şablonu dağıtmak için **Satın Alma'yı** seçin.

  Azure portalı burada şablonu dağıtmak için kullanılır. Azure PowerShell, Azure CLI ve REST API'yi de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için [şablonları dağıt'a](../azure-resource-manager/templates/deploy-powershell.md)bakın.

> [!NOTE]
> Burada daha fazla Azure Olay Izgara [şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid)örnekleri bulabilirsiniz.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. Göz simgesini seçerek olay verilerini genişletin. Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Web uygulaması aboneliği doğrulamak için kod içerir.

![Abonelik olayını görüntüleme](./media/blob-event-quickstart-portal/view-subscription-event.png)

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim.

Dosya yükleyerek Blob depolaması için bir olay tetiklemiş olursunuz. Dosyanın belirli bir içeriğe sahip olmasına gerek yoktur. Bu makalede testfile.txt adlı bir dosyaya sahip olduğunuz kabul edilmektedir ancak herhangi bir dosyayı kullanabilirsiniz.

Dosyayı Azure Blob depolama alanına yüklediğinizde, Olay Grid abone yken yapılandırdığınız bitiş noktasına bir ileti gönderir. İleti JSON biçimindedir ve bir veya daha fazla olayı içeren bir dizi içerir. Aşağıdaki örnekte, JSON iletisi tek bir olay içeren bir dizi içerir. Web uygulamanızı görüntülediğinizde blob oluşturma olayının gönderildiğini göreceksiniz.

![Sonuçları görüntüleme](./media/blob-event-quickstart-portal/view-results.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, [kaynak grubunu silin.](../azure-resource-manager/management/delete-resource-group.md?tabs=azure-portal#delete-resource-group
)

## <a name="next-steps"></a>Sonraki adımlar

Azure Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Kaynak Yöneticisi belgeleri](/azure/azure-resource-manager)
* [Azure Kaynak Yöneticisi şablonlarında kaynakları tanımlama](/azure/templates/)
* [Azure Quickstart şablonları](https://azure.microsoft.com/resources/templates/)
* [Azure Olay Izgara şablonları.](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid)
