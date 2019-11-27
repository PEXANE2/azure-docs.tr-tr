---
title: Azure blok zinciri hizmeti 'ne bağlanmak için Truffle kullanın
description: Truffle kullanarak bir Azure blok zinciri hizmeti ağına bağlanma
ms.date: 11/20/2019
ms.topic: quickstart
ms.reviewer: janders
ms.openlocfilehash: 9ea96aa32072775fe6fb9563442f5e3564d37ea5
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455805"
---
# <a name="quickstart-use-truffle-to-connect-to-azure-blockchain-service"></a>Hızlı başlangıç: Azure blok zinciri hizmetine bağlanmak için Truffle kullanın

Bu hızlı başlangıçta, Azure blok zinciri hizmeti işlem düğümünü kullanarak Truffle bağlantısını kullanırsınız. Daha sonra, blok zinciri ağınızla etkileşim kurmak için **Web3** yöntemlerini çağırmak üzere Truffle etkileşimli konsolunu kullanırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç: Azure Portal veya hızlı başlangıç kullanarak bir blok zinciri üyesi oluşturma](create-member.md) [: Azure CLI kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi](create-member-cli.md) oluşturma
* [Truffle](https://github.com/trufflesuite/truffle)'yi yükler. Truffle, [Node. js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)gibi çeşitli araçların yüklenmesini gerektirir.
* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/)'i yükler. Web3 için Python gereklidir.

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

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure blok zinciri hizmeti üyesine gidin. **İşlem düğümleri** ve varsayılan işlem düğümü bağlantısı ' nı seçin.

    ![Varsayılan işlem düğümünü seçin](./media/connect-truffle/transaction-nodes.png)

1. **Bağlantı dizelerini**seçin.
1. Bağlantı dizesini **https 'den (erişim anahtarı 1)** kopyalayın. Sonraki bölüm için dizeye ihtiyacınız vardır.

    ![Bağlantı dizesi](./media/connect-truffle/connection-string.png)

### <a name="edit-configuration-file"></a>Yapılandırma dosyasını Düzenle

Ardından, Truffle yapılandırma dosyasını işlem düğümü uç noktasıyla güncelleştirmeniz gerekir.

1. **Trufftademo** proje klasöründe, bir düzenleyicide `truffle-config.js` Truffle yapılandırma dosyasını açın.
1. Dosyanın içeriğini aşağıdaki yapılandırma bilgileriyle değiştirin. Uç nokta adresini içeren bir değişken ekleyin. Açılı ayracını önceki bölümden topladığınız değerlerle değiştirin.

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

1. Varsayılan işlem düğümüne bağlanmak için Truffle konsolunu kullanın. Komut isteminde veya kabukta aşağıdaki komutu çalıştırın:

    ``` bash
    truffle console --network defaultnode
    ```

    Truffle varsayılan işlem düğümüne bağlanır ve etkileşimli bir konsol sağlar.

    Blok zinciri ağınızla etkileşim kurmak için **Web3** nesnesi üzerinde yöntemler çağırabilirsiniz.

1. Geçerli blok numarasını döndürmek için **Getblocknumber** yöntemini çağırın.

    ```bash
    web3.eth.getBlockNumber();
    ```

    Örnek çıktı:

    ```bash
    truffle(defaultnode)> web3.eth.getBlockNumber();
    18567
    ```
1. Truffle konsolundan çıkın.

    ```bash
    .exit
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure blok zinciri hizmeti varsayılan işlem düğümüne bağlanın ve geçerli blok zinciri blok numarasını döndürmek için etkileşimli konsolu kullandınız.

Bir işlem aracılığıyla akıllı sözleşme işlevi oluşturmak, derlemek, dağıtmak ve yürütmek için Ethereum için Azure blok zinciri geliştirme setini kullanmak üzere bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetinde akıllı sözleşmeler oluşturma, derleme ve dağıtma](send-transaction.md)
