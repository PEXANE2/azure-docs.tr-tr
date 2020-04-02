---
title: Azure portalını kullanarak Blockchain Veri Yöneticisi'ni yapılandırma - Azure Blockchain Hizmeti
description: Azure portalını kullanarak Azure Blockchain Hizmeti için Blockchain Veri Yöneticisi oluşturun ve yönetin.
ms.date: 03/30/2020
ms.topic: article
ms.reviewer: ravastra
ms.openlocfilehash: beacef0660a253c90afb507618e8a1742f0c9d2d
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529608"
---
# <a name="configure-blockchain-data-manager-using-the-azure-portal"></a>Azure portalını kullanarak Blok Zinciri Veri Yöneticisi'ni yapılandırma

Blockchain verilerini yakalamak ve bir Azure Olay Izgara Konusuna göndermek için Azure Blockchain Hizmeti için Blockchain Veri Yöneticisi'ni yapılandırın.

Blockchain Veri Yöneticisi örneğini yapılandırmak için şunları

* Azure Blockchain Hizmeti hareket düğümü için Blockchain Veri Yöneticisi örneği oluşturma
* Blockchain uygulamalarınızı ekleyin

## <a name="prerequisites"></a>Ön koşullar

* [Tam Hızlı Başlatma: Azure portalını veya Quickstart'ı kullanarak blockchain üyesi oluşturun:](create-member.md) [Azure CLI'yi kullanarak bir Azure Blockchain Hizmeti blockchain üyesi oluşturun.](create-member-cli.md) Blockchain Data Manager kullanılırken Azure Blockchain Service *Standard* katmanı önerilir.
* Olay [Izgara Konusu](../../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) Oluşturma
* Azure [Etkinlik Kılavuz'undaki Olay işleyicileri](../../event-grid/event-handlers.md) hakkında bilgi edinin

## <a name="create-instance"></a>Örnek oluşturma

Blockchain Veri Yöneticisi örneği, bir Azure Blockchain Hizmeti hareket düğümünü bağlar ve izler. Yalnızca işlem düğümüne erişimi olan kullanıcılar bir bağlantı oluşturabilir. Bir örnek, işlem düğümündeki tüm ham blok ve ham hareket verilerini yakalar.

Giden bağlantı blockchain verilerini Azure Olay Grid'e gönderir. Örneği oluştururken tek bir giden bağlantıyı yapılandırırsınız. Blockchain Veri Yöneticisi, belirli bir Blockchain Veri Yöneticisi örneği için birden çok Olay Izgara Konu giden bağlantıları destekler. Blockchain verilerini tek bir hedefe gönderebilir veya blockchain verilerini birden çok hedefe gönderebilirsiniz. Başka bir hedef eklemek için, örne ek giden bağlantılar eklemeniz gerekiyor.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Blockchain Veri Yöneticisi'ne bağlanmak istediğiniz Azure Blockchain Hizmeti üyesine gidin. **Blockchain Veri Yöneticisi'ni**seçin.
1. **Ekle'yi**seçin.

    ![Blockchain Veri Yöneticisi Ekle](./media/data-manager-portal/add-instance.png)

    Şu ayrıntıları girin:

    Ayar | Açıklama
    --------|------------
    Adı | Bağlı bir Blockchain Veri Yöneticisi için benzersiz bir ad girin. Blockchain Data Manager adı küçük harfler ivedi lik ve sayılar içerebilir ve en fazla 20 karakter uzunluğundadır.
    İşlem düğümü | Bir hareket düğümü seçin. Yalnızca okuduğunuz işlem düğümleri listelenir.
    Bağlantı adı | Blockchain işlem verilerinin gönderildiği giden bağlantının benzersiz bir adını girin.
    Olay ızgara uç noktası | Blockchain Veri Yöneticisi örneğiyle aynı abonelikte bir olay ızgarası konusu seçin.

1. **Tamam'ı**seçin.

    Blockchain Veri Yöneticisi örneği oluşturmak bir dakikadan kısa sürer. Örnek dağıtıldıktan sonra otomatik olarak başlatılır. Çalışan blockchain veri yöneticisi örneği, işlem düğümündeki blockchain olaylarını yakalar ve giden bağlantılara veri gönderir.

    Yeni örnek, Azure Blockchain Hizmeti üyesi için Blockchain Veri Yöneticisi örnekleri listesinde görünür.

    ![Blockchain Data Üye örnekleri listesi](./media/data-manager-portal/instance-list.png)

## <a name="add-blockchain-application"></a>Blockchain uygulaması ekle

Blockchain uygulaması eklerseniz, Blockchain Data Manager uygulama nın olay ve özellik durumunu çözer. Aksi takdirde, yalnızca ham blok ve ham işlem verileri gönderilir. Blockchain Data Manager, sözleşme dağıtıldığında sözleşme adreslerini de keşfeder. Blockchain Data Manager örneğine birden çok blockchain uygulaması ekleyebilirsiniz.

> [!IMPORTANT]
> Şu anda, Solidity [dizi türlerini](https://solidity.readthedocs.io/en/v0.5.12/types.html#arrays) veya [eşleme türlerini](https://solidity.readthedocs.io/en/v0.5.12/types.html#mapping-types) bildiren blockchain uygulamaları tam olarak desteklenmez. Dizi veya eşleme türleri olarak bildirilen özellikler *ContractPropertiesMsg* veya *DecodedContractEventsMsg* iletilerinde deşifre edilmez.

Blockchain Data Manager akıllı bir sözleşme ABI gerektirir ve uygulama eklemek için bytecode dosyası dağıtıldı.

### <a name="get-contract-abi-and-bytecode"></a>Sözleşme ABI ve bytecode alın

SÖZLEŞME ABI akıllı sözleşme arayüzleri tanımlar. Akıllı sözleşmeyle nasıl etkileşime girilir açıklanır. Sözleşme ABI'yi panoya kopyalamak [için Ethereum uzantısı için Azure Blockchain Geliştirme Kiti'ni](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain) kullanabilirsiniz.

1. Visual Studio Code explorer bölmesinde Solidity projenizin **yapı/sözleşme** klasörünü genişletin.
1. Sözleşme meta data JSON dosyasını sağ tıklatın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. **Kopya Sözleşmesi ABI'yi**seçin.

    ![Copy Contract ABI seçimi ile Visual Studio Code bölmesi](./media/data-manager-portal/abi-devkit.png)

    SÖZLEŞME ABI panoya kopyalanır.

1. **Abi** dizisini JSON dosyası olarak kaydedin. Örneğin, *abi.json*. Dosyayı daha sonraki bir adımda kullanırsınız.

Blockchain Veri Yöneticisi, akıllı sözleşme için dağıtılan bytecode'u gerektirir. Dağıtılan bytecode akıllı sözleşme bytecode farklıdır. Bayt kodunu panoya kopyalamak için Azure blockchain geliştirme kiti uzantısını kullanırsınız.

1. Visual Studio Code explorer bölmesinde Solidity projenizin **yapı/sözleşme** klasörünü genişletin.
1. Sözleşme meta data JSON dosyasını sağ tıklatın. Dosya adı, **.json** uzantısı tarafından izlenen akıllı sözleşme adıdır.
1. **Hareket Kartkodunu Kopyala'yı**seçin.

    ![Copy Transaction Bytecode seçimi ile Visual Studio Code bölmesi](./media/data-manager-portal/bytecode-devkit.png)

    Bayt kodu panoya kopyalanır.

1. **Bayt kodu** değerini JSON dosyası olarak kaydedin. Örneğin, *bytecode.json*. Dosyayı daha sonraki bir adımda kullanırsınız.

Aşağıdaki örnek, VS Code düzenleyicisinde açık *olan abi.json* ve *bytecode.json* dosyalarını gösterir. Dosyalarınız benzer görünmelidir.

![abi.json ve bytecode.json dosyaları örneği](./media/data-manager-portal/contract-files.png)

### <a name="create-contract-abi-and-bytecode-url"></a>Sözleşme ABI ve bytecode URL'si oluşturma

Blockchain Data Manager, bir uygulama eklerken SÖZLEŞME ABI ve bytecode dosyalarına bir URL tarafından erişilebilmelidir. Özel olarak erişilebilen bir URL sağlamak için bir Azure Depolama hesabı kullanabilirsiniz.

#### <a name="create-storage-account"></a>Depolama hesabı oluştur

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

#### <a name="upload-contract-files"></a>Sözleşme dosyalarını yükleme

1. Depolama hesabı için yeni bir kapsayıcı oluşturun. **Kapsayıcı > Kapsayıcı'yı**seçin.

    ![Depolama hesabı kapsayıcısı oluşturma](./media/data-manager-portal/create-container.png)

    | Alan | Açıklama |
    |-------|-------------|
    | Adı  | Kapsayıcıya bir ad verin. Örneğin, *akıllı sözleşme* |
    | Genel erişim düzeyi | Özel'i seçin *(anonim erişim yok)* |

1. Kapsayıcıyı oluşturmak için **Tamam**'ı seçin.
1. Kapsayıcıyı seçin ve **yükle'yi**seçin.
1. [Sözleşme ABI'yi al ve bayt kodu](#get-contract-abi-and-bytecode) bölümünde oluşturduğunuz her iki JSON dosyasını seçin.

    ![Blob yükle](./media/data-manager-portal/upload-blobs.png)

    **Yükle'yi**seçin.

#### <a name="generate-url"></a>URL oluştur

Her blob için paylaşılan bir erişim imzası oluşturun.

1. ABI JSON blob'u seçin.
1. **SAS Oluştur'u** seçin
1. İstenilen erişim imza son kullanma tarihini ayarlayın ve ardından **blob SAS belirteci ve URL oluştur'u**seçin.

    ![SAS belirteci oluşturma](./media/data-manager-portal/generate-sas.png)

1. **Blob SAS URL'sini** kopyalayın ve bir sonraki bölüme kaydedin.
1. Baytkodu JSON blob için [URL oluştur](#generate-url) adımlarını yineleyin.

### <a name="add-application-to-instance"></a>Örneğine uygulama ekleme

1. Örnek listesinden Blockchain Veri Yöneticisi örneğini seçin.
1. **Blockchain uygulamalarını**seçin.
1. **Ekle'yi**seçin.

    ![Blockchain uygulaması ekleme](./media/data-manager-portal/add-application.png)

    Blockchain uygulamasının adını ve akıllı sözleşme ABI'sini ve bytecode URL'lerini girin.

    Ayar | Açıklama
    --------|------------
    Adı | Izlemek için blockchain uygulaması için benzersiz bir ad girin.
    Sözleşme ABI | Sözleşme ABI dosyasına URL yolu. Daha fazla bilgi için [bkz.](#create-contract-abi-and-bytecode-url)
    Sözleşme Bayt kodu | Bytecode dosyasına URL yolu. Daha fazla bilgi için [bkz.](#create-contract-abi-and-bytecode-url)

1. **Tamam'ı**seçin.

    Uygulama oluşturulduktan sonra, uygulama blockchain uygulamaları listesinde görünür.

    ![Blockchain uygulama listesi](./media/data-manager-portal/artifact-list.png)

Azure Depolama hesabını silebilir veya daha fazla blockchain uygulamasını yapılandırmak için kullanabilirsiniz. Azure Depolama hesabını silmek isterseniz, kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili depolama hesabını ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

## <a name="stop-instance"></a>Örneği durdur

Blockchain olaylarını yakalamayı ve giden bağlantılara veri göndermeyi durdurmak istediğinizde Blockchain Yöneticisi örneğini durdurun. Örnek durdurulduğunda, Blockchain Veri Yöneticisi için herhangi bir ücret alınmaz. Daha fazla bilgi için bkz. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/blockchain-service).

1. Genel **Bakış'a** gidin ve **Durdur'u**seçin.

    ![Örneği durdur](./media/data-manager-portal/stop-instance.png)

## <a name="next-steps"></a>Sonraki adımlar

Blockchain Data Manager ve Azure Cosmos DB'yi kullanarak blockchain işlem iletisi gezgini oluşturan bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure Cosmos DB'ye veri göndermek için Blok Zinciri Veri Yöneticisi'ni kullanma](data-manager-cosmosdb.md)