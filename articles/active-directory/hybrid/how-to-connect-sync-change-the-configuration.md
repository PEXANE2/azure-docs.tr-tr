---
title: 'Azure AD Connect eşitleme: Azure AD Connect eşitlemesinde yapılandırma değişikliği yapma'
description: Azure AD Connect eşitlemesinde yapılandırmada nasıl değişiklik yapacağınızı size sunar.
services: active-directory
author: billmath
manager: daveba
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d77882817934d5ad98f16965aeb9dc246931c495
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261170"
---
# <a name="azure-ad-connect-sync-make-a-change-to-the-default-configuration"></a>Azure AD Connect eşitleme: Varsayılan yapılandırmada değişiklik yapma
Bu makalenin amacı, Azure Etkin Dizini (Azure AD) Connect eşitlemesinde varsayılan yapılandırmada nasıl değişiklik yapacağınızı size sağlamaktır. Bazı yaygın senaryolar için adımlar sağlar. Bu bilgiyle, kendi yapılandırmanızda kendi iş kurallarınıza göre basit değişiklikler yapabilmeniz gerekir.

> [!WARNING]
> Varsayılan kutu dışı eşitleme kurallarında değişiklik yaparsanız, bu değişiklikler Azure AD Connect bir sonraki güncelleştirilmesinde üzerine yazılır ve beklenmeyen ve büyük olasılıkla istenmeyen eşitleme sonuçlarına neden olur.
>
> Varsayılan kutu dışı eşitleme kurallarının bir parmak izi vardır. Bu kurallarda bir değişiklik yaparsanız, parmak izi artık eşleşmiyor. Azure AD Connect'in yeni bir sürümü uygulamaya çalıştığınızda gelecekte sorunlar la karşılamayabilirsiniz. Yalnızca bu makalede açıklanan şekilde değişiklik yapın.

## <a name="synchronization-rules-editor"></a>Eşitleme Kuralları Düzenleyicisi
Eşitleme Kuralları Düzenleyicisi varsayılan yapılandırmayı görmek ve değiştirmek için kullanılır. **Azure AD Connect** grubunun altındaki **Başlat** menüsünde bulabilirsiniz.  
![Eşitleme Kuralı Düzenleyicisi ile başlat menüsü](./media/how-to-connect-sync-change-the-configuration/startmenu2.png)

Düzenleyiciyi açtığınızda, varsayılan kutu dışı kuralları görürsünüz.

![Eşitleme Kuralı Düzenleyicisi](./media/how-to-connect-sync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Editörde gezinme
Editörün üst kısmındaki açılır düşüşleri kullanarak, belirli bir kuralı hızla bulabilirsiniz. Örneğin, öznitelik proxyAdreslerinin dahil edildiği kuralları görmek istiyorsanız, açılır düşüşleri aşağıdakilerle değiştirebilirsiniz:  
![SRE filtreleme](./media/how-to-connect-sync-change-the-configuration/filtering.png)  
Filtrelemayı sıfırlamak ve yeni bir yapılandırma yüklemek için klavyede F5 tuşuna basın.

Sağ üstte **yeni kural ekle** düğmesi yer almaktadır. Kendi özel kuralınızı oluşturmak için bu düğmeyi kullanırsınız.

Altta seçili eşitleme kuralıüzerinde hareket etmek için düğmeler vardır. **Düzenle** ve **Sil,** yapmalarını beklediğiniz şeyi yapın. **Dışa aktarma** eşitleme kuralını yeniden oluşturmak için bir PowerShell komut dosyası üretir. Bu yordamla, eşitleme kuralını bir sunucudan diğerine taşıyabilirsiniz.

## <a name="create-your-first-custom-rule"></a>İlk özel kuralınızı oluşturun
En yaygın değişiklikler öznitelik akışları vardır. Kaynak dizininizdeki veriler Azure AD'dekiyle aynı olmayabilir. Bu bölümdeki örnekte, bir kullanıcının verilen adının her zaman *uygun durumda*olduğundan emin olun.

### <a name="disable-the-scheduler"></a>Zamanlayıcıyı devre dışı
[Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) varsayılan olarak her 30 dakikada bir çalışır. Değişiklikler yaparken ve yeni kurallarınızı sorun giderirken başlamadığından emin olun. Zamanlayıcıyı geçici olarak devre dışı kılmış `Set-ADSyncScheduler -SyncCycleEnabled $false`sayılsın, PowerShell'i başlatın ve çalıştırın.

![Zamanlayıcıyı devre dışı](./media/how-to-connect-sync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Kuralı oluşturma
1. **Yeni kural ekle'yi**tıklatın.
2. **Açıklama** sayfasında aşağıdakileri girin:  
   ![Gelen kural filtreleme](./media/how-to-connect-sync-change-the-configuration/description2.png)  
   * **Adı**: Kurala açıklayıcı bir ad verin.
   * **Açıklama**: Bir başkası kuralın ne için olduğunu anlayabilsin diye biraz açıklama verin.
   * **Bağlı Sistem**: Nesnenin bulunabileceği sistemdir. Bu durumda, **Active Directory Bağlayıcısı'nı**seçin.
   * **Bağlı Sistem/Metaverse Nesne Türü**: **Sırasıyla Kullanıcı** ve **Kişi'yi**seçin.
   * **Bağlantı Türü**: Bu değeri **Join**olarak değiştirin.
   * **Öncelik**: Sistemde benzersiz bir değer sağlar. Daha düşük bir sayısal değer daha yüksek önceliği gösterir.
   * **Etiket**: Bunu boş bırakın. Yalnızca Microsoft'un kutudan çıkan kuralları, bu kutunun bir değerle doldurulması gerekir.
3. Kapsam **filtresi sayfasına,** **givenName ISNOTNULL'u**girin.  
   ![Gelen kural kapsam filtresi](./media/how-to-connect-sync-change-the-configuration/scopingfilter.png)  
   Bu bölüm, kuralın hangi nesneleri uygulaması gerektiğini tanımlamak için kullanılır. Boş bırakılırsa, kural tüm kullanıcı nesneleri için geçerli olacaktır. Ancak, konferans odaları, hizmet hesapları ve diğer kişi olmayan kullanıcı nesneleri içerir.
4. Birleştirme **kuralları** sayfasında alanı boş bırakın.
5. **Dönüşümler** **sayfasında, Akış** **Türü'nden İfadeye**değiştirin. **Hedef Öznitelik** **için, givenName'yi**seçin. Ve **Kaynak**için, **PCase([givenName])** girin.
   ![Gelen kural dönüşümleri](./media/how-to-connect-sync-change-the-configuration/transformations.png)  
   Eşitleme altyapısı hem işlev adı hem de özniteliğin adı için büyük/küçük harf duyarlıdır. Yanlış bir şey yazarsanız, kuralı eklediğinizde bir uyarı görürsünüz. Kaydedebilir ve devam edebilirsiniz, ancak kuralı yeniden açmanız ve düzeltmeniz gerekir.
6. Kuralı kaydetmek için **Ekle'yi** tıklatın.

Yeni özel kuralınız, sistemdeki diğer eşitleme kurallarıyla birlikte görünür olmalıdır.

### <a name="verify-the-change"></a>Değişikliği doğrulama
Bu yeni değişiklikle, beklendiği gibi çalıştığından ve herhangi bir hata atmadığından emin olmak istersiniz. Sahip olduğunuz nesne sayısına bağlı olarak, bu adımı yapmanın iki yolu vardır:

- Tüm nesnelerüzerinde tam eşitleme çalıştırın.
- Tek bir nesne üzerinde önizleme ve tam eşitleme çalıştırın.

**Eşitleme** **Hizmetini** Başlat menüsünden açın. Bu bölümdeki adımların tümü bu araçtadır.

**Tüm nesnelerde tam eşitleme**  

   1. En üstteki **Bağlayıcıları** seçin. Önceki bölümde değiştirdiğiniz bağlayıcıyı (bu durumda, Etkin Dizin Etki Alanı Hizmetleri) belirleyin ve seçin. 
   2. **Eylemler**için **Çalıştır'ı**seçin.
   3. **Tam Eşitleme'yi**seçin ve ardından **Tamam'ı**seçin.
   ![Tam eşitleme](./media/how-to-connect-sync-change-the-configuration/fullsync.png)  
   Nesneler artık metaverse güncelleştirilir. Metaverse'deki nesneye bakarak değişikliklerinizi doğrulayın.

**Tek bir nesneüzerinde önizleme ve tam eşitleme**  

   1. En üstteki **Bağlayıcıları** seçin. Önceki bölümde değiştirdiğiniz bağlayıcıyı (bu durumda, Etkin Dizin Etki Alanı Hizmetleri) belirleyin ve seçin.
   2. **Arama Bağlayıcısı Alanı'nı**seçin. 
   3. Değişikliği sınamak için kullanmak istediğiniz bir nesneyi bulmak için **Kapsam'ı** kullanın. Nesneyi seçin ve **Önizleme'yi**tıklatın. 
   4. Yeni ekranda, **Önizlemeyi Edin'i**seçin.  
   ![Önizleme yi işleme](./media/how-to-connect-sync-change-the-configuration/commitpreview.png)  
   Değişiklik şimdi metaverse kararlıdır.

**Metaverse nesneyi görüntüleme**  

1. Değerin beklendiğinden ve kuralın uygulandığından emin olmak için birkaç örnek nesne seçin. 
2. Üstten **Metaverse Arama'yı** seçin. İlgili nesneleri bulmak için gereken herhangi bir filtre ekleyin. 
3. Arama sonucundan bir nesne açın. Öznitelik değerlerine bakın ve **Eşitleme Kuralları** sütununda kuralın beklendiği gibi uygulandığını da doğrulayın.  
![Meta veri deposu arama](./media/how-to-connect-sync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Zamanlayıcıyı etkinleştirme
Her şey beklendiği gibiyse, zamanlayıcıyı yeniden etkinleştirebilirsiniz. PowerShell'den `Set-ADSyncScheduler -SyncCycleEnabled $true`çalıştırın.

## <a name="other-common-attribute-flow-changes"></a>Diğer ortak öznitelik akışı değişiklikleri
Önceki bölümde, öznitelik akışında nasıl değişiklik yapıla Bu bölümde, bazı ek örnekler verilmiştir. Eşitleme kuralının nasıl oluşturulabildiğini anlatan adımlar kısaltılır, ancak önceki bölümde tam adımları bulabilirsiniz.

### <a name="use-an-attribute-other-than-the-default"></a>Varsayılan değer dışında bir öznitelik kullanma
Bu Fabrikam senaryosunda, verilen ad, soyad ve görüntü adı için yerel alfabenin kullanıldığı bir orman vardır. Bu özniteliklerin Latince karakter gösterimi uzantı öznitelikleri bulunabilir. Kuruluş, Azure AD ve Office 365'te genel bir adres listesi oluşturmak için bunun yerine bu öznitelikleri kullanmak istiyor.

Varsayılan yapılandırmayla, yerel ormandan bir nesne aşağıdaki gibi görünür:  
![Öznitelik akışı 1](./media/how-to-connect-sync-change-the-configuration/attributeflowjp1.png)

Diğer öznitelik akışları içeren bir kural oluşturmak için aşağıdakileri yapın:

1. **Başlat** menüsünden **Eşitleme Kuralları** Düzenleyicisi'ni açın.
2. **Gelen** hala solda seçilirken, **yeni kural ekle** düğmesini tıklatın.
3. Kurala bir ad ve açıklama verin. Şirket içi Etkin Dizin örneğini ve ilgili nesne türlerini seçin. **Bağlantı Türü'nde**Katıl'ı **seçin.** **Öncelik**için, başka bir kural tarafından kullanılmayan bir sayı seçin. Kutudan çıkma kuralları 100 ile başlar, böylece 50 değeri bu örnekte kullanılabilir.
  ![Öznitelik akışı 2](./media/how-to-connect-sync-change-the-configuration/attributeflowjp2.png)
4. **Kapsam filtresini** boş bırakın. (Diğer bir deyişle, ormandaki tüm kullanıcı nesneleri için geçerli olmalıdır.)
5. **Join kurallarını** boş bırakın. (Diğer bir şey, kutudan çıkma kuralının birleştirmeleri işlemesine izin verin.)
6. **Dönüşümlerde**aşağıdaki akışları oluşturun:  
  ![Öznitelik akışı 3](./media/how-to-connect-sync-change-the-configuration/attributeflowjp3.png)
7. Kuralı kaydetmek için **Ekle'yi** tıklatın.
8. **Eşitleme Hizmet Yöneticisi'ne**gidin. **Bağlayıcılar'da**kuralı eklediğiniz bağlayıcıyı seçin. **Çalıştır'ı**ve ardından **Tam Eşitleme'yi**seçin. Tam eşitleme, geçerli kuralları kullanarak tüm nesneleri yeniden hesaplar.

Bu, bu özel kurala sahip aynı nesnenin sonucudur:  
![Öznitelik akışı 4](./media/how-to-connect-sync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Özniteliklerin uzunluğu
Dize öznitelikleri varsayılan olarak dizine eklenebilir ve en büyük uzunluk 448 karakterdir. Daha fazlasını içerebilecek dize öznitelikleriyle çalışıyorsanız, öznitelik akışına aşağıdakileri eklediğinizden emin olun:  
`attributeName` <- `Left([attributeName],448)`.

### <a name="changing-the-userprincipalsuffix"></a>userPrincipalSuffix değiştirme
Active Directory'deki userPrincipalName özniteliği her zaman kullanıcılar tarafından bilinmemektedir ve oturum açma kimliği olarak uygun olmayabilir. Azure AD Connect eşitleme yükleme sihirbazı ile, posta gibi farklı bir öznitelik *seçebilirsiniz.* Ancak bazı durumlarda, öznitelik hesaplanmalıdır.

Örneğin, Contoso şirketinin biri üretim, diğeri test için olmak üzere iki Azure REKLAM dizini vardır. Test kiracılarındaki kullanıcıların oturum açma kimliğinde başka bir sonek kullanmasını isterler:  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`.

Bu ifadede, ilk @-sign (Word) kalan her şeyi almak ve sabit bir dize ile concatenate.

### <a name="convert-a-multi-value-attribute-to-single-value"></a>Çok değerli bir özniteliği tek değere dönüştürme
Active Directory'deki bazı öznitelikler, Etkin Dizin Kullanıcıları ve Bilgisayarlarında tek değerli görünseler bile şemada çok değerlidir. Bir örnek açıklama özniteliği:  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`.

Bu ifadede, öznitelik bir değer varsa, öznitelik ilk öğe *(Öğe)* almak, öncü ve sondaki boşlukları kaldırmak *(Kırpma*), ve sonra ilk 448 karakter *(Sol)* dize tutun.

### <a name="do-not-flow-an-attribute"></a>Bir öznitelik akışı etmeyin
Bu bölümün senaryosunda arka plan için [öznitelik akış işlemini denetle'ye](concept-azure-ad-connect-sync-declarative-provisioning.md#control-the-attribute-flow-process)bakın.

Bir öznitelik akışı değil iki yolu vardır. İlk [seçili öznitelikleri kaldırmak](how-to-connect-install-custom.md#azure-ad-app-and-attribute-filtering)için yükleme sihirbazı kullanarak. Bu seçenek, özniteliği daha önce hiç eşitlemediyseniz çalışır. Ancak, bu özniteliği eşitlemeye başladıysanız ve daha sonra bu özellik ile kaldırmaya başladıysanız, eşitleme altyapısı özniteliği yönetmeyi durdurur ve varolan değerler Azure AD'de bırakılır.

Bir öznitelik değerini kaldırmak ve gelecekte akmaz emin olmak istiyorsanız, özel bir kural oluşturmanız gerekir.

Bu Fabrikam senaryosunda, buluta senkronize ettiğimiz bazı özelliklerin orada olmaması gerektiğini fark ettik. Bu özniteliklerin Azure AD'den kaldırıldığından emin olmak istiyoruz.  
![Kötü uzantı öznitelikleri](./media/how-to-connect-sync-change-the-configuration/badextensionattribute.png)

1. Yeni bir gelen eşitleme kuralı oluşturun ve açıklamayı doldurun.
  ![Açıklama](./media/how-to-connect-sync-change-the-configuration/syncruledescription.png)
2. **FlowType** için **İfade** ve **Kaynak**için **AuthoritativeNull** ile öznitelik akışları oluşturun. Literal **AuthoritativeNull,** değeri doldurmaya çalışsa bile, değerin metaverse boş olması gerektiğini belirtir.
  ![Uzantı öznitelikleri için dönüştürme](./media/how-to-connect-sync-change-the-configuration/syncruletransformations.png)
3. Eşitleme kuralını kaydedin. **Eşitleme Hizmetini**başlatın, bağlayıcıyı bulun, **Çalıştır'ı**seçin ve ardından **Tam Eşitleme'yi**seçin. Bu adım, tüm öznitelik akışlarını yeniden hesaplar.
4. Bağlayıcı Alanı'nda arama yaparak amaçlanan değişikliklerin dışa aktarılmak üzere olduğunu doğrulayın.
  ![Aşamalı silme](./media/how-to-connect-sync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>PowerShell ile kurallar oluşturun
Eşitleme kuralı düzenleyicisini kullanmak, yalnızca birkaç değişiklik yaptığınızda iyi çalışır. Birçok değişiklik yapmanız gerekiyorsa, PowerShell daha iyi bir seçenek olabilir. Bazı gelişmiş özellikler yalnızca PowerShell ile kullanılabilir.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Hazır bir kural için PowerShell komut dosyasını alın
Kutudan çıkarma kuralını oluşturan PowerShell komut dosyasını görmek için eşitleme kuralları düzenleyicisindeki kuralı seçin ve **Dışa**Aktar'ı tıklatın. Bu eylem, kuralı oluşturan PowerShell komut dosyasını verir.

### <a name="advanced-precedence"></a>Gelişmiş öncelik
Kutudan çıkan eşitleme kuralları 100 öncelik değeriyle başlar. Çok sayıda ormanın varsa ve birçok özel değişiklik yapmanız gerekiyorsa, 99 eşitleme kuralı yeterli olmayabilir.

Eşitleme motoruna, kutudan çıkan kurallardan önce ek kurallar eklenmesini istediğinizi belirtebilirsiniz. Bu davranışı elde etmek için aşağıdaki adımları izleyin:

1. Eşitleme kuralları düzenleyicisinde ilk kutu dan eşitleme kuralını **(AD-User Join'ten**girin) işaretleyin ve **Dışa**Aktar'ı seçin. SR Tanımlayıcı değerini kopyalayın.  
![Değişikliköncesi PowerShell](./media/how-to-connect-sync-change-the-configuration/powershell1.png)  
2. Yeni eşitleme kuralını oluşturun. Bunu oluşturmak için eşitleme kuralları düzenleyicisini kullanabilirsiniz. Kuralı PowerShell komut dosyasına dışa aktarın.
3. **PrecedenceBefore**özelliğine, kutudan çıkma kuralından Tanımlayıcı değerini ekleyin. **Önceliği** **0**olarak ayarlayın. Tanımlayıcı özniteliğinin benzersiz olduğundan ve başka bir kuraldan guid'i yeniden kullanmadığınızdan emin olun. Ayrıca, Değişmez **Etiket** özelliğinin ayarlanmadığından emin olun. Bu özellik yalnızca kutudan çıkma kuralı için ayarlanmalıdır.
4. PowerShell komut dosyasını kaydedin ve çalıştırın. Sonuç olarak, özel kuralınız 100 öncelik değerine atanmış ve diğer tüm kutu dışı kurallar artmıştır.  
![Değişiklikten sonra PowerShell](./media/how-to-connect-sync-change-the-configuration/powershell2.png)  

Gerektiğinde aynı **Önce** önce değerini kullanarak birçok özel eşitleme kuralına sahip olabilirsiniz.

## <a name="enable-synchronization-of-usertype"></a>UserType senkronizasyonunu etkinleştirme
Azure AD Connect, 1.1.524.0 sürümünde ve sonraki sürümdeki **Kullanıcı** nesneleri için **UserType** özniteliğinin eşitleştirilmesini destekler. Daha spesifik olarak, aşağıdaki değişiklikler getirilmiştir:

- Azure AD Bağlayıcısı'ndaki nesne türü **Kullanıcı'nın** şeması, tür dizesinden olan ve tek değerli olan UserType özniteliğini içerecek şekilde genişletilir.
- Metaverse nesne türü **Kişi** şema türü dize olan ve tek değerli UserType özniteliği, içerecek şekilde genişletilir.

Şirket içi Active Directory'de karşılık gelen UserType özniteliği olmadığından, varsayılan olarak UserType özniteliği eşitleme için etkinleştirilir. Eşitlemesini el ile etkinleştirmelisiniz. Bunu yapmadan önce Azure AD tarafından uygulanan aşağıdaki davranışlara dikkat edin:

- Azure AD, UserType özniteliği için yalnızca iki değer kabul eder: **Üye** ve **Konuk.**
- Azure AD Connect'te UserType özniteliği eşitleme için etkinleştirilemezse, dizin eşitlemesi yoluyla oluşturulan Azure AD kullanıcıları, UserType özniteliğini **Üye'ye**ayarlar.
- Azure AD, mevcut Azure AD kullanıcılarındaki UserType özniteliğinin Azure AD Connect tarafından değiştirilmesine izin vermez. Yalnızca Azure AD kullanıcılarının oluşturulması sırasında ayarlanabilir ve [Powershell üzerinden değiştirilebilir.](/powershell/module/azuread/set-azureaduser?view=azureadps-2.0)

UserType özniteliğinin eşitlemesini etkinleştirmeden önce, öncelikle özniteliğin şirket içi Active Directory'den nasıl türetildikine karar vermeniz gerekir. En yaygın yaklaşımlar şunlardır:

- Kaynak özniteliği olarak kullanılmak üzere kullanılmayan bir şirket içi AD özniteliği (uzantıAttribute1 gibi) belirleyin. Belirlenen şirket içi AD özniteliği, tek değerli olması ve **Üye** veya **Konuk**değerini içeren tür **dizesinden**olmalıdır. 

    Bu yaklaşımı seçerseniz, Kullanıcı Türü özniteliğinin eşitlemesini etkinleştirmeden önce, atanan özniteliğin şirket içi Active Directory'deki tüm varolan kullanıcı nesneleri için doğru değerle dolduruldüğünden emin olmalısınız .

- Alternatif olarak, UserType özniteliğinin değerini diğer özelliklerden elde edebilirsiniz. Örneğin, şirket içi AD userPrincipalName özniteliği etki alanı bölümüyle <em>@partners.fabrikam123.org</em>biterse tüm kullanıcıları **Konuk** olarak senkronize etmek istiyorsunuz. 

    Azure AD Connect, daha önce de belirtildiği gibi, mevcut Azure AD kullanıcılarındaki UserType özniteliğinin Azure AD Connect tarafından değiştirilmesine izin vermez. Bu nedenle, karar verdiğiniz mantığın, UserType özniteliğinin kiracınızdaki tüm mevcut Azure AD kullanıcıları için zaten nasıl yapılandırıldığıyla tutarlı olduğundan emin olmalısınız.

UserType özniteliğinin eşitlemesini etkinleştirme adımları şu şekilde özetlenebilir:

1.  Eşitleme zamanlayıcısı'nı devre dışı bırakıp devam eden eşitleme olmadığını doğrulayın.
2.  Kaynak özniteliğini şirket içi AD Bağlayıcı şemasına ekleyin.
3.  UserType'ı Azure AD Bağlayıcı şemasına ekleyin.
4.  Öznitelik değerini şirket içi Active Directory'den akış için gelen bir eşitleme kuralı oluşturun.
5.  Öznitelik değerini Azure AD'ye aktetmek için giden bir eşitleme kuralı oluşturun.
6.  Tam bir eşitleme döngüsü çalıştırın.
7.  Eşitleme zamanlayıcısını etkinleştirin.

>[!NOTE]
> Bu bölümün geri kalanı bu adımları kapsar. Bunlar, tek orman topolojisi içeren ve özel eşitleme kuralları içermeyen bir Azure AD dağıtımı bağlamında açıklanmıştır. Çok ormanlı topolojiniz, özel eşitleme kurallarınınız yapılandırıldıysa veya bir evreleme sunucunuz varsa, adımları buna göre ayarlamanız gerekir.

### <a name="step-1-disable-the-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Adım 1: Eşitleme zamanlayıcısını devre dışı bırakıp devam eden eşitleme olmadığını doğrulayın
Azure AD'ye istenmeyen değişiklikler intelemeyi önlemek için, eşitleme kurallarını güncelleştirmenin ortasındayken eşitleme gerçekleşmediğinden emin olun. Yerleşik eşitleme zamanlayıcısını devre dışı kalmak için:

 1. Azure AD Connect sunucusunda bir PowerShell oturumu başlatın.
 2. Cmdlet `Set-ADSyncScheduler -SyncCycleEnabled $false`çalıştırarak zamanlanmış senkronizasyonu devre dışı bırakın.
 3. Eşitleme Hizmetini **Başlat'a** > giderek Eşitleme Hizmeti**Yöneticisi'ni**açın.
 4. **İşlemler** sekmesine gidin ve *devam eden*bir durumla hiçbir işlem olmadığını onaylayın.

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>Adım 2: Kaynak özniteliğini şirket içi AD Bağlayıcı şemasına ekleme
Tüm Azure AD öznitelikleri şirket içi AD Bağlayıcı Alanı'na alınmaz. Alınan öznitelikler listesine kaynak özniteliği eklemek için:

 1. Eşitleme Hizmeti Yöneticisi'ndeki **Bağlayıcılar** sekmesine gidin.
 2. Şirket içi AD Bağlayıcısı'na sağ tıklayın ve **Özellikler'i**seçin.
 3. Açılan iletişim kutusunda **Öznitelikleri Seç** sekmesine gidin.
 4. Kaynak özniteliğin öznitelik listesinde işaretli olduğundan emin olun.
 5. Kaydetmek için **Tamam'ı** tıklatın.
![Şirket içi AD Bağlayıcı şemasına kaynak özniteliği ekleme](./media/how-to-connect-sync-change-the-configuration/usertype1.png)

### <a name="step-3-add-the-usertype-to-the-azure-ad-connector-schema"></a>Adım 3: UserType'ı Azure AD Bağlayıcı şemasına ekleyin
Varsayılan olarak, UserType özniteliği Azure AD Bağlantı Alanı'na aktarılmaz. Kullanıcı Türü özniteliğini içe aktarılan öznitelikler listesine eklemek için:

 1. Eşitleme Hizmeti Yöneticisi'ndeki **Bağlayıcılar** sekmesine gidin.
 2. Azure AD **Bağlayıcısı'na** sağ tıklayın ve **Özellikler'i**seçin.
 3. Açılan iletişim kutusunda **Öznitelikleri Seç** sekmesine gidin.
 4. UserType özniteliğinin öznitelik listesinde işaretli olduğundan emin olun.
 5. Kaydetmek için **Tamam'ı** tıklatın.

![Azure AD Bağlayıcı şemasına kaynak özniteliği ekleme](./media/how-to-connect-sync-change-the-configuration/usertype2.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Adım 4: Şirket içi Active Directory'den öznitelik değerini akışiçin gelen bir eşitleme kuralı oluşturma
Gelen eşitleme kuralı öznitelik değerinin kaynak özyünden şirket içi Active Directory'den metaverse akmasına izin verir:

1. Eşitleme Kuralları Düzenleyicisini **Başlat'a** > giderek Eşitleme**Kuralları Düzenleyicisini**açın.
2. Arama filtresi **Yön'u** **Gelen**olacak şekilde ayarlayın.
3. Yeni bir gelen kuralı oluşturmak için **yeni kural ekle** düğmesini tıklatın.
4. **Açıklama** sekmesinin altında aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Adı | *Ad verme* | Örneğin, *AD 'den In – User UserType* |
    | Açıklama | *Açıklama sağlayın* |  |
    | Bağlı Sistem | *Şirket içi AD konektörünü seçin* |  |
    | Bağlı Sistem Nesne Türü | **Kullanıcı** |  |
    | Metaverse Nesne Türü | **Kişi** |  |
    | Bağlantı Türü | **Katıl** |  |
    | Önceliği | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmeyin. |

5. **Kapsam filtresi** sekmesine gidin ve aşağıdaki yan tümceyi içeren tek bir kapsam **filtresi grubu** ekleyin:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Kullanıcı\_ |

    Kapsam filtresi, şirket içi AD'nin bu gelen eşitleme kuralını hangi lerine karşı caydıracağı belirlenir. Bu örnekte, In *from AD - User Common* out-of---box eşitleme kuralında kullanılan ve eşitleme kuralının Azure AD Kullanıcı yazma özelliği aracılığıyla oluşturulan Kullanıcı nesnelerine uygulanmasını engelleyen aynı kapsam filtresini kullanırız. Kapsam filtresini Azure AD Connect dağıtımınıza göre değiştirmeniz gerekebilir.

6. **Dönüşüm** sekmesine gidin ve istenen dönüşüm kuralını uygulayın. Örneğin, UserType'ın kaynak özniteliği olarak kullanılmayan bir şirket içi AD özniteliği (uzantıÖzÖz1 gibi) belirlediyseniz, doğrudan öznitelik akışı uygulayabilirsiniz:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez uygulayın | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Direct | UserType | extensionAttribute1 | Denetlenme -yen | Güncelleştirme |

    Başka bir örnekte, UserType özniteliğinin değerini diğer özelliklerden türetmek istiyorsunuz. Örneğin, şirket içi AD userPrincipalName özniteliği etki alanı bölümüyle <em>@partners.fabrikam123.org</em>biterse tüm kullanıcıları Konuk olarak senkronize etmek istiyorsunuz. Şu şekilde bir ifade uygulayabilirsiniz:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez uygulayın | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | İfadeler | UserType | IIF(IsPresent([userPrincipalName]),IIF(CBool(InStr(LCase(LCase([userPrincipalName]), "@partners.fabrikam123.org"=0),"Üye","Konuk"),Hata("UserPrincipalName UserType belirlemek için mevcut değildir")) | Denetlenme -yen | Güncelleştirme |

7. Gelen kuralı oluşturmak için **Ekle'yi** tıklatın.

![Gelen eşitleme kuralı oluşturma](./media/how-to-connect-sync-change-the-configuration/usertype3.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Adım 5: Öznitelik değerini Azure AD'ye aktatmak için giden bir eşitleme kuralı oluşturma
Giden eşitleme kuralı, öznitelik değerinin metaverse'den Azure AD'deki UserType özniteliğine akmasına izin verir:

1. Eşitleme Kuralları Düzenleyicisi'ne gidin.
2. Arama filtresi **Yön'u** **Giden**olacak şekilde ayarlayın.
3. Yeni **kural ekle** düğmesini tıklatın.
4. **Açıklama** sekmesinin altında aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | ----- | ------ | --- |
    | Adı | *Ad verme* | Örneğin, *AAD out - Kullanıcı UserType* |
    | Açıklama | *Açıklama sağlayın* ||
    | Bağlı Sistem | *AAD konektörünü seçin* ||
    | Bağlı Sistem Nesne Türü | **Kullanıcı** ||
    | Metaverse Nesne Türü | **Kişi** ||
    | Bağlantı Türü | **Katıl** ||
    | Önceliği | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmeyin. |

5. **Kapsam filtresi** sekmesine gidin ve iki yan tümceiçeren tek bir kapsam **filtresi grubu** ekleyin:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | kaynakObjectType | Eşit | Kullanıcı |
    | cloudMastered | Notequal | True |

    Kapsam filtresi, bu giden eşitleme kuralının hangi Azure AD nesnesini karşılatacağı belirlenir. Bu örnekte, Out to AD - *Kullanıcı Kimliği* out-of-box eşitleme kuralından aynı kapsam filtresini kullanırız. Şirket içi Active Directory'den eşitlenmemiş Kullanıcı nesnelerine eşitleme kuralının uygulanmasını önler. Kapsam filtresini Azure AD Connect dağıtımınıza göre değiştirmeniz gerekebilir.

6. **Dönüşüm** sekmesine gidin ve aşağıdaki dönüşüm kuralını uygulayın:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez uygulayın | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Direct | UserType | UserType | Denetlenme -yen | Güncelleştirme |

7. Giden kuralı oluşturmak için **Ekle'yi** tıklatın.

![Giden eşitleme kuralı oluşturma](./media/how-to-connect-sync-change-the-configuration/usertype4.png)

### <a name="step-6-run-a-full-synchronization-cycle"></a>Adım 6: Tam bir eşitleme döngüsü çalıştırma
Genel olarak, hem Active Directory hem de Azure AD Bağlayıcısı şemalarına yeni öznitelikler eklediğimiz ve özel eşitleme kuralları getirdiğimiz için tam bir eşitleme döngüsü gereklidir. Değişiklikleri Azure AD'ye aktarmadan önce doğrulamak istiyorsunuz. 

Tam bir eşitleme döngüsü oluşturan adımları el ile çalıştırırken değişiklikleri doğrulamak için aşağıdaki adımları kullanabilirsiniz.

1. Şirket içi AD **Bağlayıcısı'nda** **tam bir ithalat** çalıştırın:

   1. Eşitleme Hizmeti Yöneticisi'ndeki **İşlemler** sekmesine gidin.
   2. Şirket içi **AD** Bağlayıcısı'na sağ tıklayın ve **Çalıştır'ı**seçin.
   3. Açılan iletişim kutusunda, **Tam İçe Aktar'ı** seçin ve ardından **Tamam'ı**tıklatın.
   4. İşlemin bitmesini bekleyin.

      > [!NOTE]
      > Kaynak özniteliği zaten içe aktarılan öznitelikler listesine dahil edilmişse, şirket içi AD Bağlayıcısı'nda tam bir alma işlemi atlayabilirsiniz. Başka bir deyişle, Adım 2 sırasında herhangi bir değişiklik yapmak zorunda [değildi: Şirket içi AD Bağlayıcı şemasıkaynak özniteliği ekleyin.](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema)

2. **Azure AD Bağlayıcısı'nda**Tam **alma** çalıştırın:

   1. Azure AD **Bağlayıcısı'na** sağ tıklayın ve **Çalıştır'ı**seçin.
   2. Açılan iletişim kutusunda, **Tam İçe Aktar'ı** seçin ve ardından **Tamam'ı**tıklatın.
   3. İşlemin bitmesini bekleyin.

3. Varolan bir Kullanıcı nesnesi üzerinde eşitleme kuralı değişikliklerini doğrulayın:

    Şirket içi Active Directory ve Azure AD'den UserType'ın kaynak özniteliği, ilgili Bağlayıcı Alanları'na aktarıldı. Tam bir eşitleme ile devam etmeden önce, şirket içi AD Bağlayıcı Alanı'nda varolan bir Kullanıcı nesnesi **üzerinde** önizleme yapın. Seçtiğiniz nesne, kaynak özniteliğinin doldurulması gerekir.
    
    Metaverse'de doldurulan UserType ile başarılı bir **Önizleme,** eşitleme kurallarını doğru yapılandırdığınızın iyi bir göstergesidir. **Önizleme**nasıl yapılacak hakkında bilgi için, [bölümüne](#verify-the-change)bakın değişikliği doğrulayın .

4. **Şirket içi AD Bağlayıcısı'nda**Tam **Senkronizasyon** Çalıştırın:

   1. Şirket içi **AD** Bağlayıcısı'na sağ tıklayın ve **Çalıştır'ı**seçin.
   2. Açılan iletişim kutusunda, **Tam Eşitleme'yi** seçin ve ardından **Tamam'ı**tıklatın.
   3. İşlemin bitmesini bekleyin.

5. **Bekleyen Dışa Aktarmayı** Azure AD'ye Doğrulayın:

   1. Azure AD **Bağlayıcısı'na** sağ tıklayın ve **Arama Bağlayıcısı Alanı'nı**seçin.
   2. Arama **Bağlayıcısı Alanı** açılır iletişim kutusunda:

      - **Kapsamı** **Bekleyen Dışa Aktarma**olarak ayarlayın.
      - Üç onay kutusunu da seçin: **Ekle,** **Değiştir**ve **Sil**.
      - Dışa aktaçları olan nesnelerin listesini almak için **Arama** düğmesini tıklatın. Belirli bir nesnenin değişikliklerini incelemek için nesneyi çift tıklatın.
      - Değişikliklerin beklendiğini doğrulayın.

6. Azure AD Bağlayıcısı'nda **Dışa** **Aktarma'yı**Çalıştır:

   1. Azure AD **Bağlayıcısı'na** sağ tıklayın ve **Çalıştır'ı**seçin.
   2. Çalıştır **Bağlayıcısı** açılır iletişim kutusunda **Dışa Aktar'ı** seçin ve **ardından Tamam'ı**tıklatın.
   3. Azure AD'ye yapılan dışa aktarmanın tamamlanmasını bekleyin.

> [!NOTE]
> Bu adımlar, Azure AD Bağlayıcısı'ndaki tam eşitleme ve dışa aktarma adımlarını içermez. Öznitelik değerleri şirket içi Active Directory'den Yalnızca Azure AD'ye aktığı ndan, bu adımlar gerekli değildir.

### <a name="step-7-re-enable-the-sync-scheduler"></a>Adım 7: Eşitleme zamanlayıcısını yeniden etkinleştirme
Yerleşik eşitleme zamanlayıcısı'nı yeniden etkinleştirin:

1. PowerShell oturumu başlatın.
2. Cmdlet `Set-ADSyncScheduler -SyncCycleEnabled $true`çalıştırarak zamanlanmış senkronizasyonu yeniden etkinleştirin.


## <a name="next-steps"></a>Sonraki adımlar
* [Bildirimsel Hükmü Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning.md)yapılandırma modeli hakkında daha fazla bilgi edinin.
* [Bildirimsel Hüküm Veren İfadeleri Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)ifade dili hakkında daha fazla bilgi edinin.

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
