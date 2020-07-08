---
title: Uygulamayı yayımcı doğrulandı olarak işaretleme-Microsoft Identity platform | Mavisi
description: Uygulamanın yayımcı tarafından doğrulanmış olarak nasıl işaretleneceğini açıklar. Bir uygulama, yayımcı doğrulanmış olarak işaretlendiğinde, yayımcının doğrulama işlemini tamamlamış bir Microsoft İş Ortağı Ağı hesabı kullanarak kimliğini doğruladığını ve bu MPN hesabını uygulama kaydıyla ilişkilendirdiğini anlamına gelir.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 05/08/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: d25a3831897ff8ad2e7dfb0c69910fee246aec21
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85477694"
---
# <a name="mark-your-app-as-publisher-verified-preview"></a>Uygulamanızı yayımcı doğrulandı olarak işaretleme (önizleme)

Bir uygulama, yayımcı doğrulanmış olarak işaretlendiğinde, yayımcı Microsoft İş Ortağı Ağı (MPN) hesabını kullanarak kimliğini doğrulamıştır ve bu MPN hesabını uygulama kaydıyla ilişkilendirmiştir. Bu makalede [Yayımcı doğrulama (Önizleme)](publisher-verification-overview.md) sürecinin nasıl tamamlanacağı açıklanır.

## <a name="quickstart"></a>Hızlı Başlangıç
Microsoft İş Ortağı Ağı (MPN) zaten kaydolduysanız ve [önkoşulları](publisher-verification-overview.md#requirements)karşıladıysanız, hemen kullanmaya başlayabilirsiniz: 

1. Önizleme [uygulaması kayıt portalı](https://aka.ms/PublisherVerificationPreview)' na gidin.

1. Bir uygulama seçin ve **marka**' e tıklayın. 

1. Yayımcıyı doğrulamak ve listelenen gereksinimleri gözden geçirmek **için MPN kimliği Ekle** ' ye tıklayın.

1. MPN KIMLIĞINIZI girip **Doğrula ve Kaydet**' e tıklayın.

Belirli avantajlar, gereksinimler ve sık sorulan sorular hakkında daha fazla ayrıntı için [genel bakış](publisher-verification-overview.md)bölümüne bakın.


## <a name="mark-your-app-as-publisher-verified"></a>Uygulamanızı yayımcı doğrulandı olarak işaretleme
[Önkoşulları](publisher-verification-overview.md#requirements)karşıladığınızdan emin olun, sonra uygulamanızı yayımcı doğrulanmış olarak işaretlemek için aşağıdaki adımları izleyin.  

1. Bir kuruluş (Azure AD) hesabıyla oturum açtığınızdan emin olun ve bu durumda, yayımcı doğrulanmış olarak işaretlemek istediğiniz uygulamalarda ve Iş Ortağı Merkezi 'nde MPN hesabında değişiklik yapma yetkisi vardır. 

    - Azure AD 'de bu kullanıcı uygulamanın sahibi olmalıdır veya şu rollerden birine sahip olmalıdır: uygulama Yöneticisi, bulut uygulaması Yöneticisi, genel yönetici. 

    - Iş Ortağı Merkezi 'nde bu kullanıcının şu rollere sahip olması gerekir: MPN Yöneticisi, hesaplar Yöneticisi veya genel yönetici (Bu, Azure AD 'de ana kopyalı bir paylaşılan roldür). 

1. Uygulama kayıt portalının önizleme sürümüne gidin:  

1. Yayımcı doğrulanmış olarak işaretlemek istediğiniz uygulamaya tıklayın ve marka dikey penceresini açın. 

1. Uygulamanın yayımcı etki alanının uygun şekilde ayarlandığından emin olun. Bu etki alanı şu olmalıdır: 

    - DNS tarafından doğrulanan özel etki alanı olarak Azure AD kiracısına eklenmelidir,  

    - MPN hesabınız için doğrulama işlemi sırasında kullanılan e-posta adresinin etki alanını eşleştirin. 

1. Bir yayımcıyı sayfanın en altına yakın bir **şekilde doğrulamak için MPN kimliği Ekle** ' ye tıklayın. 

1. **MPN kimliğinizi**girin. Bu MPN KIMLIĞI için olmalıdır: 

    - Doğrulama işlemini tamamlamış geçerli bir Microsoft İş Ortağı Ağı hesabı.  

    - Kuruluşunuz için Iş ortağı küresel hesabı (PGA). 

1. **Doğrula ve Kaydet**' e tıklayın. 

1. İsteğin işlenmesi için bekleyin, bu işlem birkaç dakika sürebilir. 

1. Doğrulama başarılı olduysa, yayımcı doğrulama penceresi kapanır ve bu da marka dikey penceresine geri döner. Doğrulanmış **yayımcının görünen adının**yanında mavi doğrulanmış bir rozet görürsünüz. 

1. Uygulamanızı onaylaması istenmeden kullananlar, bu işlemi başarılı bir şekilde tamamladıktan hemen sonra, bu işlem, sistem genelinde çoğaltılmasının biraz zaman alabilir. 

1. Uygulamanızda oturum açarak ve doğrulanmış rozet 'nin onay ekranında görünür olmasını sağlamak için bu işlevselliği test edin. Uygulamaya izin vermiş olan bir kullanıcı olarak oturum açtıysanız, bir onay istemi zorlamak için *Prompt = onay* sorgu parametresini kullanabilirsiniz. 

1. Bu işlemi, rozetinin görüntülenmesini istediğiniz diğer uygulamalar için gerektiği şekilde tekrarlayın. Bunu toplu olarak daha hızlı yapmak için Microsoft Graph kullanabilirsiniz ve PowerShell cmdlet 'leri yakında kullanılabilir olacaktır. Daha fazla bilgi için bkz. [MICROSOFT API Graph çağrıları yapma](troubleshoot-publisher-verification.md#making-microsoft-graph-api-calls) . 

Bu kadar! Süreç, sonuçlar veya genel olarak özelliği hakkında herhangi bir geribildirim varsa bize bilgi verin. 

## <a name="next-steps"></a>Sonraki adımlar
Sorun yaşıyorsanız, [sorun giderme bilgilerini](troubleshoot-publisher-verification.md)okuyun.