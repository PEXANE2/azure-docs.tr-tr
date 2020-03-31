---
title: Azure Logic Apps ile Ethereum Blockchain konektörünü kullanın - Azure Blockchain Hizmeti
description: Akıllı sözleşme işlevlerini tetiklemek ve akıllı sözleşme olaylarına yanıt vermek için Azure Logic Apps ile Ethereum Blockchain konektörünü kullanın.
ms.date: 10/14/2019
ms.topic: article
ms.reviewer: chrisseg
ms.openlocfilehash: 4a9acfd6098ed45fd92c7e3047b5d1446eeddbd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74325215"
---
# <a name="use-the-ethereum-blockchain-connector-with-azure-logic-apps"></a>Azure Logic Apps ile Ethereum Blockchain konektörünü kullanma

Akıllı sözleşme eylemleri gerçekleştirmek ve akıllı sözleşme olaylarına yanıt vermek için [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) ile [Ethereum Blockchain konektörünü](https://docs.microsoft.com/connectors/blockchainethereum/) kullanın. Örneğin, blockchain genel muhasebesinden bilgi döndüren REST tabanlı bir microservice oluşturmak istediğinizi varsayalım. Bir mantık uygulaması kullanarak, blockchain genel muhasebesinde depolanan bilgileri sorgulayan HTTP isteklerini kabul edebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

İsteğe bağlı ön koşul [Quickstart'ı tamamlayın: Azure Blockchain Hizmeti konsorsiyum ağına bağlanmak için Visual Studio Kodunu kullanın.](connect-vscode.md) Hızlı başlatma, [Ethereum için Azure Blockchain Geliştirme Kiti'ni](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) yükleyip blockchain geliştirme ortamınızı ayarlamanıza rağmen size yol gösteriyor.

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

Azure Logic Apps, sistemleri ve hizmetleri tümleştirmeniz gerektiğinde iş süreçlerini ve iş akışlarını zamanlamanıza ve otomatikleştirmenize yardımcı olur. İlk olarak, Ethereum Blockchain konektörünü kullanan bir mantık oluşturursunuz.

1. [Azure portalında](https://portal.azure.com)**Kaynak oluştur** > **Tümleştirme** > **Mantıksal Uygulama**’yı seçin.
1. **Mantık oluştur uygulaması**altında, mantık uygulamanızı nerede oluşturacağınıza ilişkin ayrıntıları sağlayın. İşin bittikten sonra **Oluştur'u**seçin.

    Mantık uygulamaları oluşturma hakkında daha fazla bilgi için [bkz.](../../logic-apps/quickstart-create-first-logic-app-workflow.md)

1. Azure uygulamanızı dağıttıktan sonra mantık uygulama kaynağınızı seçin.
1. Logic Apps Designer'da **Şablonlar**altında **Boş Mantık Uygulaması'nı**seçin.

Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur.

Ethereum Blockchain konektörü bir tetikleyicive çeşitli eylemlere sahiptir. Hangi tetikleyiciyi veya eylemi kullandığınız senaryonuza bağlıdır.

İş akışınız:

* Blockchain'de bir olay meydana geldiğinde tetikler, [Olay tetikleyicisini kullanın.](#use-the-event-trigger)
* Akıllı bir sözleşmeyi sorgular veya dağıtıyor, [Eylemleri kullanın.](#use-actions)
* Sık karşılaşılan bir senaryo izler, [Geliştirici kitini kullanarak bir iş akışı oluşturun.](#generate-a-workflow)

## <a name="use-the-event-trigger"></a>Olay tetikleyicisini kullanma

Akıllı bir sözleşme olayı gerçekleştikten sonra bir mantık uygulamasının çalışmasını istediğinizde Ethereum Blockchain olay tetikleyicilerini kullanın. Örneğin, akıllı bir sözleşme işlevi çağrıldığında e-posta göndermek istiyorsunuz.

1. Logic Apps Designer'da Ethereum Blockchain konektörünü seçin.
1. **Tetikleyiciler** sekmesinden, **akıllı bir sözleşme olayı oluştuğunda**seçin.
1. Azure Blockchain Hizmeti'ni değiştirin veya [BIR API bağlantısı oluşturun.](#create-an-api-connection)
1. Etkinlikler için denetlemek istediğiniz akıllı sözleşmeyle ilgili ayrıntıları girin.

    ![Olay tetikleyici özellikleri ile Mantık Uygulamaları Tasarımcısı](./media/ethereum-logic-app/event-properties.png)

    | Özellik | Açıklama |
    |----------|-------------|
    | **Sözleşme ABI** | Sözleşme uygulaması ikili arabirimi (ABI) akıllı sözleşme arabirimlerini tanımlar. Daha fazla bilgi için [bkz.](#get-the-contract-abi) |
    | **Akıllı sözleşme adresi** | Sözleşme adresi, Ethereum blockchain'deki akıllı sözleşme hedef adresidir. Daha fazla bilgi için [bkz.](#get-the-contract-address) |
    | **Olay adı** | Denetlemek için akıllı bir sözleşme olayı seçin. Olay mantık uygulamasını tetikler. |
    | **Aralık** ve **Sıklık** | Olayı ne sıklıkta kontrol etmek istediğinizi seçin. |

1. **Kaydet'i**seçin.

Mantık uygulamanızı tamamlamak için, Ethereum Blockchain olay tetikleyicisine dayalı bir eylem gerçekleştiren yeni bir adım ekleyebilirsiniz. Örneğin, bir e-posta gönderin.

## <a name="use-actions"></a>Eylemler kullanma

Blockchain genel muhasebesi üzerinde bir eylem gerçekleştirmek için bir mantık uygulaması istediğinizde Ethereum Blockchain eylemlerini kullanın. Örneğin, bir mantıksal uygulamaya http isteği yapıldığında akıllı sözleşme işlevi çağıran REST tabanlı bir mikro hizmet oluşturmak istiyorsunuz.

Bağlayıcı eylemleri bir tetikleyici gerektirir. Bir microservice için HTTP isteği tetikleyicisi gibi bir tetikleyiciden sonraki adım olarak Bir Ethereum Blockchain konektör eylemi kullanabilirsiniz.

1. Logic Apps Designer'da tetikleyiciyi izleyen **Yeni adım'ı** seçin.
1. Ethereum Blockchain konektörünü seçin.
1. **Eylemler** sekmesinden, kullanılabilir eylemlerden birini seçin.

    ![Eylemler özellikleri ile Mantık Apps Tasarımcısı](./media/ethereum-logic-app/action-properties.png)

1. Azure Blockchain Hizmeti'ni değiştirin veya [BIR API bağlantısı oluşturun.](#create-an-api-connection)
1. Seçtiğiniz eyleme bağlı olarak, akıllı sözleşme işleviniz hakkında aşağıdaki ayrıntıları sağlayın.

    | Özellik | Açıklama |
    |----------|-------------|
    | **Sözleşme ABI** | SÖZLEŞME ABI akıllı sözleşme arayüzleri tanımlar. Daha fazla bilgi için [bkz.](#get-the-contract-abi) |
    | **Sözleşme bayt kodu** | Derlenmiş akıllı sözleşme bytecode. Daha fazla bilgi için [bkz.](#get-the-contract-bytecode) |
    | **Akıllı sözleşme adresi** | Sözleşme adresi, Ethereum blockchain'deki akıllı sözleşme hedef adresidir. Daha fazla bilgi için [bkz.](#get-the-contract-address) |
    | **Akıllı sözleşme işlev adı** | Eylem için akıllı sözleşme işlev adını seçin. Liste, SÖZLEŞME ABI'deki ayrıntılardan doldurulur. |

    Akıllı bir sözleşme işlev adı seçtikten sonra, işlev parametreleri için gerekli alanları görebilirsiniz. Senaryonuz için gereken değerleri veya dinamik içeriği girin.

Artık mantık uygulamanızı kullanabilirsiniz. Mantık uygulaması olayı tetiklendiğinde, Ethereum Blockchain eylemi çalışır. Örneğin, bir HTTP isteği tetikleyicisi akıllı bir sözleşme durumu değerini sorgulamak için bir Ethereum blockchain eylemi çalıştırın. Bu sorgu, değeri döndüren bir HTTP yanıtıyla sonuçlanır.

## <a name="generate-a-workflow"></a>İş akışı oluşturma

Ethereum Visual Studio Code uzantısı için Azure Blockchain Geliştirme Kiti, sık karşılaşılan senaryolar için mantık uygulaması iş akışları oluşturabilir. Dört senaryo mevcuttur:

* Azure SQL Veritabanı örneğine veri yayımlama
* Azure Olay Izgaraveya Azure Hizmet Veri Servisi örneğine etkinlik yayımlama
* Rapor yayımlama
* REST tabanlı mikrohizmet

 Azure Blockchain Geliştirme Kiti, blockchain geliştirmeyi kolaylaştırmak için Truffle kullanır. Akıllı bir sözleşmeye dayalı bir mantık uygulaması oluşturmak için, akıllı sözleşme için bir Truffle çözümüne ihtiyacınız var. Azure Blockchain Service konsorsiyum ağınıza da bağlantı kurmanız gerekir. Daha fazla bilgi için, [Azure Blockchain Hizmeti konsorsiyum ağına hızlı bir şekilde bağlanmak için Visual Studio Kodunu Kullan'a](connect-vscode.md)bakın.

Örneğin, aşağıdaki adımlar quickstart **HelloBlockchain** akıllı sözleşmedayalı bir REST tabanlı microservice mantık uygulaması oluşturmak:

1. Visual Studio Code explorer kenar çubuğunda, çözümdeki **sözleşme** klasörünü genişletin.
1. **HelloBlockchain.sol'a** sağ tıklayın ve menüden **Akıllı Sözleşmeler için MikroHizmetler Oluştur'u** seçin.

    ![Akıllı Sözleşmeler seçimi için Generate Microservices ile Visual Studio Code bölmesi](./media/ethereum-logic-app/generate-logic-app.png)

1. Komut paletinde **Mantık Uygulaması'nı**seçin.
1. Sözleşme **adresini**girin. Daha fazla bilgi için [bkz.](#get-the-contract-address)
1. Mantık uygulaması için Azure aboneliğini ve kaynak grubunu seçin.

    Mantık uygulaması yapılandırması ve kod dosyaları **oluşturulan LogicApp** dizininde oluşturulur.

1. Oluşturulan **LogicApp/HelloBlockchain** dizinini görüntüleyin. Her akıllı sözleşme işlevi, olay ve özellik için bir mantık uygulaması JSON dosyası var.
1. Oluşturulan **LogicApp/HelloBlockchain/Service/property'i açın. RequestMessage.logicapp.json** dosyası ve içeriğini kopyalayın.

    ![Kopyalamak için kod ile JSON dosyası](./media/ethereum-logic-app/requestmessage.png)

1. Mantık uygulamanızda **Mantık uygulama kodu görünümünü**seçin. Varolan JSON'u oluşturulan mantık uygulaması JSON ile değiştirin.

    ![Yeni değiştirilen uygulama koduyla mantık uygulama kodu görünümü](./media/ethereum-logic-app/code-view.png)

1. Tasarımcı görünümüne geçmek için **Designer'ı** seçin.
1. Mantık uygulaması senaryo için temel adımları içerir. Ancak, Ethereum Blockchain konektörü için yapılandırma ayrıntılarını güncelleştirmeniz gerekir.
1. **Bağlantılar** adımını seçin ve Azure Blockchain Hizmeti'ne [bir API bağlantısı oluşturun](#create-an-api-connection) veya değiştirin.

    ![Bağlantılar seçimi ile tasarımcı görünümü](./media/ethereum-logic-app/microservice-logic-app.png)

1. Artık mantık uygulamanızı kullanabilirsiniz. REST tabanlı microservice'i test etmek için, mantık uygulaması istek URL'sine bir HTTP POST isteği gönderin. **HTTP'nin URL'si** içeriğini **NE ZAMAN bir HTTP isteği adımından kopyalayın.**

    ![HTTP POST URL'li Mantık Uygulamaları Tasarımcısı bölmesi](./media/ethereum-logic-app/post-url.png)

1. BIR HTTP POST isteği oluşturmak için cURL'yi kullanın. Yer tutucu metni * \<http\> POST URL'sini* önceki adımdaki URL ile değiştirin.

    ``` bash
    curl -d "{}" -H "Content-Type: application/json" -X POST "<HTTP POST URL>"
    ```

    cURL komutu mantık uygulamasından bir yanıt döndürür. Bu durumda, yanıt **RequestMessage** akıllı sözleşme işlevinden çıktıdır.

    ![RequestMessage akıllı sözleşme işlevinden kod çıktısı](./media/ethereum-logic-app/curl.png)

Geliştirme kitini kullanma hakkında daha fazla bilgi için [Ethereum wiki sayfası için Azure Blockchain Geliştirme Kiti'ne](https://github.com/Microsoft/vscode-azure-blockchain-ethereum/wiki)bakın.

## <a name="create-an-api-connection"></a>API bağlantısı oluşturma

Ethereum Blockchain konektörü için blockchain bağlantısı gereklidir. Birden çok mantık uygulaması için API bağlayıcısını kullanabilirsiniz. Bazı özellikler gereklidir ve diğerleri senaryonuza bağlıdır.

> [!IMPORTANT]
> Blockchain'de işlem oluşturmak için özel bir anahtar veya hesap adresi ve parola gereklidir. Yalnızca bir kimlik doğrulama biçimi gereklidir. Hem özel anahtar hem de hesap ayrıntılarını sağlamanız gerekmez. Sözleşmeleri sorgulamak bir hareket gerektirmez. Sözleşme durumunu sorgulayan eylemleri kullanıyorsanız, özel anahtar veya hesap adresi ve parola gerekmez.

Bir Azure Blockchain Hizmeti üyesiyle bağlantı kurmanıza yardımcı olmak için, aşağıdaki listede senaryonuza bağlı olarak ihtiyaç duyabileceğiniz olası özellikler vardır.

| Özellik | Açıklama |
|----------|-------------|
|**Bağlantı adı** | API bağlantısının adı. Gereklidir. |
|**Ethereum RPC bitiş noktası** | Azure Blockchain Hizmeti işlem düğümünün HTTP adresi. Gereklidir. Daha fazla bilgi için [RPC bitiş noktasını alın' a](#get-the-rpc-endpoint)bakın. |
|**Özel anahtar** | Ethereum hesabı özel anahtar. İşlemler için özel anahtar veya hesap adresi ve şifre gereklidir. Daha fazla bilgi için [bkz.](#get-the-private-key) |
|**Hesap adresi** | Azure Blockchain Service üye hesap adresi. İşlemler için özel anahtar veya hesap adresi ve şifre gereklidir. Daha fazla bilgi için [bkz.](#get-the-account-address) |
|**Hesap parolası** | Üyeyi oluşturduğunuzda hesap parolası ayarlanır. Parolayı sıfırlama hakkında bilgi için [Ethereum hesabına](consortium.md#ethereum-account)bakın.|

## <a name="get-the-rpc-endpoint"></a>RPC bitiş noktasını alın

Bir blockchain ağına bağlanmak için Azure Blockchain Hizmeti RPC uç noktası adresi gereklidir. Ethereum için Azure Blockchain Geliştirme Kiti'ni veya Azure portalını kullanarak bitiş noktası adresini alabilirsiniz.

**Geliştirme kitini kullanmak için:**

1. Visual Studio Code'da **Azure Blockchain Hizmeti** kapsamında konsorsiyuma sağ tıklayın.
1. **RPC Bitiş Noktası Adresini Kopyala'yı**seçin.

    ![Copy RPC Endpoint Adres seçimi ile konsorsiyumgösteren Görsel Studio Kodu bölmesi](./media/ethereum-logic-app/devkit-rpc.png)

    RPC bitiş noktası panonuza kopyalanır.

**Azure portalını kullanmak için:**

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Blockchain Service üyenize gidin. **Hareket düğümleri** ve varsayılan hareket düğümü bağlantısını seçin.

    ![(Varsayılan düğüm) seçimi ile işlem düğümleri sayfası](./media/ethereum-logic-app/transaction-nodes.png)

1. **Bağlantı dizeleri** > **Erişim tuşlarını**seçin.
1. Bitiş noktası adresini **HTTPS (Access tuşu 1)** veya **HTTPS 'den (Erişim tuşu 2)** kopyalayın.

    ![Bağlantı dizesi erişim anahtarlarıile Azure portalı](./media/ethereum-logic-app/connection-string.png)

    RPC bitiş noktası, Azure Blockchain Hizmeti üye işlem düğümünüzün adresini ve erişim anahtarını içeren HTTPS URL'sidir.

## <a name="get-the-private-key"></a>Özel anahtarı alın

Blockchain'e bir işlem gönderirken kimlik doğrulaması yapmak için Ethereum hesabının özel anahtarını kullanabilirsiniz. Ethereum hesabınızın ortak ve özel anahtarları 12 kelimelik bir mnemonic'ten oluşturulur. Ethereum için Azure Blockchain Geliştirme Kiti, bir Azure Blockchain Service konsorsiyumu üyesine bağlandığınızda bir mnemonik oluşturur. Geliştirme kiti uzantısını kullanarak bitiş noktası adresini alabilirsiniz.

1. Visual Studio Code'da komut paletini (F1) açın.
1. **Azure Blockchain'i seçin: Özel anahtarı alın.**
1. Konsorsiyum üyesine bağlanırken kaydettiğiniz mnemonik'i seçin.

    ![Mnemonik seçmek için bir seçenek ile komut paleti](./media/ethereum-logic-app/private-key.png)

    Özel anahtar panonuza kopyalanır.

## <a name="get-the-account-address"></a>Hesap adresini alma

Blockchain'e bir işlem gönderdiğinde kimlik doğrulaması yapmak için üye hesabını ve parolasını kullanabilirsiniz. Üyeyi oluşturduğunuzda parola ayarlanır.

1. Azure portalında Azure Blockchain Hizmeti'ne genel bakış sayfanıza gidin.
1. Üye **hesap** adresini kopyalayın.

    ![Üye hesap adresiile genel bakış sayfası](./media/ethereum-logic-app/member-account.png)

Hesap adresi ve şifre hakkında daha fazla bilgi için [Ethereum hesabına](consortium.md#ethereum-account)bakın.

## <a name="get-the-contract-abi"></a>Sözleşme ABI alın

SÖZLEŞME ABI akıllı sözleşme arayüzleri tanımlar. Akıllı sözleşmeyle nasıl etkileşime girilir açıklanır. Ethereum için Azure Blockchain Geliştirme Kiti'ni kullanarak SÖZLEŞME ABI'yi alabilirsiniz. Ayrıca, Solidity derleyicisi tarafından oluşturulan sözleşme meta veri dosyasından da alabilirsiniz.

**Geliştirme kitini kullanmak için:**

Akıllı sözleşmenizi oluşturmak için geliştirme kitini veya Truffle'ı kullandıysanız, sözleşme ABI'yi panoya kopyalamak için uzantıyı kullanabilirsiniz.

1. Visual Studio Code explorer bölmesinde Solidity projenizin **yapı/sözleşme** klasörünü genişletin.
1. Sözleşme meta data JSON dosyasını sağ tıklatın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. **Kopya Sözleşmesi ABI'yi**seçin.

    ![Copy Contract ABI seçimi ile Visual Studio Code bölmesi](./media/ethereum-logic-app/abi-devkit.png)

    SÖZLEŞME ABI panoya kopyalanır.

**Sözleşme meta veri dosyasını kullanmak için:**

1. Solidity projenizin **yapı/sözleşme** klasöründe yer alan sözleşme meta veri dosyasını açın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. JSON dosyasındaki **abi** bölümünü bulun.
1. **Abi** JSON dizisini kopyalayın.

    ![Sözleşme meta veri dosyasındaki ABI kodu](./media/ethereum-logic-app/abi-metadata.png)

## <a name="get-the-contract-bytecode"></a>Sözleşme bytecode alın

Sözleşme bytecode Ethereum sanal makine tarafından yürütülen derlenmiş akıllı sözleşmedir. Ethereum için Azure Blockchain Geliştirme Kiti'ni kullanarak sözleşme bytecode'una ulaşabilirsiniz. Ayrıca Solidity derleyicisinden de alabilirsiniz.

**Geliştirme kitini kullanmak için:**

Akıllı sözleşmenizi oluşturmak için geliştirme kitini veya Truffle'ı kullandıysanız, sözleşme bytecode'unu panoya kopyalamak için uzantıyı kullanabilirsiniz.

1. Visual Studio Code explorer bölmesinde Solidity projenizin **yapı/sözleşme** klasörünü genişletin.
1. Sözleşme meta data JSON dosyasını sağ tıklatın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. **Sözleşme Bytecode Kopyala'yı**seçin.

    ![Copy Contract Bytecode seçimi ile Visual Studio Code bölmesi](./media/ethereum-logic-app/bytecode-devkit.png)

    Sözleşme bytecode panoya kopyalanır.

**Sözleşme meta veri dosyasını kullanmak için:**

1. Solidity projenizin **yapı/sözleşme** klasöründe yer alan sözleşme meta veri dosyasını açın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. JSON dosyasındaki **bayt kodu** öğesini bulun.
1. **Bytecode** değerini kopyalayın.

    ![Meta verilerde bytecode ile Visual Studio Code bölmesi](./media/ethereum-logic-app/bytecode-metadata.png)

**Solidity derleyicisini kullanmak için:**

Sözleşme bytecode oluşturmak için komutu `solc --bin <smart contract>.sol` kullanın.

## <a name="get-the-contract-address"></a>Sözleşme adresini alın

Sözleşme adresi, Ethereum blockchain'deki akıllı sözleşme hedef adresidir. Bu adresi, akıllı bir sözleşmenin işlem veya sorgu durumu göndermek için kullanırsınız. Sözleşme adresini Trüf geçiş çıkışından veya sözleşme meta veri dosyasından alabilirsiniz.

**Trüf mantarı geçiş çıkışını kullanmak için:**

Trüf mantarı, akıllı sözleşmenin dağıtımından sonra sözleşme adresini görüntüler. Sözleşme **adresini** çıktıdan kopyalayın.

![Visual Studio Code'da sözleşme adresi ile trüf geçiş çıkışı](./media/ethereum-logic-app/contract-address-truffle.png)

**Sözleşme meta veri dosyasını kullanmak için:**

1. Solidity projenizin **yapı/sözleşme** klasöründe yer alan sözleşme meta veri dosyasını açın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. JSON dosyasındaki **ağlar** bölümünü bulun.
1. Özel ağlar bir sonda ağı kimliğiyle tanımlanır. Ağ bölümündeki adres değerini bulun.
1. **Adres** değerini kopyalayın.

![Visual Studio Code'da adres değeri ne kadar dır?](./media/ethereum-logic-app/contract-address-metadata.png)

## <a name="next-steps"></a>Sonraki adımlar

[Logic Apps ile daha fazlasını yapmak](https://channel9.msdn.com/Shows/Blocktalk/Doing-more-with-Logic-Apps?term=logic%20apps%20blockchain&lang-en=true)videodaki yaygın senaryoları izleyin.
