---
title: 'Azure AD Bağlantısı: Office 365 kaynakları için tercih edilen veri konumunu yapılandırma'
description: Azure Active Directory Connect eşitlesiyle Office 365 kullanıcı kaynaklarınızı kullanıcıya nasıl yaklaştırılacak açıklanır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/11/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 723411191d0990583d039a0fc9651437480807b4
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80983271"
---
# <a name="azure-active-directory-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Azure Active Directory Connect eşitle: Office 365 kaynakları için tercih edilen veri konumunu yapılandırma
Bu konunun amacı, Azure Active Directory (Azure AD) Connect eşitlemesinde tercih edilen veri konumu için özniteliği nasıl yapılandırabileceğinizi size sağlamaktır. Birisi Office 365'te Multi-Geo özelliklerini kullandığında, bu özelliği kullanıcının Office 365 verilerinin coğrafi konumunu belirlemek için kullanırsınız. (Bölge *region* ve *coğrafi* terimler birbirinin yerine kullanılır.)

## <a name="enable-synchronization-of-preferred-data-location"></a>Tercih edilen veri konumunun eşitlemesini etkinleştirme
Varsayılan olarak, kullanıcılarınız için Office 365 kaynakları Azure AD kiracınızla aynı coğrafi konumda bulunur. Örneğin, kiracınız Kuzey Amerika'da bulunuyorsa, kullanıcıların Exchange posta kutuları da Kuzey Amerika'da bulunur. Çok uluslu bir kuruluş için bu uygun olmayabilir.

Öznitelik **preferredDataLocation**ayarlayarak, bir kullanıcının geo tanımlayabilirsiniz. Kullanıcının posta kutusu ve OneDrive gibi Office 365 kaynaklarına kullanıcıyla aynı coğrafi ortamda sahip olabilir ve tüm kuruluşunuz için hala bir kiracınız olabilir.

> [!IMPORTANT]
> Multi-Geo şu anda etkin bir Kurumsal Sözleşme ve en az 500 Office 365 Hizmetleri aboneliği olan müşteriler tarafından kullanılabilir. Ayrıntılar için lütfen Microsoft temsilcinizle konuşun.
>
>

