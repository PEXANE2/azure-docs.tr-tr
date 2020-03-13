---
title: Azure blok zinciri belirteçleri nedir?
description: Azure blok zinciri belirteçleri, belirteç verme ve yönetimi için bir hizmet olarak platform (PaaS).
ms.date: 11/04/2019
ms.topic: overview
ms.reviewer: brendal
ms.openlocfilehash: e7c106ffee8ea06c7c0ce738aa8be0aba96a38ed
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79240285"
---
# <a name="what-is-azure-blockchain-tokens"></a>Azure Blok Zinciri Belirteçleri nedir?

[!INCLUDE [Preview note](./includes/preview.md)]

Azure blok zinciri belirteçleri, Azure 'daki blok zinciri genelinde standartlaştırılmış belirteç verme ve yönetimi için hizmet olarak platform (PaaS).

Azure blok zinciri belirteçlerini kullanarak, önceden oluşturulmuş belirteç şablonu kullanarak blok zinciri çözümünüz için standartlaştırılmış belirteçler oluşturabilirsiniz. Ayrıca, hizmetini kullanarak kendi belirteç şablonunuzu da oluşturabilirsiniz. Oluşturulduktan sonra, bir blok zincirindeki belirteçleri bağlamak ve bu belirteçleri vermek için Azure blok zinciri belirteçlerini kullanın. Daha sonra, belirteçleri birden çok blok zinciri ağı genelinde yönetebilirsiniz.

## <a name="templates"></a>Şablonlar

Önceden oluşturulmuş bir belirteç şablonu seçmek veya kendi belirteç şablonunuzu oluşturmak için Azure blok zinciri belirteçlerini kullanın. Azure blok zinciri belirteçleri, desteklenen davranışları temel alarak kendi belirteç şablonunuzu oluşturmanıza olanak sağlayan belirteç şablonu bileşyelbilirliğini destekler. Belirteç şablonları, çoğu blok zinciri çözümü için en sık kullanılan belirteçlerle eşlendiğinden kullanılabilir. Bir şablonla başlayabilir, kişiselleştirebilir ve çözümünüzle ilgili belirteçleri dağıtabilirsiniz.

Azure blok zinciri belirteçleri şablonları hakkında daha fazla bilgi için bkz. [Azure blok zinciri belirteçleri şablonları](templates.md).

## <a name="management"></a>Yönetim

Azure blok zinciri belirteçleri, mevcut bir blok zinciri ağına bağlanmak için Azure portal yönetim ve API 'Ler sağlar. Şu anda [Azure blok zinciri hizmeti](../service/overview.md) 'ne veya başka bir Ethereum aile blok zincirine bağlanabilirsiniz.

Bir veya birden çok blok zinciri ağına bağlandıktan sonra, blok zinciri çözümünüzde kullanılacak belirteçleri vermek ve yönetmek için Azure blok zinciri belirteçleri API 'Lerini kullanabilirsiniz. API 'Leri kullanarak, iş uygulamalarınızda ve mantığınızdaki belirteç yönetimini tümleştirebilirsiniz. Örneğin, belirteçleri doğrudan blok zincirinde yönetmek yerine belirteçleri yönetmek için REST API kullanabilirsiniz.

## <a name="blockchains-and-accounts"></a>Blok zincirler ve hesaplar

Azure blok zinciri belirteçleri, bağlantılı blok zinciri ağlarında yeni gruplar ve yeni blok zinciri hesapları oluşturmak için Azure portal yönetim ve API 'Ler sağlar. Doğrudan bağlantılı ağlarınızda yeni hesaplar oluşturabilirsiniz ve Azure blok zinciri belirteçleri, hesap özel anahtarlarınızı sizin adınıza yönetir. Grupları kullanarak, birden fazla ağdan farklı blok zinciri hesapları gruplandırabilirsiniz ve gruplar aracılığıyla erişim denetimini yönetebilirsiniz.

Azure blok zinciri belirteçleri hesap yönetimi hakkında daha fazla bilgi için bkz. [Azure blok zinciri belirteçleri hesap yönetimi](account-management.md).

## <a name="token-taxonomy-framework"></a>Belirteç taksonomi çerçevesi

Azure blok zinciri belirteçleri, belirteç taksonomi çerçevesi (TTF) adlı standartlara dayalı bir temel üzerinde oluşturulmuştur. TTF, [belirteç taksonomi girişimi](https://entethalliance.org/participate/token-taxonomy-initiative/) (TTI) belirteci çalışma grubundan oluşturulan teslim edilebilirler kümesidir. TTı çalışma grubu, belirteçler için bir iş Taksonomi ve Ethereum, çekirdek, Corda ve hiper muhasebe dokusunu dahil olmak üzere tüm önemli defterlerde uygulanabilen davranışları tanımlar. Çalışma grubunun hedefi, bir iş perspektifinden, basitme ve herkese belirteci tabanlı geliştirmeyi sağlamak için belirteçlerin kullanımını standartlayan bir çerçeve oluşturmaktır. Sektörün bu belirteçleri ve davranışlarını iş düzeyinde tanımlamasına izin vererek, belirteçlerin ayrıntılı uygulanması belirteçleri belirleyen iş mantığından soyutlanmıştır.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Azure blok zinciri haberleri için Azure blok zinciri [blogu](https://azure.microsoft.com/blog/topics/blockchain/) ' nı ziyaret ederek, blok zinciri hizmeti tekliflerini ve Azure blok zinciri Mühendisliği ekibinin bilgilerini güncel tutun.

Azure blok zinciri belirteçleri önizlemesine erişim sağlamak için [Azure blok zinciri belirteçleri ekibine başvurun](https://aka.ms/PreviewForm).

Ürün geri bildirimi sağlamak veya yeni özellikler istemek için, [blok zinciri Için Azure geri bildirim Forumu](https://aka.ms/blockchainuservoice)aracılığıyla bir fikir oylayın.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir [Azure blok zinciri belirteçleri şablonları](templates.md)hakkında daha fazla bilgi edinin.
