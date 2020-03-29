---
title: Güvenliğin en iyi uygulamalarını anlayın - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins ve Nesnelerin İnterneti için güvenlik en iyi uygulamaları hakkında bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: 5fc5ba447557aa89e8f0870c576d6d4c439f3353
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122568"
---
# <a name="azure-digital-twins-security-best-practices"></a>Azure Digital Twins güvenlik en iyi uygulamaları

Azure Digital Twins güvenliği, IoT grafiğinizdeki belirli kaynaklara ve eylemlere hassas erişim sağlar. Bunu, [rol tabanlı erişim denetimi](./security-role-based-access-control.md)olarak adlandırılan parçalı rol ve izin yönetimi yoluyla yapar.

Azure Digital Twins, Azure Active Directory (Azure AD) dahil olmak üzere Azure IoT'de bulunan diğer güvenlik özelliklerini de kullanır. Bu nedenle, Azure Digital Twins'te yerleşik uygulamaları yapılandırmak ve güvence altına almak, şu anda önerilen aynı [Azure IoT güvenlik uygulamalarının](../iot-fundamentals/iot-security-best-practices.md) çoğunu kullanmayı içerir.

Bu makalede, izlenebilir önemli en iyi uygulamaları özetler.

> [!IMPORTANT]
> IoT alanınız için maksimum güvenlik sağlamak için ek güvenlik kaynaklarını gözden geçirin. Aygıt satıcılarınızı dahil edin.

> [!TIP]
> IoT güvenlik tehditlerini ve güvenlik açıklarını algılamaya yardımcı olmak [için IoT için Azure Güvenlik Merkezi'ni](https://docs.microsoft.com/azure/asc-for-iot/) kullanın.

## <a name="iot-security-best-practices"></a>IoT güvenliği için en iyi uygulamalar

IoT cihazlarınızı güvenli bir şekilde korumak için bazı önemli uygulamalar şunlardır:

> [!div class="checklist"]
> * IoT alanınıza bağlanan her aygıtı kurcalamaya karşı korumalı bir şekilde emniyete alayın.
> * IoT alanınızdaki her aygıtın, sensörün ve kişinin rolünü sınırlayın. Tehlikeye girerse, etki en aza indirilir.
> * Cihaz IP adresi filtreleme ve bağlantı noktası kısıtlamasının olası kullanımını göz önünde bulundurun.
> * Performansı artırmak için G/Ç ve aygıt bant genişliğini sınırlayın. Hız sınırlaması, hizmet reddi saldırılarını önleyerek güvenliği artırabilir.
> * Aygıt firmware, işletim sistemi ve yazılım güncel tutun.
> * Cihaz, yazılım, ağ ve ağ geçidi güvenliğini geliştirmeye ve geliştirmeye devam ettikçe düzenli olarak denetleyin ve gözden geçirin.
> * Güvenilir, sertifikalı ve uyumlu güvenlik sistemlerini, yazılımlarını ve aygıtları kullanın. Örneğin, Azure [Bulutu'nun uyumluluk tekliflerini](https://azure.microsoft.com/overview/trusted-cloud/compliance/) inceleyin.

Bir IoT alanını güvenli bir şekilde güvence altına almak için bazı önemli uygulamalar şunlardır:

> [!div class="checklist"]
> * Kaydedilen, depolanan veya kalıcı verileri şifreleyin.
> * Parolaların veya anahtarların düzenli olarak değiştirilmesini veya yenilenmesini zorunlu kınla.
> * Erişimi ve izinleri role göre dikkatle kısıtlayın. Aşağıdaki [Rol tabanlı erişim denetimi en iyi uygulamaları](#role-based-access-control-best-practices) bölümü okuyun.
> * Her ağdaki aygıtların diğerlerinden izole edilebilmeleri için bölünmüş bir ağ topolojisi düşünün.
> * Güçlü şifreleme kullanın. Uzun parolalar, güvenli protokoller ve [çok faktörlü kimlik doğrulama](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)kullanın.

[Monitör](./how-to-configure-monitoring.md) Her zamanki çalışma aralığının dışında kalan aykırılıklara, tehditlere veya kaynak parametrelerine dikkat etmek için IoT kaynakları. İzleme yönetimi için Azure Analytics'i kullanın.

> [!IMPORTANT]
> Kapsamlı bir IoT güvenlik stratejisi başlatmak için Azure [IoT güvenlik en iyi uygulamalarını](../iot-fundamentals/iot-security-best-practices.md) okuyun.

> [!NOTE]
> Etkinlik işleme ve izleme hakkında daha fazla bilgi için [Azure Digital Twins ile Route etkinliklerini ve iletilerini](./concepts-events-routing.md)okuyun.

## <a name="azure-active-directory-best-practices"></a>Azure Active Directory en iyi uygulamaları

Azure Digital Twins, kullanıcıların kimliğini doğrulamak ve uygulamaları korumak için [Azure Active Directory'yi](https://docs.microsoft.com/azure/active-directory/authentication/) kullanır. Azure Active Directory, çeşitli modern mimariler için kimlik doğrulamayı destekler. Bunların tümü OAuth 2.0 veya OpenID Connect gibi endüstri standardı protokollere dayanmaktadır. Azure Etkin Dizini için IoT alanınızı güvence altına almak için birkaç önemli uygulama şunlardır:

> [!div class="checklist"]
> * Azure Active Directory uygulama sırlarını ve anahtarlarını [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/)gibi güvenli bir konumda saklayın.
> * Kimlik doğrulaması için uygulama sırları yerine güvenilir bir [sertifika yetkilisi](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) tarafından verilen bir sertifika kullanın.
> * Bir belirteç için OAuth 2.0 erişim kapsamını sınırlayın.
> * Belirteç geçerli olduğu süreyi ve belirteç lerinin geçerli kalıp kalmadığı doğrulayın.
> * Belirteçlerin geçerli olduğu uygun süreleri ayarlayın. Süresi dolmuş jetonları yenileyin.
> * Kullanılmayan **Yönlendirme URL'lerini** ve [Rol tabanlı erişim denetimi başına izinleri en iyi uygulamaları](#role-based-access-control-best-practices)kaldırın.

## <a name="role-based-access-control-best-practices"></a>Rol tabanlı erişim denetimi en iyi uygulamaları

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Azure IoT en iyi uygulamaları hakkında daha fazla bilgi edinmek için [IoT güvenlik en iyi uygulamalarını](../iot-fundamentals/iot-security-best-practices.md)okuyun.

* Rol tabanlı erişim denetimi hakkında bilgi edinmek için [Rol tabanlı erişim denetimini](./security-role-based-access-control.md)okuyun.

* Kimlik doğrulama hakkında bilgi edinmek için [API'lerle Kimlik Doğrulama'yı](./security-authenticating-apis.md)okuyun.
