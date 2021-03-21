---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 33212f14faa0e7c201c13474af981790c01284f3
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "104598822"
---
## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu dizine gidin.

```console
mkdir calling-quickstart && cd calling-quickstart
```

`npm init -y`Dosya **üzerinde** varsayılan ayarlarla birpackage.jsoluşturmak için öğesini çalıştırın.

```console
npm init -y
```

### <a name="install-the-package"></a>Paketi yükler

`npm install`JavaScript için istemci kitaplığı 'Nı çağıran Azure Iletişim hizmetlerini yüklemek için komutunu kullanın.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

Bu hızlı başlangıç için aşağıdaki WebPack sürümleri önerilir:

```console
"webpack": "^4.42.0",
"webpack-cli": "^3.3.11",
"webpack-dev-server": "^3.10.3"
```

`--save`Seçeneği, kitaplığı dosyadaki **package.js** bir bağımlılık olarak listeler.

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Bu hızlı başlangıç, uygulama varlıklarını paketleyip Web Pack kullanır. WebPack, WebPack-CLI ve WebPack-dev-Server NPM paketlerini yüklemek için aşağıdaki komutu çalıştırın ve bunları **üzerindepackage.js** geliştirme bağımlılıkları olarak listeleyin:

```console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```

Projenizin kök dizininde bir **index.html** dosyası oluşturun. Bu dosyayı, kullanıcının çağrı yerleştirmesini sağlayacak temel bir düzen yapılandırmak için kullanacağız.
