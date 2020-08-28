---
title: Azure SignalR hizmeti sunucusuz hızlı başlangıç-Python
description: Azure SignalR Hizmetini ve Azure İşlevlerini kullanarak bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: anthonychu
ms.service: signalr
ms.devlang: python
ms.topic: quickstart
ms.date: 12/14/2019
ms.author: antchu
ms.custom: devx-track-python
ms.openlocfilehash: d7d893d3de5541bf1108a9182dfee6ad995e061b
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89050616"
---
# <a name="quickstart-create-a-chat-room-with-azure-functions-and-signalr-service-using-python"></a>Hızlı başlangıç: Python kullanarak Azure Işlevleri ve SignalR hizmeti ile sohbet odası oluşturma

Azure SignalR hizmeti uygulamanıza kolayca gerçek zamanlı işlevsellik eklemenizi sağlar. Azure İşlevleri, herhangi bir altyapı yönetimine gerek kalmadan kodunuzu çalıştırmanıza olanak tanıyan sunucusuz bir platformdur. Bu hızlı başlangıçta, SignalR Hizmeti ve İşlevlerini sunucusuz ve gerçek zamanlı bir sohbet uygulaması oluşturmak için kullanmayı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç; macOS, Windows veya Linux üzerinde çalıştırılabilir.

[Visual Studio Code](https://code.visualstudio.com/) gibi bir kod editörünün yüklü olduğundan emin olun.

Python Azure Işlev uygulamalarını yerel olarak çalıştırmak için [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing) (sürüm 2.7.1505 veya üzeri) yüklemesini yapın.

Azure Işlevleri için [Python 3,6 veya 3,7](https://www.python.org/downloads/)gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/asrs/qspython)

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com/> sayfasında oturum açın.

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/asrs/qspython)

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/asrs/qspython)

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/asrs/qspython)

## <a name="configure-and-run-the-azure-function-app"></a>Azure İşlev Uygulamasını yapılandırıp çalıştırma

1. Azure portalın açık olduğu tarayıcıda portalın üst kısmındaki arama kutusundan adını arayarak önceden dağıttığınız SignalR Hizmeti örneğinin başarılı bir şekilde oluşturulduğundan emin olun. Açmak için örneği seçin.

    ![SignalR Hizmeti örneğini arayın](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. SignalR Hizmeti örneğinin bağlantı dizelerini görüntülemek için **Anahtarlar**’ı seçin.

1. Birincil bağlantı dizesini seçerek kopyalayın.

    ![SignalR Hizmeti Oluşturma](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-keys.png)

1. Kod Düzenleyicinizde, kopyalanmış depodaki *src/chat/Python* klasörünü açın.

1. Python işlevlerini yerel olarak geliştirmek ve test etmek için bir Python 3,6 veya 3,7 ortamında çalışmanız gerekir. Adlı bir sanal ortam oluşturmak ve etkinleştirmek için aşağıdaki komutları çalıştırın `.venv` .

    **Linux veya macOS:**

    ```bash
    python3.7 -m venv .venv
    source .venv/bin/activate
    ```

    **Pencerelerin**

    ```powershell
    py -3.7 -m venv .venv
    .venv\scripts\activate
    ```

1. *local.settings.sample.json* dosyasını *local.settings.json* olarak yeniden adlandırın.

1. **local.settings.json** dosyasının içinde bağlantı dizesini **AzureSignalRConnectionString** ayarının değerine yapıştırın. Dosyayı kaydedin.

1. Python işlevleri klasörler halinde düzenlenir. Her bir klasörde iki dosya vardır: *function.json* , işlevde kullanılan bağlamaları tanımlar ve * \_ \_ init \_ \_ . Kopyala* işlevinin gövdesidir. Bu işlev uygulamasında iki adet HTTP ile tetiklenen işlev vardır:

    - **negotiate** - Geçerli bağlantı bilgileri döndürmek için *SignalRConnectionInfo* giriş bağlamasını kullanır.
    - **messages** - İstek gövdesinde bir sohbet iletisi alır ve iletiyi bağlı olan tüm istemci uygulamalara yaymak için *SignalR* çıkış bağlamasını kullanır.

1. Sanal ortamı etkinleştirilen terminalde, *src/chat/Python* klasöründe olduğunuzdan emin olun. PıP kullanarak gerekli Python paketlerini yükler.

    ```bash
    python -m pip install -r requirements.txt
    ```

1. İşlev uygulamasını çalıştırın.

    ```bash
    func start
    ```

    ![İşlev uygulamasını Çalıştır](media/signalr-quickstart-azure-functions-python/signalr-quickstart-run-application.png)
    
[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/asrs/qspython)

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/asrs/qspython)

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/asrs/qspython)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, VS Code ' de gerçek zamanlı sunucusuz bir uygulama oluşturup çalıştırdınız. Bir sonraki adımda Azure İşlevlerini nasıl VS Code’dan dağıtacağınızı öğrenin.

> [!div class="nextstepaction"]
> [VS Code ile Azure İşlevlerini dağıtma](/azure/developer/javascript/tutorial-vscode-serverless-node-01)

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/asrs/qspython)
