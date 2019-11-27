---
title: Azure blok zinciri hizmeti geliştirmeye genel bakış
description: Azure blok zinciri hizmeti 'nde çözüm geliştirmeye giriş.
ms.date: 11/22/2019
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: 3748a1ca473d817f536ba7c912d2485ffc14de2d
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455884"
---
# <a name="azure-blockchain-service-development-overview"></a>Azure blok zinciri hizmeti geliştirmeye genel bakış

Azure blok zinciri hizmeti sayesinde, varlık izleme, dijital belirteç, bağlılık ve geri alma, tedarik zinciri finansal ve provenance gibi kurumsal senaryoları etkinleştirmek için konsorsiyum blok zinciri ağları oluşturabilirsiniz. Aşağıdaki bölümlerde, kurumsal blok zinciri çözümlerini uygulamak için Azure blok zinciri hizmeti geliştirmesi tanıtılmaktadır.

## <a name="connecting-to-azure-blockchain-service"></a>Azure blok zinciri hizmetine bağlanılıyor

Blok zinciri ağları için tam düğümler, hafif düğümler ve uzak istemciler dahil farklı türlerde istemciler vardır. Azure blok zinciri hizmeti düğümleri içeren bir blok zinciri ağı oluşturur. Blok zinciri geliştirme için Azure blok zinciri hizmeti ağ geçidinizin olarak farklı istemcileri kullanabilirsiniz. Azure blok zinciri hizmeti, temel kimlik doğrulaması veya erişim anahtarını bir geliştirme uç noktası olarak sunar. Aşağıda, Bağlan ' a kullanabileceğiniz popüler istemciler verilmiştir.

### <a name="visual-studio-code"></a>Visual Studio Code

Azure blok zinciri geliştirme seti Visual Studio Code uzantısını kullanarak, konsorsiyum üyelerine bağlanabilirsiniz. Bir konsorsiyumun bağlantısı kurulduktan sonra, akıllı sözleşmeleri bir Azure blok zinciri hizmeti Consortium üyesine derleyebilir, oluşturabilir ve dağıtabilirsiniz.

Daha fazla bilgi için bkz. [hızlı başlangıç: Azure blok zinciri hizmeti Consortium ağına bağlanmak için Visual Studio Code kullanma](connect-vscode.md).

### <a name="metamask"></a>MetaMask

MetaMask, tarayıcı tabanlı bir cüzdan (uzak istemci), RPC istemcisi ve temel sözleşme Gezgini. Diğer tarayıcı waldinden farklı olarak, MetaMask bir Web3 örneğini tarayıcı JavaScript bağlamına çıkartır ve bu, çeşitli Ethereum blok zincirlerine (*mainnet*, *Ropsten testnet*, *kovan testnet*, Yerel RPC düğümü) bağlanan bir RPC istemcisi olarak davranır vb.). Azure blok zinciri hizmetine bağlanmak ve yeniden karışımı kullanarak blok zinciri geliştirmeye başlamak için özel RPC 'yi kolayca ayarlayabilirsiniz.

Daha fazla bilgi için bkz [. hızlı başlangıç: akıllı bir sözleşmeyi bağlamak ve dağıtmak Için MetaMask kullanma](connect-metamask.md)

### <a name="geth"></a>Geth

Geth, go 'da uygulanan tam Ethereum düğümünü çalıştırmaya yönelik komut satırı arabirimidir. Tam düğüm çalıştırmanız gerekmez, ancak Azure blok zinciri hizmeti ile etkileşim kurmak için JavaScript API 'sini ortaya çıkaran bir JavaScript çalışma zamanı ortamı sağlayan etkileşimli konsolunu da başlatabilirsiniz.

Daha fazla bilgi için bkz. [hızlı başlangıç: Azure blok zinciri hizmeti işlem düğümüne eklemek Için geth kullanma](connect-geth.md).

## <a name="development-framework-configuration"></a>Geliştirme çerçevesi yapılandırması

Gelişmiş kurumsal blok zinciri çözümleri geliştirmek için bir geliştirme çerçevesi, farklı blok zinciri ağlarına bağlanmak ve akıllı sözleşme yaşam döngülerini yönetmek için gereklidir.

Truffle, Ethereum blockzincirleri üzerinde merkezi olmayan uygulamalar yazmak, derlemek, dağıtmak ve test etmek için popüler bir blok zinciri geliştirme çerçevesidir. Ayrıca, Truffle ' yı akıllı sözleşme geliştirmeyi ve geleneksel web geliştirmeyi sorunsuz bir şekilde tümleştirme girişiminde bulunan bir çerçeve olarak düşünebilirsiniz.

