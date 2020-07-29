---
title: Azure Veri Kataloğu ile Data Lake Storage 1. tümleştirme
description: Verileri kuruluşunuzda bulunabilir hale getirmek için Azure Veri Kataloğu 'nda Azure Data Lake Storage 1. verileri nasıl kaydedeceğinizi öğrenin.
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 1484c4bf17d5f437d681e32559b7e6ed4115f5f5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85515142"
---
# <a name="register-data-from-azure-data-lake-storage-gen1-in-azure-data-catalog"></a>Azure Veri Kataloğu 'nda Azure Data Lake Storage 1. verileri kaydetme
Bu makalede, verileri veri kataloğu ile tümleştirerek bir kuruluşta bulunabilir hale getirmek için Azure Data Lake Storage 1. Azure Veri Kataloğu ile tümleştirmeyi öğreneceksiniz. Verileri kataloglandırma hakkında daha fazla bilgi için bkz. [Azure Veri Kataloğu](../data-catalog/data-catalog-what-is-data-catalog.md). Veri kataloğunu kullanabileceğiniz senaryoları anlamak için bkz. [Azure Veri Kataloğu genel senaryoları](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* Data Lake Storage 1. için **Azure aboneliğinizi etkinleştirin** . [Yönergelere](data-lake-store-get-started-portal.md)bakın.
* **Data Lake Storage 1. hesabı**. [Azure Portal kullanarak Azure Data Lake Storage 1. kullanmaya başlama](data-lake-store-get-started-portal.md)yönergelerini izleyin. Bu öğretici için **datacatalogstore**adlı bir Data Lake Storage 1. hesabı oluşturun.

    Hesabı oluşturduktan sonra, ona bir örnek veri kümesi yükleyin. Bu öğreticide, [Azure Data Lake git deposundaki](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/) **AmbulanceData** klasörünün altındaki tüm. csv dosyalarını karşıya yüklememize izin verin. Blob kapsayıcısına veri yüklemek için [Azure Depolama Gezgini](https://storageexplorer.com/)gibi çeşitli istemcileri kullanabilirsiniz.
* **Azure Veri Kataloğu**. Kuruluşunuz için kuruluşunuz için oluşturulmuş bir Azure Veri Kataloğu zaten olmalıdır. Her kuruluş için yalnızca bir kataloğa izin verilir.

## <a name="register-data-lake-storage-gen1-as-a-source-for-data-catalog"></a>Veri Kataloğu için kaynak olarak Data Lake Storage 1. kaydetme

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Adresine gidin `https://azure.microsoft.com/services/data-catalog` ve **kullanmaya**başlayın ' a tıklayın.
1. Azure Veri Kataloğu portalında oturum açın ve **Verileri yayımla**'ya tıklayın.

    ![Veri kaynağını kaydetme](./media/data-lake-store-with-data-catalog/register-data-source.png "Veri kaynağını kaydetme")
1. Sonraki sayfada, **Uygulamayı Başlat**' a tıklayın. Bu işlem, bilgisayarınızda uygulama bildirim dosyasını indirir. Uygulamayı başlatmak için bildirim dosyasına çift tıklayın.
1. Hoş geldiniz sayfasında **oturum aç**' a tıklayın ve kimlik bilgilerinizi girin.

    ![Hoş Geldiniz ekranı](./media/data-lake-store-with-data-catalog/welcome.screen.png "Hoş Geldiniz ekranı")
1. Veri kaynağı seçin sayfasında **Azure Data Lake Store**' yi seçin ve ardından **İleri**' ye tıklayın.

    ![Veri kaynağı seçme](./media/data-lake-store-with-data-catalog/select-source.png "Veri kaynağı seçme")
1. Sonraki sayfada, veri kataloğuna kaydetmek istediğiniz Data Lake Storage 1. hesap adını girin. Diğer seçenekleri varsayılan olarak bırakın ve sonra **Bağlan**' a tıklayın.

    ![Veri kaynağına bağlanma](./media/data-lake-store-with-data-catalog/connect-to-source.png "Veri kaynağına bağlanma")
1. Sonraki sayfa aşağıdaki kesimlere ayrılabilir.

    a. **Sunucu hiyerarşisi** kutusu Data Lake Storage 1. Hesap klasörü yapısını temsil eder. **$Root** , Data Lake Storage 1. hesap kökünü temsil eder ve **AmbulanceData** Data Lake Storage 1. hesabının kökünde oluşturulan klasörü temsil eder.

    b. **Kullanılabilir nesneler** kutusu **AmbulanceData** klasörü altındaki dosya ve klasörleri listeler.

    c. Kaydedilecek **nesneler** kutusu, Azure Veri Kataloğu 'nda kaydetmek istediğiniz dosya ve klasörleri listeler.

    ![Veri yapısını görüntüle](./media/data-lake-store-with-data-catalog/view-data-structure.png "Veri yapısını görüntüle")
1. Bu öğreticide, dizindeki tüm dosyaları kaydetmeniz gerekir. Bu şekilde, tüm dosyaları **kaydedilecek nesneler** kutusuna taşımak için (![nesneleri taşı](./media/data-lake-store-with-data-catalog/move-objects.png "Nesneleri taşı")) düğmesine tıklayın.

    Veriler kuruluş genelinde bir veri kataloğunda kaydedilecek, daha sonra verileri hızlı bir şekilde bulmak için kullanabileceğiniz bazı meta verileri eklemek önerilen bir yaklaşımdır. Örneğin, veri sahibi için bir e-posta adresi ekleyebilirsiniz (örneğin, verileri karşıya yükleyen bir tane) veya verileri tanımlamak için bir etiket ekleyebilirsiniz. Aşağıdaki ekran yakalama, verilere eklediğiniz bir etiketi gösterir.

    ![Veri yapısını görüntüle](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Veri yapısını görüntüle")

    **Kaydet**’e tıklayın.
1. Aşağıdaki ekran yakalama, verilerin veri kataloğunda başarıyla kaydedildiğini gösterir.

    ![Kayıt Tamam](./media/data-lake-store-with-data-catalog/registration-complete.png "Veri yapısını görüntüle")
1. Veri Kataloğu portalına geri dönmek ve artık portaldan kayıtlı verilere erişebildiğinizi doğrulamak için **portalı görüntüle** ' ye tıklayın. Verileri aramak için, verileri kaydederken kullandığınız etiketi kullanabilirsiniz.

     ![Katalogda veri arama](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Katalogda veri arama")
1. Artık verilere ek açıklama ve belge ekleme gibi işlemler yapabilirsiniz. Daha fazla bilgi için aşağıdaki bağlantılara bakın.

    * [Veri kataloğunda veri kaynaklarına açıklama ekleme](../data-catalog/data-catalog-how-to-annotate.md)
    * [Veri kataloğunda belge veri kaynakları](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Veri kataloğunda veri kaynaklarına açıklama ekleme](../data-catalog/data-catalog-how-to-annotate.md)
* [Veri kataloğunda belge veri kaynakları](../data-catalog/data-catalog-how-to-documentation.md)
* [Data Lake Storage 1. diğer Azure hizmetleriyle tümleştirin](data-lake-store-integrate-with-other-services.md)
