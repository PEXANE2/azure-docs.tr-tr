---
title: Azure Veri Gölü Depolama Gen2 ile Azure Depolama Gezgini'ni kullanma
description: Hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında dizinleri ve dosya ve dizin erişim denetim listelerini (ACL) yönetmek için Azure Depolama Gezgini'ni kullanın.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: fca9fa8a964c6c9d69ffbb3036bd4774e0d1bd34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255554"
---
# <a name="use-azure-storage-explorer-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Azure Depolama Gezgini'ni kullanın

Bu makalede, hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında dizinler, dosyalar ve izinler oluşturmak ve yönetmek için [Azure Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/) nasıl kullanacağınızı gösterir.

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkinleştirilmiş bir depolama hesabı. Oluşturmak için [bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * Azure Depolama Gezgini yerel bilgisayarınızda yüklendi. Windows, Macintosh veya Linux işletim sisteminde Azure Depolama Gezgini’ni yüklemek için bkz. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

## <a name="sign-in-to-storage-explorer"></a>Depolama Gezgini'nde oturum açın

Depolama Gezgini’ni ilk kez başlattığınızda **Microsoft Azure Depolama Gezgini - Bağlan** penceresi görüntülenir. Depolama Gezgini depolama hesaplarına bağlanmak için çeşitli yollar sağlarken, aCD'leri yönetmek için şu anda yalnızca bir yol desteklenir.

|Görev|Amaç|
|---|---|
|Azure Hesabı ekleme | Kimliğinizi Azure'a yönlendirmek için sizi kuruluşunuzun oturum açma sayfasına yönlendirir. Şu anda bu, ALA'ları yönetmek ve ayarlamak istiyorsanız desteklenen tek kimlik doğrulama yöntemidir.|
|Bağlantı dizesi veya paylaşılan erişim imzası URI'si kullanma | SAS belirteci veya paylaşılan bağlantı dizesiyle doğrudan bir kapsayıcıya veya depolama hesabına erişmek için kullanılabilir. |
|Depolama hesabı adını ve anahtarını kullanma| Azure depolama alanına bağlanmak için depolama hesabı adını ve anahtarını kullanın.|

**Azure Hesabı Ekle'yi** seçin ve **Oturum Aç'ı tıklatın...** Azure hesabınızda oturum açma nız için ekrandaki istemleri izleyin.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/connect.png)

