---
title: 'Hızlı başlangıç: Web bitiş noktasına özel etkinlikler gönderme - Olay Izgara, Azure portalı'
description: 'Hızlı başlangıç: Özel bir konu yayınlamak için Azure Etkinlik Ağıt ve Azure portalını kullanın ve söz konusu konuyla ilgili etkinliklere abone olun. Olaylar bir web uygulaması tarafından işlenir.'
services: event-grid
keywords: ''
author: spelluru
ms.author: spelluru
ms.date: 11/05/2019
ms.topic: quickstart
ms.service: event-grid
ms.custom: seodec18
ms.openlocfilehash: 9edeecdfb0c0e7ef0ef6e9d1704d81b844ac8c53
ms.sourcegitcommit: fab450a18a600d72b583ecfbe6c5e53afd43408c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80293764"
---
# <a name="quickstart-route-custom-events-to-web-endpoint-with-the-azure-portal-and-event-grid"></a>Hızlı başlangıç: Azure portalı ve Olay Ağıtı ile özel olayları web bitiş noktasına yönlendirin

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Azure portalını kullanarak özel bir konu oluşturur, bu özel konuya abone olur ve sonucu görüntülemek için olayı tetiklersiniz. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Bununla birlikte, bu makaleyi basitleştirmek için olayları iletilerin toplandığı ve görüntülendiği bir web uygulamasına gönderirsiniz.

İşiniz bittiğinde, olay verilerinin web uygulamasına gönderildiğini görürsünüz.

![Sonuçları görüntüleme](./media/custom-event-quickstart-portal/view-result.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [event-grid-register-provider-portal.md](../../includes/event-grid-register-provider-portal.md)]

## <a name="create-a-custom-topic"></a>Özel konu oluşturma

Event grid konusu, olaylarınızı göndereceğiniz kullanıcı tanımlı bir uç nokta sağlar. 

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Sol navigasyon menüsündeki **Tüm hizmetleri** seçin, **Olay Izgarasını**arayın ve **Olay Izgara Konuları'nı**seçin. 

    ![Olay Izgara Konularını Seçin](./media/custom-event-quickstart-portal/select-event-grid-topics.png)
3. Olay **Izgara Konuları** sayfasında araç çubuğuna **+ Ekle'yi** seçin. 

    ![Olay Izgara Konu ekle düğmesi](./media/custom-event-quickstart-portal/add-event-grid-topic-button.png)
4. Konu **Oluştur** sayfasında aşağıdaki adımları izleyin:
    1. Özel konu için benzersiz bir **ad** sağlayın. Konu adı bir DNS girdisi ile temsil edildiğinden konu adı benzersiz olmalıdır. Görüntüde gösterilen adı kullanmayın. Bunun yerine, kendi adınızı oluşturun - 3-50 karakter arasında olmalı ve yalnızca a-z, A-Z, 0-9 ve "-" değerlerini içermelidir.
    2. Azure **aboneliğinizi**seçin.
    3. Varolan bir kaynak grubu seçin veya **yeni oluştur'u**seçin ve **kaynak grubu**için bir **ad** girin.
    4. Olay ızgarası konusu için bir **konum** seçin.
    5. **Olay Şeması** alanı için varsayılan değeri **Olay Izgara Şeması'nı** tutun. 

       ![Konu sayfası oluşturma](./media/custom-event-quickstart-portal/create-custom-topic.png)
    6. **Oluştur'u**seçin. 
5. Özel konu oluşturulduktan sonra başarılı bildirim görürsünüz. **Kaynak grubuna Git'i**seçin. 

   ![Başarılı durum bildirimini görüntüleme](./media/custom-event-quickstart-portal/success-notification.png)
6. Kaynak **Grubu** sayfasında, olay ızgarası konusunu seçin. 

   ![Olay ızgarası konu kaynağını seçin](./media/custom-event-quickstart-portal/select-event-grid-topic.png)
