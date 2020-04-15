---
title: "Quickstart: Visual Studio'u kullanarak Azure'da ilk işlevinizi oluşturun"
description: Bu hızlı başlangıçta, Visual Studio'yu kullanarak HTTP tarafından tetiklenen bir Azure İşi oluşturmayı ve yayımlamayı öğrenirsiniz.
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.topic: quickstart
ms.date: 03/06/2020
ms.custom: mvc, devcenter, vs-azure, 23113853-34f2-4f
ms.openlocfilehash: e58414fa94a4ef54eb2f288b2cd636c10611460d
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81308930"
---
# <a name="quickstart-create-your-first-function-in-azure-using-visual-studio"></a>Quickstart: Visual Studio'u kullanarak Azure'da ilk işlevinizi oluşturun

Azure İşlevler, önce bir VM oluşturmanıza veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda çalıştırmanızı sağlar.

Bu hızlı başlangıçta, Visual Studio 2019'u kullanarak Azure'da yayımladığınızda yerel olarak tetiklenen HTTP tarafından tetiklenen bir "merhaba dünyası" C# işlevi uygulamasını oluşturmayı ve test etmeyi öğrenirsiniz. 

![Tarayıcıdaki işlev localhost yanıtı](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-local-final.png)

Bu hızlı başlangıç Visual Studio 2019 için tasarlanmıştır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için önce [Visual Studio 2019'u](https://azure.microsoft.com/downloads/)yükleyin. Yükleme sırasında **Azure geliştirme** iş yükünü seçtiğinizden emin olun. Bunun yerine Visual Studio 2017'yi kullanarak bir Azure İşlevleri projesi oluşturmak istiyorsanız, öncelikle [en son Azure İşlevlerini yüklemeniz](functions-develop-vs.md#check-your-tools-version)gerekir.

![Azure geliştirme iş yüküyle Visual Studio'yu yükleyin](media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

[Azure aboneliğiniz](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/dotnet/) oluşturun.

## <a name="create-a-function-app-project"></a>İşlev uygulaması projesi oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Visual Studio, HTTP tetikleyici işlev türü için ortak kod içeren bir proje ve sınıf oluşturur. Yöntem `FunctionName` özniteliği, varsayılan olarak `Function1`. Öznitelik, `HttpTrigger` işlevin bir HTTP isteği tarafından tetiklenediğini belirtir. Ortak kod, istek gövdesi veya sorgu dizesinde yer alan bir değeri içeren bir HTTP yanıtı gönderir.

Yönteme uygun öznitelikleri uygulayarak giriş ve çıkış bağlamaları ile işlevinizin yeteneklerini genişletin. Daha fazla bilgi için, [Azure İşlevleri C# geliştirici başvurusu](functions-dotnet-class-library.md) konusunun [Tetikleyiciler ve bağlayıcılar](functions-dotnet-class-library.md#triggers-and-bindings) bölümüne göz atın.

İşlev projenizi ve HTTP ile tetiklenen işlev oluşturduğunuza göre, artık bunu yerel bilgisayarınızda test edebilirsiniz.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Visual Studio, tam Azure İşlevlerini çalışma süresini kullanarak işlevlerinizi yerel olarak test edebilmeniz için Azure İşlevler Temel Araçları ile tümleşir.  

[!INCLUDE [functions-run-function-test-local-vs](../../includes/functions-run-function-test-local-vs.md)]

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Projenizi yayımlayamadan önce Azure aboneliğinizde bir işlev uygulamanız olması gerekir. Visual Studio yayıncılığı, projenizi ilk yayımladığınızda sizin için bir işlev uygulaması oluşturur.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. İşlev uygulamasının temel URL'sini **Profil Yayımla** sayfasından kopyalayın. İşlevi `localhost:port` yerel olarak test etmek için kullandığınız URL'nin yeni temel URL ile değiştirme. Sorgu dizesini `?name=<YOUR_NAME>` bu URL'ye ekleyin ve isteği çalıştırın.

    HTTP tetikleme işlevinizi çağıran URL aşağıdaki biçimdedir:

    `http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?name=<YOUR_NAME>`

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki resim, tarayıcıdaki işlevi tarafından döndürülen uzaktan GET isteğine verilen yanıtı gösterir:

    ![Tarayıcıdaki işlev yanıtı](./media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-azure.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Visual Studio'yu kullanarak Azure'da basit bir HTTP tetikleme işlevine sahip bir C# işlevi uygulaması oluşturup yayımladınız. 

İşlevinize nasıl bir Azure Depolama kuyruğu ekleyeceğinizi öğrenmek için bir sonraki makaleye ilerleyin:
> [!div class="nextstepaction"]
> [İşlevinize Azure Depolama sırasına bağlama ekleme](functions-add-output-binding-storage-queue-vs.md)

