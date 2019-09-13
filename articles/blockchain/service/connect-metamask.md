---
title: MetaMask 'i Azure blok zinciri hizmeti ağına bağlama
description: MetaMask kullanarak bir Azure blok zinciri hizmeti ağına bağlanın ve akıllı bir sözleşme dağıtın.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 5b46c5b2e8f613d351442fdf3c8ae5ee2198f2da
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933979"
---
# <a name="quickstart-use-metamask-to-connect-and-deploy-a-smart-contract"></a>Hızlı Başlangıç: MetaMask kullanarak akıllı bir sözleşmeyi bağlama ve dağıtma

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti ağına bağlanmak ve akıllı bir sözleşmeyi dağıtmak için yeniden karışımı kullanmak üzere MetaMask kullanacaksınız. Metamask, bir Ether cüzdan yönetmek ve akıllı sözleşme eylemleri gerçekleştirmek için bir tarayıcı uzantısıdır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç: Azure Portal](create-member.md) veya[hızlı başlangıcı kullanarak bir blok zinciri üyesi oluşturun: Azure CLı kullanarak Azure blok zinciri hizmeti blok zinciri üyesi oluşturma](create-member-cli.md)
* [MetaMask Browser uzantısını](https://metamask.io) yükler
* MetaMask [cüzdan](https://metamask.zendesk.com/hc/en-us/articles/360015488971-New-to-MetaMask-Learn-How-to-Setup-MetaMask-the-First-Time) oluşturma

## <a name="get-endpoint-address"></a>Uç nokta adresini al

Blok zinciri ağına bağlanmak için Azure blok zinciri hizmeti uç noktası adresine ihtiyacınız vardır. Uç nokta adresini ve erişim anahtarlarını Azure portal bulabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure blok zinciri hizmeti üyesine gidin. **İşlem düğümleri** ve varsayılan işlem düğümü bağlantısı ' nı seçin.

    ![Varsayılan işlem düğümünü seçin](./media/connect-metamask/transaction-nodes.png)

1. **Erişim anahtarlarına > bağlantı dizeleri**seçin.
1. Endpoint adresini **https 'den (erişim anahtarı 1)** kopyalayın. Sonraki bölüm için adrese ihtiyacınız vardır.

    ![Bağlantı dizesi](./media/connect-metamask/connection-string.png)

## <a name="connect-metamask"></a>MetaMask bağlama

1. MetaMask Browser uzantısını açın ve oturum açın.
1. Ağ açılan menüsünde **özel RPC**' yi seçin.

    ![Özel RPC](./media/connect-metamask/custom-rpc.png)

1. Yeni **ağ > yenı RPC URL 'si**' nde, önceki bölümden kopyalanmış olan bitiş noktası adresinizi girin.
1. **Kaydet**’i seçin.

    Bağlantı başarılı olduysa, ağ açılan menüsünde özel ağ görüntülenir.

    ![Yeni ağ](./media/connect-metamask/new-network.png)

## <a name="deploy-smart-contract"></a>Akıllı sözleşme dağıtma

Yeniden karıştırma, tarayıcı tabanlı bir Solidity geliştirme ortamıdır. MetaMask ve yeniden karıştır kullanarak akıllı sözleşmeleri dağıtabilir ve bunlar üzerinde işlem yapabilirsiniz.

1. Tarayıcınızda `https://remix.ethereum.org` adresine gidin.
1. **Çalıştır**'ı seçin. 

    MetaMask, **ortamınızı** ağınıza **Web3** ve **hesabı** eklemek için ayarlar.

    ![Çalışma sekmesi](./media/connect-metamask/injected-web3.png)

1. **Yeni dosya oluştur**' u seçin.

    Yeni dosyayı `simple.sol`adlandırın.

    ![Dosya oluştur](./media/connect-metamask/create-file.png)

    **Tamam**’ı seçin.

1. Yeniden karıştır düzenleyicisinde, aşağıdaki **basit akıllı sözleşme** kodunu yapıştırın.

    ```solidity
    pragma solidity ^0.5.0;
             
    contract simple {
        uint balance;
                 
        constructor() public{
            balance = 0;
        }
                 
        function add(uint _num) public {
            balance += _num;
        }
                 
        function get() public view returns (uint){
            return balance;
        }
    }
    ```

    **Basit sözleşme** , **Bakiye**adlı bir durum değişkeni bildirir. Tanımlanmış iki işlev vardır. **Add** işlevi **dengelemek**için bir sayı ekler. **Get** işlevi, **Bakiyenin**değerini döndürür.

1. Sözleşmeyi derlemek için derlemek **> derlemek Için Başlat**' ı seçin. Başarılı olursa, anlaşma adı ile yeşil bir kutu görüntülenir.

    ![Derleme](./media/connect-metamask/compile.png)

1. Sözleşmeyi yürütmek için **Çalıştır** sekmesini seçin. **Basit** sözleşmeyi seçin ve ardından **dağıtın**.

    ![Özel RPC](./media/connect-metamask/deploy.png)

1. Bir MetaMask bildirimi, işlemi gerçekleştirmek için yeterli fon olduğunu uyarır.

    Ortak bir blok zinciri ağı için, işlem maliyeti için ödeme yapmanız gerekir. Bu bir konsorsiyumde özel bir ağ olduğundan, gaz fiyatını sıfır olarak ayarlayabilirsiniz.

1.  **Gaz ücretini seçin > gelişmiş > düzenleyin**, **gaz fiyatını** 0 olarak ayarlayın.

    ![Gaz fiyatı](./media/connect-metamask/gas-price.png)

    **Kaydet**’i seçin.

1. Akıllı sözleşmeyi blok zincirine dağıtmak için **Onayla** ' yı seçin.
1. **Dağıtılan sözleşmeler** bölümünde, **basit** sözleşmeyi genişletin.

    ![Dağıtılan sözleşme](./media/connect-metamask/deployed-contract.png)

    İki eylem **ekleyin** **ve bu** , sözleşmede tanımlanan işlevlere eşleyin.

1. Blok zincirinde bir işlem **ekleme** işlemi gerçekleştirmek için, eklenecek bir sayı girin ve **Ekle**' yi seçin.
1. Sözleşmeyi dağıttığınıza benzer şekilde, işlemi gerçekleştirmek için yeterli fon MetaMask bildirimi görüntülenir.

    Bu bir konsorsiyumun özel bir ağı olduğundan, gaz fiyatını sıfıra ayarlayabiliriz.

1.  **Gaz ücretini seçin > gelişmiş > düzenleyin**, **gaz fiyatını** 0 olarak ayarlayın ve **Kaydet**' i seçin.
1. İşlemi blok zincirinde gerçekleştirmek için **Onayla** ' yı seçin.
1. Eylem **Al** ' ı seçin. Bu, düğüm verilerini sorgulamak için bir çağrıdır. Bir işlem gerekli değildir.
1. Yeniden karıştırma 'nın hata ayıklama bölmesinde, blok zincirindeki işlemler hakkındaki ayrıntıları görebilirsiniz.

    ![Hata ayıklama geçmişi](./media/connect-metamask/debug.png)

    **Basit sözleşme oluşturma** , **basit. Ekle**için işlem ve **basit. Get**öğesine çağrı yapabilirsiniz.

1. Ayrıca, MetaMask içinde işlem geçmişini görebilirsiniz. MetaMask Browser uzantısını açın.
1. **Geçmiş** bölümünde, dağıtılan sözleşmenin ve işlemlerin günlüğünü görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti işlem düğümüne bağlanmak, akıllı bir sözleşme dağıtmak ve blok zincirine bir işlem göndermek için MetaMask Browser uzantısını kullandınız. Bir işlem aracılığıyla akıllı sözleşme işlevi oluşturmak, derlemek, dağıtmak ve yürütmek için Ethereum ve truffle için Azure blok zinciri geliştirme setini kullanmak üzere bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Akıllı sözleşmeleri oluşturmak, derlemek ve dağıtmak için Visual Studio Code kullanma](send-transaction.md)