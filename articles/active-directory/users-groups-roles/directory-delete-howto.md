---
title: Azure AD dizinini silme - Azure Etkin Dizini | Microsoft Dokümanlar
description: Self servis dizinleri de dahil olmak üzere silme için azure REKLAM dizininin nasıl hazırlanacağını açıklar
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: addimitu
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 47a60ed44ddf057ef983f8f76f23fd784bc3efd5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73961825"
---
# <a name="delete-a-directory-in-azure-active-directory"></a>Azure Etkin Dizini'nde bir dizini silme

Azure REKLAM dizini silindiğinde, dizinde bulunan tüm kaynaklar da silinir. Silmeden önce ilişkili kaynaklarını en aza indirerek kuruluşunuzu hazırlayın. Yalnızca bir Azure Etkin Dizin (Azure AD) global yöneticisi, bir Azure REKLAM dizinini portaldan silebilir.

## <a name="prepare-the-directory"></a>Dizini hazırlayın

Azure AD'deki bir dizini birkaç denetimden geçmeden silemezsiniz. Bu denetimler, Azure REKLAM dizinini silmenin, Office 365'te oturum açma veya Azure'daki kaynaklara erişim gibi kullanıcı erişimini olumsuz etkilediği riskini azaltır. Örneğin, bir abonelikle ilişkili dizin istemeden silinirse, kullanıcılar bu abonelik için Azure kaynaklarına erişemez. Şu koşullar denetlenir:

* Dizinde, dizini silecek bir global yönetici dışında hiçbir kullanıcı olamaz. Dizin silinmeden önce diğer tüm kullanıcılar silinmelidir. Kullanıcılar şirket içinde senkronize edilirse, önce eşitlemenin kapatılması ve kullanıcıların Azure portalı veya Azure PowerShell cmdlets'i kullanılarak bulut dizininde silinmesi gerekir.
* Dizinde uygulama bulunamaz. Dizin silinemeden önce tüm uygulamaların kaldırılması gerekir.
* Dizine bağlı çok faktörlü kimlik doğrulama sağlayıcıları olamaz.
* Microsoft Çevrimiçi Hizmetlerine (dizinle ilişkili Azure AD Premium, Microsoft Azure veya Office 365 gibi) ilişkin hiçbir aboneliğin bulunmaması gerekir. Örneğin, sizin için Azure'da varsayılan bir dizin oluşturulduysa ve Azure aboneliğinizin kimlik doğrulaması için hâlâ bu dizini kullanıyor olması halinde bu dizini silemezsiniz. Benzer şekilde, başka bir kullanıcı dizinle bir aboneliği ilişkilendirdiyse o dizini silemezsiniz.

## <a name="delete-the-directory"></a>Dizini silme

