---
title: Azure Data Factory 'de olay tabanlı Tetikleyiciler oluşturma | Microsoft Docs
description: Bir olaya yanıt olarak bir işlem hattı çalıştıran Azure Data Factory tetikleyici oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
author: sharonlo101
ms.author: shlo
manager: craigg
ms.openlocfilehash: 32edacb7dd66274757359c4eb0e8c169995026ce
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69019534"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Bir olaya yanıt olarak bir işlem hattı çalıştıran bir tetikleyici oluşturma

Bu makalede, Data Factory işlem hatlarında oluşturabileceğiniz olay tabanlı tetikleyiciler açıklanmaktadır.

Olay odaklı mimari (EDA), olaylar için üretim, algılama, tüketim ve yeniden eyleme sahip ortak bir veri tümleştirme modelidir. Veri tümleştirme senaryoları genellikle Data Factory müşterilerin Azure Depolama hesabınızdaki bir dosyanın geliş veya silme gibi olaylara göre işlem hatlarını tetiklemesine gerek duyar. Data Factory artık, bir olayda işlem hatlarını tetiklemenizi sağlayan [Azure Event Grid](https://azure.microsoft.com/services/event-grid/)ile tümleşiktir.

Bu özelliğin on dakikalık bir giriş ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> Bu makalede açıklanan tümleştirme [Azure Event Grid](https://azure.microsoft.com/services/event-grid/)bağımlıdır. Aboneliğinizin Event Grid kaynak sağlayıcısına kayıtlı olduğundan emin olun. Daha fazla bilgi için bkz. [kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md#azure-portal).

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

Bu bölümde, Azure Data Factory Kullanıcı arabiriminde nasıl bir olay tetikleyicisi oluşturacağınız gösterilmektedir.

1. **Yazma tuvaline** git

2. Sol alt köşede **Tetikleyiciler** düğmesine tıklayın

3. Tetikleyici oluşturma tarafı gezintisi ' ni açacak **+ Yeni** ' ye tıklayın

4. Tetikleyici türü **olayını** seçin

![Yeni olay tetikleyicisi oluştur](media/how-to-create-event-trigger/event-based-trigger-image1.png)

5. Azure aboneliği açılan listesinden veya depolama hesabı kaynak KIMLIĞINI kullanarak el ile depolama hesabınızı seçin. Olayların gerçekleşmesini istediğiniz kapsayıcıyı seçin. Kapsayıcı seçimi isteğe bağlıdır, ancak tüm kapsayıcıları seçtiğinizde çok sayıda olaya yol açabilir.

   > [!NOTE]
   > Olay tetikleyicisi Şu anda yalnızca Azure Data Lake Storage 2. ve genel amaçlı sürüm 2 depolama hesaplarını desteklemektedir. Azure Event Grid sınırlaması nedeniyle, Azure Data Factory yalnızca depolama hesabı başına en fazla 500 olay tetikleyicisi destekler.

6. **BLOB yolu ile başlar** ve **BLOB yolu, özellikler ile biter** ve olayları almak istediğiniz kapsayıcıları, klasörleri ve BLOB adlarını belirtmenize olanak tanır. Olay Tetikleyiciniz bu özelliklerden en az birinin tanımlanmasını gerektirir. Her iki **BLOB yolu ile başlar** ve **BLOB yolu** , bu makalenin ilerleyen kısımlarında gösterildiği gibi özellikleriyle biter.

    * **Blob yolu şununla başlar:** Blob yolu bir klasör yoluyla başlamalıdır. Geçerli değerler ve `2018/` `2018/april/shoes.csv`içerir. Kapsayıcı seçilmezse bu alan seçilemez.
    * **Blob yolu şununla biter:** Blob yolu bir dosya adı veya uzantısıyla bitmelidir. Geçerli değerler ve `shoes.csv` `.csv`içerir. Kapsayıcı ve klasör adı isteğe bağlıdır, ancak belirtildiğinde, bir `/blobs/` segmentle ayrılmaları gerekir. Örneğin, ' Orders ' adlı bir kapsayıcının değeri `/orders/blobs/2018/april/shoes.csv`olabilir. Herhangi bir kapsayıcıdaki bir klasörü belirtmek için baştaki '/' karakterini atlayın. Örneğin, `april/shoes.csv` herhangi bir kapsayıcıda ' Nisan ' adlı klasörde adında `shoes.csv` bir olay tetikleyecektir. 

7. Tetikleyicinizin **BLOB tarafından oluşturulan** bir olaya, **BLOB Deleted** olayına veya her ikisine de yanıt verip vermeyeceğini seçin. Belirttiğiniz depolama konumunda, her olay tetikleyiciyle ilişkili Data Factory işlem hatlarını tetikler.

    ![Olay tetikleyicisini yapılandırma](media/how-to-create-event-trigger/event-based-trigger-image2.png)

8. Tetikleyiciyi yapılandırdıktan sonra, **ileri ' ye tıklayın: Veri önizleme**. Bu ekranda, olay tetikleyicisi yapılandırmanızla eşleşen mevcut blob 'lar gösterilir. Belirli filtrelerinizin bulunduğundan emin olun. Çok geniş olan filtrelerin yapılandırılması, oluşturulan/silinen çok sayıda dosya ile eşleştirebilir ve maliyetinizi önemli ölçüde etkileyebilir. Filtre koşullarınız doğrulandıktan sonra **son**' a tıklayın.

    ![Olay tetikleyicisi veri önizleme](media/how-to-create-event-trigger/event-based-trigger-image3.png)

9. Bu tetikleyiciye bir işlem hattı eklemek için işlem hattı tuvaline gidin ve **tetikleyici Ekle** ' ye tıklayın ve **Yeni/Düzenle**' yi seçin. Yan gezinti göründüğünde **tetikleyici seç...** açılan listesine tıklayın ve oluşturduğunuz tetikleyiciyi seçin. İleri **' ye tıklayın: Yapılandırmanın doğru** olduğundan emin olmak için veri önizlemesi ve sonra veri önizlemenin doğru olduğunu doğrulayın.

10. İşlem hattınızda parametreler varsa, bunları tetikleyici parametre tarafı gezin ' i çalıştırır ' de belirtebilirsiniz. Olay tetikleyicisi, Blobun klasör yolunu ve dosya adını özelliklerine ve `@triggerBody().folderPath` `@triggerBody().fileName`özelliklerine yakalar. Bu özelliklerin değerlerini bir işlem hattında kullanmak için, özellikleri işlem hattı parametrelerine eşlemeniz gerekir. Özellikleri parametrelere eşleştirdikten sonra, tetikleyici tarafından yakalanan değerlere işlem hattı boyunca `@pipeline().parameters.parameterName` ifade aracılığıyla erişebilirsiniz. İşiniz bittiğinde **son** ' a tıklayın.

    ![Özellikleri ardışık düzen parametrelerine eşleme](media/how-to-create-event-trigger/event-based-trigger-image4.png)

Önceki örnekte tetikleyici, kapsayıcı örnek verilerinde. csv dosyasında bir blob yolu oluşturulduğunda,. csv ' de sonlanan bir blob yolu başlatıldığında tetiklenecek şekilde yapılandırılmıştır. **FolderPath** ve **filename** özellikleri yeni Blobun konumunu yakalar. Örneğin, MoviesDB. csv, yol örneğine eklendiğinde-Data/ `@triggerBody().folderPath` Event-Testing değerine sahiptir `sample-data/event-testing` `moviesDB.csv`ve `@triggerBody().fileName` değeri olur. Bu değerler, örnekte `sourceFolder` işlem hattı parametrelerine eşlenir ve `sourceFile` ardışık düzen boyunca `@pipeline().parameters.sourceFolder` ve `@pipeline().parameters.sourceFile` sırasıyla kullanılabilir.

## <a name="json-schema"></a>JSON şeması

Aşağıdaki tabloda olay tabanlı tetikleyicilerle ilgili şema öğelerine genel bir bakış verilmiştir:

| **JSON öğesi** | **Açıklama** | **Tür** | **İzin verilen değerler** | **Gerekli** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **kapsam** | Depolama hesabının Azure Resource Manager kaynak KIMLIĞI. | Dize | Azure Resource Manager KIMLIĞI | Evet |
| **olayları** | Bu tetikleyicinin tetiklenmesine neden olan olayların türü. | Array    | Microsoft. Storage. BlobCreated, Microsoft. Storage. BlobDeleted | Evet, bu değerlerin herhangi bir birleşimi. |
| **blobPathBeginsWith** | Blob yolu, tetikleyicinin tetiklenmesi için belirtilen Düzenle başlamalı. Örneğin, `/records/blobs/december/` yalnızca `records` kapsayıcının altındaki `december` klasörde bulunan bloblara yönelik tetikleyiciyi tetikler. | Dize   | | Şu özelliklerden en az biri için bir değer belirtmeniz gerekir: `blobPathBeginsWith` veya. `blobPathEndsWith` |
| **blobPathEndsWith** | Blob yolu, tetikleyicinin tetiklenmesi için belirtilen Düzenle bitmelidir. Örneğin, `december/boxes.csv` yalnızca bir `december` klasörde adlı `boxes` Bloblar için tetikleyiciyi harekete geçirilir. | Dize   | | Şu özelliklerden en az biri için bir değer belirtmeniz gerekir: `blobPathBeginsWith` veya. `blobPathEndsWith` |

## <a name="examples-of-event-based-triggers"></a>Olay tabanlı tetikleyicilere örnekler

Bu bölümde olay tabanlı tetikleyici ayarlarının örnekleri verilmiştir.

> [!IMPORTANT]
> Her kapsayıcı ve klasör, `/blobs/` kapsayıcı ve dosya ya da kapsayıcı, klasör ve dosya belirttiğinizde, aşağıdaki örneklerde gösterildiği gibi yolun segmentini eklemeniz gerekir. **Blobpathbeginswith**için, Data Factory Kullanıcı arabirimi, JSON tetikleyicisi `/blobs/` içindeki klasör ve kapsayıcı adı arasına otomatik olarak eklenir.

| Özellik | Örnek | Açıklama |
|---|---|---|
| **Blob yolu şununla başlar** | `/containername/` | Kapsayıcıdaki tüm Bloblar için olayları alır. |
| **Blob yolu şununla başlar** | `/containername/blobs/foldername/` | `containername` Kapsayıcı ve`foldername` klasördeki Bloblar için olayları alır. |
| **Blob yolu şununla başlar** | `/containername/blobs/foldername/subfoldername/` | Ayrıca, bir alt klasöre de başvurabilirsiniz. |
| **Blob yolu şununla başlar** | `/containername/blobs/foldername/file.txt` | Kapsayıcının altındaki `foldername` `file.txt` klasördeadlıbirblobiçinolaylarıalır`containername` . |
| **Blob yolu şununla biter** | `file.txt` | Herhangi bir yolda adlı `file.txt` bir blob için olayları alır. |
| **Blob yolu şununla biter** | `/containername/blobs/file.txt` | `file.txt` Kapsayıcı`containername`altında adlı bir blob için olayları alır. |
| **Blob yolu şununla biter** | `foldername/file.txt` | Herhangi bir kapsayıcı altındaki `file.txt` `foldername` klasöründe adlı bir blob için olayları alır. |

## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi için bkz. işlem [hattı yürütme ve Tetikleyiciler](concepts-pipeline-execution-triggers.md#triggers).
