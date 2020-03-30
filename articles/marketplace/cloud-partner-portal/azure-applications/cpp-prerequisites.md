---
title: Azure Uygulaması ön koşullar sunuyor | Azure Marketi
description: Azure Marketi'nde bir Azure uygulama teklifi yayınlamak için ön koşullar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: dsindona
ms.openlocfilehash: b927404758dc59419fb3c5e638ac32758d534681
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281757"
---
# <a name="azure-application-prerequisites"></a>Azure uygulama ön koşulları

Bu makalede, Azure Marketi'nde yönetilen bir uygulama teklifi yayınlamak için teknik ve iş ön koşulları açıklanmaktadır.  Bunu daha önce yapmadıysanız, aşağıdaki bilgi kaynaklarını gözden geçirin:
- [Azure Uygulamaları: Çözüm Şablonu Teklif Yayımlama Kılavuzu](../../marketplace-solution-templates.md) veya [Azure Uygulamaları: Yönetilen Uygulama Teklifi Yayımlama Kılavuzu,](../../marketplace-managed-apps.md) SKU türüne bağlı olarak
- [Azure Marketi](https://channel9.msdn.com/Events/Build/2018/BRK3603) videosu için Çözüm Şablonları Oluşturma ve Yönetilen Uygulamalar


## <a name="technical-requirements"></a>Teknik gereksinimler

Teknik gereksinimler aşağıdaki öğeleri içerir:

*   Azure Kaynak Yöneticisi şablonları Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) Bu makalede, Bir Azure Kaynak Yöneticisi şablonunun yapısı açıklanmaktadır. Şablonun farklı bölümlerini ve bu bölümlerde kullanılabilen özellikleri sunar. Şablon JSON ve dağıtım Için değerler oluşturmak için kullanabileceğiniz ifadeler oluşur. 
* Azure Quickstart şablonları.<br> Daha fazla bilgi için bkz.

  * [Azure Quickstart Şablonları](https://azure.microsoft.com/documentation/templates/). Azure kaynaklarını Azure Kaynak Yöneticisi aracılığıyla topluluk katkıda bulunan şablonlarla dağıtın ve daha fazla iş elde edin. Azure Resource Manager, uygulamalarınızı bildirim temelli bir şablon aracılığıyla sağlamanıza olanak tanır. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanırsınız.
  * [GitHub: Azure Kaynak Yöneticisi Quickstart Şablonları.](https://github.com/azure/azure-quickstart-templates) Bu repo, topluluk tarafından katkıda bulunulan tüm kullanılabilir Azure Kaynak Yöneticisi şablonlarını içerir. Aranabilir şablon dizini ' https://azure.microsoft.com/documentation/templates/nde tutulur.
* UI Tanımı Oluşturma<br>
Daha fazla bilgi için, [yönetilen uygulamanız için Azure portalı kullanıcı arabirimi oluştur'a](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)bakın. Bu makalede createUiDefinition.json dosyasının temel kavramları tanıtıştır. Azure portalı, yönetilen bir uygulama oluşturmak için kullanıcı arabirimini oluşturmak için bu dosyayı kullanır.


## <a name="business-requirements"></a>İş gereksinimleri

İş gereksinimleri aşağıdaki usul, sözleşme ve yasal yükümlülükleri içerir:

* Kayıtlı bir Cloud Marketplace Publisher olmalısınız. Kayıtlı değilseniz, [Bulut Pazarı Yayımcısı Ol](https://docs.microsoft.com/azure/marketplace/become-publisher
)makaledeki adımları izleyin.

>[!NOTE]
>Bulut İş Ortağı Portalı'nda oturum açabilmek için aynı Microsoft Developer Center kayıt hesabını kullanmanız gerekir. Azure Marketi teklifleriniz için yalnızca bir Microsoft hesabınız olmalıdır. Bu hesap, tek tek hizmetlere veya tekliflere özgü olmamalıdır.

* Şirketinizin (veya yan kuruluşunun) Azure Marketi tarafından desteklenen bir ülkeden/bölgeden satışta olması gerekir. Bu ülkelerin/bölgelerin geçerli bir listesi için [Bkz. Microsoft Azure MarketKatılım İlkeleri.](https://azure.microsoft.com/support/legal/marketplace/participation-policies/)
* Ürününuzun Azure Marketi tarafından desteklenen faturalandırma modelleri ile uyumlu bir şekilde lisanslanması gerekir. Daha fazla bilgi için Azure Marketi'ndeki [fatura seçeneklerine](https://docs.microsoft.com/azure/marketplace/marketplace-commercial-transaction-capabilities-and-considerations) bakın.
* Teknik desteği müşterilere ticari açıdan makul bir şekilde sağlamaksizin sorumlusunuz. Bu destek ücretsiz, ücretli veya topluluk yaklaşımları yoluyla olabilir.
* Yazılımınızı ve üçüncü taraf yazılım bağımlılıklarınızı lisanslamaktan siz sorumlusunuz.
* Teklifinizin Azure Marketi'nde ve Azure portalında listelemesi için ölçütlere uygun içerik sağlamanız gerekir.
* Microsoft Azure Marketi Katılım İlkeleri ve Yayımcı Sözleşmesi'nin koşullarını kabul etmelisiniz.
* Microsoft Azure Web Sitesi Kullanım Koşulları, Microsoft Gizlilik Bildirimi ve Microsoft Azure Sertifikalı Program Sözleşmesi'ne uymanız gerekir.


## <a name="publishing-requirements"></a>Yayımlama gereksinimleri

Yeni bir Azure uygulama teklifi yayınlamak için aşağıdaki ön koşulları karşılamanız gerekir:

* Meta verilerinizi kullanıma hazır layın. Aşağıdaki liste (kapsamlı olmayan) bu meta verilerin bir örneğini gösterir:
  * Bir başlık
  * Açıklama (HTML biçiminde)
  * 40 x 40 piksel, 90 x 90 piksel, 115 x 115 piksel ve 255 x 115 piksel: Bir logo görüntüsü (PNG formatında) ve bu sabit görüntü boyutları.
* Kullanım *Koşulları* ve *Gizlilik Politikası* belgeleri
* Başvuru belgeleri
* Destek kişileri


## <a name="next-steps"></a>Sonraki adımlar

Tüm gereksinimleri karşıladıktan [sonra, bir Azure uygulama teklifi oluşturmaya](./cpp-create-offer.md)hazır olacaksınız. 
 