1. Kuruluşunuzun Genel Yöneticisi olan bir hesapla [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.

2. **Azure Etkin Dizini'ni**seçin.

3. Silmek istediğiniz dizine geçin.
  
   ![Silmeden önce kuruluşu onaylama](./media/directory-delete-howto/delete-directory-command.png)

4. **Dizini sil'i**seçin.
  
   ![kuruluşu silmek için komutu seçin](./media/directory-delete-howto/delete-directory-list.png)

5. Dizininiz bir veya daha fazla denetimden geçmezse, nasıl geçeceğiniz hakkında daha fazla bilgi için bir bağlantı sağlanır. Tüm denetimleri geçtikten sonra işlemi tamamlamak için **Sil'i** seçin.

## <a name="if-you-cant-delete-the-directory"></a>Dizini silerseniz

Azure AD dizininizi yapılandırdığınızda, kuruluşunuz için Azure AD Premium P2, Office 365 Business Premium veya Enterprise Mobility + Security E5 gibi lisans tabanlı abonelikleri de etkinleştirmiş olabilirsiniz. Yanlışlıkla veri kaybını önlemek için, abonelikler tamamen silinene kadar bir dizini silemezsiniz. Abonelikler, dizin silme işlemine izin vermek için **Hükmün Kaldırılması** durumunda olmalıdır. **Süresi Dolan** veya **İptal Edilen** abonelik Devre **Dışı Bırakılmış** durumuna taşınır ve son aşama **Deprovisioned** durumudur.

Deneme Office 365 aboneliğinin süresi dolduğunda (ücretli İş Ortağı/CSP, Kurumsal Sözleşme veya Toplu Lisanslama dahil) ne beklemelisiniz? Office 365 veri saklama ve abonelik yaşam döngüsü hakkında daha fazla bilgi için [bkz.](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3) 

Abonelik durumu | Veri | Verilere erişim
----- | ----- | -----
Etkin (deneme süresi 30 gün) | Herkes tarafından erişilebilir veriler | Kullanıcıların Office 365 dosyalarına veya uygulamalarına normal erişimi vardır<br>Yöneticilerin Microsoft 365 yönetici merkezi ve kaynaklarına normal erişimi vardır 
Süresi doldu (30 gün) | Herkes tarafından erişilebilir veriler| Kullanıcıların Office 365 dosyalarına veya uygulamalarına normal erişimi vardır<br>Yöneticilerin Microsoft 365 yönetici merkezi ve kaynaklarına normal erişimi vardır
Engelli (30 gün) | Yalnızca yönetici tarafından erişilebilen veriler | Kullanıcılar Office 365 dosyalarına veya uygulamalarına erişemez<br>Yöneticiler Microsoft 365 yönetici merkezine erişebilir, ancak kullanıcılara lisans atayamıyor veya güncelleştiremiyor
Hükmün iptali (Devre Dışı bırakıldıktan 30 gün sonra) | Silinen veriler (başka hizmet yoksa otomatik olarak silinir) | Kullanıcılar Office 365 dosyalarına veya uygulamalarına erişemez<br>Yöneticiler, diğer abonelikleri satın almak ve yönetmek için Microsoft 365 yönetici merkezine erişebilir

## <a name="delete-a-subscription"></a>Aboneliği silme

Microsoft 365 yönetici merkezini kullanarak üç gün içinde silinecek **Hükmün Silinme** durumuna abonelik koyabilirsiniz.

1. Kuruluşunuzdaki genel yönetici olan bir hesapla [Microsoft 365 yönetici merkezinde](https://admin.microsoft.com) oturum açın. İlk varsayılan etki contoso.onmicrosoft.com olan "Contoso" dizinini silmeye çalışıyorsanız, '. gibi admin@contoso.onmicrosoft.combir UPN ile oturum açın.

2. **Yeni yönetici merkezi** geçişini deneyin'in etkin olduğundan emin olarak yeni Microsoft 365 yönetici merkezini önizleyin.

   ![Yeni M365 yönetici merkezi deneyimini önizleyin](./media/directory-delete-howto/preview-toggle.png)

3. Yeni yönetici merkezi etkinleştirildikten sonra, aboneliği silmeden önce iptal etmeniz gerekir. **Faturalama'yı** seçin ve **Hizmetler& Ürünleri'ni**seçin, ardından iptal etmek istediğiniz abonelik için **aboneliği İptal** Et'i seçin. Bir geri bildirim sayfasına getirilecektir.

   ![İptal etmek için abonelik seçin](./media/directory-delete-howto/cancel-choose-subscription.png)

4. Geri bildirim formunu doldurun ve aboneliği iptal etmek için **aboneliği Iptal** et'i seçin.

   ![Abonelik önizlemesinde komutu iptal etme](./media/directory-delete-howto/cancel-command.png)

5. Artık aboneliği silebilirsiniz. Silmek istediğiniz abonelik için **Sil'i** seçin. Abonelik, **Ürünler & hizmetler** sayfasında bulamıyorsanız, Abonelik **durumunun** **Tümü**olarak ayarlandığınızdan emin olun.

   ![Aboneliği silme bağlantısını silme](./media/directory-delete-howto/delete-command.png)

6. Aboneliği silmek ve şart ve koşulları kabul etmek için **aboneliği Sil'i** seçin. Tüm veriler üç gün içinde kalıcı olarak silinir. Fikrinizi değiştirirseniz, aboneliği üç günlük süre içinde [yeniden etkinleştirebilirsiniz.](https://docs.microsoft.com/office365/admin/subscriptions-and-billing/reactivate-your-subscription?view=o365-worldwide)
  
   ![şart ve koşulları dikkatle okuyun](./media/directory-delete-howto/delete-terms.png)

7. Şimdi abonelik durumu değişti ve abonelik silinme için işaretlenir. Abonelik 72 saat sonra **Hükmün Yok Edilsin** durumuna girer.

8. Dizininizdeki bir aboneliği sildikten ve 72 saat geçtikten sonra, Azure AD yönetici merkezinde yeniden oturum açabilirsiniz ve gerekli eylem ve dizin silme işlemini engelleyen abonelikler olmamalıdır. Azure REKLAM dizininizi başarıyla silebilirsiniz.
  
   ![silme ekranında abonelik denetimi geçmek](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="i-have-a-trial-subscription-that-blocks-deletion"></a>Silmeişlemini engelleyen bir deneme aboneliğim var

Microsoft Power BI, Rights Management Services, Microsoft Power Apps veya Dynamics 365 gibi [self servis kayıt ürünleri](https://docs.microsoft.com/office365/admin/misc/self-service-sign-up?view=o365-worldwide) vardır, bireysel kullanıcılar Office 365 üzerinden kaydolabilir ve bu da Azure AD dizininizde kimlik doğrulama için konuk bir kullanıcı oluşturur. Bu self servis ürünler, veri kaybını önlemek için dizin silinene kadar dizin silmelerini engeller. Bunlar yalnızca Azure AD yöneticisi tarafından silinebilir, kullanıcı tek tek kaydolmuş veya ürüne atanmış olsun.

Nasıl atandıklarına dair iki tür self servis kayıt ürünü vardır: 

* Org düzeyinde atama: Bir Azure AD yöneticisi ürünü tüm kuruluşa atar ve kullanıcı tek tek lisanslanmasa lar bile bu org düzeyinde atamayla hizmeti etkin bir şekilde kullanabilir.
* Kullanıcı düzeyi ataması: Self servis kaydı sırasında bireysel bir kullanıcı aslında bir yönetici olmadan kendilerine ürün atar. Kuruluş bir yönetici tarafından yönetildikten sonra (bkz. [yönetilmeyen bir dizinin yöneticisi devralınır,](domains-admin-takeover.md)yönetici ürünü self servis kaydolmadan doğrudan kullanıcılara atayabilir.  

Self servis kayıt ürünün silinmeye başladığınızda, eylem verileri kalıcı olarak siler ve hizmete tüm kullanıcı erişimini kaldırır. Teklife tek tek veya kuruluş düzeyinde atanan tüm kullanıcının oturum açması veya varolan verilere erişmesi engellenir. [Microsoft Power BI panoları](https://docs.microsoft.com/power-bi/service-export-to-pbix) veya Hak Yönetimi Hizmetleri ilkesi [yapılandırması](https://docs.microsoft.com/azure/information-protection/configure-policy#how-to-configure-the-azure-information-protection-policy)gibi self servis kayıt ürünüyle veri kaybını önlemek istiyorsanız, verilerin yedeklenmesini ve başka bir yere kaydedilmesini sağlayın.

Şu anda kullanılabilen self servis kayıt ürünleri ve hizmetleri hakkında daha fazla bilgi için, [Kullanılabilir self servis programlarına](https://docs.microsoft.com/office365/admin/misc/self-service-sign-up?view=o365-worldwide#available-self-service-programs)bakın.

Deneme Office 365 aboneliğinin süresi dolduğunda (ücretli İş Ortağı/CSP, Kurumsal Sözleşme veya Toplu Lisanslama dahil) ne beklemelisiniz? Office 365 veri saklama ve abonelik yaşam döngüsü hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/office365/admin/subscriptions-and-billing/what-if-my-subscription-expires?view=o365-worldwide)

Ürün durumu | Veri | Verilere erişim
------------- | ---- | --------------
Etkin (deneme süresi 30 gün) | Herkes tarafından erişilebilir veriler | Kullanıcıların self servis kayıt ürününe, dosyalarına veya uygulamalarına normal erişimi vardır<br>Yöneticilerin Microsoft 365 yönetici merkezi ve kaynaklarına normal erişimi vardır
Silme | Silinen veriler | Kullanıcılar self servis kayıt ürününe, dosyalarına veya uygulamalarına erişemez<br>Yöneticiler, diğer abonelikleri satın almak ve yönetmek için Microsoft 365 yönetici merkezine erişebilir

## <a name="how-can-i-delete-a-self-service-sign-up-product-in-the-azure-portal"></a>Azure portalındaki self servis kayıt ürününü nasıl silebilirim?

Microsoft Power BI veya Azure Hakları Yönetimi Hizmetleri gibi bir self servis kayıt ürününü Azure AD portalında hemen silinecek bir **Silme** durumuna koyabilirsiniz.

1. Kuruluşta Genel Yönetici olan bir hesapla [Azure AD yönetici merkezinde](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oturum açın. İlk varsayılan etki contoso.onmicrosoft.com olan "Contoso" dizinini silmeye çalışıyorsanız, admin@contoso.onmicrosoft.com'. gibi bir UPN ile oturum açın.

2. **Lisansları**seçin ve ardından **Self servis kayıt ürünlerini**seçin. Tüm self servis kayıt ürünlerini koltuk tabanlı aboneliklerden ayrı olarak görebilirsiniz. Kalıcı olarak silmek istediğiniz ürünü seçin. Microsoft Power BI'de bir örnek aşağıda verilmiştir:

    ![kullanıcı adı yanlış yazılmış veya bulunamaz](./media/directory-delete-howto/licenses-page.png)

3. Ürünü silmek ve verilerin hemen ve geri alınamaz şekilde silindiği koşulları kabul etmek için **Sil'i** seçin. Bu silme eylemi tüm kullanıcıları kaldırır ve kuruluş ürüne erişimini kaldırır. Silme işlemine devam etmek için Evet'i tıklatın.  

    ![kullanıcı adı yanlış yazılmış veya bulunamaz](./media/directory-delete-howto/delete-product.png)

4. **Evet'i**seçtiğinizde, self servis ürünün silinmesi başlatılır. Silme işlemini size bildirecek bir bildirim var.  

    ![kullanıcı adı yanlış yazılmış veya bulunamaz](./media/directory-delete-howto/progress-message.png)

5. Şimdi self-servis kayıt ürün durumu **Silindi**olarak değiştirildi. Sayfayı yenilediğinizde, ürün **Self servis kayıt ürünleri** sayfasından kaldırılmalıdır.  

    ![kullanıcı adı yanlış yazılmış veya bulunamaz](./media/directory-delete-howto/product-deleted.png)

6. Tüm ürünleri sildikten sonra, Azure AD yönetici merkezinde yeniden oturum açabilirsiniz ve gerekli eylem ve dizin silme işlemini engelleyen hiçbir ürün olmamalıdır. Azure REKLAM dizininizi başarıyla silebilirsiniz.

    ![kullanıcı adı yanlış yazılmış veya bulunamaz](./media/directory-delete-howto/delete-organization.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/)
