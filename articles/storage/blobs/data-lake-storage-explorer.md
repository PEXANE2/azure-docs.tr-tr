---
title: Azure Data Lake Storage 2. ile Azure Depolama Gezgini kullanma
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Azure Depolama Gezgini kullanın.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 02/05/2021
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 5ccef241a37e63467b681d5fd12c65072cb92e58
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99626496"
---
# <a name="use-azure-storage-explorer-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. Nesil’de dizinleri, dosyaları ve ACL’leri yönetmek için Azure Depolama Gezgini’ni kullanma

Bu makalede, hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında Dizin, dosya ve erişim denetimi listeleri (ACL 'Ler) oluşturmak ve yönetmek için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

- Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](../common/storage-account-create.md) yönergeleri izleyin.

- Yerel bilgisayarınızda yüklü Azure Depolama Gezgini. Windows, Macintosh veya Linux işletim sisteminde Azure Depolama Gezgini’ni yüklemek için bkz. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

## <a name="sign-in-to-storage-explorer"></a>Depolama Gezgini oturum açın

Depolama Gezgini’ni ilk kez başlattığınızda **Microsoft Azure Depolama Gezgini - Bağlan** penceresi görüntülenir. Depolama Gezgini depolama hesaplarına bağlanmak için çeşitli yollar sağladığından, ACL 'Leri yönetmek için şu anda yalnızca bir yol desteklenir.

|Görev|Amaç|
|---|---|
|Azure Hesabı ekleme | Sizi Azure 'da kimlik doğrulaması yapmak için kuruluşunuzun oturum açma sayfasına yönlendirir. ACL 'Leri yönetmek ve ayarlamak istiyorsanız şu anda desteklenen tek kimlik doğrulama yöntemidir.|
|Bağlantı dizesi veya paylaşılan erişim imzası URI'si kullanma | SAS belirteci veya paylaşılan bağlantı dizesiyle doğrudan bir kapsayıcıya veya depolama hesabına erişmek için kullanılabilir. |
|Depolama hesabı adını ve anahtarını kullanma| Azure depolama alanına bağlanmak için depolama hesabı adını ve anahtarını kullanın.|

**Azure hesabı ekle** ' yi seçin ve **oturum aç**' a tıklayın... Azure hesabınızda oturum açmak için ekrandaki istemleri izleyin.

![Microsoft Azure Depolama Gezgini gösteren ekran görüntüsü ve Azure hesabı ekle seçeneğini ve oturum aç düğmesini vurgular.](media/storage-quickstart-blobs-storage-explorer/connect.png)

