---
title: Azure Izleyici çalışma kitaplarını Azure AD Domain Services ile kullanma | Microsoft Docs
description: Güvenlik denetimlerini gözden geçirmek ve Azure Active Directory Domain Services yönetilen bir etki alanındaki sorunları anlamak için Azure Izleyici çalışma kitaplarını nasıl kullanacağınızı öğrenin.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/18/2020
ms.author: iainfou
ms.openlocfilehash: 99b61bdd4318bf7c77ae53cc9b77e66ebd6c098a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84733407"
---
# <a name="review-security-audit-events-in-azure-active-directory-domain-services-using-azure-monitor-workbooks"></a>Azure Izleyici çalışma kitaplarını kullanarak Azure Active Directory Domain Services güvenlik denetim olaylarını gözden geçirme

Azure Active Directory Domain Services (Azure AD DS) yönetilen etki alanının durumunu anlamanıza yardımcı olmak için güvenlik denetim olaylarını etkinleştirebilirsiniz. Bu güvenlik denetim olayları daha sonra, metin, analiz sorguları ve parametreleri zengin etkileşimli raporlara birleştiren Azure Izleyici çalışma kitaplarını kullanarak incelenebilir. Azure AD DS, denetim olaylarını ve ortamınızı yönetmenizi sağlayan güvenlik genel bakışı ve hesap etkinliğinin çalışma kitabı şablonlarını içerir.

Bu makalede, Azure AD DS 'daki güvenlik denetim olaylarını gözden geçirmek için Azure Izleyici çalışma kitaplarını nasıl kullanabileceğiniz gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlayabilmeniz için aşağıdaki kaynaklar ve ayrıcalıklar gereklidir:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Abonelikle ilişkili bir Azure Active Directory kiracısı, şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
    * Gerekirse, [bir Azure Active Directory kiracı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin][associate-azure-ad-tenant].
