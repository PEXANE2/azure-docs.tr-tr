---
title: Azure İşlevleri ve SignalR Hizmeti ile bir sohbet odası oluşturmak için Java'yı kullanın
description: Azure SignalR Hizmetini ve Azure İşlevlerini kullanarak bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: java
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: zhshang
ms.openlocfilehash: 890fc381afe0146e721e084e2dcd7eae9215d004
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77083208"
---
# <a name="quickstart-use-java-to-create-a-chat-room-with-azure-functions-and-signalr-service"></a>Quickstart: Azure Fonksiyonları ve SignalR Hizmeti ile bir sohbet odası oluşturmak için Java'yı kullanın

Azure SignalR Hizmeti, uygulamanıza kolayca gerçek zamanlı işlevsellik eklemenize olanak tanır ve Azure İşlevleri, herhangi bir altyapıyı yönetmeden kodunuzu çalıştırmanızı sağlayan sunucusuz bir platformdur. Bu hızlı başlatmada, SignalR Hizmeti ve Fonksiyonlarını kullanarak sunucusuz, gerçek zamanlı bir sohbet uygulaması oluşturmak için Java'yı kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Visual Studio Code gibi bir kod [düzenleyicisi](https://code.visualstudio.com/)
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure İşlevler Temel Araçları](https://github.com/Azure/azure-functions-core-tools#installing). Azure İşlevi uygulamalarını yerel olarak çalıştırmak için kullanılır.

   > [!NOTE]
   > Java'daki gerekli SignalR Hizmeti bağlamaları yalnızca Azure İşlemesi Temel Araçları sürüm 2.4.419 (ana bilgisayar sürümü 2.0.12332) veya daha yukarısında desteklenir.

   > [!NOTE]
   > Uzantıları yüklemek için Azure İşlevler Temel [Araçları,.NET Core SDK'nın](https://www.microsoft.com/net/download) yüklü olmasını gerektirir. Ancak JavaScript Azure İşlev uygulamaları oluşturmak için .NET bilgisi gerekmemektedir.

- [Java Developer Kit](https://www.azul.com/downloads/zulu/), sürüm 8
- [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri

> [!NOTE]
> Bu hızlı başlangıç; macOS, Windows veya Linux üzerinde çalıştırılabilir.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com/> sayfasında oturum açın.

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]

## <a name="configure-and-run-the-azure-function-app"></a>Azure İşlev Uygulamasını yapılandırıp çalıştırma

1. Azure portalın açık olduğu tarayıcıda portalın üst kısmındaki arama kutusundan adını arayarak önceden dağıttığınız SignalR Hizmeti örneğinin başarılı bir şekilde oluşturulduğundan emin olun. Açmak için örneği seçin.

    ![SignalR Hizmeti örneğini arayın](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. SignalR Hizmeti örneğinin bağlantı dizelerini görüntülemek için **Anahtarlar**’ı seçin.

1. Birincil bağlantı dizesini seçerek kopyalayın.

    ![SignalR Hizmeti Oluşturma](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-keys.png)

1. Kod düzenleyicinizde, klonlanmış depodaki *src/chat/java* klasörünü açın.

1. *local.settings.sample.json* dosyasını *local.settings.json* olarak yeniden adlandırın.

1. **local.settings.json** dosyasının içinde bağlantı dizesini **AzureSignalRConnectionString** ayarının değerine yapıştırın. Dosyayı kaydedin.

1. Fonksiyonları içeren ana dosya *src/chat/java/src/main/java/com/function/Functions.java*bulunmaktadır:

    - **negotiate** - Geçerli bağlantı bilgileri döndürmek için *SignalRConnectionInfo* giriş bağlamasını kullanır.
    - **sendMessage** - İstek gövdesinde bir sohbet iletisi alır ve iletiyi bağlı tüm istemci uygulamalarına yayınlamak için *SignalR* çıktısını kullanır.

1. *Terminalde, src/chat/java* klasöründe olduğundan emin olun. İşlev uygulamasını oluşturun.

    ```bash
    mvn clean package
    ```

1. İşlev uygulamasını yerel olarak çalıştırın.

    ```bash
    mvn azure-functions:run
    ```

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Maven'i kullanarak gerçek zamanlı sunucusuz bir uygulama oluşturup çalıştırdın. Ardından, Java Azure İşlevlerini sıfırdan nasıl oluşturabilirsiniz hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Java ve Maven ile ilk işlevinizi oluşturun](../azure-functions/functions-create-first-java-maven.md)