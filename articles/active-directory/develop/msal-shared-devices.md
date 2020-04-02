---
title: Paylaşılan aygıt moduna genel bakış
titleSuffix: Microsoft identity platform | Azure
description: Firstline Çalışanlarınız için cihaz paylaşımını etkinleştirmek için paylaşılan aygıt modu hakkında bilgi edinin.
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/31/2020
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 514782d62f117af5bfff4a5d2b3354c4e263eece
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80550246"
---
# <a name="overview-of-shared-device-mode"></a>Paylaşılan aygıt moduna genel bakış

Paylaşılan aygıt modu, Firstline Çalışanlarını destekleyen uygulamalar oluşturmanıza ve kendilerine dağıtılan aygıtlarda paylaşılan aygıt modunu etkinleştirmenize olanak tanıyan Azure Etkin Dizin özelliğidir.

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="what-are-firstline-workers"></a>Firstline İşçileri nedir?

Firstline Çalışanlar perakende çalışanları, bakım ve saha ajanları, tıbbi personel ve bir bilgisayarın önünde oturup işbirliği için kurumsal e-posta kullanmayan diğer kullanıcılardır. Aşağıdaki bölümlerfirstline çalışanları desteklemenin yönlerini ve zorluklarını ve ardından Microsoft tarafından sağlanan ve bir kuruluşun Firstline Çalışanları tarafından kullanılmasına olanak tanıyan özelliklere giriş yapılmasını sağlar.

### <a name="challenges-of-supporting-firstline-workers"></a>Firstline İşçilerini Desteklemenin Zorlukları

Firstline Worker iş akışlarını etkinleştirmek, genellikle tipik bilgi çalışanları tarafından sunulmayan zorlukları içerir. Bu tür zorluklar, yüksek ciro oranı ve bir kuruluşun temel üretkenlik araçlarına daha az aşinalığı içerebilir. Firstline İşçilerini güçlendirmek için, kuruluşlar farklı stratejiler benimsiyor. Bazıları, çalışanlarının kişisel telefonlarında iş uygulamalarını kullandığı bir kendi cihazını getir (BYOD) stratejisini benimserken, diğerleri çalışanlarına iPad veya Android tabletler gibi paylaşılan cihazlar sağlıyor.

### <a name="supporting-multiple-users-on-devices-designed-for-one-user"></a>Tek bir kullanıcı için tasarlanmış cihazlarda birden çok kullanıcıyı destekleme

iOS veya Android çalıştıran mobil cihazlar tek kullanıcılar için tasarlandığından, çoğu uygulama deneyimlerini tek bir kullanıcı tarafından kullanılmak üzere optimize eder. Bu optimize edilmiş deneyimin bir parçası, uygulamalar arasında tek oturum açmayı etkinleştirmek ve kullanıcıları aygıtlarında oturum açmaları anlamına gelir. Bir kullanıcı hesabını bir uygulamadan kaldırdığında, uygulama genellikle bunu güvenlikle ilgili bir olay olarak kabul etmez. Hatta birçok uygulama hızlı oturum açma için kullanıcının kimlik bilgilerini gizli tutar. Hatta mobil cihazınızdan bir uygulamayı sildikten sonra yeniden yüklediğinizde, yalnızca hala oturum açmış olduğunuzu fark ettiğinizde bunu kendiniz yaşamış olabilirsiniz.

### <a name="global-sign-in-and-sign-out-sso"></a>Küresel oturum açma ve imzalama (SSO)

Bir kuruluşun çalışanlarının uygulamalarını bu çalışanlar tarafından paylaşılan aygıtlar havuzunda kullanmasına izin vermek için geliştiricilerin tam tersi deneyimi etkinleştirmeleri gerekir. Çalışanlar, vardiyaları boyunca havuzdan bir cihaz seçebilmeli ve "kendi başlarına yapmak" için tek bir hareket gerçekleştirebilmeli. Vardiyalarının sonunda, tüm kişisel ve şirket bilgileri kaldırılarak aygıtüzerinde küresel olarak oturum açarak başka bir hareket gerçekleştirebilmeli ve böylece aygıt havuzuna geri döndürebilmeleri gerekir. Ayrıca, bir çalışan oturum açmayı unutursa, cihaz vardiyasının sonunda ve/veya bir hareketsizlik döneminden sonra otomatik olarak oturumunu kesmelidir.

Azure Etkin Dizin paylaşılan **aygıt modu**olarak adlandırılan bir özellik ile bu senaryoları sağlar.

## <a name="introducing-shared-device-mode"></a>Paylaşılan aygıt moduyla tanıtılması

Daha önce de belirtildiği gibi, paylaşılan aygıt modu Azure Active Directory'nin şunları yapmanızı sağlayan bir özelliğidir:

