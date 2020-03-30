---
title: Azure Quickstart - Azure portalını kullanarak etkinlik merkezi oluşturma
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240999"
---
# <a name="quickstart-create-an-event-hub-using-azure-portal"></a>Hızlı başlangıç: Azure portalı kullanarak olay hub'ı oluşturma
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu hızlı başlangıçta [Azure portalı](https://portal.azure.com) kullanarak olay hub'ı oluşturacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
- [Visual Studio 2019)](https://www.visualstudio.com/vs) veya daha sonra.
- [.NET Standard SDK'sı](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya üzeri.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynakları için mantıksal bir koleksiyondur. Tüm kaynaklar bir kaynak grubuna dağıtılır ve buradan yönetilir. Kaynak grubu oluşturmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Soldaki menüden **Kaynak grupları**'na tıklayın. Daha sonra **Ekle**'ye tıklayın.

   ![Kaynak grupları - Ekle düğmesi](./media/event-hubs-quickstart-portal/resource-groups1.png)

2. **Abonelik**için, kaynak grubunu oluşturmak istediğiniz Azure aboneliğinin adını seçin.
3. **Kaynak grubu için**benzersiz bir ad yazın. Sistem, adın seçili Azure aboneliğinde var olup olmadığını kontrol eder.
4. Kaynak grubu için bir **bölge** seçin.
5. **Gözden Geçir + Oluştur'u**seçin.

   ![Kaynak grubu - oluştur](./media/event-hubs-quickstart-portal/resource-groups2.png)
6. Gözden **Geçir + Oluştur** sayfasında **Oluştur'u**seçin. 

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Event Hubs ad alanı, tam etki alanı adının başvurduğu, içinde bir veya daha fazla olay hub'ı oluşturduğunuz benzersiz bir kapsam kapsayıcısı sağlar. Portalı kullanarak kaynak grubunuzda bir ad alanı oluşturmak için aşağıdaki eylemleri gerçekleştirin:

1. Azure portalda ekranın sol üst köşesindeki **Kaynak oluştur**'a tıklayın.
2. Sol menüdeki **Tüm hizmetleri** seçin ve **Analytics** kategorisinde **Olay Hub'larının** yanındaki **yıldız ()`*`** seçeneğini belirleyin. Olay **Hub'larının** sol daki gezinme menüsünde **SıK Kullanılanlar'a** eklenmesini onaylayın. 
    
   ![Etkinlik Hub'larını ara](./media/event-hubs-quickstart-portal/select-event-hubs-menu.png)
3. Sol gezinti menüsünde **SıK Kullanılanlar** altında Olay **Hub'larını** seçin ve araç çubuğuna **Ekle'yi** seçin.

   ![Ekle düğmesi](./media/event-hubs-quickstart-portal/event-hubs-add-toolbar.png)
4. Ad **alanı oluştur** sayfasında aşağıdaki adımları izleyin:
    1. Ad alanı için bir **ad** girin. Adın kullanılabilirliği sistem tarafından hemen kontrol edilir.
    2. Fiyatlandırma **katmanını** (Temel veya Standart) seçin.
    3. Kafka **seçeneğini etkinleştirme** seçeneğinin otomatik olarak etkinleştirilen ekidir. Azure Etkinlik Hub'ları size Kafka bitiş noktası sağlar. Bu uç nokta, Olay Hub'larınızın ad alanının [Apache Kafka](https://kafka.apache.org/intro) ileti protokolünü ve API'lerini yerel olarak anlamasını sağlar. Bu özellik sayesinde, iletişim istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Kafka konularıyla olduğu gibi etkinlik merkezlerinizle de iletişim kurabilirsiniz. Etkinlik Hub'ları [Apache Kafka 1.0](https://kafka.apache.org/10/documentation.html) ve sonraki sürümlerini destekler.
    4. Ad alanını oluşturmak istediğiniz **aboneliği** seçin.
    5. Varolan bir **kaynak grubu** seçin veya yeni bir kaynak grubu oluşturun. 
    4. Ad alanı için bir **konum** seçin.
    5. **Oluştur'u**seçin. Sistemin kaynakları tam olarak sağlaması için birkaç dakika beklemeniz gerekebilir.

       ![Olay hub'ı ad alanı oluşturma](./media/event-hubs-quickstart-portal/create-event-hub1.png)
5. Olay merkezi ad alanını görmek için **Olay Hub'ları** sayfasını yenileyin. Uyarılarda olay merkezi oluşturma durumunu denetleyebilirsiniz. 

    ![Olay hub'ı ad alanı oluşturma](./media/event-hubs-quickstart-portal/event-hubs-refresh.png)
6. Ad alanını seçin. Portalda **Olay Hub'larınızın ad alanının** ana sayfasını görürsünüz. 

   ![Ad alanı için ana sayfa](./media/event-hubs-quickstart-portal/namespace-home-page.png)
    
## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Ad alanında bir olay hub'ı oluşturmak için aşağıdaki eylemleri gerçekleştirin:

1. Olay Hub'ları Ad Alanı sayfasında, sol menüde **Olay Hub'ları'nı** seçin.
1. Pencerenin en üstündeki **+ Olay Hub’ı** seçeneğine tıklayın.
   
    ![Olay Hub'ı Ekle - düğme](./media/event-hubs-quickstart-portal/create-event-hub4.png)
1. Olay hub'ınız için bir ad yazın, ardından **Oluştur**’a tıklayın.
   
    ![Olay hub'ı oluşturma](./media/event-hubs-quickstart-portal/create-event-hub5.png)
4. Uyarılarda olay merkezi oluşturma durumunu denetleyebilirsiniz. Olay hub'ı oluşturulduktan sonra, aşağıdaki resimde gösterildiği gibi olay hub'ları listesinde görürsünüz:

    ![Olay merkezi oluşturuldu](./media/event-hubs-quickstart-portal/event-hub-created.png)

Tebrikler! Portalı kullanarak bir Event Hubs ad alanı ve bu ad alanının içinde bir olay hub'ı oluşturdunuz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir kaynak grubu, bir Event Hubs ad alanı ve bir olay hub'ı oluşturdunuz. Bir etkinlik merkezinden etkinlik almak için (veya) etkinlik göndermek için adım adım talimatlar **için, Gönder'e** bakın ve etkinlik eğitimlerini alın: 

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](get-started-java-send-v2.md)
- [Python](get-started-python-send-v2.md)
- [Javascript](get-started-java-send-v2.md)
- [Git](event-hubs-go-get-started-send.md)
- [C (yalnızca gönderme)](event-hubs-c-getstarted-send.md)
- [Apache Storm (yalnızca alma)](event-hubs-storm-getstarted-receive.md)


[Azure portal]: https://portal.azure.com/
[3]: ./media/event-hubs-quickstart-portal/sender1.png
[4]: ./media/event-hubs-quickstart-portal/receiver1.png
