---
title: Azure İşlevleri ve SignalR Hizmeti ile bir sohbet odası oluşturmak için JavaScript'i kullanın
description: Azure SignalR Hizmetini ve Azure İşlevlerini kullanarak bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: javascript
ms.topic: quickstart
ms.date: 12/14/2019
ms.author: zhshang
ms.openlocfilehash: 2726d5da2613be4ae2065246543d206cf814f353
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77083182"
---
# <a name="quickstart-use-javascript-to-create-a-chat-room-with-azure-functions-and-signalr-service"></a>Quickstart: Azure Fonksiyonları ve SignalR Hizmeti ile bir sohbet odası oluşturmak için JavaScript'i kullanın

Azure SignalR Hizmeti, uygulamanıza kolayca gerçek zamanlı işlevsellik eklemenize olanak tanır ve Azure İşlevleri, herhangi bir altyapıyı yönetmeden kodunuzu çalıştırmanızı sağlayan sunucusuz bir platformdur. Bu hızlı başlatmada, SignalR Hizmeti ve Fonksiyonlarını kullanarak sunucusuz, gerçek zamanlı bir sohbet uygulaması oluşturmak için JavaScript'i kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Visual Studio Code gibi bir kod [düzenleyicisi](https://code.visualstudio.com/)
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure İşlevler Temel Araçları](https://github.com/Azure/azure-functions-core-tools#installing), sürüm 2 veya üzeri. Azure İşlevi uygulamalarını yerel olarak çalıştırmak için kullanılır.
- [Düğüm.js](https://nodejs.org/en/download/), sürüm 10.x

   > [!NOTE]
   > Örnekler Node.js'nin diğer sürümleriyle çalışmalı, daha fazla bilgi için [Azure İşlevleri çalışma zamanı sürümleri belgelerine](../azure-functions/functions-versions.md#languages) bakın.

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

1. Kod düzenleyicinizde, klonlanmış depodaki *src/chat/javascript* klasörünü açın.

1. *local.settings.sample.json* dosyasını *local.settings.json* olarak yeniden adlandırın.

1. **local.settings.json** dosyasının içinde bağlantı dizesini **AzureSignalRConnectionString** ayarının değerine yapıştırın. Dosyayı kaydedin.

1. JavaScript işlevleri klasörler halinde düzenlenir. Her klasörde iki dosya bulunur: *function.json* işlevde kullanılan bağlamaları tanımlar ve *index.js* işlevin gövdesidir. Bu işlev uygulamasında iki adet HTTP ile tetiklenen işlev vardır:

    - **negotiate** - Geçerli bağlantı bilgileri döndürmek için *SignalRConnectionInfo* giriş bağlamasını kullanır.
    - **messages** - İstek gövdesinde bir sohbet iletisi alır ve iletiyi bağlı olan tüm istemci uygulamalara yaymak için *SignalR* çıkış bağlamasını kullanır.

1. *Terminalde, src/chat/javascript* klasöründe olduğundan emin olun. İşlev uygulamasını çalıştırın.

    ```bash
    func start
    ```

    ![SignalR Hizmeti Oluşturma](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-run-application.png)

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, VS Code'da gerçek zamanlı sunucusuz bir uygulama oluşturup çalıştırdın. Bir sonraki adımda Azure İşlevlerini nasıl VS Code’dan dağıtacağınızı öğrenin.

> [!div class="nextstepaction"]
> [VS Code ile Azure İşlevlerini dağıtma](/azure/javascript/tutorial-vscode-serverless-node-01)
