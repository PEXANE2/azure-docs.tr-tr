---
title: "Hızlı başlangıç: bir uygulamayı bir Web API 'SI sunmak üzere yapılandırma | Mavisi"
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, uygulamayı istemci uygulamalar için kullanılabilir hale getirmek üzere yeni bir izni/kapsamı ve rolü ortaya çıkarmak için bir uygulamayı nasıl yapılandıracağınızı öğreneceksiniz.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 08/05/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: 93b0c3392a32a6ff18a285d34fdaede6ceea6528
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87830300"
---
# <a name="quickstart-configure-an-application-to-expose-a-web-api"></a>Hızlı başlangıç: bir uygulamayı bir Web API 'SI göstermek üzere yapılandırma

Bir web API'si geliştirip [izinleri/kapsamları](developer-glossary.md#scopes) ve [rolleri](developer-glossary.md#roles) kullanıma sunarak istemci uygulamaları tarafından kullanılmasını sağlayabilirsiniz. Doğru şekilde yapılandırılmış olan bir web API'sini kullanıma sunmak için yapılması gereken işlemler Graph API ve Office 365 API'leri gibi diğer Microsoft web API'leri için yapılması gerekenlerle aynıdır.

Bu hızlı başlangıçta, bir uygulamayı istemci uygulamalar için kullanılabilir hale getirmek üzere yeni bir kapsam sergilemek üzere nasıl yapılandıracağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Hızlı başlangıç: bir uygulamayı Microsoft Identity platformu Ile kaydetme](quickstart-register-app.md).

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Azure portalında oturum açın ve uygulamayı seçin

