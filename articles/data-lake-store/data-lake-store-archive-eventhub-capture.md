---
title: Etkinlik Hub'larından Azure Veri Gölü Depolama Gen1'e veri yakalama | Microsoft Dokümanlar
description: Olay Hub'larından veri yakalamak için Azure Veri Gölü Depolama Gen1'i kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: bb67c1769510710b368bef4dc0b501f939b3427e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265668"
---
# <a name="use-azure-data-lake-storage-gen1-to-capture-data-from-event-hubs"></a>Olay Hub'larından veri yakalamak için Azure Veri Gölü Depolama Gen1'i kullanın

Azure Etkinlik Hub'ları tarafından alınan verileri yakalamak için Azure Veri Gölü Depolama Gen1'i nasıl kullanacağınızı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Bir Azure Veri Gölü Depolama Gen1 hesabı.** Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)

*  **Olay Hub'ları ad alanı.** Yönergeler için [bkz.](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace) Veri Gölü Depolama Gen1 hesabı nın ve Olay Hub'larının ad alanının aynı Azure aboneliğinde olduğundan emin olun.


## <a name="assign-permissions-to-event-hubs"></a>Etkinlik Hub'larına izin atama

Bu bölümde, hesap içinde Olay Hub'larından verileri yakalamak istediğiniz bir klasör oluşturursunuz. Ayrıca, veri veri sini bir Veri Gölü Depolama Gen1 hesabına yazabilmesi için Etkinlik Hub'larına izinler atarsınız. 

