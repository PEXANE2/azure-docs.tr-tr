---
title: HTTP ile tetiklenen Azure Işlevlerini el ile çalıştırma
description: Http tarafından tetiklenen bir Azure Işlevi çalıştırmak için HTTP isteği kullanma
author: craigshoemaker
ms.topic: article
ms.date: 04/23/2020
ms.author: cshoe
ms.openlocfilehash: fd7b0be967c7a0bbc605c51408448917b5222d36
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83121822"
---
# <a name="manually-run-a-non-http-triggered-function"></a>HTTP ile tetiklenmeyen bir işlevi el ile çalıştırma

Bu makalede, HTTP ile tetiklenen bir işlevin özel olarak biçimlendirilmiş HTTP isteği aracılığıyla el ile nasıl çalıştırılacağı gösterilmektedir.

Bazı bağlamlarda, dolaylı olarak tetiklenen "isteğe bağlı" bir Azure Işlevi çalıştırmanız gerekebilir.  Dolaylı tetikleyicilere örnek olarak, [başka bir kaynağın eyleminin](./functions-create-storage-blob-triggered-function.md)sonucu olarak çalışan [bir zamanlama](./functions-create-scheduled-function.md) veya işlevlerde işlevler bulunur. 

[Postman](https://www.getpostman.com/) aşağıdaki örnekte kullanılır, ancak http istekleri göndermek için [kıvrımlı](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler) veya benzer bir aracı kullanabilirsiniz.

## <a name="define-the-request-location"></a>İstek konumunu tanımlayın

HTTP ile tetiklenen bir işlev çalıştırmak için, işlevi çalıştırmak üzere Azure 'a bir istek göndermek için bir yol gerekir. Bu isteği yapmak için kullanılan URL belirli bir formu alır.

![İstek konumunu tanımlayın: Ana bilgisayar adı + klasör yolu + işlev adı](./media/functions-manually-run-non-http/azure-functions-admin-url-anatomy.png)

- **Ana bilgisayar adı:** İşlev uygulamasının adı ve *azurewebsites.net* veya özel etki alanınız öğesinden oluşturulan Genel konum.
- **Klasör yolu:** Http ile tetiklenen işlevlere HTTP isteği aracılığıyla erişmek için, isteği klasörler *yöneticisi/işlevleri*aracılığıyla göndermeniz gerekir.
- **İşlev adı:** Çalıştırmak istediğiniz işlevin adı.

Bu istek konumunu, Postman 'da, işlevi çalıştırmak için Azure isteği 'ndeki ana anahtarla birlikte kullanırsınız.

> [!NOTE]
> Yerel olarak çalışırken işlevin ana anahtarı gerekli değildir. Üstbilgiyi atlayarak [işlevi doğrudan çağırabilirsiniz](#call-the-function) `x-functions-key` .

## <a name="get-the-functions-master-key"></a>İşlevin ana anahtarını al

1. Azure portal işlevinizde gelin ve **Işlev anahtarları**' nı seçin. Ardından, kopyalamak istediğiniz işlev anahtarını seçin. 

    :::image type="content" source="./media/functions-manually-run-non-http/azure-portal-functions-master-key.png" alt-text="Kopyalanacak ana anahtarı bulun." border="true":::

1. **Anahtarı Düzenle** bölümünde, anahtar değerini panonuza kopyalayın ve ardından **Tamam**' ı seçin.

    :::image type="content" source="./media/functions-manually-run-non-http/azure-portal-functions-master-key-copy.png" alt-text="Ana anahtarı panoya kopyalayın." border="true":::

1. *_Master* anahtarını kopyaladıktan sonra, **kod + test**' i seçin ve ardından **Günlükler**' i seçin. İşlevi Postman 'dan el ile çalıştırdığınızda, bu işlevden iletiler burada günlüğe kaydedilir.

    :::image type="content" source="./media/functions-manually-run-non-http/azure-portal-function-log.png" alt-text="Ana anahtar test sonuçlarını görmek için günlükleri görüntüleyin." border="true":::

> [!CAUTION]  
> Ana anahtar tarafından verilen işlev uygulamanızda yükseltilmiş izinler nedeniyle, bu anahtarı üçüncü taraflarla paylaşmamalıdır veya bir uygulamada dağıtmanız gerekir.

## <a name="call-the-function"></a>İşlevi çağırın

Postman 'ı açın ve şu adımları izleyin:

1. **URL metin kutusuna istek konumunu**girin.
1. HTTP yönteminin **gönderi**olarak ayarlandığından emin olun.
1. **Üstbilgiler** sekmesini seçin.
1. İlk anahtar olarak **x-Functions-Key** yazın ve ana anahtarı (panodan) değer olarak yapıştırın.
1. İkinci anahtar olarak **Içerik türü** yazın ve değer olarak **Application/JSON** yazın.

    :::image type="content" source="./media/functions-manually-run-non-http/functions-manually-run-non-http-headers.png" alt-text="Postman üstbilgileri ayarları." border="true":::

1. **Gövde** sekmesini seçin.
1. İsteğin gövdesi olarak **{"Input": "test"}** yazın.

    :::image type="content" source="./media/functions-manually-run-non-http/functions-manually-run-non-http-body.png" alt-text="Postman gövde ayarları." border="true":::

1. **Gönder**’i seçin.
        
    :::image type="content" source="./media/functions-manually-run-non-http/functions-manually-run-non-http-send.png" alt-text="Postman ile bir istek gönderin." border="true":::

    Postman daha sonra **202 durumunu kabul etti**olarak bildirir.

1. Sonra, Azure portal işlevinizi geri döndürün. Günlükleri gözden geçirin ve işleve el ile yapılan çağrıdan gelen iletiler görürsünüz.

    :::image type="content" source="./media/functions-manually-run-non-http/azure-portal-functions-master-key-logs.png" alt-text="Ana anahtar test sonuçlarını görmek için günlükleri görüntüleyin." border="true":::

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İşlevleri'nde kodunuzu test etmeye yönelik stratejiler](./functions-test-a-function.md)
- [Azure Işlevi Event Grid yerel hata ayıklamayı Tetikle](./functions-debug-event-grid-trigger-local.md)
