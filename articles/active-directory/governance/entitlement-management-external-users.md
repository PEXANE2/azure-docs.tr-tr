---
title: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) dış kullanıcılar için erişimi yönetir-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetiminde dış kullanıcılara erişimi yönetmek için belirtebileceğiniz ayarlar hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 419970985b9531ffab348491730aaf6c00e143b1
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72527104"
---
# <a name="govern-access-for-external-users-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde dış kullanıcılar için erişimi yönetme (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure AD Yetkilendirme Yönetimi, kuruluşunuzun dışındaki kişilerle işbirliği yapmak için [Azure AD işletmeden işletmeye (B2B)](../b2b/what-is-b2b.md) kullanır. Azure AD B2B ile dış kullanıcılar kendi giriş dizinlerinde kimlik doğrular, ancak dizininizde bir temsili vardır. Dizininizdeki Gösterim, kullanıcıya kaynaklarınıza erişim atanmasına olanak sağlar.

Bu makalede, dış kullanıcılara erişimi yönetmek için belirtebileceğiniz ayarlar açıklanmaktadır.

## <a name="how-entitlement-management-can-help"></a>Yetkilendirme Yönetimi nasıl yardımcı olabilir

[Azure AD B2B](../b2b/what-is-b2b.md) davet deneyimini kullanırken, kaynak dizininize getirmek istediğiniz dış konuk kullanıcıların e-posta adreslerini zaten bilmeniz ve ile çalışmanız gerekir. Bu, daha küçük veya kısa süreli bir proje üzerinde çalışırken ve tüm katılımcıları zaten öğrendiğinizde harika bir şekilde çalışır, ancak birlikte çalışmak istediğiniz çok sayıda kullanıcınız varsa veya katılımcılar zaman içinde değişiyorlarsa yönetimi daha zordur.  Örneğin, başka bir kuruluşla çalışıyor ve bu kuruluşla tek bir iletişim noktasına sahip olabilirsiniz, ancak bu kuruluştan daha fazla ek kullanıcının da erişime ihtiyacı olacaktır.

Yetkilendirme Yönetimi sayesinde, belirttiğiniz kuruluşların kullanıcılarının bir erişim paketini kendi kendinize isteyebilmesini sağlayan bir ilke tanımlayabilirsiniz. Onayın gerekli olup olmadığını ve erişim için bir sona erme tarihi belirtebilirsiniz. Onay gerekiyorsa, dış kuruluştan bir veya daha fazla kullanıcıyı bir veya daha fazla kullanıcı için bir veya daha fazla kullanıcıyı kendi kuruluşlarından hangi dış kullanıcıların erişmesi gerektiğini öğrendiklerinden, bunları onaylayan olarak belirleyebilirsiniz. Erişim paketini yapılandırdıktan sonra, erişim paketinin bağlantısını dış kuruluşta kişi kişiniz (sponsor) gönderebilirsiniz. Bu iletişim, dış kuruluştaki diğer kullanıcılarla paylaşabilir ve bu bağlantıyı, erişim paketini istemek için kullanabilir. Bu kuruluştan, dizininizden zaten davet edilmiş olan kullanıcılar da bu bağlantıyı kullanabilir.

Bir istek onaylandığında, Yetkilendirme Yönetimi kullanıcıya gerekli erişimi sağlayacak ve bu kullanıcılar zaten dizininizden değilse kullanıcıyı davet edebilir. Azure AD, bunlar için otomatik olarak bir B2B Konuk hesabı oluşturur. Bir yöneticinin daha önce hangi kuruluşların işbirliği için izin verileceğini sınırlı olabileceğini, diğer kuruluşlara yönelik olarak bir [B2B izin verme veya reddetme listesi](../b2b/allow-deny-list.md) ayarlanmasına izin vermek veya engellemek için  Kullanıcıya izin verilenler veya engellenenler listesi tarafından izin verilmezse, bunlar davet edilmeyecektir.