Çoğu proje, en az iki blok zinciri düğümüyle etkileşime geçin. Geliştiriciler geliştirme sırasında yerel bir blok zinciri kullanır. Uygulama test veya yayın için hazırsanız, geliştirici bir blok zinciri ağına dağıtır. Örneğin, ana genel Ethereum ağı veya Azure blok zinciri hizmeti. Truffle, her ağ için akıllı sözleşmeleri derlemek ve dağıtmak için kullanılabilir ve son uygulama dağıtımını basitleştirir. Daha fazla bilgi için bkz. [hızlı başlangıç: Azure blok zinciri hizmeti ağına bağlanmak Için Truffle kullanma](connect-truffle.md).

## <a name="ethereum-quorum-private-transactions"></a>Ethereum çekirdeği özel işlemleri

Çekirdek, işlem artı sözleşme gizliliği ve yeni konsensus mekanizmalarına sahip bir Ethereum tabanlı dağıtılmış bir genel muhasebe protokolüdür. Go-Ethereum üzerinde önemli geliştirmeler şunları içerir:

* **Gizlilik** -çekirdek, genel ve özel durum ayrımı aracılığıyla özel işlemleri ve özel sözleşmeleri destekler ve özel verilerin ağ katılımcılarına yönlendirilme için eşler arası şifreli ileti değişimlerini kullanır.
* **Alternatif konsensus mekanizmaları** -iş açısından kanıtlama veya Stake consened, izinleri bulunan bir ağ için gerekli değildir. Çekirdek, Raft ve iBFT gibi Consortium zincirleri için tasarlanan birden çok konsensus mekanizması sunar.  Azure blok zinciri hizmeti, iBFT konsensus mekanizmasını kullanır.
* **Eş izinleri** -akıllı sözleşmeleri kullanarak düğüm ve eş izinleri yalnızca bilinen tarafların ağa katılabilmesini sağlar.
* **Daha yüksek performans** -çekirdek, genel olarak daha yüksek performans sunar.

## <a name="block-explorers"></a>Blok gezginleri

Blok araştırıcılar, bireysel blok içeriği, işlem adresi verileri ve geçmişi görüntüleyen çevrimiçi blok zinciri tarayıcılardır. Temel engelleme bilgilerine Azure blok zinciri hizmeti 'nde Azure Izleyici aracılığıyla ulaşılabilir. Ancak, geliştirme sırasında daha fazla ayrıntı bilgisine ihtiyacınız varsa, blok gezginleri yararlı olabilir.  Aşağıdaki blok araştırması, Azure blok zinciri hizmeti ile çalışır:

* Web3 Labs 'den [Epırus Azure blok zinciri hizmet Gezgini](https://azuremarketplace.microsoft.com/marketplace/apps/blk-technologies.azure-blockchain-explorer-template?tab=Overview)
* [BlockScout](https://github.com/Azure-Samples/blockchain/blob/master/ledger/template/ethereum-on-azure/technology-samples/blockscout/README.md)

Ayrıca, Blockzincirini Veri Yöneticisi ve Azure Cosmos DB kullanarak kendi blok Gezgininizi de oluşturabilirsiniz, bkz. [eğitim: blok zinciri veri Yöneticisi kullanarak Azure Cosmos DB veri gönderme](data-manager-cosmosdb.md).

## <a name="tps-measurement"></a>TPS ölçümü

Blok zinciri daha fazla kurumsal senaryolarda kullanıldığından, performans sorunlarını önlemek ve sistem verimsizlikleri önlemek için saniye başına işlem (TPS) hızı önemlidir. Yüksek işlem ücretleri, merkezi olmayan bir blok zinciri dahilinde devam etmek zor olabilir. Doğru bir TPS ölçümü, sunucu iş parçacığı, işlem kuyruğu boyutu, ağ gecikmesi ve güvenlik gibi farklı faktörlerden etkilenebilir. Geliştirme sırasında TPS hızını ölçmenize gerek duyuyorsanız, popüler bir açık kaynak aracı, bir [chaincepdir](https://github.com/drandreaskrueger/chainhammer).

## <a name="next-steps"></a>Sonraki adımlar

Azure blok zinciri hizmetinde bir Konsorsiyumu eklemek için Azure blok zinciri geliştirme seti 'ni kullanarak bir hızlı başlangıç yapın.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetine bağlanmak için Visual Studio Code kullanma](connect-vscode.md)