1. Olay Hub'larından veri yakalamak istediğiniz Veri Gölü Depolama Gen1 hesabını açın ve ardından **Data Explorer'ı**tıklatın.

    ![Veri Gölü Depolama Gen1 veri gezgini](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Veri Gölü Depolama Gen1 veri gezgini")

1.  **Yeni Klasör'ü** tıklatın ve ardından verileri yakalamak istediğiniz klasör için bir ad girin.

    ![Veri Gölü Depolama Gen1'de yeni bir klasör oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Veri Gölü Depolama Gen1'de yeni bir klasör oluşturma")

1. Data Lake Storage Gen1'in köküne izin atayın. 

    a. **Veri Gezgini'ni**tıklatın, Veri Gölü Depolama Gen1 hesabının kökünü seçin ve ardından **Access'i**tıklatın.

    ![Veri Gölü Depolama Gen1 kökü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Veri Gölü Depolama Gen1 kökü için izin atama")

    b. **Access'in**altında **Ekle'yi**tıklatın, Kullanıcı veya `Microsoft.EventHubs` **Grubu Seç'i**tıklatın ve ardından arayın. 

    ![Veri Gölü Depolama Gen1 kökü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Veri Gölü Depolama Gen1 kökü için izin atama")
    
    **Seç'i**tıklatın.

    c. **İzinatama**altında, **İzinleri Seçin'i**tıklatın. **Yürütme** **İzinleri** Ayarlama. **Bu klasöre ve tüm çocuklara** **Ekle'yi** ayarlayın. Erişim izni **girişine ve varsayılan izin girişine** **Ekle'yi** ayarlayın.

    > [!IMPORTANT]
    > Azure Etkinlik Hub'ları tarafından alınan verileri yakalamak için yeni bir klasör hiyerarşisi oluştururken, hedef klasöre erişimi sağlamanın kolay bir yoludur.  Ancak, birçok alt dosya ve klasörü içeren bir üst düzey klasörün tüm çocuklarına izin eklemek uzun sürebilir.  Kök klasörünüz çok sayıda dosya ve klasör içeriyorsa, **Execute** son hedef `Microsoft.EventHubs` klasörünüzdeki yolda her klasöre ayrı ayrı Yürüt İzinleri eklemek daha hızlı olabilir. 

    ![Veri Gölü Depolama Gen1 kökü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Veri Gölü Depolama Gen1 kökü için izin atama")

    **Tamam**'a tıklayın.

1. Veri yakalamak istediğiniz Veri Gölü Depolama Gen1 hesabının altındaki klasör için izinler atayın.

    a. **Veri Gezgini'ni**tıklatın, Veri Gölü Depolama Gen1 hesabındaki klasörü seçin ve ardından **Access'i**tıklatın.

    ![Veri Gölü Depolama Gen1 klasörü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Veri Gölü Depolama Gen1 klasörü için izin atama")

    b. **Access'in**altında **Ekle'yi**tıklatın, Kullanıcı veya `Microsoft.EventHubs` **Grubu Seç'i**tıklatın ve ardından arayın. 

    ![Veri Gölü Depolama Gen1 klasörü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Veri Gölü Depolama Gen1 klasörü için izin atama")
    
    **Seç'i**tıklatın.

    c. **İzinatama**altında, **İzinleri Seçin'i**tıklatın. **İzinleri** **Okuma, Yazma** ve **Yürütme**ayarlama. **Bu klasöre ve tüm çocuklara** **Ekle'yi** ayarlayın. Son olarak, **erişim izni girişi ve varsayılan izin girişi**ne kadar **ekle'yi** ayarlayın.

    ![Veri Gölü Depolama Gen1 klasörü için izin atama](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Veri Gölü Depolama Gen1 klasörü için izin atama")
    
    **Tamam**'a tıklayın. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'e veri yakalamak için Olay Hub'larını yapılandırın

Bu bölümde, bir Olay Hub'ları ad alanı içinde bir Olay Hub'ı oluşturursunuz. Ayrıca, Etkinlik Hub'ını bir Azure Veri Gölü Depolama Gen1 hesabına veri yakalamak için yapılandırırsınız. Bu bölüm, bir Olay Hub'ları ad alanı zaten oluşturduğunuzu varsayar.

1. Olay Hub'larının ad alanının **Genel Bakış** bölmesinden + **Olay Hub'ını**tıklatın.

    ![Olay Hub'ı Oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Olay Hub'ı Oluşturma")

1. Veri Gölü Depolama Gen1'e veri yakalamak için Olay Hub'larını yapılandırmak için aşağıdaki değerleri sağlayın.

    ![Olay Hub'ı Oluşturma](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Olay Hub'ı Oluşturma")

    a. Olay Hub'ı için bir ad sağlayın.
    
    b. Bu öğretici **için, Bölüm Sayısı** ve **İleti Bekletme'yi** varsayılan değerlere ayarlayın.
    
    c. **Capture'i** **A'ya**ayarlayın. Zaman **Penceresini** (ne sıklıkta yakalayacak) ve **Boyut Penceresini** (yakalamak için veri boyutu) ayarlayın. 
    
    d. **Yakalama Sağlayıcısı**için Azure Veri **Gölü Deposu'nu** seçin ve ardından daha önce oluşturduğunuz Veri Gölü Depolama Gen1 hesabını seçin. **Veri Gölü Yolu**için, Veri Gölü Depolama Gen1 hesabında oluşturduğunuz klasörün adını girin. Yalnızca klasöre göreli yolu sağlamanız gerekir.

    e. Örnek **yakalama dosya adı biçimlerini** varsayılan değere bırakın. Bu seçenek, yakalama klasörü altında oluşturulan klasör yapısını yönetir.

    f. **Oluştur'u**tıklatın.

## <a name="test-the-setup"></a>Kurulumu test edin

Artık Azure Etkinlik Hub'ına veri göndererek çözümü test edebilirsiniz. Etkinlikleri Azure [Etkinlik Hub'larına Gönder'deki](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md)yönergeleri izleyin. Verileri göndermeye başladığınızda, belirttiğiniz klasör yapısını kullanarak Verilerin Veri Gölü Depolama Gen1'ine yansıtTığını görürsünüz. Örneğin, Aşağıdaki ekran görüntüsünde gösterildiği gibi, Veri Gölü Depolama Gen1 hesabınızda bir klasör yapısı görürsünüz.

![Veri Gölü Depolama Gen1 örnek EventHub verileri](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Veri Gölü Depolama Gen1 örnek EventHub verileri")

> [!NOTE]
> Olay Hub'larına gelen iletiler olmasa bile, Olay Hub'ları veri gölü depolama gen1 hesabına yalnızca başlıklarla boş dosyalar yazar. Dosyalar, Olay Hub'larını oluştururken sağladığınız aynı zaman aralığında yazılır.
> 
>

## <a name="analyze-data-in-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'deki verileri analiz edin

Veriler Veri Gölü Depolama Gen1'e ulaştıktan sonra, verileri işlemek ve daraltmak için analitik işler çalıştırabilirsiniz. Azure Veri Gölü Analizi'ni kullanarak bunu nasıl yapacağınız hakkında [USQL Avro Örneği'ne](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) bakın.
  

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Verileri Azure Depolama Blobs'undan Veri Gölü Depolama Gen1'e kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
