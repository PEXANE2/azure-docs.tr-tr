---
title: 'Azure dijital TWINS isteği şu durumla başarısız oldu: 403 (yasak)'
description: "' Hizmet isteği için nedenler ve çözümler başarısız oldu. Durum: 403 (yasak) ' Azure dijital TWINS üzerinde."
ms.service: digital-twins
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.date: 7/20/2020
ms.openlocfilehash: 95372d69f58249afc946f12ec74696507429e5c6
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87125318"
---
# <a name="service-request-failed-status-403-forbidden"></a>Hizmet isteği başarısız oldu. Durum: 403 (yasak)

Bu makalede, hizmet isteklerinden Azure dijital TWINS 'e 403 hatası almaya yönelik nedenler ve çözüm adımları açıklanır. 

## <a name="symptoms"></a>Belirtiler

Bu hata, kimlik doğrulaması gerektiren birçok hizmet isteği türünde gerçekleşebilir. Etkisi, API isteğinin başarısız olması ve bir hata durumu döndürmektir `403 (Forbidden)` .

## <a name="causes"></a>Nedenler

### <a name="cause-1"></a>Neden #1

Çoğu zaman bu hata, hizmet için rol tabanlı erişim denetimi (RBAC) izinlerinizin doğru ayarlanmadığını gösterir. Azure dijital TWINS örneği için birçok eylem, **yönetmeye çalıştığınız örnekte** *Azure dijital TWINS sahibi (Önizleme)* rolüne sahip olmanızı gerektirir. 

### <a name="cause-2"></a>Neden #2

Azure dijital TWINS ile iletişim kurmak için bir istemci uygulaması kullanıyorsanız, [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) uygulama kaydı, Azure Digital TWINS hizmeti için ayarlanmış izinlere sahip olmadığından bu hata ortaya çıkabilir.

Uygulama kaydı, Azure dijital TWINS API 'Leri için yapılandırılmış erişim izinlerinin olması gerekir. Daha sonra, istemci uygulamanızın uygulama kaydıyla ilgili kimlik doğrulaması yapıldığında, uygulama kaydının yapılandırıldığı izinler verilir.

## <a name="solutions"></a>Çözümler

### <a name="solution-1"></a>Çözüm #1

İlk çözüm, Azure Kullanıcı tarafından yönetmeye çalıştığınız örnekte _**Azure dijital TWINS sahibi (Önizleme)**_ rolünün olduğunu doğrulamadır. Bu role sahip değilseniz, ayarlayın.

Bu rolün öğesinden farklı olduğunu unutmayın...
* Tüm Azure aboneliğinde *sahip* rolü. Azure *dijital TWINS sahibi (Önizleme)* , Azure dijital TWINS 'in içindeki bir roldür ve bu tek Azure dijital TWINS örneği kapsamına alınır.
* Azure dijital TWINS 'de *sahip* rolü. Bunlar, iki ayrı Azure dijital TWINS yönetim rolleridir ve *Azure Digital TWINS sahibi (Önizleme)* , önizleme sırasında yönetim için kullanılması gereken roldür.

#### <a name="check-current-setup"></a>Geçerli kurulumu denetleyin

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

#### <a name="fix-issues"></a>Sorunları giderme 

Bu rol ataması yoksa, Azure **aboneliğinizde** sahip rolü olan birinin Azure **dijital TWINS örneğinde**Azure *dijital TWINS sahibi (Önizleme)* rolü sağlamak için aşağıdaki komutu çalıştırması gerekir. 

Abonelikte bir sahibiyseniz, bu komutu kendiniz çalıştırabilirsiniz. Değilseniz, bu komutu sizin adınıza çalıştırmak için bir sahibine başvurun.

```azurecli-interactive
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<your-Azure-AD-email>" --role "Azure Digital Twins Owner (Preview)"
```

Bu rol gereksinimi ve atama süreci hakkında daha fazla bilgi için bkz. *nasıl yapılır: bir örnek ve kimlik doğrulaması ayarlama (el ile)*. [ *Set up your user's access permissions* ](how-to-set-up-instance-manual.md#set-up-your-users-access-permissions)

Bu rol ataması zaten varsa ve yine de 403 sorunuyla karşılaşırsanız, sonraki çözüme geçin.

### <a name="solution-2"></a>Çözüm #2

İkinci çözüm, Azure AD uygulama kaydının Azure Digital TWINS hizmeti için yapılandırılmış izinlere sahip olduğunu doğrulamadır. Bu yapılandırılmamışsa, bunları ayarlayın.

#### <a name="check-current-setup"></a>Geçerli kurulumu denetleyin

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

İlk olarak, Azure dijital TWINS izinleri ayarlarının kayıt üzerinde doğru şekilde ayarlandığını doğrulayın. Bunu yapmak için, menü çubuğundan *bildirim* ' ı seçerek uygulama kaydının bildirim kodunu görüntüleyin. Kod penceresinin en altına kaydırın ve altındaki bu alanları bulun `requiredResourceAccess` . Değerler aşağıdaki ekran görüntüsünde olanlarla eşleşmelidir:

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

#### <a name="fix-issues"></a>Sorunları giderme

Bunlardan herhangi biri açıklanenden farklı görünüyorsa, *nasıl yapılır: bir örnek ve kimlik doğrulaması (el ile) ayarlama*konusunun [ *istemci uygulamaları için erişim izinlerini ayarlama* bölümünde](how-to-set-up-instance-manual.md#set-up-access-permissions-for-client-applications) bir uygulama kaydı ayarlama yönergelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir Azure dijital TWINS örneği oluşturma ve kimlik doğrulama için kurulum adımlarını okuyun:
* [*Nasıl yapılır: örnek ve kimlik doğrulaması ayarlama (El Ile)*](how-to-set-up-instance-manual.md)

Azure dijital TWINS üzerinde güvenlik ve izinler hakkında daha fazla bilgi edinin:
* [*Kavramlar: Azure dijital TWINS çözümleri için güvenlik*](concepts-security.md)