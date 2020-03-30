---
title: Azure AD Etki Alanı Hizmetlerinde parola ilkeleri oluşturma ve kullanma | Microsoft Dokümanlar
description: Azure AD DS yönetilen bir etki alanında hesap parolalarını korumak ve denetlemek için ince taneli parola ilkelerini nasıl ve neden kullanacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 01/21/2020
ms.author: iainfou
ms.openlocfilehash: c4402c1ce2f051c8d1911e7c0332d4cac787ce1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77613202"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Yönetilen etki alanlarında parola ve hesap kilitleme ilkeleri

Azure Etkin Dizin Etki Alanı Hizmetlerinde (Azure AD DS) kullanıcı güvenliğini yönetmek için, hesap kilitleme ayarlarını veya minimum parola uzunluğu ve karmaşıklığını kontrol eden ince taneli parola ilkeleri tanımlayabilirsiniz. Varsayılan ince taneli parola ilkesi oluşturulur ve Azure AD DS yönetilen etki alanında tüm kullanıcılara uygulanır. Parçalı denetim sağlamak ve belirli iş veya uyumluluk gereksinimlerini karşılamak için, ek ilkeler oluşturulabilir ve belirli kullanıcı gruplarına uygulanabilir.

Bu makalede, Etkin Dizin Yönetim Merkezi'ni kullanarak Azure AD DS'de ince taneli bir parola ilkesinin nasıl oluşturulup yapılandırılabildiğini gösterilmektedir.

