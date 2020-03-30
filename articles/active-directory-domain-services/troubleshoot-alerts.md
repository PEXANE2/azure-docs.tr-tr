---
title: Azure AD Etki Alanı Hizmetlerinde sık karşılaşılan uyarılar ve çözümler | Microsoft Dokümanlar
description: Azure Etkin Dizin Etki Alanı Hizmetleri için sistem durumu durumunun bir parçası olarak oluşturulan yaygın uyarıları nasıl çözeceğinizi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 01/21/2020
ms.author: iainfou
ms.openlocfilehash: c83caf31e25ae2212ed120e77e017ac3849898e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77612903"
---
# <a name="known-issues-common-alerts-and-resolutions-in-azure-active-directory-domain-services"></a>Bilinen sorunlar: Azure Active Directory Etki Alanı Hizmetlerinde sık karşılaşılan uyarılar ve çözümler

Uygulamalar için kimlik ve kimlik doğrulamanın merkezi bir parçası olarak, Azure Active Directory Domain Services (Azure AD DS) bazen sorunlarla karşı laşMaktadır. Sorunlarla karşılaştıysanız, bazı yaygın uyarılar ve bazı şeylerin yeniden çalışmasını yardımcı olacak ilişkili sorun giderme adımları vardır. İstediğiniz zaman, ek sorun giderme yardımı için [bir Azure destek isteği][azure-support] de açabilirsiniz.

Bu makalede, Azure AD DS'deki yaygın uyarılar için sorun giderme bilgileri sağlanmaktadır.

## <a name="aadds100-missing-directory"></a>AADDS100: Eksik dizini

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanınızla ilişkili Azure REKLAM dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırmada değil. Microsoft yönetilen etki alanınızı izleyemez, yönetemez, yarayamaz ve eşitleyemez.*

### <a name="resolution"></a>Çözüm

Bu hata genellikle bir Azure aboneliği yeni bir Azure AD dizinine taşındığında ve Azure AD DS ile ilişkili eski Azure AD dizini silindiğinde kaynaklanır.

Bu hata kurtarılamaz. Uyarıyı çözmek [için, mevcut Azure AD DS yönetilen etki alanınızı silin](delete-aadds.md) ve yeni dizininizde yeniden oluşturun. Azure AD DS yönetilen etki alanını silmede sorun yaşıyorsanız, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C bu dizinde çalışıyor

### <a name="alert-message"></a>Uyarı iletisi

*Azure AD Etki Alanı Hizmetleri, Azure AD B2C Dizininde etkinleştirilemez.*

### <a name="resolution"></a>Çözüm

Azure AD DS otomatik olarak bir Azure REKLAM dizini ile eşitlenir. Azure AD dizini B2C için yapılandırılırsa, Azure AD DS dağıtılamaz ve eşitlenemez.

Azure AD DS'yi kullanmak için, yönetilen etki alanınızı Aşağıdaki adımları kullanarak Azure AD B2C olmayan bir dizinde yeniden oluşturmanız gerekir:

1. [Azure AD DS yönetilen etki alanını](delete-aadds.md) mevcut Azure AD dizininizden silin.
1. Azure AD B2C dizini olmayan yeni bir Azure AD dizini oluşturun.
1. [Yeni bir Azure AD DS yönetilen etki alanı oluşturun.](tutorial-create-instance.md)

Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini günceller ve uyarıyı kaldırır.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adres genel bir IP aralığında

### <a name="alert-message"></a>Uyarı iletisi

*Azure AD Etki Alanı Hizmetlerini etkinleştirdiğiniz sanal ağın IP adres aralığı genel bir IP aralığındadır. Azure AD Etki Alanı Hizmetleri, özel IP adresi aralığına sahip bir sanal ağda etkinleştirilmelidir. Bu yapılandırma, Microsoft'un yönetilen etki alanınızı izleme, yönetme, düzeltme ve eşitleme yeteneğini etkiler.*

### <a name="resolution"></a>Çözüm

Başlamadan önce, [özel IP v4 adres boşluklarını](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces)anladığınızdan emin olun.

Sanal ağ içinde, Sanal Kaynaklar, alt ağ için yapılandırılan aynı IP adresi aralığında Azure kaynaklarına istekte bulunabilir. Bir alt ağ için genel bir IP adresi aralığı nı yapılandırırsanız, sanal ağ içinde yönlendirilen istekler amaçlanan web kaynaklarına ulaşmayabilir. Bu yapılandırma, Azure AD DS ile öngörülemeyen hatalara neden olabilir.

