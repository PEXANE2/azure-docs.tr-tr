---
title: "Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure 'da ilk işlevinizi oluşturma"
description: Bu hızlı başlangıçta, Visual Studio kullanarak HTTP ile tetiklenen bir Azure Işlevi oluşturmayı ve yayımlamayı öğreneceksiniz.
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.topic: quickstart
ms.date: 03/06/2020
ms.custom: mvc, devcenter, vs-azure, 23113853-34f2-4f
ms.openlocfilehash: e58414fa94a4ef54eb2f288b2cd636c10611460d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81308930"
---
# <a name="quickstart-create-your-first-function-in-azure-using-visual-studio"></a>Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure 'da ilk işlevinizi oluşturma

Azure Işlevleri, öncelikle bir VM oluşturmak veya bir Web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda çalıştırmanızı sağlar.

Bu hızlı başlangıçta, daha sonra Azure 'Da yayımladığınız bir "Hello World" HTTP ile tetiklenen C# işlev uygulamasını yerel olarak oluşturmak ve test etmek için Visual Studio 2019 ' i nasıl kullanacağınızı öğrenirsiniz. 

![Tarayıcıdaki işlev localhost yanıtı](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-local-final.png)

Bu hızlı başlangıç, Visual Studio 2019 için tasarlanmıştır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için, önce [Visual Studio 2019](https://azure.microsoft.com/downloads/)' i yüklemeniz gerekir. Yükleme sırasında **Azure geliştirme** iş yükünü seçtiğinizden emin olun. Bunun yerine Visual Studio 2017 kullanarak bir Azure Işlevleri projesi oluşturmak istiyorsanız, önce [en son Azure işlevleri araçlarını](functions-develop-vs.md#check-your-tools-version)yüklemeniz gerekir.

![Azure geliştirme iş yüküyle Visual Studio 'Yu yükler](media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

[Azure aboneliğiniz](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/dotnet/) oluşturun.

## <a name="create-a-function-app-project"></a>İşlev uygulaması projesi oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Visual Studio, HTTP tetikleyici işlev türü için ortak kod içeren bir proje ve sınıf oluşturur. `FunctionName` Yöntem özniteliği, varsayılan olarak olduğu `Function1`gibi işlevin adını ayarlar. `HttpTrigger` Özniteliği, IŞLEVIN bir http isteği tarafından tetiklendiğini belirtir. Ortak kod, istek gövdesi veya sorgu dizesinde yer alan bir değeri içeren bir HTTP yanıtı gönderir.

Yöntemine uygun öznitelikleri uygulayarak giriş ve çıkış bağlamalarıyla işlevinizin yeteneklerini genişletin. Daha fazla bilgi için, [Azure İşlevleri C# geliştirici başvurusu](functions-dotnet-class-library.md) konusunun [Tetikleyiciler ve bağlayıcılar](functions-dotnet-class-library.md#triggers-and-bindings) bölümüne göz atın.

İşlev projenizi ve HTTP ile tetiklenen işlev oluşturduğunuza göre, artık bunu yerel bilgisayarınızda test edebilirsiniz.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Visual Studio, tüm Azure Işlevleri çalışma zamanını kullanarak işlevlerinizi yerel olarak test edebilmeniz için Azure Functions Core Tools ile tümleşir.  

[!INCLUDE [functions-run-function-test-local-vs](../../includes/functions-run-function-test-local-vs.md)]

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Projenizi yayımlayabilmeniz için önce Azure aboneliğinizde bir işlev uygulamasına sahip olmanız gerekir. Visual Studio yayımlama, projenizi ilk kez yayımladığınızda sizin için bir işlev uygulaması oluşturur.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Yayımlama** profili sayfasından işlev UYGULAMASıNıN temel URL 'sini kopyalayın. İşlevi yerel `localhost:port` olarak test etmek IÇIN kullandığınız URL 'nin bölümünü yenı temel URL ile değiştirin. Sorgu dizesini `?name=<YOUR_NAME>` bu URL 'ye ekleyin ve isteği çalıştırın.

    HTTP ile tetiklenen işlevinizi çağıran URL aşağıdaki biçimdedir:

    `http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?name=<YOUR_NAME>`

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki görüntüde, işlevi tarafından döndürülen uzaktan GET isteğine tarayıcıda yapılan yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-azure.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Visual Studio 'Yu kullanarak Azure 'da bir C# işlev uygulaması oluşturup basit bir HTTP tetiklenen işlevle yayımladık. 

İşleviniz için bir Azure depolama kuyruğu bağlamayı nasıl ekleyeceğinizi öğrenmek için sonraki makaleye ilerleyin:
> [!div class="nextstepaction"]
> [İşleviniz için bir Azure depolama kuyruğu bağlama ekleme](functions-add-output-binding-storage-queue-vs.md)

