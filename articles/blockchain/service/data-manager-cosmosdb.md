---
title: Azure Cosmos DB - Azure Blockchain Hizmetini güncellemek için Blockchain Veri Yöneticisi'ni kullanın
description: Blockchain verilerini Azure Cosmos DB'ye göndermek için Azure Blockchain Hizmeti için Blockchain Veri Yöneticisi'ni kullanın
ms.date: 03/08/2020
ms.topic: tutorial
ms.reviewer: chroyal
ms.openlocfilehash: 483a5246274f63549dfb2914361ede6aa001e02e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79533190"
---
# <a name="tutorial-use-blockchain-data-manager-to-send-data-to-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB'ye veri göndermek için Blockchain Veri Yöneticisi'ni kullanın

Bu eğitimde, Blockchain işlem verilerini Azure Cosmos DB'ye kaydetmek için Azure Blockchain Hizmeti için Blockchain Veri Yöneticisi'ni kullanırsınız. Blockchain Data Manager, Blockchain genel muhasebe verilerini Azure Olay Ağı Konuları'na yakalar, dönüştürür ve sunar. Azure Etkinlik Izgara'dan, Azure Cosmos DB veritabanında belge oluşturmak için bir Azure Logic App bağlayıcısı kullanırsınız. Öğretici ile bittiğinde, Azure Cosmos DB Data Explorer'da blockchain işlem verilerini keşfedebilirsiniz.

