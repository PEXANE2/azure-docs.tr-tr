---
title: Azure AD Etki Alanı Hizmetleri yle Azure Monitör Çalışma Kitaplarını Kullanma | Microsoft Dokümanlar
description: Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanında güvenlik denetimlerini gözden geçirmek ve sorunları anlamak için Azure Monitor Çalışma Kitaplarını nasıl kullanacağınızı öğrenin.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/18/2020
ms.author: iainfou
ms.openlocfilehash: bdfc7d37d99dc5511f47e33d1848c3f142a9693e
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654463"
---
# <a name="review-security-audit-events-in-azure-ad-domain-services-using-azure-monitor-workbooks"></a>Azure Monitör Çalışma Kitaplarını kullanarak Azure AD Etki Alanı Hizmetlerinde güvenlik denetimi olaylarını gözden geçirme

Azure Active Directory Etki Alanı Hizmetlerinizin (Azure AD DS) yönetilen etki alanınızın durumunu anlamanıza yardımcı olmak için güvenlik denetim etkinliklerini etkinleştirebilirsiniz. Bu güvenlik denetimi etkinlikleri, metin, analitik sorguları ve parametreleri zengin etkileşimli raporlarda birleştiren Azure Monitor Çalışma Kitapları kullanılarak gözden geçirilebilir. Azure AD DS, denetim etkinliklerini araştırmanıza ve ortamınızı yönetmenize izin veren güvenlik genel bakışı ve hesap etkinliği için çalışma kitabı şablonları içerir.

Bu makalede, Azure AD DS'deki güvenlik denetimi olaylarını incelemek için Azure Monitor Çalışma Kitaplarını nasıl kullanacağınızı gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, bir Azure [Active Directory Etki Alanı Hizmetleri örneği oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi tamamlayın.
* Azure Active Directory Etki Alanı Hizmetleri için etkinleştirilen güvenlik denetim olayları, verileri bir Log Analytics çalışma alanına aktaran etki alanını yönetti.
    * Gerekirse, [Azure Etkin Dizin Etki Alanı Hizmetleri için güvenlik denetimlerini etkinleştirin.][enable-security-audits]

## <a name="azure-monitor-workbooks-overview"></a>Azure Monitör Çalışma Kitaplarına genel bakış

Azure AD DS'de güvenlik denetimi olayları açıklandığında, yönetilen etki alanındaki sorunları çözümlemek ve tanımlamak zor olabilir. Azure Monitor, bu güvenlik denetim olaylarını toplamanıza ve verileri sorgulamanıza olanak tanır. Azure Monitör Çalışma Kitapları ile sorunları daha hızlı ve daha kolay tanımlamak için bu verileri görselleştirebilirsiniz.

Çalışma kitabı şablonları, birden çok kullanıcı ve ekip tarafından esnek yeniden kullanım için tasarlanmış hazır raporlardır. Bir çalışma kitabı şablonu açtığınızda, Azure Monitor ortamınızdaki veriler yüklenir. Şablonları kuruluşunuzdaki diğer kullanıcılar üzerinde herhangi bir etkisi olmadan kullanabilir ve şablona göre kendi çalışma kitaplarınızı kaydedebilirsiniz.

Azure AD DS aşağıdaki iki çalışma kitabı şablonu içerir:

* Güvenliğe genel bakış raporu
* Hesap faaliyet raporu

Çalışma kitaplarını nasıl düzenlip yönetici göreceğiz hakkında daha fazla bilgi için [Azure Monitor Çalışma Kitaplarına genel bakış](../azure-monitor/platform/workbooks-overview.md)bilgisine bakın.

## <a name="use-the-security-overview-report-workbook"></a>Güvenlik raporuna genel bakış çalışma kitabını kullanma

Kullanımı daha iyi anlamanıza ve olası güvenlik tehditlerini belirlemenize yardımcı olmak için, güvenlik genel bakış raporu oturum açma verilerini özetler ve kontrol etmek isteyebileceğiniz hesapları tanımlar. Belirli bir tarih aralığındaki olayları görüntüleyebilir ve hatalı parola denemeleri veya hesabın devre dışı bırakıldığı durumlar gibi belirli oturum açma olaylarını inceleyebilirsiniz.

Güvenlik genel bakış raporu için çalışma kitabı şablonuna erişmek için aşağıdaki adımları tamamlayın:

1. Azure portalında **Azure Active Directory Etki Alanı Hizmetlerini** arayın ve seçin.
1. yönetilen etki alanınızı seçin, örneğin *aaddscontoso.com*
1. Sol taraftaki menüden **İzleme > Çalışma Kitapları'nı** seçin

    ![Azure portalında Çalışma Kitapları menüsü seçeneğini seçin](./media/use-azure-monitor-workbooks/select-workbooks-in-azure-portal.png)

1. **Güvenliğe Genel Bakış Raporunu**seçin.
1. Çalışma kitabının üst kısmındaki açılır menülerden Azure aboneliğinizi ve ardından Azure Monitor çalışma alanını seçin. *Son 7 gün*gibi bir Zaman **aralığı**seçin.

    ![Azure portalında Çalışma Kitapları menüsü seçeneğini seçin](./media/use-azure-monitor-workbooks/select-query-filters.png)

    **Döşeme görünümü** ve **Grafik görünümü** seçenekleri de analiz etmek ve verileri istenilen şekilde görselleştirmek için değiştirilebilir

