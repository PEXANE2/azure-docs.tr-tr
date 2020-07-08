---
title: Event Hubs verileri Azure Data Lake Storage 1. olarak yakala
description: Azure Event Hubs tarafından alınan verileri yakalamak için Azure Data Lake Storage 1. kullanın.
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 9d663c2e95ee94811d70a0602b35842a789dd9b3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85515756"
---
# <a name="use-azure-data-lake-storage-gen1-to-capture-data-from-event-hubs"></a>Event Hubs verileri yakalamak için Azure Data Lake Storage 1. kullanma

Azure Event Hubs tarafından alınan verileri yakalamak için Azure Data Lake Storage 1. nasıl kullanacağınızı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Storage 1. hesabı**. Bir oluşturma hakkında yönergeler için bkz. Azure Data Lake Storage 1. kullanmaya [başlama](data-lake-store-get-started-portal.md).

*  **Bir Event Hubs ad alanı**. Yönergeler için bkz. [Event Hubs ad alanı oluşturma](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Data Lake Storage 1. hesabının ve Event Hubs ad alanının aynı Azure aboneliğinde bulunduğundan emin olun.


## <a name="assign-permissions-to-event-hubs"></a>Event Hubs izinleri atama

Bu bölümde, Event Hubs verileri yakalamak istediğiniz hesapta bir klasör oluşturacaksınız. Ayrıca, bir Data Lake Storage 1. hesabına veri yazabilmesi için Event Hubs izinler atarsınız. 

1. Event Hubs verileri yakalamak istediğiniz Data Lake Storage 1. hesabını açın ve **Veri Gezgini**' ye tıklayın.

    ![Data Lake Storage 1. Veri Gezgini](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Storage 1. Veri Gezgini")

1.  **Yeni klasör** ' e tıklayın ve ardından verileri yakalamak istediğiniz klasör için bir ad girin.

    ![Data Lake Storage 1. yeni bir klasör oluşturun](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Data Lake Storage 1. yeni bir klasör oluşturun")

1. Data Lake Storage 1. kökünde izin atayın. 

    a. **Veri Gezgini**' ye tıklayın, Data Lake Storage 1. hesabının kökünü seçin ve ardından **erişim**' e tıklayın.

    ![Data Lake Storage 1. kök için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Data Lake Storage 1. kök için izin atama")

    b. **Erişim**altında, **Ekle**' ye tıklayın, **Kullanıcı veya Grup Seç**' e tıklayın ve ardından arama yapın `Microsoft.EventHubs` . 

    ![Data Lake Storage 1. kök için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Data Lake Storage 1. kök için izin atama")
    
    **Seç**' e tıklayın.

    c. **Izinleri ata**altında, **izinleri seç**' e tıklayın. Yürütülecek **izinleri** ayarlayın **Execute**. **Bu klasöre ve tüm alt öğelere** **Ekle** ' ye ayarlayın. **Bir erişim izni girişi ve varsayılan izin girdisi** **olarak ekle** ' ye ayarlayın.

    > [!IMPORTANT]
    > Azure Event Hubs tarafından alınan verileri yakalamaya yönelik yeni bir klasör hiyerarşisi oluştururken, bu hedef klasöre erişim sağlamanın kolay bir yoludur.  Ancak, üst düzey bir klasörün tüm alt öğelerine çok sayıda alt dosya ve klasör içeren izinler eklemek uzun sürebilir.  Kök klasörünüz çok sayıda dosya ve klasör içeriyorsa, **Execute** `Microsoft.EventHubs` son hedef klasörünüzün yolundaki her bir klasöre ayrı ayrı yürütme izinleri eklemek daha hızlı olabilir. 

    ![Data Lake Storage 1. kök için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Data Lake Storage 1. kök için izin atama")

    **Tamam**'a tıklayın.

1. Verileri yakalamak istediğiniz Data Lake Storage 1. hesabının altındaki klasör için izinler atayın.

    a. **Veri Gezgini**' ye tıklayın, Data Lake Storage 1. hesabındaki klasörü seçin ve ardından **erişim**' e tıklayın.

    ![Data Lake Storage 1. klasörü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Data Lake Storage 1. klasörü için izin atama")

    b. **Erişim**altında, **Ekle**' ye tıklayın, **Kullanıcı veya Grup Seç**' e tıklayın ve ardından arama yapın `Microsoft.EventHubs` . 

    ![Data Lake Storage 1. klasörü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Data Lake Storage 1. klasörü için izin atama")
    
    **Seç**' e tıklayın.

    c. **Izinleri ata**altında, **izinleri seç**' e tıklayın. **Okuma, yazma** ve **yürütme** **izinlerini** ayarlayın. **Bu klasöre ve tüm alt öğelere** **Ekle** ' ye ayarlayın. Son olarak, **bir erişim izni girişi ve varsayılan izin girişi** **olarak ekle** seçeneğini belirleyin.

    ![Data Lake Storage 1. klasörü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Data Lake Storage 1. klasörü için izin atama")
    
    **Tamam**'a tıklayın. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-storage-gen1"></a>Data Lake Storage 1. verileri yakalamak için Event Hubs yapılandırma

Bu bölümde, bir Event Hubs ad alanı içinde bir olay hub 'ı oluşturacaksınız. Ayrıca, bir Azure Data Lake Storage 1. hesabına veri yakalamak için Olay Hub 'ını yapılandırırsınız. Bu bölümde, zaten bir Event Hubs ad alanı oluşturmuş olduğunuz varsayılmaktadır.

1. Event Hubs ad alanının **genel bakış** bölmesinden **+ Event hub**' a tıklayın.

    ![Olay Hub'ı Oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Olay Hub'ı Oluşturma")

1. Data Lake Storage 1. verileri yakalamaya Event Hubs yapılandırmak için aşağıdaki değerleri sağlayın.

    ![Olay Hub'ı Oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Olay Hub'ı Oluşturma")

    a. Olay Hub 'ı için bir ad sağlayın.
    
    b. Bu öğreticide, **bölüm sayısı** ve **ileti bekletme** değerlerini varsayılan değerlere ayarlayın.
    
    c. **Yakalamayı** **Açık**olarak ayarlayın. **Zaman penceresini** (yakalama sıklığı) ve **Boyut penceresini** (yakalanacak veri boyutu) ayarlayın. 
    
    d. **Yakalama sağlayıcısı**için **Azure Data Lake Store** ' yi seçin ve ardından daha önce oluşturduğunuz Data Lake Storage 1. hesabı seçin. **Data Lake yolu**için Data Lake Storage 1. hesapta oluşturduğunuz klasörün adını girin. Yalnızca klasöre göreli yolu sağlamanız gerekir.

    e. **Örnek yakalama dosya adı biçimlerini** varsayılan değere bırakın. Bu seçenek, yakalama klasörü altında oluşturulan klasör yapısını yönetir.

    f. **Oluştur**'a tıklayın.

## <a name="test-the-setup"></a>Kurulumu test etme

Artık Azure Olay Hub 'ına veri göndererek çözümü test edebilirsiniz. [Olayları Azure 'A gönderme Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md)yönergeleri izleyin. Verileri göndermeye başladıktan sonra, belirttiğiniz klasör yapısını kullanarak Data Lake Storage 1. yansıtılan verileri görürsünüz. Örneğin, Data Lake Storage 1. hesabınızda aşağıdaki ekran görüntüsünde gösterildiği gibi bir klasör yapısı görürsünüz.

![Data Lake Storage 1. 'de örnek EventHub verileri](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Data Lake Storage 1. 'de örnek EventHub verileri")

> [!NOTE]
> Event Hubs geldiği iletiler olmasa bile, Event Hubs boş dosyaları Data Lake Storage 1. hesabına yalnızca üst bilgilerle yazar. Dosyalar, Event Hubs oluştururken belirttiğiniz zaman aralığında yazılır.
> 
>

## <a name="analyze-data-in-data-lake-storage-gen1"></a>Data Lake Storage 1. verileri analiz etme

Veriler Data Lake Storage 1. olduktan sonra verileri işlemek ve cçalıştırmak için analitik işleri çalıştırabilirsiniz. Azure Data Lake Analytics kullanarak bunun nasıl yapılacağını öğrenmek için bkz. [Usql avro örneği](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) .
  

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure depolama Bloblarından veri kopyalama Data Lake Storage 1.](data-lake-store-copy-data-azure-storage-blob.md)
