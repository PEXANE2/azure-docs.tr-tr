---
title: Azure AD Domain Services parola ilkeleri oluşturma ve kullanma | Microsoft Docs
description: Azure AD DS yönetilen bir etki alanında hesap parolalarını güvenli hale getirmek ve denetlemek için hassas parola ilkelerinin nasıl ve neden kullanılacağını öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 08/08/2019
ms.author: iainfou
ms.openlocfilehash: 45fb2daaeaf9ee788207d43d805e070320372ca0
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617166"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Yönetilen etki alanlarında parola ve hesap kilitleme ilkeleri

Azure Active Directory Domain Services (Azure AD DS) ' de hesap güvenliğini yönetmek için, minimum parola uzunluğu, parola süre sonu zamanı veya parola karmaşıklığı gibi ayarları denetleyen hassas parola ilkeleri tanımlayabilirsiniz. Varsayılan parola ilkesi, Azure AD DS yönetilen bir etki alanındaki tüm kullanıcılara uygulanır. Ayrıntılı denetim sağlamak ve belirli iş veya uyumluluk ihtiyaçlarını karşılamak için, ek ilkeler oluşturulup belirli kullanıcı gruplarına uygulanabilir.

Bu makalede, Active Directory Yönetim Merkezi kullanarak hassas bir parola ilkesi oluşturma ve yapılandırma açıklanmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlayabilmeniz için aşağıdaki kaynaklar ve ayrıcalıklar gereklidir:

