---
title: Azure AD 'den uygulamalara Kullanıcı hazırlama için bir SCıM uç noktası oluşturun
description: Etki alanları arası kimlik yönetimi (SCıM) sistemi, otomatik Kullanıcı sağlamayı standartlaştırlar. Bir SCıM uç noktası geliştirmeyi, SCıM API 'nizi Azure Active Directory ile tümleştirmeyi ve bulut uygulamalarınıza kullanıcıları ve grupları sağlamayı otomatik hale getirmeye başlamasını öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/07/2020
ms.author: mimart
ms.reviewer: arvinh
ms.custom: aaddev;it-pro;seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ae36af981b113d44ac1b8fd45a1d084760b0294
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98900248"
---
# <a name="tutorial-develop-a-sample-scim-endpoint"></a>Öğretici: örnek SCıM uç noktası geliştirme

Sıfırdan yeni bir uç nokta derlemek istemediğinde, [SCIM](https://aka.ms/scimoverview)ile çalışmaya başlamak için bazı [Başvuru kodları](https://aka.ms/scimreferencecode) oluşturduk. Bu öğretici, Azure 'da SCıM başvuru kodunu dağıtmayı ve Postman kullanarak veya Azure AD SCıM istemcisiyle tümleştirerek test edileceğini açıklar. Yalnızca 5 dakikada bir kod olmadan SCıM uç noktanızı çalışır duruma getirebilirsiniz. Bu öğretici, SıMıM veya bir SıCM uç noktasını test etmeye ilgilenen diğer kişilerle çalışmaya başlamak isteyen geliştiricilere yöneliktir. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Başvuru kodunu indirin
> * SCıM uç noktanızı Azure 'da dağıtma
> * SCıM uç noktanızı test etme

Dahil edilen uç nokta özellikleri şunlardır:

|Uç Nokta|Açıklama|
|---|---|
|`/User`|Kullanıcı kaynağında CRUD işlemleri gerçekleştirme: **oluşturma**, **güncelleştirme**, **silme**, **Al**, **Listele**, **filtre**|
|`/Group`|Bir grup kaynağında CRUD işlemleri gerçekleştirme: **Oluştur**, **Güncelleştir**, **Sil**, **Al**, **Listele**, **Filtrele**|
|`/Schemas`|Desteklenen bir veya daha fazla şema alın.<br/><br/>Her bir hizmet sağlayıcı tarafından desteklenen bir kaynağın öznitelik kümesi farklılık gösterebilir. Örneğin, servis sağlayıcı B, kullanıcılar için "ad", "başlık" ve "phoneNumbers" i destekleirken "ad", "başlık" ve "e-postaları" destekler.|
|`/ResourceTypes`|Desteklenen kaynak türlerini alın.<br/><br/>Her bir hizmet sağlayıcısı tarafından desteklenen kaynak sayısı ve türleri farklılık gösterebilir. Örneğin, servis sağlayıcı B kullanıcıları ve grupları destekleirken, hizmet sağlayıcı A kullanıcıları destekler.|
|`/ServiceProviderConfig`|Hizmet sağlayıcısının SCıM yapılandırmasını al<br/><br/>Her bir hizmet sağlayıcısı tarafından desteklenen SCıM özellikleri farklılık gösterebilir. Örneğin, hizmet sağlayıcısı B, yama Işlemlerini ve şema bulmayı destekleirken, hizmet sağlayıcı A, yama işlemlerini destekler.|

## <a name="download-the-reference-code"></a>Başvuru kodunu indirin

İndirilecek [başvuru kodu](https://github.com/AzureAD/SCIMReferenceCode) aşağıdaki projeleri içerir:

- Bir SCıM API 'SI derlemek ve sağlamak için .NET Core MVC web API 'SI olan **TemforcrossdomainıdentitymanagementMicrosoft.Sys**
- SCıM uç noktasının çalışan bir örneği olan **Microsoft. SCIM. WebHostSample**

Projeler aşağıdaki klasörleri ve dosyaları içerir:

|Dosya/klasör|Description|
|-|-|
|**Şemalar** klasörü| Paylaşılan işlevler için şema gibi bazı soyut sınıflarla birlikte **Kullanıcı** ve **Grup** kaynakları için modeller.<br/><br/> **Kullanıcılar** ve adresler gibi **grupların** karmaşık özniteliklerinin sınıf tanımlarını içeren **öznitelikler** klasörü.|
|**Hizmet** klasörü | Kaynakların sorgulanma ve güncelleştirilme yöntemiyle ilgili eylemler için mantık içerir.<br/><br/> Başvuru kodunda kullanıcıları ve grupları döndürmek için hizmetler vardır.<br/><br/>**Denetleyiciler** klasörü çeşitli SCIM uç noktalarını içerir. Kaynak denetleyicileri kaynak üzerinde CRUD işlemleri gerçekleştirmeye yönelik HTTP fiillerini içerir (**Get**, **Post**, **PUT**, **Patch**, **Delete**). Denetleyiciler, eylemleri gerçekleştirmek için Hizmetleri kullanır.|
|**Protokol** klasörü|, Kaynakların SCıM RFC 'e göre döndürüldüğü şekilde ilgili eylemler için mantığı içerir:<br/><ul><li>Birden çok kaynak liste olarak döndürülüyor.</li><li>Yalnızca belirli kaynaklar bir filtreye göre döndürülüyor.</li><li>Bir sorguyu tek filtrelerin bağlantılı listeleri listesine dönüştürme.</li><li>Bir yama isteğini değer yolu ile ilgili özniteliklerle bir işleme açma.</li><li>Kaynak nesnelerine değişiklikler uygulamak için kullanılabilecek işlem türünü tanımlama.</li></ul>|
|`Microsoft.SystemForCrossDomainIdentityManagement`| Örnek kaynak kodu.|
|`Microsoft.SCIM.WebHostSample`| SCıM kitaplığının örnek uygulama.|
|*. gitignore*|Kayıt zamanında yoksayılacağını tanımlayın.|
|*CHANGELOG.md*|Örnekteki değişikliklerin listesi.|
|*CONTRIBUTING.md*|Örneğe katkıda bulunmak için yönergeler.|
|*BENİOKU.MD*|Bu **Benioku** dosyası.|
|*LISAN*|Örnek için lisans.|

> [!NOTE]
> Bu kod, bir SCıM uç noktası oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır ve **olarak** sağlanır. Dahil edilen başvuruların etkin bakım veya destek garantisi yoktur.
>
> Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur. Topluluktaki [katkılar](https://github.com/AzureAD/SCIMReferenceCode/wiki/Contributing-Overview) , depoyu oluşturmaya ve tutmaya yardımcı olmak ve diğer açık kaynaklı katkılar gibi, katkıda bulunan lisans sözleşmesi 'NI (CLA) kabul etmiş olursunuz. Bu anlaşma, sahip olduğunuz ve katkılarınızı kullanma haklarına verdiğiniz bilgileri bildirir ve Ayrıntılar için bkz. [Microsoft açık kaynak](https://cla.opensource.microsoft.com).
>
> Daha fazla bilgi için [Kullanım Kuralları hakkında SSS](https://opensource.microsoft.com/codeofconduct/faq/) bölümüne bakın veya başka soru ya da görüşleriniz olursa [opencode@microsoft.com](mailto:opencode@microsoft.com) ile iletişime geçin.

###  <a name="use-multiple-environments"></a>Birden çok ortam kullanma

Dahil edilen SCıM kodu, geliştirme sırasında ve dağıtımdan sonra kullanım için Yetkilendirmeyi denetlemek üzere bir ASP.NET Core ortamı kullanır, bkz. [ASP.NET Core birden çok ortam kullanma](https://docs.microsoft.com/aspnet/core/fundamentals/environments?view=aspnetcore-3.1).

```csharp
private readonly IWebHostEnvironment _env;
...

public void ConfigureServices(IServiceCollection services)
{
    if (_env.IsDevelopment())
    {
        ...
    }
    else
    {
        ...
    }
```

## <a name="deploy-your-scim-endpoint-in-azure"></a>SCıM uç noktanızı Azure 'da dağıtma

Burada sunulan adımlar, [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) ve [Azure Uygulama Hizmetleri](https://docs.microsoft.com/azure/app-service/)'ni kullanarak SCIM uç noktasını bir hizmete dağıtır. SCıM başvuru kodu, şirket içi sunucu tarafından barındırılan veya başka bir dış hizmete dağıtılan yerel olarak da çalıştırılabilir. 

### <a name="open-solution-and-deploy-to-azure-app-service"></a>Çözümü açın ve Azure App Service dağıtın

1. GitHub 'dan [başvuru koduna](https://github.com/AzureAD/SCIMReferenceCode) gidin ve **Kopyala ya da indir**' i seçin.

1. **Masaüstünde açmayı** seçin veya bağlantıyı kopyalayın, **Visual Studio**'yu açın, kopyalanmış bağlantıyı girmek ve yerel bir kopya oluşturmak için Kopyala ' yı **veya dışarı çek kodu** ' nu seçin.

1. **Visual Studio**'da, barındırma kaynaklarınıza erişimi olan hesapta oturum açmayı unutmayın.

1. **Çözüm Gezgini**, *Microsoft. SCIM. sln* dosyasını açın ve *Microsoft. SCIM. webhostsample* dosyasına sağ tıklayın. **Yayımla**’yı seçin.

    ![bulut yayımlama](media/use-scim-to-build-users-and-groups-endpoints/cloud-publish.png)

    > [!NOTE]
    > Bu çözümü yerel olarak çalıştırmak için projeye çift tıklayın ve **IIS Express** ' ı seçerek projeyi yerel ana bilgisayar URL 'si olan bir Web sayfası olarak başlatın.

1. **Profil oluştur** ' u seçin ve **App Service** ve **Yeni oluştur** ' un seçili olduğundan emin olun.

    ![bulut yayımlama 2](media/use-scim-to-build-users-and-groups-endpoints/cloud-publish-2.png)

1. İletişim kutusu seçeneklerinde ilerleyin ve uygulamayı dilediğiniz bir adla yeniden adlandırın. Bu ad hem uygulama hem de SCıM uç nokta URL 'sinde kullanılır.

    ![bulut yayımlama 3](media/use-scim-to-build-users-and-groups-endpoints/cloud-publish-3.png)

1. Kullanılacak kaynak grubunu seçin ve **Yayımla**' yı seçin.

1. **Azure Uygulama Hizmetleri** yapılandırması ' nda uygulamaya gidin  >   ve **Yeni uygulama ayarı** ' nı seçerek *Token__TokenIssuer* ayarı değerini ekleyin `https://sts.windows.net/<tenant_id>/` . `<tenant_id>`Azure AD tenant_id ile değiştirin ve [Postman](https://github.com/AzureAD/SCIMReferenceCode/wiki/Test-Your-SCIM-Endpoint)kullanarak SCIM uç noktasını test etmek istiyorsanız, bu değere sahip bir *ASPNETCORE_ENVIRONMENT* ayarı da ekleyin `Development` . 

   ![appservice ayarları](media/use-scim-to-build-users-and-groups-endpoints/app-service-settings.png)

   Uç noktanızı Azure portal bir kurumsal uygulamayla sınarken, ortamı farklı tutmayı seçin `Development` ve `/scim/token` test için uç noktadan oluşturulan belirteci sağlayın ya da ortamı olarak değiştirin `Production` ve belirteç alanını [Azure Portal](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#step-4-integrate-your-scim-endpoint-with-the-azure-ad-scim-client)kurumsal uygulamada boş bırakın. 

İşte bu kadar! SCıM uç noktanız artık yayımlandı ve SCıM uç noktasını test etmek için Azure App Service URL 'sini kullanmanıza izin verir.

## <a name="test-your-scim-endpoint"></a>SCıM uç noktanızı test etme

Bir SCıM uç noktasına yapılan istekler yetkilendirme gerektirir ve SCıM standardı, kimlik bilgileri, temel kimlik doğrulaması, TLS istemci kimlik doğrulaması veya [RFC 7644](https://tools.ietf.org/html/rfc7644#section-2)' de listelenen yöntemlerden herhangi biri gibi kimlik doğrulama ve yetkilendirme için birden çok seçeneği bırakır.

Kullanıcı adı/parola gibi güvenli olmayan yöntemlerden, OAuth gibi daha güvenli bir yöntem yararına kaçındığınızdan emin olun. Azure AD, uzun süreli taşıyıcı belirteçleri (Galeri ve Galeri olmayan uygulamalar için) ve OAuth yetkilendirme izni (uygulama galerisinde yayımlanan uygulamalar için) destekler.

> [!NOTE]
> Depoda belirtilen yetkilendirme yöntemleri yalnızca test içindir. Azure AD ile tümleştirilirken, yetkilendirme kılavuzunu gözden geçirebilirsiniz, bkz. [BIR SCıM uç noktası Için plan sağlama](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#authorization-for-provisioning-connectors-in-the-application-gallery). 

Geliştirme ortamı, güvenlik belirteci doğrulamasının davranışını denetlemek için başvuru kodu gibi üretime yönelik güvenli olmayan özellikleri sağlar. Belirteç doğrulama kodu, kendinden imzalı bir güvenlik belirteci kullanacak şekilde yapılandırılmıştır ve imzalama anahtarı yapılandırma dosyasında depolanır, dosyadaki *appsettings.Development.js* **belirteç: ıssuersigningkey** parametresine bakın.

```json
"Token": {
    "TokenAudience": "Microsoft.Security.Bearer",
    "TokenIssuer": "Microsoft.Security.Bearer",
    "IssuerSigningKey": "A1B2C3D4E5F6A1B2C3D4E5F6",
    "TokenLifetimeInMins": "120"
}
```

> [!NOTE]
> Uç noktaya bir **Get** isteği göndererek `/scim/token` , yapılandırılmış anahtar kullanılarak bir belirteç verilir ve sonraki yetkilendirme için taşıyıcı belirteci olarak kullanılabilir.

Varsayılan belirteç doğrulama kodu, Azure Active Directory tarafından verilen belirteci kullanacak şekilde yapılandırılmıştır ve veren kiracının dosyadaki *appsettings.js* **belirteç: tokenıssuer** parametresi kullanılarak yapılandırılmasını gerektirir.

``` json
"Token": {
    "TokenAudience": "8adf8e6e-67b2-4cf2-a259-e3dc5476c621",
    "TokenIssuer": "https://sts.windows.net/<tenant_id>/"
}
```

### <a name="use-postman-to-test-endpoints"></a>Uç noktaları test etmek için Postman kullanma

SCıM uç noktası dağıtıldıktan sonra, SCıM RFC uyumlu olduğundan emin olmak için test edebilirsiniz. Bu örnek, kullanıcılar ve gruplar üzerinde CRUD işlemlerini doğrulamak, filtreleme, grup üyeliğine yönelik güncelleştirmeler ve kullanıcıları devre dışı bırakmak için **Postman** 'da bir test kümesi sağlar.

Uç noktalar `{host}/scim/` dizinde bulunur ve standart http istekleri kullanılarak etkileşim edilebilir. Yolu değiştirmek için `/scim/` , bkz. *ControllerConstant.cs* in **azureadprovisioningscımreference**  >  **scımreferenceapı**  >  **Controllers**.

> [!NOTE]
> Yalnızca yerel testler için HTTP uç noktalarını kullanabilirsiniz, çünkü Azure AD sağlama hizmeti uç noktanızın HTTPS desteklemesini gerektirir.

#### <a name="open-postman-and-run-tests"></a>Postman açma ve Testleri çalıştırma

1. [Postman](https://www.getpostman.com/downloads/) indirin ve uygulamayı başlatın.
1. Bağlantıyı kopyalayın [https://aka.ms/ProvisioningPostman](https://aka.ms/ProvisioningPostman) ve test koleksiyonunu içeri aktarmak Için Postman içine yapıştırın.

    ![Postman koleksiyonu](media/use-scim-to-build-users-and-groups-endpoints/postman-collection.png)

1. Aşağıdaki değişkenlerle bir test ortamı oluşturun:

   |Ortam|Değişken|Değer|
   |-|-|-|
   |IIS Express kullanarak projeyi yerel olarak çalıştırma|||
   ||**Sunucu**|`localhost`|
   ||**Bağlantı noktası**|`:44359`*(şunu unutmayın **:**)*|
   ||**'Sindeki**|`scim`|
   |Kestrel kullanarak projeyi yerel olarak çalıştırma|||
   ||**Sunucu**|`localhost`|
   ||**Bağlantı noktası**|`:5001`*(şunu unutmayın **:**)*|
   ||**'Sindeki**|`scim`|
   |Uç noktayı Azure 'da barındırma|||
   ||**Sunucu**|*(SCıM URL 'nizi girin)*|
   ||**Bağlantı noktası**|*(boş bırakın)*|
   ||**'Sindeki**|`scim`|

1. Belirteç uç noktasına **Get** isteği göndermek ve sonraki istekler için **belirteç** değişkeninde depolanacak bir güvenlik belirteci almak Için Postman koleksiyonundan **Al anahtarını** kullanın. 

   ![Postman anahtar al](media/use-scim-to-build-users-and-groups-endpoints/postman-get-key.png)

   > [!NOTE]
   > SCıM uç noktalarını güvenli hale getirmek için, bağlanmadan önce bir güvenlik belirtecine ihtiyacınız vardır ve öğretici, otomatik olarak `{host}/scim/token` imzalanan bir belirteç oluşturmak için uç noktayı kullanır.

İşte bu kadar! Artık SCıM uç noktası işlevini test etmek için **Postman** koleksiyonunu çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Bir istemci için birlikte çalışabilirlik ile bir SCıM uyumlu Kullanıcı ve grup uç noktası geliştirmek için, bkz. [SCIM istemci uygulama](http://www.simplecloud.info/#Implementations2).

> [!div class="nextstepaction"]
> [Öğretici: SCıM uç noktası Için geliştirme ve plan sağlama](use-scim-to-provision-users-and-groups.md) 
>  [Öğretici: bir galeri uygulaması için sağlamayı yapılandırma](configure-automatic-user-provisioning-portal.md)