---
title: Azure Marketi için sanal makine teklifi Yayımlama Kılavuzu
description: Bu makalede, Market 'ten dağıtılması için bir sanal makine ve yazılım ücretsiz deneme sürümü yayımlama gereksinimleri açıklanmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: dsindona
ms.openlocfilehash: 2fa67d81546db86535c179a9c59d0602c1175cba
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687507"
---
# <a name="virtual-machine-offer-publishing-guide"></a>Sanal Makine teklifi Yayımlama Kılavuzu

Sanal makine görüntüleri, bir çözümü Azure Marketi 'nde yayımlamanın ana yöntemlerinden biridir. Bu teklifin gereksinimlerini anlamak için bu kılavuzu kullanın. 

Bunlar Market aracılığıyla dağıtılan ve faturalandırılan işlem teklifleridir. Kullanıcının gördüğü eyleme yapılan çağrı "Şimdi al" dır.

## <a name="free-trial"></a>Ücretsiz Deneme 

Kullanıcıları, kendi lisansını getir (KLG) Faturalandırma modelini kullanırken sınırlı süreli yazılım lisanslarına erişerek teklifinizi test etmek üzere düzenleyebilirsiniz. 

## <a name="test-drive"></a>Test Sürüşü

Hizmet olarak altyapı (IaaS) veya hizmet olarak yazılım (SaaS) uygulamaları aracılığıyla bir veya daha fazla sanal makine dağıtırsınız. Test sürüşü yayımlama seçeneğinin bir avantajı, bir sanal makinenin otomatik olarak sağlanması veya iş ortağının barındırdığı bir Kılavuzlu tura katılarak tamamen bir çözümdür. Sınama sürücüsü, müşterinize hiçbir ek ücret ödemeden bir değerlendirme sağlar. Müşterinizin deneme deneyimine yönelik mevcut bir Azure müşterisi olması gerekmez. 

Başlamak için [amp-testdrive](mailto:amp-testdrive@microsoft.com) 'da bizimle iletişim kurun. 

|Gereksinimler  |Ayrıntılar |
|---------|---------|
| Market uygulamanız var   |    IaaS veya SaaS aracılığıyla bir veya daha fazla sanal makine.      |

## <a name="interactive-demo"></a>Etkileşimli Tanıtım

Etkileşimli bir tanıtım kullanarak müşterilerinizin çözümüne yönelik kılavuzlu bir deneyim sağlarsınız. Etkileşimli Tanıtım yayımlama seçeneğinin avantajı, karmaşık çözümünüzün karmaşık bir şekilde sağlanması gerekmeden bir deneme deneyimi sağlamaktır. 

## <a name="virtual-machine-offer"></a>Sanal Makine teklifi

Müşterinizin ilişkili aboneliğine bir Sanal Gereç dağıtırken sanal makine teklif türünü kullanın. VM 'Ler, Kullandıkça Öde veya kendi lisansını getir (KLG) lisanslama modelleri kullanılarak tamamen ticari olarak etkinleştirilmiştir. Microsoft, ticari işlemi barındırır ve müşterinizin adına fatura alır. Müşteri ile Microsoft arasında, kurumsal sözleşmeler dahil tercih edilen ödeme ilişkilerini kullanmanın avantajını elde edersiniz.

> [!NOTE]
> Şu anda, bir Kurumsal Anlaşma ilişkili parasal taahhütler, VM 'nizin Azure kullanımında kullanılabilir, ancak yazılım lisans ücretlerine karşı kullanılamaz.  
> 
> [!NOTE]
> Görüntüyü ve fiyatlandırmayı özel bir teklif olarak yayımlayarak, sanal makinenizin bulma ve dağıtımını belirli bir müşteri kümesiyle kısıtlayabilirsiniz. Özel teklifler, en yakın müşterileriniz için özel teklifler oluşturma ve özelleştirilmiş yazılım ve koşulları sunma imkanını açar. Özelleştirilmiş terimler, özel fiyatlandırma ve koşullara sahip alan ve sınırlı yayın yazılımına erken erişim dahil olmak üzere çeşitli senaryolar vurgulamanızı sağlar. Özel teklifler, bu ayrıntılarla yeni bir SKU oluşturarak belirli fiyatlandırma veya ürünleri sınırlı bir müşteri kümesine vermenizi sağlar.  
> *   Özel teklifler hakkında daha fazla bilgi için, [Azure.Microsoft.com/blog/Private-offers-on-Azure-Marketplace](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace)adresinde bulunan Azure Marketi 'Ndeki özel teklifleri ziyaret edin.  

| Gereksinim | Ayrıntılar |  
|:--- |:--- | 
| Faturalandırma ve ölçüm | SANAL makinenizin KLG veya Kullandıkça Öde aylık faturalandırmasını desteklemesi gerekir. |  
| Azure ile uyumlu sanal sabit disk (VHD) | VM 'Ler Windows veya Linux üzerinde oluşturulmalıdır. <ul> <li>Linux VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure 'da desteklenen Linux dağıtımları](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Windows VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure ile uyumlu bır VHD oluşturma](./partner-center-portal/azure-vm-create-offer.md).</li> </ul> |  

>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı kabul etme artık kullanılabilir.  Teklifinizi Microsoft CSP iş ortağı kanalları aracılığıyla pazarlama hakkında daha fazla bilgi için lütfen bkz. [bulut çözümü sağlayıcıları](./cloud-solution-providers.md) .

## <a name="next-steps"></a>Sonraki adımlar

Daha önce yapmadıysanız, 

- Market hakkında [bilgi edinin](https://azuremarketplace.microsoft.com/sell) .

Kaydolduysanız ve yeni bir teklif oluşturuyor ya da mevcut bir teklif üzerinde çalışıyorsanız,

- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .
- Daha fazla bilgi için bkz. [Sanal Makine teklifi oluşturma](./partner-center-portal/azure-vm-create-offer.md) .
