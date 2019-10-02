---
title: Azure Logic Apps ile Ethereum blok zinciri bağlayıcısını kullanma
description: Akıllı sözleşme işlevlerini tetiklemek ve akıllı sözleşme olaylarına yanıt vermek için Azure Logic Apps ile Ethereum blok zinciri bağlayıcısını kullanma.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 09/26/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: chrisseg
manager: femila
ms.openlocfilehash: 48f862bea6df50480ce4f9d21993693da34cfbc9
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71720404"
---
# <a name="how-to-use-ethereum-blockchain-connector-with-azure-logic-apps"></a>Azure Logic Apps ile Ethereum blok zinciri bağlayıcısını kullanma

Akıllı sözleşme eylemleri gerçekleştirmek ve akıllı sözleşme olaylarına yanıt vermek için [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) Ile [Ethereum blok zinciri bağlayıcısını](https://docs.microsoft.com/connectors/blockchainethereum/) kullanın. Örneğin, bir blok zinciri defterindeki bilgileri döndüren REST tabanlı bir mikro hizmet oluşturmak istersiniz. Mantıksal uygulama kullanarak, blok zinciri defterinde depolanan bilgileri sorgulayan HTTP isteklerini kabul edebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* İsteğe bağlı önkoşul hızlı başlangıcını doldurun [: Azure blok zinciri hizmeti Consortium ağına bağlanmak için Visual Studio Code kullanın](connect-vscode.md). Hızlı başlangıç, [Ethereum Için Azure blok zinciri geliştirme setini](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) yüklerken ve blok zinciri geliştirme ortamınızı ayarlarken size kılavuzluk eder.

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

Sistemleri ve Hizmetleri tümleştirmeniz gerektiğinde iş süreçlerini ve iş akışlarını zamanlamanıza, otomatikleştirmenize yardımcı Azure Logic Apps. İlk olarak, Ethereum blok zinciri bağlayıcısını kullanan bir mantık oluşturursunuz.

1. [Azure Portal](https://portal.azure.com) **kaynak oluştur > tümleştirme > mantıksal uygulama**' yı seçin.
1. **Mantıksal uygulama oluştur**altında, mantıksal uygulamanızı oluşturmak için gereken ayrıntıları sağlayın. İşiniz bittiğinde **Oluştur**' u seçin.

    Azure Logic Apps oluşturma hakkında daha fazla bilgi için bkz. [Create Azure Logic Apps](../../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Azure uygulamanızı dağıtduktan sonra mantıksal uygulama kaynağınızı seçin.
1. Logic Apps tasarımcısında **Şablonlar**altında **boş mantıksal uygulama**' yı seçin.

Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur.

Ethereum blok zinciri bağlayıcısının bir tetikleyicisi ve birkaç eylemi vardır. Kullandığınız tetikleyici veya eylem, senaryonuza bağlıdır.

İş akışınız varsa aşağıdaki bölümlerden birini seçin:

* Blok zincirinde bir olay gerçekleştiğinde tetikler, [olay tetikleyicisini kullanın](#use-the-event-trigger).
* Akıllı bir sözleşmeyi sorgular veya dağıtır, [eylemleri kullanın](#use-actions).
* Ortak bir senaryoyu takip eden, [Geliştirici setini kullanarak iş akışı oluşturma](#generate-a-workflow).

## <a name="use-the-event-trigger"></a>Olay tetikleyicisini kullanma

Akıllı bir anlaşma olayı oluştuktan sonra bir mantıksal uygulamanın çalışmasını istediğinizde Ethereum blok zinciri olay tetikleyicilerini kullanın. Örneğin, bir akıllı anlaşma işlevi çağrıldığında bir e-posta göndermek istersiniz.

1. Mantıksal uygulama tasarımcısında, Ethereum blok zinciri bağlayıcısını seçin.
1. **Tetikleyiciler** sekmesinden **bir akıllı anlaşma olayının ne zaman gerçekleşeceğini**seçin.
1. Azure blok zinciri hizmetinize [BIR API bağlantısı değiştirin veya BIR API bağlantısı oluşturun](#create-an-api-connection) .
1. Olayları denetlemek istediğiniz akıllı sözleşmenin ayrıntılarını girin.

    ![Olay tetikleyicisi özellikleri](./media/ethereum-logic-app/event-properties.png)

    | Özellik | Açıklama |
    |----------|-------------|
    | **Sözleşme ABı** | Sözleşme uygulaması ikili arabirimi (ABı) akıllı sözleşme arabirimlerini tanımlar. [Sözleşmeyi alma ABI](#get-contract-abi). |
    | **Akıllı sözleşme adresi** | Sözleşme adresi, Ethereum blok zincirindeki akıllı anlaşma hedef adresidir. [Sözleşme adresini alma](#get-contract-address). |
    | **Olay adı** | Denetlenecek akıllı sözleşme olayını seçin. Olay mantıksal uygulamayı tetikler. |
    | **Aralık** ve **Sıklık** | Olayı ne sıklıkta denetlemek istediğinizi seçin. |

1. **Kaydet**’i seçin.

Mantıksal uygulamanızı tamamlayabilmeniz için Ethereum blok zinciri olay tetikleyicisine dayalı bir eylem gerçekleştiren yeni bir adım ekleyebilirsiniz. Örneğin, bir e-posta gönderin.

## <a name="use-actions"></a>Kullanım eylemleri

Mantıksal uygulamanın blok zinciri defterinde bir eylem gerçekleştirmesini istediğinizde Ethereum blok zinciri eylemlerini kullanın. Örneğin, bir mantıksal uygulamaya HTTP isteği yapıldığında bir akıllı anlaşma işlevi çağıran bir REST tabanlı mikro hizmet oluşturmak istersiniz.

Bağlayıcı eylemleri bir tetikleyici gerektirir. Bir tetikleyiciden sonraki adımla bir Ethereum blok zinciri bağlayıcı eylemini kullanabilirsiniz. Örneğin, bir mikro Sservice için HTTP isteği tetikleyicisi.

1. Mantıksal uygulama tasarımcısında, bir tetikleyiciyi izleyen **yeni adım** ' ı seçin.
1. Ethereum blok zinciri bağlayıcısını seçin.
1. **Eylemler** sekmesinde, kullanılabilir eylemlerden birini seçin.

    ![Eylem özellikleri](./media/ethereum-logic-app/action-properties.png)

1. Azure blok zinciri hizmetinize [BIR API bağlantısı değiştirin veya BIR API bağlantısı oluşturun](#create-an-api-connection) .
1. Seçtiğiniz eyleme bağlı olarak, akıllı sözleşme işleviniz hakkında aşağıdaki ayrıntıları sağlayın.

    | Özellik | Açıklama |
    |----------|-------------|
    | **Sözleşme ABı** | Sözleşme uygulaması ikili arabirimi (ABı) akıllı sözleşme arabirimlerini tanımlar. [Sözleşmeyi alma ABI](#get-contract-abi). |
    | **Sözleşme bytecode 'u** | Derlenen akıllı sözleşme bayt kodu. [Sözleşme bayt sayısını alma](#get-contract-bytecode). |
    | **Akıllı sözleşme adresi** | Sözleşme adresi, Ethereum blok zincirindeki akıllı anlaşma hedef adresidir. [Sözleşme adresini alma](#get-contract-address). |
    | **Akıllı sözleşme işlev adı** | Eylem için akıllı sözleşme işlev adını seçin. Liste, sözleşme ABı ayrıntılarından doldurulur. |

    Akıllı sözleşme işlev adı seçtikten sonra, işlev parametreleri için gerekli alanları görebilirsiniz. Senaryonuz için gereken değerleri veya dinamik içeriği girin.

Artık mantıksal uygulamanızı kullanabilirsiniz. Mantıksal uygulama olayı tetiklendiğinde, Ethereum blok zinciri eylemi çalışır. Örneğin, bir HTTP isteği tetikleyicisi, bir akıllı anlaşma durumu değerini sorgulamak için bir Ethereum blok zinciri eylemi çalıştırarak değeri döndüren bir HTTP yanıtına neden olur.

## <a name="generate-a-workflow"></a>İş akışı oluşturma

Ethereum Visual Studio Code uzantısı için Azure blok zinciri geliştirme seti, yaygın senaryolar için mantıksal uygulama iş akışları oluşturabilir. Dört senaryo mevcuttur:

* Azure SQL veritabanına veri yayımlama
* Azure Event Grid veya Azure Service Bus olay yayımlama
* Rapor yayımlama
* REST tabanlı mikro hizmet

 Azure blok zinciri geliştirme seti, blok zinciri geliştirmeyi basitleştirmek için Truffle kullanır. Akıllı sözleşmeye dayalı bir mantıksal uygulama oluşturmak için akıllı sözleşme için bir truffle çözümüne ihtiyacınız vardır. Ayrıca Azure blok zinciri hizmeti Consortium ağınıza da bir bağlantı gerekir. Daha fazla bilgi için bkz. [Visual Studio Code kullanarak bir Azure blok zinciri hizmeti konsorsiyum ağı hızlı başlangıç bağlantısı](connect-vscode.md).

Örneğin, aşağıdaki adımlar hızlı başlangıç **Helloblockzincirine** akıllı sözleşmeye DAYALı bir REST tabanlı mikro hizmet mantıksal uygulaması oluşturur:

1. VS Code Explorer kenar çubuğunda çözümünüzdeki **sözleşmeler** klasörünü genişletin.
1. **Helloblockzincirine. Nuevo** öğesine sağ tıklayın ve menüden **akıllı sözleşmeler Için mikro hizmetler oluştur** ' u seçin.

    ![Mantıksal uygulama oluşturma](./media/ethereum-logic-app/generate-logic-app.png)

1. Komut paletinde **Logic App**' i seçin.
1. **Sözleşme adresini**girin. Daha fazla bilgi için bkz. [sözleşme adresini alma](#get-contract-address).
1. Mantıksal uygulama için Azure aboneliğini ve kaynak grubunu seçin.

    Mantıksal uygulama yapılandırması ve kod dosyaları **generatedLogicApp** dizininde oluşturulur.

1. **GeneratedLogicApp/Helloblockzincirleri** dizinini görüntüleyin. Her akıllı sözleşme işlevi, olayı ve özelliği için bir mantıksal uygulama JSON dosyası vardır.
1. **GeneratedLogicApp/Helloblockzincirleri/Service/Property ' i açın. RequestMessage. logicapp. JSON** dosyası ve içeriğini kopyalayın.

    ![RequestMessage özelliği için JSON](./media/ethereum-logic-app/requestmessage.png)

1. Mantıksal uygulamanızda **mantıksal uygulama kod görünümü**' nü seçin. Var olan JSON 'ı oluşturulan mantıksal uygulama JSON ile değiştirin.

    ![Kod görünümündeki mantıksal uygulama yapılandırmasını değiştirme](./media/ethereum-logic-app/code-view.png)

1. Tasarımcı görünümüne geçiş yapmak için **Tasarımcı** ' yı seçin.
1. Mantıksal uygulama, senaryoya yönelik temel adımları içerir. Ancak, Ethereum blok zinciri bağlayıcısının yapılandırma ayrıntılarını güncelleştirmeniz gerekir.
1. **Bağlantılar** adımını seçin ve Azure blok zinciri HIZMETINIZE [bir API bağlantısı oluşturun](#create-an-api-connection) veya değiştirin.

    ![Mikro hizmet mantıksal uygulaması](./media/ethereum-logic-app/microservice-logic-app.png)

1. Artık mantıksal uygulamanızı kullanabilirsiniz. REST tabanlı mikro hizmeti test etmek için mantıksal uygulama isteği URL 'sine bir HTTP POST isteği verin. Http **isteği alındığında** , ÖĞESINDEN **http post URL** 'sini kopyalayın.

    ![HTTP POST URL 'SI](./media/ethereum-logic-app/post-url.png)

1. Bir HTTP POST isteği oluşturmak için kıvrımlı kullanın. Yer tutucu metnini değiştirin **\<HTTP Post URL 'si @ no__t-2** ÖNCEKI adımdaki URL ile.

    ``` bash
    curl -d "{}" -H "Content-Type: application/json" -X POST "<HTTP POST URL>"
    ```

    Kıvrımlı komutu mantıksal uygulamadan bir yanıt döndürür. Bu durumda, **RequestMessage** akıllı sözleşme işlevinin çıktısı.

    ![RequestMessage özelliği çıkışı](./media/ethereum-logic-app/curl.png)

Geliştirme setini kullanma hakkında daha fazla bilgi için bkz. [Ethereum wiki Için Azure blok zinciri geliştirme seti](https://github.com/Microsoft/vscode-azure-blockchain-ethereum/wiki) sayfası.

## <a name="create-an-api-connection"></a>API bağlantısı oluşturma

Ethereum blok zinciri Bağlayıcısı için bir blok zincirine yönelik bir API bağlantısı gereklidir. Birden çok Logic Apps için API bağlayıcısını kullanabilirsiniz. Bazı özellikler gereklidir ve diğerleri senaryonuza bağlıdır.

> [!IMPORTANT]
> Bir blok zincirinde işlem oluşturmak için bir özel anahtar veya hesap adresi ve parola gereklidir. Yalnızca bir kimlik doğrulama biçimi gereklidir. Hem özel anahtar hem de hesap ayrıntılarını sağlamanız gerekmez. Sözleşmelerin sorgulanması bir işlem gerektirmez. Sözleşme durumunu sorgulayan eylemler kullanıyorsanız, özel anahtar veya hesap adresi ve parola gerekli değildir.

Azure blok zinciri hizmet üyesine bir bağlantı kurmak için aşağıdaki liste, senaryonuza bağlı olarak gereken olası özelliklerdir.

| Özellik | Açıklama |
|----------|-------------|
|**Bağlantı adı** | API bağlantısının adı. Gereklidir. |
|**Ethereum RPC uç noktası** | Azure blok zinciri hizmeti işlem düğümünün HTTP adresi. Gereklidir. [RPC uç noktasını alma](#get-rpc-endpoint). |
|**Özel anahtar** | Ethereum hesabı özel anahtarı. İşlemler için özel anahtar veya hesap adresi ve parola gereklidir. [Özel anahtarı alma](#get-private-key). |
|**Hesap adresi** | Azure blok zinciri hizmeti üyesi hesap adresi. İşlemler için özel anahtar veya hesap adresi ve parola gereklidir. [Hesap adresini alma](#get-account-address). |
|**Hesap parolası** | Üye oluşturduğunuzda hesap parolası ayarlanır. Parolayı sıfırlama hakkında daha fazla bilgi için bkz. [Ethereum hesabı](consortium.md#ethereum-account).|

## <a name="get-rpc-endpoint"></a>RPC uç noktası al

Blok zinciri ağına bağlanmak için Azure blok zinciri hizmeti RPC uç noktası adresi gereklidir. Ethereum veya Azure portal için Azure blok zinciri geliştirme setini kullanarak uç nokta adresini alabilirsiniz.

**Geliştirme setini kullanma:**

1. Visual Studio Code 'de **Azure blok zinciri hizmeti** altında, konsorsiya sağ tıklayın.
1. **RPC uç noktasını Kopyala**' yı seçin.

    ![RPC uç noktasını Kopyala](./media/ethereum-logic-app/devkit-rpc.png)

    RPC uç noktası panonuza kopyalanır.

**Azure portal kullanma:**

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Azure blok zinciri hizmeti üyesine gidin. **İşlem düğümleri** ve varsayılan işlem düğümü bağlantısı ' nı seçin.

    ![Varsayılan işlem düğümünü seçin](./media/ethereum-logic-app/transaction-nodes.png)

1. **Erişim anahtarlarına > bağlantı dizeleri**seçin.
1. Endpoint adresini **https (erişim anahtarı 1)** veya erişim anahtarı 2 ' den kopyalayın.

    ![Bağlantı dizesi](./media/ethereum-logic-app/connection-string.png)

    RPC uç noktası, Azure blok zinciri hizmeti üye işlem düğümünüz için adres ve erişim anahtarı dahil olmak üzere HTTPS URL 'sidir.

## <a name="get-private-key"></a>Özel anahtar al

Ethereum hesabı özel anahtarı, blok zincirine bir işlem gönderilirken kimlik doğrulaması yapmak için kullanılabilir. Ethereum hesabınız ortak ve özel anahtarlarınız 12 sözcükten oluşan bir anımsatıcı tarafından oluşturulmuştur. Ethereum için Azure blok zinciri geliştirme seti, bir Azure blok zinciri hizmeti Konsorsiyumu üyesine bağlandığınızda bir anımsatıcı oluşturur. Geliştirme Seti uzantısını kullanarak uç nokta adresini alabilirsiniz.

1. Visual Studio Code ' de, komut paleti ' ni (F1) açın.
1. **Azure blok Zinciri: özel anahtar al**' ı seçin.
1. Consortium üyesine bağlanırken kaydettiğiniz anımsatıcı ' u seçin.

    ![Anımsatıcı Seç](./media/ethereum-logic-app/private-key.png)

    Özel anahtar panonuza kopyalanır.

## <a name="get-account-address"></a>Hesap adresini al

Üye hesabı ve parola, blok zincirine bir işlem gönderilirken kimlik doğrulaması yapmak için kullanılabilir. Üye oluşturduğunuzda parola ayarlanır.

1. Azure portal Azure blok zinciri hizmetine genel bakış sayfasına gidin.
1. **Üye hesabı** adresini kopyalayın.

    ![Üye hesabını kopyala](./media/ethereum-logic-app/member-account.png)

Hesap adresi ve parola hakkında daha fazla bilgi için bkz. [Ethereum hesabı](consortium.md#ethereum-account).

## <a name="get-contract-abi"></a>Sözleşme ABı al

Sözleşme uygulaması ikili arabirimi (ABı) akıllı sözleşme arabirimlerini tanımlar. Akıllı sözleşmeyle nasıl etkileşim kuracağınızı açıklar. Ethereum için Azure blok zinciri geliştirme seti 'ni veya Solidity derleyici sözleşmesi meta veri dosyasını kullanarak sözleşme ABı alabilirsiniz.

**Geliştirme setini kullanma:**

Akıllı sözleşmenizi derlemek için geliştirme seti veya Truffle kullandıysanız, ABı sözleşmesini Pano 'ya kopyalamak için uzantıyı kullanabilirsiniz.

1. Visual Studio Code gezgin bölmesinde, Solidity projenizin **Build/Contracts** klasörünü genişletin.
1. Sözleşme meta verileri JSON dosyasına sağ tıklayın. Dosya adı akıllı sözleşme adı ve ardından **. JSON** uzantısıdır.
1. **Sözleşme ABI Kopyala**' yı seçin.

    ![DevKit kullanarak sözleşme ABı kopyalama](./media/ethereum-logic-app/abi-devkit.png)

    Sözleşme ABı, panoya kopyalanır.

**Sözleşme meta veri dosyasını kullanma:**

1. Solidity projenizin **Build/Contracts** klasöründe yer alan Sözleşme meta veri dosyasını açın. Dosya adı akıllı sözleşme adı ve ardından **. JSON** uzantısıdır.
1. JSON dosyasında **ABI** bölümünü bulun.
1. **ABI** JSON dizisini kopyalayın.

    ![Meta verilerde sözleşme ABı bölümü](./media/ethereum-logic-app/abi-metadata.png)

## <a name="get-contract-bytecode"></a>Sözleşme bytecode 'u al

Sözleşme bayt kodu, Ethereum sanal makinesi tarafından yürütülen derlenmiş akıllı sözleşmedir. Ethereum için Azure blok zinciri geliştirme setini veya Solidity derleyicisini kullanarak sözleşme bytecode 'u alabilirsiniz.

**Geliştirme setini kullanma:**

Akıllı sözleşmenizi derlemek için geliştirme seti veya Truffle kullandıysanız, uzantıyı panoya sözleşme bytecode 'u kopyalamak için kullanabilirsiniz.

1. Visual Studio Code gezgin bölmesinde, Solidity projenizin **Build/Contracts** klasörünü genişletin.
1. Sözleşme meta verileri JSON dosyasına sağ tıklayın. Dosya adı akıllı sözleşme adı ve ardından **. JSON** uzantısıdır.
1. **Sözleşme bayt kodunu kopyala**' yı seçin.

    ![DevKit kullanarak sözleşme bytecode 'u kopyalama](./media/ethereum-logic-app/bytecode-devkit.png)

    Sözleşme bayt kodu panoya kopyalanır.

**Sözleşme meta veri dosyasını kullanma:**

1. Solidity projenizin **Build/Contracts** klasöründe yer alan Sözleşme meta veri dosyasını açın. Dosya adı akıllı sözleşme adı ve ardından **. JSON** uzantısıdır.
1. JSON dosyasında **bytecode** öğesini bulun.
1. **Bytecode** değerini kopyalayın.

    ![Meta verileri kullanarak bytecode 'u kopyalama](./media/ethereum-logic-app/bytecode-metadata.png)

**Solidity derleyicisini kullanma:**

Sözleşme bytecode 'u oluşturmak için `solc --bin <smart contract>.sol` komutunu kullanın.

## <a name="get-contract-address"></a>Sözleşme adresini al

Sözleşme adresi, Ethereum blok zincirindeki akıllı anlaşma hedef adresidir. Bu adresi, bir akıllı sözleşmenin işlemini veya sorgu durumunu göndermek için kullanırsınız. Sözleşme adresini Truffle geçiş çıktısından veya anlaşma meta veri dosyasından alabilirsiniz.

**Truffle geçişi çıkışını kullanma:**

Truffle, akıllı sözleşmenin dağıtımından sonra sözleşme adresini görüntüler. **Sözleşme adresini** çıktıdan kopyalayın.

![Truffle çıktısından sözleşme adresi](./media/ethereum-logic-app/contract-address-truffle.png)

**Sözleşme meta veri dosyasını kullanma:**

1. Solidity projenizin **Build/Contracts** klasöründe yer alan Sözleşme meta veri dosyasını açın. Dosya adı akıllı sözleşme adı ve ardından **. JSON** uzantısıdır.
1. JSON dosyasındaki **ağlar** bölümünü bulun.
1. Özel ağlar bir tamsayı ağ KIMLIĞI ile tanımlanır. Ağ bölümünün içinde adres değerini bulun.
1. **Adres** değerini kopyalayın.

![Meta verilerden sözleşme adresi](./media/ethereum-logic-app/contract-address-metadata.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Logic Apps kullanarak blok zincirini bağlayan yaygın senaryoları](https://channel9.msdn.com/Shows/Blocktalk/Doing-more-with-Logic-Apps?term=logic%20apps%20blockchain&lang-en=true)izleyin.
