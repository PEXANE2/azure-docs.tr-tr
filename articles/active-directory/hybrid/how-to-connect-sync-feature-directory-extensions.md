---
title: 'Azure AD Connect eşitleme: Dizin uzantıları | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect'teki dizin uzantıları özelliğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/12/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 80438319a6337dd6f28f9bdca8a428829b6cb0b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77917922"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect eşitleme: Dizin uzantıları
Dizin uzantılarını, Azure Etkin Dizini'ndeki (Azure AD) şemasını şirket içi Active Directory'den kendi özniteliklerinizle genişletmek için kullanabilirsiniz. Bu özellik, şirket içinde yönetmeye devam ettiğiniz öznitelikleri tüketerek LOB uygulamaları oluşturmanıza olanak tanır. Bu öznitelikler [uzantıları](https://docs.microsoft.com/graph/extensibility-overview
)aracılığıyla tüketilebilir. Microsoft [Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer)kullanarak kullanılabilir öznitelikleri görebilirsiniz. Bu özelliği Azure AD'de dinamik gruplar oluşturmak için de kullanabilirsiniz.

Şu anda, hiçbir Office 365 iş yükü bu öznitelikleri tüketir.

## <a name="customize-which-attributes-to-synchronize-with-azure-ad"></a>Azure AD ile eşitlemek için hangi öznitelikleri özelleştirme

Yükleme sihirbazındaki özel ayarlar yolunda eşitlemek istediğiniz ek öznitelikleri yapılandırırsınız.

>[!NOTE]
>Kullanılabilir Öznitelikler kutusu büyük/küçük harf duyarlıdır.

![Şema uzantısı sihirbazı](./media/how-to-connect-sync-feature-directory-extensions/extension2.png)  

Yükleme geçerli adaylar olan aşağıdaki öznitelikleri gösterir:

* Kullanıcı ve Grup nesne türleri
* Tek değerli öznitelikler: String, Boolean, Integer, İkili
* Çok değerli öznitelikler: String, İkili


>[!NOTE]
> Azure AD Connect, çok değerli dizin uzantıları olarak Azure AD'ye çok değerli Active Directory özniteliklerini eşitlemeyi desteklese de, şu anda çok değerli dizin uzantısı özniteliklerine yüklenen verileri almanın/tüketmenin bir yolu yoktur.

Öznitelikler listesi, Azure AD Connect'in yüklenmesi sırasında oluşturulan şema önbelleğinden okunur. Etkin Dizin şeasını ek özniteliklerle genişlettiyseniz, bu yeni öznitelikler görünür olmadan önce [şeayı yenilemeniz](how-to-connect-installation-wizard.md#refresh-directory-schema) gerekir.

Azure AD'deki bir nesnenin dizin uzantıları için en fazla 100 özniteliği olabilir. Maksimum uzunluk 250 karakterdir. Bir öznitelik değeri daha uzunsa, eşitleme altyapısı onu keser.

## <a name="configuration-changes-in-azure-ad-made-by-the-wizard"></a>Sihirbaz tarafından yapılan Azure AD yapılandırma değişiklikleri

Azure AD Connect'in yüklenmesi sırasında, bu özniteliklerin bulunduğu bir uygulama kaydedilir. Bu uygulamayı Azure portalında görebilirsiniz. Adı her zaman **Kiracı Şema Uzatma Uygulamasıdır.**

![Şema uzantısı uygulaması](./media/how-to-connect-sync-feature-directory-extensions/extension3new.png)

Bu uygulamayı görmek için **tüm uygulamaları** seçtiğinizden emin olun.

Öznitelikler **uzantısı \_{ApplicationId}\_** ile önceden belirlenmiştir. ApplicationId, Azure AD kiracınızdaki tüm öznitelikler için aynı değere sahiptir. Bu konudaki diğer tüm senaryolar için bu değere ihtiyacınız olacaktır.

## <a name="viewing-attributes-using-the-microsoft-graph-api"></a>Microsoft Graph API'yi kullanarak öznitelikleri görüntüleme

Bu öznitelikler artık [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer#)kullanarak Microsoft Graph API'si aracılığıyla kullanılabilir.

>[!NOTE]
> Microsoft Graph API'sinde, özniteliklerin döndürüleceğini sormanız gerekir. Açıkça şu gibi öznitelikleri `https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com?$select=extension_9d98ed114c4840d298fad781915f27e4_employeeID,extension_9d98ed114c4840d298fad781915f27e4_division`seçin: .
>
> Daha fazla bilgi için [Microsoft Graph: Sorgu parametrelerini kullanın.](https://developer.microsoft.com/graph/docs/concepts/query_parameters#select-parameter)

## <a name="use-the-attributes-in-dynamic-groups"></a>Öznitelikleri dinamik gruplarda kullanma

Daha yararlı senaryolardan biri, bu öznitelikleri dinamik güvenlik veya Office 365 gruplarında kullanmaktır.

1. Azure AD'de yeni bir grup oluşturun. İyi bir ad verin ve **Üyelik türünün** **Dinamik Kullanıcı**olduğundan emin olun.

   ![Yeni bir grupla ekran görüntüsü](./media/how-to-connect-sync-feature-directory-extensions/dynamicgroup1.png)

2. Dinamik **sorgu eklemek**için seçin. Özellikleri bakarsanız, o zaman bu genişletilmiş öznitelikleri görmezsiniz. Önce eklemen gerek. **Özel uzantı özelliklerini al'ı**tıklatın, Uygulama Kimliğini girin ve **özellikleri yenile'yi**tıklatın.

   ![Dizin uzantılarının eklendiği ekran görüntüsü](./media/how-to-connect-sync-feature-directory-extensions/dynamicgroup2.png) 

3. Özellik açılır açılır ve eklediğiniz özniteliklerin artık görünür olduğunu unutmayın.

   ![UI'de yeni özniteliklerin yer aldığı ekran görüntüsü](./media/how-to-connect-sync-feature-directory-extensions/dynamicgroup3.png)

   Gereksinimlerinize uygun ifadeyi tamamlayın. Örneğimizde kural **(user.extension_9d98ed114c4840d298fad781915f27e4_division -eq "Satış ve pazarlama")** olarak ayarlanır.

4. Grup oluşturulduktan sonra, Azure AD'ye üyeleri doldurması ve ardından üyeleri incelemesi için biraz zaman verin.

   ![Dinamik gruptaki üyelerle ekran görüntüsü](./media/how-to-connect-sync-feature-directory-extensions/dynamicgroup4.png)  

## <a name="next-steps"></a>Sonraki adımlar
Azure AD [Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
