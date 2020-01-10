---
title: İstemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvenli hale getirme
titleSuffix: Azure API Management
description: Azure API Management 'de istemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvenli hale getirme hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/08/2020
ms.author: apimpm
ms.openlocfilehash: 39a1e224173dc021cf49b535957eb4b49f4c91ee
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834330"
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Azure API Management istemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvenli hale getirme

API Management, istemci sertifikalarını kullanarak bir API 'nin arka uç hizmetine erişimi güvenli hale bırakmanıza olanak tanır. Bu kılavuzda, Azure portal Azure API Management hizmet örneğindeki sertifikaların nasıl yönetileceği gösterilmektedir. Ayrıca, bir API 'yi arka uç hizmetine erişmek için bir sertifika kullanmak üzere yapılandırmayı açıklar.

API Management REST API kullanarak sertifikaları yönetme hakkında daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">Azure API Management REST API sertifika varlığı</a>.

## <a name="prerequisites"> </a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu kılavuzda, bir API için arka uç hizmetine erişmek üzere API Management hizmeti örneğinizi istemci sertifikası kimlik doğrulamasını kullanmak üzere nasıl yapılandırabileceğiniz gösterilmektedir. Bu makaledeki adımları uygulamadan önce, arka uç hizmetinizin istemci sertifikası kimlik doğrulaması için yapılandırılmış olması gerekir ([Azure App Service sertifika kimlik doğrulamasını yapılandırmak için bu makaleye başvurun][to configure certificate authentication in Azure WebSites refer to this article]). API Management hizmetine yüklemek için sertifikaya ve parolaya erişmeniz gerekir.

## <a name="step1"> </a>Sertifikayı karşıya yükle

> [!NOTE]
> Karşıya yüklenen bir sertifika yerine, bu [örnekte](https://github.com/galiniliev/api-management-policy-snippets/blob/galin/AkvCert/examples/Look%20up%20Key%20Vault%20certificate%20using%20Managed%20Service%20Identity%20and%20call%20backend.policy.xml)gösterildiği gibi [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) hizmetinde depolanan bir sertifikayı kullanabilirsiniz.

![İstemci sertifikası Ekle](media/api-management-howto-mutual-certificates/apim-client-cert-new.png)

Yeni bir istemci sertifikasını karşıya yüklemek için aşağıdaki adımları izleyin. Henüz bir API Management hizmet örneği oluşturmadıysanız, [API Management hizmet örneği oluşturma][Create an API Management service instance]öğreticisine bakın.

1. Azure portal Azure API Management hizmet örneğinize gidin.
2. Menüden **Sertifikalar** ' ı seçin.
3. **+ Ekle** düğmesine tıklayın.
    istemci sertifikaları eklemek ![](media/api-management-howto-mutual-certificates/apim-client-cert-add.png)
4. Sertifikaya gözatıp KIMLIĞINI ve parolasını belirtin.
5. **Oluştur**'a tıklayın.

> [!NOTE]
> Sertifika **. pfx** biçiminde olmalıdır. Otomatik olarak imzalanan sertifikalara izin verilir.

Sertifika karşıya yüklendikten sonra, **Sertifikalar**içinde görüntülenir.  Birçok sertifikanız varsa, bir [API 'yi Ağ Geçidi kimlik doğrulaması için bir istemci sertifikası kullanacak şekilde yapılandırmak][Configure an API to use a client certificate for gateway authentication]üzere, istenen sertifikanın parmak izini bir yere getirin.

> [!NOTE]
> Örneğin, otomatik olarak imzalanan bir sertifika kullanırken sertifika zinciri doğrulamasını devre dışı bırakmak için, bu SSS [öğesinde](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)açıklanan adımları izleyin.

## <a name="step1a"> </a>İstemci sertifikasını silme

Bir sertifikayı silmek için bağlam menüsü **...** öğesine tıklayın ve sertifikanın yanındaki **Sil** ' i seçin.

![İstemci sertifikalarını Sil](media/api-management-howto-mutual-certificates/apim-client-cert-delete-new.png)

Sertifika bir API tarafından kullanılıyorsa, bir uyarı ekranı görüntülenir. Sertifikayı silmek için önce sertifikayı kullanmak üzere yapılandırılmış API 'lerden kaldırmanız gerekir.

![İstemci sertifikalarını silme hatası](media/api-management-howto-mutual-certificates/apim-client-cert-delete-failure.png)

## <a name="step2"> </a>Bir API 'yi Ağ Geçidi kimlik doğrulaması için bir istemci sertifikası kullanacak şekilde yapılandırma

1. Soldaki **API Management** menüsünde **API 'ler** ' e tıklayın ve API 'ye gidin.
    istemci sertifikalarını etkinleştirmek ![](media/api-management-howto-mutual-certificates/apim-client-cert-enable.png)

2. **Tasarım** sekmesinde, **arka uç** bölümünün kalem simgesine tıklayın.
3. **Ağ Geçidi kimlik bilgilerini** **istemci sertifikası** olarak değiştirin ve açılan listeden sertifikanızı seçin.
    istemci sertifikalarını etkinleştirmek ![](media/api-management-howto-mutual-certificates/apim-client-cert-enable-select.png)

4. **Save (Kaydet)** düğmesine tıklayın.

> [!WARNING]
> Bu değişiklik hemen etkili olur ve bu API işlemlerine yapılan çağrılar, arka uç sunucusunda kimlik doğrulaması yapmak için sertifikayı kullanır.


> [!TIP]
> Bir API 'nin arka uç hizmeti için Ağ Geçidi kimlik doğrulaması için bir sertifika belirtildiğinde, bu API ilkesinin bir parçası haline gelir ve ilke düzenleyicisinde görüntülenebilir.

## <a name="self-signed-certificates"></a>Otomatik olarak imzalanan sertifikalar

Otomatik olarak imzalanan sertifikalar kullanıyorsanız, API Management arka uç sistemiyle iletişim kurması için sertifika zinciri doğrulamasını devre dışı bırakmanız gerekir. Aksi takdirde, 500 hata kodu döndürür. Bunu yapılandırmak için [`New-AzApiManagementBackend`](https://docs.microsoft.com/powershell/module/az.apimanagement/new-azapimanagementbackend) (yeni arka uç için) veya [`Set-AzApiManagementBackend`](https://docs.microsoft.com/powershell/module/az.apimanagement/set-azapimanagementbackend) (mevcut arka uç için) PowerShell cmdlet 'lerini kullanabilir ve `-SkipCertificateChainValidation` parametresini `True`olarak ayarlayabilirsiniz.

```powershell
$context = New-AzApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[Azure API Management REST API Certificate entity]: https://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[to configure certificate authentication in Azure WebSites refer to this article]: ../app-service/app-service-web-configure-tls-mutual-auth.md

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API to use a client certificate for gateway authentication]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps
