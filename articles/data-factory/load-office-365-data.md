---
title: Azure Veri Fabrikası'nı kullanarak Office 365'ten veri yükleme
description: Office 365'ten verileri kopyalamak için Azure Veri Fabrikası'nı kullanma
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: jingwang
ms.openlocfilehash: 3422176ed89b7f575c11cc40e5be8420da0018b0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415809"
---
# <a name="load-data-from-office-365-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Office 365'ten veri yükleme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, _Office 365'teki_Veri Fabrikası yük verilerini Azure Blob depolama alanına nasıl kullanacağınızı gösterilmektedir. Verileri Azure Veri Gölü Gen1 veya Gen2'ye kopyalamak için benzer adımları izleyebilirsiniz. Genel olarak Office 365'ten veri kopyalama yla ilgili [Office 365 bağlayıcı makalesine](connector-office-365.md) bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüde, **bir kaynak** > **Analiz** > **Veri Fabrikası**Oluştur'u seçin: 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Yeni **veri fabrikası** sayfasında, aşağıdaki resimde gösterilen alanlar için değerler sağlayın:
      
   ![Yeni veri fabrikası sayfası](./media/load-office-365-data/new-azure-data-factory.png)
 
    * **Ad**: Azure veri fabrikanız için benzersiz bir ad girin. "Veri fabrikası adı *LoadFromOffice365Demo* kullanılamıyor" hatasını alırsanız, veri fabrikası için farklı bir ad girin. Örneğin,**loadFromOffice365Demo** _**adını**_ kullanabilirsiniz. Veri fabrikasını yeniden oluşturmayı deneyin. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasını oluşturmak için Azure aboneliğinizi seçin. 
    * **Kaynak Grubu**: Açılan listeden varolan bir kaynak grubu seçin veya **Yeni Oluştur** seçeneğini belirleyin ve bir kaynak grubunun adını girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
    * **Sürüm**: **V2**seçin.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. Bu veri depoları Azure Veri Gölü Deposu, Azure Depolama, Azure SQL Veritabanı ve benzeri bilgileri içerir.

3. **Oluştur**’u seçin.
4. Oluşturma tamamlandıktan sonra veri fabrikanıza gidin. Aşağıdaki resimde gösterildiği gibi **Veri Fabrikası** ana sayfasını görürsünüz:
   
   ![Data factory giriş sayfası](./media/load-office-365-data/data-factory-home-page.png)

5. Veri Tümleştirme Uygulamasını ayrı bir sekmede açmak için **Oluştur ve İzle** kutucuğunu seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. "Başlayalım" **sayfasında, ardışık hat lar oluştur'u**seçin.
 
    ![İşlem hattı oluşturma](./media/load-office-365-data/create-pipeline-entry.png)

2. Ardışık hatlar için **Genel sekmesinde,** ardışık hattın **adı** için "CopyPipeline" girin.

3. Etkinlikler araç kutusunda > Taşı & Dönüştür kategorisini > sürükle ve araç **kutusundan** boru hattı tasarımcısı yüzeyine kopyala. Etkinlik adı olarak "CopyFromOffice365ToBlob" belirtin.

### <a name="configure-source"></a>Kaynağı yapılandırma

1. **Kaynak sekmesine**> ardışık kanala gidin , kaynak veri kümesi oluşturmak için **+ Yeni'yi** tıklatın. 

2. Yeni Veri Kümesi penceresinde **Office 365'i**seçin ve ardından **Devam et'i**seçin.
 
3. Artık kopyalama etkinliği yapılandırma sekmesindesiniz. Veri yapılandırmasına devam etmek için Office 365 veri kümesinin yanındaki **Düzenleme** düğmesini tıklatın.

    ![Config Office 365 dataset genel](./media/load-office-365-data/transition-to-edit-dataset.png)
 
4. Office 365 veri kümesi için yeni bir sekme açıldığını görürsünüz. Özellikler penceresinin altındaki **Genel sekmesine** Ad için "SourceOffice365Dataset" girin.
 
5. Özellikler penceresinin **Bağlantı sekmesine** gidin. Bağlantılı hizmet metin kutusunun yanında **+ Yeni'yi**tıklatın.

6. Yeni Bağlantılı Hizmet penceresinde, "Office365LinkedService" adını girin, hizmet ana kimliği ve hizmet ana anahtarıgirin, ardından bağlantıyı test edin ve bağlantılı hizmeti dağıtmak için **Oluştur'u** seçin.

    ![Yeni Office 365 bağlantılı hizmet](./media/load-office-365-data/new-office-365-linked-service.png)
 
7. Bağlı hizmet oluşturulduktan sonra veri kümesi ayarlarına dönersiniz. **Tablo'nun**yanında, kullanılabilir Office 365 veri kümelerinin listesini genişletmek için aşağı oku seçin ve "BasicDataSet_v0" seçeneğini belirleyin. açılan listeden Message_v0" :

    ![Config Office 365 veri kümesi tablosu](./media/load-office-365-data/edit-dataset.png)

8. Şimdi, Office 365 veri ayıklama için ek özellikleri yapılandırmaya devam etmek için **ardışık kaynak** > **sekmesine** geri dön.  Kullanıcı kapsamı ve kullanıcı kapsamı filtresi, Office 365'ten ayıklamak istediğiniz verileri kısıtlamak için tanımlayabileceğiniz isteğe bağlı yüklemlerdir. Bu ayarları nasıl yapılandırdığınıza göre [Office 365 veri kümesi özellikleri](https://docs.microsoft.com/azure/data-factory/connector-office-365#dataset-properties) bölümüne bakın.

