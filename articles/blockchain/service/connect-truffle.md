---
title: Truffle kullanarak bağlanma
description: Truffle kullanarak bir Azure blok zinciri hizmeti ağına bağlanma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 9154bc749f7db337de67f501d5e5049dfd466156
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698480"
---
# <a name="quickstart-use-truffle-to-connect-to-an-azure-blockchain-service-network"></a>Hızlı Başlangıç: Azure blok zinciri hizmeti ağına bağlanmak için Truffle kullanın

Truffle, bir Azure blok zinciri hizmeti düğümüne bağlanmak için kullanabileceğiniz bir blok zinciri geliştirme ortamıdır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Azure blok zinciri üyesi oluşturma](create-member.md)
* [Truffle](https://github.com/trufflesuite/truffle)'yi yükler. Truffle, [Node. js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)gibi çeşitli araçların yüklenmesini gerektirir.
* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/)'i yükler. Web3 için Python gereklidir.
* [Visual Studio Code](https://code.visualstudio.com/download)'i yükler.
* [Visual Studio Code Solidity uzantısını](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity)yükler.

## <a name="create-truffle-project"></a>Truffle projesi oluştur

1. Node. js komut istemi veya kabuğu açın.
1. Dizini, Truffle proje dizinini oluşturmak istediğiniz yere değiştirin.
1. Proje için bir dizin oluşturun ve yolu yeni dizin ile değiştirin. Örneğin,

    ``` bash
    mkdir truffledemo
    cd truffledemo
    ```

1. Truffle projesini başlatın.

    ``` bash
    truffle init
    ```

1. Proje klasörüne Ethereum JavaScript API Web3 'yi yükler. Şu anda sürüm Web3 sürüm 1.0.0-Beta. 37 gereklidir.

    ``` bash
    npm install web3@1.0.0-beta.37
    ```

    Yükleme sırasında NPM uyarıları alabilirsiniz.
    
## <a name="configure-truffle-project"></a>Truffle projesini yapılandırma

Truffle projesini yapılandırmak için Azure portal işlem düğümü bilgilerine ihtiyacınız vardır.

### <a name="transaction-node-endpoint-addresses"></a>İşlem düğümü uç noktası adresleri

1. Azure portal, her bir işlem düğümüne gidin ve **bağlantı dizelerini > işlem düğümleri**' ni seçin.
1. Uç nokta URL 'sini, her işlem düğümü için **https (erişim anahtarı 1)** konumundan kopyalayın ve kaydedin. Öğreticide daha sonra akıllı sözleşme yapılandırma dosyası için uç nokta adreslerine ihtiyacınız vardır.

    ![İşlem uç noktası adresi](./media/send-transaction/endpoint.png)

### <a name="edit-configuration-file"></a>Yapılandırma dosyasını Düzenle

1. Visual Studio Code başlatın ve **dosya > klasörü aç** menüsünü kullanarak Truffle proje dizin klasörünü açın.
1. Truffle yapılandırma dosyasını `truffle-config.js`açın.
1. Dosyanın içeriğini aşağıdaki yapılandırma bilgileriyle değiştirin. Uç nokta adresini içeren bir değişken ekleyin. Açılı ayracını önceki bölümlerden topladığınız değerlerle değiştirin.

    ``` javascript
    var defaultnode = "<default transaction node connection string>";   
    var Web3 = require("web3");
    
    module.exports = {
      networks: {
        defaultnode: {
          provider: new Web3.providers.HttpProvider(defaultnode),
          network_id: "*"
        }
      }
    }
    ```

1. Değişiklikleri `truffle-config.js`kaydedin.

## <a name="connect-to-transaction-node"></a>İşlem düğümüne bağlanma

İşlem düğümüne bağlanmak için *Web3* kullanın.

1. Varsayılan işlem düğümüne bağlanmak için Truffle konsolunu kullanın.

    ``` bash
    truffle console --network defaultnode
    ```

    Truffle varsayılan işlem düğümüne bağlanır ve etkileşimli bir konsol sağlar.

    İşlem düğümünüz ile etkileşim kurmak için **Web3** nesnesi üzerinde yöntemler çağırabilirsiniz.

1. Geçerli blok numarasını döndürmek için **Getblocknumber** yöntemini çağırın.

    ```bash
    web3.eth.getBlockNumber();
    ```

    Örnek çıktı:

    ```bash
    truffle(defaultnode)> web3.eth.getBlockNumber();
    18567
    ```
1. Truffle geliştirme konsolundan çıkın.

    ```bash
    .exit
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure blok zinciri hizmeti varsayılan işlem düğümünüz ile bağlantı kurmak için bir truffle projesi oluşturdunuz.

Consortium blok zinciri ağınıza bir işlem göndermek için Truffle 'yi kullanmak üzere bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [İşlem gönder](send-transaction.md)
