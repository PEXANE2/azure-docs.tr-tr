---
title: Azure AD Etki Alanı Hizmetlerinde kuruluş birimi (OU) oluşturma | Microsoft Dokümanlar'
description: Azure AD Etki Alanı Hizmetleri yönetilen etki alanında özel bir Kuruluş Birimi (OU) oluşturma ve yönetme yi öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: 61f951c0dd6561fc8d5a5de6b80e3759fd42eb78
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655552"
---
# <a name="create-an-organizational-unit-ou-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanında Kuruluş Birimi (OU) oluşturma

Etkin Dizin Etki Alanı Hizmetleri'ndeki (AD DS) kuruluş birimleri (OSB), kullanıcı hesapları, hizmet hesapları veya bilgisayar hesapları gibi nesneleri mantıksal olarak gruplandırmanıza izin verir. Daha sonra belirli İş'lere yöneticiler atayabilir ve hedeflenen yapılandırma ayarlarını zorlamak için grup ilkesi uygulayabilirsiniz.

Azure AD DS yönetilen etki alanları aşağıdaki iki yerleşik OS'leri içerir:

* *AADDC Computers* - yönetilen etki alanına birleştirilmiş tüm bilgisayarlar için bilgisayar nesneleri içerir.
* *AADDC Kullanıcıları* - Azure AD kiracısından senkronize edilmiş kullanıcıları ve grupları içerir.

Azure AD DS kullanan iş yükleri oluşturup çalıştırırken, uygulamaların kimlik doğrulaması için hizmet hesapları oluşturmanız gerekebilir. Bu hizmet hesaplarını düzenlemek için, azure AD DS yönetilen etki alanında genellikle özel bir OU oluşturur ve ardından bu OU içinde hizmet hesapları oluşturursunuz.

Karma bir ortamda, şirket içi AD DS ortamında oluşturulan OS'ler Azure AD DS ile eşitlenmez. Azure AD DS yönetilen etki alanları düz bir OU yapısı kullanır. Tüm kullanıcı hesapları ve grupları, farklı şirket içi etki alanlarından veya ormanlardan senkronize edilmiş olsa bile, burada hiyerarşik bir OU yapısı yapılandırmış olsanız bile *AADDC Kullanıcıları* konteynerinde depolanır.

