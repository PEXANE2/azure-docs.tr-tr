---
title: Azure AD Etki Alanı Hizmetleri için güvenlik denetimlerini etkinleştirme | Microsoft Dokümanlar
description: Azure AD Etki Alanı Hizmetleri'nde analiz ve uyarılar için olayların günlüğe kaydetmeyi merkezileştirmek için güvenlik denetimlerini nasıl etkinleştirmenizi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 662362c3-1a5e-4e94-ae09-8e4254443697
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: iainfou
ms.openlocfilehash: b2138818a9092999dd54b14664f7146f087c4fed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78328655"
---
# <a name="enable-security-audits-for-azure-active-directory-domain-services"></a>Azure Etkin Dizin Etki Alanı Hizmetleri için güvenlik denetimlerini etkinleştirme

Azure Active Directory Domain Services (Azure AD DS) güvenlik denetimleri, Azure güvenlik olaylarını hedeflenen kaynaklara aktarıyor. Bu kaynaklar arasında Azure Depolama, Azure Günlük Analizi çalışma alanları veya Azure Etkinlik Hub'ı yer almaktadır. Güvenlik denetimi olaylarını etkinleştirdikten sonra, Azure AD DS seçili kategori için denetlenen tüm olayları hedeflenen kaynağa gönderir.

Etkinlikleri Azure depolama alanına arşivleyebilir ve etkinlikleri Azure Etkinlik Hub'larını kullanarak güvenlik bilgileri ve etkinlik yönetimi (SIEM) yazılımına (veya eşdeğeri) aktarabilir veya azure portalından kendi analizlerinizi ve Azure Log Analytics çalışma alanlarını kullanarak yapabilirsiniz.