> [!NOTE]
> Parola ilkeleri yalnızca Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan Azure AD DS yönetilen etki alanları için kullanılabilir. Classic kullanılarak oluşturulan eski yönetilen etki alanları için, [Klasik sanal ağ modelinden Kaynak Yöneticisi'ne geçirin.][migrate-from-classic]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
  * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
  * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
  * Gerekirse, bir Azure [Active Directory Etki Alanı Hizmetleri örneği oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi tamamlayın.
  * Azure AD DS örneği Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulmuş olmalıdır. Gerekirse, [Klasik sanal ağ modelinden Kaynak Yöneticisi'ne geçirin.][migrate-from-classic]
* Azure AD DS yönetilen etki alanına katılan bir Windows Server yönetimi VM'si.
  * Gerekirse, bir yönetim [VM oluşturmak][tutorial-create-management-vm]için öğretici tamamlayın.
* Azure AD kiracınızda *Azure AD DC yöneticileri* grubunun üyesi olan bir kullanıcı hesabı.

## <a name="default-password-policy-settings"></a>Varsayılan parola ilkesi ayarları

Ayrıntılı parola ilkeleri (FGPPs), bir etki alanında farklı kullanıcılara parola ve hesap kilitleme ilkeleri için belirli kısıtlamalar uygulamanıza izin verir. Örneğin, ayrıcalıklı hesapların güvenliğini sağlamak için normal ayrıcalıklı olmayan hesaplara göre daha sıkı hesap kilitleme ayarları uygulayabilirsiniz. Azure AD DS yönetilen bir etki alanında birden çok FGPP oluşturabilir ve bunları kullanıcılara uygulamak için öncelik sırasını belirtebilirsiniz.

İlkeler, Azure AD DS yönetilen bir etki alanında grup ilişkilendirme yoluyla dağıtılır ve yaptığınız tüm değişiklikler bir sonraki kullanıcı oturum açma sırasında uygulanır. İlkenin değiştirilmesi, zaten kilitlenmiş bir kullanıcı hesabının kilidini açmaz.

Parola ilkeleri, uygulandığı kullanıcı hesabının nasıl oluşturulduğuna bağlı olarak biraz farklı davranılır. Azure AD DS'de bir kullanıcı hesabının oluşturulabileceği iki yol vardır:

* Kullanıcı hesabı Azure AD'den senkronize edilebilir. Buna, doğrudan Azure'da oluşturulan yalnızca bulutkullanıcı hesapları ve Azure AD Connect'i kullanarak şirket içi AD DS ortamından senkronize edilen karma kullanıcı hesapları dahildir.
    * Azure AD DS'deki kullanıcı hesaplarının çoğu, Azure AD'nin eşitleme işlemi yle oluşturulur.
* Kullanıcı hesabı, Azure AD DS yönetilen bir etki alanında el ile oluşturulabilir ve Azure AD'de bulunmaz.

Tüm kullanıcılar, nasıl oluşturulduklarına bakılmaksızın, Azure AD DS'deki varsayılan parola ilkesi tarafından uygulanan aşağıdaki hesap kilitleme ilkelerine sahiptir:

* **Hesap kilitleme süresi:** 30
* **İzin verilen başarısız oturum açma denemelerinin sayısı:** 5
* **Başarısız oturum açma denemelerini sıfırlama:** 30 dakika sonra
* **Maksimum parola yaşı (ömür boyu):** 90 gün

Bu varsayılan ayarlarla, beş geçersiz parola 2 dakika içinde kullanılırsa, kullanıcı hesapları 30 dakika süreyle kilitlenir. Hesaplar 30 dakika sonra otomatik olarak açılır.

Hesap kilitlemeleri yalnızca yönetilen etki alanında gerçekleşir. Kullanıcı hesapları yalnızca Azure AD DS'de ve yalnızca yönetilen etki alanına yönelik başarısız oturum açma girişimleri nedeniyle kilitlenir. Azure AD'den veya şirket içinde senkronize edilen kullanıcı hesapları kaynak dizinlerinde, yalnızca Azure AD DS'lerinde kilitli değildir.

En fazla parola yaşını 90 günden fazla belirten bir Azure AD parola ilkeniz varsa, bu parola yaşı Azure AD DS'deki varsayılan ilkeiçin uygulanır. Azure AD DS'de farklı bir maksimum parola yaşı tanımlamak için özel bir parola ilkesi yapılandırabilirsiniz. Azure AD DS parola ilkesinde yapılandırılan maksimum parola yaşınız Azure AD veya şirket içi AD DS ortamından daha kısaysa dikkatli olun. Bu senaryoda, bir kullanıcının parolası Azure AD veya şirket içi AD DS ortamında değişiklik istenmeden önce Azure AD DS'de sona erebilir.

Azure AD DS yönetilen etki alanında el ile oluşturulan kullanıcı hesapları için varsayılan ilkeden aşağıdaki ek parola ayarları da uygulanır. Bu ayarlar Azure AD'den senkronize edilen kullanıcı hesapları için geçerli değildir, çünkü kullanıcı parolalarını doğrudan Azure AD DS'de güncelleştiremez.

* **Minimum parola uzunluğu (karakter):** 7
* **Parolalar karmaşıklık gereksinimlerini karşılamalıdır**

Varsayılan parola ilkesinde hesap kilitleme veya parola ayarlarını değiştiremezsiniz. Bunun yerine, *AAD DC Yöneticileri* grubunun üyeleri özel parola ilkeleri oluşturabilir ve bir sonraki bölümde gösterildiği gibi varsayılan yerleşik ilkeyi geçersiz kılacak (öncegelen) olarak yapılandırabilir.

## <a name="create-a-custom-password-policy"></a>Özel parola ilkesi oluşturma

Azure'da uygulama oluşturup çalıştırırken, özel bir parola ilkesi yapılandırmak isteyebilirsiniz. Örneğin, farklı hesap kilitleme ilkesi ayarlarını ayarlamak için bir ilke oluşturabilirsiniz.

Azure AD DS yönetilen etki alanında gruplara özel parola ilkeleri uygulanır. Bu yapılandırma varsayılan ilkeyi etkin bir şekilde geçersiz kılar.

Özel bir parola ilkesi oluşturmak için etki alanına katılan bir VM'den Etkin Dizin Yönetim Araçları'nı kullanırsınız. Etkin Dizin Yönetim Merkezi, OS B'ler de dahil olmak üzere Azure AD DS yönetilen bir etki alanında kaynakları görüntülemenize, görüntülemenize ve oluşturmanıza olanak tanır.

> [!NOTE]
> Azure AD DS yönetilen bir etki alanında özel bir parola ilkesi oluşturmak için, *AAD DC Yöneticileri* grubunun üyesi bir kullanıcı hesabında oturum açmanız gerekir.

1. Başlangıç ekranından **Yönetim Araçları'nı**seçin. Kullanılabilir yönetim araçlarının [listesi, bir yönetim VM oluşturmak][tutorial-create-management-vm]için öğreticiyüklü olarak gösterilmiştir.
1. OS B'ler oluşturmak ve yönetmek için, yönetim araçları listesinden **Active Directory Administrative Center'ı** seçin.
1. Sol bölmede, *aaddscontoso.com*gibi Azure AD DS yönetilen etki alanınızı seçin.
1. **Sistem** kapsayıcısını açın, ardından **Parola Ayarları Kapsayıcısı'** nı .

    Azure AD DS yönetilen etki alanı için yerleşik bir parola ilkesi gösterilir. Bu yerleşik ilkeyi değiştiremezsiniz. Bunun yerine, varsayılan ilkegeçersiz kılmak için özel bir parola ilkesi oluşturun.

    ![Etkin Dizin Yönetim Merkezi'nde parola ilkesi oluşturma](./media/password-policy/create-password-policy-adac.png)

1. Sağdaki **Görevler** panelinde **Yeni > Parola Ayarları'nı**seçin.
1. Parola **Ayarları Oluştur** iletişim kutusunda, *MyCustomFGPP*gibi ilke için bir ad girin.
1. Birden çok parola ilkesi varsa, en yüksek önceliğe veya önceliğe sahip ilke bir kullanıcıya uygulanır. Sayı ne kadar düşükse, öncelik de o kadar yüksektir. Varsayılan parola *ilkesinin önceliği 200'dür.*

    *Varsayılanı*geçersiz kılmak için özel parola ilkenizin önceliğini 1 olarak ayarlayın.

1. Diğer parola ilkesi ayarlarını istediğiniz gibi edin. Aşağıdaki önemli noktaları unutmayın:

    * Parola karmaşıklığı, yaş veya son kullanma süresi gibi ayarlar yalnızca Azure AD DS yönetilen bir etki alanında el ile oluşturulan kullanıcılar için.
    * Hesap kilitleme ayarları tüm kullanıcılar için geçerlidir, ancak yalnızca yönetilen etki alanında etkili olur, Azure AD'nin kendisinde değil.

    ![Özel bir ince taneli parola ilkesi oluşturma](./media/how-to/custom-fgpp.png)

1. Uncheck **Yanlışlıkla silmeye karşı koruyun.** Bu seçenek seçilirse, FGPP kaydedemez.
1. Doğrudan **Uygulan** bölümünde **Ekle** düğmesini seçin. Kullanıcıları **seç veya Gruplar** iletişim kutusunda **Konumlar** düğmesini seçin.

    ![Parola ilkesini uygulamak için kullanıcıları ve grupları seçin](./media/how-to/fgpp-applies-to.png)

1. Parola ilkeleri yalnızca gruplara uygulanabilir. **Konumlar** iletişim kutusunda, *aaddscontoso.com*gibi etki alanı adını genişletin ve ardından **AADDC Kullanıcıları**gibi bir OU seçin. Uygulamak istediğiniz bir kullanıcı grubu içeren özel bir OU'nuz varsa, bu OU'yi seçin.

    ![Grubun ait olduğu OU'yu seçin](./media/how-to/fgpp-container.png)

1. İlkeyi uygulamak istediğiniz grubun adını yazın ve grubun var olduğunu doğrulamak için **Adları Denetle'yi** seçin.

    ![FGPP uygulamak için grubu arayın ve seçin](./media/how-to/fgpp-apply-group.png)

1. **Şimdi Doğrudan Aapply Bölümünde** görüntülenen seçtiğiniz grubun adı ile, özel parola ilkenizi kaydetmek için **Tamam'ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Parola ilkeleri ve Active Directory Administration Center'ı kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [İnce taneli parola ilkeleri hakkında bilgi edinin](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [AD Yönetim Merkezi'ni kullanarak ince taneli parola ilkelerini yapılandırma](/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[migrate-from-classic]: migrate-from-classic-vnet.md