1. Belirli bir olay türünü ayrıntıya sokmak için, aşağıdaki örnekte gösterildiği *gibi, Hesap Kilitlendi*Gibi **Oturum Açma sonuç** kartlarından birini seçin:

    ![Örnek Güvenlik Genel Bakış Raporu Verileri Azure Monitörü Çalışma Kitaplarında görselleştirildi](./media/use-azure-monitor-workbooks/example-security-overview-report.png)

1. Grafiğin altındaki güvenlik genel bakış raporunun alt bölümü, seçili etkinlik türünü ayırır. Aşağıdaki örnek raporda gösterildiği gibi, sağ tarafta yer alan kullanıcı adlarını filtreleyebilirsiniz:

    [![](./media/use-azure-monitor-workbooks/account-lockout-details-cropped.png "Details of account lockouts in Azure Monitor Workbooks")](./media/use-azure-monitor-workbooks/account-lockout-details.png#lightbox)

## <a name="use-the-account-activity-report-workbook"></a>Hesap faaliyet raporu çalışma kitabını kullanma

Belirli bir kullanıcı hesabıyla ilgili sorunları gidermenize yardımcı olmak için, hesap faaliyet raporu ayrıntılı denetim olay günlüğü bilgilerini ayırır. Oturum açma sırasında kötü bir kullanıcı adı veya parola nın ne zaman sağlandığını ve oturum açma girişiminin kaynağını inceleyebilirsiniz.

Hesap etkinlik raporu için çalışma kitabı şablonuna erişmek için aşağıdaki adımları tamamlayın:

1. Azure portalında **Azure Active Directory Etki Alanı Hizmetlerini** arayın ve seçin.
1. yönetilen etki alanınızı seçin, örneğin *aaddscontoso.com*
1. Sol taraftaki menüden **İzleme > Çalışma Kitapları'nı** seçin
1. Hesap **Faaliyet Raporu'nu**seçin.
1. Çalışma kitabının üst kısmındaki açılır menülerden Azure aboneliğinizi ve ardından Azure Monitor çalışma alanını seçin. *Son 30 gün*gibi bir **Zaman aralığı**nı ve döşeme **görünümünün** verileri nasıl temsil etmesini istediğinizi seçin. Aşağıdaki örnek raporda gösterildiği *gibi, Felix*gibi **Hesap kullanıcı adına**göre filtreuygulayabilirsiniz:

    [![](./media/use-azure-monitor-workbooks/account-activity-report-cropped.png "Account activity report in Azure Monitor Workbooks")](./media/use-azure-monitor-workbooks/account-activity-report.png#lightbox)

    Grafiğin altındaki alan, etkinlik sonucu ve kaynak iş istasyonu gibi bilgilerle birlikte tek tek oturum açma olaylarını gösterir. Bu bilgiler, hesap kilitlemelerine neden olabilecek veya olası bir saldırıyı gösterebilecek tekrarlanan oturum açma olaylarının kaynaklarını belirlemeye yardımcı olabilir.

Güvenlik genel bakış raporunda olduğu gibi, verileri gerektiği gibi görselleştirmek ve çözümlemek için raporun üst kısmındaki farklı kutucukları inceleyebilirsiniz.

## <a name="save-and-edit-workbooks"></a>Çalışma kitaplarını kaydetme ve bu kitapları edin

Azure AD DS tarafından sağlanan iki şablon çalışma kitabı, kendi veri çözümlemenizi başlatmak için iyi bir yerdir. Veri sorgularında ve sorgularında daha ayrıntılı bilgiler almanız gerekiyorsa, kendi çalışma kitaplarınızı kaydedebilir ve sorguları edebilirsiniz.

1. Çalışma kitabı şablonlarından birinin kopyasını kaydetmek için, **Paylaşılan raporlar > > Kaydet'i > edit'i**seçin, ardından bir ad sağlayın ve kaydedin.
1. Şablonun kendi kopyasından, edit moduna girmek için **Edit'i** seçin. Raporun herhangi bir bölümünün yanındaki mavi **Edit** düğmesini seçebilir ve değiştirebilirsiniz.

Azure Monitör Çalışma Kitaplarındaki tüm grafikler ve tablolar Kusto sorguları kullanılarak oluşturulur. Kendi sorgularınızı oluşturma hakkında daha fazla bilgi için [Azure Monitor günlük sorguları][azure-monitor-queries] ve [Kusto sorguları öğreticisine][kusto-queries]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Parola ve kilitleme ilkelerini ayarlamanız gerekiyorsa, [yönetilen etki alanlarında parola ve hesap kilitleme ilkelerine][password-policy]bakın.

Kullanıcılarla ilgili sorunlar için, [hesap oturum açma sorunlarını][troubleshoot-sign-in] veya [hesap kilitleme sorunlarını][troubleshoot-account-lockout]nasıl gidereceklerini öğrenin.

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
