---
title: Azure portalını kullanarak Etkinlik Hub'larına özel küme oluşturma
description: Bu hızlı başlangıçta, Azure portalLarını kullanarak Bir Azure Etkinlik Hub'ları kümesini nasıl oluşturabileceğinizi öğrenirsiniz.
services: event-hubs
documentationcenter: ''
author: femila
manager: ''
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 12/20/2019
ms.author: femila
ms.openlocfilehash: 5b1574eaac8771043e09500225b65e4835c8e627
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77157491"
---
# <a name="quickstart-create-a-dedicated-event-hubs-cluster-using-azure-portal"></a>Hızlı başlangıç: Azure portalLarını kullanarak özel bir Olay Hub'ları kümesi oluşturma 
Olay Hub'ları kümeleri, en zorlu akış gereksinimlerine sahip müşteriler için tek kiracılı dağıtımlar sunar. Bu teklif %99,99 SLA garantilidir ve yalnızca Özel fiyatlandırma katmanımızda mevcuttur. [Bir Olay Hub'ları kümesi,](event-hubs-dedicated-overview.md) garantili kapasite ve saniye altı gecikmesi ile saniyede milyonlarca olay meydana getirebilir. Küme içinde oluşturulan ad alanları ve olay hub'ları, standart teklifin tüm özelliklerini ve daha fazlasını içerir, ancak giriş sınırı yoktur. Özel teklif ayrıca, ek ücret ödemeden popüler [Event Hubs Capture](event-hubs-capture-overview.md) özelliğini de içerir ve veri akışlarını [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md) veya Azure Veri Gölü Depolama Gen [1'e](../data-lake-store/data-lake-store-overview.md)otomatik olarak toplu olarak ve günlüğe kaydetmenize olanak tanır.

Özel kümeler, önceden tahsis edilmiş bir CPU ve bellek kaynağı olan **Kapasite Birimleri (CUs)** tarafından karşılanır ve faturalandırılır. Her küme için 1, 2, 4, 8, 12, 16 veya 20 CUs satın alabilirsiniz. Bu hızlı başlangıçta, Azure portalında 1 CU Etkinlik Hub'ı kümesi oluşturmanızda size yol bulacağız.

> [!NOTE]
> Bu self servis deneyimi şu anda [Azure Portalı'nda](https://aka.ms/eventhubsclusterquickstart)önizlemede mevcuttur. Özel teklif le ilgili herhangi bir sorunuz varsa, lütfen [Etkinlik Hub'ları ekibine](mailto:askeventhubs@microsoft.com)ulaşın.


## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıcı tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- Bir Azure hesabı. Hesabınız yoksa, başlamadan önce [bir hesap satın alın.](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) Bu özellik ücretsiz bir Azure hesabıyla desteklenmez. 
- [Visual Studio](https://visualstudio.microsoft.com/vs/) 2017 Güncelleme 3 (sürüm 15.3, 26730.01) veya daha sonra.
- [.NET Standard SDK'sı](https://dotnet.microsoft.com/download), sürüm 2.0 veya üzeri.
- [Kaynak grubu oluşturuldu.](../event-hubs/event-hubs-create.md#create-a-resource-group)

## <a name="create-an-event-hubs-dedicated-cluster"></a>Olay Hub'larına Özel Küme Oluşturma
Olay Hub'ları kümesi, bir veya daha fazla ad alanı oluşturabileceğiniz benzersiz bir kapsam kapsayıcısı sağlar. Portal self servis deneyiminin bu Önizleme aşamasında, belirli bölgelerde 1 CU kümeleri oluşturabilirsiniz. 1 CU'dan büyük bir kümeye ihtiyacınız varsa, kümenizi oluşturulduktan sonra ölçeklendirmek için bir Azure destek isteği gönderebilirsiniz.

Azure portalını kullanarak kaynak grubunuzda bir küme oluşturmak için lütfen aşağıdaki adımları tamamlayın:

1. Azure portalında bir küme oluşturmak için [bu bağlantıyı](https://aka.ms/eventhubsclusterquickstart) izleyin. Tam tersine, sol gezinti bölmesinden **Tüm hizmetleri** seçin, ardından arama çubuğuna "Olay Hub'ları Kümeleri" yazın ve sonuçlar listesinden "Olay Hub'ları Kümeleri"ni seçin.
2. Küme **Oluştur** sayfasında aşağıdakileri yapılandırın:
    1. Küme **için**bir ad girin. Adın kullanılabilirliği sistem tarafından hemen kontrol edilir.
    2. Kümeoluşturmak istediğiniz **aboneliği** seçin.
    3. Kümeoluşturmak istediğiniz **kaynak grubunu** seçin.
    4. Küme için bir **konum** seçin. Tercih ettiğiniz bölge gri renkteyse, geçici olarak kapasitenin dışındadır ve Olay Hub'ları ekibine bir [destek isteği](#submit-a-support-request) gönderebilirsiniz.
    5. Sonraki' ni **seçin:** Sayfanın altındaki Etiketler düğmesini seçin. Sistemin kaynakları tam olarak sağlaması için birkaç dakika beklemeniz gerekebilir.

        ![Olay Hub'ları Kümesi Oluşturma - Temel Bilgiler sayfası](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-basics-page.png)
3. **Etiketler** sayfasında, aşağıdakileri yapılandırın:
    1. Eklemek istediğiniz etiket için bir **ad** ve **değer** girin. Bu adım **isteğe bağlıdır.**  
    2. Gözden **Geçir + Oluştur** düğmesini seçin.

        ![Olay Hub'ları Küme sayfası oluşturma - Etiketler sayfası](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-tags-page.png)
4. Gözden **Geçir + Oluştur** sayfasında ayrıntıları gözden geçirin ve **Oluştur'u**seçin. 

    ![Olay Hub'ları Küme sayfası oluşturma - İnceleme + Sayfa Oluştur](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-review-create-page.png)

## <a name="create-a-namespace-and-event-hub-within-a-cluster"></a>Küme içinde ad alanı ve olay merkezi oluşturma

1. Kümeniz için **Olay Hub'ları Küme** sayfasında bir küme içinde ad alanı oluşturmak için üst menüden **+Namespace'i** seçin.

    ![Küme yönetimi sayfası - ad alanı ekle düğmesi](./media/event-hubs-dedicated-cluster-create-portal/cluster-management-page-add-namespace-button.png)
2. Ad alanı oluşturma sayfasında aşağıdaki adımları yapın:
    1. Ad **alanı için**bir ad girin.  Sistem, adın kullanılabilir olup olmadığını denetler.
    2. Ad alanı aşağıdaki özellikleri devralır:
        1. Abonelik Kimliği
        2. Kaynak Grubu
        3. Konum
        4. Küme Adı
    3. Ad alanını oluşturmak için **Oluştur'u** seçin. Artık kümenizi yönetebilirsiniz.  

        ![Küme sayfasında ad alanı oluşturma](./media/event-hubs-dedicated-cluster-create-portal/create-namespace-cluster-page.png)
3. Ad alanınız oluşturulduktan sonra, normalde bir ad alanı içinde oluşturacağınız gibi [bir olay hub'ı oluşturabilirsiniz.](event-hubs-create.md#create-an-event-hub) 


## <a name="submit-a-support-request"></a>Destek isteği gönderme

Oluşturulduktan sonra kümenizin boyutunu değiştirmek istiyorsanız veya tercih ettiğiniz bölge kullanılamıyorsa, lütfen aşağıdaki adımları izleyerek bir destek isteği gönderin:

1. [Azure portalında,](https://portal.azure.com)sol menüden Yardım **+ destek'i** seçin.
2. Destek menüsünden **+ Yeni destek isteği** seçin.
3. Destek sayfasında aşağıdaki adımları izleyin:
    1. **Sorun Türü**için açılan listeden **Teknik'i** seçin.
    2. **Abonelik** bölümünde aboneliğinizi seçin.
    3. **Hizmet** **için, Hizmetlerim'i**seçin ve ardından **Olay Hub'larını**seçin.
    4. **Kaynak**için, kümenizi zaten varsa seçin, aksi takdirde **Genel Soru/Kaynak Kullanılamıyor'ı**seçin.
    5. **Sorun türü için** **Kota'yı**seçin.
    6. **Sorun alt türü**için, açılan listeden aşağıdaki değerlerden birini seçin:
        1. Özelliğin bölgenizde desteklenmesini istemek için **Özel SKU** İsteği'ni seçin.
        2. Özel kümenizi büyütmek veya küçültmek **istiyorsanız, Ayrılmış Kümeyi** Ölçeklendirmek veya Küçültmek Için İstek'i seçin. 
    7. **Konu**için, sorunu açıklayın.

        ![Destek bilet sayfası](./media/event-hubs-dedicated-cluster-create-portal/support-ticket.png)

 ## <a name="delete-a-dedicated-cluster"></a>Özel bir kümeyi silme
 
1. Kümeyi silmek için üst menüden **Sil'i** seçin. Kümenizin oluşturulduktan sonra en az 4 saatlik kullanım için faturalandırılaolacağını lütfen unutmayın. 
2. Kümeyi silme isteğinizi onaylayan bir ileti görüntülenir.
3. **Kümenin adını** yazın ve kümeyi silmek için **Sil'i** seçin.

    ![Küme sayfasını silme](./media/event-hubs-dedicated-cluster-create-portal/delete-cluster-page.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Olay Hub'ları kümesi oluşturdunuz. Etkinlik hub'ından etkinlik gönderip almak ve olayları bir Azure depolama alanına veya Azure Veri Gölü Deposu'na yakalamak için adım adım talimatlar için aşağıdaki öğreticilere bakın:

- Olay gönderme ve alma 
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [Javascript](get-started-java-send-v2.md)
- [Olay Hub'larının Yakalanmasını etkinleştirmek için Azure portalını kullanma](event-hubs-capture-enable-through-portal.md)
- [Apache Kafka için Azure Etkinlik Hub'larını kullanma](event-hubs-for-kafka-ecosystem-overview.md)