[![Blockchain işlem detayı](./media/data-manager-cosmosdb/raw-msg.png)](./media/data-manager-cosmosdb/raw-msg.png#lightbox)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Blockchain Veri Yöneticisi örneği oluşturma
> * İşlem özelliklerini ve olaylarını çözmek için blockchain uygulaması ekleme
> * İşlem verilerini depolamak için bir Azure Cosmos DB hesabı ve veritabanı oluşturma
> * Azure Etkinlik Izgara Sını Azure Cosmos DB'ye bağlamak için Bir Azure Mantık Uygulaması Oluşturun
> * Bir işlemi blockchain genel muhasebesine gönderme
> * Azure Cosmos DB'de çözülmüş işlem verilerini görüntüleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Tam [Hızlı Başlatma: Azure portalını veya Quickstart'ı kullanarak blockchain üyesi oluşturun:](create-member.md) [Azure CLI'yi kullanarak bir Azure Blockchain Hizmeti blockchain üyesi oluşturun](create-member-cli.md)
* Tam [Quickstart: Azure Blockchain Hizmeti konsorsiyum ağına bağlanmak için Visual Studio Kodunu kullanın.](connect-vscode.md) Hızlı başlatma, [Ethereum için Azure Blockchain Geliştirme Kiti'ni](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) yükleyip blockchain geliştirme ortamınızı ayarlamanıza rağmen size yol gösteriyor.
* Komple [Öğretici: Akıllı sözleşmeler oluşturmak, oluşturmak ve dağıtmak için Visual Studio Kodunu kullanın.](send-transaction.md) Öğretici örnek bir akıllı sözleşme oluşturma yoluyla yürür.
* Olay [Izgara Konusu](../../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) Oluşturma
* Azure [Etkinlik Kılavuz'undaki Olay işleyicileri](../../event-grid/event-handlers.md) hakkında bilgi edinin

## <a name="create-instance"></a>Örnek oluşturma

Blockchain Veri Yöneticisi örneği, bir Azure Blockchain Hizmeti hareket düğümünü bağlar ve izler. Bir örnek, işlem düğümündeki tüm ham blok ve ham hareket verilerini yakalar. Giden bağlantı blockchain verilerini Azure Olay Grid'e gönderir. Örneği oluştururken tek bir giden bağlantıyı yapılandırırsınız.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Quickstart ön koşulunda oluşturduğunuz Azure Blockchain Hizmeti [üyesine gidin: Azure portalını kullanarak bir blockchain üyesi oluşturun.](create-member.md) **Blockchain Veri Yöneticisi'ni**seçin.
1. **Ekle'yi**seçin.

    ![Blockchain Veri Yöneticisi Ekle](./media/data-manager-cosmosdb/add-instance.png)

    Şu ayrıntıları girin:

    Ayar | Örnek | Açıklama
    --------|---------|------------
    Adı | mywatcher | Bağlı bir Blockchain Veri Yöneticisi için benzersiz bir ad girin.
    İşlem düğümü | myblockchainmember | Ön koşulda oluşturduğunuz Azure Blockchain Hizmeti üyesinin varsayılan işlem düğümünün seçeneğini belirleyin.
    Bağlantı adı | cosmosdb | Blockchain işlem verilerinin gönderildiği giden bağlantının benzersiz bir adını girin.
    Olay ızgara uç noktası | myTopic | Ön koşulda oluşturduğunuz bir olay ızgarası konusunu seçin. Not: Blockchain Veri Yöneticisi örneği ve olay ızgarası konusu aynı abonelikte olmalıdır.

1. **Tamam'ı**seçin.

    Blockchain Veri Yöneticisi örneği oluşturmak bir dakikadan kısa sürer. Örnek dağıtıldıktan sonra otomatik olarak başlatılır. Çalışan blockchain veri yöneticisi örneği, işlem düğümündeki blockchain olaylarını yakalar ve olay ızgarasına veri gönderir.

## <a name="add-application"></a>Uygulama ekleme

Blockchain Data Manager olay ve özellik durumunu çözebilmek için **helloblockchain** blockchain uygulamasını ekleyin. Blockchain Data Manager uygulama eklemek için akıllı sözleşme ABI ve bytecode dosyası gerektirir.

### <a name="get-contract-abi-and-bytecode"></a>Sözleşme ABI ve bytecode alın

SÖZLEŞME ABI akıllı sözleşme arayüzleri tanımlar. Akıllı sözleşmeyle nasıl etkileşime girilir açıklanır. Sözleşme ABI'yi panoya kopyalamak [için Ethereum uzantısı için Azure Blockchain Geliştirme Kiti'ni](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) kullanabilirsiniz.

1. Visual Studio Code explorer bölmesinde, ön koşul Öğreticisinde oluşturduğunuz **helloblockchain** Solidity projesinin **yapı/sözleşme** klasörünü genişletin: [Akıllı sözleşmeler oluşturmak, oluşturmak ve dağıtmak için Visual Studio Code'u kullanın.](send-transaction.md)
1. Sözleşme meta data JSON dosyasını sağ tıklatın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. **Kopya Sözleşmesi ABI'yi**seçin.

    ![Copy Contract ABI seçimi ile Visual Studio Code bölmesi](./media/data-manager-cosmosdb/abi-devkit.png)

    SÖZLEŞME ABI panoya kopyalanır.

1. **Abi** dizisini JSON dosyası olarak kaydedin. Örneğin, *abi.json*. Dosyayı daha sonraki bir adımda kullanırsınız.

Blockchain Veri Yöneticisi, akıllı sözleşme için dağıtılan bytecode'u gerektirir. Dağıtılan bytecode akıllı sözleşme bytecode farklıdır. Bayt kodunu panoya kopyalamak için Azure blockchain geliştirme kiti uzantısını kullanırsınız.

1. Visual Studio Code explorer bölmesinde Solidity projenizin **yapı/sözleşme** klasörünü genişletin.
1. Sözleşme meta data JSON dosyasını sağ tıklatın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. **Hareket Kartkodunu Kopyala'yı**seçin.

    ![Copy Transaction Bytecode seçimi ile Visual Studio Code bölmesi](./media/data-manager-cosmosdb/bytecode-devkit.png)

    Bayt kodu panoya kopyalanır.

1. **Bayt kodu** değerini JSON dosyası olarak kaydedin. Örneğin, *bytecode.json*. Dosyayı daha sonraki bir adımda kullanırsınız.

Aşağıdaki örnek, VS Code düzenleyicisinde açık *olan abi.json* ve *bytecode.json* dosyalarını gösterir. Dosyalarınız benzer görünmelidir.

![abi.json ve bytecode.json dosyaları örneği](./media/data-manager-cosmosdb/contract-files.png)

### <a name="create-contract-abi-and-bytecode-url"></a>Sözleşme ABI ve bytecode URL'si oluşturma

Blockchain Data Manager, bir uygulama eklerken SÖZLEŞME ABI ve bytecode dosyalarına bir URL tarafından erişilebilmelidir. Özel olarak erişilebilen bir URL sağlamak için bir Azure Depolama hesabı kullanabilirsiniz.

#### <a name="create-storage-account"></a>Depolama hesabı oluştur

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

#### <a name="upload-contract-files"></a>Sözleşme dosyalarını yükleme

1. Depolama hesabı için yeni bir kapsayıcı oluşturun. **Kapsayıcı > Kapsayıcı'yı**seçin.

    ![Depolama hesabı kapsayıcısı oluşturma](./media/data-manager-cosmosdb/create-container.png)

    | Ayar | Açıklama |
    |---------|-------------|
    | Adı  | Kapsayıcıya bir ad verin. Örneğin, *akıllı sözleşme* |
    | Genel erişim düzeyi | Özel'i seçin *(anonim erişim yok)* |

1. Kapsayıcıyı oluşturmak için **Tamam**'ı seçin.
1. Kapsayıcıyı seçin ve **yükle'yi**seçin.
1. [Sözleşme ABI'yi al ve bayt kodu](#get-contract-abi-and-bytecode) bölümünde oluşturduğunuz her iki JSON dosyasını seçin.

    ![Blob yükle](./media/data-manager-cosmosdb/upload-blobs.png)

    **Yükle'yi**seçin.

#### <a name="generate-url"></a>URL oluştur

Her blob için paylaşılan bir erişim imzası oluşturun.

1. ABI JSON blob'u seçin.
1. **SAS Oluştur'u** seçin
1. İstenilen erişim imza son kullanma tarihini ayarlayın ve ardından **blob SAS belirteci ve URL oluştur'u**seçin.

    ![SAS belirteci oluşturma](./media/data-manager-cosmosdb/generate-sas.png)

1. **Blob SAS URL'sini** kopyalayın ve bir sonraki bölüme kaydedin.
1. Baytkodu JSON blob için [URL oluştur](#generate-url) adımlarını yineleyin.

### <a name="add-helloblockchain-application-to-instance"></a>Örneğine helloblockchain uygulaması ekle

1. Örnek listesinden Blockchain Veri Yöneticisi örneğini seçin.
1. **Blockchain uygulamalarını**seçin.
1. **Ekle'yi**seçin.

    ![Blockchain uygulaması ekleme](./media/data-manager-cosmosdb/add-application.png)

    Blockchain uygulamasının adını ve akıllı sözleşme ABI'sini ve bytecode URL'lerini girin.

    Ayar | Açıklama
    --------|------------
    Adı | Izlemek için blockchain uygulaması için benzersiz bir ad girin.
    Sözleşme ABI | Sözleşme ABI dosyasına URL yolu. Daha fazla bilgi için [bkz.](#create-contract-abi-and-bytecode-url)
    Sözleşme Bayt kodu | Bytecode dosyasına URL yolu. Daha fazla bilgi için [bkz.](#create-contract-abi-and-bytecode-url)

1. **Tamam'ı**seçin.

    Uygulama oluşturulduktan sonra, uygulama blockchain uygulamaları listesinde görünür.

    ![Blockchain uygulama listesi](./media/data-manager-cosmosdb/artifact-list.png)

Azure Depolama hesabını silebilir veya daha fazla blockchain uygulamasını yapılandırmak için kullanabilirsiniz. Azure Depolama hesabını silmek isterseniz, kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili depolama hesabını ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

## <a name="create-azure-cosmos-db"></a>Azure Cosmos DB oluşturma

[!INCLUDE [cosmos-db-create-storage-account](../../../includes/cosmos-db-create-dbaccount.md)]

### <a name="add-a-database-and-container"></a>Veritabanı ve kapsayıcı ekleme

Bir veritabanı ve kapsayıcı oluşturmak için Azure portalındaki Veri Gezgini'ni kullanabilirsiniz.

1. Azure Cosmos DB hesap sayfanızdaki sol gezintiden **Veri Gezgini'ni** seçin ve ardından Yeni **Kapsayıcı'yı**seçin.
1. Ekle **kapsayıcı** bölmesine, yeni kapsayıcının ayarlarını girin.

    ![Kapsayıcı ayarları ekleme](./media/data-manager-cosmosdb/add-container.png)

    | Ayar | Açıklama
    |---------|-------------|
    | Veritabanı Kimliği | **Blockchain verilerini** yeni veritabanının adı olarak girin. |
    | Aktarım hızı | Çıktıyı saniyede **400** istek biriminde (RU/s) bırakın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.|
    | Kapsayıcı Kimliği | **İletileri** yeni kapsayıcınızın adı olarak girin. |
    | Bölüm anahtarı | **/MessageType'ı** bölüm anahtarı olarak kullanın. |

1. **Tamam'ı**seçin. Veri Gezgini yeni veritabanını ve oluşturduğunuz kapsayıcıyı görüntüler.

## <a name="create-logic-app"></a>Mantıksal Uygulama oluşturma

Azure Logic Apps, sistemleri ve hizmetleri tümleştirmeniz gerektiğinde iş süreçlerini ve iş akışlarını zamanlamanıza ve otomatikleştirmenize yardımcı olur. Olay Izgarasını Azure Cosmos DB'ye bağlamak için bir mantık uygulaması kullanabilirsiniz.

1. [Azure portalında](https://portal.azure.com)**Kaynak oluştur** > **Tümleştirme** > **Mantıksal Uygulama**’yı seçin.
1. Mantık uygulamanızı nerede oluşturacağınıza ilişkin ayrıntıları sağlayın. İşin bittikten sonra **Oluştur'u**seçin.

    Mantık uygulamaları oluşturma hakkında daha fazla bilgi için [bkz.](../../logic-apps/quickstart-create-first-logic-app-workflow.md)

1. Azure uygulamanızı dağıttıktan sonra mantık uygulama kaynağınızı seçin.
1. Logic Apps Designer'da **Şablonlar**altında **Boş Mantık Uygulaması'nı**seçin.

### <a name="add-event-grid-trigger"></a>Olay Izgara tetikleyicisi ekle

Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur. Event Grid'den Cosmos DB'ye blockchain işlem verilerini gönderirken Azure Olay Ağı tetikleyicisi kullanın.

1. Logic Apps Tasarımcısı'nda **Azure Olay Ağı** bağlayıcısını arayın ve seçin.
1. **Tetikleyiciler** sekmesinden kaynak **olayı oluştuğunda**seçin.
1. Olay Izgara Konunuza bir API bağlantısı oluşturun.

    ![Olay ızgara tetikleme ayarları](./media/data-manager-cosmosdb/event-grid-trigger.png)

    | Ayar | Açıklama
    |---------|-------------|
    | Abonelik | Olay Izgara Konusu'nu içeren aboneliği seçin. |
    | Kaynak Türü | **Microsoft.EventGrid.Topics'i**seçin. |
    | Kaynak Adı | Blockchain Veri Yöneticisi'nin işlem veri iletileri gönderdiği Olay Kılavuz Konusu'nun adını seçin. |

### <a name="add-cosmos-db-action"></a>Cosmos DB eylemi ekle

Her işlem için Cosmos DB'de belge oluşturmak için bir eylem ekleyin. İletileri kategorilere ayırmak için işlem iletisi türünü bölüm anahtarı olarak kullanın.

1. **Yeni adım**'ı seçin.
1. **Bir eylemi seçin,** **Azure Cosmos DB'yi**arayın.
1. **Belge oluşturma veya güncelleştirme > Azure Cosmos DB > Eylemleri'ni**seçin.
1. Cosmos DB veritabanınıza bir API bağlantısı oluşturun.

    ![Cosmos DB bağlantı ayarları](./media/data-manager-cosmosdb/cosmosdb-connection.png)

    | Ayar | Açıklama
    |---------|-------------|
    | Bağlantı Adı | Olay Izgara Konusu'nu içeren aboneliği seçin. |
    | DocumentDB Hesabı | [Azure Cosmos DB hesap oluşturma](#create-azure-cosmos-db) bölümünde oluşturduğunuz DocumentDB hesabını seçin. |

1. Veritabanı ve [kapsayıcı ekle](#add-a-database-and-container) bölümünde daha önce oluşturduğunuz Azure Cosmos DB'niz için Veritabanı **Kimliği** ve **Koleksiyon Kimliği'ni** girin.

1. **Belge** ayarını seçin. Dinamik *içerik* açılır ekle'de **İfade'yi** seçin ve aşağıdaki ifadeyi kopyalayıp yapıştırın:

    ```
    addProperty(triggerBody()?['data'], 'id', utcNow())
    ```

    İfade iletinin veri bölümünü alır ve kimliği zaman damgası değerine ayarlar.

1. **Yeni parametre ekle'yi** seçin ve Partition anahtar **değerini**seçin.
1. Bölüm **anahtar değerini** `"@{triggerBody()['data']['MessageType']}"`' olarak ayarlama Değer çift tırnak işaretleri ile çevrili olmalıdır.

    ![Cosmos DB ayarları ile Mantık Uygulamaları Tasarımcısı](./media/data-manager-cosmosdb/create-action.png)

    Değer, hareket iletisi türüne bölüm anahtarı ayarlar.

1. **Kaydet'i**seçin.

Mantık uygulaması Olay Izgara Konu izler. Blockchain Data Manager'dan yeni bir işlem iletisi gönderildiğinde, mantık uygulaması Cosmos DB'de bir belge oluşturur.

## <a name="send-a-transaction"></a>İşlem gönderme

Ardından, ne oluşturduğunuzu sınamak için blockchain genel muhasebesine bir hareket gönderin. Önceden koşul Öğretici'de oluşturduğunuz **HelloBlockchain** sözleşmesinin **SendRequest** işlevini [kullanın: Akıllı sözleşmeler oluşturmak, oluşturmak ve dağıtmak için Visual Studio Kodunu kullanın.](send-transaction.md)

1. **SendRequest** işlevini aramak için Azure Blockchain Geliştirme Kiti akıllı sözleşme etkileşim sayfasını kullanın. **HelloBlockchain.sol'a** sağ tıklayın ve menüden **Akıllı Sözleşme Etkileşim Sayfasını Göster'i** seçin.

    ![Menüden Akıllı Sözleşme Etkileşim sayfasını göster'i seçin](./media/data-manager-cosmosdb/contract-interaction.png)

1. **SendRequest** sözleşme eylemini seçin ve **Hello, Blockchain'e girin!** **requestMessage** parametresi için. Bir işlem aracılığıyla **SendRequest** işlevini çağırmak için Yürüt'''ün'u seçin. **Execute**

    ![SendRequest eylemini yürütme](./media/data-manager-cosmosdb/sendrequest-action.png)

SendRequest işlevi **İstek İletisi** ve **Durum** alanlarını ayarlar. **RequestMessage** için geçerli durum **Merhaba, Blockchain**geçti bağımsız değişkendir. **Durum** alan değeri **İstek**kalır.

## <a name="view-transaction-data"></a>Hareket verilerini görüntüleme

Blockchain Veri Yöneticinizi Azure Cosmos DB'ye bağladığınızda, Cosmos DB Veri Gezgini'ndeki blockchain işlem iletilerini görüntüleyebilirsiniz.

1. Cosmos DB Data Explorer görünümüne gidin. Örneğin, **cosmosdb-blockchain > Data Explorer > blockchain-data > Mesajlar > Öğeler.**

    ![Cosmos DB Veri Gezgini](./media/data-manager-cosmosdb/data-explorer.png)

    Veri Gezgini, Cosmos DB veritabanında oluşturulan blockchain veri iletilerini listeler.

1. Öğe kimliğini seçerek iletilere göz atın ve eşleşen işlem karmaile iletiyi bulun.

    [![Blockchain işlem detayı](./media/data-manager-cosmosdb/raw-msg.png)](./media/data-manager-cosmosdb/raw-msg.png#lightbox)

    Ham işlem iletisi, hareketle ilgili ayrıntıları içerir. Ancak, özellik bilgileri şifrelenir.

    Blockchain Veri Yöneticisi örneğine HelloBlockchain akıllı sözleşmesini eklediğiniz için, deşifre edilmiş özellik bilgilerini içeren bir **ContractProperties** ileti türü de gönderilir.

1. Hareket için **Sözleşme Özellikleri** iletisini bulun. Listedeki bir sonraki mesaj olmalı.

    [![Blockchain işlem detayı](./media/data-manager-cosmosdb/properties-msg.png)](./media/data-manager-cosmosdb/properties-msg.png#lightbox)

    **DecodedProperties** dizisi işlemin özelliklerini içerir.

Tebrikler! Blockchain Data Manager ve Azure Cosmos DB'yi kullanarak başarılı bir işlem iletisi gezgini oluşturdunuz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu öğretici için kullandığınız kaynakları ve kaynak gruplarını silebilirsiniz. Kaynak grubunu silmek için:

1. Azure portalında, sol gezinti bölmesindeki **Kaynak grubuna** gidin ve silmek istediğiniz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin. Kaynak grup adını girerek silme işlemini doğrulayın ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Blockchain genel muhasebeleri ile tümleştirme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure Logic Apps ile Ethereum Blockchain konektörünü kullanma](ethereum-logic-app.md)
