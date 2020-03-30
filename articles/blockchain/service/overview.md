---
title: Azure Blockchain Hizmetine genel bakış
description: Azure Blockchain Hizmetine Genel Bakış
ms.date: 03/12/2020
ms.topic: overview
ms.reviewer: ravastra
ms.openlocfilehash: b9f5deb501fb93327fa5d5cfcfd5bb583ed6135e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79205078"
---
# <a name="what-is-azure-blockchain-service"></a>Azure Blok Zinciri Hizmeti nedir?

Azure Blockchain Hizmeti, kullanıcıların Azure'da blockchain ağlarını ölçekte büyütmelerine ve çalıştırmalarına olanak tanıyan tam olarak yönetilen bir genel muhasebe hizmetidir. Azure Blockchain Service, hem altyapı yönetimi hem de blockchain ağ yönetimi için birleşik denetim sağlayarak şunları sağlar:

* Basit ağ dağıtımı ve işlemleri
* Dahili konsorsiyum yönetimi
* Tanıdık geliştirme araçlarıyla akıllı sözleşmeler geliştirin

Azure Blockchain Hizmeti, birden çok genel muhasebe protokolünü desteklemek üzere tasarlanmıştır. Şu anda, [IBFT](https://github.com/jpmorganchase/quorum/wiki/Quorum-Consensus) konsensüs mekanizmasını kullanarak Ethereum [Quorum](https://www.goquorum.com/) defteriiçin destek sağlar.

Bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Sanal makineleri ve altyapıyı yönetmek için zaman ve kaynak ayırmak yerine uygulama geliştirme ve iş mantığına odaklanabilirsiniz. Buna ek olarak, yeni beceriler öğrenmek zorunda kalmadan çözümlerinizi sunmak için seçtiğiniz açık kaynak araçları ve platformu ile uygulamanızı geliştirmeye devam edebilirsiniz.

## <a name="network-deployment-and-operations"></a>Ağ dağıtımı ve işlemler

Azure Blockchain Hizmetini dağıtma, Azure portalı, Azure CLI veya Azure Blockchain uzantısı kullanılarak Visual Studio kodu üzerinden yapılır. Dağıtım, hem işlem hem de doğrulayıcı düğümleri, güvenlik yalıtımı için Azure Sanal Ağları ve hizmet tarafından yönetilen depolama sağlama da dahil olmak üzere basitleştirilmiştir.  Ayrıca, yeni bir blockchain üyesi ni dağıtırken, kullanıcılar bir konsorsiyum oluşturur veya katılır.  Konsorsiyumlar, farklı Azure aboneliklerinde birden fazla tarafın paylaşılan bir blockchain'de birbirleriyle güvenli bir şekilde iletişim kurabilmesini sağlar.  Bu basitleştirilmiş dağıtım, blockchain ağ dağıtımını günlerden dakikaya azaltır.

### <a name="performance-and-service-tiers"></a>Performans ve hizmet katmanları

Azure Blockchain Hizmeti iki hizmet katmanı sunar: *Temel* ve *Standart.* Her katman, hafif geliştirmeyi desteklemek ve büyük ölçekli üretim blockchain dağıtımlarına kadar iş yüklerini test etmek için farklı performans ve yetenekler sunar. Kavramların geliştirilmesi, test edilmesi ve kanıtedilmesi için *Temel* katmanı kullanın. Üretim sınıfı dağıtımları için *Standart* katmanı kullanın. Her iki katmanda en az bir işlem düğümü ve bir doğrulayıcı düğüm (Temel) veya iki doğrulayıcı düğüm (Standart) bulunur. 

![Fiyatlandırma katmanları](./media/overview/pricing-tiers.png)

Standart *katman,* iki doğrulayıcı düğüm sunmanın yanı sıra, her işlem ve geçerlidüğüm için 2 *vCore* sağlarken, Temel katman 1 vCore yapılandırması sunar.  İşlem ve doğrulayıcı düğümler için 2 vCore sunarak, 1 vCore Quorum genel muhasebesine adanabilirken, kalan 1 vCore diğer altyapı ile ilgili hizmetler için kullanılabilir ve üretim blockchain iş yükleri için en iyi performansı sağlar. Fiyatlandırma ayrıntıları hakkında daha fazla bilgi için [Azure Blockchain Hizmeti fiyatlandırması](https://azure.microsoft.com/pricing/details/blockchain-service)için bkz.

### <a name="security-and-maintenance"></a>Güvenlik ve bakım

İlk blockchain üyenizi temin ettikten sonra, üyenize ek işlem düğümleri ekleme olanağınız vardır.  Varsayılan olarak, hareket düğümleri güvenlik duvarı kuralları yla güvenli hale alınır ve erişim için yapılandırma gerektirir.  Ayrıca, tüm işlem düğümleri TLS üzerinden hareket halindeki verileri şifreler.  Güvenlik duvarı kuralları, temel kimlik doğrulama, erişim anahtarları ve Azure Etkin Dizin tümleştirmesi gibi işlem düğümü erişimini güvence altına almak için birden çok seçenek vardır. Daha fazla bilgi için [işlem düğümlerini yapılandırma](configure-transaction-nodes.md) ve [Azure Etkin Dizin erişimini yapılandırma bölümüne](configure-aad.md)bakın.

Yönetilen bir hizmet olarak Azure Blockchain Hizmeti, blockchain üyenizin düğümlerinin en son ana bilgisayar işletim sistemi ve genel muhasebe yazılım yığını güncelleştirmeleriyle birlikte yamalanmasını ve yüksek kullanılabilirlik için yapılandırılmasını sağlar (yalnızca Standart katman), DevOps'lerin çoğunu ortadan kaldırır geleneksel IaaS blockchain düğümleri için gereklidir.  Düzeltme ve güncellemeler hakkında daha fazla bilgi için [desteklenen Azure Blockchain Hizmeti genel muhasebe sürümlerine](ledger-versions.md)bakın.

### <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

Buna ek olarak, Azure Blockchain Hizmeti, düğümlerin CPU'su, belleği ve depolama kullanımı hakkında öngörüler sağlayan Azure Monitor Hizmeti aracılığıyla zengin ölçümler sağlar.  Azure Monitor ayrıca, işlemler ve bloklar, işlem sıra derinliği ve etkin bağlantılar gibi blockchain ağ etkinliği hakkında yararlı bilgiler de sağlar.  Ölçümler, blockchain uygulamanız için önemli olan öngörülere ilişkin görünümler sağlamak için özelleştirilebilir.  Ayrıca, eşikler, kullanıcıların e-posta veya kısa mesaj gönderme, Mantık Uygulaması, Azure İşlevi veya özel tanımlı bir web hook'a gönderme gibi eylemleri tetiklemelerine olanak tanıyan uyarılar aracılığıyla tanımlanabilir.

![Ölçümler](./media/overview/metrics.png)

Azure Günlük Analitiği aracılığıyla, kullanıcılar Quorum genel muhasebesi ile ilgili günlükleri veya işlem düğümlerine bağlantı girişimi gibi diğer önemli bilgileri görüntüleyebilir.

## <a name="built-in-consortium-management"></a>Dahili konsorsiyum yönetimi

İlk blockchain üyenizi dağıtırken, yeni bir konsorsiyuma katılır veya oluşturursunuz.  Konsorsiyum, çok partili bir süreçte işlem yapan blockchain üyeleri arasındaki yönetişim ve bağlantıyı yönetmek için kullanılan mantıksal bir gruptur.  Azure Blockchain Hizmeti, konsorsiyumdaki üyelerin hangi eylemleri alabileceğini belirleyen önceden tanımlanmış akıllı sözleşmeler aracılığıyla yerleşik yönetim denetimleri sağlar.  Bu yönetim denetimleri konsorsiyum yöneticisi tarafından gerektiği gibi özelleştirilebilir. Yeni bir konsorsiyum oluşturduğunuzda, blockchain üyeniz konsorsiyumun varsayılan yöneticisi dir ve diğer tarafları konsorsiyumunuza katılmaya davet etme olanağı sağlar.  Bir konsorsiyuma yalnızca daha önce davet edilmişseniz katılabilirsiniz.  Bir konsorsiyuma katılırken, blockchain üyeniz konsorsiyum yöneticisi tarafından uygulamaya konan yönetim denetimlerine tabidir.

![Konsorsiyum yönetimi](./media/overview/consortium.png)

Konsorsiyumdan üye ekleme ve çıkarma gibi konsorsiyum yönetim eylemlerine PowerShell ve REST API üzerinden erişilebilir. Sağlamlık tabanlı akıllı sözleşmeleri değiştirmek ve göndermek yerine ortak arabirimleri kullanarak bir konsorsiyumu programlı olarak yönetebilirsiniz. Daha fazla bilgi için [konsorsiyum yönetimine](consortium.md)bakın.

## <a name="develop-using-familiar-development-tools"></a>Bildiğiniz geliştirme araçlarını kullanarak geliştirme

Açık kaynaklı Quorum Ethereum defterine dayanarak, Azure Blockchain Hizmeti için uygulamaları, varolan Ethereum uygulamalarında olduğu gibi geliştirebilirsiniz. Azure Blockchain Development Kit Visual Studio Code uzantısı, önde gelen endüstri ortaklarıyla çalışarak, geliştiricilerin akıllı sözleşmeler oluşturmak için Truffle Suite gibi tanıdık araçlardan yararlanmalarına olanak tanır. Azure Blockchain Geliştirme Kiti uzantısını kullanarak, geliştiriciler akıllı sözleşmelerinizi tek bir IDE'den oluşturup dağıtabilmeniz için konsorsiyum oluşturabilir veya bunlara bağlanabilir ve varolan konsorsiyuma bağlanabilir. Azure Blockchain Visual Studio Code uzantısını kullanarak, akıllı sözleşmelerinizi tek bir IDE'den oluşturup dağıtabilmek için varolan bir konsorsiyum oluşturabilir veya bunlara bağlanabilirsiniz. Daha fazla bilgi için [VS Kodu pazar yerindeki Azure Blockchain Geliştirme Kiti'ne](https://aka.ms/vscodebcextension) ve Azure [Blockchain Geliştirme Kiti kullanım kılavuzuna](https://aka.ms/vscodebcextensionwiki)bakın.

## <a name="publish-blockchain-data"></a>Blockchain verilerini yayımlama

Azure Blockchain Hizmeti için Blockchain Veri Yöneticisi, Azure hizmetleriyle güvenilir ve ölçeklenebilir blockchain genel muhasebe entegrasyonu sağlayan Azure Event Grid Topics'e Azure Blockchain Hizmeti işlem verilerini yakalar, dönüştürür ve sunar. Zincir dışı uygulamaları ve veri depolarını entegre etmek için Blockchain Data Manager'ı kullanabilirsiniz. Daha fazla bilgi için Azure [Blockchain Hizmeti için Blockchain Veri Yöneticisi'ne](data-manager.md)bakın.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Azure Blockchain haberleri için, Blockchain hizmet teklifleri ve Azure Blockchain mühendislik ekibinden gelen bilgiler hakkında güncel kalmak için [Azure Blockchain blogunu](https://azure.microsoft.com/blog/topics/blockchain/) ziyaret edin.

Ürün geri bildirimi sağlamak veya yeni özellikler istemek için [blockchain için Azure geri bildirim forumu](https://aka.ms/blockchainuservoice)aracılığıyla bir fikir için oy gönderebilirsiniz.

### <a name="community-support"></a>Topluluk desteği

Microsoft mühendisleri ve Azure Blockchain topluluk uzmanlarıyla etkileşimde bulunun.

* [Azure Blockchain MSDN forumu](https://social.msdn.microsoft.com/Forums/home?forum=azureblockchain)
* [Microsoft Teknoloji Topluluğu](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/AzureBlockchainService)

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için hızlı bir başlangıç yapmayı deneyin veya bu kaynaklardan daha fazla ayrıntı öğrenin.
* [Azure portalını kullanarak bir blockchain üyesi oluşturun](create-member.md) veya [Azure CLI kullanarak bir blockchain üyesi oluşturun](create-member-cli.md)
* Maliyet karşılaştırmaları ve hesap makineleri için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/blockchain-service)bakın.
* [Azure Blockchain Geliştirme Kiti'ni](https://github.com/Azure-Samples/blockchain-devkit) kullanarak ilk uygulamanızı oluşturun
* Azure Blockchain VSCode [Uzantılı kullanım kılavuzu](https://github.com/Microsoft/vscode-azure-blockchain-ethereum/wiki)
