---
title: Azure blok zinciri hizmeti 'nde akıllı sözleşmeleri kullanma
description: Azure blok zinciri hizmeti 'ni kullanarak akıllı bir sözleşme dağıtmayı ve bir işlem aracılığıyla işlev yürütmeyi gösteren öğretici.
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 07/31/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: chrisseg
ms.openlocfilehash: 1843bd66e11a6686c9ae81fb8e30c7b030e889b7
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705122"
---
# <a name="tutorial-use-smart-contracts-on-azure-blockchain-service"></a>Öğretici: Azure blok zinciri hizmeti 'nde akıllı sözleşmeleri kullanma

Bu öğreticide, bir akıllı sözleşme oluşturup dağıtmak ve ardından bir konsorsiyum blok zinciri ağı üzerinde bir işlem aracılığıyla akıllı sözleşme işlevini yürütmek için Ethereum için Azure blok zinciri geliştirme setini kullanacaksınız.

Ethereum için Azure blok zinciri geliştirme setini şu şekilde kullanabilirsiniz:

> [!div class="checklist"]
> * Azure blok zinciri hizmeti konsorsiyum blok zinciri üyesine bağlanma
> * Akıllı sözleşme oluşturma
> * Akıllı sözleşme dağıtma
> * Bir işlem aracılığıyla akıllı sözleşme işlevini yürütme
> * Sözleşme durumunu sorgula

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç: Azure Portal](create-member.md) veya[hızlı başlangıcı kullanarak bir blok zinciri üyesi oluşturun: Azure CLı kullanarak Azure blok zinciri hizmeti blok zinciri üyesi oluşturma](create-member-cli.md)
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Ethereum uzantısı için Azure blok zinciri geliştirme seti](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Node.js](https://nodejs.org)
* [Git](https://git-scm.com)
* [Python](https://www.python.org/downloads/release/python-2715/). Yolunuza Python. exe ' yi ekleyin. Azure blok zinciri geliştirme seti için yolunuzda Python gereklidir.
* [Truffle](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLı](https://github.com/trufflesuite/ganache-cli)

### <a name="verify-azure-blockchain-development-kit-environment"></a>Azure blok zinciri geliştirme seti ortamını doğrulama

Azure blok zinciri geliştirme seti, geliştirme ortamı önkoşullarının karşılandığını doğrular. Geliştirme ortamınızı doğrulamak için:

VS Code komut paletinden Azure blok zinciri ' **ni seçin: Karşılama sayfasını**göster.

Azure blok zinciri geliştirme seti, tamamlanmasının yaklaşık bir dakika süren bir doğrulama betiği çalıştırır. Bu çıktıyı, **terminal > yeni Terminal**' i seçerek görüntüleyebilirsiniz. Terminal menü çubuğunda, açılan menüde **Çıkış** sekmesini ve **Azure blok zincirini** seçin. Başarılı doğrulama aşağıdaki görüntüde olduğu gibi görünür:

![Geçerli geliştirme ortamı](./media/send-transaction/valid-environment.png)

 Gerekli bir aracı eksik ise, **Azure blok zinciri geliştirme seti-önizleme** adlı yeni bir sekme, yüklenecek gerekli uygulamaları ve araçları indirmek için bağlantıları listeler.

![Geliştirme Seti gerekli uygulamalar](./media/send-transaction/required-apps.png)

## <a name="connect-to-consortium-member"></a>Consortium üyesine Bağlan

Azure blok zinciri geliştirme seti VS Code uzantısını kullanarak, konsorsiyum üyelerine bağlanabilirsiniz. Bir konsorsiyumun bağlantısı kurulduktan sonra, akıllı sözleşmeleri bir Azure blok zinciri hizmeti Consortium üyesine derleyebilir, oluşturabilir ve dağıtabilirsiniz.

Azure blok zinciri hizmeti Consortium üyesine erişiminiz yoksa, önkoşul [hızlı başlangıcı ' nı doldurun: Azure Portal](create-member.md) veya[hızlı başlangıcı kullanarak bir blok zinciri üyesi oluşturun: Azure CLı](create-member-cli.md)kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi oluşturun.

1. Visual Studio Code (VS Code) gezgin bölmesinde, **Azure blok zinciri** uzantısını genişletin.
1. **Konsorsiya Bağlan**' ı seçin.

   ![Konsorsiyumun bağlantısını yapın](./media/send-transaction/connect-consortium.png)

    Azure kimlik doğrulaması istenirse, bir tarayıcı kullanarak kimlik doğrulaması yapmak için istemleri izleyin.
1. Komut paleti açılan menüsünde **Azure blok zinciri hizmeti Consortium 'A Bağlan** ' ı seçin.
1. Azure blok zinciri hizmeti Consortium üyele ilişkili aboneliği ve kaynak grubunu seçin.
1. Listeden Consortium ' ı seçin.

Konsorsiyum ve blok zinciri üyeleri, Visual Studio Gezgini yan çubuğunda listelenir.

![Gezgin 'de görünen konsorsiyum](./media/send-transaction/consortium-node.png)

## <a name="create-a-smart-contract"></a>Akıllı sözleşme oluşturma

Ethereum için Azure blok zinciri geliştirme seti, sözleşmeleri dolandırmaya, oluşturmaya ve dağıtmanıza yardımcı olmak için proje şablonları ve truffle araçlarını kullanır.

1. VS Code komut paletinden Azure blok zinciri ' **ni seçin: Yeni Solidity projesi**.
1. **Temel proje oluştur**seçeneğini belirleyin.
1. Adlı `HelloBlockchain` yeni bir klasör oluşturun ve **Yeni proje yolu**' nu seçin.

Azure blok zinciri geliştirme seti sizin için yeni bir Solidity projesi oluşturur ve başlatır. Temel proje, örnek bir **Helloblockzincirine** akıllı sözleşme ve Azure blok zinciri hizmeti 'nde Consortium üyelemenize dağıtmak için gereken tüm dosyaları içerir. Projenin oluşturulması birkaç dakika sürebilir. Azure blok zinciri için çıktıyı seçerek VS Code 'ın Terminal panelinde ilerleme durumunu izleyebilirsiniz.

Proje yapısı aşağıdaki örneğe benzer şekilde görünür:

   ![Solidity projesi](./media/send-transaction/solidity-project.png)

## <a name="build-a-smart-contract"></a>Akıllı sözleşme oluşturma

Akıllı sözleşmeler, projenin **sözleşmeler** dizininde bulunur. Akıllı sözleşmeleri bir blok zincirine dağıtmadan önce derleyebilirsiniz. Projenizdeki tüm akıllı sözleşmeleri derlemek için, **Yapı sözleşmeleri** komutunu kullanın.

1. VS Code Gezgini kenar çubuğunda, projenizdeki **sözleşmeler** klasörünü genişletin.
1. **Helloblockzincirine. Nuevo** öğesine sağ tıklayın ve menüden **sözleşmeleri oluştur** ' u seçin.

    ![Yapı sözleşmeleri](./media/send-transaction/build-contracts.png)

Azure blok zinciri geliştirme seti, akıllı sözleşmeleri derlemek için Truffle kullanır.

![Derleme çıkışı](./media/send-transaction/compile-output.png)

## <a name="deploy-a-smart-contract"></a>Akıllı sözleşme dağıtma

Truffle, sözleşmelerinizi bir Ethereum ağına dağıtmak için geçiş betikleri kullanır. Geçişler, projenin **geçiş** dizininde bulunan JavaScript dosyalarıdır.

1. Akıllı sözleşmenizi dağıtmak için, **Helloblockzincirine. Nuevo** öğesine sağ tıklayın ve menüden **sözleşmeleri dağıt** ' ı seçin.
1. **Truffle-config. js**altında Azure blok zinciri Konsorsiyumu ağınızı seçin. Projeyi oluştururken, konsorsiyum blok zinciri ağı projenin Truffle yapılandırma dosyasına eklendi.
1. **Anımsatıcı üret**' i seçin. Bir dosya adı seçin ve anımsatıcı dosyasını proje klasörüne kaydedin. Örneğin: `myblockchainmember.env`. Anımsatıcı dosyası, blok zinciri üyesiyseniz bir Ethereum özel anahtarı oluşturmak için kullanılır.

Azure blok zinciri geliştirme seti, sözleşmeleri blok zincirine dağıtmak üzere geçiş betiğini yürütmek için Truffle kullanır.

![Sözleşme başarıyla dağıtıldı](./media/send-transaction/deploy-contract.png)

## <a name="call-a-contract-function"></a>Sözleşme işlevini çağırma

**Helloblockzincirleri** sözleşmesinin **SendRequest** işlevi **RequestMessage** durum değişkenini değiştirir. Bir blok zinciri ağının durumunun değiştirilmesi bir işlem aracılığıyla yapılır. **SendRequest** işlevini bir işlem aracılığıyla yürütmek için bir komut dosyası oluşturabilirsiniz.

1. Truffle projenizin kökünde yeni bir dosya oluşturun ve bunu `sendrequest.js`adlandırın. Aşağıdaki Web3 JavaScript kodunu dosyaya ekleyin.

    ```javascript
    var HelloBlockchain = artifacts.require("HelloBlockchain");
        
    module.exports = function(done) {
      console.log("Getting the deployed version of the HelloBlockchain smart contract")
      HelloBlockchain.deployed().then(function(instance) {
        console.log("Calling SendRequest function for contract ", instance.address);
        return instance.SendRequest("Hello, blockchain!");
      }).then(function(result) {
        console.log("Transaction hash: ", result.tx);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. Azure blok zinciri geliştirme seti bir proje oluşturduğunda, Truffle yapılandırma dosyası, Consortium blok zinciri ağ uç noktası ayrıntılarınız ile oluşturulur. **Truffle-config. js** ' i projenizde açın. Yapılandırma dosyasında iki ağ listelenir: bir tane geliştirme ve bir tane, konsorsiyum ile aynı ada sahip.
1. VS Code Terminal bölmesinde, komut dosyasını Konsorsiyumu blok zinciri ağınızda yürütmek için Truffle ' yi kullanın. Terminal bölmesi menü çubuğunda açılan menüde **Terminal** sekmesini ve **PowerShell** ' i seçin.

    ```PowerShell
    truffle exec sendrequest.js --network <blockchain network>
    ```

    Blok \<zinciri ağını\> , **Truffle-config. js**' de tanımlanan blok zinciri ağının adıyla değiştirin.

Truffle, betiği blok zinciri ağınızda yürütür.

![Betik çıktısı](./media/send-transaction/execute-transaction.png)

Bir sözleşmenin işlevini bir işlem aracılığıyla yürüttüğünüzde, bir blok oluşturuluncaya kadar işlem işlenmez. Bir işlem yoluyla yürütülmesi amaçlanan işlevler, dönüş değeri yerine bir işlem KIMLIĞI döndürür.

## <a name="query-contract-state"></a>Sözleşme durumunu sorgula

Akıllı sözleşme işlevleri, durum değişkenlerinin geçerli değerini döndürebilir. Bir durum değişkeninin değerini döndürecek bir işlev ekleyelim.

1. **Helloblockzincirine. Nuevo**Içinde, **helloblockzincirine** akıllı sözleşmeye bir **GetMessage** işlevi ekleyin.

    ``` solidity
    function getMessage() public view returns (string memory)
    {
        if (State == StateType.Request)
            return RequestMessage;
        else
            return ResponseMessage;
    }
    ```

    İşlevi, sözleşmenin geçerli durumuna bağlı olarak bir durum değişkeninde depolanan iletiyi döndürür.

1. Akıllı sözleşmede değişiklikleri derlemek için **Helloblockzincirine. Nuevo** öğesine sağ tıklayın ve menüden **sözleşmeleri derle** ' yi seçin.
1. Dağıtmak için, **Helloblockzincirine. Nuevo** öğesine sağ tıklayın ve menüden **sözleşmeleri dağıt** ' ı seçin.
1. Sonra, **GetMessage** işlevini çağırmak için kullanarak bir betik oluşturun. Truffle projenizin kökünde yeni bir dosya oluşturun ve bunu `getmessage.js`adlandırın. Aşağıdaki Web3 JavaScript kodunu dosyaya ekleyin.

    ```javascript
    var HelloBlockchain = artifacts.require("HelloBlockchain");
    
    module.exports = function(done) {
      console.log("Getting the deployed version of the HelloBlockchain smart contract")
      HelloBlockchain.deployed().then(function(instance) {
        console.log("Calling getMessage function for contract ", instance.address);
        return instance.getMessage();
      }).then(function(result) {
        console.log("Request message value: ", result);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. VS Code, Terminal bölmesinde, blok zinciri ağınızda betiği yürütmek için Truffle kullanın. Terminal bölmesi menü çubuğunda açılan menüde **Terminal** sekmesini ve **PowerShell** ' i seçin.

    ```bash
    truffle exec getmessage.js --network <blockchain network>
    ```

    Blok \<zinciri ağını\> , **Truffle-config. js**' de tanımlanan blok zinciri ağının adıyla değiştirin.

Betik, getMessage işlevini çağırarak akıllı sözleşmeyi sorgular. **RequestMessage** durum değişkeninin geçerli değeri döndürüldü.

![Betik çıktısı](./media/send-transaction/execute-get.png)

Değerin **Merhaba, blok zinciri!** olmadığına dikkat edin. Bunun yerine, döndürülen değer bir yer tutucudur. Sözleşmeyi değiştirdiğinizde ve dağıttığınızda, sözleşme yeni bir sözleşme adresi alır ve durum değişkenlerine akıllı sözleşme oluşturucusunda değerler atanır. Truffle örnek **2_deploy_contracts. js** geçiş betiği, akıllı sözleşmeyi dağıtır ve bir yer tutucu değerini bağımsız değişken olarak geçirir. Oluşturucu **RequestMessage** durum değişkenini yer tutucu değerine ayarlar ve döndürülen değer.

1. **RequestMessage** durum değişkenini ayarlamak ve değeri sorgulamak için **SendRequest. js** ve **GetMessage. js** betiklerini yeniden çalıştırın.

    ![Betik çıktısı](./media/send-transaction/execute-set-get.png)

    **SendRequest. js** , **RequestMessage** durum değişkenini **Merhaba, blok zinciri!** olarak ayarlar ve **GetMessage. js** , **isteği RequestMessage** durum değişkeninin değeri için sorgular ve **Hello, blok zinciri!** döndürür.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, *blok zinciri üye önkoşulu oluştur* hızlı başlangıcı ' nda `myResourceGroup` oluşturduğunuz kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portal sol gezinti bölmesindeki **kaynak grubu** ' na gidin ve silmek istediğiniz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin. Kaynak grubu adını girip **Sil**' i seçerek silmeyi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure blok zinciri geliştirme seti 'ni kullanarak örnek bir Solidity projesi oluşturdunuz. Azure blok zinciri hizmeti 'nde barındırılan bir blok zinciri konsorsiyum ağı üzerinde bir işlem aracılığıyla işlev olarak adlandırılan akıllı bir sözleşmeyi oluşturup dağıttığınız.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetini kullanarak blok zinciri uygulamaları geliştirme](develop.md)
