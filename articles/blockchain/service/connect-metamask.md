---
title: MetaMask 'i Azure blok zinciri hizmeti ağına bağlama
description: MetaMask kullanarak bir Azure blok zinciri hizmeti ağına bağlanın ve akıllı bir sözleşme dağıtın.
ms.date: 09/12/2019
ms.topic: quickstart
ms.reviewer: janders
ms.openlocfilehash: 21e45b80a9a08dd6bdeefb0ab01fadabaa08cbce
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456030"
---
# <a name="quickstart-use-metamask-to-connect-and-deploy-a-smart-contract"></a>Hızlı başlangıç: akıllı bir sözleşmeyi bağlamak ve dağıtmak için MetaMask kullanma

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti ağına bağlanmak ve akıllı bir sözleşmeyi dağıtmak için yeniden karışımı kullanmak üzere MetaMask kullanacaksınız. Metamask, bir Ether cüzdan yönetmek ve akıllı sözleşme eylemleri gerçekleştirmek için bir tarayıcı uzantısıdır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç: Azure Portal veya hızlı başlangıç kullanarak bir blok zinciri üyesi oluşturma](create-member.md) [: Azure CLI kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi](create-member-cli.md) oluşturma
* [MetaMask Browser uzantısını](https://metamask.io) yükler
* MetaMask [cüzdan](https://metamask.zendesk.com/hc/en-us/articles/360015488971-New-to-MetaMask-Learn-How-to-Setup-MetaMask-the-First-Time) oluşturma

## <a name="get-endpoint-address"></a>Uç nokta adresini al

Blok zinciri ağına bağlanmak için Azure blok zinciri hizmeti uç noktası adresine ihtiyacınız vardır. Uç nokta adresini ve erişim anahtarlarını Azure portal bulabilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.
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
1. **Dosya**altındaki **giriş** sekmesinde **yeni dosya** ' yı seçin.

    Yeni dosyayı `simple.sol`olarak adlandırın.

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
1. Sözleşmeyi derlemek için önce Solidity derleyici bölmesini seçin, sonra **Simple. Nuevo derlemesini**seçin. 

    ![Derleme](./media/connect-metamask/compile.png)

1. **Dağıtım & Çalıştır** bölmesini seçin ve ardından **ortamı** , MetaMask ile blok zinciri üyesine bağlamak üzere **eklenmiş Web3** olarak ayarlayın.

    ![Çalışma sekmesi](./media/connect-metamask/injected-web3.png)

1. **Basit** sözleşmeyi seçin ve ardından **dağıtın**.

    ![Dağıt](./media/connect-metamask/deploy.png)


1. Bir MetaMask bildirimi, işlemi gerçekleştirmek için yeterli fon olduğunu uyarır.

    Ortak bir blok zinciri ağı için, işlem maliyeti için ödeme yapmanız gerekir. Bu bir konsorsiyumde özel bir ağ olduğundan, gaz fiyatını sıfır olarak ayarlayabilirsiniz.

1.  **Gaz ücretini seçin > gelişmiş > düzenleyin**, **gaz fiyatını** 0 olarak ayarlayın.

    ![Gaz fiyatı](./media/connect-metamask/gas-price.png)

    **Kaydet**’i seçin.

1. Akıllı sözleşmeyi blok zincirine dağıtmak için **Onayla** ' yı seçin.
1. **Dağıtılan sözleşmeler** bölümünde, **basit** sözleşmeyi genişletin.

    ![Dağıtılan sözleşme](./media/connect-metamask/deployed-contract.png)

    İki eylem **ekleyin** **ve bu** , sözleşmede tanımlanan işlevlere eşleyin.

1. Blok zincirinde bir işlem **ekleme** işlemi gerçekleştirmek için, eklenecek bir sayı girin ve **Ekle**' yi seçin. Yeniden karışmaya yönelik bir gaz tahmin hatası iletisi alabilirsiniz. İşlemi, gaz gerektirmeyen özel bir blok zincirine gönderiyorsunuz. İşlemi zorlamak için **Işlem gönder** ' i seçin.
1. Sözleşmeyi dağıttığınıza benzer şekilde, işlemi gerçekleştirmek için yeterli fon MetaMask bildirimi görüntülenir.

    Bu bir konsorsiyumun özel bir ağı olduğundan, gaz fiyatını sıfıra ayarlayabiliriz.

1.  **Gaz ücretini seçin > gelişmiş > düzenleyin**, **gaz fiyatını** 0 olarak ayarlayın ve **Kaydet**' i seçin.
1. İşlemi blok zincirine göndermek için **Onayla** ' yı seçin.
1. Eylem **Al** ' ı seçin. Bu, düğüm verilerini sorgulamak için bir çağrıdır. Bir işlem gerekli değildir.
1. Yeniden karıştırma 'nın hata ayıklama bölmesinde, blok zincirindeki işlemler hakkındaki ayrıntıları görebilirsiniz.

    ![Hata ayıklama geçmişi](./media/connect-metamask/debug.png)

    **Basit sözleşme oluşturma** , **basit. Ekle**için işlem ve **basit. Get**öğesine çağrı yapabilirsiniz.

1. Ayrıca, MetaMask içinde işlem geçmişini görebilirsiniz. MetaMask Browser uzantısını açın.
1. **Geçmiş** bölümünde, dağıtılan sözleşmenin ve işlemlerin günlüğünü görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti işlem düğümüne bağlanmak, akıllı bir sözleşme dağıtmak ve blok zincirine bir işlem göndermek için MetaMask Browser uzantısını kullandınız. Bir işlem aracılığıyla akıllı sözleşme işlevi oluşturmak, derlemek, dağıtmak ve yürütmek için Ethereum ve truffle için Azure blok zinciri geliştirme setini kullanmak üzere bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetinde akıllı sözleşmeler oluşturma, derleme ve dağıtma](send-transaction.md)