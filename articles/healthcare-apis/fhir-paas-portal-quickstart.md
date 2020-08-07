---
title: 'Hızlı başlangıç: Azure portal kullanarak FHıR için Azure API dağıtma'
description: Bu hızlı başlangıçta, FHıR için Azure API 'SI dağıtmayı ve Azure portal kullanarak ayarları yapılandırmayı öğreneceksiniz.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 03/15/2020
ms.author: matjazl
ms.openlocfilehash: 8c0448d31cd89e2ca969b81361b30bac3f9610e9
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87851945"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak FHıR için Azure API dağıtma

Bu hızlı başlangıçta, Azure portal kullanarak FHıR için Azure API 'yi dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-new-resource"></a>Yeni kaynak oluştur

[Azure Portal](https://portal.azure.com) açın ve **kaynak oluştur ' a** tıklayın

![Kaynak oluşturma](media/quickstart-paas-portal/portal-create-resource.png)

## <a name="search-for-azure-api-for-fhir"></a>FHıR için Azure API arama

Arama kutusuna "FHIR" yazarak FHıR için Azure API 'sini bulabilirsiniz:

:::image type="content" source="media/quickstart-paas-portal/portal-search-healthcare-apis.png" alt-text="Sağlık API 'Leri arama":::

## <a name="create-azure-api-for-fhir-account"></a>FHıR hesabı için Azure API oluşturma

FHıR hesabı için yeni bir Azure API 'SI oluşturmak için **Oluştur** ' u seçin:

:::image type="content" source="media/quickstart-paas-portal/portal-create-healthcare-apis.png" alt-text="FHıR hesabı için Azure API oluşturma":::

## <a name="enter-account-details"></a>Hesap ayrıntılarını girin

Var olan bir kaynak grubunu seçin veya yeni bir tane oluşturun, hesap için bir ad seçin ve son olarak **gözden geçir + oluştur**' a tıklayın:

:::image type="content" source="media/quickstart-paas-portal/portal-new-healthcareapi-details.png" alt-text="Yeni sağlık API 'si ayrıntıları":::

Oluşturma ve bekleme API 'SI dağıtımını onaylayın.

## <a name="additional-settings-optional"></a>Ek ayarlar (isteğe bağlı)

Ileri ' ye de tıklayabilirsiniz: kimlik doğrulama ayarlarını görüntülemek için **ek ayarlar** . FHıR için Azure API 'sinin varsayılan yapılandırması, [veri düzlemi rolleri atamak Için Azure RBAC kullanmaktır](configure-azure-rbac.md). Bu modda yapılandırıldığında, FHıR hizmeti için "Authority", aboneliğin Azure Active Directory kiracısına ayarlanır:

:::image type="content" source="media/rbac/confirm-azure-rbac-mode-create.png" alt-text="Varsayılan kimlik doğrulama ayarları":::

Bu durumda rol atamalarını yapılandırmak için Azure RBAC kullandığımızdan, izin verilen nesne kimliklerinin girilmesi için kutunun gri olduğunu unutmayın.

FHıR hizmetini dış veya ikincil Azure Active Directory kiracı kullanacak şekilde yapılandırmak istiyorsanız, yetkiyi değiştirebilir ve sunucu erişimine izin verilmesi gereken kullanıcı ve gruplar için nesne kimliklerini girebilirsiniz. Daha fazla bilgi için bkz. [Yerel RBAC yapılandırma](configure-local-rbac.md) Kılavuzu.

## <a name="fetch-fhir-api-capability-statement"></a>FHıR API özelliği bildirisini getir

Yeni FHıR API hesabının sağlandığını doğrulamak için, bir tarayıcı göstererek bir yetenek beyanı getirin `https://<ACCOUNT-NAME>.azurehealthcareapis.com/metadata` .

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu, FHıR için Azure API 'yi ve tüm ilgili kaynakları silebilirsiniz. Bunu yapmak için, FHıR için Azure API hesabı 'nı içeren kaynak grubunu seçin, kaynak grubunu **Sil**' i seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç kılavuzunda, FHıR için Azure API 'sini aboneliğinize dağıttınız. FHıR için Azure API 'niz üzerinde ek ayarlar ayarlamak için, ek ayarlar nasıl yapılır Kılavuzu ' na ilerleyin.

>[!div class="nextstepaction"]
>[FHıR için Azure API 'de ek ayarlar](azure-api-for-fhir-additional-settings.md)
