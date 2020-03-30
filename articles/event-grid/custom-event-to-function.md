---
title: "Hızlı başlatma: Azure İşlevi'ne özel etkinlikler gönderme - Olay Idamı"
description: "Hızlı başlangıç: Bir konuyu yayımlamak için Azure Olay Izgarasını ve Azure CLI'yi veya portalı kullanın ve bu etkinliğe abone olun. Bitiş noktası için bir Azure İşlevi kullanılır."
services: event-grid
keywords: ''
author: banisadr
ms.author: babanisa
ms.date: 11/15/2019
ms.topic: quickstart
ms.service: event-grid
ms.openlocfilehash: 5e38571cf84537fd722093b96cd277743e8ce80c
ms.sourcegitcommit: fab450a18a600d72b583ecfbe6c5e53afd43408c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80292161"
---
# <a name="quickstart-route-custom-events-to-an-azure-function-with-event-grid"></a>Hızlı başlangıç: Özel olayları Olay Ağıla azure işlevine yönlendirin

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Azure İşlevler desteklenen olay işleyicilerinden biridir. Bu makalede, Azure portalını kullanarak özel bir konu oluşturur, bu özel konuya abone olur ve sonucu görüntülemek için olayı tetiklersiniz. Olayları bir Azure İşlevi'ne gönderirsiniz.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-azure-function"></a>Azure İşlevi oluşturma

Özel konuya abone olmadan önce, olayları işlemek için bir işlev oluşturalım. Azure portalında 'Kaynak oluştur' seçeneğini tıklatın ve 'işlev' yazın, ardından 'İşlev Uygulaması'nı seçin ve oluştur'u tıklatın. Kaynak grubu altında 'Yeni oluştur' seçeneğini belirleyin ve ona bir ad verin. Bunu öğreticinin geri kalanı için kullanırsınız. İşlev Uygulamasına bir ad verin, 'Kod'da 'Yayımla' geçişini bırakın, herhangi bir çalışma zamanı ve bölgeyi seçin ve ardından oluştur tuşuna basın.

İşlev Uygulamanız hazır olduğunda, uygulamanın içine gidin ve '+ Yeni Fonksiyon'a tıklayın. Geliştirme ortamı için 'Portal Içi' seçeneğini belirleyin ve devam edin. Bir işlev oluşturma altında, daha fazla şablon görüntülemek için 'Daha fazla şablon' seçin ve ardından 'Azure Olay Izgara Tetikleyicisi' için arama yapın ve seçin. Bu tetikleyiciyi ilk kez kullanıyorsanız, uzantıyı yüklemek için 'Yükle'yi tıklatmanız gerekebilir.

![Fonksiyon Olay Izgara Tetik](./media/custom-event-to-function/grid-trigger.png)

Uzantıyı yükledikten sonra devam et'i tıklatın, işlevinize bir ad verin ve ardından oluştur'a tıklayın.

[!INCLUDE [event-grid-register-provider-portal.md](../../includes/event-grid-register-provider-portal.md)]

## <a name="create-a-custom-topic"></a>Özel konu oluşturma

Event grid konusu, olaylarınızı göndereceğiniz kullanıcı tanımlı bir uç nokta sağlar. 

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Sol navigasyon menüsündeki **Tüm hizmetleri** seçin, **Olay Izgarasını**arayın ve **Olay Izgara Konuları'nı**seçin. 

    ![Olay Izgara Konularını Seçin](./media/custom-event-to-function/select-event-grid-topics.png)
3. Olay **Izgara Konuları** sayfasında araç çubuğuna **+ Ekle'yi** seçin. 

    ![Olay Izgara Konu ekle düğmesi](./media/custom-event-to-function/add-event-grid-topic-button.png)

4. Konu **Oluştur** sayfasında aşağıdaki adımları izleyin:

    1. Özel konu için benzersiz bir **ad** sağlayın. Konu adı bir DNS girdisi ile temsil edildiğinden konu adı benzersiz olmalıdır. Görüntüde gösterilen adı kullanmayın. Bunun yerine, kendi adınızı oluşturun - 3-50 karakter arasında olmalı ve yalnızca a-z, A-Z, 0-9 ve "-" değerlerini içermelidir.
    2. Azure **aboneliğinizi**seçin.
    3. Önceki adımlardan aynı kaynak grubunu seçin.
    4. Olay ızgarası konusu için bir **konum** seçin.
    5. **Olay Şeması** alanı için varsayılan değeri **Olay Izgara Şeması'nı** tutun. 

       ![Konu sayfası oluşturma](./media/custom-event-to-function/create-custom-topic.png)
    6. **Oluştur'u**seçin. 