Bağlantı kurulduğunda Azure Depolama Gezgini yüklenir ve **Gezgin** sekmesi gösterilir. Bu görünüm, tüm Azure depolama hesaplarınızın yanı sıra [Azurıite depolama öykünücüsü](../common/storage-use-azurite.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), [Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) hesapları veya [Azure Stack](/azure-stack/user/azure-stack-storage-connect-se?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ortamları aracılığıyla yapılandırılan yerel depolama hakkında bilgi verir.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="create-a-container"></a>Kapsayıcı oluşturma

Kapsayıcı dizinleri ve dosyaları barındırır. Bir tane oluşturmak için, devam adımında oluşturduğunuz depolama hesabını genişletin. **BLOB kapsayıcıları**' nı seçin, sağ tıklayın ve **BLOB kapsayıcısı oluştur**' u seçin. Kapsayıcının adını girin. Adlandırma kapsayıcılarındaki kuralların ve kısıtlamaların listesi için [kapsayıcı oluşturma](storage-quickstart-blobs-dotnet.md#create-a-container) bölümüne bakın. Tamamlandığında, kapsayıcıyı oluşturmak için **ENTER** tuşuna basın. Kapsayıcı başarıyla oluşturulduktan sonra, seçili depolama hesabı için **BLOB kapsayıcıları** klasörü altında görüntülenir.

![Microsoft Azure Depolama Gezgini-kapsayıcı oluşturma](media/data-lake-storage-explorer/creating-a-filesystem.png)

## <a name="create-a-directory"></a>Dizin oluşturma

Bir dizin oluşturmak için, devam adımında oluşturduğunuz kapsayıcıyı seçin. Kapsayıcı şeridinde **Yeni klasör** düğmesini seçin. Dizininizin adını girin. Tamamlandığında, dizini oluşturmak için **ENTER** tuşuna basın. Dizin başarıyla oluşturulduktan sonra düzenleyici penceresinde görünür.

![Microsoft Azure Depolama Gezgini-dizin oluşturma](media/data-lake-storage-explorer/creating-a-directory.png)

## <a name="upload-blobs-to-the-directory"></a>Blob 'ları dizine yükleme

Dizin şeridinde **karşıya yükle** düğmesini seçin. Bu işlemi kullanarak klasör veya dosya yükleyebilirsiniz.

Yüklenecek dosyaları veya klasörü seçin.

![Microsoft Azure Depolama Gezgini - blob yükleme](media/data-lake-storage-explorer/upload-file.png)

**Tamam**'ı seçtiğinizde dosyalar yüklenmek üzere kuyruğa alınır ve tüm dosyalar yüklenir. Yükleme işlemi tamamlandığında sonuçlar **Etkinlikler** penceresinde gösterilir.

## <a name="view-blobs-in-a-directory"></a>Blob 'ları bir dizinde görüntüleme

**Azure Depolama Gezgini** uygulamasında, depolama hesabı altında bir dizin seçin. Ana bölmede, seçili dizindeki Blobların bir listesi gösterilir.

![Bir dizindeki Microsoft Azure Depolama Gezgini Blobları listeleme](media/data-lake-storage-explorer/list-files.png)

## <a name="download-blobs"></a>Blob’ları indirme

**Azure Depolama Gezgini** kullanarak dosyaları indirmek için, bir dosya seçiliyken şeritten **İndir** ' i seçin. Açılan dosya iletişim kutusuna dosya adı girebilirsiniz. Yerel konuma bir dosyanın indirilmesini başlatmak için **Kaydet** ' i seçin.

<a id="managing-access"></a>

## <a name="manage-acls"></a>ACL 'Leri yönetme

Kapsayıcıya, dizine veya dosyaya sağ tıklayın ve ardından **Access Control listelerini yönet**' e tıklayın.  Aşağıdaki ekran görüntüsünde, bir dizine sağ tıkladığınızda görüntülenen menü gösterilmektedir.

> [!div class="mx-imgBorder"]
> ![Azure Depolama Gezgini bir dizine sağ tıklayın](./media/data-lake-storage-explorer/manage-access-control-list-option.png)

**Erişimi yönet** iletişim kutusu, sahip ve sahipler grubu için izinleri yönetmenizi sağlar. Ayrıca, izinleri yönetebilmeniz için erişim denetim listesine yeni kullanıcılar ve gruplar eklemenize olanak tanır.

> [!div class="mx-imgBorder"]
> ![Erişimi Yönet iletişim kutusu](./media/data-lake-storage-explorer/manage-access-dialog-box.png)

Erişim denetim listesine yeni bir kullanıcı veya grup eklemek için **Ekle** düğmesini seçin. Ardından, listeye eklemek istediğiniz karşılık gelen Azure Active Directory (AAD) girişini girin ve **Ekle**' yi seçin.  Kullanıcı veya grup artık **Kullanıcılar ve gruplar:** alanında görünür ve bu da izinlerini yönetmeye başlamanızı sağlar.

> [!NOTE]
> AAD 'de bir güvenlik grubu oluşturmak ve tek kullanıcılar yerine grupta izinleri sürdürmek için en iyi uygulamadır ve önerilir. Bu öneriye ilişkin ayrıntılar ve diğer en iyi yöntemler için [Azure Data Lake Storage 2. ' de erişim denetimi modeli](data-lake-storage-access-control-model.md)' ne bakın.

Erişim ve varsayılan ACL 'Leri ayarlamak için onay kutusu denetimlerini kullanın. Bu ACL türleri arasındaki fark hakkında daha fazla bilgi edinmek için bkz. [ACL türleri](data-lake-storage-access-control.md#types-of-acls).

<a id="apply-acls-recursively"></a>

## <a name="apply-acls-recursively"></a>ACL 'Leri yinelemeli olarak Uygula

Bu değişiklikleri her alt öğe için ayrı ayrı yapmak zorunda kalmadan, ACL girişlerini bir üst dizinin varolan alt öğelerine yinelemeli olarak uygulayabilirsiniz.

ACL girdilerini yinelemeli olarak uygulamak için kapsayıcıya veya bir dizine sağ tıklayın ve **Access Control listeleri yay**' e tıklayın.  Aşağıdaki ekran görüntüsünde, bir dizine sağ tıkladığınızda görüntülenen menü gösterilmektedir.

> [!div class="mx-imgBorder"]
> ![Dizine sağ tıklayıp erişimi yay denetim ayarını seçme](./media/data-lake-storage-explorer/propagate-access-control-list-option.png)

## <a name="next-steps"></a>Sonraki adımlar

Data Lake Storage 2. erişim denetim listelerine öğrenin.

> [!div class="nextstepaction"]
> [Azure Data Lake Storage 2. Nesil'de Erişim Denetimi](./data-lake-storage-access-control.md)
