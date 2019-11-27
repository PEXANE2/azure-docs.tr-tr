---
title: Azure blok zinciri hizmetine genel bakış
description: Azure blok zinciri hizmeti 'ne genel bakış
ms.date: 11/21/2019
ms.topic: overview
ms.reviewer: janders
ms.openlocfilehash: 02cc955822987e3be6f5a2184fc49e5510b29626
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455924"
---
# <a name="what-is-azure-blockchain-service"></a>Azure Blok Zinciri Hizmeti nedir?

Azure blok zinciri hizmeti, kullanıcılara blok zinciri ağlarını Azure 'da büyüyerek büyütüp çalıştırabilme olanağı sağlayan, tam olarak yönetilen bir muhasebe hizmetidir. Hem altyapı yönetimi hem de blok zinciri ağ yönetimi için Birleşik denetim sağlayarak Azure blok zinciri hizmeti şunları sağlar:

* Basit ağ dağıtımı ve işlemleri
* Yerleşik konsorsiyum yönetimi
* Tanıdık geliştirme araçlarıyla akıllı sözleşmeler geliştirin

Azure blok zinciri hizmeti birden çok defter protokolünü destekleyecek şekilde tasarlanmıştır. Şu anda, [iBFT](https://github.com/jpmorganchase/quorum/wiki/Quorum-Consensus) konsensus mekanizması kullanılarak Ethereum [çekirdek](https://www.jpmorgan.com/Quorum) defterine yönelik destek sağlar.

Bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Sanal makineleri ve altyapıyı yönetmek için zaman ve kaynak ayırmak yerine, uygulama geliştirme ve iş mantığına odaklanırsınız. Ayrıca, yeni beceriler öğrenmek zorunda kalmadan çözümlerinizi teslim etmek için seçtiğiniz açık kaynak araçları ve platformuyla uygulamanızı geliştirmeye devam edebilirsiniz.

## <a name="network-deployment-and-operations"></a>Ağ dağıtımı ve işlemler

Azure blok zinciri hizmeti 'nin dağıtımı, Azure blok zinciri uzantısı kullanılarak Azure portal, Azure CLı veya Visual Studio Code aracılığıyla yapılır. Dağıtım basitleştirilerek, hem işlem hem de Doğrulayıcı düğümlerinin sağlanması, güvenlik yalıtımı için Azure sanal ağları ve hizmet tarafından yönetilen depolamanın sağlanması dahildir.  Ayrıca, yeni bir blok zinciri üyesi dağıttığınızda, kullanıcılar bir konsorsiyumun de oluşturulmasını veya katılıyor.  Consortiums, farklı Azure aboneliklerindeki birden fazla tarafın paylaşılan bir blok zincirinde birbirleriyle güvenli bir şekilde iletişim kurabilmelerini sağlar.  Bu basitleştirilmiş dağıtım, blok zinciri ağ dağıtımını gün ve dakika olarak azaltır.

### <a name="performance-and-service-tiers"></a>Performans ve hizmet katmanları

Azure blok zinciri hizmeti iki hizmet katmanı sunar: *temel* ve *Standart*. Her katman, hafif geliştirme ve test iş yüklerini desteklemek için farklı performans ve yetenekler sunarak çok büyük ölçekli üretim blok zinciri dağıtımlarını destekler. Her iki katmanda de en az bir işlem düğümü ve bir doğrulayıcı düğümü (temel) veya iki Doğrulayıcı düğüm (Standart) bulunur.

![Fiyatlandırma katmanları](./media/overview/pricing-tiers.png)

*Standart* katman, iki Doğrulayıcı düğüm sunumuna ek olarak her işlem ve doğrulayıcı düğümü Için 2 *sanal çekirdek* sağlarken, temel katmanda 1 sanal çekirdek yapılandırması sunulur.  İşlem ve doğrulayıcı düğümleri için 2 sanal çekirdek sunarak, 1 sanal çekirdek çekirdek defterine ayrılabilir, çünkü kalan 1 sanal çekirdek, altyapı ile ilgili diğer hizmetler için kullanılabilir ancak üretim blok zinciri iş yükleri için en iyi performansı sağlar. Fiyatlandırma ayrıntıları hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti fiyatlandırması](https://azure.microsoft.com/pricing/details/blockchain-service).

### <a name="security-and-maintenance"></a>Güvenlik ve bakım

İlk blok zinciri üyesini sağlamaktan sonra, üyeye ek işlem düğümleri ekleyebilirsiniz.  Varsayılan olarak, işlem düğümleri güvenlik duvarı kuralları aracılığıyla güvenli hale getirilir ve erişim için yapılandırma gerektirir.  Ayrıca, tüm işlem düğümleri TLS aracılığıyla hareket halindeki verileri şifreler.  Güvenlik duvarı kuralları, temel kimlik doğrulaması, erişim anahtarları ve Azure Active Directory tümleştirme dahil olmak üzere işlem düğümü erişiminin güvenliğini sağlamak için birden çok seçenek mevcuttur. Daha fazla bilgi için bkz. [işlem düğümlerini yapılandırma](configure-transaction-nodes.md) ve [Azure Active Directory erişimi yapılandırma](configure-aad.md).

Yönetilen bir hizmet olarak, Azure blok zinciri hizmeti, blok zinciri üyesinin düğümlerine en son ana bilgisayar işletim sistemi ve genel muhasebe yazılım yığın güncelleştirmeleriyle birlikte düzeltme eki uygulanmış ve yüksek kullanılabilirlik için yapılandırılmış (yalnızca Standart katman), DevOps 'un çoğunu ortadan kaldıran sağlar geleneksel IaaS blok zinciri düğümleri için gereklidir.  Düzeltme eki uygulama ve güncelleştirme hakkında daha fazla bilgi için bkz. [desteklenen Azure blok zinciri hizmeti muhasebe sürümleri](ledger-versions.md).

### <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

Ayrıca, Azure blok zinciri hizmeti, düğümlerin CPU, bellek ve depolama kullanımı hakkında Öngörüler sağlayan Azure Izleyici hizmeti aracılığıyla zengin ölçümler sağlar.  Azure Izleyici Ayrıca, blok zinciri ağ etkinliğine yönelik işlemler ve bloklar, işlem sırası derinliği ve etkin bağlantılar gibi yararlı öngörüler sağlar.  Ölçümler, blok zinciri uygulamanız için önemli olan öngörülere görünümler sunacak şekilde özelleştirilebilir.  Ayrıca, eşikler, kullanıcıların bir e-posta veya kısa mesaj gönderme, bir Logic App, Azure Işlevi çalıştırma veya özel tanımlanmış bir Web kancasına gönderme gibi eylemleri tetiklemelerine olanak sağlayan uyarılar aracılığıyla tanımlanabilir.

![Ölçümler](./media/overview/metrics.png)

Kullanıcılar, Azure Log Analytics aracılığıyla çekirdek Muhasebeyle ilgili günlükleri veya işlem düğümlerine bağlantı girişimi gibi diğer önemli bilgileri görüntüleyebilir.

## <a name="built-in-consortium-management"></a>Yerleşik konsorsiyum yönetimi

İlk blok zinciri üyesini dağıttığınızda, yeni bir konsorsiya katılırsanız ya da oluşturursunuz.  Konsorsiyum, çok taraflı bir işlemle Transact blok zinciri üyeleri arasındaki idare ve bağlantıyı yönetmek için kullanılan mantıksal bir gruptur.  Azure blok zinciri hizmeti, konsorsiyumun hangi eylemlerin yapabileceğini belirleyen önceden tanımlı akıllı sözleşmeler aracılığıyla yerleşik idare denetimleri sağlar.  Bu idare denetimleri, konsorsiyumun Yöneticisi tarafından gerektiği şekilde özelleştirilebilir. Yeni bir konsorsiyum oluşturduğunuzda, blockzincirinizin üyesi konsorsiyumun varsayılan yöneticisidir ve diğer taraflara konsorsiyume katılması için davet etme olanağı sağlar.  Yalnızca daha önce davet edildiyseniz bir konsorsiya katabilirsiniz.  Bir konsorsiya katılırken, blok zinciri üyesi, konsorsiyumun Yöneticisi tarafından yerine yerleştirilen idare denetimlerine tabidir.

![Konsorsiyum yönetimi](./media/overview/consortium.png)

Bir konsorsiyumde üye ekleme ve kaldırma gibi konsorsiyum yönetim eylemlerine PowerShell ve bir REST API erişebilirsiniz. Solidity tabanlı akıllı sözleşmeleri değiştirmek ve göndermek yerine ortak arabirimleri kullanarak bir konsorsiyunuzu programlı bir şekilde yönetebilirsiniz. Daha fazla bilgi için bkz. [konsorsiyum yönetimi](consortium.md).

## <a name="develop-using-familiar-development-tools"></a>Tanıdık geliştirme araçlarını kullanarak geliştirme

Açık kaynaklı çekirdek Ethereum defterine bağlı olarak, mevcut Ethereum uygulamalarında yaptığınız şekilde Azure blok zinciri hizmeti için uygulama geliştirebilirsiniz. Önde gelen sektör ortaklarıyla çalışan Azure blok zinciri geliştirme seti Visual Studio Code uzantısı, geliştiricilerin akıllı sözleşmeleri oluşturmak için Truffle paketi gibi tanıdık araçlardan yararlanmasını sağlar. Geliştiriciler, Azure blok zinciri geliştirme seti uzantısını kullanarak akıllı sözleşmelerinizi tek bir IDE 'den derleyip dağıtabilmeniz için ve var olan konsorsiyumunu oluşturabilir veya buna bağlanabilir. Azure blok zinciri Visual Studio Code uzantısını kullanarak, akıllı sözleşmelerinizi tek bir IDE 'den derleyip dağıtabilmeniz için mevcut bir konsorsiyum oluşturabilir veya bağlanabilirsiniz. Daha fazla bilgi için, VS Code marketi ve [Azure blok zinciri Geliştirme Seti Kullanıcı kılavuzunda](https://aka.ms/vscodebcextensionwiki) [Azure blok zinciri geliştirme seti](https://aka.ms/vscodebcextension) ' ne bakın.

## <a name="publish-blockchain-data"></a>Blok zinciri verilerini yayımlama

Azure blok zinciri hizmeti için blok zinciri Veri Yöneticisi, Azure hizmetleri ile güvenilir ve ölçeklenebilir blok zinciri ile tümleştirme sağlayan Azure Event Grid konulara Azure blok zinciri hizmeti işlem verilerini yakalar, dönüştürür ve sunar. Zincir dışı uygulamaları ve veri depolarını bütünleştirmek için Blockzincirini Veri Yöneticisi kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Için blok zinciri veri Yöneticisi](data-manager.md).

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Yardıma mı ihtiyacınız var?

* [Azure blok zinciri blogu](https://azure.microsoft.com/blog/topics/blockchain/), [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain)ve [Azure blok zinciri forumunu](https://social.msdn.microsoft.com/Forums/home?forum=azureblockchain)ziyaret edin.
* Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/921130-azure-blockchain-service) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için bir hızlı başlangıç yapın veya bu kaynaklardan daha fazla ayrıntı bulun.
* [Azure Portal kullanarak bir blok zinciri üyesi oluşturun](create-member.md) veya [Azure CLI kullanarak bir blok zinciri üyesi oluşturun](create-member-cli.md)
* Maliyet karşılaştırmaları ve hesaplayıcıları için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/blockchain-service).
* [Azure blok zinciri geliştirme setini](https://github.com/Azure-Samples/blockchain-devkit) kullanarak ilk uygulamanızı oluşturma
* Azure blok zinciri VSCode uzantısı [Kullanıcı Kılavuzu](https://github.com/Microsoft/vscode-azure-blockchain-ethereum/wiki)
