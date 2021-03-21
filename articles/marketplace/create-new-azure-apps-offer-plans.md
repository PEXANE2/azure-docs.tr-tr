---
title: Azure Uygulama teklifiniz için planlar oluşturma
description: Iş Ortağı Merkezi 'nde Azure Uygulama teklifiniz için planlar oluşturmayı öğrenin.
author: aarathin
ms.author: aarathin
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 11/06/2020
ms.openlocfilehash: fd5ee9085cf716128c5b3ae073f963c76d2bd17a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94370465"
---
# <a name="how-to-create-plans-for-your-azure-application-offer"></a>Azure Uygulama teklifiniz için planlar oluşturma

Microsoft Commercial Market aracılığıyla satılan tekliflerin, teklifinizi ticari Market 'te listelemek için en az bir plana sahip olması gerekir. Aynı teklifde farklı seçeneklere sahip çeşitli planlar oluşturabilirsiniz. Bu planlar (bazen SKU 'Lar olarak adlandırılır) plan türü (_çözüm şablonu_ veya _yönetilen uygulama_), para veya hedef kitle bakımından farklı olabilir. Planlar hakkında genel yönergeler için bkz. [ticari Market teklifleri Için planlar ve fiyatlandırma](plans-pricing.md).

## <a name="create-a-plan"></a>Plan oluşturma

1. **Plana genel bakış** sekmesinin üst kısmındaki **+ Yeni plan oluştur**' u seçin.
1. Görüntülenen iletişim kutusunda, **plan kimliği** kutusuna benzersiz BIR plan kimliği girin. Bu KIMLIK, ürün URL 'sindeki müşterilere görünür olacaktır. En fazla 50 küçük alfasayısal karakter, çizgi veya alt çizgi kullanın. **Oluştur**' u seçtikten sonra plan kimliğini değiştiremezsiniz.
1. **Plan adı** kutusuna bu plan için benzersiz bir ad girin. Tekliflerinizi hangi plana seçeceğinize karar verirken, müşteriler bu adı görür. En fazla 50 karakter kullanın.
1. **Oluştur**’u seçin.

## <a name="define-the-plan-setup"></a>Plan kurulumunu tanımlama

**Plan kurulumu** sekmesi, plan türünü, başka bir plandan teknik yapılandırmayı yeniden kullanıp kullanmayacağını ve planın hangi Azure bölgelerine kullanılabilir olacağını ayarlamanıza olanak sağlar. Bu sekmede verdiğiniz yanıtlar, bu plan için diğer sekmelerde görüntülenecek alanları etkileyecektir.

### <a name="select-the-plan-type"></a>Plan türünü seçin

- **Plan türü** listesinden **çözüm şablonu** veya **yönetilen uygulama**' yı seçin.

