---
title: Değiştirilmiş varsayılan kurallar nasıl düzeltilir - Azure AD Connect | Microsoft Dokümanlar
description: Azure AD Connect ile birlikte gelen değiştirilmiş varsayılan kuralları nasıl düzelttiğinizi öğrenin.
services: active-directory
author: billmath
manager: daveba
editor: curtand
ms.reviewer: darora10
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4626e0149028a140d143fb8d0969a03b732201fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79036974"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Azure AD Connect'te değiştirilmiş varsayılan kuralları düzeltme

Azure Etkin Dizin (Azure AD) Connect eşitleme için varsayılan kuralları kullanır.  Ne yazık ki, bu kurallar tüm kuruluşlar için evrensel olarak geçerli değildir. Gereksinimlerinize bağlı olarak, bunları değiştirmeniz gerekebilir. Bu makalede, en yaygın özelleştirmeler iki örnek açıklanmaktadır ve bu özelleştirmeler elde etmek için doğru yolu açıklar.

>[!NOTE] 
> Gerekli özelleştirmeyi elde etmek için varolan varsayılan kuralları değiştirme desteklenmez. Bunu yaparsanız, bu kuralları gelecek sürümlerde en son sürüme güncelleştirmeyi engeller. İhtiyacınız olan hata düzeltmelerini veya yeni özellikleri almazsınız. Bu belge, varolan varsayılan kuralları değiştirmeden aynı sonuca nasıl ulaşılabildiğini açıklar. 

## <a name="how-to-identify-modified-default-rules"></a>Değiştirilmiş varsayılan kuralları belirleme
Azure AD Connect sürümü 1.3.7.0 ile başlayarak, değiştirilmiş varsayılan kuralı belirlemek kolaydır. **Masaüstündeki Uygulamalar'a**gidin ve **Eşitleme Kuralları Düzenleyicisi'ni**seçin.

![Azure AD Connect, Senkronizasyon Kuralları Düzenleyicisi vurgulanır](media/how-to-connect-fix-default-rules/default1.png)

Düzenleyici'de, değiştirilen varsayılan kurallar, adın önünde bir uyarı simgesiyle gösterilir.

![Uyarı simgesi](media/how-to-connect-fix-default-rules/default2.png)

 Yanında aynı ada sahip devre dışı bırakılmış bir kural da görüntülenir (bu standart varsayılan kuraldır).