Office 365 için tüm coğrafi verilerin izinin listesi [verilerinizin bulunduğu yerde bulunabilir?](https://aka.ms/datamaps).

Multi-Geo için kullanılabilen Office 365'teki coğrafi kakışlar şunlardır:

| Coğrafi Bölge | tercihDataLocation değeri |
| --- | --- |
| Asya Pasifik | Apc |
| Avustralya | Aus |
| Kanada | -bilirsiniz |
| Avrupa Birliği | EUR |
| Fransa | FRA |
| Hindistan | IND |
| Japonya | JPN |
| Güney Kore | KOR |
| Güney Afrika | ZAF |
| İsviçre | CHE |
| Birleşik Arap Emirlikleri | ARE |
| Birleşik Krallık | Gbr |
| Amerika Birleşik Devletleri | Nam |

* Bu tabloda bir coğrafi listelenmemişse (örneğin, Güney Amerika), Multi-Geo için kullanılamaz.

* Tüm Office 365 iş yükleri, kullanıcının coğrafi ayarını desteklemez.

### <a name="azure-ad-connect-support-for-synchronization"></a>Senkronizasyon için Azure AD Connect desteği

Azure AD Connect, sürüm 1.1.524.0 ve sonraki sürümlerde **Kullanıcı** nesneleri için **tercih edilen DataLocation** özniteliğinin eşitleştirilmesini destekler. Daha ayrıntılı şekilde belirtmek gerekirse:

* Azure AD Bağlayıcısı'ndaki nesne türü **Kullanıcı'nın** **şeması, tercih edilen DataLocation** özniteliğini içerecek şekilde genişletilir. Öznitelik türü, tek değerli dize.
* Metaverse nesne türü **Kişi** şeması **tercih edilen DataLocation** özniteliği içerecek şekilde genişletilir. Öznitelik türü, tek değerli dize.

Varsayılan olarak, **preferredDataLocation** eşitleme için etkinleştirilir. Bu özellik daha büyük kuruluşlar için tasarlanmıştır. Windows Server 2019'daki Active Directory şeması, bu amaçla kullanmanız gereken **MSDS tercihli DataLocation** özelliğine sahiptir. Active Directory şemasını güncelleştirmediyseniz ve bunu yapamıyorsanız, kullanıcılarınız için Office 365 coğrafi özelliğini tutmak için bir öznitelik tanımlamanız gerekir. Bu her organizasyon için farklı olacak.

> [!IMPORTANT]
> Azure AD, **bulut Kullanıcı nesnelerinde** **tercih edilen DataLocation** özniteliğinin Azure AD PowerShell kullanılarak doğrudan yapılandırılmasına olanak tanır. Bu özniteliği **eşitlenmiş Kullanıcı nesnelerinde**yapılandırmak için Azure AD Connect'i kullanmanız gerekir.

Eşitleme etkinleştirmeden önce:

* Active Directory şemasını 2019'a yükseltmediyseniz, kaynak özniteliği olarak hangi şirket içi Active Directory özniteliğinin kullanılacağına karar verin. Bu tür, tek **değerli dize**olmalıdır.
* Azure AD PowerShell'i kullanarak Azure AD'deki varolan **senkronize Kullanıcı nesnelerinde** **tercih edilen Veri Konumu** özniteliğini daha önce yapılandırmışsanız, öznitelik değerlerini şirket içi Active Directory'deki ilgili **Kullanıcı** nesnelerine geri taşımanız gerekir.

    > [!IMPORTANT]
    > Bu değerleri geri aktarmazsanız, Azure AD **Connect, tercih edilen DataLocation** özniteliği için eşitleme etkinleştirildiğinde Azure AD'deki varolan öznitelik değerlerini kaldırır.

* Kaynak özniteliğini artık en az birkaç şirket içi Active Directory User nesnesi üzerinde yapılandırın. Bunu doğrulama için daha sonra kullanabilirsiniz.

Aşağıdaki bölümler, **tercih edilen DataLocation** özniteliğinin eşitlemesini etkinleştirmek için adımları sağlar.

> [!NOTE]
> Adımlar, tek orman topolojisi içeren ve özel eşitleme kuralları olmayan bir Azure AD dağıtımı bağlamında açıklanmıştır. Çok ormanlı bir topolojiniz, özel eşitleme kurallarınınız yapılandırıldıysa veya bir evreleme sunucunuz varsa, adımları buna göre ayarlamanız gerekir.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Adım 1: Eşitleme zamanlayıcısını devre dışı bırakıp devam etmekte olan eşitleme olmadığını doğrulayın
Azure AD'ye aktamasını istenmeyen değişikliklerin olmasını önlemek için, eşitleme kurallarını güncelleştirmenin ortasındayken eşitleme gerçekleşmediğinden emin olun. Yerleşik eşitleme zamanlayıcısını devre dışı kalmak için:

1. Azure AD Connect sunucusunda bir PowerShell oturumu başlatın.
2. Bu cmdlet çalıştırarak zamanlanmış senkronizasyonu `Set-ADSyncScheduler -SyncCycleEnabled $false`devre dışı bırakın: .
3.  >  **START**Eşitleme Hizmeti'ne giderek **Eşitleme Hizmet** **Yöneticisi'ni**başlatın.
4. **İşlemler** sekmesini seçin ve durum devam *eden*bir işlem olmadığını onaylayın.

![Senkronizasyon Servis Yöneticisi ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-refresh-the-schema-for-active-directory"></a>Adım 2: Active Directory için şema yenileyin
Active Directory şemasını 2019'a güncellediyseniz ve Connect şema uzantısından önce yüklenmişse, Connect şema önbelleğinde güncelleştirilmiş şema yoktur. Daha sonra, seçekeğin uI' de görünmesi için sihirbazdan gelen şeayyı yenilemeniz gerekir.

1. Azure AD Connect sihirbazını masaüstünden başlatın.
2. **Dizin şemasını yenile** seçeneğini seçin ve **İleri'yi**tıklatın.
3. Azure REKLAM kimlik bilgilerinizi girin ve **İleri'yi**tıklatın.
4. **Dizin Şemasını Yenile** sayfasında, tüm ormanların seçildiğinden emin olun ve **İleri'yi**tıklatın.
5. Tamamlandığında sihirbazı kapatın.

![Connect sihirbazında Dizin Şema'sını Yenile ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-refreshschema.png)

## <a name="step-3-add-the-source-attribute-to-the-on-premises-active-directory-connector-schema"></a>Adım 3: Kaynak özniteliğini şirket içi Active Directory Connector şemasına ekleme
**Bu adım yalnızca Connect sürüm 1.3.21 veya daha büyük çalıştırdığınızda gereklidir. 1.4.18 veya daha yeniyseniz, 5.**  
Tüm Azure AD öznitelikleri şirket içi Active Directory bağlayıcı alanına alınmaz. Varsayılan olarak eşitlenmemiş bir öznitelik kullanmayı seçtiyseniz, bunu almanız gerekir. Alınan öznitelikler listesine kaynak özniteliği eklemek için:

1. Eşitleme Hizmet Yöneticisi'nde **Bağlayıcılar** sekmesini seçin.
2. Şirket içi Active Directory Bağlayıcısı'na sağ tıklayın ve **Özellikler'i**seçin.
3. Açılan iletişim kutusunda **Öznitelikleri Seç** sekmesine gidin.
4. Kullanmak için seçtiğiniz kaynak özniteliğinin öznitelik listesinde işaretli olduğundan emin olun. Özniteliğinizi görmüyorsanız, **Tümünü Göster** onay kutusunu seçin.
5. Kaydetmek için **Tamam'ı**seçin.

![Eşitleme Hizmet Yöneticisi ve Özellikleri iletişim kutusunun ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-4-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Adım 4: Azure AD Bağlayıcısı şemasına **tercihli Veri Konumu** ekleme
**Bu adım yalnızca Connect sürüm 1.3.21 veya daha büyük çalıştırdığınızda gereklidir. 1.4.18 veya daha yeniyseniz, 5.**  
Varsayılan olarak, **tercih edilen DataLocation** özniteliği Azure AD Bağlayıcısı alanına alınmaz. İçe aktarılan öznitelikler listesine eklemek için:

1. Eşitleme Hizmet Yöneticisi'nde **Bağlayıcılar** sekmesini seçin.
2. Azure AD bağlayıcısını sağ tıklatın ve **Özellikler'i**seçin.
3. Açılan iletişim kutusunda **Öznitelikleri Seç** sekmesine gidin.
4. Listede **tercih edilen DataLocation** özniteliğini seçin.
5. Kaydetmek için **Tamam'ı**seçin.

![Eşitleme Hizmet Yöneticisi ve Özellikleri iletişim kutusunun ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-5-create-an-inbound-synchronization-rule"></a>Adım 5: Gelen eşitleme kuralı oluşturma
Gelen eşitleme kuralı, öznitelik değerinin şirket içi Active Directory'deki kaynak öznitelikten metaverse akmasına izin verir.

1.  >  **START**Eşitleme Kuralları Düzenleyicisi'ne giderek **Eşitleme Kuralları** **Düzenleyicisini**başlatın.
2. Arama filtresi **Yön'u** **Gelen**olacak şekilde ayarlayın.
3. Yeni bir gelen kuralı oluşturmak için **yeni kural ekle'yi**seçin.
4. **Açıklama** sekmesinin altında aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Adı | *Ad verme* | Örneğin, "IN from AD – User preferredDataLocation" |
    | Açıklama | *Özel bir açıklama sağlayın* |  |
    | Bağlı Sistem | *Şirket içi Active Directory Connector'ı seçin* |  |
    | Bağlı Sistem Nesne Türü | **Kullanıcı** |  |
    | Metaverse Nesne Türü | **Kişi** |  |
    | Bağlantı Türü | **Katıl** |  |
    | Önceliği | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmeyin. |

5. Tüm nesneleri içerecek şekilde **Kapsam filtresini** boş tutun. Kapsam filtresini Azure AD Connect dağıtımınıza göre değiştirmeniz gerekebilir.
6. **Dönüşüm sekmesine**gidin ve aşağıdaki dönüşüm kuralını uygulayın:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez uygulayın | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    |Direct | tercihDataLocation | Kaynak özniteliğini seçin | Denetlenme -yen | Güncelleştir |

7. Gelen kuralı oluşturmak için **Ekle'yi**seçin.

![Gelen senkronizasyon kuralıoluşturma ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-6-create-an-outbound-synchronization-rule"></a>Adım 6: Giden eşitleme kuralı oluşturma
Giden eşitleme kuralı, öznitelik değerinin metaverse'den Azure AD'deki **tercih edilen DataLocation** özniteliğine akmasına izin verir:

1. **Eşitleme Kuralları Düzenleyicisine**gidin.
2. Arama filtresi **Yön'u** **Giden**olacak şekilde ayarlayın.
3. **Yeni kural ekle'yi**seçin.
4. **Açıklama** sekmesinin altında aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | ----- | ------ | --- |
    | Adı | *Ad verme* | Örneğin, "Out to Azure AD – Kullanıcı tercih iveri konumu" |
    | Açıklama | *Açıklama sağlayın* ||
    | Bağlı Sistem | *Azure AD Bağlayıcısını seçin* ||
    | Bağlı Sistem Nesne Türü | **Kullanıcı** ||
    | Metaverse Nesne Türü | **Kişi** ||
    | Bağlantı Türü | **Katıl** ||
    | Önceliği | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmeyin. |

5. **Kapsam filtresi** sekmesine gidin ve iki yan tümceiçeren tek bir kapsam filtresi grubu ekleyin:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | kaynakObjectType | Eşit | Kullanıcı |
    | cloudMastered | Notequal | True |

    Kapsam filtresi, bu giden eşitleme kuralının uygulandığı Azure AD nesnelerini belirler. Bu örnekte, "Out to Azure AD – Kullanıcı Kimliği" OOB (kullanıma aç) eşitleme kuralındaki aynı kapsam filtresini kullanırız. Şirket içi Active Directory'den eşitlenmemiş **Kullanıcı** nesnelerine eşitleme kuralının uygulanmasını önler. Kapsam filtresini Azure AD Connect dağıtımınıza göre değiştirmeniz gerekebilir.

6. **Dönüşüm** sekmesine gidin ve aşağıdaki dönüşüm kuralını uygulayın:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez uygulayın | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Direct | tercihDataLocation | tercihDataLocation | Denetlenme -yen | Güncelleştir |

7. Giden kuralı oluşturmak için **Ekle'yi** kapat.

![Giden senkronizasyon kuralı oluşturma ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-7-run-full-synchronization-cycle"></a>Adım 7: Tam eşitleme döngüsünü çalıştırın
Genel olarak, tam eşitleme döngüsü gereklidir. Bunun nedeni, hem Etkin Dizin hem de Azure AD Bağlayıcısı şemasına yeni öznitelikler eklemeniz ve özel eşitleme kuralları nın getirilmesidir. Değişiklikleri Azure AD'ye aktarmadan önce doğrulayın. Değişiklikleri doğrulamak için aşağıdaki adımları kullanabilirsiniz, tam bir eşitleme döngüsü oluşturan adımları el ile çalıştırırken.

1. Şirket içi Active Directory Connector'da **tam alma** çalıştırın:

   1. Eşitleme Hizmeti Yöneticisi'ndeki **İşlemler** sekmesine gidin.
   2. Şirket içi **Active Directory Connector'a**sağ tıklayın ve **Çalıştır'ı**seçin.
   3. İletişim kutusunda, Tam **İçe Aktar'ı**seçin ve **Tamam'ı**seçin.
   4. İşlemin tamamlanmasını bekleyin.

      > [!NOTE]
      > Kaynak özniteliği zaten içe aktarılan öznitelikler listesine dahil edilmişse, şirket içi Etkin Dizin Bağlayıcısı'nda tam içe aktarmayı atlayabilirsiniz. Başka bir deyişle, bu makalede daha önce adım 2 sırasında herhangi bir değişiklik yapmak zorunda değildi.

2. Azure AD Bağlayıcısı'nda **tam içe aktarma** yı çalıştırın:

   1. Azure AD **Bağlayıcısı'na**sağ tıklayın ve **Çalıştır'ı**seçin.
   2. İletişim kutusunda, Tam **İçe Aktar'ı**seçin ve **Tamam'ı**seçin.
   3. İşlemin tamamlanmasını bekleyin.

3. Varolan bir **Kullanıcı** nesnesi üzerinde eşitleme kuralı değişikliklerini doğrulayın.

   Şirket içi Active Directory'den kaynak özniteliği ve Azure AD'den **tercih edilen DataLocation,** her ilgili bağlayıcı alanına aktarılmıştır. Tam eşitleme adımına geçmeden önce, şirket içi Active Directory Connector alanında varolan bir **Kullanıcı** nesnesi üzerinde önizleme yapın. Seçtiğiniz nesnenin kaynak özniteliği nin doldurulması gerekir. Metaverse'de bulunan **tercihli DataLocation** ile başarılı bir önizleme, eşitleme kurallarını doğru yapılandırdığınızın iyi bir göstergesidir. Önizlemenin nasıl yapılacağını öğrenmek için [bkz.](how-to-connect-sync-change-the-configuration.md#verify-the-change)

4. Şirket içi Active Directory Connector'da **Tam Senkronizasyon** çalıştırın:

   1. Şirket içi **Active Directory Connector'a**sağ tıklayın ve **Çalıştır'ı**seçin.
   2. İletişim kutusunda **Tam Eşitleme'yi**seçin ve **Tamam'ı**seçin.
   3. İşlemin tamamlanmasını bekleyin.

5. **Bekleyen Dışa Aktarmayı** Azure AD'ye Doğrulayın:

   1. Azure AD **Bağlayıcısı'na**sağ tıklayın ve **Arama Bağlayıcısı Alanı'nı**seçin.
   2. Arama **Bağlayıcısı Alanı** iletişim kutusunda:

        a. **Kapsamı** **Bekleyen Dışa Aktarma**olarak ayarlayın.<br>
        b. **Ekle, Değiştir ve Sil**dahil olmak üzere üç onay kutusunu da seçin.<br>
        c. Dışa aktak edilecek değişikliklerle nesnelerin listesini görüntülemek için **Ara'yı**seçin. Belirli bir nesnenin değişikliklerini incelemek için nesneyi çift tıklatın.<br>
        d. Değişikliklerin beklendiğini doğrulayın.

6. Azure AD **Bağlayıcısı'nda** **Dışa Aktarma'yı** Çalıştırma

   1. Azure AD **Bağlayıcısı'na**sağ tıklayın ve **Çalıştır'ı**seçin.
   2. **Bağlayıcıyı Çalıştır** iletişim kutusunda **Dışa Aktar'ı**ve **Tamam'ı**seçin.
   3. İşlemin tamamlanmasını bekleyin.

> [!NOTE]
> Adımların Azure AD Bağlayıcısı'ndaki tam eşitleme adımını veya Active Directory Connector'daki dışa aktarma adımını içermediğini fark edebilirsiniz. Öznitelik değerleri şirket içi Active Directory'den Yalnızca Azure AD'ye aktığından, adımlar gerekli değildir.

## <a name="step-8-re-enable-sync-scheduler"></a>Adım 8: Eşitleme zamanlayıcısını yeniden etkinleştirme
Yerleşik eşitleme zamanlayıcısı'nı yeniden etkinleştirin:

1. PowerShell oturumu başlatın.
2. Bu cmdlet çalıştırarak zamanlanmış eşitleme yeniden etkinleştirin:`Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-9-verify-the-result"></a>Adım 9: Sonucu doğrulayın
Şimdi yapılandırmayı doğrulama ve kullanıcılarınız için etkinleştirme zamanıdır.

1. Kullanıcıüzerinde seçilen özniteliğe coğrafi özelliği ekleyin. Kullanılabilir coğrafi kınların listesi bu tabloda bulunabilir.  
![Kullanıcıya eklenen AD özniteliğinin ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Özniteliğin Azure AD'ye eşitlemesini bekleyin.
3. Exchange Online PowerShell'i kullanarak, posta kutusu bölgesinin doğru ayarlandığını doğrulayın.  
![Exchange Online PowerShell ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Kiracınızın bu özelliği kullanabilmesi için işaretlendiğini varsayarsak, posta kutusu doğru coğrafi kınızda taşınır. Bu, posta kutusunun bulunduğu sunucu adına bakarak doğrulanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Office 365'te Multi-Geo hakkında daha fazla bilgi edinin:

* [Ignite'da Çok Coğrafi oturumlar](https://aka.ms/MultiGeoIgnite)
* [OneDrive'da Çok Coğrafi](https://aka.ms/OneDriveMultiGeo)
* [SharePoint Online'da Çok Coğrafi](https://aka.ms/SharePointMultiGeo)

Eşitleme motorundaki yapılandırma modeli hakkında daha fazla bilgi edinin:

* [Bildirimsel Hükmü Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning.md)yapılandırma modeli hakkında daha fazla bilgi edinin.
* [Bildirimsel Hüküm Veren İfadeleri Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)ifade dili hakkında daha fazla bilgi edinin.

Genel bakış konuları:

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
