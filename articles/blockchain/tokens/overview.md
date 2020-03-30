---
title: Azure Blockchain Belirteçleri Nedir
description: Azure Blockchain Belirteçleri, belirteç verme ve yönetimi için hizmet (PaaS) olarak bir platformdur.
ms.date: 11/04/2019
ms.topic: overview
ms.reviewer: brendal
ms.openlocfilehash: e7c106ffee8ea06c7c0ce738aa8be0aba96a38ed
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240285"
---
# <a name="what-is-azure-blockchain-tokens"></a>Azure Blok Zinciri Belirteçleri nedir?

[!INCLUDE [Preview note](./includes/preview.md)]

Azure Blockchain Belirteçleri, Azure'daki blockchain genel muhasebelerinde standart laştırılmış belirteç verme ve yönetim için hizmet (PaaS) olarak bir platformdur.

Azure Blockchain Belirteçleri'ni kullanarak blockchain çözümünüz için önceden oluşturulmuş bir belirteç şablonu kullanarak standart laştırılmış belirteçler oluşturabilirsiniz. Ayrıca hizmeti kullanarak kendi belirteç şablonunuzu oluşturabilirsiniz. Oluşturulduktan sonra, bir blockchain'e bağlanmak ve belirteçleri vermek için Azure Blockchain Belirteçleri'ni kullanın. Yayımlandıktan sonra, belirteçleri birden çok blockchain ağı arasında yönetebilirsiniz.

## <a name="templates"></a>Şablonlar

Önceden oluşturulmuş bir belirteç şablonu seçmek veya kendi belirteç şablonunuzu oluşturmak için Azure Blockchain Belirteçleri'ni kullanın. Azure Blockchain Belirteçleri, desteklenen davranışlara dayalı olarak kendi belirteç şablonunuzu oluşturmanıza olanak tanıyan belirteç şablonu oluşturma özelliğini destekler. Belirteç şablonları, en sık kullanılan belirteçlerle eşlenebildikleri nden çoğu blockchain çözümü için kullanılabilir. Bir şablonla başlayabilir, kişiselleştirebilir ve çözümünüz için belirteçleri dağıtabilirsiniz.

Azure Blockchain Belirteçleri şablonları hakkında daha fazla bilgi için Bkz. [Azure Blockchain Belirteçleri şablonları.](templates.md)

## <a name="management"></a>Yönetim

Azure Blockchain Belirteçleri, varolan bir blockchain ağına bağlanmak için Azure portal yönetimi ve API'ler sağlar. Şu anda [Azure Blockchain Hizmeti'ne](../service/overview.md) veya başka bir Ethereum aile blockchain'ine bağlanabilirsiniz.

Bir veya birden çok blockchain abuna bağlandıktan sonra, blockchain çözümünüzde kullanılmak üzere belirteçleri çıkarmak ve yönetmek için Azure Blockchain Belirteçleri API'lerini kullanabilirsiniz. API'leri kullanarak, belirteç yönetimini iş uygulamalarınıza ve mantığınıza entegre edebilirsiniz. Örneğin, belirteçleri doğrudan blockchain'de yönetmek yerine belirteçleri yönetmek için REST API'sini kullanabilirsiniz.

## <a name="blockchains-and-accounts"></a>Blockchain'ler ve hesaplar

Azure Blockchain Belirteçleri, bağlı blockchain ağlarında yeni gruplar ve yeni blockchain hesapları oluşturmak için Azure portal yönetimi ve API'ler sağlar. Doğrudan bağlı ağlarınızda yeni hesaplar oluşturabilirsiniz ve Azure Blockchain Belirteçleri hesabınızın özel anahtarlarını sizin adınıza yönetir. Grupları kullanarak, birden çok ağdan farklı blockchain hesaplarını gruplayabilir ve gruplar aracılığıyla erişim denetimini yönetebilirsiniz.

Azure Blockchain Belirteçleri hesap yönetimi hakkında daha fazla bilgi [için](account-management.md)bkz.

## <a name="token-taxonomy-framework"></a>Token Taksonomi Çerçevesi

Azure Blockchain Belirteçleri, Token Taksonomi Çerçevesi (TTF) adlı standartlara dayalı bir temel üzerine kurulmuştur. TTF, [Token Taksonomi Girişimi](https://entethalliance.org/participate/token-taxonomy-initiative/) (TTI) belirteç çalışma grubundan oluşturulan bir teslim edilebilirler kümesidir. TTI çalışma grubu, Ethereum, Quorum, Corda ve Hyperledger Fabric dahil olmak üzere tüm büyük defterlerde uygulanabilen belirteçler ve davranışları için bir iş taksonomisi tanımlar. Çalışma grubunun amacı, belirteçlere dayalı gelişimi demokratikleştirmek ve basitleştirmeyi sağlamak için belirteçlerin kullanımını iş perspektifinden standartlaştıran bir çerçeve oluşturmaktır. Endüstrinin bu belirteçleri ve davranışlarını iş düzeyinde tanımlamasına izin vererek, belirteçlerin ayrıntılı uygulanması belirteçleri manipüle eden iş mantığından soyutlanır.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Azure Blockchain haberleri için, Blockchain hizmet teklifleri ve Azure Blockchain mühendislik ekibinden gelen bilgiler hakkında güncel kalmak için [Azure Blockchain blogunu](https://azure.microsoft.com/blog/topics/blockchain/) ziyaret edin.

Azure Blockchain Belirteçleri önizlemesine erişmek için [Azure Blockchain Belirteçleri ekibine başvurun.](https://aka.ms/PreviewForm)

Ürün geri bildirimi sağlamak veya yeni özellikler istemek için [blockchain için Azure geri bildirim forumu](https://aka.ms/blockchainuservoice)aracılığıyla bir fikir için oy gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir Azure [Blockchain Belirteçleri şablonları](templates.md)hakkında daha fazla bilgi edinin.
