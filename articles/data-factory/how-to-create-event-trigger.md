---
title: Azure Veri Fabrikası'nda olay tabanlı tetikleyiciler oluşturma
description: Azure Veri Fabrikası'nda bir olaya yanıt olarak bir ardışık hattı çalıştıran bir tetikleyiciyi nasıl oluşturabilirsiniz öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 10/18/2018
ms.openlocfilehash: d697fb8afe3e92dfe54eb5d89a2ef59425cb0cde
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414924"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Bir olaya yanıt olarak bir ardışık hatlar hattını çalıştıran bir tetikleyici oluşturma
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Veri Fabrikası ardışık hatlarınızda oluşturabileceğiniz olay tabanlı tetikleyiciler açıklanmaktadır.

Olay odaklı mimari (EDA), üretim, algılama, tüketim ve olaylara tepki içeren ortak bir veri tümleştirme desenidir. Veri tümleştirme senaryoları genellikle Veri Fabrikası müşterilerinin Azure Depolama hesabınızda bir dosyanın gelişi veya silinmesi gibi olaylara bağlı olarak ardışık hatları tetiklemesini gerektirir. Veri Fabrikası artık [Azure Olay Ağıla](https://azure.microsoft.com/services/event-grid/)entegre edilmiştir ve bu da bir etkinlikte ardışık hatları tetiklemenize olanak tanır.

Bu özelliğin on dakikalık tanıtımı ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> Bu makalede açıklanan tümleştirme [Azure Olay Idamı'na](https://azure.microsoft.com/services/event-grid/)bağlıdır. Aboneliğinizin Olay Izgara kaynak sağlayıcısına kayıtlı olduğundan emin olun. Daha fazla bilgi için [Kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal)bkz.

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

Bu bölümde, Azure Veri Fabrikası Kullanıcı Arabirimi içinde nasıl olay tetikleyicisi oluşturulabileceğiniz gösterilmektedir.

1. **Yazarkan Tuvaline** Git

1. Sol alt **köşede, Tetikleyiciler** düğmesine tıklayın

1. Create trigger side nav'ı açacak **+ Yeni'yi** tıklatın

1. Tetikleyici türü **Olay'ı** seçin

    ![Yeni olay tetikleyicisi oluşturma](media/how-to-create-event-trigger/event-based-trigger-image1.png)

1. Azure abonelik açılır tarihinden veya Depolama hesabı kaynak kimliğini el ile kullanarak depolama hesabınızı seçin. Olayların hangi kapsayıcıda gerçekleşmesini istediğinizi seçin. Konteyner seçimi isteğe bağlıdır, ancak tüm kapsayıcıları seçmenin çok sayıda olaya yol açabileceğini dikkatli olun.

   > [!NOTE]
   > Olay Tetikleyicisi şu anda yalnızca Azure Veri Gölü Depolama Gen2 ve Genel amaçlı sürüm 2 depolama hesaplarını destekler. Azure Olay Ağı sınırlaması nedeniyle, Azure Veri Fabrikası depolama hesabı başına en fazla 500 olay tetikleyicisini destekler.

1. **Blob yolu ile başlar** ve **Blob yolu** özellikleri ile sona erer olayları almak istediğiniz kapsayıcılar, klasörler ve blob adlarını belirtmenize olanak sağlar. Olay tetikleyiciniz, bu özelliklerden en az birinin tanımlanmasını gerektirir. Bu makalede daha sonra örneklerde gösterildiği gibi, hem **Blob yolu ile başlar** hem de **Blob yolu** özellikleri ile sona erer için desen çeşitliliği kullanabilirsiniz.

    * **Blob yolu ile başlar:** Blob yolu bir klasör yolu ile başlamalıdır. Geçerli değerler `2018/` `2018/april/shoes.csv`içerir ve . Bir kapsayıcı seçili değilse, bu alan seçilemiyor.
    * **Blob yolu ile sona erer:** Blob yolu bir dosya adı veya uzantısı ile sona ermelidir. Geçerli değerler `shoes.csv` `.csv`içerir ve . Kapsayıcı ve klasör adı isteğe bağlıdır, ancak belirtildiğinde, bir `/blobs/` segment tarafından ayrılması gerekir. Örneğin, 'siparişler' adlı bir kapsayıcının `/orders/blobs/2018/april/shoes.csv`değeri. Herhangi bir kapsayıcıda bir klasör belirtmek için, önde gelen '/' karakterini atlayın. Örneğin, `april/shoes.csv` herhangi bir kapsayıcıda 'april' adlı klasörde adı geçen `shoes.csv` herhangi bir dosyada bir olay tetikler. 

1. Tetikleyicinizin **Blob tarafından oluşturulan** bir olaya, **Blob silinmiş** olaya mı yoksa her ikisine mi yanıt vereceğini seçin. Belirtilen depolama konumunuzda, her olay tetikleyiciyle ilişkili Veri Fabrikası ardışık hatlarını tetikler.

    ![Olay tetikleyicisini yapılandırma](media/how-to-create-event-trigger/event-based-trigger-image2.png)

1. Tetikleyicinizin sıfır baytlı lekeleri yok sayıp yoksaymayacağını seçin.

1. Tetikleyicinizi yapılandırıldıktan sonra **İleri: Veri önizlemesini**tıklatın. Bu ekran, olay tetikleyici yapılandırmanızla eşleşen varolan lekeleri gösterir. Belirli filtrelerin olduğundan emin olun. Çok geniş olan filtreleri yapılandırmak, oluşturulan/silinen çok sayıda dosyayla eşleşebilir ve maliyetinizi önemli ölçüde etkileyebilir. Filtre koşullarınız doğrulandıktan sonra **Finish'i**tıklatın.

    ![Olay tetikleyici veri önizleme](media/how-to-create-event-trigger/event-based-trigger-image3.png)

1. Bu tetikleyiciye bir ardışık yol hattı eklemek için, ardışık hatlar tuvaline gidin ve **tetikleyici ekle'yi** tıklatın ve **Yeni/Edit'i**seçin. Yan gezinme göründüğünde, **Tetikle...** açılır seçeneğini tıklatın ve oluşturduğunuz tetikleyiciyi seçin. **Sonraki'yi tıklatın:** Yapılandırmanın doğru olduğunu doğrulamak için veri önizlemesi ve ardından Veri önizlemesini doğrulamak için **Sonraki** doğru.

1. Ardışık alanınızın parametreleri varsa, bunları tetikleme parametre yan navigasyonu üzerinde belirtebilirsiniz. Olay tetikleyici özellikleri `@triggerBody().folderPath` ve blob klasör yolu ve `@triggerBody().fileName`dosya adını yakalar. Bu özelliklerin değerlerini bir ardışık dizinde kullanmak için, özellikleri boru hattı parametreleri ile eşlemelisiniz. Özellikleri parametrelerle eşledikten sonra, ardışık yol boyunca `@pipeline().parameters.parameterName` ifade aracılığıyla tetikleyici tarafından yakalanan değerlere erişebilirsiniz. Bittikten sonra **Bitir'i** tıklatın.

    ![Özellikleri boru hattı parametrelerine eşleme](media/how-to-create-event-trigger/event-based-trigger-image4.png)

Önceki örnekte, kapsayıcı örnek verilerinde .csv ile biten bir blob yolu oluşturulduğunda tetikleyici ateş edilebiyi ayarlayarak ateşlenir. **FolderPath** ve **fileName** özellikleri yeni blob'un konumunu yakalar. Örneğin, MoviesDB.csv yol örnek veri/olay testi ne zaman `@triggerBody().folderPath` eklenir, `sample-data/event-testing` bir `@triggerBody().fileName` değeri vardır `moviesDB.csv`ve bir değeri vardır . Bu değerler örnekte boru hattı `sourceFolder` parametrelerine `sourceFile` eşlenir ve boru hattı `@pipeline().parameters.sourceFolder` `@pipeline().parameters.sourceFile` boyunca sırasıyla ve sırasıyla kullanılabilir.

## <a name="json-schema"></a>JSON şema

Aşağıdaki tablo, olay tabanlı tetikleyicilerle ilgili şema öğelerine genel bir bakış sağlar:

| **JSON Elementi** | **Açıklama** | **Tür** | **İzin Verilen Değerler** | **Gerekli** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **Kapsam** | Depolama Hesabı'nın Azure Kaynak Yöneticisi kaynak kimliği. | Dize | Azure Kaynak Yöneticisi Kimliği | Evet |
| **Olay** | Bu tetikleyicinin ateş lesonuç açmasına neden olan olaylar türü. | Dizi    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Evet, bu değerlerin herhangi bir kombinasyonu. |
| **blobPathBeginsWith** | Blob yolu, tetikleyicinin ateşlemesi için sağlanan desenle başlamalıdır. Örneğin, `/records/blobs/december/` yalnızca kapsayıcının altındaki `december` klasördeki lekeler için `records` tetikleyiciyi tetikler. | Dize   | | Bu özelliklerden en az biri için bir `blobPathBeginsWith` `blobPathEndsWith`değer sağlamanız gerekir: veya . |
| **blobPathEndsWith** | Blob yolu, tetikleyicinin ateşlemesi için sağlanan desenle sona ermelidir. Örneğin, `december/boxes.csv` yalnızca bir `boxes` `december` klasörde adı verilen lekeler için tetikleyiciyi tetikler. | Dize   | | Bu özelliklerden en az biri için bir `blobPathBeginsWith` `blobPathEndsWith`değer sağlamanız gerekir: veya . |
| **yoksayBoşBlobs** | Sıfır bayt lekeleri bir boru hattı çalışmasını tetikleyecek olsun ya da olmasın. Varsayılan olarak, bu doğru olarak ayarlanır. | Boole | true veya false | Hayır |

## <a name="examples-of-event-based-triggers"></a>Olay tabanlı tetikleyiciörnekleri

Bu bölümde olay tabanlı tetikleyici ayarları örnekleri verilmektedir.

> [!IMPORTANT]
> Kapsayıcı ve klasör, kapsayıcı ve dosya veya kapsayıcı, klasör ve dosya belirttiğiniz de, aşağıdaki örneklerde gösterildiği gibi, yolun `/blobs/` kesimini eklemeniz gerekir. **blobPathBeginsWith**için, Veri Fabrikası UI `/blobs/` otomatik olarak tetikleyici JSON klasör ve kapsayıcı adı arasında ekler.

| Özellik | Örnek | Açıklama |
|---|---|---|
| **Blob yolu ile başlar** | `/containername/` | Kapsayıcıdaki herhangi bir leke için olayları alır. |
| **Blob yolu ile başlar** | `/containername/blobs/foldername/` | `containername` Kapsayıcı ve `foldername` klasördeki tüm lekeler için olayları alır. |
| **Blob yolu ile başlar** | `/containername/blobs/foldername/subfoldername/` | Bir alt klasöre de başvuruda bulunabilirsiniz. |
| **Blob yolu ile başlar** | `/containername/blobs/foldername/file.txt` | Kapsayıcının `file.txt` `foldername` altındaki klasörde adı geçen bir blob için olayları alır. `containername` |
| **Blob yolu ile biter** | `file.txt` | Herhangi bir yolda adlı `file.txt` bir blob için olayları alır. |
| **Blob yolu ile biter** | `/containername/blobs/file.txt` | Kapsayıcı `file.txt` `containername`altında adlı bir blob için olayları alır. |
| **Blob yolu ile biter** | `foldername/file.txt` | Herhangi bir kapsayıcının `file.txt` altındaki `foldername` klasörde adı geçen bir blob için olayları alır. |

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi [için, Bkz. Boru Hattı yürütme ve tetikleyiciler.](concepts-pipeline-execution-triggers.md#trigger-execution)
