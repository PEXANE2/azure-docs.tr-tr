---
title: Azure SignalR hizmetindeki Yönetilen kimlikler
description: Yönetilen kimliklerin Azure SignalR hizmetinde nasıl çalıştığını, sunucusuz senaryolarda yönetilen kimlik kullanmak için nasıl yapılandırılacağını öğrenin.
author: chenyl
ms.service: signalr
ms.topic: article
ms.date: 06/8/2020
ms.author: chenyl
ms.openlocfilehash: 8e122be74d623c7f1ea5576a5fe527dfa538d1d1
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988714"
---
# <a name="how-to-use-managed-identities-for-azure-signalr-service"></a>Azure SignalR hizmeti için Yönetilen kimlikler kullanma

Bu konu başlığı altında, Azure SignalR hizmeti için yönetilen bir kimliğin nasıl oluşturulacağı ve sunucusuz senaryolarda nasıl kullanılacağı gösterilmektedir.

> [!Important] 
> Azure SignalR hizmeti yalnızca tek bir yönetilen kimliği destekleyebilir. Bu, sistem tarafından atanan bir kimlik veya Kullanıcı tarafından atanan bir kimlik ekleyebileceğiniz anlamına gelir. 

## <a name="add-a-system-assigned-identity"></a>Sistem tarafından atanan kimlik ekleme

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Portalda yönetilen bir kimlik ayarlamak için ilk olarak bir SignalR hizmeti oluşturun ve sonra özelliği etkinleştirin.

1. Portalda bir SignalR hizmeti oluşturun. Portalda bu sayfaya gidin.

2. **Kimlik**seçin.

4. **Sistem atandı** sekmesinde **durumu** **Açık**olarak değiştirin. **Kaydet**’e tıklayın.

    :::image type="content" source="media/signalr-howto-use-managed-identity/system-identity-portal.png" alt-text="Portala sistem tarafından atanan kimlik ekleme":::

## <a name="add-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik ekleme

Kullanıcı tarafından atanan bir kimlikle bir SignalR hizmeti oluşturmak için kimlik oluşturmanız ve ardından kaynak tanımlayıcısını hizmetinize eklemeniz gerekir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

İlk olarak, Kullanıcı tarafından atanan bir kimlik kaynağı oluşturmanız gerekir.

1. [Bu yönergelere](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)göre Kullanıcı tarafından atanan bir yönetilen kimlik kaynağı oluşturun.

2. Portalda bir SignalR hizmeti oluşturun. Portalda bu sayfaya gidin.

3. **Kimlik**seçin.

4. **Kullanıcı atandı** sekmesinde **Ekle**' ye tıklayın.

5. Daha önce oluşturduğunuz kimliği arayın ve seçin. **Ekle**'ye tıklayın.

    :::image type="content" source="media/signalr-howto-use-managed-identity/user-identity-portal.png" alt-text="Portalda Kullanıcı tarafından paylaşılan kimlik ekleme":::

## <a name="use-managed-identity-in-serverless-scenarios"></a>Sunucusuz senaryolarda yönetilen kimliği kullanma

SignalR hizmeti tam olarak yönetilen bir hizmettir, bu nedenle belirteçleri el ile almak için yönetilen kimliği kullanamazsınız. Bunun yerine, SignalR hizmeti, erişim belirtecini almak için ayarladığınız yönetilen kimliği kullanır ve `Authorization` sunucusuz senaryolarda bir yukarı akış isteğinde üst bilgi olarak ayarlanır.

### <a name="enable-managed-identity-authentication-in-upstream-settings"></a>Yukarı akış ayarlarında yönetilen kimlik doğrulamasını etkinleştir

1. Sistem tarafından atanan bir kimlik veya Kullanıcı tarafından atanan kimlik ekleme

2. Yukarı akış ayarlarını yapılandırın ve *kimlik doğrulama* ayarları olarak *managedıdentity* kullanın. [Yukarı akış ayarlarında](concept-upstream.md#create-upstream-settings), kimlik doğrulamasıyla yukarı akış ayarları oluşturmayı öğrenin.

3. Yönetilen kimlik doğrulama ayarları ' nda hedef *kaynağı*belirtebilirsiniz. *Kaynak* , `aud` yukarı akış uç noktalarınızda doğrulamanın bir parçası olarak kullanılabilen, alınan erişim belirtecinde talep haline gelir. *Kaynak* aşağıdakilerden biri olabilir
    - Olmamalıdır
    - Hizmet sorumlusunun uygulama (istemci) KIMLIĞI
    - Hizmet sorumlusunun uygulama KIMLIĞI URI 'SI
    - Azure hizmetlerinin kaynak kimlikleri, bkz. [Azure hizmetlerinin kaynak kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities#azure-services-that-support-azure-ad-authentication)

    > [!NOTE]
    > Erişim belirtecini hizmetinize kendiniz doğrularsınız, *kaynak* biçiminden birini seçebilirsiniz. *Kimlik doğrulama* ayarlarındaki *kaynağın* ve doğrulamanın tutarlı olduğundan emin olmak için. Veri düzlemi RBAC kullanıyorsanız, hizmet sağlayıcı isteklerinin *kaynağını* kullanmanız gerekir.

### <a name="validate-access-token"></a>Erişim belirtecini doğrula

`Authorization`Üstbilgideki belirteç bir [Microsoft Identity platform erişim belirtecidir](https://docs.microsoft.com/azure/active-directory/develop/access-tokens#validating-tokens).

Erişim belirteçlerini doğrulamak için uygulamanızın hedef kitleyi ve imzalama belirteçlerini de doğrulaması gerekir. Bu, OpenID keşif belgesindeki değerlere karşı doğrulanması gerekir. Örneğin, belgenin kiracı bağımsız sürümü konumunda bulunur [https://login.microsoftonline.com/common/.well-known/openid-configuration](https://login.microsoftonline.com/common/.well-known/openid-configuration) .

Azure AD ara yazılımı, erişim belirteçlerini doğrulamaya yönelik yerleşik yeteneklere sahiptir ve seçtiğiniz dilde bir tane bulmak için [örneklerimize](https://docs.microsoft.com/azure/active-directory/develop/sample-v2-code) göz atabilirsiniz.

Belirteç doğrulamanın nasıl işleneceğini gösteren kitaplıklar ve kod örnekleri sağlıyoruz. Aşağıdaki bilgiler, temeldeki işlemi anlamak isteyen kişiler için verilmiştir. Ayrıca, JWT doğrulaması için kullanılabilen birkaç üçüncü taraf açık kaynak kitaplığı vardır. neredeyse her platform için en az bir seçenek ve burada dil vardır. Azure AD kimlik doğrulama kitaplıkları ve kod örnekleri hakkında daha fazla bilgi için bkz. [v 2.0 kimlik doğrulama kitaplıkları](https://docs.microsoft.com/azure/active-directory/develop/reference-v2-libraries).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SignalR Hizmeti ile Azure İşlevleri geliştirme ve yapılandırma](signalr-concept-serverless-development-config.md)