7. Olay ızgaranız için **Olay Izgara Konu** sayfasını görürsünüz. Bu sayfayı açık tutun. Daha sonra hızlı bir şekilde kullanırsınız. 

    ![Olay Izgara Konu giriş sayfası](./media/custom-event-quickstart-portal/event-grid-topic-home-page.png)

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma
Özel konu için abonelik oluşturmadan önce, olay iletisi için bir bitiş noktası oluşturun. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu hızlı başlangıcı basitleştirmek için, olay iletilerini görüntüleyin bir [önceden oluşturulmuş web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtırsınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Makale sayfasında, çözümü aboneliğinize dağıtmak için **Azure'a Dağıt'ı** seçin. Azure portalında parametre değerlerini girin.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`
1. Siteyi görürsünüz ancak henüz yayımlanmış olay yoktur.

   ![Yeni siteyi görüntüleme](./media/custom-event-quickstart-portal/view-site.png)

## <a name="subscribe-to-custom-topic"></a>Özel konuya abone olma

Event Grid’e hangi olayları izlemek istediğinizi ve olayların nereye gönderileceğini bildirmek için bir Event Grid konusuna abone olursunuz.

1. Şimdi, özel konunuz için **Olay Izgara Konu** sayfasında araç çubuğunda **+ Etkinlik Aboneliği'ni** seçin.

   ![Olay aboneliği ekleyin](./media/custom-event-quickstart-portal/new-event-subscription.png)
2. Etkinlik **Aboneliği Oluştur** sayfasında aşağıdaki adımları izleyin:
    1. Etkinlik aboneliği için bir **ad** girin.
    3. **Bitiş Noktası türü**için Web **Kancası'nı** seçin. 
    4. **Bitiş noktası seçin.** 

       ![Olay abonelik değerlerini sağlama](./media/custom-event-quickstart-portal/provide-subscription-values.png)
    5. Web kancası uç noktası için web uygulamanızın URL'sini girin ve ana sayfa URL'sine `api/updates` ekleyin. **Seçimi Onayla**'yı seçin.

       ![Uç nokta URL'si sağlama](./media/custom-event-quickstart-portal/provide-endpoint.png)
    6. **Etkinlik Aboneliği Oluştur** **sayfasında, Oluştur'u**seçin.

3. Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. Göz simgesini seçerek olay verilerini genişletin. Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Web uygulaması aboneliği doğrulamak için kod içerir.

    ![Abonelik olayını görüntüleme](./media/custom-event-quickstart-portal/view-subscription-event.png)

## <a name="send-an-event-to-your-topic"></a>Konunuza olay gönderme

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. Özel konunuza bir test olayı göndermek için Azure CLI veya PowerShell kullanın. Normalde olay verilerini bir uygulama veya Azure hizmeti gönderir.

Birinci örnekte, Azure CLI kullanılmaktadır. Özel konunun URL’si ve anahtarı ile örnek olay verilerini alır. `<topic name>` yerine özel konunuzun adını yazın. Örnek olay verileri oluşturulur. JSON’un `data` öğesi, olayınızın yüküdür. Bu alana doğru oluşturulmuş herhangi bir JSON gelebilir. Ayrıca, gelişmiş yönlendirme ve filtreleme için konu alanını da kullanabilirsiniz. CURL, HTTP istekleri gönderen bir yardımcı programdır.


### <a name="azure-cli"></a>Azure CLI
1. Azure portalında **Bulut Kabuğu'nu**seçin. Bulut Kabuğu penceresinin sol üst köşesinde **Bash'i** seçin. 

    ![Bulut Kabuk - Bash](./media/custom-event-quickstart-portal/cloud-shell-bash.png)
1. Konunun **bitiş noktasını** almak için aşağıdaki komutu çalıştırın: Komutu kopyalayıp yapıştırdıktan sonra, komutu çalıştırmadan önce **konu adını** ve kaynak **grup adını** güncelleştirin. 

    ```azurecli
    endpoint=$(az eventgrid topic show --name <topic name> -g <resource group name> --query "endpoint" --output tsv)
    ```
2. Özel konu **nun anahtarını** almak için aşağıdaki komutu çalıştırın: Komutu kopyalayıp yapıştırdıktan sonra, komutu çalıştırmadan önce **konu adını** ve **kaynak grup** adını güncelleştirin. 

    ```azurecli
    key=$(az eventgrid topic key list --name <topic name> -g <resource group name> --query "key1" --output tsv)
    ```
3. Aşağıdaki ifadeyi olay tanımıyla birlikte kopyalayın ve **ENTER**tuşuna basın. 

    ```json
    event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/motorcycles", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "make": "Ducati", "model": "Monster"},"dataVersion": "1.0"} ]'
    ```
4. Olayı göndermek için aşağıdaki **Curl** komutunu çalıştırın:

    ```
    curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
    ```

### <a name="azure-powershell"></a>Azure PowerShell
İkinci örnek, benzer adımları gerçekleştirmek için PowerShell’i kullanır.

1. Azure portalında **Bulut Kabuğu'nu** seçin `https://shell.azure.com/`(alternatif olarak gidin). Cloud Shell penceresinin sol üst köşesinde **PowerShell'i** seçin. Azure CLI bölümündeki örnek **Bulut Kabuğu** pencere resmine bakın.
2. Aşağıdaki değişkenleri ayarlayın. Her komutu kopyalayıp yapıştırdıktan sonra, komutu çalıştırmadan önce **konu adını** ve kaynak **grup adını** güncelleştirin:

    ```powershell
    $resourceGroupName = <resource group name>
    $topicName = <topic name>
    ```
3. Konunun **bitiş noktasını** ve **tuşlarını** almak için aşağıdaki komutları çalıştırın:

    ```powershell
    $endpoint = (Get-AzEventGridTopic -ResourceGroupName $resourceGroupName -Name $topicName).Endpoint
    $keys = Get-AzEventGridTopicKey -ResourceGroupName $resourceGroupName -Name $topicName
    ```
4. Etkinliği hazırlayın. Bulut Kabuğu penceresindeki ifadeleri kopyalayın ve çalıştırın. 

    ```powershell
    $eventID = Get-Random 99999

    #Date format should be SortableDateTimePattern (ISO 8601)
    $eventDate = Get-Date -Format s

    #Construct body using Hashtable
    $htbody = @{
        id= $eventID
        eventType="recordInserted"
        subject="myapp/vehicles/motorcycles"
        eventTime= $eventDate   
        data= @{
            make="Ducati"
            model="Monster"
        }
        dataVersion="1.0"
    }
    
    #Use ConvertTo-Json to convert event body from Hashtable to JSON Object
    #Append square brackets to the converted JSON payload since they are expected in the event's JSON payload syntax
    $body = "["+(ConvertTo-Json $htbody)+"]"
    ```
5. Olayı göndermek için **Invoke-WebRequest** cmdlet'ini kullanın. 

    ```powershell
    Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
    ```

### <a name="verify-in-the-event-grid-viewer"></a>Olay Izgara Görüntüleyicisinde Doğrula
Olayı tetiklediniz ve Event Grid, iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Az önce gönderdiğiniz olayı görmek için web uygulamanızı görüntüleyin.

```json
{
  "id": "974",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2019-03-28T01:11:59+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/myegridrg/providers/Microsoft.EventGrid/topics/myegridtopic"
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu olayla çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Aksi takdirde, bu makalede oluşturduğunuz kaynakları silin.

1. Sol menüde **Kaynak Grupları'nı** seçin. Sol menüde göremiyorsanız, sol menüdeki **Tüm Hizmetler'i** seçin ve **Kaynak Grupları'nı**seçin. 
2. **Kaynak** Grubu sayfasını başlatmak için kaynak grubunu seçin. 
3. Araç çubuğundaki **kaynak grubunu sil'i** seçin. 
4. Kaynak grubunun adını girerek silmeişlemini onaylayın ve **Sil'i**seçin. 

    ![Kaynak grupları](./media/custom-event-quickstart-portal/delete-resource-groups.png)

    Resimde gördüğünüz diğer kaynak grubu Bulut Bulut penceresi tarafından oluşturuldu ve kullanıldı. Bulut Kabuğu penceresini daha sonra kullanmayı düşünmüyorsanız silin. 

## <a name="next-steps"></a>Sonraki adımlar

Özel konu ve olay abonelikleri oluşturma işlemini öğrendiğinize göre artık Event Grid’in size nasıl yardımcı olabileceği konusunda daha fazla bilgi edinebilirsiniz:

- [Event Grid Hakkında](overview.md)
- [Blob depolama olaylarını bir özel web uç noktasına yönlendirme](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Büyük verileri bir veri ambarına akışla aktarma](event-grid-event-hubs-integration.md)
