---
title: Değiştirilen varsayılan kuralları çözme-Azure AD Connect | Microsoft Docs
description: Azure AD Connect ile birlikte gelen değiştirilmiş varsayılan kuralları nasıl düzelteceğinizi öğrenin.
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
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79036974"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Azure AD Connect değiştirilen varsayılan kuralları çözme

Azure Active Directory (Azure AD) Connect, eşitleme için varsayılan kuralları kullanır.  Ne yazık ki bu kurallar tüm kuruluşlara evrensel olarak uygulanmaz. Gereksinimlerinize göre, bunları değiştirmeniz gerekebilir. Bu makalede en yaygın özelleştirmelerin iki örneği ele alınmaktadır ve bu özelleştirmeler elde etmenin doğru yolu açıklanmaktadır.

>[!NOTE] 
> Gerekli özelleştirmeyi başarmak için mevcut varsayılan kuralları değiştirme desteklenmez. Bunu yaparsanız, bu kuralların sonraki sürümlerde en son sürüme güncelleştirilmesini engeller. İhtiyaç duyduğunuz hata düzeltmelerini veya yeni özellikleri almazsınız. Bu belgede, varolan varsayılan kuralları değiştirmeden aynı sonuca nasıl ulaşacağı açıklanmaktadır. 

## <a name="how-to-identify-modified-default-rules"></a>Değiştirilen varsayılan kuralları belirleme
Azure AD Connect sürüm 1.3.7.0 başlayarak, değiştirilen varsayılan kuralı kolayca belirlemek kolaydır. **Masaüstündeki uygulamalar**' a gidin ve **eşitleme kuralları Düzenleyicisi**' ni seçin.

![Azure AD Connect, eşitleme kuralları Düzenleyicisi vurgulanmış olarak](media/how-to-connect-fix-default-rules/default1.png)

Düzenleyicide, değiştirilen tüm varsayılan kurallar adının önünde bir uyarı simgesiyle gösterilir.

![Uyarı simgesi](media/how-to-connect-fix-default-rules/default2.png)

 Aynı ada sahip devre dışı bırakılmış bir kural de görünür (Bu, standart varsayılan kuraldır).

