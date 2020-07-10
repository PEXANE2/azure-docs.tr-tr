---
title: Azure Kurumsal aktarımlar
description: Azure EA aktarımlarını açıklar
author: bandersmsft
ms.reviewer: baolcsva
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 07/06/2020
ms.author: banders
ms.openlocfilehash: ff10d9ec9b92b85d057e556a21c144b9e8f155cf
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86043283"
---
# <a name="azure-enterprise-transfers"></a>Azure Kurumsal aktarımlar

Bu makalede kurumsal aktarımlara ilişkin bir genel bakış sunulmaktadır.

## <a name="transfer-an-enterprise-account-to-a-new-enrollment"></a>Kurumsal bir hesabı yeni bir kayda aktarma

Hesap aktarımı işlemi, bir hesap sahibini bir kayıttan diğerine taşır. Hesap sahibine ilişkin tüm abonelikler hedef kayda taşınır. Birden fazla etkin kaydınız olduğunda ve yalnızca seçili hesap sahiplerini taşımak istediğinizde hesap aktarımını kullanın.

Eylem bir kurumsal yönetici tarafından gerçekleştirilemediği için bu bölüm yalnızca bilgilendirme amaçlıdır. Bir kurumsal hesabı yeni bir kayda aktarmak için destek isteği oluşturulması gerekir.

Kurumsal bir hesabı yeni bir kayda aktarırken aşağıdaki noktaları aklınızda bulundurun:

- Yalnızca istekte belirtilen hesaplar aktarılır. Tüm hesaplar seçilirse hepsi aktarılır.
- Kaynak kaydının durumu etkin veya genişletilmiş olarak korunur. Kaydı süresi dolana kadar kullanmaya devam edebilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

Bir hesap aktarımı istediğinizde aşağıdaki bilgileri sağlayın:

- Hedef kaydın sayısı, hesap adı ve aktarımın yapılacağı hesabın sahibinin e-posta adresi
- Kaynak kaydı için, aktarılacak kayıt numarası ve hesap

Hesap aktarımından önce göz önünde bulundurmanız gereken diğer noktalar:

- Hedef ve kaynak kaydı için bir EA yöneticisinden onay gerekir
- Hesap aktarımı gereksinimlerinizi karşılamıyorsa, kayıt aktarımı yapmayı düşünün.
- Hesap aktarımı, belirli hesaplarla ilgili tüm hizmetleri ve abonelikleri aktarır.
- Aktarım tamamlandıktan sonra, aktarılan hesap kaynak kayıt altında etkin değil olarak, hedef kaydın altında ise etkin olarak görünür.
- Hesap, kaynak kayıtta etkin aktarım tarihine ve hedef kayıtta başlangıç tarihine karşılık gelen bitiş tarihini gösterir.
- Hesap için etkin aktarım tarihi öncesinde gerçekleştirilen tüm kullanımlar, kaynak kaydın altında kalır.

## <a name="transfer-enterprise-enrollment-to-a-new-one"></a>Kuruluş kaydını yeni bir kayda aktarma

Kayıt aktarımı şu durumlarda göz önünde bulundurulur:

- Geçerli bir kaydın Ön Ödeme döneminin sona ermesi.
- Bir kaydın süresi dolmuş/uzatılmış durumda olması ve yeni bir anlaşmanın yapılması.
- Birden fazla kaydınızın olması ve tüm hesaplarla faturaları tek kayıtta birleştirmek istemeniz.

Eylem bir kurumsal yönetici tarafından gerçekleştirilemediği için bu bölüm yalnızca bilgilendirme amaçlıdır. Bir kurumsal kaydı yeni bir kayda aktarmak için destek isteği oluşturulması gerekir.

Bir kuruluş kaydının tamamını bir kayda aktarmak istediğinizde aşağıdaki eylemler gerçekleşir:

- Tüm EA departman yöneticileri de dahil olmak üzere tüm Azure hizmetleri, abonelikleri, hesapları, departmanları ve kayıt yapısının tamamı yeni hedef kayda aktarılır.
- Kayıt durumu _Aktarıldı_ olarak ayarlanır. Aktarılan kayıt yalnızca geçmiş kullanım raporlama amacıyla kullanılabilir.
- Aktarılan bir kayda rol veya abonelik ekleyemezsiniz. Aktarıldı durumu, kayda aykırı ek kullanım yapılmasını önler.
- Anlaşmadaki kalan Azure Ön Ödeme bakiyeleri, geleceğe dönük hükümler de dahil olmak üzere kaybedilir.
-    Aktardığınız kayıtta satın alınmış ayrılmış örnekler varsa ayrılmış örnek satın alma ücreti kaynak kayıtta kalır ancak tüm ayrılmış örnek avantajları yeni kayıtta kullanılmak üzere aktarılır.
-    Market için tek seferlik satın alma ücreti ve eski kayıtta tahakkuk etmiş olan aylık sabit ücretler yeni kayda aktarılmaz. Tüketime dayalı market ücretleri aktarılır.

