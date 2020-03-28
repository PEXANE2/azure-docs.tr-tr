---
title: HTTP tarafından tetiklenen olmayan Azure İşlevlerini el ile çalıştırma
description: HTTP tetiklenen olmayan bir Azure İşlevlerini çalıştırmak için bir HTTP isteği kullanma
author: craigshoemaker
ms.topic: tutorial
ms.date: 12/12/2018
ms.author: cshoe
ms.openlocfilehash: 4ce7b8590e4718585fe841921466e049dc204928
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75769141"
---
# <a name="manually-run-a-non-http-triggered-function"></a>HTTP ile tetiklenmeyen bir işlevi el ile çalıştırma

Bu makalede, özel olarak biçimlendirilmiş HTTP isteği aracılığıyla, HTTP tarafından tetiklenen olmayan bir işlevin el ile nasıl çalıştırılacağA çalışılmış olması gösterilmektedir.

Bazı bağlamlarda, dolaylı olarak tetiklenen bir Azure İşi"ni "isteğe bağlı" çalıştırmanız gerekebilir.  Dolaylı tetikleyicilere örnek [olarak, zamanlamadaki işlevler](./functions-create-scheduled-function.md) veya başka bir [kaynağın eylemi](./functions-create-storage-blob-triggered-function.md)sonucunda çalışan işlevler verilebilir. 

[Postacı](https://www.getpostman.com/) aşağıdaki örnekte kullanılır, ancak http isteklerini göndermek için [cURL,](https://curl.haxx.se/) [Fiddler](https://www.telerik.com/fiddler) veya benzeri herhangi bir araç kullanabilirsiniz.

## <a name="define-the-request-location"></a>İstek konumunu tanımlama

HTTP tarafından tetiklenen olmayan bir işlevi çalıştırmak için, işlevi çalıştırmak için Azure'a istek göndermenin bir yolunu niçin yapmanız gerekir. Bu isteği yapmak için kullanılan URL belirli bir form alır.

![İstek konumunu tanımlayın: ana bilgisayar adı + klasör yolu + işlev adı](./media/functions-manually-run-non-http/azure-functions-admin-url-anatomy.png)

- **Ev sahibi adı:** İşlev uygulamasının, işlev uygulamasının adından ve *azurewebsites.net* veya özel etki alanınızdan oluşan genel konumu.
- **Klasör yolu:** HTTP tarafından tetiklenen olmayan işlevlere http isteği yle erişmek için, isteği *yönetici/işlevler*klasörleri üzerinden göndermeniz gerekir.
- **Fonksiyon adı:** Çalıştırmak istediğiniz işlevin adı.

Bu istek konumunu Postacı'da, işlevi çalıştırmak için Azure'a istekte işlevin ana anahtarıyla birlikte kullanırsınız.

> [!NOTE]
> Yerel olarak çalışırken, işlevin ana anahtarı gerekli değildir. `x-functions-key` Üstbilgiat atlayarak işlevi doğrudan [arayabilirsiniz.](#call-the-function)

## <a name="get-the-functions-master-key"></a>Fonksiyonun ana anahtarını alın

Azure portalındaki işlevinize gidin ve **Yönet'e** tıklayın ve **Ana Bilgisayar Anahtarlarını** bulun. Panonuza ana anahtarı kopyalamak için *_master* satırdaki **Kopyala** düğmesini tıklatın.

![İşlev Yönetimi ekranından ana anahtarı kopyalama](./media/functions-manually-run-non-http/azure-portal-functions-master-key.png)

Ana anahtarı kopyaladıktan sonra, kod dosyası penceresine dönmek için işlev adını tıklatın. Ardından, **Günlükler** sekmesine tıklayın. Postacı'dan işlevi el ile çalıştırdığınızda burada günlüğe kaydedilen işlevden iletileri görürsünüz.

> [!CAUTION]  
> Ana anahtar tarafından verilen işlev uygulamanızdaki yüksek izinler nedeniyle, bu anahtarı üçüncü taraflarla paylaşmamalı veya bir uygulamada dağıtmamalısınız.

## <a name="call-the-function"></a>İşlevi çağırın

Postacı'yı açın ve aşağıdaki adımları izleyin:

1. URL **metin kutusuna istek konumunu**girin.
2. HTTP yönteminin **POST**olarak ayarlandığından emin olun.
3. **Üstbilgi** sekmesine **tıklayın.**
4. İlk anahtar olarak **x-functions** **tuşu** girin ve ana anahtarı (panodan) **değer** kutusuna yapıştırın.
5. İkinci **anahtar** olarak **İçerik-Yazın** girin ve **değer**olarak **uygulama/json** girin.

    ![Postacı başlıkları ayarları](./media/functions-manually-run-non-http/functions-manually-run-non-http-headers.png)

6. **Vücut** sekmesine **tıklayın.**
7. İstek için gövde olarak **{ "giriş": "test" }** girin.

    ![Postacı vücut ayarları](./media/functions-manually-run-non-http/functions-manually-run-non-http-body.png)

8. **Gönder**’e tıklayın.

    ![Postacı ile istek gönderme](./media/functions-manually-run-non-http/functions-manually-run-non-http-send.png)

Postacı daha sonra **202 Kabul edilen**bir durum bildirir.

Ardından, Azure portalındaki işlevinize dönün. *Günlükler* penceresini bulun ve işleve manuel çağrıdan gelen iletileri görürsünüz.

![Manuel aramadan fonksiyon günlüğü sonuçları](./media/functions-manually-run-non-http/azure-portal-function-log.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İşlevleri'nde kodunuzu test etmeye yönelik stratejiler](./functions-test-a-function.md)
- [Azure İşlevi Olay Izgara yerel hata ayıklama tetikleme](./functions-debug-event-grid-trigger-local.md)