Dış kullanıcının en son sonsuza kadar erişimini istemediğiniz için ilkede 180 gün gibi bir sona erme tarihi belirtirsiniz. 180 gün sonra, erişimleri genişletilmemişse, Yetkilendirme Yönetimi ilgili erişim paketiyle ilişkili tüm erişimi kaldırır. Varsayılan olarak, Yetkilendirme Yönetimi üzerinden davet edilen kullanıcının başka bir erişim paketi ataması yoksa, son atamalarını kaybederlerse, Konuk hesabının 30 gün boyunca oturum açması engellenir ve daha sonra kaldırılır. Bu, gereksiz hesapların uzamasını önler. Aşağıdaki bölümlerde açıklandığı gibi, bu ayarlar yapılandırılabilir.

## <a name="how-access-works-for-external-users"></a>Access 'in dış kullanıcılar için nasıl çalıştığı

Aşağıdaki diyagram ve adımlar, dış kullanıcılara bir erişim paketine erişim izni verme hakkında genel bakış sağlar.

![Dış kullanıcıların yaşam döngüsünü gösteren diyagram](./media/entitlement-management-external-users/external-users-lifecycle.png)

1. Dizininizde, [dizininizde olmayan kullanıcılar için](entitlement-management-access-package-create.md#for-users-not-in-your-directory)bir ilke içeren bir erişim paketi oluşturun.

1. Bir [erişim portalı bağlantısını](entitlement-management-access-package-settings.md) , kendi kullanıcılarına erişim paketi istemek için paylaştıkları dış kuruluştan gönderebilirsiniz.

1. Dış Kullanıcı (Bu örnekteki**Istek sahibi** ), erişim paketine [erişim istemek](entitlement-management-request-access.md) için erişim portalı bağlantısını kullanır.

1. Onaylayan [, isteği onaylar](entitlement-management-request-approve.md) (veya istek otomatik olarak onaylandı).

1. İstek, [teslim durumuna](entitlement-management-process.md)geçer.

1. B2B davet işlemini kullanarak, dizininizde bir Konuk Kullanıcı hesabı (Bu örnekte**Istek sahibi a (konuk)** ) oluşturulur. Bir [izin verilenler listesi veya reddetme listesi](../b2b/allow-deny-list.md) tanımlanmışsa, liste ayarı uygulanır.

1. Konuk kullanıcıya erişim paketindeki tüm kaynaklara erişim atanır. Azure AD 'de ve diğer Microsoft Online Services ya da bağlı SaaS uygulamalarına yapılan değişikliklerin yapılması biraz zaman alabilir. Daha fazla bilgi için bkz. [değişiklikler uygulandığında](entitlement-management-access-package-resources.md#when-changes-are-applied).

1. Dış Kullanıcı, erişiminin [teslim](entitlement-management-process.md)edildiğini belirten bir e-posta alır.

1. Kaynaklara erişmek için dış kullanıcı e-postadaki bağlantıya tıklayabilir veya doğrudan davet işlemini tamamlamaya yönelik dizin kaynaklarından herhangi birine erişmeyi deneyebilir.

1. İlke ayarlarına bağlı olarak, zaman geçtikçe, dış Kullanıcı için erişim paketi atamasının süresi dolar ve dış kullanıcının erişimi kaldırılır.

1. Dış Kullanıcı ayarlarının yaşam döngüsüne bağlı olarak, dış kullanıcının artık herhangi bir erişim paketi ataması yoksa, dış kullanıcının oturum açması engellenir ve Konuk Kullanıcı hesabı dizininizden kaldırılır.

## <a name="manage-the-lifecycle-of-external-users"></a>Dış kullanıcıların yaşam döngüsünü yönetme

Bir dış Kullanıcı, bir erişim paketi isteği ile dizininizden ne olacağını, artık herhangi bir erişim paketi ataması olmadığını seçebilirsiniz. Bu durum, Kullanıcı tüm erişim paketi atamalarını yeniden alıyorsa veya son erişim paketi atamasının süresi dolarsa meydana gelir. Varsayılan olarak, bir dış kullanıcının artık herhangi bir erişim paketi ataması yoksa, dizininizde oturum açması engellenir. 30 gün sonra, Konuk Kullanıcı hesabı dizininizden kaldırılır.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüde, **Yetkilendirme Yönetimi** bölümünde, **Ayarlar**' a tıklayın.

1. **Düzenle**’ye tıklayın.

    ![Dış kullanıcıların yaşam döngüsünü yönetme ayarları](./media/entitlement-management-external-users/settings-external-users.png)

1. **Dış kullanıcıların yaşam döngüsünü Yönet** bölümünde dış kullanıcılar için farklı ayarları seçin.

1. Dış Kullanıcı herhangi bir erişim paketine son atamasını kaybederse, bu dizinde oturum açmasını engellemek isterseniz, **blok dış kullanıcıyı bu dizine oturum açmasını, bu dizinde** **Evet**olarak ayarlayın.

    > [!NOTE]
    > Bir kullanıcının bu dizinde oturum açması engellenirse, Kullanıcı erişim paketini yeniden isteyemeyecek veya bu dizinde ek erişim isteyemeyecektir. Daha sonra diğer erişim paketlerine erişim istemesi gerekiyorsa, bunları oturum açmasını engellemeyi yapılandırmayın.

1. Dış Kullanıcı herhangi bir erişim paketine son atamasını kaybederse, bu dizindeki Konuk Kullanıcı hesabını kaldırmak istiyorsanız, **dış kullanıcıyı kaldır** seçeneğini **Evet**olarak ayarlayın.

    > [!NOTE]
    > Yetkilendirme Yönetimi yalnızca, yetkilendirme yönetimi aracılığıyla davet edilen hesapları kaldırır. Ayrıca, bir kullanıcının bu dizindeki ve paket atamalarındaki kaynaklara eklenmiş olması durumunda bile bu dizinden oturum açması ve bu dizinden kaldırılması engellendiğine unutmayın. Konuk, erişim paketi atamaları almadan önce bu dizinde mevcutsa kalır. Bununla birlikte, Konuk bir erişim paketi ataması aracılığıyla davet ediliyorsa ve davet edildikten sonra bir OneDrive Iş veya SharePoint Online sitesine atandıktan sonra yine de kaldırılacaktır.

1. Bu dizindeki Konuk Kullanıcı hesabını kaldırmak istiyorsanız, kaldırılmadan önce geçecek gün sayısını ayarlayabilirsiniz. Konuk Kullanıcı hesabını, herhangi bir erişim paketine en son atamasını yitirdikleri anda kaldırmak istiyorsanız, **bu dizinden dış Kullanıcı kaldırılmadan önce geçecek gün sayısını** **0**olarak ayarlayın.

1. **Kaydet** düğmesine tıklayın.

## <a name="enable-a-catalog-for-external-users"></a>Dış kullanıcılar için Katalog etkinleştirme

[Yeni bir katalog](entitlement-management-catalog-create.md)oluşturduğunuzda, kullanıcıların katalogda erişim paketleri istemesi için dış dizinlerden gelen kullanıcılara izin vermek üzere bir ayar vardır. Dış kullanıcıların katalogda erişim paketleri istemek için izinleri olmasını istemiyorsanız, **dış kullanıcılar Için etkin** ' i **Hayır**olarak ayarlayın.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi veya katalog sahibi

![Yeni Katalog bölmesi](./media/entitlement-management-shared/new-catalog.png)

Ayrıca, kataloğu oluşturduktan sonra bu ayarı değiştirebilirsiniz.

![Katalog ayarlarını Düzenle](./media/entitlement-management-shared/catalog-edit.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Dizininizde bulunmayan kullanıcılar için](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
- [Kaynak kataloğu oluşturma ve yönetme](entitlement-management-catalog-create.md)
- [Temsili ve roller](entitlement-management-delegate.md)