### <a name="effective-transfer-date"></a>Geçerli aktarım tarihi

Geçerli aktarım tarihi, hedef kaydın başlangıç tarihi veya daha sonraki bir tarih olabilir.

Kaynak kayıt kullanımı, Azure Ön Ödemesi karşılığında veya fazla kullanım olarak ücretlendirilir. Geçerli aktarım tarihinden sonra gerçekleşen kullanımlar yeni kayda aktarılır ve uygun şekilde ücretlendirilir.

### <a name="prerequisites"></a>Ön koşullar

Bir kayıt aktarımı istediğinizde aşağıdaki bilgileri sağlayın:

- Kaynak kaydı için, kayıt numarası.
- Hedef kaydı için, aktarımın yapılacağı kayıt numarası.
- Kayıt aktarımı geçerlilik tarihi, hedef kaydın başlangıç tarihi veya sonrasındaki bir tarih olabilir. Seçilen tarih, daha önce düzenlenmiş bir fazla kullanım faturası için kullanımı etkilemez.

Kayıt aktarımından önce göz önünde bulundurmanız gereken diğer noktalar:

- Hem hedef hem de kaynak kaydı EA Yöneticilerinden onay gerekir.
- Kayıt aktarımı gereksinimlerinizi karşılamıyorsa, hesap aktarımı yapmayı düşünün.
- Kaynak kaydı durumu aktarıldı olarak güncelleştirilir ve yalnızca geçmiş kullanım raporlama amaçları için kullanılabilir olur.

### <a name="azure-prepayment"></a>Azure Ön Ödemesi

Azure Ön Ödemesi, kayıtlar arasında aktarılamaz. Azure Ön Ödeme bakiyeleri, sipariş edildiği kayda sözleşmeyle bağlıdır. Azure Ön Ödemesi, hesap veya kayıt aktarma işleminin bir parçası olarak aktarılmaz.

### <a name="no-services-affected-for-account-and-enrollment-transfers"></a>Hesap ve kayıt aktarımları için etkilenen hizmet yok

Hesap veya kayıt aktarımı esnasında kesinti süresi yoktur. Gerekli tüm bilgiler sağlanırsa isteğiniz gün içinde tamamlanabilir.

## <a name="change-account-owner"></a>Hesap sahibini değiştirme

Azure EA portalı, abonelikleri bir hesap sahibinden diğerine aktarabilir. Daha fazla bilgi için bkz. [Hesap sahibini değiştirme](ea-portal-get-started.md#change-account-owner).

## <a name="subscription-transfer-effects"></a>Abonelik aktarımının etkileri

Bir Azure aboneliği aynı Azure Active Directory kiracısındaki bir hesaba aktarılırsa, abonelikteki kaynakları yönetmek için [rol tabanlı erişim denetimine (RBAC)](../../role-based-access-control/overview.md) sahip olan tüm kullanıcılar, gruplar ve hizmet sorumluları aynı erişime sahip olmaya devam eder.

Aboneliğe RBAC erişimi olan kullanıcıları görüntülemek için:

1. Azure portalında **Abonelikler**’i açın.
2. Görüntülemek istediğiniz aboneliği seçin ve ardından **Erişim denetimi (IAM)** seçeneğini belirleyin.
3. **Rol atamaları**’nı seçin. Rol atamaları sayfasında, aboneliğe RBAC erişimi olan tüm kullanıcılar listelenir.

Abonelik farklı bir Azure AD kiracısındaki bir hesaba aktarılırsa, abonelikteki kaynakları yönetmek için [RBAC](../../role-based-access-control/overview.md) iznine sahip olan tüm kullanıcılar, gruplar ve hizmet sorumluları _erişimi_ kaybeder. RBAC erişimi mevcut olmasa da aboneliğe aşağıdaki güvenlik mekanizmaları aracılığıyla erişilebilir:

- Abonelik kaynaklarına kullanıcı yöneticisi hakları veren yönetim sertifikaları. Daha fazla bilgi için bkz. [Azure için Yönetim Sertifikası Oluşturma ve Karşıya Yükleme](../../cloud-services/cloud-services-certs-create.md).
- Depolama gibi hizmetler için erişim anahtarları. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../../storage/common/storage-account-overview.md).
- Azure Sanal Makineleri gibi hizmetler için Uzaktan Erişim kimlik bilgileri.

Alıcının Azure kaynaklarına erişimi kısıtlaması gerekiyorsa, hizmetle ilişkili tüm gizli dizileri güncelleştirmeyi düşünmelidir. Çoğu kaynak aşağıdaki adımlar kullanılarak güncelleştirilebilir:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin.
3. Kaynağı seçin.
4. Kaynak sayfasında, mevcut gizli dizileri görüntülemek ve güncelleştirmek için **Ayarlar**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure EA portalı sorunlarını gidermek için yardıma ihtiyacınız varsa [Azure EA portalı erişim sorunlarını giderme](ea-portal-troubleshoot.md) konusuna bakın.