Bir **çözüm şablonu** planı tamamen müşteri tarafından yönetilir. **Yönetilen bir uygulama** planı, yayımcıların uygulamayı müşteri adına yönetmesine olanak sağlar. Bu iki plan türüyle ilgili ayrıntılar için bkz. [plan türleri](plan-azure-application-offer.md#types-of-plans).

### <a name="re-use-technical-configuration-optional"></a>Teknik yapılandırmayı yeniden kullanma (isteğe bağlı)

Bu teklif dahilinde aynı türde birden fazla plan oluşturduysanız ve teknik yapılandırma aralarında özdeş ise, teknik yapılandırmayı başka bir plandan yeniden kullanabilirsiniz. Bu plan yayımlandıktan sonra bu ayar değiştirilemez.

#### <a name="to-re-use-technical-configuration"></a>Teknik yapılandırmayı yeniden kullanmak için

1. **Bu planı, aynı türdeki başka bir plandan teknik yapılandırmayı yeniden kullanır** onay kutusunu seçin.
1. Görüntülenen listede istediğiniz temel planı seçin.

> [!NOTE]
> Paketleri başka bir plandan yeniden kullandığınızda, tüm **Teknik yapılandırma** sekmesi bu plandan kaybolur. İleride yaptığınız tüm güncelleştirmeler de dahil olmak üzere diğer plandaki teknik yapılandırma ayrıntıları, bu plan için de kullanılır.

### <a name="select-azure-regions-clouds"></a>Azure bölgelerini (bulutlar) seçin

Planınız en az bir Azure bölgesinde kullanılabilir duruma getirilmelidir. Planınız yayımlandıktan ve belirli bir Azure bölgesinde kullanılabilir olduktan sonra, bu bölgeyi teklifinizden kaldıramazsınız.

#### <a name="azure-global-region"></a>Azure genel bölgesi

**Azure genel** onay kutusu varsayılan olarak seçilidir. Bu, planınızı ticari Market tümleştirmesi olan tüm Azure küresel bölgelerindeki müşteriler için kullanılabilir hale getirir. Yönetilen uygulama planları için planınızı kullanılabilir hale getirmek istediğiniz pazarlar arasından seçim yapabilirsiniz.

Teklifinizi bu bölgeden kaldırmak için **Azure genel** onay kutusunu temizleyin.

#### <a name="azure-government-region"></a>Azure Kamu bölgesi

Bu bölge, ABD Federal, eyalet, yerel veya üç aylık varlıklardan müşterilere yönelik denetimli erişim sağlar ve bu da onlara sunmaya uygun iş ortakları sağlar. Yayımcı olarak tüm uyumluluk denetimleri, güvenlik ölçüleri ve en iyi uygulamalardan sorumludur. Azure Kamu fiziksel olarak yalıtılmış veri merkezleri ve ağlar (yalnızca ABD 'de bulunur) kullanır.

Azure Kamu Hizmetleri, belirli kamu düzenlemelerine ve gereksinimlerine tabi olan verileri işler. Örneğin, Fedrampa, NıST 800,171 (DIB), ıTAR, ıRS 1075, DoD L4 ve CJıS. Bu programlara yönelik sertifikalarınıza yönelik olarak bir açıklama getirmek için, bunları tanımlayan 100 'e kadar bağlantı sağlayabilirsiniz. Bunlar, program üzerinde doğrudan listelemesine bağlantılar ya da kendi Web sitelerinizde uyumluluğun açıklamalarını bağlar olabilir. Bu bağlantılar yalnızca Azure Kamu müşterileri tarafından görülebilir.

##### <a name="to-select-the-azure-government-region"></a>Azure Kamu bölgesini seçmek için

1. **Azure Kamu** onay kutusunu seçin.
1. **Azure Kamu sertifikaları**' nın altında **+ sertifika ekle (en fazla 100)** seçeneğini belirleyin.
1. Görüntülenen kutulara bir ad ve sertifika bağlantısı sağlayın.
1. Başka bir sertifika eklemek için adım 2 ve 3 ' ü tekrarlayın.

Sonraki sekmeye geçmeden önce **Taslağı kaydet** ' i seçin: plan listesi.

## <a name="define-the-plan-listing"></a>Plan listesini tanımlama

Plan **listesi sekmesi,** planın listeleme ayrıntılarını yapılandırdığınız yerdir. Bu sekme, aynı teklifteki planlar arasındaki farkı gösteren belirli bilgileri görüntüler. Plan adı, Özet ve açıklama ' yı ticari Market 'te görünmesini istediğiniz şekilde tanımlayabilirsiniz.

1. **Plan adı** kutusunda, bu plan için daha önce verdiğiniz ad burada görünür. İstediğiniz zaman değiştirebilirsiniz. Bu ad, ticari Market 'te teklifinizin yazılım planının başlığı olarak görünür ve 100 karakterle sınırlıdır.
1. **Plan Özeti** kutusunda planınızın kısa bir özetini (teklifin değil) sağlayın. Bu Özet 100 karakterle sınırlıdır.
1. **Plan açıklaması** kutusunda, bu yazılım planının ne kadar benzersiz olduğunu ve teklifinizdeki diğer planlardan herhangi bir farkı olduğunu açıklayın. Teklifi değil, yalnızca plan. Bu açıklama en fazla 2.000 karakter içerebilir.
1. Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakilerden birini yapın:

- Bir çözüm şablonu planı yapılandırıyorsanız, [çözüm şablonu planı yapılandırma](create-new-azure-apps-offer-solution.md)' ya gidin.
- Yönetilen bir uygulama planı yapılandırıyorsanız, [yönetilen uygulama planını yapılandırma](create-new-azure-apps-offer-managed.md)' ya gidin.
