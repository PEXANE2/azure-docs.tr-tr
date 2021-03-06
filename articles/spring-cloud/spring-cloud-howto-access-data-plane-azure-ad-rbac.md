---
title: Azure AD, RBAC ile veri düzlemine erişme
description: Azure Active Directory rol tabanlı erişim denetimi ile veri düzlemine erişme.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 02/04/2021
ms.custom: devx-track-java
ms.openlocfilehash: 2909d40fbc7cb5b310ea17f17a6e683ce47638ce
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102220140"
---
# <a name="access-the-data-plane-with-azure-active-directory-and-role-based-access-control"></a>Azure Active Directory ve rol tabanlı Access Control veri düzlemine erişin

Bu makalede Azure Active Directory (AAD) rol tabanlı erişim denetimi (RBAC) kullanılarak Azure Spring Cloud config sunucusuna ve hizmet kayıt defteri uç noktalarına nasıl erişebileceğiniz açıklanmaktadır.

## <a name="assign-role-to-aad-usergroup-msi-or-service-principal"></a>AAD Kullanıcı/Grup, MSI veya hizmet sorumlusu için rol atama

AAD 'yi ve RBAC 'yi kullanmak için, *Azure Spring Cloud Data Reader* rolünü aşağıdaki yordamda bir kullanıcıya, gruba veya hizmet sorumlusuna atamanız gerekir:

1. Hizmet örneğinizin hizmet genel bakış sayfasına gidin.

2. Erişim denetimi dikey penceresini açmak için **Access Control (IAM)** seçeneğine tıklayın.

3. **Ekle** düğmesine tıklayın ve **rol atamaları ekleyin** (eklemek için yetkilendirme gerekebilir).

4. **Rol** altında *Azure yay bulutu veri okuyucuyu* bulun ve seçin.
5. `User, group, or service principal` `User assigned managed identity` Kullanıcı türüne göre veya erişimi atayın. Kullanıcı arayın ve seçin.  
6. Şuna tıklayın: `Save`

   ![rol atama](media/access-data-plane-aad-rbac/assign-data-reader-role.png)

## <a name="access-data-plane"></a>Veri düzlemi 'ne erişim

Bir AAD kullanıcısına *Azure Spring Cloud Data Reader* rolü atandıktan sonra, müşteriler Kullanıcı, hizmet sorumlusu veya yönetilen kimlik Ile Azure CLI 'da oturum açabilir.  Erişim belirteci almak için bkz. [Azure CLI kimlik doğrulaması](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) .  Ardından aşağıdaki komutları kullanın.

```azurecli
az login
az account get-access-token
```

Şu anda CLı, yapılandırma sunucusu ve hizmet kayıt defteri 'nin varsayılan uç noktalarını destekler. Daha fazla bilgi için bkz. [üretime hazırlık uç noktaları](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints). 

Müşteriler, uç noktalara erişerek yapılandırma sunucusu ve hizmet kayıt defterinin desteklenen uç noktalarının tam listesini de alabilir:
* *https://SERVICE_NAME.Root_Endpoint/eureka/actuator/*
* *https://SERVICE_NAME.Root_Endpoint/config/actuator/* 

Başlıktaki erişim belirteci yetkilendirme sağlar. Yalnızca "GET" yöntemi desteklenir.

Örneğin, *https://SERVICE_NAME.Root_Endpoint/eureka/actuator/health* Eureka sistem durumunu görmek için gibi bir uç noktaya erişin.

Çeşitli kök uç noktaları, farklı bulut türlerine göre aşağıda gösterilmiştir.

| Bulut          | Kök uç noktası              |
| -------------- | -------------------------- |
| Genel         | svc.azuremicroservices.io  |
| Ay pastası/Çin | svc.microservices.azure.cn |

Yanıt *401* ise, rolün başarıyla atanıp atanmadığından emin olun.  Rolün etkili olması veya erişim belirtecinin sona ermediğinden emin olmak birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure CLı kimlik doğrulaması](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)
* [Üretime hazırlık uç noktaları](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints)

## <a name="see-also"></a>Ayrıca bkz.
* [Rol ve izinler oluşturma](spring-cloud-howto-permissions.md)