Uygulamayı yapılandırmadan önce, aşağıdaki adımları izleyin:

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki Gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve **uygulama kayıtları**' i seçin.
1. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtiğinizde, uygulamanın **Genel Bakış** veya ana kayıt sayfasını görürsünüz.
1. Yeni bir kapsamı kullanıma sunmak için kullanıcı birimi veya uygulama bildirimi arasından kullanmak istediğiniz yöntemi seçin:
    * [Kullanıcı arabirimi aracılığıyla yeni bir kapsamı kullanıma sunma](#expose-a-new-scope-through-the-ui)
    * [Uygulama bildirimi aracılığıyla yeni bir kapsamı veya rolü kullanıma sunma](#expose-a-new-scope-or-role-through-the-application-manifest)

## <a name="expose-a-new-scope-through-the-ui"></a>Kullanıcı arabirimi aracılığıyla yeni bir kapsamı kullanıma sunma

[![Kullanıcı arabirimini kullanarak bir API 'yi kullanıma sunma](./media/quickstart-update-azure-ad-app-preview/expose-api-through-ui-expanded.png)](./media/quickstart-update-azure-ad-app-preview/expose-api-through-ui-expanded.png#lightbox)

Kullanıcı arabirimi aracılığıyla yeni bir kapsamı kullanıma sunmak için:

1. Uygulamanın **Genel Bakış** sayfasında, **API’yi kullanıma sunma** bölümünü seçin.

1. **Kapsam ekle**’yi seçin.

1. Bir **Uygulama Kimliği URI’si** ayarlamadıysanız, bir tane girmeniz istenir. Uygulama kimliği URI’nizi girin veya sağlanan kimliği kullanın ve ardından **Kaydet ve devam et** seçeneğini belirleyin.

1. **Kapsam ekleyin** sayfası görüntülendiğinde, kapsamınızın bilgilerini girin:

    | Alan | Açıklama |
    |-------|-------------|
    | **Kapsam adı** | Kapsamınız için anlamlı bir ad girin.<br><br>Örneğin, `Employees.Read.All`. |
    | **Kimler onaylayabilir** | Bu kapsama kullanıcılar tarafından onay verilip verilemeyeceğini veya yönetici onayının gerekli olup olmadığını seçin. Daha yüksek ayrıcalıklı izinler için **Yalnızca yöneticiler**’i seçin. |
    | **Yönetici onayı görünen adı** | Kapsamınız için yöneticilerin göreceği anlamlı bir açıklama girin.<br><br>Örneğin, `Read-only access to Employee records` |
    | **Yönetici onayı açıklaması** | Kapsamınız için yöneticilerin göreceği anlamlı bir açıklama girin.<br><br>Örneğin, `Allow the application to have read-only access to all Employee data.` |

    Kullanıcılar kapsamınıza onay verebiliyorsa, aşağıdaki alanlar için değerler ekleyin:

    | Alan | Açıklama |
    |-------|-------------|
    | **Kullanıcı onayı görünen adı** | Kapsamınız için kullanıcıların göreceği anlamlı bir ad girin.<br><br>Örneğin, `Read-only access to your Employee records` |
    | **Kullanıcı onayı açıklaması** | Kapsamınız için kullanıcıların göreceği anlamlı bir açıklama girin.<br><br>Örneğin, `Allow the application to have read-only access to your Employee data.` |

1. **Durum**’u ayarlayın ve işiniz bittiğinde **Kapsam ekle**’yi seçin.

1. Seçim Uygulamanızın kullanıcılarına tanımladığınız kapsamlara izin sorulmasını engellemek için, istemci uygulamasını Web API 'nize erişmek üzere "önceden Yetkilendir" seçeneğini kullanabilirsiniz. Kullanıcılarınızın izin reddetme fırsatı olmadığı için *yalnızca* güvendiğiniz istemci uygulamalarına ön yetki vermiş olmanız gerekir.
    1. **Yetkili istemci uygulamaları**altında, **istemci uygulaması Ekle** ' yi seçin.
    1. Önceden yetkilendirmek istediğiniz istemci uygulamanın **uygulama (istemci) kimliğini** girin. Örneğin, daha önce kaydetmiş olduğunuz bir Web uygulamasının.
    1. **Yetkili kapsamlar**altında izin istemini bastırmak istediğiniz kapsamları seçin ve ardından **Uygulama Ekle**' yi seçin.

    İstemci uygulaması artık önceden yetkilendirilmiş bir istemci uygulamasıdır (PCA) ve oturum açarken kullanıcılardan onay istenmez.

1. [Web API'sinin diğer uygulamaların kullanımına sunulduğunu doğrulama](#verify-the-web-api-is-exposed-to-other-applications) adımlarını izleyin.

## <a name="expose-a-new-scope-or-role-through-the-application-manifest"></a>Uygulama bildirimi aracılığıyla yeni bir kapsamı veya rolü kullanıma sunma

Uygulama bildirimi, bir Azure AD uygulama kaydının özniteliklerini tanımlayan uygulama varlığını güncelleştirme mekanizması olarak görev yapar.

[![Bildirimde oauth2Permissions koleksiyonunu kullanarak yeni bir kapsam kullanıma sunma](./media/quickstart-update-azure-ad-app-preview/expose-new-scope-through-app-manifest-expanded.png)](./media/quickstart-update-azure-ad-app-preview/expose-new-scope-through-app-manifest-expanded.png#lightbox)

Uygulama bildirimini düzenleyerek yeni bir kapsamı ortaya çıkarmak için:

1. Uygulamanın **Genel Bakış** sayfasında, **Bildirim** bölümünü seçin. Bildirimi portalda **Düzenleme** seçeneği sunana web tabanlı bir bildirim düzenleyici açılır. İsteğe bağlı olarak **İndir** seçeneğini belirleyip bildirimi yerel ortamda düzenledikten sonra **Yükle** seçeneğiyle uygulamanıza yeniden uygulayabilirsiniz.

    Bu örnekte `oauth2Permissions` koleksiyonuna aşağıdaki JSON öğesini ekleyerek kaynak/API öğesinde `Employees.Read.All` adlı yeni bir kapsamı kullanıma sunma gösterilmektedir.

    `id`Değeri programlı olarak veya [Guidgen](https://www.microsoft.com/download/details.aspx?id=55984)gibi bir GUID oluşturma aracı kullanarak oluşturun.

      ```json
      {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
      }
      ```

1. İşlemi tamamladıktan sonra **Kaydet**’e tıklayın. Web API'nizi dizininizdeki diğer uygulamalar tarafından kullanılacak şekilde yapılandırmış oldunuz.
1. [Web API'sinin diğer uygulamaların kullanımına sunulduğunu doğrulama](#verify-the-web-api-is-exposed-to-other-applications) adımlarını izleyin.

Uygulama varlığı ve şeması hakkında daha fazla bilgi için bkz. Microsoft Graph 'ın [uygulama][ms-graph-application] kaynak türü başvurusu belgeleri.

Şema başvurusu da dahil olmak üzere uygulama bildirimi hakkında daha fazla bilgi için bkz. [Azure AD uygulama bildirimini anlama](reference-app-manifest.md).

## <a name="verify-the-web-api-is-exposed-to-other-applications"></a>Web API'sinin diğer uygulamaların kullanımına sunulduğunu doğrulama

1. Azure AD kiracınıza dönün, **uygulama kayıtları**' yi seçin ve ardından yapılandırmak istediğiniz istemci uygulamasını bulun ve seçin.
1. [Bir istemci uygulamasını web API'lerine erişecek şekilde yapılandırma](quickstart-configure-app-access-web-apis.md) bölümünde belirtilen adımları tekrarlayın.
1. [BIR API seçme](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)adımına geldiğinizde, kaynağı (Web API 'si uygulama kaydı) seçin.
    * Web API uygulaması kaydını Azure portal kullanarak oluşturduysanız API kaynağınız **My API 'Lerim** sekmesinde listelenir.
    * Proje oluşturma sırasında Web API uygulaması kaydınızı oluşturmak için Visual Studio 'ya izin verirseniz, API kaynağınız **Kuruluşumun kullandığı API 'ler** sekmesinde listelenir.

Web API kaynağını seçtikten sonra, istemci izin istekleri için kullanılabilir yeni kapsamı görmeniz gerekir.

## <a name="using-the-exposed-scopes"></a>Sunulan kapsamları kullanma

İstemci, Web API 'nize erişim izinleri ile uygun şekilde yapılandırıldıktan sonra, Azure AD tarafından bir OAuth 2,0 erişim belirteci verilebilir. İstemci, Web API 'sini çağırdığında, kapsam ( `scp` ) talebinin, uygulama kaydında istenen izinlere ayarlanmış erişim belirtecini gösterir.

Gerekirse daha sonra ek kapsamları kullanıma sunabilirsiniz. Web API'niz farklı işlevlerle ilişkilendirilmiş birden fazla kapsamı kullanıma sunabilir. Kaynağınız alınan OAuth 2.0 erişim belirtecindeki kapsam (`scp`) taleplerini değerlendirerek web API'si erişimini çalışma zamanında denetleyebilir.

Uygulamalarınızda, tam kapsam değeri, Web API 'nizin **uygulama KIMLIĞI URI** 'sinin (kaynak) ve **Kapsam adının**bir birleşimi olur.

Örneğin, Web API 'nizin uygulama KIMLIĞI URI 'SI ise `https://contoso.com/api` ve kapsam adınız ise `Employees.Read.All` , tam kapsam şu şekilde olur:

`https://contoso.com/api/Employees.Read.All`

## <a name="next-steps"></a>Sonraki adımlar

Kapsamları yapılandırarak Web API 'nizi kullanıma açdığınıza göre, bu kapsamlara erişim izni ile istemci uygulamanızın kaydını yapılandırın.

> [!div class="nextstepaction"]
> [Web API erişimi için uygulama kaydını yapılandırma](quickstart-configure-app-access-web-apis.md)

<!-- REF LINKS -->
[ms-graph-application]: /graph/api/resources/application
