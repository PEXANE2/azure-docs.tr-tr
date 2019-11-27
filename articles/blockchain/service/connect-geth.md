---
title: Azure blok zinciri hizmetine eklemek için geth kullanma
description: Azure blok zinciri hizmeti işlem düğümündeki bir geth örneğine iliştirme
ms.date: 11/20/2019
ms.topic: quickstart
ms.reviewer: janders
ms.openlocfilehash: 9da78eac1dc429bcc0ad52bb9cb2f1fb743a90d4
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455837"
---
# <a name="quickstart-use-geth-to-attach-to-an-azure-blockchain-service-transaction-node"></a>Hızlı başlangıç: Azure blok zinciri hizmeti işlem düğümüne eklemek için geth kullanma

Bu hızlı başlangıçta, Azure blok zinciri hizmeti işlem düğümündeki bir geth örneğine eklemek için geth istemcisini kullanırsınız. İliştirildikten sonra, Web3 JavaScript Dapp API 'sini çağırmak için geth JavaScript konsolunu kullanırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Geth](https://github.com/ethereum/go-ethereum/wiki/geth) 'ı yükler
* [Hızlı başlangıç: Azure Portal veya hızlı başlangıç kullanarak bir blok zinciri üyesi oluşturma](create-member.md) [: Azure CLI kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi](create-member-cli.md) oluşturma

## <a name="get-geth-connection-string"></a>Geth bağlantı dizesi al

Azure portal bir Azure blok zinciri hizmeti işlem düğümü için geth bağlantı dizesini edinebilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure blok zinciri hizmeti üyesine gidin. **İşlem düğümleri** ve varsayılan işlem düğümü bağlantısı ' nı seçin.

    ![Varsayılan işlem düğümünü seçin](./media/connect-geth/transaction-nodes.png)

1. **Bağlantı dizelerini**seçin.
1. Bağlantı dizesini **https 'den (erişim anahtarı 1)** kopyalayın. Sonraki bölüm için dizeye ihtiyacınız vardır.

    ![Bağlantı dizesi](./media/connect-geth/connection-string.png)

## <a name="connect-to-geth"></a>Geth 'a bağlanma

1. Bir komut istemi veya kabuk açın.
1. İşlem düğümünüz üzerinde çalışan geth örneğine iliştirmek için geth Attach alt komutunu kullanın. Bağlantı dizesini Attach alt komutu için bir bağımsız değişken olarak yapıştırın. Örneğin:

    ``` bash
    geth attach <connection string>
    ```

1. İşlem düğümünün Ethereum konsoluna bağlandıktan sonra Web3 JavaScript Dapp API 'sini veya yönetim API 'sini çağırabilirsiniz.

    Örneğin, Chainıd 'yi bulmak için aşağıdaki API 'yi kullanın.

    ``` bash
    admin.nodeInfo.protocols.istanbul.config.chainId
    ```

    Bu örnekte, Chainıd 661 ' dir.

    ![Azure blok zinciri hizmeti seçeneği](./media/connect-geth/geth-attach.png)

1. Konsola bağlantıyı kesmek için `exit`yazın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure blok zinciri hizmeti işlem düğümündeki bir geth örneğine eklemek için geth istemcisini kullandınız. Bir işlem aracılığıyla akıllı sözleşme işlevi oluşturmak, derlemek, dağıtmak ve yürütmek için Ethereum için Azure blok zinciri geliştirme setini kullanmak üzere bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetinde akıllı sözleşmeler oluşturma, derleme ve dağıtma](send-transaction.md)
