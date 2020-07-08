---
title: Azure SignalR hizmetindeki Yönetilen kimlikler
description: Yönetilen kimliklerin Azure SignalR hizmetinde nasıl çalıştığını ve sunucusuz senaryolarda yönetilen bir kimliğin nasıl kullanılacağını öğrenin.
author: chenyl
ms.service: signalr
ms.topic: article
ms.date: 06/8/2020
ms.author: chenyl
ms.openlocfilehash: abe7503e7eb73d533ae901af21de001960173fb0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559409"
---
# <a name="managed-identities-for-azure-signalr-service"></a>Azure SignalR hizmeti için Yönetilen kimlikler

Bu makalede, Azure SignalR hizmeti için yönetilen bir kimliğin nasıl oluşturulacağı ve sunucusuz senaryolarda nasıl kullanılacağı gösterilmektedir.

> [!Important] 
> Azure SignalR hizmeti yalnızca bir yönetilen kimliği destekleyebilir. Bu, sistem tarafından atanan bir kimlik veya Kullanıcı tarafından atanan bir kimlik ekleyebileceğiniz anlamına gelir. 

## <a name="add-a-system-assigned-identity"></a>Sistem tarafından atanan kimlik ekleme

Azure portal yönetilen bir kimlik ayarlamak için öncelikle bir Azure SignalR hizmet örneği oluşturup özelliği etkinleştirmeniz gerekir.

1. Portalda normalde yaptığınız gibi bir Azure SignalR hizmeti örneği oluşturun. Portalda buna gidin.

2. **Kimlik**seçin.

4. **Sistem atandı** sekmesinde **durumu** **Açık**olarak değiştirin. **Kaydet**'i seçin.

    :::image type="content" source="media/signalr-howto-use-managed-identity/system-identity-portal.png" alt-text="Portala sistem tarafından atanan bir kimlik ekleme":::

## <a name="add-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik ekleme

Kullanıcı tarafından atanan bir kimlikle Azure SignalR hizmeti örneği oluşturmak için kimlik oluşturmanız ve ardından kaynak tanımlayıcısını hizmetinize eklemeniz gerekir.

1. [Bu yönergelere](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)göre Kullanıcı tarafından atanan bir yönetilen kimlik kaynağı oluşturun.

2. Portalda normalde yaptığınız gibi bir Azure SignalR hizmeti örneği oluşturun. Portalda buna gidin.

3. **Kimlik**seçin.

4. **Kullanıcı atandı** sekmesinde **Ekle**' yi seçin.

5. Daha önce oluşturduğunuz kimliği arayın ve seçin. **Ekle**'yi seçin.

    :::image type="content" source="media/signalr-howto-use-managed-identity/user-identity-portal.png" alt-text="Portalda Kullanıcı tarafından atanan bir kimlik ekleme":::

## <a name="use-a-managed-identity-in-serverless-scenarios"></a>Sunucusuz senaryolarda yönetilen bir kimlik kullanma

Azure SignalR hizmeti, tam olarak yönetilen bir hizmettir, bu nedenle belirteçleri el ile almak için yönetilen bir kimlik kullanamazsınız. Bunun yerine, Azure SignalR hizmeti, erişim belirteci almak için ayarladığınız yönetilen kimliği kullanır. Daha sonra hizmet, erişim belirtecini `Authorization` sunucusuz senaryolarda bir yukarı akış isteğindeki üstbilgiye ayarlar.

### <a name="enable-managed-identity-authentication-in-upstream-settings"></a>Yukarı akış ayarlarında yönetilen kimlik doğrulamasını etkinleştir

1. Sistem tarafından atanan bir kimlik veya Kullanıcı tarafından atanan kimlik ekleyin.

2. Yukarı akış ayarlarını yapılandırın ve **kimlik doğrulama** ayarları olarak **managedıdentity** kullanın. Kimlik doğrulamasıyla yukarı akış ayarları oluşturmayı öğrenmek için bkz. [yukarı akış ayarları](concept-upstream.md).

3. Yönetilen kimlik kimlik doğrulama ayarları ' nda, **kaynak**için hedef kaynağı belirtebilirsiniz. Kaynak, `aud` yukarı akış uç noktalarında doğrulamanın bir parçası olarak kullanılabilecek, alınan erişim belirtecinde bir talep olacaktır. Kaynak aşağıdakilerden biri olabilir:
    - Olmamalıdır
    - Hizmet sorumlusunun uygulama (istemci) KIMLIĞI
    - Hizmet sorumlusunun uygulama KIMLIĞI URI 'SI
    - [Azure hizmetinin kaynak KIMLIĞI](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities#azure-services-that-support-azure-ad-authentication)

    > [!NOTE]
    > Bir erişim belirtecini hizmetinize kendiniz doğruladıysanız, kaynak biçimlerinden birini seçebilirsiniz. Yalnızca **kimlik doğrulama** ayarlarındaki **kaynak** değerinin ve doğrulamanın tutarlı olduğundan emin olun. Bir veri düzlemi için rol tabanlı erişim denetimi (RBAC) kullanıyorsanız, hizmet sağlayıcısının istediği kaynağı kullanmanız gerekir.

### <a name="validate-access-tokens"></a>Erişim belirteçlerini doğrula

`Authorization`Başlıktaki belirteç bir [Microsoft Identity platform erişim belirtecidir](https://docs.microsoft.com/azure/active-directory/develop/access-tokens#validating-tokens).

Erişim belirteçlerini doğrulamak için uygulamanızın izleyiciyi ve imza belirteçlerini de doğrulaması gerekir. Bu, OpenID keşif belgesindeki değerlere karşı doğrulanması gerekir. Örneğin, [belgenin kiracı bağımsız sürümüne](https://login.microsoftonline.com/common/.well-known/openid-configuration)bakın.

Azure Active Directory (Azure AD) ara yazılımı, erişim belirteçlerini doğrulamaya yönelik yerleşik yeteneklere sahiptir. Seçtiğiniz dilde bir tane bulmak için [örneklerimize](https://docs.microsoft.com/azure/active-directory/develop/sample-v2-code) göz atabilirsiniz.

Belirteç doğrulamanın nasıl işleneceğini gösteren kitaplıklar ve kod örnekleri sağlıyoruz. JSON Web Token (JWT) doğrulaması için kullanılabilen çeşitli açık kaynak iş ortağı kitaplıkları da vardır. Neredeyse her platform ve dil için en az bir seçenek vardır. Azure AD kimlik doğrulama kitaplıkları ve kod örnekleri hakkında daha fazla bilgi için bkz. [Microsoft Identity platform kimlik doğrulama kitaplıkları](https://docs.microsoft.com/azure/active-directory/develop/reference-v2-libraries).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SignalR Hizmeti ile Azure İşlevleri geliştirme ve yapılandırma](signalr-concept-serverless-development-config.md)
