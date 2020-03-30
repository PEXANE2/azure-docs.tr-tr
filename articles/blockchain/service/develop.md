---
title: Azure Blockchain Hizmeti geliştirme genel bakış
description: Azure Blockchain Hizmeti'nde çözüm geliştirmeye giriş.
ms.date: 03/26/2020
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: 4a98e5ea025894303bc5c77bba0c6154a08315f2
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348372"
---
# <a name="azure-blockchain-service-development-overview"></a>Azure Blockchain Hizmeti geliştirme genel bakış

Azure Blockchain Hizmeti ile, varlık izleme, dijital belirteç, sadakat ve ödül, tedarik zinciri mali ve kaynak gibi kurumsal senaryoları etkinleştirmek için konsorsiyum blockchain ağları oluşturabilirsiniz. Aşağıdaki bölümler, kurumsal blockchain çözümlerini uygulamak için Azure Blockchain Hizmeti geliştirmesini tanıtMaktadır.

## <a name="connecting-to-azure-blockchain-service"></a>Azure Blockchain Hizmetine Bağlanma

Tam düğümler, ışık düğümleri ve uzak istemciler de dahil olmak üzere blockchain ağları için farklı istemci türleri vardır. Azure Blockchain Hizmeti düğümleri içeren bir blockchain ağı oluşturur. Blockchain geliştirme için Azure Blockchain Hizmeti'ne ağ geçidiniz olarak farklı istemciler kullanabilirsiniz. Azure Blockchain Hizmeti, geliştirme bitiş noktası olarak temel kimlik doğrulama veya erişim anahtarı sunar. Aşağıda connect'i kullanabileceğiniz popüler istemciler yer alıyor.

### <a name="visual-studio-code"></a>Visual Studio Code

Azure Blockchain Geliştirme Kiti Görsel Stüdyo Kodu uzantısını kullanarak konsorsiyum üyelerine bağlanabilirsiniz. Bir konsorsiyuma bağlandıktan sonra, bir Azure Blockchain Service konsorsiyumu üyesine akıllı sözleşmeler derleyebilir, oluşturabilir ve dağıtabilirsiniz.

Gelişmiş kurumsal blockchain çözümleri geliştirmek için, farklı blockchain ağlarına bağlanmak ve akıllı sözleşmeli yaşam döngülerini yönetmek için bir geliştirme çerçevesi gereklidir. Çoğu proje en az iki blockchain düğümü ile etkileşime girmektedir. Geliştiriciler geliştirme sırasında yerel bir blockchain kullanır. Uygulama test veya sürüm için hazır olduğunda, geliştirici bir blockchain ağına dağıtır. Örneğin, ana ortak Ethereum ağı veya Azure Blockchain Hizmeti. Visual Studio Code'daki Ethereum uzantısı için Azure Blockchain Geliştirme Kiti Truffle kullanır. Trüf mantarı, Ethereum blockchains'da merkezi olmayan uygulamaları yazmak, derlemek, dağıtmak ve test etmek için popüler bir blockchain geliştirme çerçevesidir. Ayrıca Truffle sorunsuz akıllı sözleşme geliştirme ve geleneksel web geliştirme entegre etmeye çalışır bir çerçeve olarak düşünebilirsiniz.

