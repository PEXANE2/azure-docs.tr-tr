---
title: Öğretici - Pilot Azure AD Varolan bir eşitlenmiş AD ormanı için bulut sağlama
description: Öğretici.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 03/04/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: aba42e6bd9b11e47d793219c0ff06b9177d609f5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78298828"
---
# <a name="pilot-cloud-provisioning-for-an-existing-synced-ad-forest"></a>Mevcut bir eşitlenmiş AD ormanı için pilot bulut sağlama 

Bu öğretici, Azure Active Directory (Azure AD) Connect eşitlemeyi kullanarak zaten senkronize edilmiş bir test Active Directory ormanı için bulut sağlama da size yardımcı olur.

![Oluşturma](media/tutorial-migrate-aadc-aadccp/diagram.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Bu öğreticiyi denemeden önce aşağıdaki öğeleri göz önünde bulundurun:
1. Bulut sağlamanın temellerini bildiğinizden emin olun. 
2. Azure AD Connect eşitleme sürümünü 1.4.32.0 veya daha sonra çalıştırdığınızdan ve eşitleme kurallarını belgelenmiş şekilde yapılandırdığınızdan emin olun. Pilot uygulama yaparken, Azure AD Connect eşitleme kapsamından bir test OU veya grubu kaldırıyor olursunuz. Nesneleri kapsam dışına taşımak, Azure AD'de bu nesnelerin silinmesine neden olabilir. Kullanıcı nesneleri söz konusu olduğunda, Azure AD'deki nesneler yumuşak silinir ve geri yüklenebilir. Grup nesneleri söz konusu olduğunda, Azure AD'deki nesneler zor silinir ve geri yüklenemez. Azure AD Connect eşitlemesinde, pilot uygulama senaryosu durumunda silinmesini önleyecek yeni bir bağlantı türü tanıtıldı. 
3. Pilot kapsamındaki nesnelerin ms-ds-tutarlılıkGUID'in doldurulmasını, böylece bulut sağlamanın nesnelerle sert eşleştiğinden emin olun. 

   > [!NOTE]
   > Azure AD Connect eşitleme, grup nesneleri için varsayılan olarak *ms-ds tutarlılığıGUID'i* doldurmaz.

4. Bu gelişmiş bir senaryo. Bu öğreticide belgelenen adımları tam olarak izlediğinizi emin olun.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticinin tamamlanması için gerekli ön koşullar aşağıda dır.
- Azure AD Connect eşitleme sürümü 1.4.32.0 veya sonraki sürümiçeren bir test ortamı
- Eşitleme kapsamında olan ve pilot tarafından kullanılabilen bir OU veya grup. Küçük bir nesne kümesiyle başlamanızı öneririz.
- Windows Server 2012 R2 veya daha sonra çalışan ve sağlama aracısını barındıran bir sunucu.  Bu, Azure AD Connect sunucusuyla aynı sunucu olamaz.
- AAD Connect eşitleme için kaynak çapa ya *objectGuid* veya *ms-ds-consistencyGUID* olmalıdır

## <a name="update-azure-ad-connect"></a>Azure AD Bağlantısını Güncelleştir

En az olarak, Azure AD 1.4.32.0 [bağlamanız](https://www.microsoft.com/download/details.aspx?id=47594) gerekir. Azure AD Connect eşitlemeyi güncelleştirmek [için Azure AD Connect'teki adımları tamamlayın: En son sürüme yükseltin.](../hybrid/how-to-upgrade-previous-version.md)  

## <a name="stop-the-scheduler"></a>Zamanlayıcıyı durdurun
Azure AD Connect eşitleme, şirket içi dizininizde bir zamanlayıcı kullanarak meydana gelen değişiklikleri eşitler. Özel kuralları değiştirmek ve eklemek için, siz üzerinde çalışırken eşitlemelerin çalışmaması için zamanlayıcıyı devre dışı katmak istiyorsunuz.  Aşağıdaki adımları kullanın:

1.  Azure AD Connect'i çalıştıran sunucuda PowerShell'i Yönetim Ayrıcalıklarıyla eşitleyin.
2.  `Stop-ADSyncSyncCycle` öğesini çalıştırın.  Enter tuşuna basın.
3.  `Set-ADSyncScheduler -SyncCycleEnabled $false` öğesini çalıştırın.

>[!NOTE] 
>AAD Connect eşitlemi için kendi özel zamanlayıcınızı çalıştırıyorsanız, lütfen zamanlayıcıyı devre dışı edin. 

## <a name="create-custom-user-inbound-rule"></a>Özel kullanıcı gelen kuralı oluşturma

 1. Masaüstündeki uygulama menüsünden senkronizasyon düzenleyicisini aşağıda gösterildiği gibi başlatın:</br>
 ![Senkronizasyon Kural Düzenleyicisi Menüsü](media/how-to-cloud-custom-user-rule/user8.png)</br>
 
 2. Yön için açılan listeden **Gelen'i** seçin ve **yeni kural ekle'ye**tıklayın.
 ![Özel kural](media/how-to-cloud-custom-user-rule/user1.png)</br>
 
 3. **Açıklama** sayfasında, aşağıdakileri girin ve **İleri'yi**tıklatın :

    **Adı:** Kurala anlamlı bir ad verin<br>
    **Açıklama:** Anlamlı bir açıklama ekleme<br>
    **Bağlı Sistem:** Özel eşitleme kuralını yazdığınız AD bağlayıcısını seçin<br>
    **Bağlı Sistem Nesne Türü:** Kullanıcı<br>
    **Metaverse Nesne Türü:** Kişi<br>
    **Bağlantı Türü:** Katılın<br>
    **Öncelik:** Sistemde benzersiz bir değer sağlama<br>
    **Etiket:** Bunu boş bırak.<br>
    ![Özel kural](media/how-to-cloud-custom-user-rule/user2.png)</br>
 
 4. Kapsam **filtresi** sayfasında, pilotun kapalı olmasını istediğiniz OU veya güvenlik grubunu girin.  OU'yu filtrelemek için, ayırt edilen adın OU bölümünü ekleyin. Bu kural, bu OU'da bulunan tüm kullanıcılariçin geçerliolacaktır.  Yani, DN "OU=CPUsers,DC=contoso,DC=com ile biterse, bu filtreyi eklersiniz.  Ardından **İleri**’ye tıklayın. 

    |Kural|Öznitelik|İşleç|Değer|
    |-----|----|----|-----|
    |Kapsam OU|Dn|ENDSWITH|OU'nun seçkin adı.|
    |Kapsam grubu||İsÜYE|Güvenlik grubunun seçkin adı.|

    ![Özel kural](media/how-to-cloud-custom-user-rule/user3.png)</br>
 
 5. **Birleştirme** kuralları sayfasında **İleri'yi**tıklatın.
 6. **Dönüşümler** sayfasında, Sabit dönüşüm ekleyin: cloudNoFlow özniteliğine doğru akış. **Ekle**’ye tıklayın.
 ![Özel kural](media/how-to-cloud-custom-user-rule/user4.png)</br>

Tüm nesne türleri (kullanıcı, grup ve ilgili kişi) için aynı adımların izlenmesi gerekir. Yapılandırılan AD Bağlayıcısı / AD ormanı başına adımları yineleyin. 

## <a name="create-custom-user-outbound-rule"></a>Özel kullanıcı giden kuralı oluşturma

 1. Yön için açılan listeden **Giden'i** seçin ve **Ekle kuralına**tıklayın.
 ![Özel kural](media/how-to-cloud-custom-user-rule/user5.png)</br>
 
 2. **Açıklama** sayfasında, aşağıdakileri girin ve **İleri'yi**tıklatın :

    **Adı:** Kurala anlamlı bir ad verin<br>
    **Açıklama:** Anlamlı bir açıklama ekleme<br>
    **Bağlı Sistem:** Özel eşitleme kuralını yazdığınız AAD bağlayıcısını seçin<br>
    **Bağlı Sistem Nesne Türü:** Kullanıcı<br>
    **Metaverse Nesne Türü:** Kişi<br>
    **Bağlantı Türü:** JoinNoFlow<br>
    **Öncelik:** Sistemde benzersiz bir değer sağlama<br>
    **Etiket:** Bunu boş bırak.<br>
    
    ![Özel kural](media/how-to-cloud-custom-user-rule/user6.png)</br>
 
 3. Kapsam **filtresi** sayfasında **cloudNoFlow** eşit **True'yu**seçin. Ardından **İleri**’ye tıklayın.
 ![Özel kural](media/how-to-cloud-custom-user-rule/user7.png)</br>
 
 4. **Birleştirme** kuralları sayfasında **İleri'yi**tıklatın.
 5. **Dönüşümler** sayfasında **Ekle'yi**tıklatın.

Tüm nesne türleri (kullanıcı, grup ve ilgili kişi) için aynı adımların izlenmesi gerekir.

## <a name="install-the-azure-ad-connect-provisioning-agent"></a>Azure AD Connect sağlama aracısını yükleme
1. Kurumsal yönetici izinleri ile kullanacağınız sunucuda oturum açın.  [Temel AD ve Azure ortamı](tutorial-basic-ad-azure.md) öğreticisini kullanıyorsanız, bu CP1 olacaktır.
2. Azure AD Connect bulut sağlama aracısını [burada](how-to-install.md#install-the-agent)belirtilen adımları kullanarak indirin.
3. Azure AD Connect bulut sağlama (AADConnectProvisioningAgent.Installer) çalıştırın
3. Sıçrama ekranında, lisans koşullarını **kabul edin** ve **Yükle'yi**tıklatın.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install1.png)</br>

4. Bu işlem tamamlandığında yapılandırma sihirbazı başlatılacaktır.  Azure AD global yönetici hesabınızla oturum açın.
5. Etkin **Dizin Ekle** ekranında **Dizini Ekle'yi** tıklatın ve ardından Active Directory yönetici hesabınızla oturum açın.  Bu işlem şirket içi dizininizi de ekleyecektir.  **İleri**'ye tıklayın.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install3.png)</br>

6. Yapılandırma **tam** ekranında, **Onayla'yı**tıklatın.  Bu işlem aracıyı kaydeder ve yeniden başlatacaktır.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install4.png)</br>

7. Bu işlem tamamlandıktan sonra, **başarıyla doğrulanmış** bir bildirim görmeniz gerekir.  **Çıkış'ı**tıklatabilirsiniz.</br>
![Hoş Geldiniz ekranı](media/how-to-install/install5.png)</br>
8. İlk sıçrama ekranını hala **görüyorsanız, Kapat'ı**tıklatın.

## <a name="verify-agent-installation"></a>Aracı yüklemeyi doğrula
Aracı doğrulaması Azure portalında ve aracıyı çalıştıran yerel sunucuda gerçekleşir.

### <a name="azure-portal-agent-verification"></a>Azure portal aracısı doğrulaması
Aracının Azure tarafından görüldüğünü doğrulamak için aşağıdaki adımları izleyin:

1. Azure Portal’da oturum açın.
2. Solda Azure **Etkin Dizin'i**seçin, **Azure AD Bağlantısı'nı** tıklatın ve merkezde **sağlamayı yönet 'i (önizleme)** seçin.</br>
![Azure portalda](media/how-to-install/install6.png)</br>

3.  Azure **AD Sağlama (önizleme)** ekranında **tüm aracıları gözden geçir'i**tıklatın.
![Azure REKLAM Sağlama](media/how-to-install/install7.png)</br>
 
4. Şirket **içi sağlama aracıları ekranında** yüklediğiniz aracıları göreceksiniz.  Söz konusu aracının orada olduğunu ve **Devre Dışı**bırakıldığını doğrulayın.  Aracı, varsayılan ![Sağlama aracıları tarafından devre dışı bırakılır](media/how-to-install/verify1.png)</br>

### <a name="on-the-local-server"></a>Yerel sunucuda
Aracının çalıştığını doğrulamak için aşağıdaki adımları izleyin:

1.  Yönetici hesabıyla sunucuda oturum açma
2.  **Hizmetleri** başlatarak veya Başlat/Çalıştır/Services.msc'ye giderek açın.
3.  **Hizmetler** altında **Microsoft Azure AD Connect Agent Updater** ve **Microsoft Azure AD Connect Provisioning Agent'ın** orada olduğundan ve durumun **çalışmadığından**emin olun.
![Hizmetler](media/how-to-troubleshoot/troubleshoot1.png)

## <a name="configure-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlama yı yapılandırma
Sağlamayapılandırmak için aşağıdaki adımları kullanın:

 1. Azure AD portalında oturum açın.
 2. **Azure Etkin Dizini'ni** tıklatın
 3. **Azure AD Bağlantısı'nı** tıklatın
 4. **Hükmü Yönet'i seçin (Önizleme)**
 ![](media/how-to-configure/manage1.png)</br>
 5.  **Yeni Yapılandırma'yı** tıklatın
 ![](media/tutorial-single-forest/configure1.png)</br>
 6.  Yapılandırma ekranında, bir **Bildirim e-postası**girin, seçiciyi **Etkinleştir'e** taşıyın ve **Kaydet'e**tıklayın.
 ![](media/tutorial-single-forest/configure2.png)</br>
 7. **Yapılandırma**altında, yapılandırma kuralının kapsamını değiştirmek için **Tüm kullanıcıları** seçin.
 ![](media/how-to-configure/scope2.png)</br>
 8. Sağda, "OU=CPUsers,DC=contoso,DC=com" oluşturduğunuz belirli OU'yu içerecek şekilde kapsamı değiştirin.
 ![](media/tutorial-existing-forest/scope2.png)</br>
 9.  **Bitti** ve **Kaydet'i**tıklatın.
 10. Kapsam artık tek bir kuruluş birimi olarak ayarlanmalıdır. 
 ![](media/tutorial-existing-forest/scope3.png)</br>
 

## <a name="verify-users-are-provisioned-by-cloud-provisioning"></a>Kullanıcıların bulut sağlama tarafından sağlanmış olduğunu doğrulayın
Artık şirket içi dizinimizde bulunan kullanıcıların senkronize edildiğini ve artık Azure AD kiracıdışında bulunduğunu doğrulayaceksiniz.  Bunun tamamlanmasının birkaç saat sürebileceğini unutmayın.  Kullanıcıların bulut sağlama ile sağlama da olduğunu doğrulamak için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com) gidip Azure aboneliği olan bir hesapla oturum açın.
2. Solda Azure **Etkin Dizini'ni** seçin
3. Azure **AD Bağlantısı'na** tıklayın
4. **Provizyon'u Yönet 'e tıklayın (önizleme)**
5. **Günlükler** düğmesine tıklayın
6. Kullanıcının bulut sağlama tarafından sağlanmış olduğunu doğrulamak için bir kullanıcı adı arama

Ayrıca, kullanıcının ve grubun Azure AD'de olduğunu da doğrulayabilirsiniz.

## <a name="start-the-scheduler"></a>Zamanlayıcıyı başlatın
Azure AD Connect eşitleme, şirket içi dizininizde bir zamanlayıcı kullanarak meydana gelen değişiklikleri eşitler. Artık kuralları değiştirdiğinize göre, zamanlayıcıyı yeniden başlatabilirsiniz.  Aşağıdaki adımları kullanın:

1.  Azure AD Connect çalıştıran sunucuda PowerShell'i Yönetim Ayrıcalıklarıyla eşitle
2.  `Set-ADSyncScheduler -SyncCycleEnabled $true` öğesini çalıştırın.
3.  `Start-ADSyncSyncCycle` öğesini çalıştırın.  Enter tuşuna basın.  

>[!NOTE] 
>AAD Connect eşitlemi için kendi özel zamanlayıcınızı çalıştırıyorsanız, lütfen zamanlayıcıyı etkinleştirin. 

## <a name="something-went-wrong"></a>Bir sorun oluştu
Pilotun beklendiği gibi çalışmaması durumunda, aşağıdaki adımları izleyerek Azure AD Connect eşitleme kurulumuna geri dönebilirsiniz:
1.  Azure portalında sağlama yapılandırmasını devre dışı kılabilir. 
2.  Eşitleme Kuralı Düzenleyicisi aracını kullanarak Bulut Sağlama için oluşturulan tüm özel eşitleme kurallarını devre dışı kınla. Devre dışı bırakma, tüm bağlayıcılar üzerinde tam eşitleme neden olmalıdır.

## <a name="configure-azure-ad-connect-sync-to-exclude-the-pilot-ou"></a>Pilot OU'yu hariç tutmak için Azure AD Connect eşitlemesini yapılandırın
Pilot OU kullanıcılarının bulut sağlama tarafından başarıyla yönetildiğini doğruladıktan sonra, yukarıda oluşturulan pilot OU'yu hariç tutmak için Azure AD Connect'i yeniden yapılandırabilirsiniz.  Bulut sağlama aracısı, bu kullanıcılar için eşitleme işlemlerini ileriye doğru işleyecek.  Azure AD Connect'i kapsamak için aşağıdaki adımları kullanın.

 1. Azure AD Connect çalıştıran sunucuda, Azure AD Connect simgesine çift tıklayın.
 2. **Yapıla'yı** tıklatın
 3. **Eşitleme seçeneklerini özelleştir'i** seçin ve sonrakini tıklatın.
 4. Azure AD'de oturum açın ve **İleri'yi**tıklatın.
 5. **Dizinlerinizi Bağla** ekranında **İleri'yi**tıklatın.
 6. Etki **Alanı ve OU filtreleme** ekranında, **seçili etki alanlarını ve OU'ları eşitle'yi**seçin.
 7. Etki alanınızı genişletin ve **CPUsers** OU'yu **seçin.**  **İleri**'ye tıklayın.
![Kapsam](media/tutorial-existing-forest/scope1.png)</br>
 9. **İsteğe Bağlı özellikler** ekranında **İleri'yi**tıklatın.
 10. **Yapılandırmaya Hazır** ekranında **Yapılandırma'yı**tıklatın.
 11. Bu tamamlandıktan **sonra, Çıkış'ı**tıklatın. 

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