5. Özel konu oluşturulduktan sonra başarılı bildirim görürsünüz. **Kaynak grubuna Git'i**seçin. 

   ![Başarılı durum bildirimini görüntüleme](./media/custom-event-to-function/success-notification.png)

6. Kaynak **Grubu** sayfasında, olay ızgarası konusunu seçin. 

   ![Olay ızgarası konu kaynağını seçin](./media/custom-event-to-function/select-event-grid-topic.png)

7. Olay ızgaranız için **Olay Izgara Konu** sayfasını görürsünüz. Bu sayfayı açık tutun. Daha sonra hızlı bir şekilde kullanırsınız. 

    ![Olay Izgara Konu giriş sayfası](./media/custom-event-to-function/event-grid-topic-home-page.png)

## <a name="subscribe-to-custom-topic"></a>Özel konuya abone olma

Event Grid’e hangi olayları izlemek istediğinizi ve olayların nereye gönderileceğini bildirmek için bir Event Grid konusuna abone olursunuz.

1. Şimdi, özel konunuz için **Olay Izgara Konu** sayfasında araç çubuğunda **+ Etkinlik Aboneliği'ni** seçin.

   ![Olay aboneliği ekleyin](./media/custom-event-to-function/new-event-subscription.png)

2. Etkinlik **Aboneliği Oluştur** sayfasında aşağıdaki adımları izleyin:
    1. Etkinlik aboneliği için bir **ad** girin.
    3. Bitiş Noktası türü için **Azure İşlevi'ni** seçin. **Endpoint type** 
    4. **Bitiş noktası seçin.** 

       ![Olay abonelik değerlerini sağlama](./media/custom-event-to-function/provide-subscription-values.png)

    5. İşlev bitiş noktası için, İşlev Uygulamanızın içinde olduğu Azure Aboneliği ve Kaynak Grubu'nu seçin ve ardından daha önce oluşturduğunuz İşlev Uygulaması ve işlevini seçin. **Seçimi Onayla**'yı seçin.

       ![Uç nokta URL'si sağlama](./media/custom-event-to-function/provide-endpoint.png)

    6. **Etkinlik Aboneliği Oluştur** **sayfasında, Oluştur'u**seçin.

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
Olayı tetiklediniz ve Event Grid, iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Olay Izgara tetikleme işlevine gidin ve günlükleri açın. Günlüklerde olayın veri yükü bir kopyasını görmeniz gerekir. Önce günlükler penceresini açtığından veya yeniden bağlanmaya bastığınıza emin değilseniz ve ardından tekrar bir test olayı göndermeyi deneyin.

![Başarılı İşlev Tetik Günlüğü](./media/custom-event-to-function/successful-function.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu olayla çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Aksi takdirde, bu makalede oluşturduğunuz kaynakları silin.

1. Sol menüde **Kaynak Grupları'nı** seçin. Sol menüde göremiyorsanız, sol menüdeki **Tüm Hizmetler'i** seçin ve **Kaynak Grupları'nı**seçin. 
2. **Kaynak** Grubu sayfasını başlatmak için kaynak grubunu seçin. 
3. Araç çubuğundaki **kaynak grubunu sil'i** seçin. 
4. Kaynak grubunun adını girerek silmeişlemini onaylayın ve **Sil'i**seçin. 

    ![Kaynak grupları](./media/custom-event-to-function/delete-resource-groups.png)

    Resimde gördüğünüz diğer kaynak grubu Bulut Bulut penceresi tarafından oluşturuldu ve kullanıldı. Bulut Kabuğu penceresini daha sonra kullanmayı düşünmüyorsanız silin. 

## <a name="next-steps"></a>Sonraki adımlar

Artık konu oluşturma ve olay aboneliklerini öğrendiğinize göre, Event Grid’in size nasıl yardımcı olabileceği konusunda daha fazla bilgi edinebilirsiniz:

- [Event Grid Hakkında](overview.md)
- [Blob depolama olaylarını bir özel web uç noktasına yönlendirme](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Büyük verileri bir veri ambarına akışla aktarma](event-grid-event-hubs-integration.md)