Daha fazla bilgi [için, Azure Blockchain Hizmeti konsorsiyum ağına bağlanmak için Quickstart: Visual Studio Code'u kullanın.](connect-vscode.md)

### <a name="metamask"></a>MetaMaske

MetaMask tarayıcı tabanlı cüzdan (uzak istemci), RPC istemcisi ve temel sözleşme gezginidir. Diğer tarayıcı cüzdanlarının aksine, MetaMask tarayıcı JavaScript bağlamına bir web3 örneği enjekte eder ve çeşitli Ethereum blockchain'lerine *(mainnet,* *Ropsten testneti,* *Kovan testneti,* yerel RPC düğümü, vb.) bağlanan bir RPC istemcisi olarak hareket eder. Azure Blockchain Hizmeti'ne bağlanmak ve Remix'i kullanarak blockchain geliştirmeye başlamak için özel RPC'yi kolayca ayarlayabilirsiniz.

Daha fazla bilgi için [Bkz. Quickstart: Akıllı bir sözleşmeyi bağlamak ve dağıtmak için MetaMask'i kullanın](connect-metamask.md)

### <a name="geth"></a>Geth

Geth, Go'da uygulanan tam bir Ethereum düğümunu çalıştırmak için komut satırı arabirimidir. Tam düğüm çalıştırmanız gerekmez, ancak Azure Blockchain Hizmeti ile etkileşimde kalmak için bir JavaScript API'sini ortaya çıkaran bir JavaScript çalışma zamanı ortamı sağlayan etkileşimli konsolunu başlatabilirsiniz.

Daha fazla bilgi [için, Bir Azure Blockchain Hizmeti hareket düğümüne eklemek için Geth kullanın: Hızlı Başlat'a](connect-geth.md)bakın.

## <a name="ethereum-quorum-private-transactions"></a>Ethereum Quorum özel işlemler

Quorum işlem artı sözleşme gizlilik ve yeni konsensüs mekanizmaları ile Ethereum tabanlı dağıtılmış bir genel muhasebe protokolüdür. Go-Ethereum üzerinde önemli geliştirmeler şunlardır:

* **Gizlilik** - Quorum, kamu ve özel devlet ayrımı yoluyla özel işlemleri ve özel sözleşmeleri destekler ve ağ katılımcılarına özel verilerin yönlendirilmiş aktarımı için eşler arası şifreli mesaj alışverişini kullanır.
* **Alternatif konsensüs mekanizmaları** - izin verilen bir ağ için iş kanıtı veya hisse kanıtı konsensüsü gerekmez. Quorum, RAFT ve IBFT gibi konsorsiyum zincirleri için tasarlanmış birden fazla konsensüs mekanizması sunmaktadır.Azure Blockchain Hizmeti IBFT konsensüs mekanizmasını kullanır.
* **Eş izni** - düğüm ve eş izinleri akıllı sözleşmeleri kullanarak yalnızca bilinen tarafların ağa katılmasını sağlar.
* **Yüksek Performans** - Quorum, public Geth'ten daha yüksek performans sunar.

## <a name="block-explorers"></a>Blok kaşifleri

Blok kaşifleri, tek tek blok içeriğini, işlem adresi verilerini ve geçmişini görüntüleyen çevrimiçi blockchain tarayıcılarıdır. Temel blok bilgileri Azure Blockchain Hizmeti'nde Azure Monitor aracılığıyla kullanılabilir. Ancak, geliştirme sırasında daha fazla ayrıntı bilgisine ihtiyacınız varsa, blok kaşifleri yararlı olabilir.  Aşağıdaki blok kaşifleri Azure Blockchain Hizmeti ile çalışır:

* Web3 Labs'dan [Epirus Azure Blockchain Servis Gezgini](https://azuremarketplace.microsoft.com/marketplace/apps/blk-technologies.azure-blockchain-explorer-template?tab=Overview)
* [BlockScout](https://github.com/Azure-Samples/blockchain/blob/master/ledger/template/ethereum-on-azure/technology-samples/blockscout/README.md)

Blockchain Data Manager ve Azure Cosmos DB'yi kullanarak kendi blok gezgininizi de oluşturabilirsiniz, [bkz.](data-manager-cosmosdb.md)

## <a name="tps-measurement"></a>TPS ölçümü

Blockchain daha fazla kurumsal senaryoda kullanıldığından, darboğazları ve sistem verimsizliklerini önlemek için saniye başına işlemler (TPS) hızı önemlidir. Yüksek işlem oranlarını merkezi olmayan bir blockchain içinde korumak zor olabilir. Doğru bir TPS ölçümü sunucu iş parçacığı, işlem sırası boyutu, ağ gecikmesi ve güvenlik gibi farklı etkenler tarafından etkilenebilir. Geliştirme sırasında TPS hızını ölçmek gerekiyorsa, popüler bir açık kaynak aracı [ChainHammer](https://github.com/drandreaskrueger/chainhammer)olduğunu.

## <a name="next-steps"></a>Sonraki adımlar

Azure Blockchain Hizmeti'ndeki bir konsorsiyuma bağlanmak üzere Ethereum için Azure Blockchain Geliştirme Kiti'ni kullanarak hızlı bir başlangıç yapmayı deneyin.

> [!div class="nextstepaction"]
> [Azure Blockchain Hizmetine bağlanmak için Visual Studio Kodunu kullanma](connect-vscode.md)