* Firstline İşçilerini destekleyen uygulamalar oluşturun
* Aygıtları Firstline Çalışanları'na dağıtın ve paylaşılan aygıt modunu açın

### <a name="build-applications-that-support-firstline-workers"></a>Firstline İşçilerini destekleyen uygulamalar oluşturun

Paylaşılan aygıt modu adı verilen bir aygıt durumunu etkinleştirmek için Microsoft Kimlik Doğrulama Kitaplığı (MSAL) ve [Microsoft Authenticator uygulamasını](../user-help/user-help-auth-app-overview.md) kullanarak uygulamalarınızda Firstline Çalışanları'nı destekleyebilirsiniz. *shared device mode* Bir aygıt paylaşılan aygıt modundayken, Microsoft uygulamanıza kullanıcı verilerini koruyarak, kullanıcının aygıttaki kullanıcı durumuna göre davranışını değiştirmesine olanak tanıyan bilgiler sağlar.

Desteklenen özellikler şunlardır:

* Desteklenen herhangi bir uygulama aracılığıyla **kullanıcı aygıtı genelinde oturum açın.**
* Desteklenen herhangi bir uygulama aracılığıyla **kullanıcı aygıtı genelinde oturum açın.**
* Uygulamanızın paylaşılan aygıt modunda olan bir aygıtta olup olmadığını belirlemek için **aygıtın durumunu sorgulayın.**
* Uygulamanızın son kullanıldığından beri herhangi bir şeyin değişip değişmediğini belirlemek için aygıtın **kullanıcının aygıt durumunu sorgulayın.**

Paylaşılan cihaz modunu desteklemek, uygulamanız için hem bir güvenlik geliştirmesi hem de özellik yükseltmesi olarak kabul edilmelidir ve sağlık ve finans gibi yüksek denetimli ortamlarda benimsenmesini artırmaya yardımcı olabilir.

Kullanıcılarınız, verilerinin başka bir kullanıcıya sızdırılmadığından emin olmak için size güvenir. Aygıt Modunu Paylaş, yönetiminizde bir değişikliğin gerçekleştiğini uygulamanıza belirtmek için yararlı sinyaller sağlar. Uygulamanız, uygulama her kullanıldığında cihazdaki kullanıcının durumunu kontrol etmek ve önceki kullanıcının verilerini temizlemekten sorumludur. Bu, çoklu görev de arka plandan yeniden yüklenip yeniden yüklenmemesini içerir. Kullanıcı değişikliğinde, hem önceki kullanıcının verilerinin temizlendiğinden hem de uygulamanızda görüntülenen önbelleğe alınmış verilerin kaldırıldığından emin olmalısınız. Uygulamanıza paylaşılan cihaz modu özelliğini ekledikten sonra her zaman kapsamlı bir güvenlik gözden geçirme işlemi gerçekleştirmenizi öneririz.

Paylaşılan aygıt modunu destekleyecek şekilde uygulamalarınızı nasıl değiştirebilirsiniz hakkında ayrıntılı bilgi için bu makalenin sonundaki [Sonraki adımlar](#next-steps) bölümüne bakın.

### <a name="deploy-devices-to-firstline-workers-and-turn-on-shared-device-mode"></a>Aygıtları Firstline Çalışanları'na dağıtın ve paylaşılan aygıt modunu açın

Uygulamalarınız paylaşılan aygıt modunu destekledikten ve gerekli verileri ve güvenlik değişikliklerini ekledikten sonra, bunları Firstline Çalışanları tarafından kullanılabilir olarak tanıtabilirsiniz.

Bir kuruluşun aygıt yöneticileri, Microsoft Intune gibi bir mobil aygıt yönetimi (MDM) çözümü aracılığıyla cihazlarını ve uygulamalarınızı mağazalarına ve işyerlerine dağıtabilir. Sağlama işleminin bir parçası aygıtı *Paylaşılan Aygıt*olarak işaretlemektir. Yöneticiler, [Microsoft Authenticator uygulamasını](../user-help/user-help-auth-app-overview.md) dağıtarak ve paylaşılan aygıt modunu yapılandırma parametreleri üzerinden ayarlayarak paylaşılan aygıt modunu yapılandırır. Bu adımları gerçekleştirdikten sonra, paylaşılan aygıt modunu destekleyen tüm uygulamalar, kullanıcı durumunu yönetmek ve aygıt ve kuruluş için güvenlik özellikleri sağlamak için Microsoft Authenticator uygulamasını kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan cihaz modu için iOS ve Android platformlarını destekliyoruz. Başvurularınızda Firstline İşçilerini desteklemeye başlamak için platformunuz için aşağıdaki belgeleri inceleyin.

* [iOS için paylaşılan aygıt modunu destekleme](msal-ios-shared-devices.md)
* [Android için paylaşılan cihaz modunu destekleme](msal-android-shared-devices.md)
