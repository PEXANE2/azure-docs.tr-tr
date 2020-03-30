---
title: Grup tabanlı lisanslama ek senaryoları - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory grup tabanlı lisanslama için daha fazla senaryo
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 139d7e0cf2b57cc466dc97370b90a599257ce755
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266292"
---
# <a name="scenarios-limitations-and-known-issues-using-groups-to-manage-licensing-in-azure-active-directory"></a>Azure Etkin Dizini'nde lisanslamayı yönetmek için grupları kullanarak senaryolar, sınırlamalar ve bilinen sorunlar

Azure Etkin Dizin (Azure AD) grup tabanlı lisanslama hakkında daha gelişmiş bir anlayış elde etmek için aşağıdaki bilgileri ve örnekleri kullanın.

## <a name="usage-location"></a>Kullanım konumu

Bazı Microsoft hizmetleri tüm konumlarda kullanılamaz. Lisansın bir kullanıcıya atanmadan önce, yöneticinin kullanıcıüzerindeki **Kullanım konumu** özelliğini belirtilmesi gerekir. [Azure portalında,](https://portal.azure.com) **Kullanıcı** &gt; **Profil** &gt; **Ayarları'nda**kullanım konumunu belirtebilirsiniz.

Grup lisans ataması için, kullanım yeri belirtilmeyen kullanıcılar dizinin konumunu devralır. Birden çok konumda kullanıcılarınız varsa, kullanıcıları lisanslı gruplara eklemeden önce bunu kullanıcı kaynaklarınızda doğru şekilde yansıttığınızdan emin olun.

> [!NOTE]
> Grup lisans ataması, kullanıcı üzerinde varolan bir kullanım konumu değerini asla değiştirmez. Kullanım konumunu her zaman Azure AD'deki kullanıcı oluşturma akışının bir parçası olarak ayarlamanızı öneririz (örneğin, AAD Connect yapılandırması yoluyla) - lisans atamasının sonucunun her zaman doğru olmasını ve kullanıcıların izin verilmeyen konumlarda hizmet almamasını sağlar.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>Dinamik gruplarla grup tabanlı lisanslama kullanma

Grup tabanlı lisanslamayı herhangi bir güvenlik grubuyla kullanabilirsiniz, bu da Azure AD dinamik gruplarıyla birleştirilebildiği anlamına gelir. Dinamik gruplar, kullanıcıları gruplardan otomatik olarak eklemek ve kaldırmak için kullanıcı kaynak özniteliklerine karşı kurallar çalıştırın.

Örneğin, kullanıcılara atamak istediğiniz bazı ürün kümeleri için dinamik bir grup oluşturabilirsiniz. Her grup, kullanıcıları özniteliklerine göre ekleyen bir kuralla doldurulur ve her gruba almalarını istediğiniz lisanslar atanır. Özniteliği şirket içinde atayabilir ve Azure AD ile senkronize edebilir veya özniteliği doğrudan bulutta yönetebilirsiniz.

Lisanslar, gruba eklendikten kısa bir süre sonra kullanıcıya atanır. Öznitelik değiştirildiğinde, kullanıcı grupları terk eder ve lisanslar kaldırılır.

### <a name="example"></a>Örnek

Hangi kullanıcıların Microsoft web hizmetlerine erişmesi gerektiğine karar veren şirket içi kimlik yönetimi çözümü örneğini göz önünde bulundurun. Kullanıcının sahip olması gereken lisansları temsil eden bir dize değerini depolamak için **extensionAttribute1** kullanır. Azure AD Connect, Azure AD ile senkronize eder.

Kullanıcıların bir lisansa ihtiyacı olabilir, ancak diğerine gerek meyebilir veya her ikisine de gereksinim duyabilir. Aşağıda, Office 365 Enterprise E5 ve Enterprise Mobility + Security (EMS) lisanslarını gruplar halinde kullanıcılara dağıttığınız bir örnek verilmiştir:

#### <a name="office-365-enterprise-e5-base-services"></a>Office 365 Kurumsal E5: temel hizmetler

![Office 365 Enterprise E5 taban hizmetlerinin ekran görüntüsü](./media/licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Kurumsal Mobilite + Güvenlik: lisanslı kullanıcılar

![Kurumsal Mobilite + Güvenlik lisanslı kullanıcıların ekran görüntüsü](./media/licensing-group-advanced/o365-e5-licensed-users.png)

Bu örnekte, bir kullanıcıyı değiştirin ve uzantısı `EMS;E5_baseservices;` Attribute1'i kullanıcının her iki lisansa da sahip olmasını istiyorsanız değerine ayarlayın. Bu değişikliği şirket içinde yapabilirsiniz. Değişiklik bulutla eşitlendikten sonra, kullanıcı otomatik olarak her iki gruba da eklenir ve lisanslar atanır.

![Kullanıcının uzantısıAttribute1 ayarlamak için nasıl gösteren ekran görüntüsü](./media/licensing-group-advanced/user-set-extensionAttribute1.png)

> [!WARNING]
> Varolan bir grubun üyelik kuralını değiştirirken dikkatli olun. Bir kural değiştirildiğinde, grubun üyeliği yeniden değerlendirilir ve artık yeni kuralla eşleşmeyen kullanıcılar kaldırılır (bu işlem sırasında yeni kuralla hala eşleşen kullanıcılar etkilenmez). Bu kullanıcıların lisansları hizmet kaybına veya bazı durumlarda veri kaybına neden olabilecek işlem sırasında kaldırılacaktır.
> 
> Lisans ataması için bağlı olduğunuz büyük bir dinamik grubunuz varsa, bunları ana gruba uygulamadan önce daha küçük bir test grubundaki önemli değişiklikleri doğrulamayı düşünün.

## <a name="multiple-groups-and-multiple-licenses"></a>Birden fazla grup ve birden çok lisans

Kullanıcı, lisansları olan birden çok grubun üyesi olabilir. Göz önünde bulundurulması gereken bazı şeyler şunlardır:

- Aynı ürün için birden çok lisans çakışabilir ve bu lisanslar tüm etkinleştirilen hizmetlerin kullanıcıya uygulanmasına neden olabilir. Aşağıdaki örnekte iki lisans grubu gösterilmektedir: *E3 temel hizmetleri,* ilk olarak tüm kullanıcılara dağıtılabilmek için temel hizmetleri içerir. Ve *E3 genişletilmiş hizmetler* yalnızca bazı kullanıcılara dağıtmak için ek hizmetler (Sway ve Planlayıcı) içerir. Bu örnekte, kullanıcı her iki gruba da eklendi:

  ![Etkin hizmetlerin ekran görüntüsü](./media/licensing-group-advanced/view-enabled-services.png)

  Sonuç olarak, kullanıcı, bu ürün için yalnızca bir lisans kullanırken, etkinleştirilen üründeki 12 hizmetten 7'ye sahiptir.

- *E3* lisansının seçilmesi, grup lisans ataması tarafından kullanıcı için hangi hizmetlerin etkinleştirildiğine ilişkin bilgiler de dahil olmak üzere daha fazla ayrıntı gösterir.

## <a name="direct-licenses-coexist-with-group-licenses"></a>Doğrudan lisanslar grup lisansları ile birlikte var

Bir kullanıcı bir gruptan lisans devraldığında, kullanıcının özelliklerindeki bu lisans atamasını doğrudan kaldıramaz veya değiştiremezsiniz. Değişiklikler grupta yapılmalı ve daha sonra tüm kullanıcılara yayılmalıdır.

Ancak, devralınan lisansa ek olarak aynı ürün lisansını doğrudan kullanıcıya atamak mümkündür. Diğer kullanıcıları etkilemeden yalnızca bir kullanıcı için üründen ek hizmetler etkinleştirebilirsiniz.

Doğrudan atanan lisanslar kaldırılabilir ve devralınan lisansları etkilemez. Bir gruptan Office 365 Enterprise E3 lisansını devralan kullanıcıyı düşünün.

Başlangıçta, kullanıcı lisansı yalnızca dört hizmet planı sağlayan *E3 temel hizmetler* grubundan devralır.

1. Kullanıcıya doğrudan E3 lisansı atamak için **Atama'yı** seçin. Bu durumda, Yammer Enterprise hariç tüm hizmet planlarını devre dışı bırakın.

    Sonuç olarak, kullanıcı hala E3 ürün sadece bir lisans kullanır. Ancak doğrudan atama, yalnızca o kullanıcı için Yammer Enterprise hizmetini sağlar. Hangi hizmetlerin grup üyeliği tarafından doğrudan atamaya karşı etkinleştirildigini görebilirsiniz.

1. Doğrudan atama kullandığınızda, aşağıdaki işlemlere izin verilir:

   - Yammer Enterprise doğrudan kullanıcı kaynağında kapatılabilir. Diğer hizmet geçişlerinin aksine, bu hizmet için çizimdeki **Açık/Kapalı** geçiş etkinleştirildi. Hizmet doğrudan kullanıcı üzerinde etkinleştirildiğinden, değiştirilebilir.
   - Doğrudan atanan lisansın bir parçası olarak ek hizmetler de etkinleştirilebilir.
   - **Kaldır** düğmesi, doğrudan lisansı kullanıcıdan kaldırmak için kullanılabilir. Kullanıcının artık yalnızca devralınan grup lisansına sahip olduğunu ve yalnızca özgün hizmetlerin etkin kaldığını görebilirsiniz:

## <a name="managing-new-services-added-to-products"></a>Ürünlere eklenen yeni hizmetleri yönetme

Microsoft, ürün lisans planına yeni bir hizmet eklediğinde, ürün lisansını atadığınız tüm gruplarda varsayılan olarak etkinleştirilir. Ürün değişiklikleriyle ilgili bildirimlere abone olan kiracınızdaki kullanıcılar, yaklaşan hizmet eklemeleri hakkında bildirimde bulunan e-postaları önceden alır.

Yönetici olarak, değişiklikten etkilenen tüm grupları gözden geçirebilir ve her gruptaki yeni hizmeti devre dışı bırakmak gibi bir eylemde olabilirsiniz. Örneğin, yalnızca dağıtım için belirli hizmetleri hedefleyen gruplar oluşturduysanız, bu grupları yeniden ziyaret edebilir ve yeni eklenen hizmetlerin devre dışı bırakıldığından emin olabilirsiniz.

Aşağıda, bu işlemin nasıl görünebileceğine bir örnek verilmiştir:

1. Başlangıçta, *Office 365 Enterprise E5* ürünlerini çeşitli gruplara atadınız. *O365 E5 - Exchange* adı verilen bu gruplardan biri yalnızca üyeleri için yalnızca *Exchange Online (Plan 2)* hizmetini etkinleştirmek üzere tasarlanmıştır.

2. Microsoft'tan, E5 ürünürünün yeni bir hizmetle genişletileceğine dair bir bildirim aldınız - *Microsoft Stream*. Hizmet kiracınızda kullanıma sunulduğunda aşağıdakileri yapabilirsiniz:

3. [**Azure Etkin Dizin > Lisansları > Tüm ürünlerblade**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) gidin ve *Office 365 Enterprise E5*seçin, ardından bu ürüne sahip tüm grupların listesini görüntülemek için **Lisanslı Gruplar'ı** seçin.

4. İncelemek istediğiniz gruba tıklayın (bu durumda, *O365 E5 - Yalnızca exchange).* Bu, **Lisanslar** sekmesiaçılır. E5 lisansına tıkladığınızda, etkin olan tüm hizmetleri listeleyen bir bıçak açılır.
   > [!NOTE]
   > *Microsoft Stream* hizmeti, *Exchange Online* hizmetine ek olarak bu gruba otomatik olarak eklenmiştir ve etkinleştirilmiştir:

   ![Grup lisansına eklenen yeni hizmetin ekran görüntüsü](./media/licensing-group-advanced/manage-new-services.png)

5. Bu gruptaki yeni hizmeti devre dışı atmak istiyorsanız, hizmetin yanındaki **Aç/Kapama** düğmesini tıklatın ve değişikliği onaylamak için **Kaydet** düğmesini tıklatın. Azure AD artık değişikliği uygulamak için gruptaki tüm kullanıcıları işleyecek; gruba eklenen yeni kullanıcılar *Microsoft Stream* hizmetini etkinleştirmez.

   > [!NOTE]
   > Kullanıcılar, başka bir lisans ataması (üyesi oldukları başka bir grup veya doğrudan lisans ataması) aracılığıyla hizmeti etkinleştirebilir.

6. Gerekirse, atanan bu ürünile diğer gruplar için aynı adımları gerçekleştirin.

## <a name="use-powershell-to-see-who-has-inherited-and-direct-licenses"></a>Lisansları kimin devrettiğini ve doğrudan kullandığını görmek için PowerShell'i kullanın
Kullanıcıların doğrudan atanmış bir lisansa sahip olup olmadığını veya bir gruptan devralınıp alınıp alınamamış olup olmadığını kontrol etmek için powershell komut dosyası kullanabilirsiniz.

1. Kimlik `connect-msolservice` doğrulaması yapmak ve kiracınıza bağlanmak için cmdlet'i çalıştırın.

2. `Get-MsolAccountSku`kiracıtüm sağlanan ürün lisansları keşfetmek için kullanılabilir.

   ![Get-Msolaccountsku cmdlet ekran görüntüsü](./media/licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. [Bu PowerShell komut dosyası](licensing-ps-examples.md#check-if-user-license-is-assigned-directly-or-inherited-from-a-group)ile ilgilendiğiniz lisans için *AccountSkuId* değerini kullanın. Bu, lisansın nasıl atandığı yla ilgili bilgileri içeren bu lisansa sahip kullanıcıların bir listesini üretir.

## <a name="use-audit-logs-to-monitor-group-based-licensing-activity"></a>Grup tabanlı lisanslama etkinliğini izlemek için Denetim günlüklerini kullanma

Aşağıdakiler de dahil olmak üzere grup tabanlı lisanslamayla ilgili tüm etkinlikleri görmek için [Azure AD denetim günlüklerini](../reports-monitoring/concept-audit-logs.md#audit-logs) kullanabilirsiniz:
- gruplarda lisans ları değiştirenler
- sistem bir grup lisans değişikliğini işlemeye başladığında ve bittiğinde
- grup lisans ataması sonucunda kullanıcıya hangi lisans değişiklikleri nin yapıldığı.

>[!NOTE]
> Denetim günlükleri, portalın Azure Etkin Dizin bölümünde çoğu bıçakta kullanılabilir. Bunlara nereden eriştiğinize bağlı olarak, filtreler yalnızca bıçağın bağlamıyla ilgili etkinliği göstermek için önceden uygulanabilir. Beklediğiniz sonuçları görmüyorsanız, Azure [**Etkin Dizin > Etkinlik > Denetim günlükleri**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Audit)altında [filtrelenmemiş](../reports-monitoring/concept-audit-logs.md#filtering-audit-logs) denetim günlüklerine erişin.

### <a name="find-out-who-modified-a-group-license"></a>Grup lisansLarını kimin değiştirip değiştirip değiştirtti

1. *Grup lisansını ayarla* ve **Uygula'yı**tıklatacak **etkinlik** filtresini ayarlayın.
2. Sonuçlar, gruplar üzerinde ayarlanan veya değiştirilen tüm lisans durumlarını içerir.
   >[!TIP]
   > Sonuçları kapsamak için *Hedef* filtresine grubun adını da yazabilirsiniz.

3. Değiştirilenlerin ayrıntılarını görmek için listede bir öğe seçin. *Değiştirilen Özellikler* altında lisans ataması için hem eski hem de yeni değerler listelenir.

Burada ayrıntıları ile son grup lisans değişiklikleri, bir örnektir:

![Ekran görüntüsü grubu lisans değişiklikleri](./media/licensing-group-advanced/audit-group-license-change.png)

### <a name="find-out-when-group-changes-started-and-finished-processing"></a>Grup değişikliklerinin ne zaman başladığını ve işlemenin ne zaman tamamlanabildiğini öğrenin

Bir grup üzerinde lisans değiştiğinde, Azure AD değişiklikleri tüm kullanıcılara uygulamaya başlar.

1. Grupların ne zaman işlemeye başladığını görmek **için, Etkinlik** filtresini *kullanıcılara grup tabanlı lisansı uygulamayı başlatacak*şekilde ayarlayın. İşlemin aktörü, tüm grup lisans değişikliklerini yürütmek için kullanılan bir sistem hesabı olan *Microsoft Azure AD Grubu Tabanlı Lisanslama* olduğunu unutmayın.
   >[!TIP]
   > *Değiştirilen Özellikler* alanını görmek için listedeki bir öğeyi tıklatın - işlenmek üzere alınan lisans değişikliklerini gösterir. Bu, bir grupta birden çok değişiklik yaptıysanız ve hangisinin işlendiğinden emin değilseniz yararlıdır.

2. Benzer şekilde, grupların işlemeyi ne zaman tamamlayıp bitirdiğini görmek *için, kullanıcılara grup tabanlı lisans uygulayarak filtre*değerini kullanın.
   > [!TIP]
   > Bu durumda, *Değiştirilen Özellikler* alanı sonuçların bir özetini içerir - bu işlem herhangi bir hata yla sonuçlanmış olup olmadığını hızlı bir şekilde kontrol etmek için yararlıdır. Örnek çıktı:
   > ```
   > Modified Properties
   > ...
   > Name : Result
   > Old Value : []
   > New Value : [Users successfully assigned licenses: 6, Users for whom license assignment failed: 0.];
   > ```

3. Tüm kullanıcı değişiklikleri de dahil olmak üzere bir grubun nasıl işlendiğine dair tam günlüğü görmek için aşağıdaki filtreleri ayarlayın:
   - **Başlatılan Tarafından (Aktör)**: "Microsoft Azure AD Grup Tabanlı Lisanslama"
   - **Tarih Aralığı** (isteğe bağlı): belirli bir grubun işe başlayıp işlemden bittiğini bildiğiniz zamanlar için özel aralık

Bu örnek çıktı, işlemenin başlangıcını, ortaya çıkan tüm kullanıcı değişikliklerini ve işlemin tamamlanmasını gösterir.

![Ekran görüntüsü grubu lisans değişiklikleri](./media/licensing-group-advanced/audit-group-processing-log.png)

>[!TIP]
> *Kullanıcı lisansLarını Değiştir* ile ilgili öğeleri tıklatmak, her bir kullanıcıya uygulanan lisans değişikliklerinin ayrıntılarını gösterir.

## <a name="deleting-a-group-with-an-assigned-license"></a>Lisans atanmış olan bir grubu silme

Etkin lisans atanmış bir grubu silmek mümkün değildir. Yönetici, lisansların kullanıcılardan kaldırılmasına neden olacağını fark etmeyen bir grubu silebilir - bu nedenle, silinmeden önce herhangi bir lisansın gruptan kaldırılmasını şart görüyoruz.

Azure portalındaki bir grubu silmeye çalışırken aşağıdaki gibi bir ![hata bildirimi görebilirsiniz: Ekran görüntüsü grubu silme başarısız oldu](./media/licensing-group-advanced/groupdeletionfailed.png)

Gruptaki **Lisanslar** sekmesine gidin ve atanmış lisans olup olmadığına bakın. Evet ise, bu lisansları kaldırın ve grubu yeniden silmeyi deneyin.

PowerShell veya Graph API aracılığıyla grubu silmeye çalışırken benzer hatalar görebilirsiniz. Şirket içinde eşitlenmiş bir grup kullanıyorsanız, Azure AD Connect grubu Azure AD'de silmek için başarısız olursa hataları da bildirebilir. Bu gibi durumlarda, gruba atanmış lisans olup olmadığını kontrol ettiğinizden emin olun ve önce bunları kaldırın.

## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar

Grup tabanlı lisanslama kullanıyorsanız, aşağıdaki sınırlamalar ve bilinen sorunlar listesini tanımak iyi bir fikirdir.

- Grup tabanlı lisanslama şu an için başka grup içeren grupları (iç içe grupları) desteklememektedir. İçine yerleştirilmiş başka bir grup olan bir gruba lisans uyguladığınızda yalnızca grubun birinci düzeyindeki üyelerine lisans atanır.

- Özellik yalnızca güvenlik grupları ve securityEnabled=TRUE olan Office 365 gruplarıyla kullanılabilir.

- [Microsoft 365 yönetici merkezi](https://admin.microsoft.com) şu anda grup tabanlı lisanslamayı desteklememektedir. Bir kullanıcı bir gruptan lisans devralırsa, bu lisans Office yönetici portalında normal bir kullanıcı lisansı olarak görünür. Bu lisansı değiştirmeye veya lisansı kaldırmaya çalışırsanız, portal bir hata iletisi döndürür. Devralınan grup lisansları doğrudan kullanıcı üzerinde değiştirilemez.

- Lisanslar büyük bir grup için atandığında veya değiştirildiğinde (örneğin, 100.000 kullanıcı), performansı etkileyebilir. Özellikle, Azure AD otomasyonu tarafından oluşturulan değişikliklerin hacmi, Azure AD ve şirket içi sistemler arasındaki dizin eşitlemenizin performansını olumsuz etkileyebilir.

- Kullanıcılarınızın üyeliklerini yönetmek için dinamik grupları kullanıyorsanız kullanıcının grubun bir parçası olduğunu doğrulayın. Bu durum lisans atama için gereklidir. Aksi takdirde dinamik grubun [üyelik kuralı işleme durumunu denetleyin](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule).

- Bazı yüksek yük durumlarında, gruplar için lisans değişikliklerinin veya varolan lisanslara sahip gruplara üyelik değişikliklerinin işlenmesi uzun sürebilir. Değişikliklerinizin 60K veya daha az kullanıcının grup boyutunu işlemesinin 24 saatten uzun süreceğini görürseniz, lütfen araştırmamıza izin vermek için [bir destek bileti açın.](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/supportRequest) 

- Lisans yönetimi otomasyonu, ortamdaki her türlü değişike otomatik olarak tepki vermez. Örneğin, lisanslarınız bitmiş olabilir ve bu da bazı kullanıcıların hata durumuna çıkmasına neden olabilir. Kullanılabilir koltuk sayısını serbest hale getirmek için, doğrudan atanan bazı lisansları diğer kullanıcılardan kaldırabilirsiniz. Ancak, sistem otomatik olarak bu değişikliğin tepki ve bu hata durumunda kullanıcıları düzeltmek değildir.

  Bu tür sınırlamalara geçici çözüm olarak, Azure AD'deki **Grup** bıçağına gidebilir ve **Yeniden İşlem'i**tıklatın. Bu komut, bu gruptaki tüm kullanıcıları işler ve mümkünse hata durumlarını çözer.

## <a name="next-steps"></a>Sonraki adımlar

Grup tabanlı lisanslama aracılığıyla lisans yönetimine yönelik diğer senaryolar hakkında daha fazla bilgi edinmek için bkz:

* [Azure Active Directory'de grup tabanlı lisanslama nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'de gruba lisans atama](licensing-groups-assign.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](licensing-groups-resolve-problems.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](licensing-groups-migrate-users.md)
* [Azure Active Directory'de grup tabanlı lisanslama yı kullanarak kullanıcıların ürün lisansları arasında geçiş ilertirme](../users-groups-roles/licensing-groups-change-licenses.md)
* [Azure Active Directory'de grup tabanlı lisanslama için PowerShell örnekleri](../users-groups-roles/licensing-ps-examples.md)
