---
title: Azure hızlı başlangıç-Azure portal kullanarak bir olay hub 'ı oluşturma
description: Bu hızlı başlangıçta Azure portalı kullanarak Azure olay hub'ı oluşturmayı ve .NET Standard SDK kullanarak olay gönderip almayı öğreneceksiniz.
services: event-hubs
documentationcenter: ''
author: spelluru
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: 5e80ab6d5ed0076e03f5378cbe975b15d0a28f47
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78357106"
---
# <a name="quickstart-create-an-event-hub-using-azure-portal"></a>Hızlı başlangıç: Azure portalı kullanarak olay hub'ı oluşturma
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu hızlı başlangıçta [Azure portalı](https://portal.azure.com) kullanarak olay hub'ı oluşturacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- [Visual Studio 2019)](https://www.visualstudio.com/vs) veya sonraki bir sürümü.
- [.NET Standard SDK'sı](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya üzeri.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynakları için mantıksal bir koleksiyondur. Tüm kaynaklar bir kaynak grubuna dağıtılır ve buradan yönetilir. Kaynak grubu oluşturmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüden **Kaynak grupları**'na tıklayın. Daha sonra **Ekle**'ye tıklayın.

   ![Kaynak grupları - Ekle düğmesi](./media/event-hubs-quickstart-portal/resource-groups1.png)

2. **Abonelik**için, kaynak grubunu oluşturmak istediğiniz Azure aboneliğinin adını seçin.
3. **Kaynak grubu için**benzersiz bir ad yazın. Sistem, adın seçili Azure aboneliğinde var olup olmadığını kontrol eder.
4. Kaynak grubu için bir **bölge** seçin.
5. **Gözden geçir + oluştur**' u seçin.

   ![Kaynak grubu - oluştur](./media/event-hubs-quickstart-portal/resource-groups2.png)
6. **Gözden geçir + oluştur** sayfasında **Oluştur**' u seçin. 

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Event Hubs ad alanı, tam etki alanı adının başvurduğu, içinde bir veya daha fazla olay hub'ı oluşturduğunuz benzersiz bir kapsam kapsayıcısı sağlar. Portalı kullanarak kaynak grubunuzda bir ad alanı oluşturmak için aşağıdaki eylemleri gerçekleştirin:

1. Azure portalda ekranın sol üst köşesindeki **Kaynak oluştur**'a tıklayın.
2. Sol taraftaki menüden **tüm hizmetler** ' i seçin ve **analiz** kategorisinde **Event Hubs** ' ın yanındaki **yıldız (`*`)** seçeneğini belirleyin. **Event Hubs** sol gezinti menüsünde **Sık kullanılanlara** eklendiğini doğrulayın. 
    
   ![Event Hubs arayın](./media/event-hubs-quickstart-portal/select-event-hubs-menu.png)
3. Sol gezinti menüsünde **Sık Kullanılanlar** ' ın altında **Event Hubs** ' ı seçin ve araç çubuğunda **Ekle** ' yi seçin.

   ![Ekle düğmesi](./media/event-hubs-quickstart-portal/event-hubs-add-toolbar.png)
4. **Ad alanı oluştur** sayfasında, aşağıdaki adımları uygulayın:
    1. Ad alanı için bir **ad** girin. Adın kullanılabilirliği sistem tarafından hemen kontrol edilir.
    2. **Fiyatlandırma katmanını** (temel veya standart) seçin.
    3. **Enable Kafka** seçeneğinin otomatik olarak etkinleştirildiğini unutmayın. Azure Event Hubs ile bir Kafka uç noktası sağlar. Bu uç nokta, Event Hubs ad alanının [Apache Kafka](https://kafka.apache.org/intro) ileti protokolünü ve API 'leri yerel olarak anlamasına olanak sağlar. Bu özellik sayesinde ile Kafka konularını Protokolü istemcilerinize değiştirme veya kendi kümelerini çalıştırmak gibi event hubs ile iletişim kurabilir. Event Hubs, [1,0 ve üzeri sürümleri Apache Kafka](https://kafka.apache.org/10/documentation.html) destekler.
    4. Ad alanını oluşturmak istediğiniz **aboneliği** seçin.
    5. Var olan bir **kaynak grubunu** seçin veya yeni bir kaynak grubu oluşturun. 
    4. Ad alanı için bir **konum** seçin.
    5. **Oluştur**’u seçin. Sistemin kaynakları tam olarak sağlaması için birkaç dakika beklemeniz gerekebilir.

       ![Olay hub'ı ad alanı oluşturma](./media/event-hubs-quickstart-portal/create-event-hub1.png)
5. Olay Hub 'ı ad alanını görmek için **Event Hubs** sayfasını yenileyin. Uyarı içinde Olay Hub 'ı oluşturma durumunu denetleyebilirsiniz. 

    ![Olay hub'ı ad alanı oluşturma](./media/event-hubs-quickstart-portal/event-hubs-refresh.png)
6. Ad alanını seçin. Portalda **Event Hubs ad alanınız** için giriş sayfasını görürsünüz. 

   ![Ad alanı için ana sayfa](./media/event-hubs-quickstart-portal/namespace-home-page.png)
    
## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Ad alanında bir olay hub'ı oluşturmak için aşağıdaki eylemleri gerçekleştirin:

1. Event Hubs ad alanı sayfasında, soldaki menüden **Event Hubs** ' i seçin.
1. Pencerenin en üstündeki **+ Olay Hub’ı** seçeneğine tıklayın.
   
    ![Olay Hub'ı Ekle - düğme](./media/event-hubs-quickstart-portal/create-event-hub4.png)
1. Olay hub'ınız için bir ad yazın, ardından **Oluştur**’a tıklayın.
   
    ![Olay hub'ı oluşturma](./media/event-hubs-quickstart-portal/create-event-hub5.png)
4. Uyarı ' da Olay Hub 'ı oluşturma durumunu kontrol edebilirsiniz. Olay Hub 'ı oluşturulduktan sonra, aşağıdaki görüntüde gösterildiği gibi Olay Hub 'ları listesinde görürsünüz:

    ![Olay Hub 'ı oluşturuldu](./media/event-hubs-quickstart-portal/event-hub-created.png)

Tebrikler! Portalı kullanarak bir Event Hubs ad alanı ve bu ad alanının içinde bir olay hub'ı oluşturdunuz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir kaynak grubu, bir Event Hubs ad alanı ve bir olay hub'ı oluşturdunuz. Olay Hub 'ından olay alma (veya) olayları gönderme hakkında adım adım yönergeler için, bkz. **olayları gönderme ve alma** öğreticileri: 

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](get-started-java-send-v2.md)
- [Python](get-started-python-send-v2.md)
- [JavaScript](get-started-java-send-v2.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (yalnızca gönderme)](event-hubs-c-getstarted-send.md)
- [Apache Storm (yalnızca alma)](event-hubs-storm-getstarted-receive.md)


[Azure portal]: https://portal.azure.com/
[3]: ./media/event-hubs-quickstart-portal/sender1.png
[4]: ./media/event-hubs-quickstart-portal/receiver1.png
