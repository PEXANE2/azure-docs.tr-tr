---
title: Azure portalından Linux'ta bir işlev uygulaması oluşturma
description: Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
ms.topic: how-to
ms.date: 02/28/2019
ms.openlocfilehash: b2de36faf07ad661ff8817adc48b726f54990ceb
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754122"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan"></a>Azure Uygulama Hizmeti planında Linux'ta bir işlev uygulaması oluşturma

Azure İşlevleri, işlevlerinizi Linux’ta varsayılan bir Azure App Service kapsayıcısında barındırmanıza olanak sağlar. Bu makale, Bir [Uygulama Hizmeti planında](functions-scale.md#app-service-plan)çalışan bir Linux barındırılan işlev uygulaması oluşturmak için [Azure portalını](https://portal.azure.com) nasıl kullanacağınızı size iletir. Ayrıca [kendi özel kapsayıcınızı getirebilirsiniz](functions-create-function-linux-custom-image.md).

![Azure portalında işlev uygulaması oluşturma](./media/create-function-app-linux-app-service-plan/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com> sayfasında oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Linux’ta işlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun yürütülmesine yönelik bir ortam sağlar. Kaynakları daha kolay yönetme, dağıtım, ölçekleme ve paylaşma için işlevleri mantıksal bir birim olarak gruplandırmanıza olanak tanır. Bu makalede, işlev uygulamanızı oluştururken bir Uygulama Hizmeti planı oluşturursunuz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesini seçip **İşlem** > **İşlev Uygulaması**’nı seçin.

    ![Azure portalında işlev uygulaması oluşturma](./media/create-function-app-linux-app-service-plan/function-app-create-flow.png)

2. Görüntünün altındaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/create-function-app-linux-app-service-plan/function-app-create-flow2.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. | 
    | **[Kaynak Grubu](../azure-resource-manager/management/overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
    | **İşletim Sistemi** | Linux | Fonksiyon uygulaması Linux üzerinde çalışır. |
    | **Yayımlama** | Kod | **Runtime** Stack'iniz için varsayılan Linux kapsayıcısı kullanılır. Sağlamanız gereken tek şey işlev uygulaması proje kodunuzdur. Başka bir seçenek özel bir [Docker görüntü](functions-create-function-linux-custom-image.md)yayımlamaktır. |
    | **[Hosting planı](functions-scale.md)** | App Service planı | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. Bir Uygulama Hizmeti planında çalıştırdığınızda, [işlev uygulamanızın ölçeklemesini](functions-scale.md)kontrol edebilirsiniz.  |
    | **Uygulama hizmeti planı/Konumu** | Plan oluşturma | **Yeni Oluştur'u** seçin ve bir Uygulama **Hizmeti planı** adı kaynağı oluşturun. Yakınınızdaki bir [bölgede](https://azure.microsoft.com/regions/) veya işlevlerinizin erişebileceği diğer hizmetlere yakın bir konumda bir **Konum** seçin. İstediğiniz **[Fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/app-service/linux/)** seçin. <br/>Aynı Uygulama Hizmeti planında hem Linux hem de Windows işlev uygulamalarını çalıştıramazsınız. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F# için **.NET** işlevlerini seçin. |
    | **[Depolama](../storage/common/storage-account-create.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Dilerseniz [depolama hesabı gereksinimlerini](storage-considerations.md#storage-account-requirements) karşılayan mevcut bir hesap da kullanabilirsiniz. |
    | **[Application Insights](functions-monitoring.md)** | Etkin | Uygulama Öngörüleri varsayılan olarak devre dışı bırakılır. Uygulama Öngörüleri entegrasyonunu şimdi etkinleştirmenizi ve Uygulama Hizmeti planı konumunuza yakın bir barındırma konumu seçmenizi öneririz. Bunu daha sonra yapmak istiyorsanız, [Azure İşlerini İzle'ye](functions-monitoring.md)bakın.  |

3. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

4. Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/create-function-app-linux-app-service-plan/function-app-create-notification.png)

5. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin.

Ardından, yeni işlev uygulamasında bir işlev oluşturun. İşlev uygulamanız kullanılabilir hale getirilebilir hale getirse bile, tam olarak başlatılması birkaç dakika sürebilir.

## <a name="create-an-http-triggered-function"></a><a name="create-function"></a>HTTP ile tetiklenen bir işlev oluşturma

Bu bölümde, portaldaki yeni işlev uygulamanızda nasıl bir işlev oluşturulabileceğiniz gösterilmektedir.

> [!NOTE]
> Portal geliştirme deneyimi Azure İşlevlerini denemek için yararlı olabilir. Çoğu senaryoda, işlevlerinizi yerel olarak geliştirmeyi ve [Visual Studio Code](functions-create-first-function-vs-code.md#create-an-azure-functions-project) veya [Azure İşlevleri Temel Araçları'nı](functions-run-local.md#create-a-local-functions-project)kullanarak projeyi işlev uygulamanızda yayımlamayı düşünün.  

1. Yeni işlev uygulamanızda Genel **Bakış** sekmesini seçin ve yüklendikten sonra tamamen **+ Yeni işlevi**seçin.

    ![Genel Bakış sekmesinden yeni bir işlev oluşturma](./media/create-function-app-linux-app-service-plan/overview-create-function.png)

1. **Quickstart** sekmesinde **Portal'ı**seçin ve **Devam et'i**seçin.

    ![Fonksiyon geliştirme platformunuzu seçin.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-choose-portal.png)

1. **Web Kancası + API**'yi ve ardından **Oluştur**'u seçin.

    ![Azure portalındaki İşlevler hızlı başlangıcı.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-node-webhook.png)

Dile özgü HTTP ile tetiklenen işlev şablonu kullanılarak bir işlev oluşturulur.

Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni işlevinizde sağ üst kısımdaki **</> İşlev URL'sini al**'a tıklayın, **varsayılan (İşlev anahtarı)** seçeneğini belirleyin ve ardından **Kopyala**'ya tıklayın. 

    ![Azure portalından işlev URL’sini kopyalama](./media/create-function-app-linux-app-service-plan/function-app-develop-tab-testing.png)

2. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. `&name=<yourname>` sorgu dizesi değerini bu URL’nin sonuna ekleyin ve isteği yürütmek için klavyenizdeki `Enter` tuşuna basın. İşlev tarafından döndürülen yanıtın tarayıcıda gösterildiğini görürsünüz.  

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/create-function-app-linux-app-service-plan/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

3. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. Önceki yürütme işleminden alınan izleme çıktısını görmek için, portalda işlevinize geri dönün ve ekranın altındaki oka tıklayarak **Günlükler**’i genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/create-function-app-linux-app-service-plan/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak işlev uygulaması oluşturdunuz.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Daha fazla bilgi için bkz. [Azure İşlevleri HTTP bağlamaları](functions-bindings-http-webhook.md).
