---
title: Azure AD Connect - UserCertificate özniteliğinin neden olduğu Büyük Nesne hataları | Microsoft Dokümanlar
description: Bu konu, userCertificate özniteliğinin neden olduğu Büyük Nesne hataları için düzeltme adımlarını sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 146ad5b3-74d9-4a83-b9e8-0973a19828d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: c851b5ef024e6584e6f8c93995208b08a91fbb60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "62095498"
---
# <a name="azure-ad-connect-sync-handling-largeobject-errors-caused-by-usercertificate-attribute"></a>Azure AD Connect eşitleme: UserCertificate özniteliğinin neden olduğu Büyük Nesne hatalarını işleme

Azure AD, **userCertificate** özniteliği üzerinde en fazla **15** sertifika değeri sınırı uygular. Azure AD Connect, Azure AD'ye 15'ten fazla değeri olan bir nesne dışa ysa, Azure AD iletiyle birlikte bir **LargeObject** hatası döndürür:

>*"Sağlanan nesne çok büyük. Bu nesnedeki öznitelik değerlerinin sayısını kırpın. İşlem bir sonraki eşitleme döngüsünde yeniden denenecektir..."*

Büyük Nesne hatası diğer AD öznitelikleri neden olabilir. Bunun gerçekten userCertificate özniteliğinden kaynaklandığını doğrulamak için, şirket içi AD'de veya [Senkronizasyon Hizmet Yöneticisi Metaverse Arama'da](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-mvsearch)nesneye karşı doğrulamanız gerekir.

