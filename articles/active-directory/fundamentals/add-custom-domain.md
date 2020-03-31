---
title: Özel etki alanınızı ekleyin - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Etkin Dizin'i kullanarak özel bir etki alanı nın nasıl eklenacağına ilişkin yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: ajburnle
ms.reviewer: elkuzmen
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: de91bd7e1e4c5f9909213f663dd3ede0f979d4de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262158"
---
# <a name="add-your-custom-domain-name-using-the-azure-active-directory-portal"></a>Azure Active Directory portalını kullanarak özel alan adınızı ekleyin

Her yeni Azure AD kiracısı, * \<alan adı>.onmicrosoft.com*adı ile birlikte gelir. İlk etki alanı adını değiştiremezsiniz veya silemezsiniz, ancak kuruluşunuzun adlarını ekleyebilirsiniz. Özel alan adları eklemek, kullanıcılarınıza tanıdık olan *alain\@contoso.com*gibi kullanıcı adları oluşturmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

Özel bir etki alanı adı eklemeden önce, etki alanı kayıt şirketiyle etki alanı adınızı oluşturun. Akredite bir etki alanı kayıt şirketi için [Bkz. ICANN-Akredite Kayıt Şirketleri.](https://www.icann.org/registrar-reports/accredited-list.html)

## <a name="create-your-directory-in-azure-ad"></a>Azure AD'de dizininizi oluşturun

Etki alanı adınızı aldıktan sonra, ilk Azure AD dizininizi oluşturabilirsiniz. Aboneliğin **Sahibi** rolüne sahip bir hesap kullanarak dizininizde Azure portalında oturum açın.

[Kuruluşunuz için yeni bir kiracı oluştur'daki](active-directory-access-create-new-tenant.md#create-a-new-tenant-for-your-organization)adımları izleyerek yeni dizini oluşturun.

>[!IMPORTANT]
>Kiracıyı oluşturan kişi otomatik olarak o kiracının Global yöneticisidir. Global yönetici kiracıya ek yöneticiler ekleyebilir.

Abonelik rolleri hakkında daha fazla bilgi için [Azure RBAC rolleri'ne](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-rbac-roles)bakın.

>[!TIP]
> Şirket içi Windows Server AD'nizi Azure AD ile fesatlaştırmayı planlıyorsanız, dizinlerinizi senkronize etmek için Azure AD Connect aracını çalıştırdığınızda **bu etki alanını yerel Active Directory ile tek oturum açma için yapılandırmayı planladığımı** seçmeniz gerekir.
>
> Ayrıca aynı etki alanı adını sihirbazın şirket içi dizininizi **Azure AD Etki Alanı** ile birleştirme adımında da kaydetmeniz gerekir. Kurulumun nasıl göründüğünü görmek [için](../hybrid/how-to-connect-install-custom.md#verify-the-azure-ad-domain-selected-for-federation)bkz. Azure AD Connect aracınız yoksa [buradan indirebilirsiniz.](https://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="add-your-custom-domain-name-to-azure-ad"></a>Azure AD'ye özel alan adınızı ekleyin

Dizininizi oluşturduktan sonra, özel alan adınızı ekleyebilirsiniz.

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

1. Herhangi bir sayfadan *Azure Active Directory'yi* arayın ve seçin. Sonra **Özel etki alanı adlarını** > seçin**Özel etki alanı ekleyin.**

    ![Özel alan adları sayfası, özel etki alanı ekle gösterilir](media/add-custom-domain/add-custom-domain.png)

1. **Özel alan adı,** kuruluşunuzun yeni adını girin, bu örnekte, *contoso.com.* **Etki alanı ekle**'yi seçin.

    ![Özel etki alanı adı sayfası ekle ile özel alan adları sayfası](media/add-custom-domain/add-custom-domain-blade.png)

    >[!IMPORTANT]
    >Düzgün çalışması için *.com*, *.net*veya başka bir üst düzey uzantı eklemeniz gerekir.

    Doğrulanmamış etki alanı eklenir. **contoso.com** sayfası DNS bilgilerinizi gösterir. Bu bilgileri kaydedin. DNS'yi yapılandırmak için bir TXT kaydı oluşturmak için daha sonra gereksinim duymanız gerekir.

    ![DNS giriş bilgilerini içeren Contoso sayfası](media/add-custom-domain/contoso-blade-with-dns-info.png)

## <a name="add-your-dns-information-to-the-domain-registrar"></a>DNS bilgilerinizi etki alanı kayıt şirketine ekleme

Azure AD'ye özel alan adınızı ekledikten sonra, etki alanı kayıt şirketinize dönmeniz ve kopyalanan TXT dosyanızdan Azure AD DNS bilgilerini eklemeniz gerekir. Etki alanınız için bu TXT kaydının oluşturulması, etki alan adınızın sahipliğini doğrular.

Etki alanı kayıt şirketinize geri dön ve kopyalanan DNS bilgilerinize dayalı olarak etki alanınız için yeni bir TXT kaydı oluşturun. Yaşam süresini (TTL) 3600 saniyeye (60 dakika) ayarlayın ve ardından kaydı kaydedin.

>[!IMPORTANT]
>İstediğiniz kadar alan adı kaydedebilirsiniz. Ancak, her etki alanı Azure AD'den kendi TXT kaydını alır. Etki alanı kayıt şirketine TXT dosya bilgilerini girerken dikkatli olun. Yanlış veya yinelenen bilgileri yanlışlıkla girerseniz, yeniden denemeden önce TTL saatlerini (60 dakika) beklemeniz gerekir.

## <a name="verify-your-custom-domain-name"></a>Özel alan adınızı doğrulama

Özel alan adınızı kaydettikten sonra Azure AD'de geçerli olduğundan emin olun. Etki alanı kayıt şirketinizden Azure AD'ye yayılma anında olabilir veya etki alanı kayıt şirketinize bağlı olarak birkaç gün sürebilir.

Özel alan adınızı doğrulamak için aşağıdaki adımları izleyin:

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com/) oturum açın.

1. Herhangi bir sayfadan *Azure Active Directory'yi* arayın ve seçin, ardından **Özel alan adlarını**seçin.

1. **Özel alan adlarında,** özel alan adını seçin. Bu örnekte, **contoso.com'** yi seçin.

    ![Fabrikam - Özel alan adları sayfası, contoso vurgulanmış](media/add-custom-domain/custom-blade-with-contoso-highlighted.png)

1. **contoso.com** sayfasında, özel etki alanınızın düzgün kaydolduğundan ve Azure AD için geçerli olduğundan emin olmak için **Doğrula'yı** seçin.

    ![DNS giriş bilgilerini içeren Contoso sayfası ve Doğrula düğmesi](media/add-custom-domain/contoso-blade-with-dns-info-verify.png)

Özel alan adınızı doğruladıktan sonra doğrulama TXT veya MX dosyanızı silebilirsiniz.

## <a name="common-verification-issues"></a>Sık kullanılan doğrulama sorunları

Azure AD özel bir etki alanı adını doğrulayamıyorsa, aşağıdaki önerileri deneyin:

- **En az bir saat bekleyin ve tekrar deneyin.** Azure AD’nin etki alanını doğrulayabilmesi için DNS kayıtlarının yayılması gerekir. Bu işlem bir saat veya daha fazla sürebilir.

- **DNS kaydının doğru olduğundan emin olun.** Etki alanı adı kayıt sitesine geri dön. Girişin orada olduğundan ve Azure AD tarafından sağlanan DNS giriş bilgileriyle eşleştiğinden emin olun.

  Kayıt şirketi sitesindeki kaydı güncelleştiremezseniz, girişi ekleme ve doğru luğunu doğrulama izni olan biriyle paylaşın.

- **Etki alanı adının başka bir dizinde zaten kullanılmadığından emin olun.** Etki alanı adı yalnızca tek bir dizinde doğrulanabilir. Etki alanı adınız şu anda başka bir dizinde doğrulanmışsa, yeni dizinde de doğrulanamamaktadır. Bu yineleme sorununu gidermek için etki alanı adını eski dizinden silmeniz gerekir. Etki alanı adlarını silme hakkında daha fazla bilgi için [bkz.](../users-groups-roles/domains-manage.md)

- **Yönetilmeyen Power BI kiracınız olmadığından emin olun.** Kullanıcılarınız Self servis kayıt yoluyla Power BI'yi etkinleştirdiyse ve kuruluşunuz için yönetilmeyen bir kiracı oluşturduysa, PowerShell'i kullanarak yönetimi dahili veya harici yönetici olarak devralmanız gerekir. Daha fazla bilgi için bkz. [Azure Active Directory’de yönetilmeyen bir dizini yönetici olarak devralma](../users-groups-roles/domains-admin-takeover.md).

## <a name="next-steps"></a>Sonraki adımlar

- Dizininize başka bir Global yöneticisi ekleyin. Daha fazla bilgi [için, rolleri ve yöneticileri nasıl ataysüreceğiniz](active-directory-users-assign-role-azure-portal.md)bölümüne bakın.

- Etki alanınıza kullanıcı ekleyin. Daha fazla bilgi [için, kullanıcıların nasıl ekleyeceğiniz veya silinene](add-users-azure-active-directory.md)bakın.

- Azure AD'de alan adı bilgilerinizi yönetin. Daha fazla bilgi için [bkz.](../users-groups-roles/domains-manage.md)

- Windows Server'ın Azure Etkin Dizin'inyanında kullanmak istediğiniz şirket içi sürümleri varsa, [bkz.](../connect/active-directory-aadconnect.md)
