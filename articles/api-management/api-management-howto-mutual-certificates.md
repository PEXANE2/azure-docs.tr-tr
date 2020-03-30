---
title: İstemci sertifikası kimlik doğrulamasını kullanarak güvenli arka uç hizmetleri
titleSuffix: Azure API Management
description: Azure API Yönetimi'nde istemci sertifikası kimlik doğrulamasını kullanarak arka uç hizmetlerini nasıl güvence altına alaabileceğinizi öğrenin.
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
ms.openlocfilehash: b0ddf6dda99ee666e3052b5a70e51c7e4208a374
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80347098"
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Azure API Management'ta istemci sertifikası kimlik doğrulamasını kullanarak arka uç hizmetlerinin güvenliğini sağlama

API Yönetimi, istemci sertifikalarını kullanarak bir API'nin arka uç hizmetine erişimi sağlamanızı sağlar. Bu kılavuz, Azure portalındaki Azure API Yönetimi hizmeti örneğindeki sertifikaların nasıl yönetileceğinizi gösterir. Ayrıca, bir ARKA Uç hizmetine erişmek için bir sertifikayı kullanmak üzere API'yi nasıl yapılandırılabildiğini de açıklar.

API Yönetimi REST API'sini kullanarak sertifikaları yönetme hakkında bilgi için <a href="https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">Azure API Yönetimi REST API Sertifikası varlığına</a>bakın.

## <a name="prerequisites"></a><a name="prerequisites"> </a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu kılavuz, API'nin arka uç hizmetine erişmek için istemci sertifikası kimlik doğrulamasını kullanacak ŞEKILDE API Yönetimi hizmet örneğini nasıl yapılandırabileceğinizi gösterir. Bu makaledeki adımları takip etmeden önce, arka uç hizmetinizin istemci sertifikası kimlik doğrulaması için yapılandırılması gerekir[(Azure Uygulama Hizmeti'nde sertifika kimlik doğrulamasını yapılandırmak için bu makaleye bakın).][to configure certificate authentication in Azure WebSites refer to this article] API Yönetimi hizmetine yüklemek için sertifikaya ve parolaya erişmeniz gerekir.

## <a name="upload-a-certificate"></a><a name="step1"> </a>Sertifika Yükleme

> [!NOTE]
> Yüklenen bir sertifika yerine, bu [örnekte](https://github.com/galiniliev/api-management-policy-snippets/blob/galin/AkvCert/examples/Look%20up%20Key%20Vault%20certificate%20using%20Managed%20Service%20Identity%20and%20call%20backend.policy.xml)gösterildiği gibi [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) hizmetinde depolanan bir sertifikakullanabilirsiniz.

![İstemci sertifikaları ekleme](media/api-management-howto-mutual-certificates/apim-client-cert-new.png)

Yeni bir istemci sertifikası yüklemek için aşağıdaki adımları izleyin. Henüz bir API Yönetimi hizmeti örneği oluşturmadıysanız, bir [API Yönetimi hizmeti örneği oluştur][Create an API Management service instance]öğreticiye bakın.

1. Azure portalındaki Azure API Yönetimi hizmet örneğinize gidin.
2. Menüden **Sertifikalar'ı** seçin.
3. **+ Ekle** düğmesine tıklayın.
    ![İstemci sertifikaları ekleme](media/api-management-howto-mutual-certificates/apim-client-cert-add.png)
4. Sertifikaya göz atın, kimliğini ve parolasını girin.
5. **Oluştur'u**tıklatın.

> [!NOTE]
> Sertifika **.pfx** formatında olmalıdır. Kendi imzalı sertifikalara izin verilir.

Sertifika yüklendikten sonra **Sertifikalarda**gösterir.  Çok sayıda sertifikanız varsa, [ağ geçidi kimlik doğrulaması için istemci sertifikası kullanacak bir API'yi yapılandırmak için][Configure an API to use a client certificate for gateway authentication]istenen sertifikanın parmak izini not edin.

> [!NOTE]
> Örneğin kendi imzalı bir sertifika kullanırken sertifika zinciri doğrulaması kapatmak için bu SSS [öğesinde](api-management-faq.md#can-i-use-a-self-signed-tlsssl-certificate-for-a-back-end)açıklanan adımları izleyin.

## <a name="delete-a-client-certificate"></a><a name="step1a"> </a>İstemci sertifikasını silme

Bir sertifikayı silmek için bağlam **menüsünü** ... tıklatın ve sertifikanın yanındaki **Sil'i** seçin.

![İstemci sertifikalarını silme](media/api-management-howto-mutual-certificates/apim-client-cert-delete-new.png)

Sertifika bir API tarafından kullanılıyorsa, bir uyarı ekranı görüntülenir. Sertifikayı silmek için, sertifikayı kullanmak üzere yapılandırılan tüm API'lerden kaldırmanız gerekir.

![İstemci sertifikaları hatalarını silme](media/api-management-howto-mutual-certificates/apim-client-cert-delete-failure.png)

## <a name="configure-an-api-to-use-a-client-certificate-for-gateway-authentication"></a><a name="step2"> </a>Ağ geçidi kimlik doğrulaması için istemci sertifikası kullanacak bir API'yi yapılandırma

1. Soldaki **API Yönetimi** menüsünden **API'leri** tıklatın ve API'ye gidin.
    ![İstemci sertifikalarını etkinleştirme](media/api-management-howto-mutual-certificates/apim-client-cert-enable.png)

2. **Tasarım** sekmesinde, **Arka Uç** bölümünün bir kalem simgesine tıklayın.
3. Ağ **Geçidi kimlik bilgilerini** **İstemci sertifikası** ile değiştirin ve açılır geçitten sertifikanızı seçin.
    ![İstemci sertifikalarını etkinleştirme](media/api-management-howto-mutual-certificates/apim-client-cert-enable-select.png)

4. **Kaydet**'e tıklayın.

> [!WARNING]
> Bu değişiklik hemen etkili olur ve bu API işlemleri için çağrılar arka uç sunucuda kimlik doğrulaması için sertifika kullanır.


> [!TIP]
> Bir API'nin arka uç hizmeti için ağ geçidi kimlik doğrulaması için bir sertifika belirtildiğinde, bu API için ilkenin bir parçası olur ve ilke düzenleyicisinde görüntülenebilir.

## <a name="self-signed-certificates"></a>Kendi imzalı sertifikalar

Kendi imzalı sertifikalar kullanıyorsanız, API Yönetimi'nin arka uç sistemiyle iletişim kurabilmesi için sertifika zinciri doğrulaması devre dışı bırakmanız gerekir. Aksi takdirde bir 500 hata kodu döndürecek. Bunu yapılandırmak [`New-AzApiManagementBackend`](https://docs.microsoft.com/powershell/module/az.apimanagement/new-azapimanagementbackend) için, (yeni arka uç için) veya [`Set-AzApiManagementBackend`](https://docs.microsoft.com/powershell/module/az.apimanagement/set-azapimanagementbackend) (mevcut arka uç için) `-SkipCertificateChainValidation` PowerShell `True`cmdlets kullanabilirsiniz ve parametre yi ayarlamak .

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
