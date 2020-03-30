---
title: Visual Studio'yu kullanarak işlevleri Azure Depolama'ya bağlayın
description: Visual Studio'yu kullanarak C# sınıfı kitaplık işlevlerinizi Azure Depolama kuyruğuna bağlamak için çıktı bağlamayı nasıl ekleyeceğinizi öğrenin.
ms.date: 07/22/2019
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: 171479a0f60741b545a171315e99cc5e4e8bc843
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74849216"
---
# <a name="connect-functions-to-azure-storage-using-visual-studio"></a>Visual Studio'yu kullanarak işlevleri Azure Depolama'ya bağlayın

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

Bu makalede, [önceki hızlı başlangıç makalesinde] oluşturduğunuz işlevi Azure Depolama'ya bağlamak için Visual Studio'yu nasıl kullanacağınızı gösterir. Bu işleve eklediğiniz çıktı bağlama, HTTP isteğindeki verileri Azure Kuyruk depolama kuyruğundaki iletiye yazar. 

Çoğu bağlama, Bağlı hizmete erişmek için Işlevlerin kullandığı depolanmış bir bağlantı dizesi gerektirir. Bunu kolaylaştırmak için, işlev uygulamanızla oluşturduğunuz Depolama hesabını kullanırsınız. Bu hesaba bağlantı zaten adlı `AzureWebJobsStorage`bir uygulama ayarında depolanır.  

## <a name="prerequisites"></a>Ön koşullar

Bu makaleye başlamadan önce şunları 

 - [Visual Studio quickstart](./functions-create-first-function-vs-code.md)tam bölüm 1 . 

- Visual Studio'dan Azure aboneliğinizde oturum açın.

## <a name="download-the-function-app-settings"></a>Fonksiyon uygulama ayarlarını indirin

Önceki [hızlı başlangıç makalesinde,](functions-create-first-function-vs-code.md)gerekli Depolama hesabıyla birlikte Azure'da bir işlev uygulaması oluşturdunuz. Bu hesabın bağlantı dizesi Azure'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken Depolama hesabınıza bağlanmak için uygulama ayarlarını *local.settings.json* dosyasına indirmeniz gerekir. 

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin. 

1. **Eylemler**altında Azure **Uygulama Hizmeti Ayarlarını Edit'i**seçin. 

    ![Uygulama ayarlarını değiştirme](media/functions-add-output-binding-storage-queue-vs/edit-app-settings.png)

1. **AzureWebJobsStorage**altında, **Uzak dize** değerini **Yerel'e**kopyalayın ve ardından **Tamam'ı**seçin. 

Bağlantı ayarını `AzureWebJobsStorage` kullanan depolama aygıtı bağlama, artık yerel olarak çalışırken Sıra depolama alanınıza bağlanabilir.

## <a name="register-binding-extensions"></a>Bağlama uzantılarını kaydetme

Sıra depolama çıktısı bağlama kullanıyorsanız, projeyi çalıştırmadan önce Depolama bağlayıcıları uzantısının yüklenmesi gerekir. HTTP ve zamanlayıcı tetikleyicileri dışında, bağlamalar uzatma paketleri olarak uygulanır. 

1. **Araçlar** menüsünden **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'nu**seçin. 

1. Konsolda, Depolama uzantılarını yüklemek için aşağıdaki [Install-Package](/nuget/tools/ps-ref-install-package) komutunu çalıştırın:

    ```Command
    Install-Package Microsoft.Azure.WebJobs.Extensions.Storage -Version 3.0.6
    ````

Artık, depolama çıktısını projenize bağlayabilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

Bağlama tanımlandıktan sonra, işlev `name` imzasında bir öznitelik olarak erişmek için bağlama kullanabilirsiniz. Çıktı bağlama kullanarak, kimlik doğrulama, sıra başvurusu alma veya veri yazmak için Azure Depolama SDK kodunu kullanmanız gerekmez. İşlevler çalışma zamanı ve sıra çıktısı bağlama bu görevleri sizin için yapar.

[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

[!INCLUDE [functions-run-function-test-local-vs](../../includes/functions-run-function-test-local-vs.md)]

Çıktı bağlama `outqueue` ilk kullanıldığında, Işlevler çalışma zamanı tarafından depolama hesabınızda adı geçen yeni bir kuyruk oluşturulur. Kuyruğun yeni iletiyle birlikte oluşturulduğunu doğrulamak için Bulut Gezgini'ni kullanırsınız.

## <a name="examine-the-output-queue"></a>Çıkış kuyruğunu inceleme

1. **View** menüsünden Visual Studio'da **Cloud Explorer'ı**seçin.

1. **Bulut Gezgini'nde,** Azure aboneliğinizi ve **Depolama Hesaplarınızı**genişletin, ardından işleviniz tarafından kullanılan depolama hesabını genişletin. Depolama hesabı adını hatırlayamıyorsanız, `AzureWebJobsStorage` *local.settings.json* dosyasındaki bağlantı dize ayarını kontrol edin.  

1. **Sıradüğüm'ü** genişletin ve ardından Visual Studio'daki sıranın içeriğini görüntülemek için dış sıra adlı **sırayı** çift tıklatın. 

   Kuyruk, HTTP ile tetiklenen işlevi çalıştırdığınızda kuyruk çıkış bağlamasının oluşturduğu iletiyi içerir. İşlevi varsayılan `name`*Azure* değeri ile çağırdıysanız, kuyruk iletisi *İşleve geçirilen ad: Azure* şeklinde olur.

    ![Azure Depolama Gezgini'nde gösterilen sıra iletisi](./media/functions-add-output-binding-storage-queue-vs-code/function-queue-storage-output-view-queue.png)

1. İşlevi yeniden çalıştırın, başka bir istek gönderin ve kuyrukta yeni bir ileti nin göründüğünü görürsünüz.  

Şimdi, güncelleştirilmiş işlev uygulamasını Azure'da yeniden yayımlama zamanı.

## <a name="redeploy-and-verify-the-updated-app"></a>Güncellenen uygulamayı yeniden dağıtın ve doğrulayın

1. **Çözüm Gezgini'nde,** projeyi sağ tıklatın ve **Yayımla'yı**seçin , ardından projeyi Azure'da yeniden yayımlamak için **Yayımla'yı** seçin.

1. Dağıtım tamamlandıktan sonra, yeniden dağıtılan işlevi sınamak için tarayıcıyı yeniden kullanabilirsiniz. Daha önce olduğu gibi, `&name=<yourname>` sorgu dizesini URL'ye ekleyin.

1. Çıktı bağlamanın kuyrukta yeni bir ileti oluşturduğunu doğrulamak için [depolama kuyruğundaki iletiyi](#examine-the-output-queue) yeniden görüntüleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Depolama kuyruğuna veri yazmak için HTTP tetiklenen işlevinizi güncellediniz. İşlevler geliştirme hakkında daha fazla bilgi edinmek için [Visual Studio'yı kullanarak Azure İşlevlerini Geliştir'e](functions-develop-vs.md)bakın.

Ardından, işlev uygulamanız için Uygulama Öngörüleri izlemeetkinleştirmelisiniz:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/
[önceki quickstart makale]: functions-create-your-first-function-visual-studio.md
