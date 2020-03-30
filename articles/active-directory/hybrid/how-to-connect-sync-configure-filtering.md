---
title: 'Azure AD Connect eşitleme: Filtreleme yapılandırma | Microsoft Dokümanlar'
description: Azure AD Connect eşitlemesinde filtrelemenin nasıl yapılandırılabildiğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 880facf6-1192-40e9-8181-544c0759d506
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 983699dfbfe3e8fa332da4810d1514a11029077f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261105"
---
# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect Eşitleme: Filtrelemeyi yapılandırma
Filtreleme'yi kullanarak, şirket içi dizininizden Azure Etkin Dizini'nde (Azure AD) hangi nesnelerin görünebileceğini denetleyebilirsiniz. Varsayılan yapılandırma, yapılandırılan ormanlarda tüm etki alanlarındaki tüm nesneleri alır. Genel olarak, bu önerilen yapılandırmadır. Exchange Online ve Skype for Business gibi Office 365 iş yüklerini kullanan kullanıcılar, e-posta gönderip herkesi arayabilmek için eksiksiz bir Global Adres Listesi'nden yararlanır. Varsayılan yapılandırma ile, onlar Exchange veya Lync bir şirket içi uygulama ile olurdu aynı deneyime sahip olacaktır.

Ancak bazı durumlarda, varsayılan yapılandırmada bazı değişiklikler yapmanız gerekir. İşte bazı örnekler:

