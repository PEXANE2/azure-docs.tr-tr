---
title: Azure Veri Kataloğu'nda Azure Veri Gölü Depolama Gen1'den veri kaydetme | Microsoft Dokümanlar
description: Azure Veri Kataloğu'nda Azure Veri Gölü Depolama Gen1'den veri kaydetme
services: data-lake-store,data-catalog
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: fd887560c0011fb1ec2141e33f02f7e3d8a39c81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60196611"
---
# <a name="register-data-from-azure-data-lake-storage-gen1-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda Azure Veri Gölü Depolama Gen1'den veri kaydetme
Bu makalede, Veri Kataloğu ile tümleştirerek verilerinizi kuruluş içinde bulunabilir hale getirmek için Azure Veri Gölü Depolama Gen1'i Azure Veri Kataloğu ile nasıl entegre edeceğinizi öğreneceksiniz. Veri kataloglama hakkında daha fazla bilgi için [Azure Veri Kataloğu'na](../data-catalog/data-catalog-what-is-data-catalog.md)bakın. Veri Kataloğu'nu kullanabileceğiniz senaryoları anlamak için [Azure Veri Kataloğu ortak senaryolarına](../data-catalog/data-catalog-common-scenarios.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* Veri Gölü Depolama Gen1 için **Azure aboneliğinizi etkinleştirin.** Bkz. [yönergeler](data-lake-store-get-started-portal.md).
* **Bir Veri Gölü Depolama Gen1 hesabı**. [Azure Portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın](data-lake-store-get-started-portal.md)yönergeleri izleyin. Bu öğretici için, **datacatalogstore**adlı bir Veri Gölü Depolama Gen1 hesabı oluşturun.

    Hesabı oluşturduktan sonra, bir örnek veri kümesi yükleyin. Bu öğretici için, [Azure Veri Gölü Git Deposu'ndaki](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/) **AmbulanceData** klasörü altındaki tüm .csv dosyalarını yükleyelim. Verileri bir blob kapsayıcısına yüklemek için [Azure Depolama Gezgini](https://storageexplorer.com/)gibi çeşitli istemciler kullanabilirsiniz.
* **Azure Veri Kataloğu**. Kuruluşunuzun kuruluşunuz için zaten bir Azure Veri Kataloğu oluşturulmuş olması gerekir. Her kuruluş için yalnızca bir kataloga izin verilir.

## <a name="register-data-lake-storage-gen1-as-a-source-for-data-catalog"></a>Veri Gölü Depolama Gen1'i Veri Kataloğu için kaynak olarak kaydedin

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. 'ye `https://azure.microsoft.com/services/data-catalog`gidin ve **Başlat'ı**tıklatın.
1. Azure Veri Kataloğu portalında oturum açın ve **Verileri yayımla**'ya tıklayın.

    ![Veri kaynağını kaydetme](./media/data-lake-store-with-data-catalog/register-data-source.png "Veri kaynağını kaydetme")
1. Sonraki sayfada Uygulamayı **Başlat'ı**tıklatın. Bu, uygulama bildirimi dosyasını bilgisayarınıza indirir. Uygulamayı başlatmak için manifesto dosyasına çift tıklayın.
1. Hoş Geldiniz sayfasında **Oturum Aç'ı**tıklatın ve kimlik bilgilerinizi girin.

    ![Hoş Geldiniz ekranı](./media/data-lake-store-with-data-catalog/welcome.screen.png "Hoş Geldiniz ekranı")
1. Veri Kaynağı Seç sayfasında **Azure Veri Gölü Deposu'nu**seçin ve sonra **İleri'yi**tıklatın.

    ![Veri kaynağı seçme](./media/data-lake-store-with-data-catalog/select-source.png "Veri kaynağı seçme")
1. Bir sonraki sayfada, Veri Kataloğu'na kaydetmek istediğiniz Veri Gölü Depolama Gen1 hesap adını girin. Diğer seçenekleri varsayılan olarak bırakın ve sonra **Bağlan'ı**tıklatın.

    ![Veri kaynağına bağlanma](./media/data-lake-store-with-data-catalog/connect-to-source.png "Veri kaynağına bağlanma")
1. Bir sonraki sayfa aşağıdaki bölümlere ayrılabilir.

    a. **Sunucu Hiyerarşisi** kutusu Veri Gölü Depolama Gen1 hesap klasörü yapısını temsil eder. **$Root** Veri Gölü Depolama Gen1 hesap kökünü temsil eder ve **AmbulanceData,** Veri Gölü Depolama Gen1 hesabının kökünde oluşturulan klasörü temsil eder.

    b. **Kullanılabilir nesneler** **kutusu, AmbulanceData** klasörü altındaki dosya ve klasörleri listeler.

    c. **Kutuda kaydedilecek nesneler,** Azure Veri Kataloğu'na kaydetmek istediğiniz dosya ve klasörleri listeler.

    ![Veri yapısını görüntüleme](./media/data-lake-store-with-data-catalog/view-data-structure.png "Veri yapısını görüntüleme")
1. Bu öğretici için, tüm dosyaları dizine kaydettirmeniz gerekir. Bunun için, tüm dosyaları **kayıtlı** kutuya taşımak için (![nesneleri taşı](./media/data-lake-store-with-data-catalog/move-objects.png "Nesneleri taşıma")) düğmesini tıklatın.

    Veriler kuruluş çapında bir veri kataloğuna kaydedilecektir, daha sonra verileri hızla bulmak için kullanabileceğiniz bazı meta verileri eklemek önerilir bir yaklaşımdır. Örneğin, veri sahibine (örneğin, verileri yükleyen bir e-posta adresi) veya verileri tanımlamak için bir etiket ekleyebilirsiniz. Aşağıdaki ekran yakalama, verilere eklediğiniz bir etiketi gösterir.

    ![Veri yapısını görüntüleme](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Veri yapısını görüntüleme")

    **Kaydol'u**tıklatın.
1. Aşağıdaki ekran yakalama, verilerin Veri Kataloğu'na başarıyla kaydolduğunu gösterir.

    ![Kayıt tamamlandı](./media/data-lake-store-with-data-catalog/registration-complete.png "Veri yapısını görüntüleme")
1. Veri Kataloğu portalına geri dönmek ve kayıtlı verilere artık portaldan erişebileceğinizi doğrulamak için **Portalı Görüntüle'yi** tıklatın. Verileri aramak için, verileri kaydederken kullandığınız etiketi kullanabilirsiniz.

     ![Katalogdaki verileri arama](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Katalogdaki verileri arama")
1. Artık verilere ek açıklamalar ve belgeler ekleme gibi işlemler gerçekleştirebilirsiniz. Daha fazla bilgi için aşağıdaki bağlantılara bakın.

    * [Veri Kataloğu'ndaki veri kaynaklarına açıklama](../data-catalog/data-catalog-how-to-annotate.md)
    * [Veri Kataloğu'ndaki belge veri kaynakları](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Veri Kataloğu'ndaki veri kaynaklarına açıklama](../data-catalog/data-catalog-how-to-annotate.md)
* [Veri Kataloğu'ndaki belge veri kaynakları](../data-catalog/data-catalog-how-to-documentation.md)
* [Veri Gölü Depolama Gen1'i diğer Azure hizmetleriyle tümleştirin](data-lake-store-integrate-with-other-services.md)