> [!IMPORTANT]
> Azure AD DS güvenlik denetimleri yalnızca Azure Kaynak Yöneticisi tabanlı örnekler için kullanılabilir. Geçiş şekli hakkında bilgi için, [Klasik sanal ağ modelinden Kaynak Yöneticisi'ne Azure AD DS'yi geçir'e][migrate-azure-adds]bakın.

## <a name="audit-event-categories"></a>Denetim olay kategorileri

Azure AD DS güvenlik denetimleri, geleneksel AD DS etki alanı denetleyicileri için geleneksel denetimle uyumludur. Karma ortamlarda, olayları analiz ederken aynı mantığın kullanılabileceğini, böylece varolan denetim modellerini yeniden kullanabilirsiniz. Sorun gidermeniz veya çözümlemeniz gereken senaryoya bağlı olarak, farklı denetim olay kategorilerini hedeflemeniz gerekir.

Aşağıdaki denetim olay kategorileri mevcuttur:

| Denetim Kategori Adı | Açıklama |
|:---|:---|
| Hesap Girişi|Denetimler, bir etki alanı denetleyicisi veya yerel bir Güvenlik Hesapları Yöneticisi (SAM) üzerinde hesap verilerini doğrulamaya çalışır.</p>Oturum açma ve Oturum kapatma ilkesi ayarları ve etkinlikleri belirli bir bilgisayara erişme girişimlerini izler. Bu kategorideki ayarlar ve olaylar, kullanılan hesap veritabanına odaklanır. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[Kimlik Bilgisi Doğrulamayı Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-credential-validation)</li><li>[Kerberos Kimlik Doğrulaması Hizmetini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-authentication-service)</li><li>[Kerberos Hizmet Bileti İşlemlerini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-service-ticket-operations)</li><li>[Diğer Oturum Açma/Giriş Etkinliklerini Denetle](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li></ul>|
| Hesap Yönetimi|Denetimler kullanıcı ve bilgisayar hesapları ve grupları değişiklikleri. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[Uygulama Grubu Yönetimini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-group-management)</li><li>[Bilgisayar Hesabı Yönetimini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-computer-account-management)</li><li>[Dağıtım Grubu Yönetimini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-distribution-group-management)</li><li>[Diğer Hesap Yönetimini Denetle](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-account-management-events)</li><li>[Güvenlik Grubu Yönetimini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-group-management)</li><li>[Kullanıcı Hesabı Yönetimini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-account-management)</li></ul>|
| Detay Takibi|O bilgisayardaki tek tek uygulamaların ve kullanıcıların faaliyetlerini denetler ve bilgisayarın nasıl kullanıldığını anlar. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[DPAPI Etkinliğini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-dpapi-activity)</li><li>[Denetim PNP etkinliği](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-pnp-activity)</li><li>[İşlem Oluşturmayı Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-creation)</li><li>[İşlem Sonlandırmayı Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-termination)</li><li>[RPC Olaylarını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-rpc-events)</li></ul>|
| Dizin Hizmetleri Erişimi|Denetimler, Active Directory Etki Alanı Hizmetleri'ndeki (AD DS) nesnelere erişmeye ve nesneleri değiştirmeye çalışır. Bu denetim olayları yalnızca etki alanı denetleyicilerinde günlüğe kaydedilir. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[Ayrıntılı Dizin Hizmeti Çoğaltmayı Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-directory-service-replication)</li><li>[Dizin Hizmeti Erişimini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-access)</li><li>[Dizin Hizmeti Değişikliklerini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-changes)</li><li>[Dizin Hizmeti Çoğaltmasını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-replication)</li></ul>|
| Oturum Açma-Giriş|Denetimler, bir bilgisayarda etkileşimli olarak veya ağ üzerinden oturum açmaya çalışır. Bu olaylar, kullanıcı etkinliğini izlemek ve ağ kaynaklarındaki olası saldırıları tanımlamak için yararlıdır. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[Hesap Kilitlemeyi Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-account-lockout)</li><li>[Kullanıcı/Cihaz Taleplerini Denetle](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-device-claims)</li><li>[IPsec Genişletilmiş Modunu Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-extended-mode)</li><li>[Denetim Grubu Üyeliği](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-group-membership)</li><li>[IPsec Ana Modunu Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-main-mode)</li><li>[IPsec Hızlı Modunu Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-quick-mode)</li><li>[Oturum Kapatmayı Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logoff)</li><li>[Oturum Açmayı Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logon)</li><li>[Ağ İlkesi Sunucusunu Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-network-policy-server)</li><li>[Diğer Oturum Açma/Giriş Etkinliklerini Denetle](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li><li>[Özel Oturum Açmayı Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-special-logon)</li></ul>|
|Nesne Erişimi| Denetimler, ağ veya bilgisayardaki belirli nesnelere veya nesne türlerine erişmeye çalışır. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[Oluşturulan Uygulamayı Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-generated)</li><li>[Sertifika Hizmetlerini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-certification-services)</li><li>[Ayrıntılı Dosya Paylaşımını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-file-share)</li><li>[Dosya Paylaşımını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-share)</li><li>[Denetim Dosya Sistemi](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-system)</li><li>[Platform Bağlantısı Filtrelemeyi Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection)</li><li>[Platform Paketi Bırakma Filtrelemesini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-packet-drop)</li><li>[Denetim Kolu Manipülasyonu](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-handle-manipulation)</li><li>[Denetim Çekirdeği Nesnesi](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kernel-object)</li><li>[Diğer Nesne Erişim Olaylarını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-object-access-events)</li><li>[Denetim Kayıt Defteri](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-registry)</li><li>[Denetim Çıkarılabilir Depolama](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-removable-storage)</li><li>[SAM Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sam)</li><li>[Denetim Merkezi Erişim Politikası Evreleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-central-access-policy-staging)</li></ul>|
|İlke Değişikliği|Denetimler, yerel bir sistem veya ağdaki önemli güvenlik ilkelerinde değişiklik gösterir. İlkeler genellikle ağ kaynaklarının güvenli yardımcı olmak için yöneticiler tarafından belirlenir. Değişiklikleri veya bu ilkeleri değiştirme girişimlerini izlemek, bir ağ için güvenlik yönetiminin önemli bir yönü olabilir. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[İlke Değişimini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-audit-policy-change)</li><li>[Kimlik Doğrulama İlkesi Değişikliğini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authentication-policy-change)</li><li>[Yetkilendirme İlkesi Değişikliğini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authorization-policy-change)</li><li>[Filtre Platformu İlke Değişikliğini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-policy-change)</li><li>[MPSSVC Kural Düzeyi İlke Değişikliğini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-mpssvc-rule-level-policy-change)</li><li>[Diğer Politika Değişikliğini Denetle](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-policy-change-events)</li></ul>|
|Ayrıcalık Kullanımı| Belirli izinlerin bir veya daha fazla sistemde kullanımını denetler. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[Hassas Olmayan Ayrıcalık Kullanımını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-non-sensitive-privilege-use)</li><li>[Hassas Ayrıcalık Kullanımını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sensitive-privilege-use)</li><li>[Diğer Ayrıcalık Kullanım Olaylarını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-privilege-use-events)</li></ul>|
|Sistem| Diğer kategorilerde yer almayan ve olası güvenlik etkileri olan bir bilgisayarda sistem düzeyindeki değişiklikleri denetler. Bu kategori aşağıdaki alt kategorileri içerir:<ul><li>[IPsec Sürücüsünü Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-driver)</li><li>[Diğer Sistem Olaylarını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-system-events)</li><li>[Güvenlik Durumu Değişikliğini Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-state-change)</li><li>[Güvenlik Sistemi Uzantısını Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-system-extension)</li><li>[Sistem Bütünlüğünü Denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-system-integrity)</li></ul>|

## <a name="event-ids-per-category"></a>Kategori başına olay lı iLikler

 Azure AD DS güvenlik denetimleri, belirli bir eylem denetlenebilir bir olayı tetiklediğinde aşağıdaki olay adlarını kaydeder:

| Etkinlik Kategori Adı | Olay II'leri |
|:---|:---|
|Hesap Oturum Açma güvenliği|4767, 4774, 4775, 4776, 4777|
|Hesap Yönetimi güvenliği|4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4730, 4731, 4732, 4733, 4734, 4735, 4737, 4738, 4740, 4741, 4742, 4743, 4754, 4755, 4756, 4757, 4758, 4764, 4765, 4766, 4780, 4781, 4782, 4793, 4798, 4799, 5376, 5377|
|Detay İzleme güvenliği|None|
|DS Access güvenliği|5136, 5137, 5138, 5139, 5141|
|Oturum Açma-Giriş güvenliği|4624, 4625, 4634, 4647, 4648, 4672, 4675, 4964|
|Nesne Erişimi güvenliği|None|
|İlke Değişikliği güvenliği|4670, 4703, 4704, 4705, 4706, 4707, 4713, 4715, 4716, 4717, 4718, 4719, 4739, 4864, 4865, 4866, 4867, 4904, 4906, 4911, 4912|
|Ayrıcalık Kullanım güvenliği|4985|
|Sistem güvenliği|4612, 4621|

## <a name="security-audit-destinations"></a>Güvenlik denetimi hedefleri

Azure Depolama, Azure Etkinlik Hub'ları veya Azure Günlük Analizi çalışma alanlarını Azure AD DS güvenlik denetimleri için hedef kaynak olarak kullanabilirsiniz. Bu hedefler birleştirilebilir. Örneğin, güvenlik denetim olaylarını arşivlemek için Azure Depolama'yı, ancak kısa vadede bilgileri analiz etmek ve raporlamak için bir Azure Log Analytics çalışma alanı kullanabilirsiniz.

Aşağıdaki tabloda her hedef kaynak türü için senaryolar özetler.

> [!IMPORTANT]
> Azure AD DS güvenlik denetimlerini etkinleştirmeden önce hedef kaynağı oluşturmanız gerekir. Bu kaynakları Azure portalını, Azure PowerShell'i veya Azure CLI'yi kullanarak oluşturabilirsiniz.

| Hedef Kaynak | Senaryo |
|:---|:---|
|Azure Storage| Birincil gereksiniminiz güvenlik denetim olaylarını arşivleme amacıyla depolamak olduğunda bu hedef kullanılmalıdır. Diğer hedefler arşivleme amacıyla kullanılabilir, ancak bu hedefler birincil arşivleme ihtiyacının ötesinde yetenekler sağlar. <br /><br />Azure AD DS güvenlik denetim olaylarını etkinleştirmeden önce [önce bir Azure Depolama hesabı oluşturun.](../storage/common/storage-account-create.md)|
|Azure Event Hubs| Bu hedef, birincil gereksiniminiz güvenlik denetim olaylarını veri analizi yazılımı veya güvenlik bilgileri & olay yönetimi (SIEM) yazılımı gibi ek yazılımlarla paylaşmak olduğunda kullanılmalıdır.<br /><br />Azure AD DS güvenlik denetim etkinliklerini etkinleştirmeden önce Azure [portalLarını kullanarak bir etkinlik merkezi oluşturun](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)|
|Azure Günlük Analizi Çalışma Alanı| Bu hedef, birincil gereksiniminiz Azure portalından güvenli denetimleri doğrudan analiz etmek ve gözden geçirmek olduğunda kullanılmalıdır.<br /><br />Azure AD DS güvenlik denetim etkinliklerini etkinleştirmeden önce, [Azure portalında Bir Günlük Analizi çalışma alanı oluşturun.](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)|

## <a name="enable-security-audit-events-using-the-azure-portal"></a>Azure portalını kullanarak güvenlik denetimi olaylarını etkinleştirme

Azure portalını kullanarak Azure AD DS güvenlik denetim olaylarını etkinleştirmek için aşağıdaki adımları tamamlayın.

> [!IMPORTANT]
> Azure AD DS güvenlik denetimleri geriye dönük değildir. Geçmişten olayları geri alamaz veya yeniden oynatamazsınız. Azure AD DS, yalnızca güvenlik denetimleri etkinleştirildikten sonra meydana gelen olayları gönderebilir.

1. https://portal.azure.com adresinden Azure portalında oturum açın.
1. Azure portalının üst kısmında, Azure **AD Etki Alanı Hizmetlerini**arayın ve seçin. yönetilen etki alanınızı seçin( *örneğin aaddscontoso.com.*
1. Azure AD DS penceresinde, sol taraftaki **Tanılama ayarlarını** seçin.
1. Hiçbir tanılama varsayılan olarak yapılandırılamaz. Başlamak için **tanı ayarını ekle'yi**seçin.

    ![Azure AD Etki Alanı Hizmetleri için tanılama ayarı ekleme](./media/security-audit-events/add-diagnostic-settings.png)

1. *Aadds-auditing*gibi tanılama yapılandırması için bir ad girin.

    İstediğiniz güvenlik denetim hedefi için kutuyu işaretleyin. Bir Azure Depolama hesabı, Azure etkinlik merkezi veya Günlük Analizi çalışma alanı arasından seçim yapabilirsiniz. Bu hedef kaynaklar Azure aboneliğinizde zaten mevcut olmalıdır. Bu sihirbazda hedef kaynakları oluşturamazsınız.

    ![Yakalamak için gerekli hedef ve denetim olayları türünü etkinleştirme](./media/security-audit-events/diagnostic-settings-page.png)

    * **Azure depolama**
        * **Depolama hesabına Arşiv'i**seçin ve ardından **Yapıl'ı**seçin.
        * Güvenlik denetim olaylarını arşivlemek için kullanmak istediğiniz **Abonelik** ve **Depolama hesabını** seçin.
        * Hazır olduğunda **Tamam'ı**seçin.
    * **Azure etkinlik merkezleri**
        * **Bir olay hub'ına Akış'ı**seçin ve ardından **Yapıl'ı**seçin.
        * **Abonelik** ve **Olay hub ad alanını**seçin. Gerekirse, ayrıca bir **Olay hub adı** ve sonra Olay hub **ilke adı**seçin.
        * Hazır olduğunda **Tamam'ı**seçin.
    * **Azure Günlük Analitik çalışma alanları**
        * **Günlük Analitiğine Gönder'i**seçin, ardından güvenlik denetimi olaylarını depolamak için kullanmak istediğiniz **Abonelik** ve Günlük Analizi **Çalışma Alanını** seçin.

1. Belirli hedef kaynak için dahil olmasını istediğiniz günlük kategorilerini seçin. Denetim olaylarını bir Azure Depolama hesabına gönderirseniz, verileri saklamak için gün sayısını tanımlayan bir bekletme ilkesi de yapılandırabilirsiniz. *Varsayılan 0* ayarı tüm verileri korur ve bir süre sonra olayları döndürmez.

    Tek bir yapılandırma içinde hedeflenen her kaynak için farklı günlük kategorileri seçebilirsiniz. Bu yetenek, Log Analytics için hangi günlük kategorilerini tutmak istediğinizi ve örneğin arşivlemek istediğiniz kategorileri seçmenize olanak tanır.

1. Bittiğinde, değişikliklerinizi işlemek için **Kaydet'i** seçin. Hedef kaynaklar, yapılandırma kaydedildikten kısa bir süre sonra Azure AD DS güvenlik denetim olaylarını almaya başlar.

## <a name="enable-security-audit-events-using-azure-powershell"></a>Azure PowerShell'i kullanarak güvenlik denetimi olaylarını etkinleştirme

Azure PowerShell'i kullanarak Azure AD DS güvenlik denetim olaylarını etkinleştirmek için aşağıdaki adımları tamamlayın. Gerekirse önce [Azure PowerShell modülünü yükleyin ve Azure aboneliğinize bağlanın.](/powershell/azure/install-az-ps)

> [!IMPORTANT]
> Azure AD DS güvenlik denetimleri geriye dönük değildir. Geçmişten olayları geri alamaz veya yeniden oynatamazsınız. Azure AD DS, yalnızca güvenlik denetimleri etkinleştirildikten sonra meydana gelen olayları gönderebilir.

1. [Connect-AzAccount](/powershell/module/Az.Accounts/Connect-AzAccount) cmdlet'i kullanarak Azure aboneliğinizde kimlik doğrulaması verin. İstendiğinde, hesap kimlik bilgilerinizi girin.

    ```azurepowershell
    Connect-AzAccount
    ```

1. Güvenlik denetim olayları için hedef kaynak oluşturun.

    * **Azure depolama** - [Azure PowerShell'i kullanarak bir depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-powershell)
    * **Azure etkinlik hub'ları** - [Azure PowerShell'i kullanarak bir etkinlik merkezi oluşturun.](../event-hubs/event-hubs-quickstart-powershell.md) Ayrıca, etkinlik merkezi *ad alanına*Azure AD DS izinleri veren bir yetkilendirme kuralı oluşturmak için [New-AzEventHubAuthorizationRule](/powershell/module/az.eventhub/new-azeventhubauthorizationrule) cmdlet'i kullanmanız gerekebilir. Yetkilendirme kuralı **Yönet,** **Dinle**ve **Gönder** haklarını içermelidir.

        > [!IMPORTANT]
        > Etkinlik merkezinin kendisinde değil, olay göbeği ad alanında yetkilendirme kuralını ayarladığınızdan emin olun.

    * **Azure Günlük Analizi çalışma alanları** - [Azure PowerShell ile Bir Günlük Analizi çalışma alanı oluşturun.](../azure-monitor/learn/quick-create-workspace-posh.md)

1. [Get-AzResource](/powershell/module/Az.Resources/Get-AzResource) cmdlet'ini kullanarak Azure AD DS yönetilen etki alanınız için kaynak kimliğini alın. $aadds adlı bir değişken *oluşturun. Değeri* tutmak için ResourceId:

    ```azurepowershell
    $aadds = Get-AzResource -name aaddsDomainName
    ```

1. Azure AD Etki Alanı Hizmetleri güvenlik denetim etkinlikleri için hedef kaynağı kullanmak üzere [Set-AzDiagnosticSetting](/powershell/module/Az.Monitor/Set-AzDiagnosticSetting) cmdlet'i kullanarak Azure Tanılama ayarlarını yapılandırın. Aşağıdaki örneklerde, değişken *$aadds. ResourceId* önceki adımdan kullanılır.

    * **Azure depolama** - *StorageAccountId'i* depolama hesabı adınız ile değiştirin:

        ```powershell
        Set-AzDiagnosticSetting `
            -ResourceId $aadds.ResourceId `
            -StorageAccountId storageAccountId `
            -Enabled $true
        ```

    * **Azure etkinlik hub'ları** - *eventHubName'i* etkinlik hub'ınızın adıyla ve *eventHubRuleId'i* yetkilendirme kuralı kimliğinizle değiştirin:

        ```powershell
        Set-AzDiagnosticSetting -ResourceId $aadds.ResourceId `
            -EventHubName eventHubName `
            -EventHubAuthorizationRuleId eventHubRuleId `
            -Enabled $true
        ```

    * **Azure Log Analytic çalışma alanları** - Çalışma *alanını* Log Analytics çalışma alanının kimliğiyle değiştirin:

        ```powershell
        Set-AzureRmDiagnosticSetting -ResourceId $aadds.ResourceId `
            -WorkspaceID workspaceId `
            -Enabled $true
        ```

## <a name="query-and-view-security-audit-events-using-azure-monitor"></a>Azure Monitor'u kullanarak güvenlik denetim olaylarını sorgula ve görüntüleme

Günlük Analitik çalışma alanları, Azure Monitor ve Kusto sorgu dilini kullanarak güvenlik denetim olaylarını görüntülemenize ve çözümlemenize izin tanır. Bu sorgu dili, okunması kolay bir sözdizimi yle güç analitik özelliklerine sahip yalnızca okunur kullanım için tasarlanmıştır. Kusto sorgu dilleriyle başlamak için daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure İzleyici belgeleri](https://docs.microsoft.com/azure/azure-monitor/)
* [Azure Monitör'de Günlük Analitiği ile başlayın](../azure-monitor/log-query/get-started-portal.md)
* [Azure Monitor'da günlük sorgularına başlayın](../azure-monitor/log-query/get-started-queries.md)
* [Log Analytics verilerinin panolarını oluşturma ve paylaşma](../azure-monitor/learn/tutorial-logs-dashboards.md)

Aşağıdaki örnek sorgular, Azure AD DS'den güvenlik denetimi olaylarını çözümlemeye başlamak için kullanılabilir.

### <a name="sample-query-1"></a>Örnek sorgu 1

Son yedi gündeki tüm hesap kilitleme olaylarını görüntüleyin:

```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
```

### <a name="sample-query-2"></a>Örnek sorgu 2

3 Şubat 2020 saat 09:00 ile 09:00 arasındaki tüm hesap kilitleme olaylarını *(4740)* görüntüleyin. ve 10 Şubat 2020 gece yarısı, tarih ve saate göre artan sıralanır:

```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= datetime(2020-02-03 09:00) and TimeGenerated <= datetime(2020-02-10)
| where OperationName has "4740"
| sort by TimeGenerated asc
```

### <a name="sample-query-3"></a>Örnek sorgu 3

Kullanıcı adlı hesap için yedi gün önce (şu andan itibaren) hesap oturum açma olaylarını görüntüleyin:

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-4"></a>Örnek sorgu 4

Kötü bir parola *(0xC0000006a)* kullanarak oturum açmagirişiminde bulunan kullanıcı adlı hesap için yedi gün önce hesap oturum açma olaylarını görüntüleyin:

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc000006a" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-5"></a>Örnek sorgu 5

Hesap kilitlenirken oturum açmaya çalışan kullanıcı adlı hesap için yedi gün önceki hesap oturum açma olaylarını görüntüleyin *(0xC0000234):*

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-6"></a>Örnek sorgu 6

Tüm kilitlenen kullanıcılar için gerçekleşen tüm oturum açma denemeleri için yedi gün önceki hesap oturum açma etkinliklerinin sayısını görüntüleyin:

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| summarize count()
```

## <a name="next-steps"></a>Sonraki adımlar

Kusto hakkında özel bilgiler için aşağıdaki makalelere bakın:

* Kusto sorgu diline [genel bakış.](/azure/kusto/query/)
* [Kusto öğretici](/azure/kusto/query/tutorial) sorgu temelleri hakkında bilgi vermek için.
* Verilerinizi görmenin yeni yollarını öğrenmenize yardımcı olan [örnek sorgular.](/azure/kusto/query/samples)
* Kusto başarı için sorgularınızı optimize etmek için [en iyi uygulamaları.](/azure/kusto/query/best-practices)

<!-- LINKS - Internal -->
[migrate-azure-adds]: migrate-from-classic-vnet.md
