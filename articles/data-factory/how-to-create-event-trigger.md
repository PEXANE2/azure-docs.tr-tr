---
title: Azure Data Factory olay tabanlı Tetikleyiciler oluşturma
description: Bir olaya yanıt olarak bir işlem hattı çalıştıran Azure Data Factory tetikleyici oluşturmayı öğrenin.
ms.service: data-factory
author: chez-charlie
ms.author: chez
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 10/18/2018
ms.openlocfilehash: 7dde05e02421ef8d2ea46fd0d50687ede6e5d884
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101727794"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-a-storage-event"></a>Bir depolama olayına yanıt olarak bir işlem hattı çalıştıran bir tetikleyici oluşturma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Data Factory işlem hatlarında oluşturabileceğiniz depolama olay tetikleyicileri açıklanmaktadır.

Olay odaklı mimari (EDA), olaylar için üretim, algılama, tüketim ve yeniden eyleme sahip ortak bir veri tümleştirme modelidir. Veri tümleştirme senaryoları genellikle Data Factory müşterilerin Azure Blob depolama hesabındaki bir dosyayı alma veya silme gibi, depolama hesabında oluşan olaylara göre işlem hatlarını tetiklemesine gerek duyar. Data Factory, bu olaylar üzerinde işlem hatlarını tetiklemenizi sağlayan [Azure Event Grid](https://azure.microsoft.com/services/event-grid/)yerel olarak tümleştirilir.

Bu özelliğin on dakikalık bir giriş ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> Bu makalede açıklanan tümleştirme [Azure Event Grid](https://azure.microsoft.com/services/event-grid/)bağımlıdır. Aboneliğinizin Event Grid kaynak sağlayıcısına kayıtlı olduğundan emin olun. Daha fazla bilgi için bkz. [kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). *Microsoft. EventGrid/Eventabonelikleri/** eylemini yapabilmelisiniz. Bu eylem EventGrid EventSubscription katılımcısı yerleşik rolünün bir parçasıdır.

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

Bu bölümde, Azure Data Factory Kullanıcı arabirimi içinde bir depolama olay tetikleyicisi oluşturma gösterilmektedir.

1. **Yazma tuvaline** git

1. Sol alt köşede **Tetikleyiciler** düğmesine tıklayın

1. Tetikleyici oluşturma tarafı gezintisi ' ni açacak **+ Yeni** ' ye tıklayın

1. Tetikleyici türü **depolama olayı** seçin

    ![Yeni depolama olayı tetikleyicisi oluştur](media/how-to-create-event-trigger/event-based-trigger-image1.png)

1. Azure aboneliği açılan listesinden veya depolama hesabı kaynak KIMLIĞINI kullanarak el ile depolama hesabınızı seçin. Olayların gerçekleşmesini istediğiniz kapsayıcıyı seçin. Kapsayıcı seçimi isteğe bağlıdır, ancak tüm kapsayıcıları seçtiğinizde çok sayıda olaya yol açabilir.

   > [!NOTE]
   > Depolama olayı tetikleyicisi Şu anda yalnızca Azure Data Lake Storage 2. ve genel amaçlı sürüm 2 depolama hesaplarını desteklemektedir. Azure Event Grid sınırlaması nedeniyle, Azure Data Factory yalnızca depolama hesabı başına en fazla 500 depolama olay tetikleyicisi destekler.

   > [!NOTE]
   > Yeni bir depolama olay tetikleyicisi oluşturmak ve değiştirmek için, Data Factory oturum açmak için kullanılan Azure hesabının depolama hesabında en az *sahip* iznine sahip olması gerekir. Ek izin gerekmez: Azure Data Factory için hizmet sorumlusu, depolama hesabı veya Event Grid için özel _izin gerektirmez._

1. **BLOB yolu ile başlar** ve **BLOB yolu, özellikler ile biter** ve olayları almak istediğiniz kapsayıcıları, klasörleri ve BLOB adlarını belirtmenize olanak tanır. Depolama olay Tetikleyiciniz, bu özelliklerden en az birinin tanımlanmasını gerektirir. Her iki **BLOB yolu ile başlar** ve **BLOB yolu** , bu makalenin ilerleyen kısımlarında gösterildiği gibi özellikleriyle biter.

    * **BLOB yolu şununla başlar:** Blob yolu bir klasör yoluyla başlamalıdır. Geçerli değerler `2018/` ve içerir `2018/april/shoes.csv` . Kapsayıcı seçilmezse bu alan seçilemez.
    * **BLOB yolu şununla biter:** Blob yolu bir dosya adı veya uzantısıyla bitmelidir. Geçerli değerler `shoes.csv` ve içerir `.csv` . Kapsayıcı ve klasör adı isteğe bağlıdır, ancak belirtildiğinde, bir segmentle ayrılmaları gerekir `/blobs/` . Örneğin, ' Orders ' adlı bir kapsayıcının değeri olabilir `/orders/blobs/2018/april/shoes.csv` . Herhangi bir kapsayıcıdaki bir klasörü belirtmek için baştaki '/' karakterini atlayın. Örneğin, `april/shoes.csv` `shoes.csv` herhangi bir kapsayıcıda ' Nisan ' adlı klasörde adında bir olay tetikleyecektir.
    * Note: blob yolu **ile başlar** ve **biter** , depolama olay tetikleyicisinde izin verilen tek örüntü eşleşmektedir. Diğer joker karakter eşleştirme türleri tetikleyici türü için desteklenmez.

1. Tetikleyicinizin **BLOB tarafından oluşturulan** bir olaya, **BLOB Deleted** olayına veya her ikisine de yanıt verip vermeyeceğini seçin. Belirttiğiniz depolama konumunda, her olay tetikleyiciyle ilişkili Data Factory işlem hatlarını tetikler.

    ![Depolama olay tetikleyicisini yapılandırma](media/how-to-create-event-trigger/event-based-trigger-image2.png)

1. Tetikleyicinizin sıfır bayt içeren Blobları yoksayıp saymayacağını seçin.

1. Tetikleyiciyi yapılandırdıktan sonra Ileri ' ye tıklayın **: veri önizleme**. Bu ekranda, depolama olay tetikleyicisi yapılandırmanızla eşleşen mevcut blob 'lar gösterilir. Belirli filtrelerinizin bulunduğundan emin olun. Çok geniş olan filtrelerin yapılandırılması, oluşturulan/silinen çok sayıda dosya ile eşleştirebilir ve maliyetinizi önemli ölçüde etkileyebilir. Filtre koşullarınız doğrulandıktan sonra **son**' a tıklayın.

    ![Depolama olayı tetikleyicisi veri önizleme](media/how-to-create-event-trigger/event-based-trigger-image3.png)

1. Bu tetikleyiciye bir işlem hattı eklemek için işlem hattı tuvaline gidin ve **tetikleyici Ekle** ' ye tıklayın ve **Yeni/Düzenle**' yi seçin. Yan gezinti göründüğünde **tetikleyici seç...** açılan listesine tıklayın ve oluşturduğunuz tetikleyiciyi seçin. Ileri ' ye tıklayın: yapılandırmanın doğru olduğunu doğrulamak için **veri önizleme** ' **ye tıklayın ve ardından veri** önizlemenin doğru olduğundan emin olun.

1. İşlem hattınızda parametreler varsa, bunları tetikleyici parametre tarafı gezin ' i çalıştırır ' de belirtebilirsiniz. Depolama olayı tetikleyicisi, Blobun klasör yolunu ve dosya adını özelliklerine ve özelliklerine yakalar `@triggerBody().folderPath` `@triggerBody().fileName` . Bu özelliklerin değerlerini bir işlem hattında kullanmak için, özellikleri işlem hattı parametrelerine eşlemeniz gerekir. Özellikleri parametrelere eşleştirdikten sonra, tetikleyici tarafından yakalanan değerlere işlem `@pipeline().parameters.parameterName` hattı boyunca ifade aracılığıyla erişebilirsiniz. İşiniz bittiğinde **son** ' a tıklayın.

    ![Özellikleri ardışık düzen parametrelerine eşleme](media/how-to-create-event-trigger/event-based-trigger-image4.png)

Önceki örnekte tetikleyici, kapsayıcı örnek verilerinde. csv dosyasında bir blob yolu oluşturulduğunda,. csv ' de sonlanan bir blob yolu başlatıldığında tetiklenecek şekilde yapılandırılmıştır. **FolderPath** ve **filename** özellikleri yeni Blobun konumunu yakalar. Örneğin, yola MoviesDB.csv eklendiğinde-Data/Event-Testing `@triggerBody().folderPath` değeri, değerine sahiptir `sample-data/event-testing` ve değeri vardır `@triggerBody().fileName` `moviesDB.csv` . Bu değerler, örnekte işlem hattı parametrelerine eşlenir `sourceFolder` ve `sourceFile` ardışık düzen boyunca `@pipeline().parameters.sourceFolder` ve `@pipeline().parameters.sourceFile` sırasıyla kullanılabilir.

## <a name="json-schema"></a>JSON şeması

Aşağıdaki tabloda, depolama olay tetikleyicilerle ilgili şema öğelerine genel bir bakış sunulmaktadır:

| **JSON öğesi** | **Açıklama** | **Tür** | **İzin Verilen Değerler** | **Gerekli** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **kapsam** | Depolama hesabının Azure Resource Manager kaynak KIMLIĞI. | Dize | Azure Resource Manager KIMLIĞI | Yes |
| **olayları** | Bu tetikleyicinin tetiklenmesine neden olan olayların türü. | Dizi    | Microsoft. Storage. BlobCreated, Microsoft. Storage. BlobDeleted | Evet, bu değerlerin herhangi bir birleşimi. |
| **blobPathBeginsWith** | Blob yolu, tetikleyicinin tetiklenmesi için belirtilen Düzenle başlamalı. Örneğin, `/records/blobs/december/` yalnızca `december` kapsayıcının altındaki klasörde bulunan bloblara yönelik tetikleyiciyi tetikler `records` . | Dize   | | Şu özelliklerden en az biri için bir değer belirtmeniz gerekir: `blobPathBeginsWith` veya `blobPathEndsWith` . |
| **blobPathEndsWith** | Blob yolu, tetikleyicinin tetiklenmesi için belirtilen Düzenle bitmelidir. Örneğin, `december/boxes.csv` yalnızca bir klasörde adlı Bloblar için tetikleyiciyi harekete geçirilir `boxes` `december` . | Dize   | | Şu özelliklerden en az biri için bir değer belirtmeniz gerekir: `blobPathBeginsWith` veya `blobPathEndsWith` . |
| **ıgnoreemptyblob 'Lar** | Sıfır baytlık Blobların bir işlem hattı çalıştırmasını tetikleyip tetikleyemayacağı. Varsayılan olarak, bu true olarak ayarlanır. | Boole | true veya false | Hayır |

## <a name="examples-of-storage-event-triggers"></a>Depolama olay tetikleyicisi örnekleri

Bu bölümde, depolama olay tetikleyicisi ayarlarının örnekleri verilmiştir.

> [!IMPORTANT]
> `/blobs/`Her kapsayıcı ve klasör, kapsayıcı ve dosya ya da kapsayıcı, klasör ve dosya belirttiğinizde, aşağıdaki örneklerde gösterildiği gibi yolun segmentini eklemeniz gerekir. **Blobpathbeginswith** için, Data Factory kullanıcı ARABIRIMI, `/blobs/` JSON tetikleyicisi içindeki klasör ve kapsayıcı adı arasına otomatik olarak eklenir.

| Özellik | Örnek | Açıklama |
|---|---|---|
| **Blob yolu şununla başlar** | `/containername/` | Kapsayıcıdaki tüm Bloblar için olayları alır. |
| **Blob yolu şununla başlar** | `/containername/blobs/foldername/` | Kapsayıcı ve klasördeki Bloblar için olayları alır `containername` `foldername` . |
| **Blob yolu şununla başlar** | `/containername/blobs/foldername/subfoldername/` | Ayrıca, bir alt klasöre de başvurabilirsiniz. |
| **Blob yolu şununla başlar** | `/containername/blobs/foldername/file.txt` | `file.txt` `foldername` Kapsayıcının altındaki klasörde adlı bir blob için olayları alır `containername` . |
| **Blob yolu şununla biter** | `file.txt` | Herhangi bir yolda adlı bir blob için olayları alır `file.txt` . |
| **Blob yolu şununla biter** | `/containername/blobs/file.txt` | Kapsayıcı altında adlı bir blob için olayları alır `file.txt` `containername` . |
| **Blob yolu şununla biter** | `foldername/file.txt` | `file.txt`Herhangi bir kapsayıcı altındaki klasöründe adlı bir blob için olayları alır `foldername` . |

## <a name="next-steps"></a>Sonraki adımlar

Tetikleyiciler hakkında ayrıntılı bilgi için bkz. işlem [hattı yürütme ve Tetikleyiciler](concepts-pipeline-execution-triggers.md#trigger-execution).