> [!NOTE]
> Sanal ağınızda yapılandırılan internetteki IP adresi aralığına sahipseniz, bu uyarı yoksayılabilir. Ancak, Azure AD Etki Alanı Hizmetleri, öngörülemeyen hatalara yol açabileceğinden, bu yapılandırmayla [SLA'ya](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)bağlanamaz.

Bu uyarıyı gidermek için, mevcut Azure AD DS yönetilen etki alanınızı silin ve özel IP adresi aralığına sahip sanal ağda yeniden oluşturun. Azure AD DS yönetilen etki alanı kullanılamadığı ve OS B'ler veya hizmet hesapları gibi oluşturduğunuz özel kaynaklar kaybolduğuiçin bu işlem kesintiye uğrar.

1. [Azure AD DS yönetilen etki alanını](delete-aadds.md) dizininizden silin.
1. Sanal ağ IP adresi aralığını güncelleştirmek için Azure portalında *Sanal ağı* arayın ve seçin. Azure AD DS için, herkese açık bir IP adresi aralığı kümesine sahip olan sanal ağı seçin.
1. **Ayarlar**altında *Adres Alanı'nı*seçin.
1. Varolan adres aralığını seçip düzenleyerek veya ek bir adres aralığı ekleyerek adres aralığını güncelleştirin. Yeni IP adresi aralığının özel bir IP aralığında olduğundan emin olun. Hazır olduğunuzda, değişiklikleri **kaydedin.**
1. Sol navigasyonda **Alt Ağlar'ı** seçin.
1. Vermek istediğiniz alt ağı seçin veya ek bir alt ağ oluşturun.
1. Özel bir IP adresi aralığını güncelleştirin veya belirtin ve değişikliklerinizi **kaydedin.**
1. [Yeni bir Azure AD DS yönetilen etki alanı oluşturun.](tutorial-create-instance.md) Özel IP adresi aralığıyla güncelleştirilmiş sanal ağ alt netini seçtiğinizden emin olun.

Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini günceller ve uyarıyı kaldırır.

## <a name="aadds106-your-azure-subscription-is-not-found"></a>AADDS106: Azure aboneliğiniz bulunamadı

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanınızla ilişkili Azure aboneliğiniz silindi.  Azure AD Etki Alanı Hizmetleri' nin düzgün çalışmaya devam edilebisolması için etkin bir abonelik gerekir.*

### <a name="resolution"></a>Çözüm

Azure AD DS etkin bir abonelik gerektirir ve farklı bir aboneye taşınamamalıdır. Azure AD DS yönetilen etki alanının ilişkili olduğu Azure aboneliği silinirse, bir Azure aboneliğini ve Azure AD DS yönetilen etki alanını yeniden oluşturmanız gerekir.

1. [Azure aboneliği oluşturun.](../cost-management-billing/manage/create-subscription.md)
1. [Azure AD DS yönetilen etki alanını](delete-aadds.md) mevcut Azure AD dizininizden silin.
1. [Yeni bir Azure AD DS yönetilen etki alanı oluşturun.](tutorial-create-instance.md)

## <a name="aadds107-your-azure-subscription-is-disabled"></a>AADDS107: Azure aboneliğiniz devre dışı

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanınızla ilişkili Azure aboneliğiniz etkin değildir.  Azure AD Etki Alanı Hizmetleri' nin düzgün çalışmaya devam edilebisolması için etkin bir abonelik gerekir.*

### <a name="resolution"></a>Çözüm

Azure AD DS etkin bir abonelik gerektirir. Azure AD DS yönetilen etki alanının ilişkili olduğu Azure aboneliği etkin değilse, aboneliği yeniden etkinleştirmek için aboneliği yenilemeniz gerekir.

1. [Azure aboneliğinizi yenileyin.](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable)
2. Abonelik yenilendikten sonra, Azure AD DS bildirimi yönetilen etki alanını yeniden etkinleştirmenize olanak tanır.

Yönetilen etki alanı yeniden etkinleştirildiğinde, Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini güncelleştirir ve uyarıyı kaldırır.

## <a name="aadds108-subscription-moved-directories"></a>AADDS108: Abonelik dizinleri taşındı

### <a name="alert-message"></a>Uyarı iletisi

*Azure AD Etki Alanı Hizmetleri tarafından kullanılan abonelik başka bir dizine taşındı. Azure AD Etki Alanı Hizmetlerinin düzgün çalışması için aynı dizinde etkin bir aboneliğe sahip olması gerekir.*

### <a name="resolution"></a>Çözüm

Azure AD DS etkin bir abonelik gerektirir ve farklı bir aboneye taşınamamalıdır. Azure AD DS yönetilen etki alanının ilişkili olduğu Azure aboneliği taşınırsa, aboneliği önceki dizine taşıyın veya [yönetilen etki alanınızı](delete-aadds.md) varolan dizinden silin ve [seçilen abonelikte değiştirilen Bir Azure AD DS yönetilen etki alanı oluşturun.](tutorial-create-instance.md)

## <a name="aadds109-resources-for-your-managed-domain-cannot-be-found"></a>AADDS109: Yönetilen etki alanınız için kaynaklar bulunamıyor

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanınız için kullanılan bir kaynak silindi. Bu kaynak, Azure AD Etki Alanı Hizmetlerinin düzgün çalışması için gereklidir.*

### <a name="resolution"></a>Çözüm

Azure AD DS, genel IP adresleri, sanal ağ arabirimleri ve yük dengeleyicisi gibi düzgün çalışması için ek kaynaklar oluşturur. Bu kaynaklardan herhangi biri silinirse, yönetilen etki alanı desteklenmeyen durumdadır ve etki alanının yönetilmesini engeller. Bu kaynaklar hakkında daha fazla bilgi için Azure [AD DS tarafından kullanılan Ağ kaynaklarına](network-considerations.md#network-resources-used-by-azure-ad-ds)bakın.

Bu gerekli kaynaklardan biri silindiğinde bu uyarı oluşturulur. Kaynak 4 saatten kısa bir süre önce silinmişse, Azure platformunun silinen kaynağı otomatik olarak yeniden oluşturma olasılığı vardır. Aşağıdaki adımlar, kaynak silme için sistem durumu ve zaman damgasının nasıl denetlenir:

1. Azure portalında Etki Alanı **Hizmetlerini**arayın ve seçin. azure AD DS yönetilen etki alanınızı seçin( örneğin *aaddscontoso.com.*
1. Sol daki gezinmede **Sağlık'ı**seçin.
1. Sistem durumu sayfasında, *AADDS109*kimliğiyle uyarıyı seçin.
1. Uyarının ilk ne zaman bulunduğuna ait bir zaman damgası var. Bu zaman damgası 4 saatten kısa ysa, Azure platformu kaynağı otomatik olarak yeniden oluşturabilir ve uyarıyı kendi kendine çözebilir.

    Uyarı 4 saatten eskiyse, Azure AD DS yönetilen etki alanı kurtarılamaz durumdadır. [Azure AD DS yönetilen etki alanını silin](delete-aadds.md) ve ardından yeni yönetilen etki alanı [oluşturun.](tutorial-create-instance.md)

## <a name="aadds110-the-subnet-associated-with-your-managed-domain-is-full"></a>AADDS110: Yönetilen etki alanınızla ilişkili alt ağ dolu

### <a name="alert-message"></a>Uyarı iletisi

*Azure AD Etki Alanı Hizmetlerinin dağıtımı için seçilen alt ağ doludur ve oluşturulması gereken ek etki alanı denetleyicisi için alan yok.*

### <a name="resolution"></a>Çözüm

Azure AD DS'nin sanal ağ alt ağı, otomatik olarak oluşturulan kaynaklar için yeterli IP adresine ihtiyaç duyar. Bu IP adresi alanı, bir bakım olayı olduğunda yedek kaynak oluşturma gereksinimini içerir. Kullanılabilir IP adreslerinin tükenme riskini en aza indirmek için, kendi SANAL M'leriniz gibi ek kaynakları Azure AD DS ile aynı sanal ağ alt ağına dağıtmayın.

Bu hata kurtarılamaz. Uyarıyı çözmek [için, mevcut Azure AD DS yönetilen etki alanınızı silin](delete-aadds.md) ve yeniden oluşturun. Azure AD DS yönetilen etki alanını silmede sorun yaşıyorsanız, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

## <a name="aadds111-service-principal-unauthorized"></a>AADDS111: Hizmet sorumlusu yetkisiz

### <a name="alert-message"></a>Uyarı iletisi

*Azure AD Etki Alanı Hizmetleri'nin etki alanınıza hizmet etmek için kullandığı bir hizmet ilkesi, Azure aboneliğindeki kaynakları yönetme yetkisine sahip değildir. Hizmet sorumlusunun yönetilen etki alanınıza hizmet vermek için izin ler alması gerekir.*

### <a name="resolution"></a>Çözüm

Otomatik olarak oluşturulan bazı hizmet ilkeleri, Azure AD DS yönetilen bir etki alanı için kaynakları yönetmek ve oluşturmak için kullanılır. Bu hizmet ilkelerinden birinin erişim izinleri değiştirilirse, etki alanı kaynakları doğru şekilde yönetemez. Aşağıdaki adımlar, bir hizmet sorumlusuna erişim izinlerini nasıl anlayacağınızı ve ardından erişim izinlerini nasıl verabileceğinizi gösterir:

1. Rol [tabanlı erişim denetimi ve Azure portalındaki uygulamalara nasıl erişim izni verilip verilemediği](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)hakkında bilgi edinin.
2. *Abba844e-bc0e-44b0-947a-dc74e5d09022* kimliğine sahip hizmet müdürünün sahip olduğu erişimi gözden geçirin ve daha önceki bir tarihte reddedilen erişimi tanıyın.

## <a name="aadds112-not-enough-ip-address-in-the-managed-domain"></a>AADDS112: Yönetilen etki alanında yeterli IP adresi yok

### <a name="alert-message"></a>Uyarı iletisi

*Bu etki alanında sanal ağın alt ağının yeterli IP adresine sahip olmadığını belirledik. Azure AD Etki Alanı Hizmetleri, etkinleştirilen alt ağ içinde en az iki kullanılabilir IP adresine ihtiyaç duyar. Alt ağ içinde en az 3-5 yedek IP adresine sahip olmanızı öneririz. Bu durum, alt ağ içinde başka sanal makineler dağıtılırsa, böylece kullanılabilir IP adreslerinin sayısını tüketiyorsa veya alt ağdaki kullanılabilir IP adreslerinin sayısında bir kısıtlama varsa oluşabilir.*

### <a name="resolution"></a>Çözüm

Azure AD DS'nin sanal ağ alt ağı, otomatik olarak oluşturulan kaynaklar için yeterli IP adresine ihtiyaç duyar. Bu IP adresi alanı, bir bakım olayı olduğunda yedek kaynak oluşturma gereksinimini içerir. Kullanılabilir IP adreslerinin tükenme riskini en aza indirmek için, kendi SANAL M'leriniz gibi ek kaynakları Azure AD DS ile aynı sanal ağ alt ağına dağıtmayın.

Bu uyarıyı gidermek için, mevcut Azure AD DS yönetilen etki alanınızı silin ve yeterince büyük bir IP adresi aralığına sahip sanal ağda yeniden oluşturun. Azure AD DS yönetilen etki alanı kullanılamadığı ve OS B'ler veya hizmet hesapları gibi oluşturduğunuz özel kaynaklar kaybolduğuiçin bu işlem kesintiye uğrar.

1. [Azure AD DS yönetilen etki alanını](delete-aadds.md) dizininizden silin.
1. Sanal ağ IP adresi aralığını güncelleştirmek için Azure portalında *Sanal ağı* arayın ve seçin. Küçük IP adresi aralığına sahip Azure AD DS için sanal ağı seçin.
1. **Ayarlar**altında *Adres Alanı'nı*seçin.
1. Varolan adres aralığını seçip düzenleyerek veya ek bir adres aralığı ekleyerek adres aralığını güncelleştirin. Yeni IP adresi aralığının Azure AD DS alt ağ aralığı için yeterince büyük olduğundan emin olun. Hazır olduğunuzda, değişiklikleri **kaydedin.**
1. Sol navigasyonda **Alt Ağlar'ı** seçin.
1. Vermek istediğiniz alt ağı seçin veya ek bir alt ağ oluşturun.
1. Güncelleme veya yeterince büyük bir IP adresi aralığı belirtin sonra değişikliklerinizi **kaydedin.**
1. [Yeni bir Azure AD DS yönetilen etki alanı oluşturun.](tutorial-create-instance.md) Yeterince büyük bir IP adresi aralığına sahip güncelleştirilmiş sanal ağ alt netini seçtiğinizden emin olun.

Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini günceller ve uyarıyı kaldırır.

## <a name="aadds113-resources-are-unrecoverable"></a>AADDS113: Kaynaklar kurtarılamaz

### <a name="alert-message"></a>Uyarı iletisi

*Azure AD Etki Alanı Hizmetleri tarafından kullanılan kaynaklar beklenmeyen bir durumda algılandı ve kurtarılamadı.*

### <a name="resolution"></a>Çözüm

Bu hata kurtarılamaz. Uyarıyı çözmek [için, mevcut Azure AD DS yönetilen etki alanınızı silin](delete-aadds.md) ve yeniden oluşturun. Azure AD DS yönetilen etki alanını silmede sorun yaşıyorsanız, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

## <a name="aadds114-subnet-invalid"></a>AADDS114: Subnet geçersiz

### <a name="alert-message"></a>Uyarı iletisi

*Azure AD Etki Alanı Hizmetlerinin dağıtımı için seçilen alt ağ geçersizdir ve kullanılamaz.*

### <a name="resolution"></a>Çözüm

Bu hata kurtarılamaz. Uyarıyı çözmek [için, mevcut Azure AD DS yönetilen etki alanınızı silin](delete-aadds.md) ve yeniden oluşturun. Azure AD DS yönetilen etki alanını silmede sorun yaşıyorsanız, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

## <a name="aadds115-resources-are-locked"></a>AADDS115: Kaynaklar kilitlendi

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanı tarafından kullanılan ağ kaynaklarından biri veya birkaçı, hedef kapsam kilitlendikçe çalıştırılamaz.*

### <a name="resolution"></a>Çözüm

Kaynak kilitleri, değişikliği veya silmeyi önlemek için Azure kaynaklarına uygulanabilir. Azure AD DS yönetilen bir hizmet olduğundan, Azure platformunun yapılandırma değişiklikleri yapabilmesi gerekir. Azure AD DS bileşenlerinin bazılarına kaynak kilidi uygulanırsa, Azure platformu yönetim görevlerini gerçekleştiremez.

Azure AD DS bileşenlerindeki kaynak kilitlerini denetlemek ve bunları kaldırmak için aşağıdaki adımları tamamlayın:

1. Sanal ağ, ağ arabirimi veya genel IP adresi gibi kaynak grubunuzdaki Azure AD DS ağ bileşenlerinin her biri için Azure portalındaki işlem günlüklerini kontrol edin. Bu işlem günlükleri, bir işlemin neden başarısız olduğunu ve kaynak kilidi uygulandığı yeri belirtmelidir.
1. Kilidin uygulandığı kaynağı seçin, ardından **Kilitler'in**altında kilidi seçin ve kilidi kaldırın.

## <a name="aadds116-resources-are-unusable"></a>AADDS116: Kaynaklar kullanılamaz

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanı tarafından kullanılan ağ kaynaklarından biri veya birkaçı ilke kısıtlaması nedeniyle çalıştırılamaz.*

### <a name="resolution"></a>Çözüm

İlkeler, hangi yapılandırma eylemlerine izin verilebilen Azure kaynaklarına ve kaynak gruplarına uygulanır. Azure AD DS yönetilen bir hizmet olduğundan, Azure platformunun yapılandırma değişiklikleri yapabilmesi gerekir. Azure AD DS bileşenlerinin bazılarında bir ilke uygulanırsa, Azure platformu yönetim görevlerini gerçekleştiremeyebilir.

Azure AD DS bileşenlerinde uygulanan ilkeleri denetlemek ve güncelleştirmek için aşağıdaki adımları tamamlayın:

1. Sanal ağ, NIC veya genel IP adresi gibi kaynak grubunuzdaki Azure AD DS ağ bileşenlerinin her biri için Azure portalındaki işlem günlüklerini kontrol edin. Bu işlem günlükleri, bir işlemin neden başarısız olduğunu ve kısıtlayıcı bir ilkenin uygulandığı yeri belirtmelidir.
1. İlkenin uygulandığı kaynağı seçin, ardından **İlkeler**altında, ipolitikası daha az kısıtlayıcı olacak şekilde seçin ve düzenlemeyi seçin.

## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Eşitleme bir süredir tamamlanmadı

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanı en son [tarihte] Azure AD ile eşitlenmiştir. Kullanıcılar yönetilen etki alanında oturum açamayabilir veya grup üyelikleri Azure AD ile eşit olmayabilir.*

### <a name="resolution"></a>Çözüm

Yönetilen etki alanının yapılandırmasında ki sorunları gösteren uyarılar için [Azure AD DS sistem durumunu denetleyin.](check-health.md) Ağ yapılandırmasıyla ilgili sorunlar Azure AD'den eşitleme engelleyebilir. Yapılandırma sorununu gösteren uyarıları çözebilirseniz, iki saat bekleyin ve eşitlemeişleminin başarıyla tamamlanıp tamamlanmadığını görmek için tekrar kontrol edin.

Aşağıdaki yaygın nedenler eşitlemenin Azure AD DS yönetilen etki alanlarında durmasına neden olur:

* Gerekli ağ bağlantısı engellendi. Azure sanal ağını sorunlara karşı nasıl kontrol edebilirsiniz ve nelere yönelik olarak daha fazla bilgi edinmek için, [sorun giderme ağı güvenlik gruplarına](alert-nsg.md) ve [Azure AD Etki Alanı Hizmetleri ağ gereksinimlerine](network-considerations.md)bakın.
*  Azure AD DS yönetilen etki alanı dağıtıldığında parola eşitleme ayarlanmadı veya başarıyla tamamlanmadı. Yalnızca [bulut kullanıcıları](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds) veya karma kullanıcılar için [ön-prem'den](tutorial-configure-password-hash-sync.md)parola eşitleme ayarlayabilirsiniz.

## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Bir süre içinde bir yedekleme alınmadı

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanı en son [tarihte] yedeklenmiştir.*

### <a name="resolution"></a>Çözüm

Yönetilen etki alanının yapılandırmasındaki sorunları gösteren uyarılar için [Azure AD DS sistem durumunu denetleyin.](check-health.md) Ağ yapılandırmasıyla ilgili sorunlar, Azure platformunun başarılı bir şekilde yedekleme almasını engelleyebilir. Yapılandırma sorununu gösteren uyarıları çözebilirseniz, iki saat bekleyin ve eşitlemeişleminin başarıyla tamamlanıp tamamlanmadığını görmek için tekrar kontrol edin.

## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Engelli aboneliği nedeniyle askıya alma

### <a name="alert-message"></a>Uyarı iletisi

*Etki alanıyla ilişkili Azure aboneliği etkin olmadığından yönetilen etki alanı askıya alınır.*

### <a name="resolution"></a>Çözüm

> [!WARNING]
> Azure AD DS yönetilen bir etki alanı uzun bir süre askıya alınmışsa, silinme tehlikesi vardır. Askıya alma nedenini mümkün olan en kısa sürede çözün. Daha fazla bilgi için bkz: [Azure AD DS için askıya alınan durumları anlayın.](suspension.md)

Azure AD DS etkin bir abonelik gerektirir. Azure AD DS yönetilen etki alanının ilişkili olduğu Azure aboneliği etkin değilse, aboneliği yeniden etkinleştirmek için aboneliği yenilemeniz gerekir.

1. [Azure aboneliğinizi yenileyin.](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable)
2. Abonelik yenilendikten sonra, Azure AD DS bildirimi yönetilen etki alanını yeniden etkinleştirmenize olanak tanır.

Yönetilen etki alanı yeniden etkinleştirildiğinde, Azure AD DS yönetilen etki alanının sistem durumu iki saat içinde otomatik olarak kendini güncelleştirir ve uyarıyı kaldırır.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Geçersiz bir yapılandırma nedeniyle süspansiyon

### <a name="alert-message"></a>Uyarı iletisi

*Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alınır. Hizmet, yönetilen etki alanınız için etki alanı denetleyicilerini uzun zamandır yönetemiyor, yarayamıyordu veya güncelleştiremiyor.*

### <a name="resolution"></a>Çözüm

> [!WARNING]
> Azure AD DS yönetilen bir etki alanı uzun bir süre askıya alınmışsa, silinme tehlikesi vardır. Askıya alma nedenini mümkün olan en kısa sürede çözün. Daha fazla bilgi için bkz: [Azure AD DS için askıya alınan durumları anlayın.](suspension.md)

Yönetilen etki alanının yapılandırmasındaki sorunları gösteren uyarılar için [Azure AD DS sistem durumunu denetleyin.](check-health.md) Yapılandırma sorununu gösteren uyarıları çözebilirseniz, iki saat bekleyin ve eşitlemeişleminin tamamlanıp tamamlanmadığını görmek için tekrar kontrol edin. Hazır olduğunuzda, Azure AD DS yönetilen etki alanını yeniden etkinleştirmek için [bir Azure destek isteği açın.][azure-support]

## <a name="next-steps"></a>Sonraki adımlar

Sorunlarınız hala varsa, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
