---
title: Öğretici-mevcut eşitlenmiş bir AD Ormanı için pilot Azure AD Connect bulut sağlama
description: öğreticide.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b83f634e9f5954e7a465761b117b6ee32f843aa2
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75425091"
---
# <a name="pilot-cloud-provisioning-for-an-existing-synced-ad-forest"></a>Mevcut bir eşitlenmiş AD ormanı için pilot bulut sağlama 

Bu öğreticide, zaten Azure Active Directory (Azure AD) Connect Sync kullanılarak eşitlenmiş bir test Active Directory ormanı için bulut sağlamasının nasıl ele aldığı anlatılmaktadır.

![Create](media/tutorial-migrate-aadc-aadccp/diagram.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Bu öğreticiyi denemeden önce aşağıdaki öğeleri göz önünde bulundurun:
1. Bulut sağlama temelleri hakkında bilgi sahibi olduğunuzdan emin olun. 
2. Azure AD Connect Sync sürüm 1.4.32.0 veya üstünü kullandığınızdan ve eşitleme kurallarını belgelenen şekilde yapılandırdığınızdan emin olun. Ne zaman bir test OU 'su veya grubu Azure AD Connect eşitleme kapsamından kaldıracaksınız. Nesneleri kapsam dışına taşımak, Azure AD 'de bu nesnelerin silinmesine yol açar. Kullanıcı nesneleri söz konusu olduğunda, Azure AD 'deki nesneler geçici olarak silinir ve geri yüklenebilir. Grup nesneleri söz konusu olduğunda, Azure AD 'deki nesneler sabit olarak silinir ve geri yüklenemez. Azure AD Connect eşitlemede yeni bir bağlantı türü tanıtılmıştır, bu, bir pilot senaryo durumunda silmeyi engeller. 
3. Pilot kapsamdaki nesnelerin, bulut sağlama ile nesnelerle eşleştiğinden emin olmak için ms-DS-ımıbu GUID doldurulmuş olduğundan emin olun. 

   > [!NOTE]
   > Azure AD Connect eşitleme, Grup nesneleri için varsayılan olarak *MS-DS-ımuz GUID 'yi* doldurmaz. [Bu blog postasında](https://blogs.technet.microsoft.com/markrenoden/2017/10/13/choosing-a-sourceanchor-for-groups-in-multi-forest-sync-with-aad-connect/) belgelenen adımları izleyerek, Grup nesneleri için *MS-DS-ımuz GUID* 'yi doldurun.

4. Bu gelişmiş bir senaryodur. Bu öğreticide açıklanan adımları tam olarak izlediğinizden emin olun.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için gerekli Önkoşullar aşağıda verilmiştir
- Azure AD Connect Sync sürüm 1.4.32.0 veya üzeri bir test ortamı
- Eşitleme kapsamındaki bir OU veya grup ve pilot kullanılabilir. Küçük bir nesne kümesiyle başlamasını öneririz.
- Sağlama aracısını barındıracak Windows Server 2012 R2 veya üstünü çalıştıran bir sunucu.  Bu, Azure AD Connect sunucusuyla aynı sunucu olamaz.
- AAD Connect eşitlemesine ait kaynak bağlantısı, *Objectguıd* veya *MS-DS-ımdsguıd* olmalıdır

## <a name="update-azure-ad-connect"></a>Güncelleştirme Azure AD Connect

Minimum olarak, [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.4.32.0 sahip olmanız gerekir. Azure AD Connect eşitlemesini güncelleştirmek için Azure AD Connect şu adımları uygulayın [: en son sürüme yükseltme](../hybrid/how-to-upgrade-previous-version.md).  

## <a name="stop-the-scheduler"></a>Zamanlayıcıyı durdur
Azure AD Connect eşitleme, bir Zamanlayıcı kullanarak şirket içi dizininizde gerçekleşen değişiklikleri eşitler. Özel kuralları değiştirmek ve eklemek için, bu Zamanlayıcı 'yı devre dışı bırakmak istiyorsunuz. bu sayede, bu işlem üzerinde çalışırken eşitlemeler çalışmaz.  Aşağıdaki adımları kullanın:

1.  Azure AD Connect çalıştıran sunucuda, açık PowerShell 'i yönetici ayrıcalıklarıyla eşitleyin.
2.  `Stop-ADSyncSyncCycle` öğesini çalıştırın.  ENTER tuşuna basın.
3.  `Set-ADSyncScheduler -SyncCycleEnabled $false` öğesini çalıştırın.

>[!NOTE] 
>AAD Connect eşitleme için kendi özel zamanlayıcıyı çalıştırıyorsanız lütfen Zamanlayıcıyı devre dışı bırakın. 

## <a name="create-custom-user-inbound-rule"></a>Özel Kullanıcı gelen kuralı oluştur

 1. Aşağıda gösterildiği gibi, masaüstündeki uygulama menüsünden eşitleme düzenleyicisini başlatın:</br>
 Eşitleme kuralı Düzenleyicisi menüsünü ![](media/how-to-cloud-custom-user-rule/user8.png)</br>
 
 2. Yön için açılan listeden **gelen** ' ı seçin ve **Yeni kural ekle**' ye tıklayın.
 Özel kural ![](media/how-to-cloud-custom-user-rule/user1.png)</br>
 
 3. **Açıklama** sayfasında, aşağıdakileri girin ve **İleri**' ye tıklayın:

    **Ad:** Kurala anlamlı bir ad verin<br>
    **Açıklama:** Anlamlı bir açıklama ekleyin<br>**bağlı sistem  
    :** için özel eşitleme KURALı yazarken ad bağlayıcısını seçin<br>
    **Bağlı sistem nesne türü:** Kullanıcısını<br>
    **Meta veri deposu nesne türü:** Kişiler<br>
    **Bağlantı türü:** Ayrılma<br>
    **Öncelik:** Sistemde benzersiz bir değer sağlayın<br>
    **Etiket:** Bunu boş bırakın<br>
    Özel kural ![](media/how-to-cloud-custom-user-rule/user2.png)</br>
 
 4. **Kapsam filtresi** sayfasında, pilot uygulamayı açmak istediğiniz OU veya güvenlik grubunu girin.  OU 'ya filtre uygulamak için, ayırt edici adın OU kısmını ekleyin. Bu kural, o OU 'da bulunan tüm kullanıcılara uygulanır.  Yani, DN "OU = CPUsers, DC = contoso, DC = com" ile sonlanıyorsa, bu filtreyi eklersiniz.  Ardından **İleri**'ye tıklayın. 

    |Kural|Öznitelik|İşleç|Değer|
    |-----|----|----|-----|
    |Kapsamı bulunan OU|DN|ENDSWITH|OU 'nun ayırt edici adı.|
    |Kapsam grubu||ISMEMBEROF|Güvenlik grubunun ayırt edici adı.|

    ![Özel kural](media/how-to-cloud-custom-user-rule/user3.png)</br>
 
 5. Kural **Birleştir** sayfasında, **İleri**' ye tıklayın.
 6. **Dönüşümler** sayfasında, sabit bir dönüşüm ekleyin: cloudNoFlow özniteliğine akış doğru. **Ekle**'ye tıklayın.
 Özel kural ![](media/how-to-cloud-custom-user-rule/user4.png)</br>

Tüm nesne türleri için aynı adımların izlenmesi gerekir (Kullanıcı, Grup ve kişi). Yapılandırılmış AD Bağlayıcısı/AD Ormanı başına adımları yineleyin. 

## <a name="create-custom-user-outbound-rule"></a>Özel Kullanıcı giden kuralı oluştur

 1. Yön için açılan listeden **giden** ' ı seçin ve **Kural Ekle**' ye tıklayın.
 Özel kural ![](media/how-to-cloud-custom-user-rule/user5.png)</br>
 
 2. **Açıklama** sayfasında, aşağıdakileri girin ve **İleri**' ye tıklayın:

    **Ad:** Kurala anlamlı bir ad verin<br>
    **Açıklama:** Anlamlı bir açıklama ekleyin<br>**bağlı sistem  
    :** için özel eşitleme KURALı yazarken AAD bağlayıcısını seçin<br>
    **Bağlı sistem nesne türü:** Kullanıcısını<br>
    **Meta veri deposu nesne türü:** Kişiler<br>
    **Bağlantı türü:** JoinNoFlow<br>
    **Öncelik:** Sistemde benzersiz bir değer sağlayın<br>
    **Etiket:** Bunu boş bırakın<br>
    
    ![Özel kural](media/how-to-cloud-custom-user-rule/user6.png)</br>
 
 3. **Kapsam filtresi** sayfasında **cloudnoflow** eşittir **true**' ı seçin. Ardından **İleri**'ye tıklayın.
 Özel kural ![](media/how-to-cloud-custom-user-rule/user7.png)</br>
 
 4. Kural **Birleştir** sayfasında, **İleri**' ye tıklayın.
 5. **Dönüşümler** sayfasında, **Ekle**' ye tıklayın.

Tüm nesne türleri için aynı adımların izlenmesi gerekir (Kullanıcı, Grup ve kişi).

## <a name="install-the-azure-ad-connect-provisioning-agent"></a>Azure AD Connect sağlama aracısını yükler
1. Kurumsal Yönetici izinleriyle kullanacağınız sunucuda oturum açın.  [Temel ad ve Azure ortamı](tutorial-basic-ad-azure.md) öğreticisini KULLANıYORSANıZ, CP1 olacaktır.
2. Azure AD Connect bulut sağlama aracısını [buradan](https://go.microsoft.com/fwlink/?linkid=2109037)indirin.
3. Azure AD Connect bulut sağlamasını çalıştırın (AADConnectProvisioningAgent. Installer)
3. Giriş ekranında, lisans koşullarını **kabul edin** ve **yükler**' e tıklayın.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install1.png)</br>

4. Bu işlem tamamlandıktan sonra Yapılandırma Sihirbazı başlatılır.  Azure AD Genel Yönetici hesabınızla oturum açın.
5. **Bağlan Active Directory** ekranında, **Dizin Ekle** ' ye tıklayın ve Active Directory Yönetici hesabınızla oturum açın.  Bu işlem, şirket içi dizininizi ekleyecek.  **İleri**’ye tıklayın.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install3.png)</br>

6. **Yapılandırma Tamam** ekranında **Onayla**' ya tıklayın.  Bu işlem aracıyı kaydedip yeniden başlatacak.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install4.png)</br>

7. Bu işlem tamamlandıktan sonra, **başarıyla doğrulandığına** ilişkin bir uyarı görmeniz gerekir.  **Çıkış**' a tıklayabilirsiniz.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install5.png)</br>
8. İlk giriş ekranını hala görüyorsanız **Kapat**' a tıklayın.

## <a name="verify-agent-installation"></a>Aracı yüklemesini doğrulama
Aracı doğrulaması Azure portal ve aracıyı çalıştıran yerel sunucu üzerinde oluşur.

### <a name="azure-portal-agent-verification"></a>Aracı doğrulama Azure portal
Aracının Azure tarafından görüldüğünü doğrulamak için şu adımları izleyin:

1. Azure Portal’da oturum açın.
2. Sol tarafta **Azure Active Directory**' ı seçin, **Azure AD Connect** ' a tıklayın ve ardından **yönetimi sağlama (Önizleme)** seçeneğini belirleyin.</br>
![Azure Portal](media/how-to-install/install6.png)</br>

3.  **Azure AD sağlama (Önizleme)** ekranında **tüm aracıları gözden geçir**' e tıklayın.
Azure AD sağlama](media/how-to-install/install7.png) ![</br>
 
4. **Şirket içi sağlama aracıları ekranında** , yüklediğiniz aracıları görürsünüz.  Söz konusu aracının orada olduğunu ve **devre dışı**olarak işaretlendiğinden emin olun.  Aracı varsayılan ![sağlama aracılarını devre dışı bırakılır](media/how-to-install/verify1.png)</br>

### <a name="on-the-local-server"></a>Yerel sunucuda
Aracının çalıştığını doğrulamak için şu adımları izleyin:

1.  Yönetici hesabıyla sunucuda oturum açma
2.  Hizmetlere giderek veya Start/Run/Services. msc ' ye giderek **Hizmetleri** açın.
3.  **Hizmetler** ' in altında **Microsoft Azure AD aracı Güncelleştirici** ' ı bağlama ve **Microsoft Azure AD Connect sağlama aracısının** aynı ve **çalışır**durumda olduğundan emin olun.
![Hizmetler](media/how-to-troubleshoot/troubleshoot1.png)

## <a name="configure-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlamasını yapılandırma
Sağlamayı yapılandırmak için aşağıdaki adımları kullanın:

 1. Azure AD portalında oturum açın.
 2. **Azure Active Directory** tıklayın
 3. **Azure AD Connect** tıklayın
 4. **Sağlamayı Yönet (Önizleme)** 
 ![](media/how-to-configure/manage1.png) seçin</br>
 5.  **Yeni yapılandırma**
 tıklayın ![](media/tutorial-single-forest/configure1.png)</br>
 6.  Yapılandırma ekranında bir **bildirim e-postası**girin, seçiciyi **etkinleştirmek** için taşıyın ve **Kaydet**' e tıklayın.
 ![](media/tutorial-single-forest/configure2.png)</br>
 7. **Yapılandır**' ın altında, yapılandırma kuralının kapsamını değiştirmek için **tüm kullanıcılar** ' ı seçin.
 ![](media/how-to-configure/scope2.png)</br>
 8. Sağ tarafta, yeni oluşturduğunuz kuruluş birimini (OU = CPUsers, DC = contoso, DC = com ") içerecek şekilde kapsamını değiştirin.
 ![](media/tutorial-existing-forest/scope2.png)</br>
 9.  **Bitti** ve **Kaydet**' e tıklayın.
 10. Kapsam artık tek bir kuruluş birimine ayarlanmalıdır. 
 ![](media/tutorial-existing-forest/scope3.png)</br>
 

## <a name="verify-users-are-provisioned-by-cloud-provisioning"></a>Kullanıcıların bulut sağlaması tarafından sağlandığını doğrulayın
Artık şirket içi dizinimizde bulunan kullanıcıların eşitlenmiş olduğunu ve artık Azure AD kiracısında bulunduğunu doğrulayacaksınız.  Bu işlem işleminin tamamlanması birkaç saat sürebilir.  Kullanıcıların bulut sağlamaya göre sağlamasını doğrulamak için şu adımları izleyin:

1. [Azure portalına](https://portal.azure.com) gidip Azure aboneliği olan bir hesapla oturum açın.
2. Sol tarafta **Azure Active Directory** ' yi seçin.
3. **Azure AD Connect** tıklayın
4. **Sağlamayı yönetme (Önizleme)** seçeneğine tıklayın
5. **Günlükler** düğmesine tıklayın
6. Kullanıcının bulut sağlaması tarafından sağlandığını doğrulamak için bir Kullanıcı adı arayın

Ayrıca, Azure AD 'de Kullanıcı ve grubun mevcut olduğunu doğrulayabilirsiniz.

## <a name="start-the-scheduler"></a>Zamanlayıcıyı başlatma
Azure AD Connect eşitleme, bir Zamanlayıcı kullanarak şirket içi dizininizde gerçekleşen değişiklikleri eşitler. Artık kuralları değiştirdiğimize göre, Scheduler 'ı yeniden başlatabilirsiniz.  Aşağıdaki adımları kullanın:

1.  Azure AD Connect çalıştıran sunucuda, açık PowerShell 'i yönetici ayrıcalıklarıyla eşitleyin
2.  `Set-ADSyncScheduler -SyncCycleEnabled $true` öğesini çalıştırın.
3.  `Start-ADSyncSyncCycle` öğesini çalıştırın.  ENTER tuşuna basın.  

>[!NOTE] 
>AAD Connect eşitleme için kendi özel zamanlayıcıyı çalıştırıyorsanız lütfen zamanlayıcıyı etkinleştirin. 

## <a name="something-went-wrong"></a>Bir sorun oluştu
Pilot 'ın beklendiği gibi çalışmayolmaması durumunda aşağıdaki adımları izleyerek Azure AD Connect eşitleme kurulumuna geri dönebilirsiniz:
1.  Azure portal sağlama yapılandırmasını devre dışı bırakın. 
2.  Eşitleme kuralı Düzenleyicisi aracını kullanarak bulut sağlaması için oluşturulan tüm özel eşitleme kurallarını devre dışı bırakın. Devre dışı bırakma, tüm bağlayıcılarda tam eşitlemeye neden olmalıdır.

## <a name="configure-azure-ad-connect-sync-to-exclude-the-pilot-ou"></a>Pilot OU 'yu dışlamak için Azure AD Connect eşitlemesini yapılandırın
Pilot OU 'dan gelen kullanıcıların bulut sağlama tarafından başarıyla yönetildiğini doğrulandıktan sonra, yukarıda oluşturulan pilot OU 'yu dışlamak için Azure AD Connect yeniden yapılandırabilirsiniz.  Bulut sağlama Aracısı, ileri giderek bu kullanıcıların eşitlemesini işleyecek.  Azure AD Connect kapsam için aşağıdaki adımları kullanın.

 1. Azure AD Connect çalıştıran sunucuda Azure AD Connect simgesine çift tıklayın.
 2. **Yapılandır** ' a tıklayın
 3. **Eşitleme seçeneklerini Özelleştir** ' i seçin ve ileri ' ye tıklayın.
 4. Azure AD 'de oturum açın ve **İleri**' ye tıklayın.
 5. **Dizinlerinizi bağlama** ekranında **İleri**' ye tıklayın.
 6. **Etki alanı ve OU filtreleme** ekranında **Seçili etki alanlarını ve OU 'ları Eşitle**' yi seçin.
 7. Etki alanınızı genişletin ve **Cpusers** OU 'sunu devre dışı olarak **seçin** .  **İleri**’ye tıklayın.
![kapsam](media/tutorial-existing-forest/scope1.png)</br>
 9. **Isteğe bağlı özellikler** ekranında **İleri**' ye tıklayın.
 10. **Yapılandırmaya hazırlanma** ekranında **Yapılandır**' a tıklayın.
 11. Tamamlandıktan sonra **Çıkış**' a tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