* Etkin bir Azure aboneliği.
  * Azure aboneliğiniz yoksa [bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Abonelikle ilişkili bir Azure Active Directory kiracısı, şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
  * Gerekirse, [bir Azure Active Directory kiracı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin][associate-azure-ad-tenant].
* Azure AD kiracınızda etkinleştirilmiş ve yapılandırılmış Azure Active Directory Domain Services yönetilen bir etki alanı.
  * Gerekirse, [Azure Active Directory Domain Services bir örnek oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi doldurun.
* Azure AD DS yönetilen etki alanına katılmış bir Windows Server Yönetim sanal makinesi.
  * Gerekirse, [bir yönetim sanal makinesi oluşturmak][tutorial-create-management-vm]için öğreticiyi izleyin.
* Azure AD kiracınızda *Azure AD DC Administrators* grubunun üyesi olan bir kullanıcı hesabı.

## <a name="fine-grained-password-policies-fgpp-overview"></a>Hassas parola ilkeleri (FGPP) genel bakış

Hassas parola ilkeleri (FGPPs), bir etki alanındaki farklı kullanıcılara parola ve hesap kilitleme ilkeleri için özel kısıtlamalar uygulamanıza imkan tanır. Örneğin, ayrıcalıklı hesapların güvenliğini sağlamak için, normal ayrıcalıklı olmayan hesaplara göre daha sıkı parola ayarları uygulayabilirsiniz. Azure AD DS yönetilen bir etki alanında parola ilkeleri belirtmek için birden çok FGPPs oluşturabilirsiniz.

Aşağıdaki parola ayarları FGPP kullanılarak yapılandırılabilir:

* Minimum parola uzunluğu
* Parola geçmişi
* Parolaların karmaşıklık gereksinimlerini karşılaması gerekir
* En az parola yaşı
* Maksimum parola yaşı
* Hesap kilitleme ilkesi
  * Hesap kilitleme süresi
  * İzin verilen başarısız oturum açma denemesi sayısı
  * Sonrasında başarısız oturum açma denemesi sayısını Sıfırla

FGPP yalnızca Azure AD DS oluşturulan kullanıcıları etkiler. Azure AD 'de Azure AD DS yönetilen etki alanı ile eşitlenen bulut kullanıcıları ve etki alanı kullanıcıları, parola ilkelerinden etkilenmez.

İlkeler, Azure AD DS yönetilen etki alanındaki grup ilişkilendirmesi aracılığıyla dağıtılır ve yaptığınız tüm değişiklikler sonraki Kullanıcı oturumu sırasında uygulanır. İlkenin değiştirilmesi, zaten kilitlenen bir kullanıcı hesabının kilidini açmıyor.

## <a name="default-fine-grained-password-policy-settings"></a>Varsayılan hassas parola ilkesi ayarları

Azure AD DS yönetilen bir etki alanında, aşağıdaki parola ilkeleri varsayılan olarak yapılandırılır ve tüm kullanıcılara uygulanır:

* **En az parola uzunluğu (karakter):** 7
* **En fazla parola yaşı (ömür):** 90 gün
* **Parolaların karmaşıklık gereksinimlerini karşılaması gerekir**

Aşağıdaki hesap kilitleme ilkeleri daha sonra varsayılan olarak yapılandırılır:

* **Hesap kilitleme süresi:** 30
* **İzin verilen başarısız oturum açma girişimi sayısı:** 5
* **Başarısız oturum açma denemesi sayısı sıfırlama sayısı:** 30 dakika

Bu varsayılan ayarlarla, 2 dakika içinde beş geçersiz parola kullanılırsa, Kullanıcı hesapları 30 dakika boyunca kilitlenir. Hesaplar 30 dakika sonra otomatik olarak açılır.

Varsayılan yerleşik hassas parola ilkesini değiştiremez veya silemezsiniz. Bunun yerine, *AAD DC yöneticileri* grubunun üyeleri BIR özel FGPP oluşturabilir ve bunu, sonraki bölümde gösterildiği gibi varsayılan yerleşik FGPP 'yi geçersiz kılacak şekilde yapılandırabilir (öncelikli hale getirebilirsiniz).

## <a name="create-a-custom-fine-grained-password-policy"></a>Özel bir hassas parola ilkesi oluşturma

Azure 'da ve uygulamalar oluştururken, özel bir FGPP yapılandırmak isteyebilirsiniz. Özel bir FGPP oluşturma ihtiyacı, farklı bir hesap kilitleme ilkesi ayarlamak veya yönetilen etki alanı için varsayılan parola yaşam süresi ayarını yapılandırmak için gereklidir.

Özel bir FGPP oluşturup Azure AD DS yönetilen etki alanındaki belirli gruplara uygulayabilirsiniz. Bu yapılandırma, varsayılan FGPP 'yi etkin bir şekilde geçersiz kılar. Ayrıca, ayrıntılı parola ilkeleri oluşturabilir ve bunları Azure AD DS yönetilen etki alanında oluşturduğunuz özel OU 'Lara uygulayabilirsiniz.

Hassas parola ilkesi oluşturmak için, etki alanına katılmış bir VM 'den Active Directory yönetim araçlarını kullanırsınız. Active Directory Yönetim Merkezi, OU 'Lar dahil olmak üzere Azure AD DS yönetilen bir etki alanında kaynakları görüntülemenize, düzenlemenize ve oluşturmanıza olanak sağlar.

> [!NOTE]
> Azure AD DS yönetilen bir etki alanında hassas bir parola ilkesi oluşturmak için, *AAD DC Administrators* grubunun üyesi olan bir kullanıcı hesabında oturum açmış olmanız gerekir.

1. Başlangıç ekranından **Yönetim Araçları**' nı seçin. [Yönetim sanal makinesi oluşturmak][tutorial-create-management-vm]için öğreticide yüklü olan kullanılabilir yönetim araçlarının bir listesi gösterilir.
1. OU 'Ları oluşturup yönetmek için, yönetim araçları listesinden **Active Directory Yönetim Merkezi** ' yi seçin.
1. Sol bölmede, *contoso.com*gibi Azure AD DS yönetilen etki alanınızı seçin.
1. Sağdaki **Görevler** panelinde **Yeni > parola ayarları**' nı seçin.
1. **Parola ayarlarını oluştur** iletişim kutusunda, Ilke Için *Mycustomfgpp*gibi bir ad girin. Varsayılan FGPP 'yi ( *200*olan *1*) geçersiz kılmak için önceliği uygun olarak ayarlayın.

    Kullanıcının önceki *24* paroladan farklı bir parola oluşturmasını gerektirmek için **Parola geçmişini zorlama** gibi diğer parola ilkesi ayarlarını istediğiniz gibi düzenleyin.

    ![Özel bir hassas parola ilkesi oluşturma](./media/how-to/custom-fgpp.png)

1. **Yanlışlıkla silinmeye karşı koru**seçeneğinin işaretini kaldırın. Bu seçenek işaretliyse, FGPP 'yi kaydedemezsiniz.
1. **Doğrudan Için geçerli** bölümünde, **Ekle** düğmesini seçin. **Kullanıcıları veya grupları seç** Iletişim kutusunda **konumlar** düğmesine tıklayın.

    ![Parola ilkesinin uygulanacağı kullanıcıları ve grupları seçin](./media/how-to/fgpp-applies-to.png)

1. Hassas parola ilkeleri yalnızca gruplara uygulanabilir. **Konumlar** iletişim kutusunda, *contoso.com*gibi etki alanı adını genişletin ve ardından **aaddc kullanıcıları**gibi bir OU seçin. Uygulamak istediğiniz kullanıcı grubunu içeren özel bir OU varsa, bu OU 'yu seçin.

    ![Grubun ait olduğu OU 'yu seçin](./media/how-to/fgpp-container.png)

1. İlkeyi uygulamak istediğiniz grubun adını yazın ve ardından grubun var olduğunu doğrulamak için **adları denetle** ' yi seçin.

    ![FGPP uygulanacak grubu arayıp seçin](./media/how-to/fgpp-apply-group.png)

1. Şimdi, bölümünde seçtiğiniz grubun adı bölüm **Için geçerli** olarak görüntüleniyorsa, özel parola ilkenizi kaydetmek için **Tamam** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Hassas parola ilkeleri hakkında daha fazla bilgi ve Active Directory Yönetim Merkezi 'ni kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hassas parola ilkeleri hakkında bilgi edinin](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [AD Yönetim merkezini kullanarak hassas parola ilkelerini yapılandırma](/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