![Standart varsayılan kuralı ve değiştirilen varsayılan kuralı gösteren eşitleme kuralları Düzenleyicisi](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Ortak özelleştirmeler
Varsayılan kurallar için ortak özelleştirmeler aşağıda verilmiştir:

- Öznitelik akışını Değiştir
- Kapsam filtresini Değiştir
- JOIN koşulunu Değiştir

Herhangi bir kuralı değiştirmeden önce:

- Eşitleme zamanlayıcısını devre dışı bırakın. Zamanlayıcı, varsayılan olarak her 30 dakikada bir çalışır. Değişiklik yaparken ve yeni kurallarınızın sorunlarını giderirken başlatılmadığından emin olun. Zamanlayıcı 'yı geçici olarak devre dışı bırakmak için PowerShell 'i başlatın ve `Set-ADSyncScheduler -SyncCycleEnabled $false`çalıştırın.
 Eşitleme zamanlayıcısını devre dışı bırakmak için PowerShell komutlarını ![](media/how-to-connect-fix-default-rules/default3.png)

- Kapsam filtresindeki değişiklik, hedef dizindeki nesnelerin silinmesine neden olabilir. Nesnelerin kapsamı içinde herhangi bir değişiklik yapmadan önce dikkatli olun. Etkin sunucuda değişiklik yapmadan önce bir hazırlama sunucusunda değişiklik yapmanızı öneririz.
- Yeni kural eklendikten sonra, [eşitleme kuralını doğrula](#validate-sync-rule) bölümünde belirtildiği gibi tek bir nesne üzerinde önizleme çalıştırın.
- Yeni bir kural ekledikten veya herhangi bir özel eşitleme kuralını değiştirdikten sonra tam eşitleme çalıştırın. Bu eşitleme, tüm nesnelere yeni kurallar uygular.

## <a name="change-attribute-flow"></a>Öznitelik akışını Değiştir
Öznitelik akışını değiştirmek için üç farklı senaryo vardır:
- Yeni bir öznitelik ekleniyor.
- Varolan bir özniteliğin değerini geçersiz kılma.
- Mevcut bir özniteliğin eşitlenmemelidir.

Bunları standart varsayılan kuralları değiştirmeden yapabilirsiniz.

### <a name="add-a-new-attribute"></a>Yeni öznitelik Ekle
Bir özniteliğin kaynak dizininizden hedef dizine akan olmadığını fark ederseniz, bu işlemi onarmak için [Azure AD Connect Sync: Directory Extensions](how-to-connect-sync-feature-directory-extensions.md) kullanın.

Uzantılar sizin için çalışmazsa, aşağıdaki bölümlerde açıklanan iki yeni eşitleme kuralı eklemeyi deneyin.


#### <a name="add-an-inbound-sync-rule"></a>Gelen eşitleme kuralı ekleme
Bir gelen eşitleme kuralı, özniteliğin kaynağının bir bağlayıcı alanı olduğu ve hedef metadize olduğu anlamına gelir. Örneğin, şirket içi Active Directory yeni bir öznitelik akışına Azure Active Directory için, yeni bir gelen eşitleme kuralı oluşturun. **Eşitleme kuralları düzenleyicisini**başlatın, yön olarak **gelen** ' ı seçin ve **Yeni kural ekle**' yi seçin. 

 ![Eşitleme Kuralları Düzenleyicisi](media/how-to-connect-fix-default-rules/default3a.png)

Kuralı adlandırmak için kendi adlandırma kuralınızı izleyin. Burada, **ad-User ' dan özel**' i kullanırız. Bu, kuralın özel bir kural olduğu ve Active Directory bağlayıcı alanından metadize 'ye gelen bir gelen kural olduğu anlamına gelir.   

 ![Gelen eşitleme kuralı oluştur](media/how-to-connect-fix-default-rules/default3b.png)

Kuralın gelecekteki bakımının kolay olması için kuralın kendi açıklamasını sağlayın. Örneğin, açıklama kuralın amacının ne olduğunu ve neden gerekli olduğunu temel alabilir.

**Bağlı sistem**, **bağlı sistem nesne türü**ve meta veri deposu **nesne türü** alanları için seçimlerinizi yapın.

0 ile 99 arasında bir öncelik değeri belirtin (sayı ne kadar düşükse, öncelik o kadar yüksektir). **Etiket**Için **parola eşitlemesini etkinleştirin**ve **devre dışı** alanları, varsayılan seçimleri kullanın.

**Kapsam filtresi** boş tut. Bu, kuralın Active Directory bağlı sistem ile meta veri deposu arasında katılmış tüm nesneler için geçerli olduğu anlamına gelir.

**JOIN kurallarını** boş tut. Bu, kuralın standart varsayılan kuralda tanımlanan JOIN koşulunu kullandığı anlamına gelir. Bu, standart varsayılan kuralı devre dışı bırakmamalıdır veya silmemelidir. Hiçbir JOIN koşulu yoksa, öznitelik akamaz. 

Öznitelindeki uygun dönüştürmeleri ekleyin. Hedef özniteliyorsanız sabit bir değer akışı yapmak için bir sabit atayabilirsiniz. Kaynak veya hedef öznitelik arasında doğrudan eşlemeyi kullanabilirsiniz. Ya da özniteliği için bir ifade kullanabilirsiniz. Kullanabileceğiniz çeşitli [ifade işlevleri](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference) aşağıda verilmiştir.

#### <a name="add-an-outbound-sync-rule"></a>Giden eşitleme kuralı ekleme
Özniteliği hedef dizine bağlamak için bir giden kuralı oluşturmanız gerekir. Bu, kaynağın metadize olduğu ve hedefin bağlı sistem olduğu anlamına gelir. Bir giden kuralı oluşturmak için, **eşitleme kuralları düzenleyicisini**başlatın, **yönü** **giden**olarak değiştirin ve **Yeni kural ekle**' yi seçin. 

![Eşitleme Kuralları Düzenleyicisi](media/how-to-connect-fix-default-rules/default3c.png)

Gelen kuralında olduğu gibi, kuralı adlandırmak için kendi adlandırma kuralınızı de kullanabilirsiniz. **Bağlı sistemi** Azure AD kiracısı olarak seçin ve öznitelik değerini ayarlamak istediğiniz bağlı sistem nesnesini seçin. 0 ile 99 arasında önceliği ayarlayın. 

![Giden eşitleme kuralı oluştur](media/how-to-connect-fix-default-rules/default3d.png)

**Kapsam filtre** ve **ekleme kurallarının** boş kalmasını sağlayın. Dönüştürmeyi sabit, doğrudan veya ifade olarak girin. 

Artık Active Directory bir kullanıcı nesnesi için yeni bir öznitelik oluşturmayı Azure Active Directory. Herhangi bir nesneden kaynak ve hedefe herhangi bir öznitelik eşlemek için bu adımları kullanabilirsiniz. Daha fazla bilgi için bkz. [özel eşitleme kuralları oluşturma](how-to-connect-create-custom-sync-rule.md) ve [kullanıcıları sağlamaya hazırlanma](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization).

### <a name="override-the-value-of-an-existing-attribute"></a>Varolan bir özniteliğin değerini geçersiz kıl
Zaten eşlenmiş bir özniteliğin değerini geçersiz kılmak isteyebilirsiniz. Örneğin, her zaman Azure AD 'deki bir özniteliğe null değer ayarlamak istiyorsanız yalnızca bir gelen kuralı oluşturmanız yeterlidir. Sabit değeri `AuthoritativeNull`, hedef özniteliğe akış yapın. 

>[!NOTE] 
> Bu durumda `Null` yerine `AuthoritativeNull` kullanın. Bunun nedeni, null olmayan değerin, daha düşük önceliğe sahip olsa bile null değeri değiştirdiği (kuraldaki daha yüksek bir sayı değeri). diğer yandan `AuthoritativeNull`, diğer kurallara göre null olmayan bir değerle değiştirilmez. 

### <a name="dont-sync-existing-attribute"></a>Var olan özniteliği eşitleme
Bir özniteliği eşitlemeden dışlamak istiyorsanız, Azure AD Connect içinde belirtilen öznitelik filtreleme özelliğini kullanın. Masaüstü simgesinden **Azure AD Connect** başlatın ve ardından **eşitleme seçeneklerini Özelleştir**' i seçin.

![Ek görev seçeneklerini Azure AD Connect](media/how-to-connect-fix-default-rules/default4.png)

 **Azure AD uygulaması ve öznitelik filtrelemesinin** seçili olduğundan emin olun ve **İleri**' yi seçin.

![İsteğe bağlı özellikler Azure AD Connect](media/how-to-connect-fix-default-rules/default5.png)

Eşitlemeden dışlamak istediğiniz öznitelikleri temizleyin.

![Azure AD Connect öznitelikleri](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="change-scoping-filter"></a>Kapsam filtresini Değiştir
Azure AD Eşitleme nesnelerden çoğu zaman sürer. Nesnelerin kapsamını azaltabilir ve standart varsayılan eşitleme kurallarını değiştirmeden, aktarılacak nesne sayısını azaltabilirsiniz. 

Eşitmekte olduğunuz nesnelerin kapsamını azaltmak için aşağıdaki yöntemlerden birini kullanın:

- Cloudfiltrelenmiş özniteliği
- Kuruluş birimi filtreleme

Eşitlenen kullanıcıların kapsamını azaldıysanız, Parola karması eşitlemesi de filtrelenmiş kullanıcılar için de duraklar. Nesneler zaten eşitleniyorsa, kapsamı azaltdıktan sonra filtrelenmiş nesneler hedef dizinden silinir. Bu nedenle, çok dikkatli bir şekilde kapsamınız olduğundan emin olun.

>[!IMPORTANT] 
> Azure AD Connect tarafından yapılandırılan nesnelerin kapsamını artırma önerilmez. Bunun yapılması, Microsoft destek ekibinin özelleştirmeleri anlamasına zorlaştırıyor. Nesnelerin kapsamını artırmanız gerekiyorsa, var olan kuralı düzenleyin, kopyalayın ve özgün kuralı devre dışı bırakın. 

### <a name="cloudfiltered-attribute"></a>Cloudfiltrelenmiş özniteliği
Bu özniteliği Active Directory ayarlayamazsınız. Bu özniteliğin değerini yeni bir gelen kuralı ekleyerek ayarlayın. Daha sonra bu özniteliği meta veri deposunda ayarlamak için **dönüştürme** ve **ifade** kullanabilirsiniz. Aşağıdaki örnek, Bölüm adı **HRD** (büyük/küçük harf duyarsız) ile başlayan tüm kullanıcıları eşitlemek istemediğinizi gösterir:

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

Önce departmanı kaynaktan (Active Directory) küçük harfe dönüştürüyoruz. Daha sonra, `Left` işlevini kullanarak yalnızca ilk üç karakteri aldık ve bunu `hrd`karşılaştırdık. Eşleşirse, değer `True`olarak ayarlanır, aksi takdirde `NULL`. Değeri null olarak ayarlamak için, daha düşük önceliğe sahip başka bir kural (daha yüksek bir sayı değeri), farklı bir koşula göre yazılabilir. [Eşitleme kuralını doğrula bölümünde belirtildiği](#validate-sync-rule) gibi eşitleme kuralını doğrulamak için bir nesnede önizlemeyi çalıştırın.

![Gelen eşitleme kuralı seçeneklerini oluşturma](media/how-to-connect-fix-default-rules/default7a.png)

### <a name="organizational-unit-filtering"></a>Kuruluş birimi filtreleme
Bir veya daha fazla kuruluş birimi (OU) oluşturabilir ve bu OU 'Lara eşitlemek istemediğiniz nesneleri taşıyabilirsiniz. Ardından, Azure AD Connect ' de OU filtrelemesini yapılandırın. Masaüstü simgesinden **Azure AD Connect** başlatın ve aşağıdaki seçenekleri belirleyin. Ayrıca, Azure AD Connect yükleme sırasında OU filtresini yapılandırabilirsiniz. 

![Ek görevler Azure AD Connect](media/how-to-connect-fix-default-rules/default8.png)

Sihirbazı izleyin ve eşitlemek istemediğiniz OU 'Ları temizleyin.

![Azure AD Connect etki alanı ve OU filtreleme seçenekleri](media/how-to-connect-fix-default-rules/default9.png)

## <a name="change-join-condition"></a>JOIN koşulunu Değiştir
Azure AD Connect tarafından yapılandırılan varsayılan JOIN koşullarını kullanın. Varsayılan JOIN koşullarını değiştirmek, Microsoft desteğinin özelleştirmeleri anlamasına ve ürünü desteklemeye zorlaştırmasına neden olur.

## <a name="validate-sync-rule"></a>Eşitleme kuralını doğrula
Yeni eklenen eşitleme kuralını, tam eşitleme döngüsünü çalıştırmadan önizleme özelliğini kullanarak doğrulayabilirsiniz. Azure AD Connect, **eşitleme hizmeti**' ni seçin.

![Azure AD Connect, eşitleme hizmeti vurgulanmış olarak](media/how-to-connect-fix-default-rules/default10.png)

**Metadize aramasını**seçin. Kapsam nesnesini **kişi**olarak seçin, **yan tümce Ekle**' yi seçin ve arama ölçütlerinizle bahsetin. Ardından **Ara**' yı seçin ve arama sonuçlarında nesneye çift tıklayın. Bu adımı çalıştırmadan önce, ormanda içeri aktar ve Eşitle ' yi çalıştırarak, bu nesne için Azure AD Connect verilerinizin güncel olduğundan emin olun.

![Eşitleme Hizmeti Yöneticisi](media/how-to-connect-fix-default-rules/default11.png)

Meta veri deposu **nesne özellikleri**üzerinde **Bağlayıcılar**' ı seçin, ilgili bağlayıcıdaki (orman) nesneyi seçin ve Özellikler ' i seçin. **..**

![Meta Veri Deposu Nesne Özellikleri](media/how-to-connect-fix-default-rules/default12.png)

**Önizleme seç...**

![Bağlayıcı Alanı Nesne Özellikleri](media/how-to-connect-fix-default-rules/default13a.png)

Önizleme penceresinde, sol bölmedeki **Önizleme Oluştur** ve **öznitelik akışını içeri aktar** ' ı seçin.

![Önizleme](media/how-to-connect-fix-default-rules/default14.png)
 
Burada, yeni eklenen kuralın nesnede çalıştırıldığına ve `cloudFiltered` özniteliğini true olarak ayarlamış olduğuna dikkat edin.

![Önizleme](media/how-to-connect-fix-default-rules/default15a.png)
 
Değiştirilen kuralı varsayılan kuralla karşılaştırmak için, her iki kuralı da metin dosyası olarak ayrı dışarı aktarın. Bu kurallar bir PowerShell betik dosyası olarak verilir. Değişiklikleri görmek için herhangi bir dosya karşılaştırma aracını (örneğin, Windiff) kullanarak bunları karşılaştırabilirsiniz. 
 
Değiştirilen kuralda, `msExchMailboxGuid` özniteliğinin **doğrudan**değil **ifade** türüyle değiştirildiğini unutmayın. Ayrıca, değer **null** ve **executeonce** seçeneği olarak değişir. Tanımlanan ve öncelik farklılıklarını yoksayabilirsiniz. 

![Windiff aracı çıkışı](media/how-to-connect-fix-default-rules/default17.png)
 
Kurallarınızı varsayılan ayarlara geri değiştirecek şekilde onarmak için, değiştirilen kuralı silin ve varsayılan kuralı etkinleştirin. Elde edilmeye çalıştığınız özelleştirmeyi kaybetmemeniz gerekir. Hazırsanız **tam eşitleme**' yi çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)