* [Çok Azure AD dizini topolojisini](plan-connect-topologies.md#each-object-only-once-in-an-azure-ad-tenant)kullanmayı planlıyorsunuz. Ardından, belirli bir Azure REKLAM dizinine hangi nesnelerin eşitleniyi olduğunu denetlemek için bir filtre uygulamanız gerekir.
* Azure veya Office 365 için bir pilot çalıştırın ve yalnızca Azure AD'deki bir kullanıcı alt kümesini istiyorsunuz. Küçük pilot, işlevselliği göstermek için tam bir Global Adres Listesi olması önemli değildir.
* Azure AD'de istemediğiniz birçok hizmet hesabınız ve diğer kişisel olmayan hesaplarınız vardır.
* Uyumluluk nedenleriyle şirket içinde hiçbir kullanıcı hesabını silmezsiniz. Sadece onları devre dışı bırak. Ancak Azure AD'de yalnızca etkin hesapların bulunmasını istersiniz.

Bu makalede, farklı filtreleme yöntemlerinin nasıl yapılandırılabildiğini kapsar.

> [!IMPORTANT]
> Microsoft, resmi olarak belgelenen eylemler dışında Azure AD Connect eşitlemesinin değiştirilmesini veya çalıştırılmasını desteklemez. Bu eylemlerden herhangi biri, Azure AD Connect eşitlemesinde tutarsız veya desteklenmeyen bir duruma neden olabilir. Sonuç olarak, Microsoft bu tür dağıtımlar için teknik destek sağlayamaz.

## <a name="basics-and-important-notes"></a>Temel bilgiler ve önemli notlar
Azure AD Connect eşitlemesinde, istediğiniz zaman filtreleme yi etkinleştirebilirsiniz. Dizin eşitlemesi varsayılan yapılandırmasıyla başlar ve ardından filtreleme yapılandırırsanız, filtrelenen nesneler artık Azure AD ile eşitlenmez. Bu değişiklik nedeniyle, Azure AD'de daha önce eşitlenmiş ancak daha sonra filtre uygulanmış tüm nesneler Azure AD'de silinir.

Filtrelemede değişiklik yapmaya başlamadan önce, henüz doğru olarak doğru olarak doğrulanmadığını yanlışlıkla dışa aktarmamak için [zamanlanan görevi devre dışı](#disable-the-scheduled-task) bırakdığınızdan emin olun.

Filtreleme aynı anda birçok nesneyi kaldırabildiği için, Azure AD'de herhangi bir değişiklik dışa aktarmaya başlamadan önce yeni filtrelerinizin doğru olduğundan emin olmak istersiniz. Yapılandırma adımlarını tamamladıktan sonra, dışa aktarmadan önce [doğrulama adımlarını](#apply-and-verify-changes) izlemenizi ve Azure AD'de değişiklik yapmanızı öneririz.

Birçok nesneyi kazara silmekten korunmak için "[yanlışlıkla silmesini önleme](how-to-connect-sync-feature-prevent-accidental-deletes.md)" özelliği varsayılan olarak açıktır. Filtreleme nedeniyle birçok nesneyi silerseniz (varsayılan olarak 500), silmelerin Azure AD'ye geçmesine izin vermek için bu makaledeki adımları izlemeniz gerekir.

Kasım 2015'ten önce bir yapı kullanıyorsanız[(1.0.9125),](reference-connect-version-history.md#1091250)filtre yapılandırmasında değişiklik yapar sanız ve parola karma eşitlemesini kullanırsanız, yapılandırmayı tamamladıktan sonra tüm parolaların tam eşitlesini tetiklemeniz gerekir. Parolatam eşitlemetetikleme ile ilgili adımlar için [bkz.](tshoot-connect-password-hash-synchronization.md#trigger-a-full-sync-of-all-passwords) Build 1.0.9125 veya daha sonra iseniz, normal **tam eşitleme** eylemi parolaların eşitlenip eşitlenmemesi gerektiğini ve bu ek adımın artık gerekli olup olmadığını da hesaplar.

Bir filtreleme hatası nedeniyle **kullanıcı** nesneleri Azure AD'de yanlışlıkla silinmişse, filtreleme yapılandırmalarınızı kaldırarak Azure AD'deki kullanıcı nesnelerini yeniden oluşturabilirsiniz. Ardından dizinlerinizi yeniden eşitleyebilirsiniz. Bu eylem, kullanıcıları Azure AD'deki geri dönüşüm kutusundan geri yüklenir. Ancak, diğer nesne türlerini silemezsiniz. Örneğin, yanlışlıkla bir güvenlik grubunu silerseniz ve bir kaynağı ACL için kullandıysanız, grup ve aCL'leri kurtarılamaz.

Azure AD Connect yalnızca bir zamanlar kapsamda olduğunu düşündüğü nesneleri siler. Azure AD'de başka bir eşitleme altyapısı tarafından oluşturulan nesneler varsa ve bu nesneler kapsam içinde değilse, filtreleme ekleme bunları kaldırmaz. Örneğin, Azure AD'de dizininizin tamamının tam bir kopyasını oluşturan bir DirSync sunucusuyla başlarsanız ve başlangıçtan itibaren etkinleştirilmiş filtrelemeyle paralel olarak yeni bir Azure AD Connect eşitleme sunucusu yüklerseniz, Azure AD Connect ekstra nesneleri kaldırmaz DirSync tarafından oluşturulur.

Azure AD Connect'in daha yeni bir sürümünü yüklediğinizde veya yükselttidiğinizde filtreleme yapılandırması korunur. Yapılandırmanın ilk eşitleme döngüsünü çalıştırmadan önce yeni bir sürüme yükseltmeden sonra yanlışlıkla değiştirilmediğini doğrulamak her zaman en iyi yöntemdir.

Birden fazla ormanın varsa, bu konuda açıklanan filtreleme yapılandırmalarını her ormana uygulamanız gerekir (hepsi için aynı yapılandırmayı istediğinizi varsayarsak).

### <a name="disable-the-scheduled-task"></a>Zamanlanan görevi devre dışı
Her 30 dakikada bir eşitleme döngüsünü tetikleyen yerleşik zamanlayıcıyı devre dışı katmak için aşağıdaki adımları izleyin:

1. PowerShell istemine gidin.
2. Zamanlayıcıyı devre dışı kakmak için çalıştırın. `Set-ADSyncScheduler -SyncCycleEnabled $False`
3. Bu makalede belgelenen değişiklikleri yapın.
4. Zamanlayıcıyı yeniden etkinleştirmek için çalıştırın. `Set-ADSyncScheduler -SyncCycleEnabled $True`

**1.1.105.0'dan önce bir Azure AD Connect yapısı kullanıyorsanız**  
Her üç saatte bir eşitleme döngüsünü tetikleyen zamanlanmış görevi devre dışı bırakabilmek için aşağıdaki adımları izleyin:

1. **Başlat** menüsünden **Görev Zamanlayıcısı'nı** başlatın.
2. Doğrudan **Görev Zamanlayıcısı Kitaplığı**altında, **Azure AD Eşitleme Zamanlayıcısı**adlı görevi bulun , sağ tıklatın ve **Devre Dışı' yı**seçin.  
   ![Görev Zamanlayıcısı](./media/how-to-connect-sync-configure-filtering/taskscheduler.png)  
3. Artık yapılandırma değişiklikleri yapabilir ve eşitleme altyapısını **Eşitleme Service Manager** konsolundan el ile çalıştırabilirsiniz.

Tüm filtreleme değişikliklerinizi tamamladıktan sonra, geri gelmeyi ve görevi yeniden **etkinleştirmeyi** unutmayın.

## <a name="filtering-options"></a>Filtreleme seçenekleri
Dizin eşitleme aracına aşağıdaki filtreleme yapılandırma türlerini uygulayabilirsiniz:

* [**Grup Tabanlı**](#group-based-filtering): Tek bir gruba dayalı filtreleme, yalnızca yükleme sihirbazı kullanılarak ilk yüklemede yapılandırılabilir.
* [**Etki Alanı Tabanlı**](#domain-based-filtering): Bu seçeneği kullanarak, hangi etki alanlarının Azure AD ile eşitlediğinizi seçebilirsiniz. Azure AD Connect eşitlemeyi yükledikten sonra şirket içi altyapınızda değişiklik yaptığınızda, eşitleme motoru yapılandırmasından etki alanları ekleyebilir ve kaldırabilirsiniz.
* [**Kuruluş birimi (OU)-tabanlı**](#organizational-unitbased-filtering): Bu seçeneği kullanarak, hangi OS'ların Azure AD ile eşitleleştiğini seçebilirsiniz. Bu seçenek, seçili OSB'lerde tüm nesne türleri içindir.
* [**Öznitelik tabanlı**](#attribute-based-filtering): Bu seçeneği kullanarak, nesnelerdeki öznitelik değerlerini temel alarak nesneleri filtreleyebilirsiniz. Ayrıca, farklı nesne türleri için farklı filtrelerde de olabilir.

Aynı anda birden çok filtreleme seçeneği kullanabilirsiniz. Örneğin, OU tabanlı filtrelemayı yalnızca bir OU'ya nesneleri eklemek için kullanabilirsiniz. Aynı zamanda, nesneleri daha fazla filtrelemek için öznitelik tabanlı filtreleme kullanabilirsiniz. Birden çok filtreleme yöntemi kullandığınızda, filtreler filtreler arasında mantıksal bir "AND" kullanır.

## <a name="domain-based-filtering"></a>Etki alanı tabanlı filtreleme
Bu bölümde etki alanı filtrenizi yapılandırmak için adımlar sağlar. Azure AD Connect'i yükledikten sonra ormanınıza etki alanları eklediyseniz veya kaldırdıysanız, filtreleme yapılandırmasını da güncelleştirmeniz gerekir.

Etki alanı tabanlı filtrelemayı değiştirmenin tercih edilen yolu yükleme sihirbazını çalıştırmak ve [etki alanını ve OU filtrelemi'yi](how-to-connect-install-custom.md#domain-and-ou-filtering)değiştirmektir. Yükleme sihirbazı, bu konuda belgelenen tüm görevleri otomatikleştirir.

Yükleme sihirbazını herhangi bir nedenle çalıştıramıyorsanız, yalnızca aşağıdaki adımları izlemeniz gerekir.

Etki alanı tabanlı filtreleme yapılandırması aşağıdaki adımlardan oluşur:

1. Eşitleme eklemeye eklemek istediğiniz etki alanlarını seçin.
2. Eklenen ve kaldırılan her etki alanı için çalışan profillerini ayarlayın.
3. [Değişiklikleri uygulayın ve doğrulayın.](#apply-and-verify-changes)

### <a name="select-the-domains-to-be-synchronized"></a>Senkronize edilecek etki alanlarını seçin
Eşitlenecek etki alanlarını seçmenin iki yolu vardır:
    - Eşitleme Hizmetini Kullanma
    - Azure AD Connect sihirbazını kullanma.


#### <a name="select-the-domains-to-be-synchronized-using-the-synchronization-service"></a>Eşitleme Hizmetini kullanarak senkronize edilecek etki alanlarını seçin
Etki alanı filtresini ayarlamak için aşağıdaki adımları yapın:

1. **ADSyncAdmins** güvenlik grubunun bir üyesi olan bir hesabı kullanarak Azure AD Connect eşitlemi çalıştıran sunucuda oturum açın.
2. **Eşitleme Hizmetini** **Başlat** menüsünden başlatın.
3. **Bağlayıcılar'ı**seçin ve **Bağlayıcılar** listesinde **Active Directory Domain Services**türüne sahip Bağlayıcı'yı seçin. **Eylemler'de**, **Özellikler'i**seçin.  
   ![Konektör özellikleri](./media/how-to-connect-sync-configure-filtering/connectorproperties.png)  
4. **Dizin Bölümlerini Yapıla'yı**tıklatın.
5. **Dizin bölümleri** seç listesinde, etki alanlarını gerektiği gibi seçin ve seçin. Yalnızca eşitlemek istediğiniz bölümlerin seçildiğini doğrulayın.  
   ![Bölümler](./media/how-to-connect-sync-configure-filtering/connectorpartitions.png)  
   Şirket içi Active Directory altyapınızı değiştirdiyseniz ve ormandan etki alanları eklediyseniz veya kaldırdıysanız, güncelleştirilmiş bir liste almak için **Yenile** düğmesini tıklatın. Yenilediğinizde, kimlik bilgileri istenir. Windows Server Active Directory'ye okuma erişimi olan herhangi bir kimlik belgesi sağlayın. İletişim kutusunda önceden doldurulmuş kullanıcı olması gerekmez.  
   ![Gerekli yenileme](./media/how-to-connect-sync-configure-filtering/refreshneeded.png)  
6. İşi bittiğinde, **Tamam'ı**tıklatarak **Özellikler** iletişim kutusunu kapatın. Etki alanlarını ormandan kaldırdıysanız, bir ileti açılır penceresi bir etki alanının kaldırıldığını ve yapılandırmanın temizleneceğini söyler.
7. Çalışma profillerini ayarlamaya devam edin.

#### <a name="select-the-domains-to-be-synchronized-using-the-azure-ad-connect-wizard"></a>Azure AD Connect sihirbazını kullanarak senkronize edilecek etki alanlarını seçin
Etki alanı filtresini ayarlamak için aşağıdaki adımları yapın:

1.  Azure AD Bağlantı sihirbazını başlatın
2.  **Yapılandır**'a tıklayın.
3.  **Eşitleme Seçeneklerini Özelleştir'i** seçin ve **İleri'yi**tıklatın.
4.  Azure AD kimlik bilgilerinizi girin
5.  Bağlı **Dizinler** ekranında **İleri'yi**tıklatın.
6.  Etki **Alanı ve OU filtreleme sayfasında** **Yenile'yi**tıklatın.  Yeni etki alanları şimdi görünür ve silinen etki alanları kaybolur.
   ![Bölümler](./media/how-to-connect-sync-configure-filtering/update2.png)  

### <a name="update-the-run-profiles"></a>Çalışma profillerini güncelleştirme
Etki alanı filtrenizi güncellediyseniz, çalışan profillerini de güncelleştirmeniz gerekir.

1. **Bağlayıcılar** listesinde, önceki adımda değiştirdiğiniz Bağlayıcı'nın seçildiğinden emin olun. **Eylemler'de,** Çalıştır **Profillerini Yapılandır'ı**seçin.  
   ![Konektör çalıştırma profilleri 1](./media/how-to-connect-sync-configure-filtering/connectorrunprofiles1.png)  
2. Aşağıdaki profilleri bulun ve tanımlayın:
    * Tam İçeri Aktarma
    * Tam Eşitleme
    * Delta İçeri Aktarma
    * Delta Eşitleme
    * Dışarı Aktarma
3. Her profil için **eklenen** ve **kaldırılan** etki alanlarını ayarlayın.
    1. Beş profilin her biri **için, eklenen** her etki alanı için aşağıdaki adımları yapın:
        1. Çalıştır profilini seçin ve **Yeni Adım'ı**tıklatın.
        2. **YapılAşı adım** sayfasında, Açılan **Yazı türünde,** yapılandırıyorolduğunuz profille aynı ada sahip adım türünü seçin. Ardından **İleri**’ye tıklayın.  
        ![Konektör çalıştırma profilleri 2](./media/how-to-connect-sync-configure-filtering/runprofilesnewstep1.png)  
        3. Bağlayıcı **Yapılandırma** sayfasında, **Bölüm** açılır menüsünde etki alanı filtrenize eklediğiniz etki alanının adını seçin.  
        ![Konektör çalıştırma profilleri 3](./media/how-to-connect-sync-configure-filtering/runprofilesnewstep2.png)  
        4. **Profili Yapılandır'ı** kapatmak için **Bitiş'i**tıklatın.
    2. Beş profilin her biri **için, kaldırılan** her etki alanı için aşağıdaki adımları yapın:
        1. Çalışma profilini seçin.
        2. **Bölüm** özniteliğinin **Değeri** GUID ise, çalıştır adımını seçin ve **Adımı Sil'i**tıklatın.  
        ![Konektör çalıştırma profilleri 4](./media/how-to-connect-sync-configure-filtering/runprofilesdeletestep.png)  
    3. Değişikliğinizi doğrulayın. Eşitlemek istediğiniz her etki alanı, her çalışma profilinde bir adım olarak listelenmelidir.
4. **Yapılandırılan Profilleri** kapat'ı kapatmak için **Tamam'ı**tıklatın.
5.  Yapılandırmayı tamamlamak **için, Tam alma** ve **Delta eşitleme**çalıştırmanız gerekir. Bölümü okumaya devam [uygula ve değişiklikleri doğrulayın.](#apply-and-verify-changes)

## <a name="organizational-unitbased-filtering"></a>Kuruluş birimi tabanlı filtreleme
OU tabanlı filtreleme değiştirmek için tercih edilen yolu yükleme sihirbazı çalışan ve [etki alanı ve OU filtreleme değiştirerek.](how-to-connect-install-custom.md#domain-and-ou-filtering) Yükleme sihirbazı, bu konuda belgelenen tüm görevleri otomatikleştirir.

Yükleme sihirbazını herhangi bir nedenle çalıştıramıyorsanız, yalnızca aşağıdaki adımları izlemeniz gerekir.

Kuruluş birimi tabanlı filtrelemayı yapılandırmak için aşağıdaki adımları yapın:

1. **ADSyncAdmins** güvenlik grubunun bir üyesi olan bir hesabı kullanarak Azure AD Connect eşitlemi çalıştıran sunucuda oturum açın.
2. **Eşitleme Hizmetini** **Başlat** menüsünden başlatın.
3. **Bağlayıcılar'ı**seçin ve **Bağlayıcılar** listesinde **Active Directory Domain Services**türüne sahip Bağlayıcı'yı seçin. **Eylemler'de**, **Özellikler'i**seçin.  
   ![Konektör özellikleri](./media/how-to-connect-sync-configure-filtering/connectorproperties.png)  
4. **Dizin Bölümlerini Yapıla'yı**tıklatın, yapılandırmak istediğiniz etki alanını seçin ve ardından **Kapsayıcılar'ı**tıklatın.
5. Sizden istendiğinde, şirket içi Active Directory'nize okuma erişimi sağlayan herhangi bir kimlik bilgileri sağlayın. İletişim kutusunda önceden doldurulmuş kullanıcı olması gerekmez.
6. **Kapsayıcıları Seç** iletişim kutusunda, bulut diziniyle eşitlemek istemediğiniz OS'leri temizleyin ve **ardından Tamam'ı**tıklatın.  
   ![Kapsayıcıları Seç iletişim kutusundaki OS'ler](./media/how-to-connect-sync-configure-filtering/ou.png)  
   * Windows 10 bilgisayarlarınızın Azure AD ile başarıyla senkronize edilememesi için **Bilgisayarlar** kapsayıcısı seçilmelidir. Etki alanına katılan bilgisayarlarınız diğer İŞ Su'larında bulunuyorsa, bunların seçildiğinden emin olun.
   * Güvenler içeren birden çok ormanınız varsa, **ForeignSecurityPrincipals** kapsayıcısı seçili olmalıdır. Bu kapsayıcı, ormanlar arası güvenlik grubu üyeliğinin çözümlenebilmesine olanak tanır.
   * Aygıt geri yazma özelliğini etkinleştirdiyseniz, **RegisteredDevices** OU seçilmelidir. Grup geri yazma gibi başka bir geri yazma özelliği kullanıyorsanız, bu konumların seçildiğinden emin olun.
   * Kullanıcıların, iNetOrgPersons'ların, Grupların, Kişilerin ve Bilgisayarların bulunduğu diğer OU'ları seçin. Resimde, tüm bu OU'lar ManagedObjects OU'da yer alır.
   * Grup tabanlı filtreleme kullanıyorsanız, grubun bulunduğu OU'nun eklenmesi gerekir.
   * Filtreleme yapılandırması bittikten sonra eklenen yeni OS'lerin eşitlenip eşitlenmediğini yapılandırabileceğinizi unutmayın. Ayrıntılar için sonraki bölüme bakın.
7. İşi bittiğinde, **Tamam'ı**tıklatarak **Özellikler** iletişim kutusunu kapatın.
8. Yapılandırmayı tamamlamak **için, Tam alma** ve **Delta eşitleme**çalıştırmanız gerekir. Bölümü okumaya devam [uygula ve değişiklikleri doğrulayın.](#apply-and-verify-changes)

### <a name="synchronize-new-ous"></a>Yeni OS'leri senkronize etme
Filtreleme yapılandırıldıktan sonra oluşturulan yeni OR'lar varsayılan olarak eşitlenir. Bu durum seçili bir onay kutusu yla gösterilir. Ayrıca bazı alt OS'ler seçeneğini de kaldırabilirsiniz. Bu davranışı elde etmek için, mavi onay işareti (varsayılan durumu) ile beyaz olana kadar kutuyu tıklatın. Ardından eşitlemek istemediğiniz alt OS'leri seçin.

Tüm alt OS'ler eşitlenmişse, kutu mavi onay işaretiyle beyazdır.  
![Tüm kutuları seçili OU](./media/how-to-connect-sync-configure-filtering/ousyncnewall.png)

Bazı alt OS'ler seçilmemişse, kutu beyaz onay işaretiyle gri renktedir.  
![OU bazı alt-OUs seçili olmayan](./media/how-to-connect-sync-configure-filtering/ousyncnew.png)

Bu yapılandırma ile Yönetilen Nesneler altında oluşturulan yeni bir OU eşitlenir.

Azure AD Connect yükleme sihirbazı her zaman bu yapılandırmayı oluşturur.

### <a name="dont-synchronize-new-ous"></a>Yeni OS'leri senkronize etmeyin
Filtreleme yapılandırması bittikten sonra eşitleme altyapısını yeni OS'leri eşitlememek için yapılandırabilirsiniz. Bu durum, ui'de işaretsiz düz gri görünen kutu yla gösterilir. Bu davranışı elde etmek için, onay işareti olmadan beyaz olana kadar kutuyu tıklatın. Ardından eşitlemek istediğiniz alt OSB'leri seçin.

![KÖKÜ seçilmemiş OU](./media/how-to-connect-sync-configure-filtering/oudonotsyncnew.png)

Bu yapılandırmaile Yönetilen Nesneler altında oluşturulan yeni bir OU eşitlenmez.

## <a name="attribute-based-filtering"></a>Öznitelik tabanlı filtreleme
Kasım 2015'i[(1.0.9125)](reference-connect-version-history.md#1091250)kullandığınızdan veya bu adımların çalışması için daha sonra oluşturduğundan emin olun.

> [!IMPORTANT]
>Microsoft, **Azure AD Connect**tarafından oluşturulan varsayılan kuralları değiştirmemenizi önerir. Kuralı değiştirmek istiyorsanız, kuralı kopyalayın ve özgün kuralı devre dışı edin. Klonlanan kuralda herhangi bir değişiklik yapın. Bunu yaparak (orijinal kuralı devre dışı bırakarak) bu kural aracılığıyla etkinleştirilen hata düzeltmelerini veya özelliklerini kaçıracağınızı lütfen unutmayın.

Öznitelik tabanlı filtreleme, nesneleri filtrelemenin en esnek yoludur. Bir nesneAzure AD ile senkronize edildiğinde hemen hemen her yönünü denetlemek için [bildirimsel sağlama](concept-azure-ad-connect-sync-declarative-provisioning.md) gücünü kullanabilirsiniz.

Active Directory'den metaverse'e [gelen](#inbound-filtering) filtreleme ve metaverse'den Azure AD'ye [giden](#outbound-filtering) filtreleme uygulayabilirsiniz. Bakımı en kolay olduğu için gelen filtreleme uygulamanızı öneririz. Değerlendirmenin yapılabilmesi için yalnızca birden fazla ormandaki nesnelere katılmak gerekiyorsa giden filtreleme kullanmanız gerekir.

### <a name="inbound-filtering"></a>Gelen filtreleme
Gelen filtreleme, Azure AD'ye giden nesnelerin eşitlenecek bir değere ayarlanmayan metaverse özniteliği buluta sahip olması gereken varsayılan yapılandırmayı kullanır. Bu özniteliğin değeri **True**olarak ayarlanmışsa, nesne eşitlenmez. Bu **False**olarak ayarlanamamalıdır , tasarım gereği. Diğer kuralların bir değer eve katkıda bulunma yeteneğine sahip olduğundan emin olmak için, bu öznitelik yalnızca **True** veya **NULL** (yok) değerlerine sahip olması gerekir.

Gelen filtrelemede, eşitleme yapacak veya eşitlemeyecek nesneleri belirlemek için **kapsam** gücünü kullanırsınız. Burası, kendi kuruluşunuzun gereksinimlerine uyacak ayarlamalar yaptığınız yerdir. Kapsam modülünde eşitleme kuralının kapsamı ne zaman olduğunu belirlemek için bir **grup** ve yan **tümce** vardır. Bir grup bir veya çok sayıda yan tümce içerir. Birden çok yan tümce arasında mantıksal bir "VE" ve birden çok grup arasında mantıksal bir "OR" vardır.

Bir örneğe bakalım:  
![Kapsam](./media/how-to-connect-sync-configure-filtering/scope.png)  
Bu **(bölüm = IT) VEYA (bölüm = Satış VE c = ABD)** olarak okunmalıdır.

Aşağıdaki örneklerde ve adımlarda, kullanıcı nesnesini örnek olarak kullanırsınız, ancak bunu tüm nesne türleri için kullanabilirsiniz.

Aşağıdaki örneklerde öncelik değeri 50 ile başlar. Bu kullanılmayan herhangi bir sayı olabilir, ancak 100'den düşük olmalıdır.

#### <a name="negative-filtering-do-not-sync-these"></a>Negatif filtreleme: "Bunları eşitlemeyin"
Aşağıdaki örnekte, **uzantısıAttribute15** değeri **NoSync**sahip tüm kullanıcılar filtre (eşitlemek değil) .

1. **ADSyncAdmins** güvenlik grubunun bir üyesi olan bir hesabı kullanarak Azure AD Connect eşitlemi çalıştıran sunucuda oturum açın.
2. **Eşitleme** **Kuralları Düzenleyicisini** Başlat menüsünden başlatın.
3. **Gelen'in seçildiğinden** emin olun ve **Yeni Kural Ekle'yi**tıklatın.
4. Kurala " IN from AD –*User DoNotSyncFilter*" gibi açıklayıcı bir ad verin. Doğru ormanı seçin, **CS nesne türü**olarak **Kullanıcı'yı** seçin ve **MV nesne türü**olarak **Kişi'yi** seçin. **Bağlantı Türü'nde**Katıl'ı **seçin.** **Öncelik'te,** şu anda başka bir eşitleme kuralı (örneğin 50) tarafından kullanılmayan bir değer yazın ve sonra **İleri'yi**tıklatın.  
   ![Gelen 1 açıklama](./media/how-to-connect-sync-configure-filtering/inbound1.png)  
5. **Kapsam filtresinde**Grup **Ekle'yi**tıklatın ve **Yan tümce ekle'yi**tıklatın. **Öznitelik**olarak , **ExtensionAttribute15**seçin. **Operatör'ün** **EQUAL**olarak ayarlandıklarına emin olun ve **Değer** kutusuna **NoSync** değerini yazın. **İleri**'ye tıklayın.  
   ![Gelen 2 kapsam](./media/how-to-connect-sync-configure-filtering/inbound2.png)  
6. **Birleştirme** kurallarını boş bırakın ve **sonra İleri'yi**tıklatın.
7. **Dönüşüm Ekle'yi**tıklatın, **FlowType'ı Sabit**olarak seçin ve Hedef **Öznitelik**olarak **cloudFiltered'ı** seçin. **FlowType** **Kaynak** metin kutusuna **True**yazın. Kuralı kaydetmek için **Ekle'yi** tıklatın.  
   ![Gelen 3 dönüşümü](./media/how-to-connect-sync-configure-filtering/inbound3.png)
8. Yapılandırmayı tamamlamak için **Tam eşitleme**çalıştırmanız gerekir. Bölümü okumaya devam [uygula ve değişiklikleri doğrulayın.](#apply-and-verify-changes)

#### <a name="positive-filtering-only-sync-these"></a>Pozitif filtreleme: "yalnızca bunları eşitle"
Konferans odaları gibi senkronize edilmesi açık olmayan nesneleri de göz önünde bulundurmanız gerektiği için pozitif filtrelemeyi ifade etmek daha zor olabilir. Ayrıca, **AD - User Join'ten**gelen kutu dan gelen varsayılan filtreyi geçersiz kılacaksınız. Özel filtrenizi oluştururken, kritik sistem nesnelerini, çoğaltma çakışma nesnelerini, özel posta kutularını ve Azure AD Connect hizmet hesaplarını eklemediğinizden emin olun.

Pozitif filtreleme seçeneği iki eşitleme kuralı gerektirir. Eşitlemek için nesnelerin doğru kapsamı ile bir kural (veya birkaç) gerekir. Ayrıca, henüz eşitlenmeleri gereken bir nesne olarak tanımlanmamış tüm nesneleri filtreleyen ikinci bir catch-all eşitleme kuralına da ihtiyacınız vardır.

Aşağıdaki örnekte, yalnızca bölüm özniteliğinin **Satış**değerine sahip olduğu kullanıcı nesnelerini eşitlersiniz.

1. **ADSyncAdmins** güvenlik grubunun bir üyesi olan bir hesabı kullanarak Azure AD Connect eşitlemi çalıştıran sunucuda oturum açın.
2. **Eşitleme** **Kuralları Düzenleyicisini** Başlat menüsünden başlatın.
3. **Gelen'in seçildiğinden** emin olun ve **Yeni Kural Ekle'yi**tıklatın.
4. Kurala " IN from AD –*User Sales sync*" gibi açıklayıcı bir ad verin. Doğru ormanı seçin, **CS nesne türü**olarak **Kullanıcı'yı** seçin ve **MV nesne türü**olarak **Kişi'yi** seçin. **Bağlantı Türü'nde**Katıl'ı **seçin.** **Öncelik'te,** şu anda başka bir eşitleme kuralı (örneğin 51) tarafından kullanılmayan bir değer yazın ve sonra **İleri'yi**tıklatın.  
   ![Gelen 4 açıklama](./media/how-to-connect-sync-configure-filtering/inbound4.png)  
5. **Kapsam filtresinde**Grup **Ekle'yi**tıklatın ve **Yan tümce ekle'yi**tıklatın. **Öznitelik**olarak , **bölüm**seçin . Işlecinin **EQUAL**olarak ayarlandıklarına emin olun ve **Değer** kutusuna **Satış** değerini yazın. **İleri**'ye tıklayın.  
   ![Gelen 5 kapsam](./media/how-to-connect-sync-configure-filtering/inbound5.png)  
6. **Birleştirme** kurallarını boş bırakın ve **sonra İleri'yi**tıklatın.
7. **Dönüşüm Ekle'yi**tıklatın, **Akış Türü**olarak **Sabit'i** seçin ve **Hedef Öznitelik**olarak **cloudFiltered'ı** seçin. **Kaynak** kutusuna **False**yazın. Kuralı kaydetmek için **Ekle'yi** tıklatın.  
   ![Gelen 6 dönüşümü](./media/how-to-connect-sync-configure-filtering/inbound6.png)  
   Bu, cloudFiltered'i açıkça **False**olarak ayarladığınız özel bir durumdur.
8. Şimdi catch-all senkronizasyon kuralını oluşturmalıyız. Kurala " IN from AD –*User Catch-all filtresi "* gibi açıklayıcı bir ad verin. Doğru ormanı seçin, **CS nesne türü**olarak **Kullanıcı'yı** seçin ve **MV nesne türü**olarak **Kişi'yi** seçin. **Bağlantı Türü'nde**Katıl'ı **seçin.** **Öncelik**olarak, şu anda başka bir Eşitleme Kuralı (örneğin 99) tarafından kullanılmayan bir değer yazın. Önceki eşitleme kuralından daha yüksek (daha düşük öncelik) bir öncelik değeri seçtiniz. Ancak, ek bölümleri eşitlemeye başlamak istediğinizde daha sonra daha fazla filtreleme eşitleme kuralı ekleyebilmeniz için bazı odayı da terk ettiniz. **İleri**'ye tıklayın.  
   ![Gelen 7 açıklama](./media/how-to-connect-sync-configure-filtering/inbound7.png)  
9. **Kapsam filtresini** boş bırakın ve **İleri'yi**tıklatın. Boş bir filtre kuralın tüm nesnelere uygulanacağı anlamına geldiğini gösterir.
10. **Birleştirme** kurallarını boş bırakın ve **sonra İleri'yi**tıklatın.
11. **Dönüşüm Ekle'yi**tıklatın, **Akış Türü**olarak **Sabit'i** seçin ve **Hedef Öznitelik**olarak **cloudFiltered'ı** seçin. **Kaynak** kutusuna **True**yazın. Kuralı kaydetmek için **Ekle'yi** tıklatın.  
    ![Gelen 3 dönüşümü](./media/how-to-connect-sync-configure-filtering/inbound3.png)  
12. Yapılandırmayı tamamlamak için **Tam eşitleme**çalıştırmanız gerekir. Bölümü okumaya devam [uygula ve değişiklikleri doğrulayın.](#apply-and-verify-changes)

Gerekirse, eşitleme daha fazla nesne eklemek ilk türde daha fazla kural oluşturabilirsiniz.

### <a name="outbound-filtering"></a>Giden filtreleme
Bazı durumlarda, filtrelemeyalnızca nesneler metaverse katıldıktan sonra yapmak gerekir. Örneğin, bir nesnenin eşitlenmeli mi olduğunu belirlemek için kaynak ormanındaki posta özniteliğine ve hesap ormanındaki userPrincipalName özniteliğine bakmak gerekebilir. Bu gibi durumlarda, giden kural üzerinde filtreleme oluşturun.

Bu örnekte, filtrelemi yalnızca posta ve kullanıcıAdı sona eren kullanıcılar @contoso.com eşitlenerek değiştirebilirsiniz:

1. **ADSyncAdmins** güvenlik grubunun bir üyesi olan bir hesabı kullanarak Azure AD Connect eşitlemi çalıştıran sunucuda oturum açın.
2. **Eşitleme** **Kuralları Düzenleyicisini** Başlat menüsünden başlatın.
3. **Kurallar Türü**altında, **Giden'i**tıklatın.
4. Kullandığınız Connect sürümüne bağlı olarak, **AAD out** adlı kuralı bulmak - Kullanıcı Join veya **Out To AAD - User Join SOAInAD**, ve **Edit'i**tıklatın .
5. Açılan pencerede, kuralın bir kopyasını oluşturmak için **Evet'i** yanıtlayın.
6. **Açıklama** sayfasında, **Önceliği** 50 gibi kullanılmayan bir değerle değiştirin.
7. Sol navigasyonda **Kapsam filtresini** tıklatın ve ardından **madde ekle'yi**tıklatın. **Öznitelik**olarak, **posta**seçin. **Operator'da** **ENDSWITH'i**seçin. **Değer'de**, ** \@contoso.com**yazın ve sonra **Ekle yan tümcesini**tıklatın. **Öznitelik**olarak , **select userPrincipalName**. **Operator'da** **ENDSWITH'i**seçin. **Değer**, ** \@contoso.com**türü .
8. **Kaydet**'e tıklayın.
9. Yapılandırmayı tamamlamak için **Tam eşitleme**çalıştırmanız gerekir. Bölümü okumaya devam [uygula ve değişiklikleri doğrulayın.](#apply-and-verify-changes)

## <a name="apply-and-verify-changes"></a>Değişiklikleri uygulama ve doğrulama
Yapılandırma değişikliklerinizi yaptıktan sonra, bunları sistemde zaten mevcut olan nesnelere uygulamanız gerekir. Ayrıca, eşitleme altyapısında şu anda olmayan nesnelerin işlenmesi (ve eşitleme altyapısının içeriğini doğrulamak için kaynak sistemi yeniden okuması gerekir) de olabilir.

**Etki alanı** veya **kuruluş birimi** filtreleme kullanarak yapılandırmayı değiştirdiyseniz, tam **alma**işlemi yapmanız gerekir, ardından **Delta eşitleme.**

**Öznitelik** filtreleme kullanarak yapılandırmayı değiştirdiyseniz, **Tam eşitleme**yapmanız gerekir.

Aşağıdaki adımları yapın:

1. **Eşitleme Hizmetini** **Başlat** menüsünden başlatın.
2. **Bağlayıcılar'ı**seçin. **Bağlayıcılar** listesinde, yapılandırma değişikliği ni daha önce yaptığınız Bağlayıcı'yı seçin. **Eylemler'de** **Çalıştır'ı**seçin.  
   ![Konektör çalıştır](./media/how-to-connect-sync-configure-filtering/connectorrun.png)  
3. **Run profillerinde,** önceki bölümde belirtilen işlemi seçin. İki eylemi çalıştırmanız gerekiyorsa, ilki bittikten sonra ikinciyi çalıştırın. **(Seçili** bağlayıcı için Durum **sütunu Boşta'dır.)**

Eşitlemeden sonra, tüm değişiklikler dışa aktarılmak üzere sahnelenir. Azure AD'deki değişiklikleri gerçekten yapmadan önce, tüm bu değişikliklerin doğru olduğunu doğrulamak istiyorsunuz.

1. Bir komut istemi başlatın `%ProgramFiles%\Microsoft Azure AD Sync\bin`ve ' ye gidin.
2. `csexport "Name of Connector" %temp%\export.xml /f:x` öğesini çalıştırın.  
   Bağlayıcının adı Eşitleme Hizmeti'ndedir. Azure AD için "contoso.com – AAD" adlı bir adı vardır.
3. `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` öğesini çalıştırın.
4. Artık Microsoft Excel'de incelenebilen export.csv adlı %geçici %'lik bir dosyanız var. Bu dosya, dışa aktarılmak üzere olan tüm değişiklikleri içerir.
5. Veri veya yapılandırmada gerekli değişiklikleri yapın ve dışa aktarılmak üzere olan değişiklikler beklediğiniz olana kadar bu adımları (İçe Aktarma, Eşitle ve Doğrula) yeniden çalıştırın.

Memnun olduğunuzda, değişiklikleri Azure AD'ye aktarın.

1. **Bağlayıcılar'ı**seçin. **Bağlayıcılar** listesinde Azure AD Bağlayıcısı'nı seçin. **Eylemler'de** **Çalıştır'ı**seçin.
2. **Çalıştır profillerinde** **Dışa Aktar'ı**seçin.
3. Yapılandırmanız değişirse, sayı yapılandırılan eşiğe (varsayılan 500 olarak) daha fazla olduğunda dışa aktarmada bir hata görürsünüz. Bu hatayı görürseniz, "[yanlışlıkla silmekten engeller](how-to-connect-sync-feature-prevent-accidental-deletes.md)" özelliğini geçici olarak devre dışı bmalısınız.

Şimdi zamanlayıcıyı tekrar etkinleştirme zamanı.

1. **Başlat** menüsünden **Görev Zamanlayıcısı'nı** başlatın.
2. Doğrudan **Görev Zamanlayıcısı Kitaplığı**altında, **Azure AD Eşitleme Zamanlayıcısı**adlı görevi bulun , sağ tıklatın ve **Etkinleştir'i**seçin.

## <a name="group-based-filtering"></a>Grup tabanlı filtreleme
Azure AD Connect'i ilk yüklediğinizde [özel yükleme](how-to-connect-install-custom.md#sync-filtering-based-on-groups)yi kullanarak grup tabanlı filtreleme yapılandırabilirsiniz. Yalnızca küçük bir nesne kümesinin eşitolmasını istediğiniz bir pilot dağıtım için tasarlanmıştır. Grup tabanlı filtrelemi devre dışı bettiğinizde, yeniden etkinleştirilemez. Özel bir yapılandırmada grup tabanlı filtreleme kullanmak *desteklenmez.* Yalnızca yükleme sihirbazını kullanarak bu özelliği yapılandırmak için desteklenir. Pilot unuzu tamamladığınızda, bu konudaki diğer filtreleme seçeneklerinden birini kullanın. Grup tabanlı filtreleme ile birlikte OU tabanlı filtreleme kullanırken, grubun ve üyelerinin bulunduğu OU(lar) dahil edilmelidir.

Birden çok AD ormanLarını eşitlerken, her AD bağlayıcısı için farklı bir grup belirterek grup tabanlı filtreleme yapılandırabilirsiniz. Bir REKLAMı olan bir ormanda bir kullanıcıyı eşitlemek istiyorsanız ve aynı kullanıcının diğer AD ormanlarında bir veya daha fazla karşılık gelen nesnesi varsa, kullanıcı nesnesinin ve karşılık gelen tüm nesnelerinin grup tabanlı filtreleme kapsamında olduğundan emin olmalısınız. Örnekler için:

* Bir ormanda, başka bir ormanda karşılık gelen Bir FSP (Yabancı Güvenlik Sorumlusu) nesnesi olan bir kullanıcınız vardır. Her iki nesne de grup tabanlı filtreleme kapsamında olmalıdır. Aksi takdirde, kullanıcı Azure AD ile eşitlenmez.

* Bir ormanda, başka bir ormanda karşılık gelen kaynak hesabı (örn. bağlantılı posta kutusu) olan bir kullanıcınız vardır. Ayrıca, kullanıcıyı kaynak hesabına bağlamak için Azure AD Connect'i yapılandırıldınız. Her iki nesne de grup tabanlı filtreleme kapsamında olmalıdır. Aksi takdirde, kullanıcı Azure AD ile eşitlenmez.

* Bir ormanda, başka bir ormanda karşılık gelen posta bağlantısı olan bir kullanıcınız vardır. Ayrıca, kullanıcıyı posta ilgilisiyle ilişkilendirecek şekilde Azure AD Connect'i yapılandırıldınız. Her iki nesne de grup tabanlı filtreleme kapsamında olmalıdır. Aksi takdirde, kullanıcı Azure AD ile eşitlenmez.


## <a name="next-steps"></a>Sonraki adımlar
- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.
- [Şirket içi kimliklerinizi Azure AD ile tümleştirme](whatis-hybrid-identity.md)hakkında daha fazla bilgi edinin.
