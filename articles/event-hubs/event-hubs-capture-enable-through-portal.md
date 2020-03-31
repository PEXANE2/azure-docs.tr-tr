---
title: Etkinlik Hub'ları - Azure portalLarını kullanarak akış olaylarını yakalama
description: Bu makalede, Azure portalını kullanarak Azure Etkinlik Hub'ları üzerinden aktarılan olayların yakalanmasını nasıl etkinleştirin.
services: event-hubs
documentationcenter: ''
author: spelluru
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.custom: seodec18
ms.devlang: na
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: spelluru
ms.openlocfilehash: 8a6d9456b00e5520e6f4fbb9ccb77b0260731ddd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77187363"
---
# <a name="enable-capturing-of-events-streaming-through-azure-event-hubs"></a>Azure Etkinlik Hub'ları aracılığıyla aktarılan olayların yakalanmasını etkinleştirme

Azure [Olay Hub'ları Yakalama,][capture-overview] Etkinlik Hub'larında akış verilerini otomatik olarak seçtiğiniz bir [Azure Blob depolama alanına](https://azure.microsoft.com/services/storage/blobs/) veya Azure Veri Gölü Depolama [Gen1 veya Gen 2](https://azure.microsoft.com/services/data-lake-store/) hesabına teslim etmenizi sağlar.

[Azure portalını](https://portal.azure.com) kullanarak olay hub'ı oluşturma sırasında Yakalama özelliğini yapılandırabilirsiniz. Verileri bir Azure [Blob depolama](https://azure.microsoft.com/services/storage/blobs/) kapsayıcısına veya Bir [Azure Veri Gölü Depolama Gen 1 veya Gen 2](https://azure.microsoft.com/services/data-lake-store/) hesabına yakalayabilirsiniz.

Daha fazla bilgi için bkz. [Event Hubs Capture'a genel bakış][capture-overview].

## <a name="capture-data-to-azure-storage"></a>Azure Depolama'ya veri yakalama

Bir olay hub’ı oluşturduğunuzda **Olay Hub'ı oluştur** portal ekranında **Aç** düğmesine tıklayarak Yakalama özelliğini etkinleştirebilirsiniz. Ardından **Yakalama Sağlayıcısı** kutusunda **Azure Storage**’a tıklayarak bir Depolama Hesabı ve kapsayıcı belirtirsiniz. Event Hubs Yakalama özelliği, depolama ile hizmetten hizmete kimlik doğrulama kullandığından depolama bağlantı dizesi belirtmenize gerek yoktur. Kaynak seçici, depolama hesabınız için kaynak URI'sini otomatik olarak seçer. Azure Resource Manager kullanıyorsanız bu URI'yi dize olarak açıkça belirtmeniz gerekir.

Zaman penceresi varsayılan olarak 5 dakikadır. En düşük değer 1, en yüksek değer ise 15'tir. **Boyut** penceresi 10-500 MB aralığındadır.

![Yakalama için zaman penceresi][1]

> [!NOTE]
> Yakalama penceresi sırasında hiçbir olay oluşmadığında boş dosyaları yaymayı etkinleştirebilir veya devre dışı kullanabilirsiniz. 

## <a name="capture-data-to-azure-data-lake-storage-gen-2"></a>Azure Veri Gölü Depolama Gen 2'ye veri yakalama 

1. Azure Depolama hesabı oluşturmak için [bir depolama hesabı oluştur](../storage/common/storage-account-create.md?tabs=azure-portal#create-a-storage-account) makalesini izleyin. Azure Veri Gölü Depolama Gen 2 hesabı yapmak için **Hiyerarşik ad alanını** **Gelişmiş** sekmesinde **Etkin** olarak ayarlayın.
2. Olay hub'ı oluştururken aşağıdaki adımları yapın: 

    1. **Yakalama**için **A'yı** seçin. 
    2. Yakalama sağlayıcısı olarak **Azure Depolama'yı** seçin. **Yakalama sağlayıcısı** için gördüğünüz Azure Veri **Gölü Deposu** seçeneği, Azure Veri Gölü Depolama'nın Gen 1'i içindir. Azure Veri Gölü Depolama'nın Gen 2'sini kullanmak için **Azure Depolama'yı**seçin.
    2. **Kapsayıcıseç** düğmesini seçin. 

        ![Veri Gölü Depolama Gen 2'ye yakalamayı etkinleştirme](./media/event-hubs-capture-enable-through-portal/data-lake-storage-gen2.png)
3. Listeden **Azure Veri Gölü Depolama Gen 2** hesabını seçin. 

    ![Veri Gölü Depolama Gen 2'yi seçin](./media/event-hubs-capture-enable-through-portal/select-data-lake-storage-gen2.png)
4. **Kapsayıcıyı** seçin (Data Lake Storage Gen 2'deki dosya sistemi).

    ![Depolama alanında dosya sistemini seçin](./media/event-hubs-capture-enable-through-portal/select-file-system-data-lake-storage.png)
5. Olay **Hub'ı Oluştur** sayfasında **Oluştur'u**seçin. 

    ![Oluştur düğmesini seçin](./media/event-hubs-capture-enable-through-portal/create-event-hub-data-lake-storage.png)

    > [!NOTE]
    > Bu kullanıcı arabirimini (UI) kullanarak Bir Azure Veri Gölü Depolama Gen 2'de oluşturduğunuz **kapsayıcı, Depolama Gezgini'ndeki** **Dosya sistemleri** altında gösterilir. Benzer şekilde, Veri Gölü Depolama Gen 2 hesabında oluşturduğunuz dosya sistemi bu kullanıcı bir arada bir arada bir kapsayıcı olarak görünür. 


## <a name="capture-data-to-azure-data-lake-storage-gen-1"></a>Azure Veri Gölü Depolama Gen 1'e veri yakalama 

Azure Veri Gölü Depolama Gen 1'e veri yakalamak için bir Veri Gölü Depolama Gen 1 hesabı ve etkinlik hub'ı oluşturursunuz:

### <a name="create-an-azure-data-lake-storage-gen-1-account-and-folders"></a>Azure Veri Gölü Depolama Gen 1 hesabı ve klasörleri oluşturma

1. [Azure portalını kullanarak Azure Veri Gölü Depolama Gen 1'e başlayın'daki](../data-lake-store/data-lake-store-get-started-portal.md)yönergeleri izleyerek bir Veri Gölü Depolama hesabı oluşturun.
2. Veri Gölü Depolama Gen 1 hesabındaki verileri Olay Hub'larından yakalamak istediğiniz bir klasör oluşturmak ve Veri Gölü Depolama Gen 1 hesabınıza veri yazabilmesi için Etkinlik Hub'larına izinler atamak için [Etkinlik Hub'ları](../data-lake-store/data-lake-store-archive-eventhub-capture.md#assign-permissions-to-event-hubs) bölümüne atama izinlerini izleyin.  


### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

1. Etkinlik hub'ı, oluşturduğunuz Azure Veri Gölü Depolama Gen 1 hesabıyla aynı Azure aboneliğinde olmalıdır. **Event Hub'ı oluştur** portal sayfasının **Yakalama** bölümünde **Açık** düğmesine tıklayarak olay hub'ını oluşturun. 
2. **Event Hub'ı oluştur** portal sayfasında, **Yakalama Sağlayıcısı** kutusundan **Azure Data Lake Store**’u seçin.
3. **Veri Gölü Deposu** açılır listesinin yanındaki Depo'yu **seçin'de,** daha önce oluşturduğunuz Veri Gölü Depolama Gen 1 hesabını belirtin ve Veri Gölü **Yolu** alanında oluşturduğunuz veri klasörüne giden yolu girin.

    ![Veri Gölü Depolama hesabını seçin][3]


## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>Mevcut bir olay hub'ında Yakalama özelliğini yapılandırma

Event Hubs ad alanlarında mevcut olan olay hub'ları üzerinde Yakalama özelliğini yapılandırabilirsiniz. Yakalama özelliğini mevcut bir olay hub'ında etkinleştirmek veya Yakalama ayarlarınızı değiştirmek için ad alanına tıklayarak genel bakış ekranını yükleyin, ardından Yakalama ayarını etkinleştirmek veya değiştirmek istediğiniz olay hub'ına tıklayın. Son olarak, açık sayfanın sol tarafındaki **Yakala** seçeneğine tıklayın ve aşağıdaki şekilde gösterildiği gibi ayarları düzenleyin:

### <a name="azure-blob-storage"></a>Azure Blob Depolama

![Azure Blob Depolamayı Yapılandırma][2]

### <a name="azure-data-lake-storage-gen-2"></a>Azure Data Lake Storage 2. Nesil

![Azure Veri Gölü Depolama Gen 2'yi Yapılandırma](./media/event-hubs-capture-enable-through-portal/configure-data-lake-storage-gen2.png)

### <a name="azure-data-lake-storage-gen-1"></a>Azure Veri Gölü Depolama Gen 1 

![Azure Veri Gölü Depolamayı Yapılandırma][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png

## <a name="next-steps"></a>Sonraki adımlar

- Olay hub’larını yakalama hakkında daha fazla bilgi için bkz. [Event Hubs Yakalama özelliğine genel bakış][capture-overview].
- Dilerseniz Azure Resource Manager şablonlarını kullanarak da Event Hubs Yakalama özelliğini yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager şablonu kullanarak Yakalama özelliğini etkinleştirme](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).
- [Kaynağı Event Hubs ad alanı olarak bir Azure Event Grid aboneliği oluşturmayı öğrenin](store-captured-data-data-warehouse.md)
- [Azure portalını kullanarak Azure Veri Gölü Deposu'na başlayın](../data-lake-store/data-lake-store-get-started-portal.md)

[capture-overview]: event-hubs-capture-overview.md
