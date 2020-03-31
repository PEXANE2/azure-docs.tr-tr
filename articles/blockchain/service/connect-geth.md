---
title: Azure Blockchain Hizmetine bağlanmak için Geth'i kullanma
description: Azure Blockchain Hizmeti hareket düğümünde Geth örneğine ekleme
ms.date: 11/20/2019
ms.topic: quickstart
ms.reviewer: janders
ms.openlocfilehash: 9da78eac1dc429bcc0ad52bb9cb2f1fb743a90d4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74455837"
---
# <a name="quickstart-use-geth-to-attach-to-an-azure-blockchain-service-transaction-node"></a>Quickstart: Azure Blockchain Hizmeti işlem düğümüne eklemek için Geth'i kullanın

Bu hızlı başlatmada, Geth istemcisini azure blockchain hizmet hareket düğümündeki geth örneğine eklemek için kullanırsınız. Bağlandıktan sonra, bir web3 JavaScript Dapp API aramak için Geth JavaScript konsolu kullanın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Geth'i](https://github.com/ethereum/go-ethereum/wiki/geth) Yükle
* Tam [Hızlı Başlatma: Azure portalını veya Quickstart'ı kullanarak blockchain üyesi oluşturun:](create-member.md) [Azure CLI'yi kullanarak bir Azure Blockchain Hizmeti blockchain üyesi oluşturun](create-member-cli.md)

## <a name="get-geth-connection-string"></a>Geth bağlantı dizesi alın

Azure portalında Bir Azure Blockchain Hizmeti işlem düğümü için Geth bağlantı dizesini alabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Blockchain Service üyenize gidin. **Hareket düğümleri** ve varsayılan hareket düğümü bağlantısını seçin.

    ![Varsayılan hareket düğümünü seçin](./media/connect-geth/transaction-nodes.png)

1. **Bağlantı dizeleri'ni**seçin.
1. Bağlantı dizesini **HTTPS'den kopyalayın (Erişim tuşu 1)**. Bir sonraki bölüm için dize gerekir.

    ![Bağlantı dizesi](./media/connect-geth/connection-string.png)

## <a name="connect-to-geth"></a>Geth'e bağlan

1. Komut istemini veya kabuğu açın.
1. İşlem düğümünüzde çalışan Geth örneğine eklemek için Geth ekle alt komutunu kullanın. Bağlantı dizesini ekle alt komutu için bir bağımsız değişken olarak yapıştırın. Örnek:

    ``` bash
    geth attach <connection string>
    ```

1. İşlem düğümün Ethereum konsoluna bağlandıktan sonra web3 JavaScript Dapp API'yi veya yönetici API'sini arayabilirsiniz.

    Örneğin, chainId'i bulmak için aşağıdaki API'yi kullanın.

    ``` bash
    admin.nodeInfo.protocols.istanbul.config.chainId
    ```

    Bu örnekte, chainId 661'dir.

    ![Azure Blockchain Hizmeti seçeneği](./media/connect-geth/geth-attach.png)

1. Konsoldan bağlantısını kesmek `exit`için .

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, Geth istemcisini Azure Blockchain Hizmeti işlem düğümündeki geth örneğine eklemek için kullandınız. Bir işlem aracılığıyla akıllı bir sözleşme işlevi oluşturmak, oluşturmak, dağıtmak ve yürütmek için Ethereum için Azure Blockchain Geliştirme Kiti'ni kullanmak için bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure Blockchain Hizmeti'nde akıllı sözleşmeler oluşturma, oluşturma ve dağıtma](send-transaction.md)