![Eşitleme Kuralları Düzenleyicisi, standart varsayılan kuralı ve değiştirilmiş varsayılan kuralı nı gösterir](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Sık özelleştirmeler
Varsayılan kurallara göre sık karşılaşılan özelleştirmeler şunlardır:

- Öznitelik akışını değiştirme
- Kapsam filtresini değiştirme
- Birleştirme koşulunu değiştirme

Herhangi bir kuralı değiştirmeden önce:

- Eşitleme zamanlayıcısını devre dışı kınla. Zamanlayıcı varsayılan olarak her 30 dakikada bir çalışır. Değişiklik yaparken ve yeni kurallarınızı sorun giderirken başlamadığından emin olun. Zamanlayıcıyı geçici olarak devre dışı kılmış `Set-ADSyncScheduler -SyncCycleEnabled $false`sayılsın, PowerShell'i başlatın ve çalıştırın.
 ![Eşitleme zamanlayıcısı devre dışı kalmak için PowerShell komutları](media/how-to-connect-fix-default-rules/default3.png)

- Kapsam filtresindeki değişiklik, hedef dizindeki nesnelerin silinmesine neden olabilir. Nesnelerin kapsamlarında herhangi bir değişiklik yapmadan önce dikkatli olun. Etkin sunucuda değişiklik yapmadan önce bir evreleme sunucusunda değişiklik yapmanızı öneririz.
- Yeni bir kural ekledikten sonra [Eşitle Kuralını Doğrula](#validate-sync-rule) bölümünde belirtildiği gibi tek bir nesne üzerinde önizleme çalıştırın.
- Yeni bir kural ekledikten veya herhangi bir özel eşitleme kuralını değiştirdikten sonra tam bir eşitleme çalıştırın. Bu eşitleme tüm nesneleriçin yeni kurallar uygular.

## <a name="change-attribute-flow"></a>Öznitelik akışını değiştirme
Öznitelik akışını değiştirmek için üç farklı senaryo vardır:
- Yeni bir öznitelik ekleme.
- Varolan bir öznitelik değerini geçersiz kılma.
- Varolan bir özniteliği eşitlememeyi seçme.

Bunları standart varsayılan kuralları değiştirmeden yapabilirsiniz.

### <a name="add-a-new-attribute"></a>Yeni bir öznitelik ekleme
Kaynak dizininizden hedef dizine bir öznitelik akmadığını fark ederseniz, bunu düzeltmek için [Azure AD Connect eşitleme: Dizin uzantılarını](how-to-connect-sync-feature-directory-extensions.md) kullanın.

Uzantılar sizin için işe yaramazsa, aşağıdaki bölümlerde açıklanan iki yeni eşitleme kuralı eklemeyi deneyin.


#### <a name="add-an-inbound-sync-rule"></a>Gelen eşitleme kuralı ekleme
Gelen eşitleme kuralı, öznitelik kaynağının bağlayıcı boşluk, hedefin ise metaverse olduğu anlamına gelir. Örneğin, şirket içi Active Directory'den Azure Etkin Dizin'e yeni bir öznitelik akışı sağlamak için yeni bir gelen eşitleme kuralı oluşturun. **Eşitleme Kuralları**Düzenleyicisi'ni başlatın, yön olarak **Gelen'i** seçin ve **yeni kural ekle'yi**seçin. 

 ![Eşitleme Kuralları Düzenleyicisi](media/how-to-connect-fix-default-rules/default3a.png)

Kuralı adlandırmak için kendi adlandırma kuralınızı izleyin. Burada, **AD Custom In**kullanın - Kullanıcı . Bu, kuralın özel bir kural olduğu ve Etkin Dizin bağlayıcısı alanından metaverse'e gelen bir kural olduğu anlamına gelir.   

 ![Gelen eşitleme kuralı oluşturma](media/how-to-connect-fix-default-rules/default3b.png)

Kuralın gelecekteki bakımının kolay olması için kuralın kendi açıklamasını sağlayın. Örneğin, açıklama kuralın amacının ne olduğuna ve neden gerekli olduğuna bağlı olabilir.

**Bağlı Sistem, Bağlı Sistem** **Nesne Türü**ve **Metaverse Object Type** alanları için seçimlerinizi yapın.

Öncelik değerini 0'dan 99'a kadar belirtin (sayı ne kadar düşükse, öncelik o kadar yüksektir). **Etiket**, **Parola Eşitlemesini Etkinleştir**ve Devre **Dışı Bırakılan** alanlar için varsayılan seçimleri kullanın.

**Kapsam filtresini** boş tutun. Bu, kuralın Active Directory Connected System ile metaverse arasında birleşen tüm nesneler için geçerli olduğu anlamına gelir.

**Birleştirme kurallarını** boş tutun. Bu, bu kuralın standart varsayılan kuralda tanımlanan birleştirme koşulunu kullandığı anlamına gelir. Bu, standart varsayılan kuralı devre dışı düşürmemek veya silmemek için başka bir nedendir. Birleştirme koşulu yoksa, öznitelik akmaz. 

Özniteliğiniz için uygun dönüşümler ekleyin. Hedef özniteliğinize sabit bir değer akışı sağlamak için sabit bir sabit atayabilirsiniz. Kaynak veya hedef özniteliği arasında doğrudan eşleme kullanabilirsiniz. Veya öznitelik için bir ifade kullanabilirsiniz. Burada kullanabileceğiniz çeşitli [ifade işlevleri](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference) verebilirsiniz.

#### <a name="add-an-outbound-sync-rule"></a>Giden eşitleme kuralı ekleme
Özniteliği hedef dizine bağlamak için giden bir kural oluşturmanız gerekir. Bu, kaynağın metaverse olduğu ve hedefin bağlı sistem olduğu anlamına gelir. Giden bir kural oluşturmak için **Eşitleme Kuralları Düzenleyicisi'ni**başlatın, **Giden Yöne Yönü** değiştirin ve yeni kural **Outbound** **ekle'yi**seçin. 

![Eşitleme Kuralları Düzenleyicisi](media/how-to-connect-fix-default-rules/default3c.png)

Gelen kuralında olduğu gibi, kuralı adlandırmak için kendi adlandırma kuralınızı kullanabilirsiniz. Azure AD kiracı olarak **Bağlı Sistem'i** seçin ve öznitelik değerini ayarlamak istediğiniz bağlı sistem nesnesini seçin. Önceliği 0'dan 99'a ayarlayın. 

![Giden eşitleme kuralı oluşturma](media/how-to-connect-fix-default-rules/default3d.png)

**Kapsam filtresini** tutun ve **Birleştirme kuralları** boş tutun. Dönüşümü sabit, doğrudan veya ifade olarak doldurun. 

Artık Active Directory'den Azure Active Directory'ye bir kullanıcı nesnesi akışı için nasıl yeni bir öznitelik yapacağınızı biliyorsunuz. Herhangi bir nesneden kaynağa ve hedefe herhangi bir öznitelik eşlemek için bu adımları kullanabilirsiniz. Daha fazla bilgi için bkz: [Özel eşitleme kuralları oluşturma](how-to-connect-create-custom-sync-rule.md) ve kullanıcıları sağlamak için [hazırlanın.](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization)

### <a name="override-the-value-of-an-existing-attribute"></a>Varolan bir özniteliğin değerini geçersiz kılma
Zaten eşlenmiş bir öznitelik değerini geçersiz kılmak isteyebilirsiniz. Örneğin, Azure AD'deki bir öznitelik için her zaman null bir değer ayarlamak istiyorsanız, yalnızca gelen bir kural oluşturmanız yeterlidir. Sabit değeri, `AuthoritativeNull`hedef özniteliğe akış olun. 

>[!NOTE] 
> Bu `AuthoritativeNull` `Null` durumda yerine kullanın. Bunun nedeni, null olmayan değerin, daha düşük önceliğe (kuraldaki daha yüksek bir sayı değeri) sahip olsa bile, null değerinin yerini almasıdır. `AuthoritativeNull`, diğer taraftan, diğer kurallar tarafından null olmayan bir değer ile değiştirilmez. 

### <a name="dont-sync-existing-attribute"></a>Varolan özniteliği eşitleme
Bir özniteliği eşitlemeden dışlamak istiyorsanız, Azure AD Connect'te sağlanan öznitelik filtreleme özelliğini kullanın. **Azure AD Connect'i** masaüstü simgesinden başlatın ve ardından **senkronize ekleme seçeneklerini özelleştir'i**seçin.

![Azure AD Ek görevler seçeneklerini bağlayın](media/how-to-connect-fix-default-rules/default4.png)

 Azure **AD uygulaması nın ve öznitelik filtrelemenin** seçildiğinden emin olun ve **İleri'yi**seçin.

![Azure AD Bağlama isteğe bağlı özellikler](media/how-to-connect-fix-default-rules/default5.png)

Eşitleme dışında hariç tutmak istediğiniz öznitelikleri temizleyin.

![Azure AD Connect öznitelikleri](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="change-scoping-filter"></a>Kapsam filtresini değiştirme
Azure AD Eşitlemi nesnelerin çoğuyla ilgilenir. Standart varsayılan eşitleme kurallarını değiştirmeden nesnelerin kapsamını azaltabilir ve dışa aktarılacak nesne sayısını azaltabilirsiniz. 

Eşitlediğiniz nesnelerin kapsamını azaltmak için aşağıdaki yöntemlerden birini kullanın:

- cloudFiltreed öznitelik
- Organizasyon birimi filtreleme

Eşitlenen kullanıcıların kapsamını azaltırsanız, filtreuygulanmış kullanıcılar için parola karma eşitleme de durur. Nesneler zaten eşitlenmişse, kapsamı azalttıktan sonra, filtreden çıkan nesneler hedef dizinden silinir. Bu nedenle, çok dikkatli bir şekilde kapsama emin olun.

>[!IMPORTANT] 
> Azure AD Connect tarafından yapılandırılan nesnelerin kapsamını artırmanız önerilmez. Bunu yapmak, Microsoft destek ekibinin özelleştirmeleri anlamasını zorlaştırır. Nesnelerin kapsamını artırmanız gerekiyorsa, varolan kuralı edin, kopyalave özgün kuralı devre dışı edin. 

### <a name="cloudfiltered-attribute"></a>cloudFiltreed öznitelik
Bu özniteliği Active Directory'de ayarlayamadığınız. Yeni bir gelen kuralı ekleyerek bu özniteliğin değerini ayarlayın. Daha sonra bu özniteliği metaverse ayarlamak için **Dönüşüm** ve **İfade** kullanabilirsiniz. Aşağıdaki örnek, departman adı **HRD** (büyük/küçük harf duyarsız) ile başlayan tüm kullanıcıları eşitlemek istemediğinizi gösterir:

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

Bölümü ilk olarak kaynaktan (Active Directory) küçük harfe dönüştürdük. Daha sonra, `Left` işlevi kullanarak, biz sadece ilk üç `hrd`karakter aldı ve ile karşılaştırıldığında . Eşleşirse, değer `True`, aksi `NULL`takdirde ayarlanır. Değeri null'a ayarlarken, daha düşük önceliğe (daha yüksek bir sayı değeri) sahip başka bir kural ona farklı bir koşulla yazabilir. [Eşitleme kuralını doğrulamak](#validate-sync-rule) için tek bir nesne üzerinde önizleme çalıştırın.

![Gelen eşitleme kuralı seçenekleri oluşturma](media/how-to-connect-fix-default-rules/default7a.png)

### <a name="organizational-unit-filtering"></a>Kuruluş birimi filtreleme
Bir veya daha fazla kuruluş birimi (OS) oluşturabilir ve bu OS'lerle eşitlemek istemediğiniz nesneleri taşıyabilirsiniz. Ardından, Azure AD Connect'te OU filtresini yapılandırın. **Azure AD Connect'i** masaüstü simgesinden başlatın ve aşağıdaki seçenekleri seçin. Azure AD Connect'in yüklenmesi sırasında OU filtresini de yapılandırabilirsiniz. 

![Azure AD Ek görevleri bağlama](media/how-to-connect-fix-default-rules/default8.png)

Sihirbazı izleyin ve eşitlemek istemediğiniz OS'leri temizleyin.

![Azure AD Connect Etki Alanı ve OU filtreleme seçenekleri](media/how-to-connect-fix-default-rules/default9.png)

## <a name="change-join-condition"></a>Birleştirme koşulunu değiştirme
Azure AD Connect tarafından yapılandırılan varsayılan birleştirme koşullarını kullanın. Varsayılan birleştirme koşullarını değiştirmek, Microsoft desteğinin özelleştirmeleri anlamasını ve ürünü desteklemesini zorlaştırır.

## <a name="validate-sync-rule"></a>Eşitleme kuralını doğrula
Önizleme özelliğini kullanarak, tam eşitleme döngüsünü çalıştırmadan yeni eklenen eşitleme kuralını doğrulayabilirsiniz. Azure AD Connect'te **Eşitleme Hizmeti'ni**seçin.

![Azure AD Connect, Senkronizasyon Hizmeti vurgulanır](media/how-to-connect-fix-default-rules/default10.png)

**Metaverse Arama'yı**seçin. Kapsam nesnesini **kişi**olarak seçin, **Yan Tümce Ekle'yi**seçin ve arama ölçütlerinizi belirtin. Ardından, **Arama'yı**seçin ve arama sonuçlarında nesneyi çift tıklatın. Azure AD Connect'teki verilerinizin, bu adımı çalıştırmadan önce ormanda içe aktarma ve eşitleme çalıştırarak bu nesne için güncel olduğundan emin olun.

![Eşitleme Hizmeti Yöneticisi](media/how-to-connect-fix-default-rules/default11.png)

**Metaverse Nesne**Özellikleri'nde, **Bağlayıcılar'ı**seçin, ilgili bağlayıcıdaki nesneyi (orman) seçin ve **Özellikler'i seçin... seçeneğini belirleyin.**

![Metaverse Nesne Özellikleri](media/how-to-connect-fix-default-rules/default12.png)

**Önizleme'yi seçin...**

![Bağlayıcı Alan Nesne Özellikleri](media/how-to-connect-fix-default-rules/default13a.png)

Önizleme penceresinde, sol bölmede **Önizleme** oluştur ve **Öznitelik Akışını İçe'yi** seçin.

![Önizleme](media/how-to-connect-fix-default-rules/default14.png)
 
Burada, yeni eklenen kuralın nesne üzerinde çalıştırıldığını `cloudFiltered` ve özniteliği doğru olarak ayarladığını unutmayın.

![Önizleme](media/how-to-connect-fix-default-rules/default15a.png)
 
Değiştirilen kuralı varsayılan kuralla karşılaştırmak için, her iki kuralı da metin dosyaları olarak ayrı ayrı dışa aktarın. Bu kurallar PowerShell komut dosyası dosyası olarak dışa aktarılır. Değişiklikleri görmek için herhangi bir dosya karşılaştırma aracını (örneğin, windiff) kullanarak karşılaştırabilirsiniz. 
 
Değiştirilen kuralda öznitelik `msExchMailboxGuid` **Doğrudan**yerine **İfade** türüne değiştirilir dikkat edin. Ayrıca, değer **NULL** ve **ExecuteOnce** seçeneği olarak değiştirilir. Tanımlanan ve Öncelik farklılıklarını yoksayabilirsiniz. 

![windiff araç çıktısı](media/how-to-connect-fix-default-rules/default17.png)
 
Kurallarınızı varsayılan ayarlara geri değiştirmek için düzeltmek için değiştirilen kuralı silin ve varsayılan kuralı etkinleştirin. Ulaşmaya çalıştığınız özelleştirmeyi kaybetmemenizi sağlayın. Hazır olduğunuzda, Tam **Eşitleme'yi çalıştırın.**

## <a name="next-steps"></a>Sonraki adımlar
- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Ekspres ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)



