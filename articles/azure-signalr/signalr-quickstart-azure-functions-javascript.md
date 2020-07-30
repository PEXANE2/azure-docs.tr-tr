---
title: JavaScript kullanarak Azure Işlevleri ve SignalR hizmeti ile bir sohbet odası oluşturun
description: Azure SignalR Hizmetini ve Azure İşlevlerini kullanarak bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: javascript
ms.topic: quickstart
ms.date: 12/14/2019
ms.author: zhshang
ms.custom: devx-track-javascript
ms.openlocfilehash: 3f1051b81666a036203eb1c810875d837ef9816f
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87386886"
---
# <a name="quickstart-use-javascript-to-create-a-chat-room-with-azure-functions-and-signalr-service"></a>Hızlı başlangıç: Azure Işlevleri ve SignalR hizmeti ile sohbet odası oluşturmak için JavaScript kullanma

Azure SignalR hizmeti, uygulamanıza kolayca gerçek zamanlı işlevselliği eklemenizi sağlar ve Azure Işlevleri, herhangi bir altyapıyı yönetmeden kodunuzu çalıştırmanıza olanak tanıyan sunucusuz bir platformdur. Bu hızlı başlangıçta, SignalR hizmeti ve Işlevleri 'ni kullanarak sunucusuz, gerçek zamanlı bir sohbet uygulaması oluşturmak için JavaScript 'ı kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio Code](https://code.visualstudio.com/) gibi bir kod Düzenleyicisi
- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing), sürüm 2 veya üzeri. Azure Function uygulamalarını yerel olarak çalıştırmak için kullanılır.
- [Node.js](https://nodejs.org/en/download/), sürüm 10. x

   > [!NOTE]
   > Örneklerin diğer Node.js sürümleriyle çalışması gerekir. daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanı sürümleri belgeleri](../azure-functions/functions-versions.md#languages) .

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

1. Kod Düzenleyicinizde, kopyalanmış depodaki *src/chat/JavaScript* klasörünü açın.

1. *local.settings.sample.json* dosyasını *local.settings.json* olarak yeniden adlandırın.

1. **local.settings.json** dosyasının içinde bağlantı dizesini **AzureSignalRConnectionString** ayarının değerine yapıştırın. Dosyayı kaydedin.

1. JavaScript işlevleri klasörler halinde düzenlenir. Her klasörde iki dosya bulunur: *function.json* işlevde kullanılan bağlamaları tanımlar ve *index.js* işlevin gövdesidir. Bu işlev uygulamasında iki adet HTTP ile tetiklenen işlev vardır:

    - **negotiate** - Geçerli bağlantı bilgileri döndürmek için *SignalRConnectionInfo* giriş bağlamasını kullanır.
    - **messages** - İstek gövdesinde bir sohbet iletisi alır ve iletiyi bağlı olan tüm istemci uygulamalara yaymak için *SignalR* çıkış bağlamasını kullanır.

1. Terminalde, *src/chat/JavaScript* klasöründe olduğunuzdan emin olun. İşlev uygulamasını çalıştırın.

    ```bash
    func start
    ```

    ![SignalR Hizmeti Oluşturma](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-run-application.png)

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, VS Code ' de gerçek zamanlı sunucusuz bir uygulama oluşturup çalıştırdınız. Bir sonraki adımda Azure İşlevlerini nasıl VS Code’dan dağıtacağınızı öğrenin.

> [!div class="nextstepaction"]
> [VS Code ile Azure İşlevlerini dağıtma](/azure/developer/javascript/tutorial-vscode-serverless-node-01)
