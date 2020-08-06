---
title: Azure Marketi 'nde sanal makine teklifleri için Yayımlama Kılavuzu
description: Bu makalede, Azure Marketi 'nden dağıtılması için bir sanal makine ve yazılım ücretsiz deneme sürümü yayımlama gereksinimleri açıklanmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 04/15/2020
ms.openlocfilehash: 9f408174b9a8dff925a9c834ea75bf725ec6f913
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837354"
---
# <a name="publishing-guide-for-virtual-machine-offers"></a>Sanal makine teklifleri için Yayımlama Kılavuzu

Sanal makine (VM) görüntülerini yayımlama, bir çözümü Azure Marketi 'Nde yayımlamanın ana yöntemlerinden biridir. Bu teklif türü için gereksinimleri anlamak için bu kılavuzu kullanın. 

Sanal makine teklifleri, Azure Marketi aracılığıyla dağıtılan ve faturalandırılan işlem teklifleridir. Kullanıcının gördüğü eyleme yapılan çağrı *Şimdi bu uygulamayı alır*.

## <a name="free-trial"></a>Ücretsiz deneme sürümü 

Teklifinizi test etmek üzere kullanıcıları düzenlemek için, kendi lisansını getir (KLG) Faturalandırma modelini kullanırken sınırlı süreli yazılım lisanslarına erişin. 

## <a name="test-drive"></a>Sınama sürücüsü

Hizmet olarak altyapı (IaaS) veya hizmet olarak yazılım (SaaS) uygulamaları aracılığıyla bir veya daha fazla sanal makine dağıtabilirsiniz. *Test sürücüsü* yayımlama seçeneğinin bir avantajı, bir sanal makinenin veya tüm çözümünün iş ortağı tarafından barındırılan bir Kılavuzlu tura göre otomatik olarak kurulumu. Bir sınama sürücüsü, müşterilerinizin VM 'Leri hiçbir ek ücret ödemeden değerlendirmelerini sağlar. Deneme deneyimiyle birlikte çalışmak için müşterinin mevcut bir Azure müşterisi olması gerekmez. 

Sınama sürücüleri hakkında daha fazla bilgi için bkz. [test sürücüsü nedir?](what-is-test-drive.md)

|Gereksinimler  |Ayrıntılar |
|---------|---------|
| Azure Market uygulamanız var   |  IaaS veya SaaS aracılığıyla bir veya daha fazla sanal makine.      |

## <a name="interactive-demo"></a>Etkileşimli tanıtım

Bu teklifle, etkileşimli bir tanıtım kullanarak müşterilerinize çözümünüze yönelik kılavuzlu bir deneyim verirsiniz. Etkileşimli Tanıtım yayımlama seçeneğinin avantajı, karmaşık çözümünüz için karmaşık bir kurulum sağlamak zorunda kalmadan bir deneme deneyimi sunabilmeniz sağlamaktır. 

## <a name="virtual-machine-offer"></a>Sanal makine teklifi

Müşterinizle ilişkili aboneliğe bir Sanal Gereç dağıtırken *sanal makine* teklif türünü kullanın. VM 'Ler, Kullandıkça Öde veya kendi lisansını getir (KLG) lisanslama modellerini kullanarak tam olarak ticari olarak etkinleştirilmiştir. Microsoft, ticari işlemi barındırır ve müşterinizin adına fatura alır. Müşteri ile Microsoft arasında, kurumsal sözleşmeler dahil tercih edilen ödeme ilişkilerini kullanmanın avantajını elde edersiniz.

> [!NOTE]
> Şu anda, bir Kurumsal Anlaşma ilişkili parasal taahhütler, VM 'nizin Azure kullanımında kullanılabilir, ancak yazılım lisans ücretlerine karşı kullanılamaz.  
> 
> [!NOTE]
> Görüntüyü ve fiyatlandırmayı özel bir teklif olarak yayımlayarak, sanal makinenizin bulma ve dağıtımını belirli bir müşteri kümesiyle kısıtlayabilirsiniz. Özel teklifler, en yakın müşterileriniz için özel teklifler oluşturma ve özelleştirilmiş yazılım ve koşulları sunma imkanını açar. Özelleştirilmiş terimler, özel fiyatlandırma ve koşullara sahip alan ve sınırlı yayın yazılımına erken erişim dahil olmak üzere çeşitli senaryolar vurgulamanızı sağlar. Özel teklifler, bu ayrıntılarla yeni bir plan oluşturarak belirli fiyatları veya ürünleri sınırlı bir müşteri kümesine vermenizi sağlar.  
>
> Daha fazla bilgi için bkz. [Azure Marketi 'Ndeki özel teklifler](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace).  

| Gereksinim | Ayrıntılar |  
|:--- |:--- | 
| Faturalandırma ve ölçüm | SANAL makinenizin KLG veya Kullandıkça Öde aylık faturalandırmasını desteklemesi gerekir. |  
| Azure ile uyumlu sanal sabit disk (VHD) | VM 'Ler Windows veya Linux üzerinde oluşturulmalıdır. VHD oluşturma hakkında daha fazla bilgi için bkz. <ul> <li>[Azure 'da desteklenen Linux dağıtımları](../virtual-machines/linux/endorsed-distros.md) (Linux VHD 'ler için).</li> <li>[Azure ile uyumlu bır VHD oluşturun](./partner-center-portal/azure-vm-create-offer.md) (Windows VHD 'ler için).</li> </ul> |  

>[!Note]
>Bulut çözümü sağlayıcısı (CSP) iş ortağı kanalı kabul etme artık kullanılabilir. Teklifinizi Microsoft CSP iş ortağı kanalları aracılığıyla pazarlama hakkında daha fazla bilgi için bkz. [Cloud Solution Providers](./cloud-solution-providers.md).

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, [Azure Marketi ile bulut işletmenizi nasıl büyütireceğinizi](https://azuremarketplace.microsoft.com/sell)öğrenin.

Iş Ortağı Merkezi 'nde çalışmaya kaydolmak ve başlamak için:

- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .
- Daha fazla bilgi için bkz. [Sanal Makine teklifi oluşturma](./partner-center-portal/azure-vm-create-offer.md) .