Bağlantı kurulduğunda Azure Depolama Gezgini yüklenir ve **Gezgin** sekmesi gösterilir. Bu görünümde tüm Azure depolama hesaplarınıza ek olarak [Azure Depolama Öykünücüsü](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), [Cosmos DB](../../cosmos-db/storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) hesapları veya [Azure Stack](/azure-stack/user/azure-stack-storage-connect-se?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ortamları üzerinden yapılandırılan yerel depolama alanlarını görebilirsiniz.

![Microsoft Azure Depolama Gezgini - Bağlan penceresi](media/storage-quickstart-blobs-storage-explorer/mainpage.png)

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir kapsayıcı dizinleri ve dosyaları tutar. Bir tane oluşturmak için, devam aşamasında oluşturduğunuz depolama hesabını genişletin. **Blob Kapsayıcıları**'nı ve ardından **Blob Kapsayıcısı Oluştur**'u seçin. Kapsayıcınızın adını girin. Kapsayıcıları adlandırma yla ilgili kurallar ve kısıtlamalar listesi için [kapsayıcı oluştur](storage-quickstart-blobs-dotnet.md#create-a-container) bölümüne bakın. Tamamlandığında, kapsayıcıyı oluşturmak için **Enter** tuşuna basın. Kapsayıcı başarıyla oluşturulduktan sonra, seçili depolama hesabı için **Blob Kapsayıcılar** klasörü altında görüntülenir.

![Microsoft Azure Depolama Gezgini - Kapsayıcı oluşturma](media/data-lake-storage-explorer/creating-a-filesystem.png)

## <a name="create-a-directory"></a>Dizin oluşturma

Bir dizin oluşturmak için, devam adımda oluşturduğunuz kapsayıcıyı seçin. Kapsayıcı şeridinde Yeni **Klasör** düğmesini seçin. Dizininizin adını girin. Tamamlandığında, dizin oluşturmak için **Enter** tuşuna basın. Dizin başarıyla oluşturulduktan sonra düzenleyici penceresinde görünür.

![Microsoft Azure Depolama Gezgini - Dizin oluşturma](media/data-lake-storage-explorer/creating-a-directory.png)

## <a name="upload-blobs-to-the-directory"></a>Dizine blob'lar yükleyin

Dizin şeridinde **Yükle** düğmesini seçti. Bu işlemi kullanarak klasör veya dosya yükleyebilirsiniz.

Yüklenecek dosyaları veya klasörü seçin.

![Microsoft Azure Depolama Gezgini - blob yükleme](media/data-lake-storage-explorer/upload-file.png)

**Tamam**'ı seçtiğinizde dosyalar yüklenmek üzere kuyruğa alınır ve tüm dosyalar yüklenir. Yükleme işlemi tamamlandığında sonuçlar **Etkinlikler** penceresinde gösterilir.

## <a name="view-blobs-in-a-directory"></a>Dizindeki lekeleri görüntüleme

Azure **Depolama Gezgini** uygulamasında, depolama hesabı nın altında bir dizini seçin. Ana bölme, seçili dizindeki lekelerin listesini gösterir.

![Microsoft Azure Depolama Gezgini - dizindeki lekeleri listele](media/data-lake-storage-explorer/list-files.png)

## <a name="download-blobs"></a>Blob’ları indirme

Seçili bir dosyayla **Azure Depolama Gezgini'ni**kullanarak dosyaları indirmek için şeritten **İndir'i** seçin. Açılan dosya iletişim kutusuna dosya adı girebilirsiniz. Dosyanın yerel konuma indirilmeye başlını vermek için **Kaydet'i** seçin.

## <a name="managing-access"></a>Erişimi yönetme

İzinleri kapsayıcınızın kökünde ayarlayabilirsiniz. Bunu yapmak için, azure depolama explorer'ına bireysel hesabınızla giriş yapmış olmanız ve bunu yapma haklarınızın (bağlantı dizesiyle değil) oturum açmış olmanız gerekir. Kapsayıcınıza sağ tıklayın ve **İzni Yönet** iletişim kutusunu açarak **İzinleri Yönet'i**seçin.

![Microsoft Azure Depolama Gezgini - Dizin erişimini yönet](media/storage-quickstart-blobs-storage-Explorer/manageperms.png)

**İzni Yönet** iletişim kutusu, sahibi ve sahipleri grubu için izinleri yönetmenize olanak tanır. Ayrıca, izinleri yönetebileceğiniz erişim denetim listesine yeni kullanıcılar ve gruplar eklemenize de olanak tanır.

Erişim denetim listesine yeni bir kullanıcı veya grup eklemek için **kullanıcı ekle veya grup** alanını seçin.

Listeye eklemek istediğiniz ilgili Azure Etkin Dizin (AAD) girişini girin ve ardından **Ekle'yi**seçin.

Kullanıcı veya grup artık **Kullanıcılar ve gruplarda görünür:** alan, onların izinlerini yönetmeye başlamak için izinleri.

> [!NOTE]
> AAD'de bir güvenlik grubu oluşturmak ve tek tek kullanıcılar yerine grup üzerindeki izinleri korumak en iyi yöntemdir ve önerilir. Bu öneri yle ilgili ayrıntılar ve diğer en iyi uygulamalar [için Veri Gölü Depolama Gen2 için en iyi uygulamalara](data-lake-storage-best-practices.md)bakın.

Atayabileceğiniz iki izin kategorisi vardır: ALA'lara ve varsayılan ALA'lara erişin.

* **Erişim**: Access AcLs bir nesneye erişimi denetler. Dosyaların ve dizinlerin her ikisi de erişim ALA'larına sahiptir.

* **Varsayılan**: Bu dizin altında oluşturulan alt öğeleriçin erişim ALA'larını belirleyen bir dizinle ilişkili AA şablonu. Dosyalarda varsayılan ALA'lar yoktur.

Bu kategorilerin her ikisi içinde de, daha sonra dosyalara veya dizinlere atayabileceğiniz üç izin vardır: **Oku,** **Yaz**ve **Yürüt.**

>[!NOTE]
> Burada seçim yapmak, dizinin içindeki varolan herhangi bir öğeiçin izin ler belirlemez. Dosya zaten varsa, her bir öğeye gidip izinleri el ile ayarlamanız gerekir.

Tek tek dizinlerin yanı sıra, ince taneli erişim denetimi sağlayan tek tek dosyalardaki izinleri yönetebilirsiniz. Hem dizinler hem de dosyalar için izinleri yönetme işlemi yukarıda açıklandığı gibi aynıdır. İzinleri yönetmek istediğiniz dosyaya veya dizine sağ tıklayın ve aynı işlemi izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Data Lake Storage Gen2'deki erişim denetim listelerini öğrenin.

> [!div class="nextstepaction"]
> [Azure Veri Gölü Depolama Gen2'de erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)