* Azure AD kiracınızda etkinleştirilmiş ve yapılandırılmış Azure Active Directory Domain Services yönetilen bir etki alanı.
    * Gerekirse, [Azure Active Directory Domain Services yönetilen bir etki alanı oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi doldurun.
* Log Analytics çalışma alanına veri akışı yapan Azure Active Directory Domain Services yönetilen etki alanınız için etkinleştirilen güvenlik denetim olayları.
    * Gerekirse, [Azure Active Directory Domain Services için güvenlik denetimlerini etkinleştirin][enable-security-audits].

## <a name="azure-monitor-workbooks-overview"></a>Azure Izleyici çalışma kitaplarına genel bakış

Azure AD DS 'de güvenlik denetim olayları açıldığında, yönetilen etki alanındaki sorunları analiz etmek ve belirlemek zor olabilir. Azure Izleyici, bu güvenlik denetim olaylarını toplamanızı ve verileri sorgulamanızı sağlar. Azure Izleyici çalışma kitaplarında, sorunları daha hızlı ve kolay bir şekilde belirlemek için bu verileri görselleştirebilirsiniz.

Çalışma kitabı şablonları, birden çok kullanıcı ve takımlar tarafından esnek yeniden kullanım için tasarlanan, seçkin raporlardır. Bir çalışma kitabı şablonunu açtığınızda, Azure Izleyici ortamınızdaki veriler yüklenir. Kuruluşunuzdaki diğer kullanıcılara bir etkisi olmadan şablonlar kullanabilir ve şablonu temel alarak kendi çalışma kitaplarınızı kaydedebilirsiniz.

Azure AD DS, aşağıdaki iki çalışma kitabı şablonunu içerir:

* Güvenliğe genel bakış raporu
* Hesap etkinliği raporu

Çalışma kitaplarını düzenleme ve yönetme hakkında daha fazla bilgi için bkz. [Azure Izleyici çalışma kitaplarına genel bakış](../azure-monitor/platform/workbooks-overview.md).

## <a name="use-the-security-overview-report-workbook"></a>Güvenlik Genel Bakış raporu çalışma kitabını kullanma

Kullanımı daha iyi anlamanıza ve olası güvenlik tehditlerini belirlemenize yardımcı olmak için, güvenlik genel bakış raporu, oturum açma verilerini özetler ve iade etmek isteyebileceğiniz hesapları tanımlar. Belirli bir tarih aralığındaki olayları görüntüleyebilir ve hatalı parola denemeleri veya hesabın devre dışı bırakıldığı durumlar gibi belirli oturum açma olaylarına gidebilirsiniz.

Güvenlik Genel Bakış raporunun çalışma kitabı şablonuna erişmek için aşağıdaki adımları izleyin:

1. Azure portal **Azure Active Directory Domain Services** arayın ve seçin.
1. Yönetilen etki alanınızı seçin (örneğin, *aaddscontoso.com* )
1. Sol taraftaki menüden **izleme > çalışma kitaplarını** seçin

    ![Azure portal çalışma kitapları menü seçeneğini belirleyin](./media/use-azure-monitor-workbooks/select-workbooks-in-azure-portal.png)

1. **Güvenlik genel bakış raporunu**seçin.
1. Çalışma kitabının en üstündeki aşağı açılan menülerden Azure aboneliğinizi ve ardından Azure Izleyici çalışma alanını seçin. *Son 7 gün*gibi bir **zaman aralığı**seçin.

    ![Azure portal çalışma kitapları menü seçeneğini belirleyin](./media/use-azure-monitor-workbooks/select-query-filters.png)

    **Kutucuk görünümü** ve **grafik görünümü** seçenekleri, verileri istenen şekilde analiz etmek ve görselleştirmek için de değiştirilebilir

1. Belirli bir olay türünün detayına gitmek için, aşağıdaki örnekte gösterildiği gibi, *Hesap kilitli*gibi **oturum açma sonuç** kartlarının birini seçin:

    ![Azure Izleyici çalışma kitaplarında görselleştirilen örnek güvenliğe genel bakış rapor verileri](./media/use-azure-monitor-workbooks/example-security-overview-report.png)

1. Grafiğin altındaki Güvenlik Genel Bakış raporunun alt bölümü, seçilen etkinlik türünü ayırır. Aşağıdaki örnek raporda gösterildiği gibi, sağ tarafta yer alan kullanıcı adlarıyla filtre uygulayabilirsiniz:

    [![](./media/use-azure-monitor-workbooks/account-lockout-details-cropped.png "Details of account lockouts in Azure Monitor Workbooks")](./media/use-azure-monitor-workbooks/account-lockout-details.png#lightbox)

## <a name="use-the-account-activity-report-workbook"></a>Hesap etkinliği rapor çalışma kitabını kullan

Belirli bir kullanıcı hesabı ile ilgili sorunları gidermenize yardımcı olması için hesap etkinliği raporu ayrıntılı denetim olay günlüğü bilgilerini ayırır. Oturum açma sırasında hatalı bir kullanıcı adının veya parolanın ne zaman sağlandığını ve oturum açma girişiminin kaynağını gözden geçirebilirsiniz.

Hesap etkinliği raporunun çalışma kitabı şablonuna erişmek için aşağıdaki adımları izleyin:

1. Azure portal **Azure Active Directory Domain Services** arayın ve seçin.
1. Yönetilen etki alanınızı seçin (örneğin, *aaddscontoso.com* )
1. Sol taraftaki menüden **izleme > çalışma kitaplarını** seçin
1. **Hesap etkinliği raporunu**seçin.
1. Çalışma kitabının en üstündeki aşağı açılan menülerden Azure aboneliğinizi ve ardından Azure Izleyici çalışma alanını seçin. *Son 30 gün*gibi bir **zaman aralığı**seçin, sonra **kutucuk görünümünün** verileri nasıl göstermesini istediğinizi belirleyin. Aşağıdaki örnek raporda gösterildiği gibi, *Felix*gibi **hesap Kullanıcı adına**göre filtreleyebilirsiniz:

    [![](./media/use-azure-monitor-workbooks/account-activity-report-cropped.png "Account activity report in Azure Monitor Workbooks")](./media/use-azure-monitor-workbooks/account-activity-report.png#lightbox)

    Grafiğin altındaki alanda, etkinlik sonucu ve kaynak iş istasyonu gibi bilgilerle birlikte tek tek oturum açma olayları gösterilmektedir. Bu bilgiler, hesap kilitlenmelerini neden olabilecek veya olası bir saldırıyı gösterebilen oturum açma olaylarının yinelenen kaynaklarını belirlemesine yardımcı olabilir.

Güvenlik Genel Bakış raporunda olduğu gibi, verileri gerektiğinde görselleştirmek ve analiz etmek için raporun en üstündeki farklı bölmelerde ayrıntıya gidebilirsiniz.

## <a name="save-and-edit-workbooks"></a>Çalışma kitaplarını kaydetme ve düzenleme

Azure AD DS tarafından sunulan iki şablon çalışma kitabı, kendi veri analiziyle başlamak için iyi bir yerdir. Veri sorguları ve araştırmalar hakkında daha ayrıntılı bilgi almanız gerekiyorsa kendi çalışma kitaplarınızı kaydedebilir ve sorguları düzenleyebilirsiniz.

1. Çalışma kitabı şablonlarından birinin bir kopyasını kaydetmek için **düzenle > > paylaşılan raporlar olarak kaydet**' i seçin, sonra bir ad girip kaydedin.
1. Şablon kopyanızın düzenleme modunu girmek için **Düzenle** ' yi seçin. Raporun herhangi bir bölümünün yanındaki mavi **düzenleme** düğmesini seçebilir ve değiştirebilirsiniz.

Azure Izleyici çalışma kitaplarında bulunan tüm grafikler ve tablolar kusto sorguları kullanılarak oluşturulur. Kendi sorgularınızı oluşturma hakkında daha fazla bilgi için bkz. [Azure izleyici günlük sorguları][azure-monitor-queries] ve [kusto sorguları öğreticisi][kusto-queries].

## <a name="next-steps"></a>Sonraki adımlar

Parola ve kilitleme ilkelerini ayarlamanız gerekiyorsa, bkz. [yönetilen etki alanlarında parola ve hesap kilitleme ilkeleri][password-policy].

Kullanıcılar ile ilgili sorunlar için, [Hesap oturum açma sorunlarını][troubleshoot-sign-in] veya [Hesap kilitleme sorunlarını][troubleshoot-account-lockout]giderme hakkında bilgi edinin.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[enable-security-audits]: security-audit-events.md
[password-policy]: password-policy.md
[troubleshoot-sign-in]: troubleshoot-sign-in.md
[troubleshoot-account-lockout]: troubleshoot-account-lockout.md
[azure-monitor-queries]: ../azure-monitor/log-query/query-language.md
[kusto-queries]: https://docs.microsoft.com/azure/kusto/query/tutorial?pivots=azuredataexplorer
