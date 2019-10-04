---
title: Python işleviniz için bir Azure depolama kuyruğu bağlama ekleme
description: Azure CLı ve Işlevleri temel araçlarını kullanarak Python işlevinizin Azure depolama kuyruğu çıkışı bağlamayı nasıl ekleyeceğinizi öğrenin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 92ee9b0a8a0906bca31d7dcb1730c3464d0d6cbc
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839188"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Python işleviniz için bir Azure depolama kuyruğu bağlama ekleme

Azure Işlevleri, kendi tümleştirme kodunuzu yazmak zorunda kalmadan Azure hizmetlerini ve diğer kaynakları işlevlere bağlamanıza olanak tanır. Hem giriş hem de çıktıyı temsil eden bu *bağlamalar*, işlev tanımı içinde bildirilmiştir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır. *Tetikleyici* özel bir giriş bağlama türüdür. Bir işlevde yalnızca bir tetikleyici olsa da, birden çok giriş ve çıkış bağlaması olabilir. Daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

Bu makalede, [önceki hızlı başlangıç makalesinde](functions-create-first-function-python.md) oluşturduğunuz Işlevi bir Azure depolama kuyruğu ile tümleştirme işlemi gösterilmektedir. Bu işleve eklediğiniz çıkış bağlaması, verileri bir HTTP isteğinden kuyruktaki bir iletiye yazar.

Çoğu bağlamanın, bağlı hizmete erişmek için kullandığı depolanan bir bağlantı dizesi gerekir. Bu bağlantıyı daha kolay hale getirmek için, işlev uygulamanızla oluşturduğunuz depolama hesabını kullanırsınız. Bu hesap bağlantısı zaten `AzureWebJobsStorage` adlı bir uygulama ayarında depolanıyor.  

## <a name="prerequisites"></a>Prerequisites

Bu makaleye başlamadan önce, [Python hızlı](functions-create-first-function-python.md)başlangıcının 1. bölümündeki adımları uygulayın.

[!INCLUDE [functions-cloud-shell-note](../../includes/functions-cloud-shell-note.md)]

## <a name="download-the-function-app-settings"></a>İşlev uygulaması ayarlarını indirin

[!INCLUDE [functions-app-settings-download-local-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Uzantı paketlerini etkinleştir

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Artık, depolama çıkış bağlamasını projenize ekleyebilirsiniz.

## <a name="add-an-output-binding"></a>Çıkış bağlaması ekleme

Işlevlerde, her bağlama türü, function. json dosyasında tanımlanacak bir `direction`, `type` ve benzersiz bir `name` gerektirir. Bu öznitelikleri tanımlama yöntemi, işlev uygulamanızın diline bağlıdır.

[!INCLUDE [functions-add-output-binding-json](../../includes/functions-add-output-binding-json.md)]

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlamayı kullanan kodu ekleyin

[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]

Çıkış bağlamayı kullandığınızda, kimlik doğrulaması, kuyruk başvurusu alma veya veri yazma için Azure depolama SDK kodunu kullanmanız gerekmez. Işlevler çalışma zamanı ve kuyruk çıkış bağlaması bu görevleri sizin için işler.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırın

Daha önce olduğu gibi, Işlevleri çalışma zamanını yerel olarak başlatmak için aşağıdaki komutu kullanın:

```bash
func host start
```

> [!NOTE]  
> Önceki hızlı başlangıçta ana bilgisayar. JSON içindeki uzantı paketlerinin etkinleştirilmesi nedeniyle, [depolama bağlama uzantısı](functions-bindings-storage-blob.md#packages---functions-2x) , başlangıç sırasında diğer Microsoft bağlama uzantılarıyla birlikte indirilip sizin için yüklendi.

@No__t-0 işlevinizin URL 'sini çalışma zamanı çıktısından kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. @No__t-0 sorgu dizesini bu URL 'ye ekleyin ve isteği çalıştırın. Önceki makalede yaptığınız gibi tarayıcıda aynı yanıtı görmeniz gerekir.

Bu kez, çıkış bağlama aynı zamanda depolama hesabınızda `outqueue` adlı bir kuyruk oluşturur ve bu dizeyi içeren bir ileti ekler.

Ardından, yeni kuyruğu görüntülemek ve bir iletinin eklendiğini doğrulamak için Azure CLı 'yi kullanırsınız. Kuyruğunuzu, [Microsoft Azure Depolama Gezgini][Azure Storage Explorer] veya [Azure Portal](https://portal.azure.com)kullanarak da görüntüleyebilirsiniz.

### <a name="set-the-storage-account-connection"></a>Depolama hesabı bağlantısını ayarla

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

### <a name="query-the-storage-queue"></a>Depolama kuyruğunu sorgulama

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

Şimdi güncelleştirilmiş işlev uygulamasını Azure 'da yeniden yayımlamanız zaman.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

Yine, dağıtılan işlevi test etmek için kıvrımlı veya tarayıcı kullanabilirsiniz. Daha önce olduğu gibi, aşağıdaki örnekte olduğu gibi `&name=<yourname>` sorgu dizesini URL 'ye ekleyin:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

Çıktı bağlamasının sırada yeni bir ileti üretdiğini doğrulamak için [depolama kuyruğu iletisini inceleyebilirsiniz](#query-the-storage-queue) .

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen işlevinizi bir depolama kuyruğuna veri yazmak için güncelleştirdiniz. Python ile Azure Işlevleri geliştirme hakkında daha fazla bilgi için bkz. [Azure Işlevleri Python Geliştirici Kılavuzu](functions-reference-python.md) ve [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md). Python 'da tüm Işlev projelerinin örnekleri için bkz. [Python işlevleri örnekleri](/samples/browse/?products=azure-functions&languages=python). 

Sonra, işlev uygulamanız için Application Insights izlemeyi etkinleştirmelisiniz:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmeyi etkinleştir](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/