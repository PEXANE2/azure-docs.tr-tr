---
title: HTTP ile tetiklenen Azure Işlevlerini el ile çalıştırma
description: Http tarafından tetiklenen bir Azure Işlevi çalıştırmak için HTTP isteği kullanma
author: craigshoemaker
ms.topic: tutorial
ms.date: 12/12/2018
ms.author: cshoe
ms.openlocfilehash: 8198ff6579aff839ff9aacb729e2f3f8d3472fae
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230469"
---
# <a name="manually-run-a-non-http-triggered-function"></a>HTTP ile tetiklenmeyen bir işlevi el ile çalıştırma

Bu makalede, HTTP ile tetiklenen bir işlevin özel olarak biçimlendirilmiş HTTP isteği aracılığıyla el ile nasıl çalıştırılacağı gösterilmektedir.

Bazı bağlamlarda, dolaylı olarak tetiklenen "isteğe bağlı" bir Azure Işlevi çalıştırmanız gerekebilir.  Dolaylı tetikleyicilere örnek olarak, [başka bir kaynağın eyleminin](./functions-create-storage-blob-triggered-function.md)sonucu olarak çalışan [bir zamanlama](./functions-create-scheduled-function.md) veya işlevlerde işlevler bulunur. 

[Postman](https://www.getpostman.com/) aşağıdaki örnekte kullanılır, ancak http istekleri göndermek için [kıvrımlı](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler) veya benzer bir aracı kullanabilirsiniz.

## <a name="define-the-request-location"></a>İstek konumunu tanımlayın

HTTP ile tetiklenen bir işlev çalıştırmak için, işlevi çalıştırmak üzere Azure 'a bir istek göndermeniz gerekir. Bu isteği yapmak için kullanılan URL belirli bir formu alır.

![İstek konumunu tanımlayın: Ana bilgisayar adı + klasör yolu + işlev adı](./media/functions-manually-run-non-http/azure-functions-admin-url-anatomy.png)

- **Ana bilgisayar adı:** İşlev uygulamasının adı ve *azurewebsites.net* veya özel etki alanınız öğesinden oluşturulan Genel konum.
- **Klasör yolu:** Http ile tetiklenen işlevlere HTTP isteği aracılığıyla erişmek için, isteği klasörler *yöneticisi/işlevleri*aracılığıyla göndermeniz gerekir.
- **İşlev adı:** Çalıştırmak istediğiniz işlevin adı.

Bu istek konumunu, Postman 'da, işlevi çalıştırmak için Azure isteği 'ndeki ana anahtarla birlikte kullanırsınız.

> [!NOTE]
> Yerel olarak çalışırken işlevin ana anahtarı gerekli değildir. `x-functions-key` üstbilgisini atlayarak [işlevi doğrudan çağırabilirsiniz](#call-the-function) .

## <a name="get-the-functions-master-key"></a>İşlevin ana anahtarını al

Azure portal işlevinizde gelin ve **Yönet** ' e tıklayın ve **konak anahtarlarını** bulun bölümüne tıklayın. Ana anahtarı panonuza kopyalamak için *_master* satırındaki **Kopyala** düğmesine tıklayın.

![Işlev yönetimi ekranından ana anahtarı Kopyala](./media/functions-manually-run-non-http/azure-portal-functions-master-key.png)

Ana anahtarı kopyaladıktan sonra, kod dosyası penceresine dönmek için işlev adına tıklayın. Sonra, **Günlükler** sekmesine tıklayın. İşlevi Postman 'dan el ile çalıştırdığınızda, bu işlevden iletiler burada günlüğe kaydedilir.

> [!CAUTION]  
> Ana anahtar tarafından verilen işlev uygulamanızda yükseltilmiş izinler nedeniyle, bu anahtarı üçüncü taraflarla paylaşmamalıdır veya bir uygulamada dağıtmanız gerekir.

## <a name="call-the-function"></a>İşlevi çağırın

Postman 'ı açın ve şu adımları izleyin:

1. **URL metin kutusuna istek konumunu**girin.
2. HTTP yönteminin **gönderi**olarak ayarlandığından emin olun.
3. **Üstbilgiler** sekmesine **tıklayın** .
4. İlk **anahtar** olarak **x-Functions-Key** girin ve ana anahtarı (panodan) **değer** kutusuna yapıştırın.
5. İkinci **anahtar** olarak **içerik türü** girin ve **uygulama/JSON** **değerini değer**olarak girin.

    ![Postman üstbilgileri ayarları](./media/functions-manually-run-non-http/functions-manually-run-non-http-headers.png)

6. **Gövde** sekmesine **tıklayın** .
7. İsteğin gövdesi olarak **{"Input": "test"}** girin.

    ![Postman gövde ayarları](./media/functions-manually-run-non-http/functions-manually-run-non-http-body.png)

8. **Gönder**' e tıklayın.

    ![Postman ile istek gönderme](./media/functions-manually-run-non-http/functions-manually-run-non-http-send.png)

Postman daha sonra **202 durumunu kabul etti**olarak bildirir.

Sonra, Azure portal işlevinizi geri döndürün. *Günlükler* penceresini bulun ve işleve el ile yapılan çağrıdan gelen iletiler görürsünüz.

![El ile çağrının işlev günlüğü sonuçları](./media/functions-manually-run-non-http/azure-portal-function-log.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Işlevlerinde kodunuzu test etme stratejileri](./functions-test-a-function.md)
- [Azure Işlevi Event Grid yerel hata ayıklamayı Tetikle](./functions-debug-event-grid-trigger-local.md)
