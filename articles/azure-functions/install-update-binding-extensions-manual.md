---
title: Azure İşlevlerini bağlama uzantılarını el ile yükleme veya güncelleştirme
description: Dağıtılan işlev uygulamaları için Azure İşlevleri bağlama uzantılarını nasıl yükleyip güncelleştirdiğinizi öğrenin.
ms.topic: reference
ms.date: 09/26/2018
ms.openlocfilehash: e8716f691a5d19ddac7fece47c423e1f7787b9db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75768869"
---
# <a name="manually-install-or-update-azure-functions-binding-extensions-from-the-portal"></a>Azure İşlevlerini bağlama uzantılarını portaldan el ile yükleme veya güncelleştirme

Sürüm 2.x ile başlayarak, Azure İşlevleri çalışma zamanı tetikleyiciler ve bağlamalar için kod uygulamak için bağlama uzantıları kullanır. Bağlama uzantıları NuGet paketlerinde sağlanır. Bir uzantıyı kaydetmek için, aslında bir paket yüklersiniz. İşlevler geliştirirken, bağlama uzantıları yükleme şekliniz geliştirme ortamına bağlıdır. Daha fazla bilgi için [bkz.](./functions-bindings-register.md)

Bazen azure portalında bağlama uzantılarınızı el ile yüklemeniz veya güncelleştirmeniz gerekir. Örneğin, kayıtlı bir bağlamayı daha yeni bir sürüme güncelleştirmeniz gerekebilir. Ayrıca, portaldaki **Tümleştir** sekmesine yüklenemez desteklenen bir bağlama yı kaydetmeniz gerekebilir.

## <a name="install-a-binding-extension"></a>Bağlama uzantısı yükleme

Portaldan uzantıları el ile yüklemek veya güncelleştirmek için aşağıdaki adımları kullanın.

1. Azure [portalında](https://portal.azure.com)işlev uygulamanızı bulun ve seçin. Genel **Bakış** sekmesini seçin ve **Durdur'u**seçin.  İşlev uygulamasının durdurulması, değişikliklerin yapılabilmeleri için dosyaların kilidini açar.

1. Platform **özellikleri** sekmesini seçin ve **Geliştirme araçları** altında **Gelişmiş Araçlar (Kudu)** seçin. Kudu bitiş noktası`https://<APP_NAME>.scm.azurewebsites.net/`( ) yeni bir pencerede açılır.

1. Kudu penceresinde Hata **Ayıklama konsolu** > **CMD'yi**seçin.  

1. Komut penceresinde, klasörü `D:\home\site\wwwroot` silmek `bin` için yanındaki silme simgesine gidin ve seçin. Silme işlemini onaylamak için **Tamam'ı** seçin.

1. İşlev uygulamasının bağlama `extensions.csproj` uzantılarını tanımlayan dosyanın yanındaki edit simgesini seçin. Proje dosyası çevrimiçi düzenleyicide açılır.

1. **ItemGroup'ta** **PackageReference** öğelerinin gerekli eklemelerini ve güncelleştirmelerini yapın, ardından **Kaydet'i**seçin. Desteklenen paket sürümlerinin geçerli listesi hangi [paketlere ihtiyacım var?](https://github.com/Azure/azure-functions-host/wiki/Updating-your-function-app-extensions#what-nuget-packages-do-i-need) Üç Azure Depolama bağlayıcısı da Microsoft.Azure.WebJobs.Extensions.Storage paketini gerektirir.

1. Klasörden, `wwwroot` klasörde başvurulan derlemeleri yeniden oluşturmak `bin` için aşağıdaki komutu çalıştırın.

    ```cmd
    dotnet build extensions.csproj -o bin --no-incremental --packages D:\home\.nuget
    ```

1. Portaldaki **Genel Bakış** sekmesinde, işlev uygulamasını yeniden başlatmak için **Başlat'ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