Bu makalede, Azure AD DS yönetilen etki alanınızda nasıl ou oluşturabileceğiniz gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, bir Azure [Active Directory Etki Alanı Hizmetleri örneği oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi tamamlayın.
* Azure AD DS yönetilen etki alanına katılan bir Windows Server yönetimi VM'si.
    * Gerekirse, bir yönetim [VM oluşturmak][tutorial-create-management-vm]için öğretici tamamlayın.
* Azure AD kiracınızda *Azure AD DC yöneticileri* grubunun üyesi olan bir kullanıcı hesabı.

## <a name="custom-ou-considerations-and-limitations"></a>Özel OU hususlarve sınırlamalar

Azure AD DS yönetilen bir etki alanında özel OSB oluşturduğunuzda, kullanıcı yönetimi ve grup ilkesi uygulama için ek yönetim esnekliği kazanırsınız. Şirket içi AD DS ortamıyla karşılaştırıldığında, Azure AD DS'de özel bir OU yapısı oluştururken ve yönetirken bazı sınırlamalar ve dikkat edilmesi gereken ler vardır:

* Özel İş'ler oluşturmak için kullanıcıların *AAD DC Yöneticileri* grubunun bir üyesi olması gerekir.
* Özel bir OU oluşturan bir kullanıcıya, bu OU üzerinde yönetim ayrıcalıkları (tam denetim) verilir ve kaynak sahibi dir.
    * Varsayılan olarak, *AAD DC Yöneticileri* grubu da özel OU tam kontrole sahiptir.
* *AADDC Kullanıcıları* için varsayılan OU, Azure AD kiracınızdaki tüm eşitlenmiş kullanıcı hesaplarını içeren oluşturulur.
    * Kullanıcıları veya grupları *AADDC Users* OU'dan oluşturduğunuz özel OU'lara taşıyamazsınız. Yalnızca Azure AD DS yönetilen etki alanında oluşturulan kullanıcı hesapları veya kaynakları özel İŞ'lere taşınabilir.
* Özel İŞ'ler altında oluşturduğunuz kullanıcı hesapları, gruplar, hizmet hesapları ve bilgisayar nesneleri Azure AD kiracınızda kullanılamaz.
    * Bu nesneler Microsoft Graph API'sini kullanarak veya Azure REKLAM Arabirimi'nde görünmüyor; bunlar yalnızca Azure AD DS yönetilen etki alanınızda kullanılabilir.

## <a name="create-a-custom-ou"></a>Özel bir OU oluşturma

Özel bir OU oluşturmak için, etki alanına katılan bir VM'den Etkin Dizin Yönetim Araçları'nı kullanırsınız. Etkin Dizin Yönetim Merkezi, OS B'ler de dahil olmak üzere Azure AD DS yönetilen bir etki alanında kaynakları görüntülemenize, görüntülemenize ve oluşturmanıza olanak tanır.

> [!NOTE]
> Azure AD DS yönetilen bir etki alanında özel bir OU oluşturmak için, *AAD DC Yöneticileri* grubunun üyesi bir kullanıcı hesabında oturum açmanız gerekir.

1. Yönetim VM'nizde oturum açın. Azure portalını kullanarak bağlanma adımlarını öğrenmek [için windows server vm'ye bağlan'a][connect-windows-server-vm]bakın.
1. Başlangıç ekranından **Yönetim Araçları'nı**seçin. Kullanılabilir yönetim araçlarının [listesi, bir yönetim VM oluşturmak][tutorial-create-management-vm]için öğreticiyüklü olarak gösterilmiştir.
1. OS B'ler oluşturmak ve yönetmek için, yönetim araçları listesinden **Active Directory Administrative Center'ı** seçin.
1. Sol bölmede, *aaddscontoso.com*gibi Azure AD DS yönetilen etki alanınızı seçin. Varolan İŞ VE kaynakların listesi gösterilir:

    ![Etkin Dizin Yönetim Merkezi'nde Azure AD DS yönetilen etki alanınızı seçin](./media/create-ou/create-ou-adac-overview.png)

1. **Görevler** bölmesi Active Directory Administrative Center'ın sağ tarafında gösterilir. *etki*alanı altında, aaddscontoso.com gibi, Yeni > **Kuruluş Birimi**seçin.

    ![Active Directory Administrative Center'da yeni bir OU oluşturma seçeneğini seçin](./media/create-ou/create-ou-adac-new-ou.png)

1. Kuruluş **Birimi Oluştur** iletişim kutusunda, *MyCustomOu*gibi yeni OU için bir **Ad** belirtin. Hizmet hesapları için *Özel OU*gibi OU için kısa bir açıklama sağlayın. İstenirse, OU için **Yönetilen** Alan'ı da ayarlayabilirsiniz. Özel OU oluşturmak için **Tamam'ı**seçin.

    ![Active Directory Administrative Center'dan özel bir OU oluşturma](./media/create-ou/create-ou-dialog.png)

1. Active Directory Administrative Center'da, özel OU artık listelenir ve kullanılabilir:

    ![Active Directory Administrative Center'da kullanılmak üzere özel OU](./media/create-ou/create-ou-done.png)

## <a name="next-steps"></a>Sonraki adımlar

Yönetim araçlarını kullanma veya hizmet hesapları oluşturma ve kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Aktif Dizin Yönetim Merkezi: Başlarken](https://technet.microsoft.com/library/dd560651.aspx)
* [Hizmet Hesapları Adım Adım Kılavuzu](https://technet.microsoft.com/library/dd548356.aspx)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm
