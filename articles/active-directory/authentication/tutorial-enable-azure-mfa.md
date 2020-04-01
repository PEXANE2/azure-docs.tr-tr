---
title: Azure Multi-Factor Authentication’ı etkinleştirme
description: Bu eğitimde, bir grup kullanıcı için Azure Çok Faktörlü Kimlik Doğrulaması'nı nasıl etkinleştirdiğinizi ve oturum açma etkinliği sırasında ikincil faktör istemini nasıl test ettiğinizi öğrenirsiniz.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 02/11/2020
ms.author: iainfou
author: iainfoulds
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 253eb23be03c1cc0f2abf4ad1fed734426dc287d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77154826"
---
# <a name="tutorial-secure-user-sign-in-events-with-azure-multi-factor-authentication"></a>Öğretici: Azure Çok Faktörlü Kimlik Doğrulama ile kullanıcı oturum açma etkinliklerini güvenli hale

Çok faktörlü kimlik doğrulama (MFA), oturum açma olayı sırasında kullanıcıdan ek tanımlama formları istendiği bir işlemdir. Bu istek, cep telefonlarına bir kod girmek veya parmak izi tadına varmak olabilir. İkinci bir kimlik doğrulama biçimi ne zaman gerekiyorsa, bu ek faktör saldırganın elde etmesi veya çoğaltması kolay bir şey olmadığından güvenlik artırılır.

Azure Çok Faktörlü Kimlik Doğrulama ve Koşullu Erişim ilkeleri, belirli oturum açma etkinlikleri sırasında kullanıcılar için MFA'yı etkinleştirme esnekliği sağlar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir grup kullanıcı için Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek için Koşullu Erişim ilkesi oluşturma
> * MFA için gereken ilke koşullarını yapılandırma
> * MFA işlemini kullanıcı olarak test edin

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Azure AD Premium veya deneme lisansı etkin olan çalışan bir Azure AD kiracısı.
    * Gerekirse, [ücretsiz bir oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* *Genel yönetici* ayrıcalıklarına sahip bir hesap.
* *Testuser*gibi bildiğiniz bir parolaya sahip yönetici olmayan bir kullanıcı. Bu öğreticide bu hesabı kullanarak son kullanıcı Azure Çok Faktörlü Kimlik Doğrulama deneyimini sınarsınız.
    * Bir kullanıcı oluşturmanız gerekiyorsa, [bkz.](../add-users-azure-active-directory.md)
* Yönetici olmayan kullanıcının üyesi olduğu *MFA-Test Grubu*gibi bir grup. Bu eğitimde bu grup için Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirin.
    * Bir grup oluşturmanız gerekiyorsa, [nasıl bir grup oluşturabileceğinize ve Azure Etkin Dizini'ne nasıl üye ekleyeceğinize](../active-directory-groups-create-azure-portal.md)bakın.

## <a name="create-a-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

Azure Çok Faktörlü Kimlik Doğrulama'yı etkinleştirme ve kullanmanın önerilen yolu Koşullu Erişim ilkeleridir. Koşullu Erişim, oturum açma olaylarına tepki veren ilkeler oluşturmanıza ve tanımlamanıza ve kullanıcıya bir uygulamaya veya hizmete erişim izni verilmeden önce ek eylemler istemenize olanak tanır.

![Oturum açma işlemini güvence altına almak için Koşullu Erişim'in nasıl çalıştığına genel bakış diyagramı](media/tutorial-enable-azure-mfa/conditional-access-overview.png)

Koşullu Erişim ilkeleri, kullanıcıların her yerde ve her zaman üretken olmalarını sağlamak ve aynı zamanda kuruluşunuzu korumak amacıyla ayrıntılı ve özel olabilir. Bu öğreticide, bir kullanıcı Azure portalında giriş yaptığında MFA için istekte olacak temel bir Koşullu Erişim ilkesi oluşturalım. Bu serinin daha sonraki bir öğreticisinde, risk tabanlı Koşullu Erişim ilkesini kullanarak Azure Çok Faktörlü Kimlik Doğrulaması'nı yapılandırabilirsiniz.

İlk olarak, bir Koşullu Erişim ilkesi oluşturun ve test kullanıcı grubunuzu aşağıdaki gibi atayın:

1. *Genel yönetici* izinlerine sahip bir hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure Active **Directory'yi**arayın ve seçin, ardından sol taraftaki menüden **Güvenlik'i** seçin.
1. **Koşullu Erişim'i**seçin, ardından **+ Yeni ilkeyi**seçin.
1. *MFA Pilot*gibi ilke için bir ad girin.
1. **Atamalar**altında, **Kullanıcıları ve grupları**seçin, ardından kullanıcıları seçin ve radyo yu **seçin.**
1. Kullanıcılar ve **gruplar**için kutuyu işaretleyin, ardından kullanılabilir Azure REKLAM kullanıcılarına ve gruplarına göz atmak için **seçin.**
1. *MFA-Test Grubu*gibi Azure REKLAM grubuna göz atın ve seçin, ardından **Seç'i**seçin.

    [![](media/tutorial-enable-azure-mfa/select-group-for-conditional-access-cropped.png "Select your Azure AD group to use with the Conditional Access policy")](media/tutorial-enable-azure-mfa/select-group-for-conditional-access.png#lightbox)

1. Grup için Koşullu Erişim ilkesini uygulamak için **Bitti'yi**seçin.

## <a name="configure-the-conditions-for-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulama için koşulları yapılandırma

Oluşturulan Koşullu Erişim ilkesi ve atanan bir test kullanıcı grubu yla, artık ilkeyi tetikleyen bulut uygulamalarını veya eylemlerini tanımlayın. Bu bulut uygulamaları veya eylemleri, MFA için istem gibi ek işlem gerektirdiğine karar verdiğiniz senaryolardır. Örneğin, bir finansal uygulamaya erişimin veya yönetim araçlarının kullanımının ek doğrulama istemi olarak gerektirdiğine karar verebilirsiniz.

Bu öğretici için, Koşullu Erişim ilkesini, bir kullanıcı Azure portalında kaydolduğunda MFA gerektirecek şekilde yapılandırın.

1. **Bulut uygulamaları veya eylemleri**’ni seçin. Koşullu Erişim ilkesini *Tüm bulut uygulamalarına* veya Select *uygulamalarına*uygulamayı seçebilirsiniz. Esneklik sağlamak için, belirli uygulamaları ilkeden de hariç tutabilirsiniz.

    Bu öğretici için, *Ekle* **sayfasında, uygulamaları seç** radyo düğmesini seçin.

1. **Seç'i**seçin, ardından kullanılabilir oturum açma olayları listesine göz atın.

    Bu öğretici için, ilkenin Azure portalında oturum açma etkinlikleri için geçerli olması için **Microsoft Azure Yönetimi'ni** seçin.

1. Seçili uygulamaları uygulamak için **Seç**ve **Bitti'yi**seçin.

    ![Koşullu Erişim ilkesine eklemek için Microsoft Azure Yönetimi uygulamasını seçin](media/tutorial-enable-azure-mfa/select-azure-management-app.png)

Erişim denetimleri, onaylanmış bir istemci uygulamasına ihtiyaç duyma veya Karma Azure AD'si birleştirilmiş bir aygıtı kullanma gibi bir kullanıcıya erişim izni verilmesi için gereksinimleri tanımlamanıza izin verir. Bu öğreticide, azure portalında oturum açma etkinliği sırasında erişim denetimlerini MFA gerektirecek şekilde yapılandırın.

1. *Access denetimleri* **altında, Grant'i**seçin, ardından **Erişim Eki** Radyo düğmesinin seçildiğinden emin olun.
1. **Çok faktörlü kimlik doğrulamayı gerektir**' i için kutuyu işaretleyin , ardından **Seç'i**seçin.

Koşullu Erişim ilkeleri, yapılandırmanın kullanıcıları nasıl etkileyeceğini görmek istiyorsanız *yalnızca Rapor* olarak ayarlanabilir veya şu anda ilkeyi kullanmak istemiyorsanız *Kapalı* dır. Bu öğretici için bir test kullanıcı grubu hedeflenmiş olarak, ilkeyi etkinleştirin ve ardından Azure Çok Faktörlü Kimlik Doğrulaması'nı sınayın.

1. Etkinleştir *ilkesini* **A'ya**ayarlama
1. Koşullu Erişim ilkesini uygulamak için **Oluştur'u**seçin.

## <a name="test-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication'ı test etme

Koşullu Erişim ilkenizi ve Azure Çok Faktörlü Kimlik Doğrulama'yı iş başında görelim. İlk olarak, MFA gerektirmeyen bir kaynakta aşağıdaki gibi oturum açın:

1. InPrivate veya gizli modda yeni bir tarayıcı penceresi açın ve[https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)
1. *Test kullanıcısı*gibi yönetici olmayan test kullanıcınızla oturum açın. MFA'yı tamamlamanız için bir istek yok.
1. Tarayıcı penceresini kapatın.

Şimdi Azure portalında oturum açın. Azure portalı Koşullu Erişim ilkesinde ek doğrulama gerektirecek şekilde yapılandırıldıkça, bir Azure Çok Faktörlü Kimlik Doğrulama istemi alırsınız.

1. InPrivate veya gizli modda yeni bir tarayıcı penceresi [https://portal.azure.com](https://portal.azure.com)açın ve göz atın.
1. *Test kullanıcısı*gibi yönetici olmayan test kullanıcınızla oturum açın. Azure Çok Faktörlü Kimlik Doğrulaması için kaydolmanız ve bunları kullanmanız gerekir. İşlemi tamamlamak ve Azure portalında başarılı bir şekilde oturum açtığınızı doğrulamak için istemleri izleyin.

    ![Tarayıcı istemlerini izleyin ve ardından oturum açmanız için kayıtlı çok faktörlü kimlik doğrulama isteminizde](media/tutorial-enable-azure-mfa/azure-multi-factor-authentication-browser-prompt.png)

1. Tarayıcı penceresini kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin bir parçası olarak yapılandırılan Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek için Koşullu Erişim ilkesini artık kullanmak istemiyorsanız, aşağıdaki adımları kullanarak ilkeyi silin:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure Active **Directory'yi**arayın ve seçin, ardından sol taraftaki menüden **Güvenlik'i** seçin.
1. **Koşullu erişimi**seçin, ardından *MFA Pilot* gibi oluşturduğunuz ilkeyi seçin
1. **Sil'i**seçin, ardından ilkeyi silmek istediğinizi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, seçili bir kullanıcı grubu için Koşullu Erişim ilkelerini kullanarak Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirdin. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir grup Azure AD kullanıcısı için Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek için Koşullu Erişim ilkesi oluşturun
> * MFA için gereken ilke koşullarını yapılandırma
> * MFA işlemini kullanıcı olarak test edin

> [!div class="nextstepaction"]
> [Self servis parola sıfırlama (SSPR) için parola yazma yı etkinleştirme](tutorial-enable-writeback.md)
