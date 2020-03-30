---
title: MetaMask'i azure blockchain servis ağına bağlayın
description: MetaMask'i kullanarak bir Azure Blockchain Servis ağına bağlanın ve akıllı bir sözleşme dağıtın.
ms.date: 09/12/2019
ms.topic: quickstart
ms.reviewer: janders
ms.openlocfilehash: c0bad9efde44ce53f6b0656af3ac4af32ffe051d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79205104"
---
# <a name="quickstart-use-metamask-to-connect-and-deploy-a-smart-contract"></a>Quickstart: Akıllı bir sözleşmeyi bağlamak ve dağıtmak için MetaMask'i kullanın

Bu hızlı başlangıçta, bir Azure Blockchain Hizmet ağına bağlanmak için MetaMask'ı ve akıllı bir sözleşme dağıtmak için Remix'i kullanacaksınız. Metamask bir Eter cüzdan yönetmek ve akıllı sözleşme eylemleri gerçekleştirmek için bir tarayıcı uzantısıdır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Tam [Hızlı Başlatma: Azure portalını veya Quickstart'ı kullanarak blockchain üyesi oluşturun:](create-member.md) [Azure CLI'yi kullanarak bir Azure Blockchain Hizmeti blockchain üyesi oluşturun](create-member-cli.md)
* [MetaMask tarayıcı uzantısını](https://metamask.io) yükleme
* MetaMask [cüzdan](https://metamask.zendesk.com/hc/en-us/articles/360015488971-New-to-MetaMask-Learn-How-to-Setup-MetaMask-the-First-Time) oluşturma

## <a name="get-endpoint-address"></a>Uç nokta adresini alma

Blockchain ağına bağlanmak için Azure Blockchain Hizmeti bitiş noktası adresine ihtiyacınız vardır. Bitiş noktası adresi ve erişim anahtarları Azure portalındadır.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Blockchain Service üyenize gidin.
1. **Hareket düğümleri** ve varsayılan hareket düğümü bağlantısını seçin.

    ![Varsayılan hareket düğümünü seçin](./media/connect-metamask/transaction-nodes.png)

1. **Bağlantı dizeleri > Erişim tuşlarını**seçin.
1. Bitiş noktası adresini HTTPS'den kopyalayın **(Erişim tuşu 1)**.

    ![Bağlantı dizesi](./media/connect-metamask/connection-string.png)

## <a name="connect-metamask"></a>MetaMaske'yi Bağla

1. MetaMask tarayıcı uzantısını açın ve oturum açın.
1. Ağ açılır düşüşünde **Özel RPC'yi**seçin.

    ![Özel RPC](./media/connect-metamask/custom-rpc.png)

1. **Yeni Ağ > Yeni RPC URL'sinde,** yukarıda kopyaladığınız uç nokta adresini yapıştırın.
1. **Kaydet'i**seçin.

    Bağlantı başarılı olduysa, özel ağ ağ açılır ken görüntülenir.

    ![Yeni ağ](./media/connect-metamask/new-network.png)

## <a name="deploy-smart-contract"></a>Akıllı sözleşme dağıtma

Remix tarayıcı tabanlı Solidity geliştirme ortamıdır. MetaMask ve Remix'i birlikte kullanarak, akıllı sözleşmeler üzerinde dağıtabilir ve eylemlerde olabilirsiniz.

1. Tarayıcınızda `https://remix.ethereum.org` adresine gidin.
1. **Dosya'nın**altındaki **Giriş** sekmesinde **Yeni dosyayı** seçin.

    Yeni dosyayı `simple.sol`adlandırın.

    ![Dosya oluşturma](./media/connect-metamask/create-file.png)

    **Tamam'ı**seçin.
1. Remix düzenleyicisinde, aşağıdaki basit **akıllı sözleşme** koduna yapıştırın.

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

    Basit **sözleşme,** **bakiye**adlı bir durum değişkeni bildirir. Tanımlanmış iki işlev vardır. **Ekle** işlevi **dengeye**bir sayı ekler. **Get** işlevi **bakiye**değerini döndürür.
1. Sözleşmeyi derlemek için önce Solidity derleyici bölmesini seçin, ardından **Simplee.sol'u derle'i**seçin.

    ![Derlemek](./media/connect-metamask/compile.png)

1. & **Çalıştır** bölmesini seçin ve MetaMask aracılığıyla blockchain üyenize bağlanmak için **Ortamı** **Enjekte Edilen Web3'e** ayarlayın.

    ![Sekmeyi çalıştır](./media/connect-metamask/injected-web3.png)

1. **Basit** sözleşmeyi seçin ve sonra **Dağıt.**

    ![Dağıtma](./media/connect-metamask/deploy.png)


1. MetaMask bildirimi, işlemi gerçekleştirmek için yeterli kaynak bulunmadığıkonusunda sizi uyarır.

    Genel bir blockchain ağı için işlem maliyetini ödemek için Eter'e ihtiyacınız vardır. Bu bir konsorsiyumda özel bir ağ olduğundan, gaz fiyatını sıfıra ayarlayabilirsiniz.

1.  **Gelişmiş > Edit > Gaz Ücreti**seçin, Gaz **Fiyatını** 0 olarak ayarlayın.

    ![Gaz fiyatı](./media/connect-metamask/gas-price.png)

    **Kaydet'i**seçin.

1. Akıllı sözleşmeyi blockchain'e dağıtmak için **Onayla'yı** seçin.
1. **Dağıtılan Sözleşmeler** bölümünde, **basit** sözleşmeyi genişletin.

    ![Dağıtılan sözleşme](./media/connect-metamask/deployed-contract.png)

    İki eylem, **eklemek** ve **almak**, sözleşmede tanımlanan işlevlere eş.

1. Blockchain'de **ekleme** işlemi gerçekleştirmek için eklemek için bir sayı girin ve **sonra ekle'yi**seçin. Remix'ten gaz tahmini hata mesajı alabilirsiniz: "İşlemi gaz gerektirmeyen özel bir blockchain'e gönderiyorsunuz." Hareketi zorlamak için **Hareket Gönder'i** seçin.
1. Sözleşmeyi dağıttığınızda olduğu gibi, metamask bildirimi işlemi gerçekleştirmek için yeterli kaynak bulunmadığı konusunda sizi uyarır.

    Bu bir konsorsiyumda özel bir ağ olduğu için gaz fiyatını sıfıra ayarlayabiliriz.

1. **Gelişmiş > > Gaz Ücreti'ni**seçin, Gaz **Fiyatını** 0 olarak ayarlayın ve **Kaydet'i**seçin.
1. İşlemi blockchain'e göndermek için **Onayla'yı** seçin.
1. Eylem **al'ı** seçin. Bu, düğüm verilerini sorgulamaçağrısıdır. Bir işlem gerekli değildir.

Remix hata ayıklama bölmesi blockchain'deki işlemlerle ilgili ayrıntıları gösterir:

    ![Debug history](./media/connect-metamask/debug.png)

    You can see the **simple** contract creation, transaction for **simple.add**, and call to **simple.get**.

MetaMask'te işlem geçmişini görmek için MetaMask tarayıcı uzantısını açın ve dağıtılan sözleşme ve hareketlerin günlüklüğünü **tarih** bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Blockchain Hizmet işlem düğümüne bağlanmak, akıllı bir sözleşme dağıtmak ve blockchain'e bir hareket göndermek için MetaMask tarayıcı uzantısını kullandınız. Bir işlem aracılığıyla akıllı bir sözleşme işlevi oluşturmak, oluşturmak, dağıtmak ve yürütmek için Ethereum ve Truffle için Azure Blockchain Geliştirme Kiti'ni kullanmak için bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure Blockchain Hizmeti'nde akıllı sözleşmeler oluşturma, oluşturma ve dağıtma](send-transaction.md)