---
title: Azure blok zinciri hizmetini kullanarak işlem gönderme
description: Azure blok zinciri hizmetini kullanarak akıllı bir sözleşme dağıtma ve özel bir işlem gönderme hakkında öğretici.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 3cfbbdc5b95d1607738b132980320d2ff7c99788
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698392"
---
# <a name="tutorial-send-transactions-using-azure-blockchain-service"></a>Öğretici: Azure blok zinciri hizmetini kullanarak işlem gönderme

Bu öğreticide, sözleşme ve işlem gizliliğini test etmek için işlem düğümleri oluşturacaksınız.  Yerel bir geliştirme ortamı oluşturmak ve akıllı bir sözleşme dağıtmak ve özel bir işlem göndermek için Truffle kullanacaksınız.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * İşlem düğümleri Ekle
> * Bir akıllı sözleşme dağıtmak için Truffle kullanma
> * İşlem gönder
> * İşlem gizliliğini doğrula

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Azure Portal kullanarak bir blok zinciri üyesi oluşturma](create-member.md)
* [Hızlı başlangıç: Bir konsorsiyum ağına bağlanmak için Truffle kullanın](connect-truffle.md)
* [Truffle](https://github.com/trufflesuite/truffle)'yi yükler. Truffle, [Node. js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)gibi çeşitli araçların yüklenmesini gerektirir.
* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/)'i yükler. Web3 için Python gereklidir.
* [Visual Studio Code](https://code.visualstudio.com/Download) yüklensin
* [Visual Studio Code Solidity uzantısını](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity) yükler

## <a name="create-transaction-nodes"></a>İşlem düğümleri oluştur

Varsayılan olarak, bir işlem düğümünüz vardır. İki tane daha ekleyeceğiz. Düğümlerden biri özel işleme katılır. Diğeri özel işleme dahil değildir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure blok zinciri üyesine gidin ve **ekle > işlem düğümleri**' ni seçin.
1. Adlı `alpha`yeni bir işlem düğümü için ayarları doldurun.

    ![İşlem düğümü oluştur](./media/send-transaction/create-node.png)

    | Ayar | Değer | Açıklama |
    |---------|-------|-------------|
    | Ad | `alpha` | İşlem düğümü adı. Ad, işlem düğümü uç noktasının DNS adresini oluşturmak için kullanılır. Örneğin: `alpha-mymanagedledger.blockchain.azure.com`. |
    | istemcisiyle yönetilen bir cihaz için) | Güçlü parola | Parola, temel kimlik doğrulamasıyla işlem düğümü uç noktasına erişmek için kullanılır.

1. **Oluştur**’u seçin.

    Yeni bir işlem düğümü sağlanması yaklaşık 10 dakika sürer.

1. Adlı `beta`bir işlem düğümü eklemek için 2 ile 4 arasındaki adımları tekrarlayın.

Düğümler sağlandığında öğreticiye devam edebilirsiniz. Sağlama tamamlandığında, üç işlem düğümünüz olur.

## <a name="open-truffle-console"></a>Truffle konsolunu açın

