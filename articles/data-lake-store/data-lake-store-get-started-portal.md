---
title: Azure Veri Gölü Depolama Gen1 ile başlayın - portal
description: Bir Veri Gölü Depolama Gen1 hesabı oluşturmak ve hesaptaki temel işlemleri gerçekleştirmek için Azure portalını kullanın.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 1bfb9362aa0e893a8bf1a758e5a0069e85ab227b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265590"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-the-azure-portal"></a>Azure portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [Powershell](data-lake-store-get-started-powershell.md)
> * [Azure CLI](data-lake-store-get-started-cli-2.0.md)
>
>

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Bir Veri Gölü Depolama Gen1 hesabı oluşturmak ve klasör oluşturma, veri dosyaları yükleme ve indirme, hesabınızı silme vb. gibi temel işlemleri gerçekleştirmek için Azure portalını nasıl kullanacağınızı öğrenin. Daha fazla bilgi için Bkz. [Azure Veri Gölü Depolama Gen1'e Genel Bakış.](data-lake-store-overview.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. Nesil hesabı oluşturma

1. Yeni [Azure portalında](https://portal.azure.com)oturum açın.
2. **Veri Gölü Depolama Gen1 > Depolama > kaynak oluştur'u**tıklatın.
3. Yeni **Veri Gölü Depolama Gen1** bıçak, aşağıdaki ekran görüntüsünde gösterildiği değerleri sağlayın:

    ![Yeni bir Veri Gölü Depolama Gen1 hesabı oluşturma](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Yeni bir Veri Gölü Depolama Gen1 hesabı oluşturma")

   * **Adı**. Veri Gölü Depolama Gen1 hesabı için benzersiz bir ad girin.
   * **Abonelik**. Yeni bir Veri Gölü Depolama Gen1 hesabı oluşturmak istediğiniz aboneliği seçin.
   * **Kaynak Grubu**. Mevcut bir kaynak grubunu seçin veya yeni grup oluşturmak için **Yeni oluştur** seçeneğini belirleyin. Kaynak grubu, bir uygulama için ilgili kaynakları bir arada tutan bir kapsayıcıdır. Daha fazla bilgi için bkz. [Azure'da Kaynak Grupları](../azure-resource-manager/management/overview.md#resource-groups).
   * **Konum**: Veri Gölü Depolama Gen1 hesabını oluşturmak istediğiniz bir konum seçin.
   * **Şifreleme Ayarları**. Üç seçenek vardır:

     * **Şifrelemeyi etkinleştirme**.
     * Şifreleme anahtarlarınızı yönetmek için **Data Lake Storage Gen1 tarafından yönetilen anahtarları kullanın.**
     * **Kendi Anahtar Kasanızdaki anahtarları kullanın**. Mevcut bir Azure Key Vault’u seçin veya yeni bir Key Vault oluşturun. Anahtar Kasası'ndaki anahtarları kullanmak için Azure Anahtar Kasası'na erişmek için Veri Gölü Depolama Gen1 hesabı için izin ler atamanız gerekir. Yönergeler için bkz. [Azure Key Vault'a izinleri atama](#assign-permissions-to-azure-key-vault).

        ![Veri Gölü Depolama Gen1 şifrelemesi](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Veri Gölü Depolama Gen1 şifrelemesi")

        **Şifreleme Ayarları** dikey penceresinde **Tamam**’a tıklayın.

        Daha fazla bilgi için Azure [Veri Gölü Depolama Gen1'deki verilerin Şifrelemesi'ne](./data-lake-store-encryption.md)bakın.

4. **Oluştur'u**tıklatın. Hesabı panoya sabitlemeyi seçtiyseniz, panoya geri alınırsınız ve Veri Gölü Depolama Gen1 hesap sağlamanızın ilerlemesini görebilirsiniz. Data Lake Storage Gen1 hesabı sağlandıktan sonra hesap bıçağı ortaya çıkar.

## <a name="assign-permissions-to-azure-key-vault"></a><a name="assign-permissions-to-azure-key-vault"></a>Azure Key Vault’a izin atama

Veri Gölü Depolama Gen1 hesabında şifreleme yapılandırmak için Bir Azure Anahtar Kasası'nın anahtarlarını kullandıysanız, Veri Gölü Depolama Gen1 hesabı ile Azure Anahtar Kasası hesabı arasındaki erişimi yapılandırmanız gerekir. Bunu yapmak için aşağıdaki adımları gerçekleştirin.

1. Azure Anahtar Kasası'ndaki anahtarları kullandıysanız, Veri Gölü Depolama Gen1 hesabının bıçağı üstte bir uyarı görüntüler. Uyarıya tıklayarak **Şifreleme** sayfasını açın.

    ![Veri Gölü Depolama Gen1 şifrelemesi](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Veri Gölü Depolama Gen1 şifrelemesi")
2. Bu dikey pencere, erişimi yapılandırmak için iki seçenek gösterir.

    ![Veri Gölü Depolama Gen1 şifrelemesi](./media/data-lake-store-get-started-portal/adls-encryption-4.png "Veri Gölü Depolama Gen1 şifrelemesi")

   * Erişim yapılandırmak için ilk seçenekte **İzin Ver**'e tıklayın. İlk seçenek, yalnızca Veri Gölü Depolama Gen1 hesabını oluşturan kullanıcı da Azure Anahtar Kasası için bir yönetici olduğunda etkinleştirilir.
   * Diğer seçenek, dikey pencerede görüntülenen PowerShell cmdlet’ini çalıştırmak içindir. Azure Anahtar Kasası’nın sahibi olmanız ya da Azure Anahtar Kasası’nda izin verme yetkisine sahip olmanız gerekir. Cmdlet’i çalıştırdıktan sonra, erişimi yapılandırmak için dikey pencereye dönün ve **Etkinleştir**’e tıklayın.

> [!NOTE]
> Azure Kaynak Yöneticisi şablonlarını kullanarak bir Veri Gölü Depolama Gen1 hesabı da oluşturabilirsiniz. Bu şablonlara [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/?term=data+lake+store)’ndan erişebilirsiniz:
> * Veri şifrelemesi olmadan: [Veri şifrelemesi olmadan Azure Veri Gölü Depolama Gen1 hesabını dağıtın.](https://azure.microsoft.com/resources/templates/101-data-lake-store-no-encryption/)
> * Veri Gölü Depolama Gen1 kullanarak veri şifreleme ile: [Şifreleme (Veri Gölü) ile Veri Gölü Depolama Gen1 hesabı dağıtın.](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/)
> * Azure Key Vault kullanarak veri şifreleme ile: [Şifreleme (Key Vault) ile Veri Gölü Depolama Gen1 hesabı dağıtın.](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/)
>
>

## <a name="create-folders"></a><a name="createfolder"></a>Klasör oluşturma

Verileri yönetmek ve depolamak için Veri Gölü Depolama Gen1 hesabınızın altında klasörler oluşturabilirsiniz.

1. Oluşturduğunuz Veri Gölü Depolama Gen1 hesabını açın. Sol bölmeden Tüm **kaynaklar'ı**tıklatın ve ardından **Tüm kaynaklar** çubuğundan klasör oluşturmak istediğiniz hesap adını tıklatın. Hesabı başlangıç panosuna sabitlediyseniz bu hesap kutucuğuna tıklayın.
2. Veri Gölü Depolama Gen1 hesap bıçak, **Veri Gezgini'ni**tıklatın.

    ![Veri Gölü Depolama Gen1 hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Veri Gölü Depolama Gen1 hesabında klasör oluşturma")
3. Veri Gezgini dikey penceresinde, **Yeni Klasör**'e tıklayın, yeni klasör için bir ad girin ve ardından **Tamam**'a tıklayın.

    ![Veri Gölü Depolama Gen1 hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Veri Gölü Depolama Gen1 hesabında klasör oluşturma")

    Yeni oluşturulan klasör, **Veri Gezgini** dikey penceresinde listelenir. Herhangi bir düzeye kadar iç içe klasörler oluşturabilirsiniz.

    ![Veri Gölü hesabında klasör oluşturma](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Veri Gölü hesabında klasör oluşturma")

## <a name="upload-data"></a><a name="uploaddata"></a>Karşıya veri yükleme

Verilerinizi doğrudan kök düzeyindeki bir Veri Gölü Depolama Gen1 hesabına veya hesap içinde oluşturduğunuz bir klasöre yükleyebilirsiniz.

1. **Veri Gezgini** dikey penceresinde **Yükle**'ye tıklayın.
2. **Dosyaları karşıya yükleme** dikey penceresinde yüklemek istediğiniz dosyaları bulun ve **Seçili dosyaları ekle**'ye tıklayın. Yüklemek için birden fazla dosya da seçebilirsiniz.

    ![Karşıya veri yükleme](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Karşıya veri yükleme")

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

## <a name="actions-available-on-the-stored-data"></a><a name="properties"></a>Depolanan verilerde kullanılabilen eylemler

Dosyanın yanındaki üç nokta simgesine tıklayın ve açılan menüden verilerle ilgili gerçekleştirmek istediğiniz eyleme tıklayın.

![Verilere yönelik özellikler](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Verilere yönelik özellikler")

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama

Azure Active Directory ve erişim denetimi (AçS) kullanarak Veri Gölü Depolama Gen1 hesabınızda depolanan verileri güvenli hale getirebilirsiniz. Bunun nasıl yapılacağını gösteren talimatlar için [bkz.](data-lake-store-secure-data.md)

## <a name="delete-your-account"></a>Hesabınızı silin

Veri Gölü Depolama Gen1 hesabınızdan bir Veri Gölü Depolama Gen1 hesabını silmek için **Sil'i**tıklatın. Eylemi onaylamak için silmek istediğiniz hesabın adını girmeniz istenir. Hesabın adını girin ve ardından **Sil**'e tıklayın.

![Veri Gölü Depolama Gen1 hesabını silme](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Data Lake hesabını silme")

## <a name="next-steps"></a>Sonraki adımlar

* [Büyük veri gereksinimleri için Azure Veri Gölü Depolama Gen1'i kullanma](data-lake-store-data-scenarios.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
