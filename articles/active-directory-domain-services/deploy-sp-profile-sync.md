---
title: Azure AD DS ile SharePoint Kullanıcı Profili hizmetini etkinleştirin | Microsoft Dokümanlar
description: SharePoint Server için profil eşitlemeyi desteklemek için Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanını nasıl yapılandırabileceğinizi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 01/21/2020
ms.author: iainfou
ms.openlocfilehash: a684a669c491e35b5c6b62dd318b4fe61edeb52b
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655388"
---
# <a name="configure-azure-active-directory-domain-services-to-support-user-profile-synchronization-for-sharepoint-server"></a>SharePoint Server için kullanıcı profili eşitlemesi desteklemek için Azure Active Directory Etki Alanı Hizmetlerini yapılandırın

SharePoint Server, kullanıcı profillerini eşitlemek için bir hizmet içerir. Bu özellik, kullanıcı profillerinin merkezi bir konumda depolanmasına ve birden çok SharePoint sitesi ve çiftliğinde erişilebilir olmasını sağlar. SharePoint Server kullanıcı profili hizmetini yapılandırmak için, uygun izinlerin Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanında verilmesi gerekir. Daha fazla bilgi için [SharePoint Server'da kullanıcı profili eşitleme](https://technet.microsoft.com/library/hh296982.aspx)konusuna bakın.

Bu makalede, SharePoint Server kullanıcı profili eşitleme hizmetine izin verecek şekilde Azure AD DS'yi nasıl yapılandırabileceğiniz gösterilmektedir.

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
* Kullanıcı profili eşitleme hizmeti için bir SharePoint hizmet hesabı.
    * Gerekirse, [SharePoint Server'daki yönetim ve hizmet hesapları için Plan'a][sharepoint-service-account]bakın.

## <a name="service-accounts-overview"></a>Hizmet hesaplarına genel bakış

Azure AD DS yönetilen etki alanında, *Kullanıcılar* kuruluş biriminin (OU) bir parçası olarak **AAD DC Hizmet Hesapları** adında bir güvenlik grubu bulunur. Bu güvenlik grubunun üyelerine aşağıdaki ayrıcalıklar verilir:

- DSE kökünde Dizin Değişiklikleri ayrıcalığını **çoğaltın.**
- *Yapılandırma* adlandırma bağlamında`cn=configuration` **Dizin Değişiklikleri ayrıcalığını (kapsayıcı) çoğaltma.**

**AAD DC Service Accounts** güvenlik grubu da yerleşik grup **Pre-Windows 2000 Uyumlu Erişim**üyesidir.

Bu güvenlik grubuna eklendiğinde, SharePoint Server kullanıcı profili eşitleme hizmetinin hizmet hesabına doğru çalışması için gerekli ayrıcalıklar verilir.

## <a name="enable-support-for-sharepoint-server-user-profile-sync"></a>SharePoint Server kullanıcı profili eşitlemi için desteği etkinleştirme

SharePoint Server'ın hizmet hesabı, dizindeki değişiklikleri çoğaltmak ve SharePoint Server kullanıcı profilinin doğru çalışmasına izin vermek için yeterli ayrıcalıklara ihtiyaç duyar. Bu ayrıcalıkları sağlamak için, SharePoint kullanıcı profili eşitlemesi için kullanılan hizmet hesabını **AAD DC Hizmet Hesapları** grubuna ekleyin.

Azure AD DS yönetimi VM'nizden aşağıdaki adımları tamamlayın:

> [!NOTE]
> Azure AD DS yönetilen bir etki alanında grup üyeliğini yeniden oluşturmak için, *AAD DC Yöneticileri* grubunun üyesi bir kullanıcı hesabında oturum açmanız gerekir.

1. Başlangıç ekranından **Yönetim Araçları'nı**seçin. Kullanılabilir yönetim araçlarının [listesi, bir yönetim VM oluşturmak][tutorial-create-management-vm]için öğreticiyüklü olarak gösterilmiştir.
1. Grup üyeliğini yönetmek için, yönetim araçları listesinden **Active Directory Administrative Center'ı** seçin.
1. Sol bölmede, *aaddscontoso.com*gibi Azure AD DS yönetilen etki alanınızı seçin. Varolan İŞ ve kaynakların listesi gösterilir.
1. **Kullanıcılar** OU'yu seçin ve ardından *AAD DC Hizmet Hesapları* güvenlik grubunu seçin.
1. **Üye**seçin, sonra **Ekle'yi seçin... seçeneğini belirleyin.**
1. SharePoint hizmet hesabının adını girin ve **ardından Tamam'ı**seçin. Aşağıdaki örnekte, SharePoint hizmet hesabı *spadmin*adlı:

    ![SharePoint hizmet hesabını AAD DC Hizmet Hesapları güvenlik grubuna ekleme](./media/deploy-sp-profile-sync/add-member-to-aad-dc-service-accounts-group.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [SharePoint Server'da profil eşitleme için Grant Active Directory Etki Alanı Hizmetleri izinlerine](https://technet.microsoft.com/library/hh296982.aspx) bakın

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md

<!-- EXTERNAL LINKS -->
[sharepoint-service-account]: /sharepoint/security-for-sharepoint-server/plan-for-administrative-and-service-accounts