Büyük Nesne hatalarıyla kiracınızdaki nesnelerin listesini elde etmek için aşağıdaki yöntemlerden birini kullanın:

 * Eşitlenme için Azure AD Connect Health için kiracınız etkinse, sağlanan [Eşitleme Hata Raporu'na](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-sync) başvurabilirsiniz.
 
 * Her eşitleme döngüsünün sonunda gönderilen dizin eşitleme hataları için bildirim e-postası, Büyük Nesne hataları olan nesnelerin listesine sahiptir. 
 * [Eşitleme Hizmet Yöneticisi İşlemleri sekmesi,](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-operations) en son Azure AD işlemine dışa aktar'ı tıklattığınızda Büyük Nesne hataları olan nesnelerin listesini görüntüler.
 
## <a name="mitigation-options"></a>Azaltma seçenekleri
Büyük Nesne hatası çözülene kadar, aynı nesnedeki diğer öznitelik değişiklikleri Azure AD'ye dışa aktarılamaz. Hatayı gidermek için aşağıdaki seçenekleri göz önünde bulundurabilirsiniz:

 * 1.1.524.0 veya sonrası oluşturmak için Azure AD Connect'i yükseltin. Azure AD Connect build 1.1.524.0 yapısında, öznitelikler15'ten fazla değere sahipse, kutudan çıkan eşitleme kuralları kullanıcı Sertifikası ve kullanıcıSMIMECertificate özniteliklerini dışa aktarmamak için güncelleştirildi. Azure AD Connect'in nasıl yükseltilir hakkında ayrıntılı bilgi için [Azure AD Connect: Önceki sürümden en son'a yükseltme makalesine](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version)bakın.

 * Azure AD Connect'te, **15'ten fazla sertifika değerine sahip nesneler için gerçek değerler yerine boş değer**dışa aktabilen bir giden **eşitleme kuralı** uygulayın. Bu seçenek, 15'ten fazla değere sahip nesneler için Azure AD'ye dışa aktarılmak üzere sertifika değerlerinden herhangi birinin gerektirmiyorsa uygundur. Bu eşitleme kuralının nasıl uygulanacağı yla ilgili ayrıntılar için, [kullanıcı Sertifikası özniteliğinin dışa aktarını sınırlamak için eşitleme kuralını uygulama](#implementing-sync-rule-to-limit-export-of-usercertificate-attribute)sonraki bölüme bakın.

 * Kuruluşunuz tarafından artık kullanılmayan değerleri kaldırarak şirket içi AD nesnesindeki sertifika değerlerinin sayısını (15 veya daha az) azaltın. Öznitelik şişkinliği süresi dolmuş veya kullanılmayan sertifikalardan kaynaklanıyorsa, bu uygundur. Şirket içi AD'nizde süresi dolmuş sertifikaları bulma, yedekleme ve silmeye yardımcı olmak için [burada bulunan PowerShell komut dosyasını](https://gallery.technet.microsoft.com/Remove-Expired-Certificates-0517e34f) kullanabilirsiniz. Sertifikaları silmeden önce, kuruluşunuzdaki Ortak Anahtar-Altyapı yöneticileriyle doğrulamanız önerilir.

 * Kullanıcı Sertifikası özniteliğinin Azure AD'ye dışlanması için Azure AD Connect'i yapılandırın. Öznitelik Microsoft Çevrimiçi Hizmetleri tarafından belirli senaryoları etkinleştirmek için kullanılabildiğinden, genel olarak bu seçeneği önermiyoruz. Özellikle:
    * Kullanıcı nesnesindeki userCertificate özniteliği Exchange Online ve Outlook istemcileri tarafından ileti imzalama ve şifreleme için kullanılır. Bu özellik hakkında daha fazla bilgi edinmek [için ileti imzalama ve şifreleme için S/MIME maddesine](https://technet.microsoft.com/library/dn626158(v=exchg.150).aspx)bakın.

    * Bilgisayar nesnesindeki userCertificate özniteliği, Windows 10 şirket içi etki alanı birleştirilmiş aygıtların Azure AD'ye bağlanmasına izin vermek için Azure AD tarafından kullanılır. Bu özellik hakkında daha fazla bilgi edinmek için lütfen [etki alanına katılan aygıtları Windows 10 deneyimleri için Azure AD'ye bağlayın.](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy)

## <a name="implementing-sync-rule-to-limit-export-of-usercertificate-attribute"></a>Kullanıcı Sertifikası özniteliğinin dışa aktarını sınırlamak için eşitleme kuralını uygulama
userCertificate özniteliğinin neden olduğu Büyük Nesne hatasını gidermek için, Azure AD Connect'te **15'ten fazla sertifika değerine sahip nesneler için gerçek değerler yerine boş değer**dışa aktayan bir giden eşitleme kuralı uygulayabilirsiniz. Bu **bölümde, Kullanıcı** nesneleri için eşitleme kuralını uygulamak için gereken adımlar açıklanmaktadır. Adımlar **Kişi** ve **Bilgisayar** nesneleri için uyarlanabilir.

> [!IMPORTANT]
> Null değeri dışa aktarma, daha önce Azure AD'ye başarıyla aktarılan sertifika değerlerini kaldırır.

Adımlar şu şekilde özetlenebilir:

1. Eşitleme zamanlayıcısı'nı devre dışı bırakıp devam etmekte olan eşitleme olmadığını doğrulayın.
3. userCertificate özniteliği için varolan giden eşitleme kuralını bulun.
4. Gereken giden eşitleme kuralını oluşturun.
5. Büyük Nesne hatası yla varolan bir nesnenin yeni eşitleme kuralını doğrulayın.
6. LargeObject hatası ile kalan nesnelere yeni eşitleme kuralını uygulayın.
7. Azure AD'ye aktamayı bekleyen beklenmeyen bir değişiklik olmadığını doğrulayın.
8. Değişiklikleri Azure AD'ye aktarın.
9. Eşitleme zamanlayıcısını yeniden etkinleştirin.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. Adım. Eşitleme zamanlayıcısı'nı devre dışı bırakıp devam etmekte olan eşitleme olmadığını doğrulayın
Azure AD'ye istenmeyen değişikliklerin dışa aktarılmasını önlemek için yeni bir eşitleme kuralı uygularken senkronizasyon gerçekleşmediğinden emin olun. Yerleşik eşitleme zamanlayıcısını devre dışı kalmak için:
1. Azure AD Connect sunucusunda PowerShell oturumunu başlatın.

2. Cmdlet çalıştırarak zamanlanmış senkronizasyonu devre dışı bırakın:`Set-ADSyncScheduler -SyncCycleEnabled $false`

> [!Note]
> Önceki adımlar yalnızca yerleşik zamanlayıcıyla Azure AD Connect'in yeni sürümleri (1.1.xxx.x) için geçerlidir. Windows Görev Zamanlayıcısı kullanan Azure AD Connect'in eski sürümlerini (1.0.xxx.x) kullanıyorsanız veya periyodik eşitlemayı tetiklemek için kendi özel zamanlayıcınızı (yaygın olmayan) kullanıyorsanız, bunları buna göre devre dışı bırakmalısınız.

1. START → Eşitleme Hizmeti'ne giderek **Eşitleme Hizmet Yöneticisi'ni** başlatın.

1. **İşlemler** sekmesine gidin ve durumu *"devam eden"* bir işlem olmadığını onaylayın.

### <a name="step-2-find-the-existing-outbound-sync-rule-for-usercertificate-attribute"></a>2. Adım userCertificate özniteliği için varolan giden eşitleme kuralını bulma
Kullanıcı nesneleri için kullanıcı Sertifikası özniteliğini Azure AD'a dışa aktarmak üzere etkinleştirilmiş ve yapılandırılan varolan bir eşitleme kuralı olmalıdır. **Öncelik** ve kapsam filtresi yapılandırmasını bulmak için bu **eşitleme** kuralını bulun:

1. START → Eşitleme Kuralları Düzenleyicisi'ne giderek **Eşitleme Kuralları** Düzenleyicisi'ni başlatın.

2. Arama filtrelerini aşağıdaki değerlerle yapılandırın:

    | Öznitelik | Değer |
    | --- | --- |
    | Yön |**Giden** |
    | MV Nesne Türü |**Kişi** |
    | Bağlayıcı |*Azure AD bağlayıcınızın adı* |
    | Bağlayıcı Nesne Türü |**Kullanıcı** |
    | MV özniteliği |**userCertificate** |

3. Kullanıcı nesneleri için kullanıcıCertficiate özniteliğini dışa aktarmak için Azure AD bağlayıcısına OOB (hazır) eşitleme kurallarını kullanıyorsanız, *"Out to AAD – User ExchangeOnline"* kuralını geri almalısınız.
4. Bu eşitleme kuralının **öncelik** değerini not edin.
5. Eşitleme kuralını seçin ve **Edit'i**tıklatın.
6. *"Ayrılmış Kural Onayını Edit"* açılır iletişim kutusunda **No'yu**tıklatın. (Endişelenmeyin, bu eşitleme kuralında herhangi bir değişiklik yapmayacağız).
7. Edit ekranında Kapsam **filtresi** sekmesini seçin.
8. Kapsam filtresi yapılandırmasını not edin. OOB eşitleme kuralını kullanıyorsanız, aşağıdakiler de dahil olmak üzere **iki yan tümce içeren tam olarak bir kapsam filtresi grubu**olmalıdır:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | kaynakObjectType | Eşit | Kullanıcı |
    | cloudMastered | Notequal | True |

### <a name="step-3-create-the-outbound-sync-rule-required"></a>3. Adım Gereken giden eşitleme kuralını oluşturma
Yeni eşitleme kuralı, varolan eşitleme kuralından aynı **kapsam filtresine** ve **daha yüksek önceliğe** sahip olmalıdır. Bu, yeni eşitleme kuralının varolan eşitleme kuralıyla aynı nesne kümesine geçerli olmasını sağlar ve userCertificate özniteliği için varolan eşitleme kuralını geçersiz kılar. Eşitleme kuralını oluşturmak için:
1. Eşitleme Kuralları Düzenleyicisi'nde **yeni kural ekle** düğmesini tıklatın.
2. Açıklama **sekmesialtında,** aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Adı | *Ad verme* | Örneğin, *"Out to AAD – UserCertificate için özel geçersiz kılma"* |
    | Açıklama | *Açıklama sağlayın* | Örneğin, *"userCertificate özniteliği 15'ten fazla değere sahipse, NULL'u dışa aktarın."* |
    | Bağlı Sistem | *Azure AD Bağlayıcısını seçin* |
    | Bağlı Sistem Nesne Türü | **Kullanıcı** | |
    | Metaverse Nesne Türü | **Kişi** | |
    | Bağlantı Türü | **Katıl** | |
    | Önceliği | *1 - 99 arasında bir sayı seçti* | Seçilen sayı varolan eşitleme kuralı tarafından kullanılmamalıdır ve varolan eşitleme kuralından daha düşük bir değere (ve dolayısıyla daha yüksek önceliğe) sahiptir. |

3. **Kapsam filtresi** sekmesine gidin ve varolan eşitleme kuralının kullandığı aynı kapsam filtresini uygulayın.
4. Birleştirme **kurallarını** atlayın sekmesi.
5. Aşağıdaki yapılandırmayı kullanarak yeni bir dönüşüm eklemek için **Dönüşümler** sekmesine gidin:

    | Öznitelik | Değer |
    | --- | --- |
    | Akış Türü |**İfadeler** |
    | Hedef Öznitelik |**userCertificate** |
    | Kaynak Özniteliği |*Aşağıdaki ifadeyi kullanın:*`IIF(IsNullOrEmpty([userCertificate]), NULL, IIF((Count([userCertificate])> 15),AuthoritativeNull,[userCertificate]))` |
    
6. Eşitleme kuralını oluşturmak için **Ekle** düğmesini tıklatın.

### <a name="step-4-verify-the-new-sync-rule-on-an-existing-object-with-largeobject-error"></a>4. Adım. Büyük Nesne hatasıyla varolan bir nesnenin yeni eşitleme kuralını doğrulama
Bu, oluşturulan eşitleme kuralının diğer nesnelere uygulamadan önce Büyük Nesne hatası olan varolan bir AD nesnesi üzerinde doğru çalıştığını doğrulamak içindir:
1. Eşitleme Hizmeti Yöneticisi'ndeki **İşlemler** sekmesine gidin.
2. En son Azure AD dışa aktar işlemini seçin ve Büyük Nesne hataları olan nesnelerden birine tıklayın.
3.  Bağlayıcı Alan Nesne Özellikleri açılır ekranında **Önizleme** düğmesine tıklayın.
4. Önizleme açılır ekranında Tam **eşitleme'yi** seçin ve **Önizlemeyi İkile'yi tıklatın.**
5. Önizleme ekranını ve Bağlayıcı Alanı Nesne Özellikleri ekranını kapatın.
6. Eşitleme Hizmeti Yöneticisi'ndeki **Bağlayıcılar** sekmesine gidin.
7. **Azure AD** Bağlayıcısı'na sağ tıklayın ve **Çalıştır'ı seçin...**
8. Çalıştır Bağlayıcısı açılır penceresinde **Dışa** Aktar adımını seçin ve **Tamam'ı**tıklatın.
9. Azure AD'ye Dışa Aktar'ın tamamlanmasını bekleyin ve bu nesnede artık Büyük Nesne hatası olmadığını onaylayın.

### <a name="step-5-apply-the-new-sync-rule-to-remaining-objects-with-largeobject-error"></a>5. Adım. LargeObject hatası ile kalan nesnelere yeni eşitleme kuralını uygulama
Eşitleme kuralı eklendikten sonra, AD Bağlayıcısı'nda tam bir eşitleme adımı çalıştırmanız gerekir:
1. Eşitleme Hizmeti Yöneticisi'ndeki **Bağlayıcılar** sekmesine gidin.
2. **AD** Bağlayıcısı'na sağ tıklayın ve **Çalıştır'ı seçin...**
3. Çalıştır Bağlayıcısı açılır penceresinde **Tam Eşitleme** adımını seçin ve **Tamam'ı**tıklatın.
4. Tam Eşitleme adımının tamamlanmasını bekleyin.
5. Birden fazla AD Bağlayıcınız varsa, kalan AD Bağlayıcıları için yukarıdaki adımları yineleyin. Genellikle, birden çok şirket içi dizininvarsa birden çok bağlayıcı gereklidir.

### <a name="step-6-verify-there-are-no-unexpected-changes-waiting-to-be-exported-to-azure-ad"></a>6. Adım. Azure AD'ye aktamayı bekleyen beklenmeyen değişiklikler olmadığını doğrulama
1. Eşitleme Hizmeti Yöneticisi'ndeki **Bağlayıcılar** sekmesine gidin.
2. **Azure AD** Bağlayıcısı'na sağ tıklayın ve **Arama Bağlayıcısı Alanı'nı**seçin.
3. Arama Bağlayıcısı Alanı açılır penceresinde:
    1. Kapsamı **Bekleyen Dışa Aktarma**olarak ayarlayın.
    2. **Ekle**, **Değiştir** ve Sil dahil olmak üzere 3 onay kutusunun tümünü **işaretleyin.**
    3. Azure AD'ye aktamayı bekleyen değişikliklerle tüm nesneleri döndürmek için **Ara** düğmesini tıklatın.
    4. Beklenmeyen bir değişiklik olmadığını doğrulayın. Belirli bir nesnenin değişikliklerini incelemek için nesneye çift tıklayın.

### <a name="step-7-export-the-changes-to-azure-ad"></a>7. Adım. Değişiklikleri Azure AD'ye aktarma
Değişiklikleri Azure AD'ye aktarmak için:
1. Eşitleme Hizmeti Yöneticisi'ndeki **Bağlayıcılar** sekmesine gidin.
2. **Azure AD** Bağlayıcısı'na sağ tıklayın ve **Çalıştır'ı seçin...**
4. Çalıştır Bağlayıcısı açılır penceresinde **Dışa** Aktar adımını seçin ve **Tamam'ı**tıklatın.
5. Azure AD'ye Dışak'ın tamamlanmasını bekleyin ve artık Büyük Nesne hatası olmadığını onaylayın.

### <a name="step-8-re-enable-sync-scheduler"></a>8. Adım Eşitleme zamanlayıcısı'nı yeniden etkinleştirme
Sorun çözüldüğüne göre, yerleşik eşitleme zamanlayıcısı'nı yeniden etkinleştirin:
1. PowerShell oturumunu başlatın.
2. Cmdlet çalıştırarak zamanlanmış senkronizasyonu yeniden etkinleştirin:`Set-ADSyncScheduler -SyncCycleEnabled $true`

> [!Note]
> Önceki adımlar yalnızca yerleşik zamanlayıcıyla Azure AD Connect'in yeni sürümleri (1.1.xxx.x) için geçerlidir. Windows Görev Zamanlayıcısı kullanan Azure AD Connect'in eski sürümlerini (1.0.xxx.x) kullanıyorsanız veya periyodik eşitlemayı tetiklemek için kendi özel zamanlayıcınızı (yaygın olmayan) kullanıyorsanız, bunları buna göre devre dışı bırakmalısınız.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