1. Node. js komut istemi veya kabuğu açın.
1. Önkoşul [hızlı başlangıcı ' ndan Truffle proje dizini için yolu değiştirin: Bir konsorsiyum ağına](connect-truffle.md)bağlanmak Için Truffle kullanın. Örneğin,

    ```bash
    cd truffledemo
    ```

1. Varsayılan işlem düğümüne bağlanmak için Truffle konsolunu kullanın.

    ``` bash
    truffle console --network defaultnode
    ```

    Truffle varsayılan işlem düğümüne bağlanır ve etkileşimli bir konsol sağlar.

## <a name="create-ethereum-account"></a>Ethereum hesabı oluştur

Varsayılan işlem düğümüne bağlanmak ve bir Ethereum hesabı oluşturmak için Web3 kullanın. İşlem düğümünüz ile etkileşim kurmak için Web3 nesnesi üzerinde yöntemler çağırabilirsiniz.

1. Varsayılan işlem düğümünde yeni bir hesap oluşturun. Password parametresini kendi güçlü parolanızla değiştirin.

    ```bash
    web3.eth.personal.newAccount("1@myStrongPassword");
    ```

    Döndürülen hesap adresini ve parolayı unutmayın. Sonraki bölümde Ethereum hesap adresi ve parolasının olması gerekir.

1. Truffle geliştirme ortamından çıkın.

    ```bash
    .exit
    ```

## <a name="configure-truffle-project"></a>Truffle projesini yapılandırma

Truffle projesini yapılandırmak için Azure portal işlem düğümü bilgilerine ihtiyacınız vardır.

### <a name="transaction-node-public-key"></a>İşlem düğümü ortak anahtarı

Her işlem düğümünün bir ortak anahtarı vardır. Ortak anahtar, düğüme özel bir işlem göndermenizi sağlar. Varsayılan işlem düğümünden *Alpha* işlem düğümüne bir işlem göndermek için *Alfa* işlem düğümünün ortak anahtarı gerekir.

Ortak anahtarı işlem düğümü listesinden alabilirsiniz. Alfa düğümünün ortak anahtarını kopyalayın ve daha sonra öğreticide değerini kaydedin.

![İşlem düğüm listesi](./media/send-transaction/node-list.png)

### <a name="transaction-node-endpoint-addresses"></a>İşlem düğümü uç noktası adresleri

1. Azure portal, her bir işlem düğümüne gidin ve **bağlantı dizelerini > işlem düğümleri**' ni seçin.
1. Uç nokta URL 'sini, her işlem düğümü için **https (erişim anahtarı 1)** konumundan kopyalayın ve kaydedin. Öğreticide daha sonra akıllı sözleşme yapılandırma dosyası için uç nokta adreslerine ihtiyacınız vardır.

    ![İşlem uç noktası adresi](./media/send-transaction/endpoint.png)

### <a name="edit-configuration-file"></a>Yapılandırma dosyasını Düzenle

1. Visual Studio Code başlatın ve **dosya > klasörü aç** menüsünü kullanarak Truffle proje dizin klasörünü açın.
1. Truffle yapılandırma dosyasını `truffle-config.js`açın.
1. Dosyanın içeriğini aşağıdaki yapılandırma bilgileriyle değiştirin. Uç nokta adreslerini ve hesap bilgilerini içeren değişkenler ekleyin. Açılı ayraç bölümlerini önceki bölümlerden topladığınız değerlerle değiştirin.

    ``` javascript
    var defaultnode = "<default transaction node connection string>";
    var alpha = "<alpha transaction node connection string>";
    var beta = "<beta transaction node connection string>";
    
    var myAccount = "<Ethereum account address>";
    var myPassword = "<Ethereum account password>";
    
    var Web3 = require("web3");
    
    module.exports = {
      networks: {
        defaultnode: {
          provider:(() =>  {
          const AzureBlockchainProvider = new Web3.providers.HttpProvider(defaultnode);
    
          const web3 = new Web3(AzureBlockchainProvider);
          web3.eth.personal.unlockAccount(myAccount, myPassword);
    
          return AzureBlockchainProvider;
          })(),
    
          network_id: "*",
          gasPrice: 0,
          from: myAccount
        },
        alpha: {
          provider: new Web3.providers.HttpProvider(alpha),
          network_id: "*",
        },
        beta: {
          provider: new Web3.providers.HttpProvider(beta),
          network_id: "*",
        }
      },
      compilers: {
        solc: {
          evmVersion: "byzantium"
        }
      }
    }
    ```

1. Değişiklikleri `truffle-config.js`kaydedin.

## <a name="create-smart-contract"></a>Akıllı sözleşme oluştur

1. **Sözleşmeler** klasöründe adlı `SimpleStorage.sol`yeni bir dosya oluşturun. Aşağıdaki kodu ekleyin.

    ```solidity
    pragma solidity >=0.4.21 <0.6.0;
    
    contract SimpleStorage {
        string public storedData;
    
        constructor(string memory initVal) public {
            storedData = initVal;
        }
    
        function set(string memory x) public {
            storedData = x;
        }
    
        function get() view public returns (string memory retVal) {
            return storedData;
        }
    }
    ```
    
1. **Geçişler** klasöründe adlı `2_deploy_simplestorage.js`yeni bir dosya oluşturun. Aşağıdaki kodu ekleyin.

    ```solidity
    var SimpleStorage = artifacts.require("SimpleStorage.sol");
    
    module.exports = function(deployer) {
    
      // Pass 42 to the contract as the first constructor parameter
      deployer.deploy(SimpleStorage, "42", {privateFor: ["<alpha node public key>"], from:"<Ethereum account address>"})  
    };
    ```

1. Açılı ayraçların içindeki değerleri değiştirin.

    | Değer | Açıklama
    |-------|-------------
    | \<alpha node public key\> | Alfa düğümünün ortak anahtarı
    | \<Ethereum account address\> | Varsayılan işlem düğümünde oluşturulan Ethereum hesabı adresi

    Bu örnekte, **Storedata** değerinin ilk değeri 42 olarak ayarlanmıştır.

    **Privatefor** , sözleşmenin kullanılabildiği düğümleri tanımlar. Bu örnekte, varsayılan işlem düğümünün hesabı özel işlemleri **Alfa** düğümüne ekleyebilir. Tüm özel işlem katılımcıları için ortak anahtarlar eklersiniz. **Privatefor:** ve **From:** dahil değilseniz, akıllı sözleşme işlemleri geneldir ve tüm Consortium üyeleri tarafından görülebilir.

1. **Dosya > Tümünü Kaydet**' i seçerek tüm dosyaları kaydedin.

## <a name="deploy-smart-contract"></a>Akıllı sözleşme dağıtma

Varsayılan işlem düğümü ağına dağıtmak `SimpleStorage.sol` için Truffle kullanın.

```bash
truffle migrate --network defaultnode
```

Truffle ilk olarak derlenir ve sonra **Simplestorage** akıllı sözleşmesini dağıtır.

Örnek çıktı:

```
admin@desktop:/mnt/c/truffledemo$ truffle migrate --network defaultnode

2_deploy_simplestorage.js
=========================

   Deploying 'SimpleStorage'
   -------------------------
   > transaction hash:    0x3f695ff225e7d11a0239ffcaaab0d5f72adb545912693a77fbfc11c0dbe7ba72
   > Blocks: 2            Seconds: 12
   > contract address:    0x0b15c15C739c1F3C1e041ef70E0011e641C9D763
   > account:             0x1a0B9683B449A8FcAd294A01E881c90c734735C3
   > balance:             0
   > gas used:            0
   > gas price:           0 gwei
   > value sent:          0 ETH
   > total cost:          0 ETH


   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:                   0 ETH


Summary
=======
> Total deployments:   2
> Final cost:          0 ETH
```

## <a name="validate-contract-privacy"></a>Sözleşme gizliliğini doğrula

Sözleşme gizliliği nedeniyle, sözleşme değerleri yalnızca **Privatefor**ile bildiridiğimiz düğümlerden sorgulanabilir. Bu örnekte, hesap bu düğümde bulunduğundan varsayılan işlem düğümünü sorgulayabiliriz. 

1. Truffle konsolunu kullanarak varsayılan işlem düğümüne bağlanın.

    ```bash
    truffle console --network defaultnode
    ```

1. Truffle konsolunda, sözleşme örneğinin değerini döndüren kodu yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    Varsayılan işlem düğümünü sorgulamak başarılı olursa 42 değeri döndürülür. Örneğin:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network defaultnode
    truffle(defaultnode)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    '42'
    ```

1. Truffle konsolundan çıkın.

    ```bash
    .exit
    ```

Için Privatenode 'un ortak **anahtarını bildirdiğimiz** **için**, **Alfa** düğümünü sorgulayabilir.

1. Truffle konsolunu kullanarak **Alfa** düğümüne bağlanın.

    ```bash
    truffle console --network alpha
    ```

1. Truffle konsolunda, sözleşme örneğinin değerini döndüren kodu yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    **Alfa** düğümünü sorgulamak başarılı olursa 42 değeri döndürülür. Örneğin:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network alpha
    truffle(alpha)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    '42'
    ```

1. Truffle konsolundan çıkın.

    ```bash
    .exit
    ```

**Privatefor için** **Beta** düğümünün ortak anahtarını bildirmedik, ancak sözleşme gizliliği nedeniyle **Beta** düğümünü sorgulayamayacağız.

1. Truffle konsolunu kullanarak **Beta** düğümüne bağlanın.

    ```bash
    truffle console --network beta
    ```

1. Sözleşme örneğinin değerini döndüren bir kod yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

1. Bir sözleşmenin özel olması bu yana **Beta** düğümünü sorgulama başarısız olur. Örneğin:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network beta
    truffle(beta)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    Thrown:
    Error: Returned values aren't valid, did it run Out of Gas?
        at XMLHttpRequest._onHttpResponseEnd (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:345:8)
        at XMLHttpRequest._setReadyState (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:219:8)
        at XMLHttpRequestEventTarget.dispatchEvent (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request-event-target.ts:44:13)
        at XMLHttpRequest.request.onreadystatechange (/mnt/c/truffledemo/node_modules/web3-providers-http/src/index.js:96:13)
    ```

1. Truffle konsolundan çıkın.

    ```bash
    .exit
    ```
    
## <a name="send-a-transaction"></a>İşlem gönder

1. Adlı `sampletx.js`bir dosya oluşturun. Bunu projenizin köküne kaydedin.
1. Aşağıdaki betik, Contract **storedData** değişken değerini 65 olarak ayarlar. Kodu yeni dosyaya ekleyin.

    ```javascript
    var SimpleStorage = artifacts.require("SimpleStorage");
    
    module.exports = function(done) {
      console.log("Getting deployed version of SimpleStorage...")
      SimpleStorage.deployed().then(function(instance) {
        console.log("Setting value to 65...");
        return instance.set("65", {privateFor: ["<alpha node public key>"], from:"<Ethereum account address>"});
      }).then(function(result) {
        console.log("Transaction:", result.tx);
        console.log("Finished!");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

    Açılı ayraçlar içindeki değerleri değiştirin ve sonra dosyayı kaydedin.

    | Value | Açıklama
    |-------|-------------
    | \<alpha node public key\> | Alfa düğümünün ortak anahtarı
    | \<Ethereum account address\> | Varsayılan işlem düğümünde Ethereum hesap adresi oluşturuldu.

    **Privatefor** , işlemin kullanılabildiği düğümleri tanımlar. Bu örnekte, varsayılan işlem düğümünün hesabı özel işlemleri **Alfa** düğümüne ekleyebilir. Tüm özel işlem katılımcıları için ortak anahtarlar eklemeniz gerekir.

1. Komut dosyasını varsayılan işlem düğümü için yürütmek üzere Truffle kullanın.

    ```bash
    truffle exec sampletx.js --network defaultnode
    ```

1. Truffle konsolunda, sözleşme örneğinin değerini döndüren kodu yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    İşlem başarılı olduysa 65 değeri döndürülür. Örneğin:
    
    ```
    Getting deployed version of SimpleStorage...
    Setting value to 65...
    Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
    Finished!
    ```

1. Truffle konsolundan çıkın.

    ```bash
    .exit
    ```
    
## <a name="validate-transaction-privacy"></a>İşlem gizliliğini doğrula

İşlem gizliliği nedeniyle, işlemler yalnızca **Privatefor**ile bildirdiğiniz düğümlerde gerçekleştirilebilir. Bu örnekte, **Privatefor** **Alpha** düğümünün ortak anahtarını bildirdiğimiz için işlem gerçekleştirebiliriz. 

1. İşlemi **Alfa** düğümünde yürütmek Için Truffle kullanın.

    ```bash
    truffle exec sampletx.js --network alpha
    ```
    
1. Sözleşme örneğinin değerini döndüren kodu yürütün.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```
    
    İşlem başarılı olduysa 65 değeri döndürülür. Örneğin:

    ```
    Getting deployed version of SimpleStorage...
    Setting value to 65...
    Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
    Finished!
    ```
    
1. Truffle konsolundan çıkın.

    ```bash
    .exit
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, Azure blok zinciri hizmeti tarafından oluşturduğunuz `myResourceGroup` kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portal sol gezinti bölmesindeki **kaynak grubu** ' na gidin ve silmek istediğiniz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin. Kaynak grubu adını girip **Sil**' i seçerek silmeyi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, sözleşme ve işlem gizliliğini göstermek için iki işlem düğümü eklediniz. Özel bir akıllı sözleşme dağıtmak için varsayılan düğümü kullandınız. , Sözleşme değerlerini sorgulayarak ve blok zincirinde işlem gerçekleştirerek gizliliği test edersiniz.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetini kullanarak blok zinciri uygulamaları geliştirme](develop.md)
