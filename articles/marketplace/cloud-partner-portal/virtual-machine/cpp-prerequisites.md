---
title: Microsoft Azure için sanal makine ön koşulları | Azure Marketi
description: Azure Marketi'nde vm teklifi yayınlamak için gereken ön koşulların listesi.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: dsindona
ms.openlocfilehash: 24c560aff9e8ba7ca0858fdb7fa11a59301c2360
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277625"
---
# <a name="virtual-machine-prerequisites"></a>Sanal makine önkoşulları

Bu makalede, Azure Marketi'nde bir VM teklifi yayımlamadan [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/)önce karşılamanız gereken teknik ve iş gereksinimleri listelenebilmelidir.  Bunu zaten yapmadıysanız, Sanal [Makine Teklifi Yayın Kılavuzu'nu](../../marketplace-virtual-machines.md)inceleyin.


## <a name="technical-requirements"></a>Teknik gereksinimler

Sanal makine (VM) çözümlerini yayınlamak için teknik ön koşullar basittir:

- Etkin bir Azure hesabınız olmalıdır. Yoksa, [Microsoft Azure sitesine](https://azure.microsoft.com)kaydolabilirsiniz.  
- Windows veya Linux VM geliştirmeyi destekleyecek şekilde yapılandırılmış bir ortamınız olmalıdır.  Daha fazla bilgi için ilişkili VM dokümantasyon sitesine bakın:
    - [Linux VM’leri ile İlgili Belgeler](https://docs.microsoft.com/azure/virtual-machines/linux/)
    - [Windows VM’leri ile İlgili Belgeler](https://docs.microsoft.com/azure/virtual-machines/windows/)


## <a name="business-requirements"></a>İş gereksinimleri

İş gereksinimleri usul, sözleşme ve yasal yükümlülükleri içerir: 

<!-- TD: Aren't most of these business requirements common to all AMP offerings?  If yes, then move to higher level, perhaps to the AMP section "Become a Cloud Marketplace Publisher" -->
<!-- TD: Need references for remaining docs/business reqs!-->

- Kayıtlı bir Cloud Marketplace Publisher olmalısınız.  Henüz kayıtlı değilseniz, [Bulut Pazarı Yayıncıol](https://docs.microsoft.com/azure/marketplace/become-publisher)makaledeki adımları izleyin.

    > [!NOTE]
    > [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com)oturum açabilmek için aynı Microsoft Developer Center kayıt hesabını kullanmalısınız.
    > Azure Marketi teklifleriniz için yalnızca bir Microsoft hesabınız olmalıdır. Bireysel hizmetlere veya tekliflere özgü olmamalıdır.
    
- Şirketinizin (veya yan kuruluşunun) Azure Marketi tarafından desteklenen bir satış ülkesinde/bölgesinde bulunması gerekir.  Bu ülkelerin/bölgelerin geçerli bir listesi için [Bkz. Microsoft Azure MarketKatılım İlkeleri.](https://azure.microsoft.com/support/legal/marketplace/participation-policies/)
- Ürününuzun Azure Marketi tarafından desteklenen faturalandırma modelleri ile uyumlu bir şekilde lisanslanması gerekir.  Daha fazla bilgi için [Azure Marketi'ndeki Faturalandırma seçeneklerine](https://docs.microsoft.com/azure/marketplace/billing-options-azure-marketplace)bakın. 
- Teknik desteği müşterilere ticari açıdan makul bir şekilde sağlamaksizin sorumlusunuz. Bu destek ücretsiz, ücretli veya topluluk yaklaşımları yoluyla olabilir.
- Yazılımınızı ve üçüncü taraf yazılım bağımlılıklarınızı lisanslamaktan siz sorumlusunuz.
- Teklifinizin Azure Marketi'nde ve Azure portalında listelemesi için ölçütlere uygun içerik sağlamanız gerekir. <!-- TD: Meaning/links? -->
- [Microsoft Azure Marketi Katılım İlkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) ve Yayımcı Sözleşmesi'nin koşullarını kabul etmelisiniz.
- [Microsoft Azure Web Sitesi Kullanım Koşulları,](https://azure.microsoft.com/support/legal/website-terms-of-use/)Microsoft Gizlilik [Bildirimi](https://privacy.microsoft.com/privacystatement)ve Microsoft Azure Sertifikalı [Program Sözleşmesi'ne](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/)uymanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Bu ön koşullarla karşılaştıktan [sonra, VM teklifinizi oluşturabilirsiniz.](./cpp-create-offer.md)
