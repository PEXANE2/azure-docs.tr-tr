---
title: Azure Işlevleri ve SignalR hizmeti ile sohbet odası oluşturmak için Java 'Yı kullanma
description: Azure SignalR Hizmetini ve Azure İşlevlerini kullanarak bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: java
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: zhshang
ms.custom: devx-track-java
ms.openlocfilehash: b67480f2b6de37822d74162f46caa4d7e50dc193
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321794"
---
# <a name="quickstart-use-java-to-create-a-chat-room-with-azure-functions-and-signalr-service"></a>Hızlı başlangıç: Java kullanarak Azure Işlevleri ve SignalR hizmeti ile sohbet odası oluşturma

Azure SignalR hizmeti, uygulamanıza kolayca gerçek zamanlı işlevselliği eklemenizi sağlar ve Azure Işlevleri, herhangi bir altyapıyı yönetmeden kodunuzu çalıştırmanıza olanak tanıyan sunucusuz bir platformdur. Bu hızlı başlangıçta, SignalR hizmeti ve Işlevleri 'ni kullanarak sunucusuz, gerçek zamanlı bir sohbet uygulaması oluşturmak için Java 'Yı kullanırsınız.

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio Code](https://code.visualstudio.com/) gibi bir kod Düzenleyicisi
- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing). Azure Function uygulamalarını yerel olarak çalıştırmak için kullanılır.

   > [!NOTE]
   > Java 'daki gerekli SignalR hizmeti bağlamaları yalnızca Azure Function Core araçları sürüm 2.4.419 (ana bilgisayar sürümü 2.0.12332) veya üzeri sürümlerde desteklenir.

   > [!NOTE]
   > Uzantıları yüklemek için Azure Functions Core Tools [.NET Core SDK](https://www.microsoft.com/net/download) yüklü olmasını gerektirir. Ancak JavaScript Azure İşlev uygulamaları oluşturmak için .NET bilgisi gerekmemektedir.

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

1. Kod Düzenleyicinizde, kopyalanmış depodaki *src/chat/Java* klasörünü açın.

1. *local.settings.sample.json* dosyasını *local.settings.json* olarak yeniden adlandırın.

1. **local.settings.json** dosyasının içinde bağlantı dizesini **AzureSignalRConnectionString** ayarının değerine yapıştırın. Dosyayı kaydedin.

1. İşlevleri içeren ana dosya *src/chat/Java/src/Main/Java/com/Function/Functions. Java*' da bulunmaktadır:

    - **negotiate** - Geçerli bağlantı bilgileri döndürmek için *SignalRConnectionInfo* giriş bağlamasını kullanır.
    - **SendMessage** -istek gövdesinde bir sohbet iletisi alır ve iletiyi tüm bağlı istemci uygulamalarına yayımlamak Için *SignalR* çıkış bağlamasını kullanır.

1. Terminalde, *src/chat/Java* klasöründe olduğunuzdan emin olun. İşlev uygulamasını oluşturun.

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

Bu hızlı başlangıçta, Maven kullanarak gerçek zamanlı sunucusuz bir uygulama oluşturup çalıştırdınız. Ardından, sıfırdan Java Azure Işlevleri oluşturma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Java ve Maven ile ilk işlevinizi oluşturma](../azure-functions/functions-create-first-java-maven.md)