9. Tarih filtrelerinden birini seçmeniz ve başlangıç saati ve bitiş saati değerlerini sağlamanız gerekir.

10. İleti veri kümesi için şemayı almak için **Şema'yı Aktar** sekmesini tıklatın.

    ![Config Office 365 dataset şema](./media/load-office-365-data/edit-source-properties.png)

### <a name="configure-sink"></a>Havuzu yapılandırma

1. **Büzerek**> sekmesine gidin ve bir lavabo veri kümesi oluşturmak için **+ Yeni'yi** seçin.
 
2. Yeni Veri Kümesi penceresinde, Office 365'ten kopyalanırken yalnızca desteklenen hedeflerin seçildiğine dikkat edin. **Azure Blob Depolama'yı**seçin, İkili biçim'i seçin ve ardından Devam **et'i**seçin.  Bu eğitimde, Office 365 verilerini bir Azure Blob Depolama alanına kopyalarsınız.

3. Veri yapılandırmasına devam etmek için Azure Blob Depolama veri kümesinin yanındaki **Düzenleme** düğmesini tıklatın.

4. Özellikler penceresinin **Genel sekmesine** Ad'da "OutputBlobDataset" girin.

5. Özellikler penceresinin **Bağlantı sekmesine** gidin. Bağlı hizmet metin kutusunun yanındaki **+ Yeni** seçeneğini belirleyin.

6. Yeni Bağlantılı Hizmet penceresinde, ad olarak "AzureStorageLinkedService" girin, kimlik doğrulama yöntemleriaçılır listesinden "Hizmet Sorumlusu" seçeneğini belirleyin, Hizmet Bitiş Noktası, Kiracı, Hizmet asıl kimliği ve Hizmet ana anahtarını doldurun ve ardından bağlantılı hizmeti dağıtmak için Kaydet'i seçin.  Azure Blob Depolama için hizmet temel kimlik doğrulaması nasıl ayarlanır için [buraya](connector-azure-blob-storage.md#service-principal-authentication) bakın.

    ![Yeni Blob bağlantılı hizmet](./media/load-office-365-data/configure-blob-linked-service.png)


## <a name="validate-the-pipeline"></a>İşlem hattını doğrulama

İşlem hattını doğrulamak için araç çubuğundan **Doğrula**'yı seçin.

Ayrıca sağ üstteki Kodu tıklatarak ardışık alanla ilişkili JSON kodunu da görebilirsiniz.

## <a name="publish-the-pipeline"></a>Ardışık hattı yayımlama

Üstteki araç çubuğunda **Tümünü Yayımla'yı**seçin. Bu eylem, oluşturduğunuz varlıkları (veri kümeleri ve işlem hatları) Data Factory'de yayımlar.

![Değişiklikleri yayımlama](./media/load-office-365-data/publish-changes.png) 

## <a name="trigger-the-pipeline-manually"></a>İşlem hattını el ile tetikleme

Araç çubuğunda **Tetikleyici Ekle'yi** ve ardından Şimdi **Tetikle'yi**seçin. İşlem Hattı Çalıştırma sayfasında **Son**’u seçin. 

## <a name="monitor-the-pipeline"></a>İşlem hattını izleme

Soldaki **İzleyici** sekmesine gidin. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Etkinlik ayrıntılarını görüntülemek ve ardışık hattı yeniden çalıştırmak için **Eylemler** sütunundaki bağlantıları kullanabilirsiniz.

![İşlem hattını izleme](./media/load-office-365-data/pipeline-status.png) 

İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görmek için Eylemler sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Bu örnekte, tek bir etkinlik olduğundan listede tek giriş görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için Eylemler sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin.

![Etkinliği izleme](./media/load-office-365-data/activity-status.png) 

Bu bağlam için ilk kez veri talep ediyorsanız (hangi veri tablosuna erişiliyor, hangi hedef hesaba yüklenen veriler ve hangi kullanıcı kimliği veri erişim isteğini yapıyor), kopyalama etkinliği durumunu **Devam Ediyor**olarak görürsünüz ve yalnızca Eylemler altındaki "Ayrıntılar" bağlantısını tıklattığınızda durumu **RequesetingConsent**olarak görürsünüz.  Veri ayıklama işlemi nin devam edebilmesi için veri erişimi onaylayıcı grubunun bir üyesinin Ayrıcalıklı Erişim Yönetimi'ndeki isteği onaylaması gerekir.

_İzin isteme durumu:_
![Etkinlik yürütme ayrıntıları - istek onayı](./media/load-office-365-data/activity-details-request-consent.png) 

_Veri ayıklama durumu:_

![Etkinlik yürütme ayrıntıları - veri ayıklama](./media/load-office-365-data/activity-details-extract-data.png) 

Onay sağlandıktan sonra, veri ayıklama devam edecek ve bir süre sonra, ardışık hatlar çalışması başarılı olarak gösterecektir.

![Monitör boru hattı - başarılı](./media/load-office-365-data/pipeline-monitoring-succeeded.png) 

Şimdi hedef Azure Blob Depolama'ya gidin ve Office 365 verilerinin İkili biçimde ayıklandığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure SQL Veri Ambarı desteği hakkında bilgi edinmek için aşağıdaki makaleye ilerleyin: 

> [!div class="nextstepaction"]
>[Office 365 bağlayıcısı](connector-office-365.md)