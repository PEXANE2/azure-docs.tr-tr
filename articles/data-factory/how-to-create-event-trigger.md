---
title: Azure Data Factory olay tabanlı Tetikleyiciler oluşturma
description: Bir olaya yanıt olarak bir işlem hattı çalıştıran Azure Data Factory tetikleyici oluşturmayı öğrenin.
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
ms.openlocfilehash: 10f0079f47e5d2fd99b358fcc5cfb4c80aa9bd91
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84508905"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Bir olaya yanıt olarak bir işlem hattı çalıştıran bir tetikleyici oluşturma
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Data Factory işlem hatlarında oluşturabileceğiniz olay tabanlı tetikleyiciler açıklanmaktadır.

Olay odaklı mimari (EDA), olaylar için üretim, algılama, tüketim ve yeniden eyleme sahip ortak bir veri tümleştirme modelidir. Veri tümleştirme senaryoları genellikle Data Factory müşterilerin Azure Depolama hesabınızdaki bir dosyanın geliş veya silme gibi olaylara göre işlem hatlarını tetiklemesine gerek duyar. Data Factory artık, bir olayda işlem hatlarını tetiklemenizi sağlayan [Azure Event Grid](https://azure.microsoft.com/services/event-grid/)ile tümleşiktir.

Bu özelliğin on dakikalık bir giriş ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> Bu makalede açıklanan tümleştirme [Azure Event Grid](https://azure.microsoft.com/services/event-grid/)bağımlıdır. Aboneliğinizin Event Grid kaynak sağlayıcısına kayıtlı olduğundan emin olun. Daha fazla bilgi için bkz. [kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). *Microsoft. EventGrid/Eventabonelikleri/** eylemini yapabilmelisiniz. Bu eylem EventGrid EventSubscription katılımcısı yerleşik rolünün bir parçasıdır.

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

Bu bölümde, Azure Data Factory Kullanıcı arabiriminde nasıl bir olay tetikleyicisi oluşturacağınız gösterilmektedir.

1. **Yazma tuvaline** git

1. Sol alt köşede **Tetikleyiciler** düğmesine tıklayın

1. Tetikleyici oluşturma tarafı gezintisi ' ni açacak **+ Yeni** ' ye tıklayın

1. Tetikleyici türü **olayını** seçin

    ![Yeni olay tetikleyicisi oluştur](media/how-to-create-event-trigger/event-based-trigger-image1.png)

1. Azure aboneliği açılan listesinden veya depolama hesabı kaynak KIMLIĞINI kullanarak el ile depolama hesabınızı seçin. Olayların gerçekleşmesini istediğiniz kapsayıcıyı seçin. Kapsayıcı seçimi isteğe bağlıdır, ancak tüm kapsayıcıları seçtiğinizde çok sayıda olaya yol açabilir.

   > [!NOTE]
   > Olay tetikleyicisi Şu anda yalnızca Azure Data Lake Storage 2. ve genel amaçlı sürüm 2 depolama hesaplarını desteklemektedir. Depolama hesabında en az *sahip* erişiminizin olması gerekir.  Azure Event Grid sınırlaması nedeniyle, Azure Data Factory yalnızca depolama hesabı başına en fazla 500 olay tetikleyicisi destekler.

1. **BLOB yolu ile başlar** ve **BLOB yolu, özellikler ile biter** ve olayları almak istediğiniz kapsayıcıları, klasörleri ve BLOB adlarını belirtmenize olanak tanır. Olay Tetikleyiciniz bu özelliklerden en az birinin tanımlanmasını gerektirir. Her iki **BLOB yolu ile başlar** ve **BLOB yolu** , bu makalenin ilerleyen kısımlarında gösterildiği gibi özellikleriyle biter.

    * **BLOB yolu şununla başlar:** Blob yolu bir klasör yoluyla başlamalıdır. Geçerli değerler `2018/` ve içerir `2018/april/shoes.csv` . Kapsayıcı seçilmezse bu alan seçilemez.
    * **BLOB yolu şununla biter:** Blob yolu bir dosya adı veya uzantısıyla bitmelidir. Geçerli değerler `shoes.csv` ve içerir `.csv` . Kapsayıcı ve klasör adı isteğe bağlıdır, ancak belirtildiğinde, bir segmentle ayrılmaları gerekir `/blobs/` . Örneğin, ' Orders ' adlı bir kapsayıcının değeri olabilir `/orders/blobs/2018/april/shoes.csv` . Herhangi bir kapsayıcıdaki bir klasörü belirtmek için baştaki '/' karakterini atlayın. Örneğin, `april/shoes.csv` `shoes.csv` herhangi bir kapsayıcıda ' Nisan ' adlı klasörde adında bir olay tetikleyecektir. 

1. Tetikleyicinizin **BLOB tarafından oluşturulan** bir olaya, **BLOB Deleted** olayına veya her ikisine de yanıt verip vermeyeceğini seçin. Belirttiğiniz depolama konumunda, her olay tetikleyiciyle ilişkili Data Factory işlem hatlarını tetikler.

    ![Olay tetikleyicisini yapılandırma](media/how-to-create-event-trigger/event-based-trigger-image2.png)

1. Tetikleyicinizin sıfır bayt içeren Blobları yoksayıp saymayacağını seçin.

1. Tetikleyiciyi yapılandırdıktan sonra Ileri ' ye tıklayın **: veri önizleme**. Bu ekranda, olay tetikleyicisi yapılandırmanızla eşleşen mevcut blob 'lar gösterilir. Belirli filtrelerinizin bulunduğundan emin olun. Çok geniş olan filtrelerin yapılandırılması, oluşturulan/silinen çok sayıda dosya ile eşleştirebilir ve maliyetinizi önemli ölçüde etkileyebilir. Filtre koşullarınız doğrulandıktan sonra **son**' a tıklayın.

    ![Olay tetikleyicisi veri önizleme](media/how-to-create-event-trigger/event-based-trigger-image3.png)

1. Bu tetikleyiciye bir işlem hattı eklemek için işlem hattı tuvaline gidin ve **tetikleyici Ekle** ' ye tıklayın ve **Yeni/Düzenle**' yi seçin. Yan gezinti göründüğünde **tetikleyici seç...** açılan listesine tıklayın ve oluşturduğunuz tetikleyiciyi seçin. Ileri ' ye tıklayın: yapılandırmanın doğru olduğunu doğrulamak için **veri önizleme** ' **ye tıklayın ve ardından veri** önizlemenin doğru olduğundan emin olun.

1. İşlem hattınızda parametreler varsa, bunları tetikleyici parametre tarafı gezin ' i çalıştırır ' de belirtebilirsiniz. Olay tetikleyicisi, Blobun klasör yolunu ve dosya adını özelliklerine ve özelliklerine yakalar `@trigger().outputs.body.folderPath` `@trigger().outputs.body.fileName` . Bu özelliklerin değerlerini bir işlem hattında kullanmak için, özellikleri işlem hattı parametrelerine eşlemeniz gerekir. Özellikleri parametrelere eşleştirdikten sonra, tetikleyici tarafından yakalanan değerlere işlem `@pipeline().parameters.parameterName` hattı boyunca ifade aracılığıyla erişebilirsiniz. İşiniz bittiğinde **son** ' a tıklayın.

    ![Özellikleri ardışık düzen parametrelerine eşleme](media/how-to-create-event-trigger/event-based-trigger-image4.png)

Önceki örnekte tetikleyici, kapsayıcı örnek verilerinde. csv dosyasında bir blob yolu oluşturulduğunda,. csv ' de sonlanan bir blob yolu başlatıldığında tetiklenecek şekilde yapılandırılmıştır. **FolderPath** ve **filename** özellikleri yeni Blobun konumunu yakalar. Örneğin, yola MoviesDB.csv eklendiğinde-Data/Event-Testing `@trigger().outputs.body.folderPath` değeri, değerine sahiptir `sample-data/event-testing` ve değeri vardır `@trigger().outputs.body.fileName` `moviesDB.csv` . Bu değerler, örnekte işlem hattı parametrelerine eşlenir `sourceFolder` ve `sourceFile` ardışık düzen boyunca `@pipeline().parameters.sourceFolder` ve `@pipeline().parameters.sourceFile` sırasıyla kullanılabilir.

## <a name="json-schema"></a>JSON şeması

Aşağıdaki tabloda olay tabanlı tetikleyicilerle ilgili şema öğelerine genel bir bakış verilmiştir:

| **JSON öğesi** | **Açıklama** | **Tür** | **İzin Verilen Değerler** | **Gerekli** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **kapsam** | Depolama hesabının Azure Resource Manager kaynak KIMLIĞI. | Dize | Azure Resource Manager KIMLIĞI | Evet |
| **olayları** | Bu tetikleyicinin tetiklenmesine neden olan olayların türü. | Dizi    | Microsoft. Storage. BlobCreated, Microsoft. Storage. BlobDeleted | Evet, bu değerlerin herhangi bir birleşimi. |
| **blobPathBeginsWith** | Blob yolu, tetikleyicinin tetiklenmesi için belirtilen Düzenle başlamalı. Örneğin, `/records/blobs/december/` yalnızca `december` kapsayıcının altındaki klasörde bulunan bloblara yönelik tetikleyiciyi tetikler `records` . | Dize   | | Şu özelliklerden en az biri için bir değer belirtmeniz gerekir: `blobPathBeginsWith` veya `blobPathEndsWith` . |
| **blobPathEndsWith** | Blob yolu, tetikleyicinin tetiklenmesi için belirtilen Düzenle bitmelidir. Örneğin, `december/boxes.csv` yalnızca bir klasörde adlı Bloblar için tetikleyiciyi harekete geçirilir `boxes` `december` . | Dize   | | Şu özelliklerden en az biri için bir değer belirtmeniz gerekir: `blobPathBeginsWith` veya `blobPathEndsWith` . |
| **ıgnoreemptyblob 'Lar** | Sıfır baytlık Blobların bir işlem hattı çalıştırmasını tetikleyip tetikleyemayacağı. Varsayılan olarak, bu true olarak ayarlanır. | Boole | true veya false | Hayır |

## <a name="examples-of-event-based-triggers"></a>Olay tabanlı tetikleyicilere örnekler

Bu bölümde olay tabanlı tetikleyici ayarlarının örnekleri verilmiştir.

> [!IMPORTANT]
> `/blobs/`Her kapsayıcı ve klasör, kapsayıcı ve dosya ya da kapsayıcı, klasör ve dosya belirttiğinizde, aşağıdaki örneklerde gösterildiği gibi yolun segmentini eklemeniz gerekir. **Blobpathbeginswith**için, Data Factory kullanıcı ARABIRIMI, `/blobs/` JSON tetikleyicisi içindeki klasör ve kapsayıcı adı arasına otomatik olarak eklenir.

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
