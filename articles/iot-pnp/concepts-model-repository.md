---
title: Azure IoT model deposunun kavramlarını anlayın | Microsoft Docs
description: Bir çözüm geliştiricisi veya BT uzmanı olarak, Azure IoT model deposunun temel kavramları hakkında bilgi edinin.
author: prashmo
ms.author: prashmo
ms.date: 07/24/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 7d736721e2676a42da90aead3144f8016329f730
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475507"
---
# <a name="azure-iot-model-repository"></a>Azure IoT model deposu

Azure IoT model deposu, cihaz oluşturucuların IoT Tak ve Kullan cihaz modellerini yönetmesine ve paylaşmasına olanak sağlar. Cihaz modelleri, [dijital TWINS modelleme dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)KULLANıLARAK tanımlanan JSON ld belgelerdir. Model deposu hizmetinde depolanan modeller, özel olarak erişim denetimi aracılığıyla veya IoT Tak ve Kullan bulut çözümünü bütünleştirmek ve geliştirmek için herhangi bir kimlik doğrulaması gerektirmeden, çözüm geliştiricileri ile genel olarak paylaşılabilir.

Model deposuna şunu kullanarak erişebilirsiniz:

- [Azure IoT model deposu](https://aka.ms/iotmodelrepo) portalı
- [Azure IoT model deposu REST API](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/getmodelasync/getmodelasync)
- [Azure CLı IoT modeli depo komutları](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp?view=azure-cli-latest)

## <a name="public-models"></a>Ortak modeller

Model deposunda depolanan genel dijital ikizi modelleri, herhangi bir kimlik doğrulaması olmadan uygulamasında kullanmak ve bütünleştirmek için herkes tarafından kullanılabilir. Ayrıca, genel modeller cihaz oluşturucular ve çözüm geliştiricilerinin IoT Tak ve Kullan cihaz modellerini paylaşmasını ve yeniden kullanmasına olanak sağlayan açık bir ekonomik sistem sağlar.

Ortak hale getirmek için model deposunda model yayımlama yönergeleri için **Şirket modelleri** altındaki [model yayımlama](#publish-a-model) bölümüne bakın.

Model deposu portalını kullanarak bir ortak modeli görüntülemek için:

1. [Azure IoT model deposu portalı](https://aka.ms/iotmodelrepo)' na gidin.

1. **Ortak modelleri görüntüle**' yi seçin.

    ![Ortak modelleri görüntüle](./media/concepts-model-repository/public-models.png)

REST API kullanarak ortak modeli program aracılığıyla görüntülemek için bkz. model REST API belgelerini [Al](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/getmodelasync/getmodelasync) .

```csharp
var httpClient = new HttpClient();
httpClient.BaseAddress = new Uri("https://repo.azureiotrepository.com");

var modelId = "dtmi:com:mxchip:model;1";
var response = await httpClient.GetAsync($"/models/{modelId}?api-version=2020-05-01-preview").ConfigureAwait(false);
```

CLı kullanarak ortak bir modeli görüntülemek için bkz. Azure CLı [model al](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp/model?view=azure-cli-latest#ext-azure-iot-az-iot-pnp-model-show) komutu.

## <a name="company-models"></a>Şirket modelleri

Şirket modeli deposu, kuruluşunuz için Azure IoT model deposundaki, şirketinizin veya kuruluşunuzun kullanıcıları tarafından yazılan dijital ikizi modellerini oluşturmak ve yönetmek için bir kiracıya yöneliktir. Şirket modelleri yalnızca şirket veya kuruluşunuzun kimliği doğrulanmış kullanıcıları tarafından kullanılabilir. Model deposu kiracı yöneticisi, şirket veya kuruluştaki diğer kullanıcılara şirket modeli deposundaki modellere izin atayabilir ve erişimi denetleyebilir.

### <a name="set-up-your-company-model-repository"></a>Şirket modeli deponuzu ayarlama

Model deposuna erişmek için *iş veya okul Azure Active Directory (Azure AD) hesabınızı* kullanın. Kuruluşunuzun zaten bir Azure AD kiracısı varsa, bu Azure AD kiracısında Kullanıcı hesaplarını ve hizmet sorumlularını kullanabilirsiniz.

Bir Azure AD kiracısı ayarlama ve bir Azure AD kiracısında Kullanıcı veya hizmet sorumlusu oluşturma hakkında bilgi edinmek için bkz. [ek bilgi](#additional-information) bölümü.

- Kuruluşunuzdaki ilk kullanıcı model deposuna erişmek veya portalda oturum açmak için, **Kiracı Yöneticisi** rolü vermiş olursunuz. Bu rol, kuruluşunuzun depo kiracısındaki diğer kullanıcılara roller atamanıza olanak tanır.

- Başka roller, **Okuma modelleri** veya **model oluşturma**gibi bir **Kiracı Yöneticisi** atanabilir.

### <a name="understand-access-management"></a>Erişim yönetimini anlama

Aşağıdaki tabloda, şirket modeli deposundaki desteklenen yetenekler ve bunlarla ilişkili izinler özetlenmektedir:

| Özellik  | İzin| Açıklama|
|-------------|-----------|------------|
|Modelleri oku|Modelleri oku|Varsayılan olarak, şirket kiracısındaki tüm kullanıcılar şirket modellerini görüntüleyebilir. Ayrıca, Kullanıcı aynı zamanda diğer şirketler tarafından bunlara paylaşılan özel model (ler) de görüntüleyebilir.|
|Erişimi Yönetme|Erişimi Yönetme|Kuruluştaki diğer kullanıcılar için Kullanıcı rolü atamasını (ekleme veya kaldırma) yönetin.|
|Model oluşturma|Model oluşturma|Şirket modeli deposunda modeller oluşturun.|
|Modelleri Yayımla|Modelleri Yayımla|Modelleri, modelin görüntülemesi için herkese açık hale getirmek üzere yayımlayın.|

Aşağıdaki tabloda, desteklenen roller ve erişim yönetimi için kullanılabilen model deposundaki özellikleri özetlenmektedir.

|Rol|Özellik|
|----|----------|
|TenantAdministrator|Erişimi yönetme, modelleri okuma|
|Oluşturucu|Model oluşturma, modelleri okuma|
|Publisher|Modelleri Yayımla, modelleri oku|

#### <a name="passing-a-security-token-when-accessing-company-models-with-a-rest-api"></a>Bir REST API şirket modellerine erişirken bir güvenlik belirteci geçirme

Özel veya paylaşılan şirket modellerini yönetmek için REST API 'Lerini çağırdığınızda, Kullanıcı veya hizmet sorumlusu için JWT biçiminde bir yetkilendirme belirteci sağlamanız gerekir. Bir kullanıcı veya hizmet sorumlusu için JWT belirteci alma hakkında bilgi edinmek için [ek bilgiler](#additional-information) bölümüne bakın.

Şirket modellerini veya paylaşılan modelleri hedeflerken, JWT belirtecinin API 'deki yetkilendirme HTTP üstbilgisine geçirilmesi gerekir. Ortak modeller hedeflenirken JWT belirteci gerekli değildir.

```csharp
// sample token
var authorizationToken = "eyJhbGciOiJIUzI1NiIsInR5cCTI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c";
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authorizationToken);
```

### <a name="view-company-or-shared-models"></a>Şirket veya paylaşılan modelleri görüntüleme

Bir modeli okumak için depo kiracının *okuyucu* rolünün bir üyesi olmanız veya modelin sizinle paylaşılması gerekir. Sizinle paylaşılan yayımdan kaldırılan modellerin listesini ve sizinle paylaşılan yayımlanmış modellerin listesini görebilirsiniz. Varsayılan olarak kullanıcılar, şirketlerinin modellerini, diğer şirketlerin paylaştığı modelleri ve tüm ortak modelleri okuyabilir.

Portalı kullanarak bir şirketi veya paylaşılan modeli görüntülemek için:

1. [Azure IoT model deposu portalında](https://aka.ms/iotmodelrepo)oturum açın.

1. Sol bölmedeki **Şirket modellerini** Genişlet – **yayımdan kaldırıldı**

    ![Şirket modellerini görüntüleme](./media/concepts-model-repository/view-company-models.png)

1. Sol bölmedeki **paylaşılan modelleri Genişlet – yayımdan kaldırıldı**

    ![Paylaşılan modelleri görüntüle](./media/concepts-model-repository/view-shared-models.png)

REST API kullanarak bir şirketi veya paylaşılan modeli görüntülemek için, [modeli al](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/getmodelasync/getmodelasync) REST API belgelerine bakın. HTTP isteğindeki bir JWT yetkilendirme üst bilgisine geçme hakkında daha fazla bilgi için, bkz. [Şirket modellerine erişirken bir güvenlik belirteci geçirme REST API](#passing-a-security-token-when-accessing-company-models-with-a-rest-api) .

```csharp
var modelId = "dtmi:com:mxchip:model;1";
var response = await httpClient.GetAsync($"/models/{modelId}?api-version=2020-05-01-preview").ConfigureAwait(false);
```

CLı kullanarak bir şirket modelini veya paylaşılan modeli görüntülemek için bkz. Azure CLı [model al](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp/model?view=azure-cli-latest#ext-azure-iot-az-iot-pnp-model-show) komutu.

### <a name="manage-roles"></a>Rolleri yönetme

Kiracı Yöneticisi, şirket veya kuruluşa özel modeller oluşturabilmek, modeller yayınlamak veya diğer kullanıcılar için rolleri yönetmek için depo kiracısındaki kullanıcılara roller atayabilir.

Portalı kullanarak bir model deposu kiracı rolüne kullanıcı eklemek için:

1. [Azure IoT model deposu portalında](https://aka.ms/iotmodelrepo)oturum açın.

1. Sol bölmede **erişim yönetimi** ' ni seçin ve **+ Ekle**' yi seçin. **Izin Ekle** bölmesinde, role eklemek istediğiniz kullanıcının çalışma adresini yazın:

    ![İş adresi ekle](./media/concepts-model-repository/add-user.png)

1. **Rol** açılan menüsünden kullanıcıyı eklemek istediğiniz rolü seçin. Sonra **Kaydet**' i seçin:

    ![Rol Seç](./media/concepts-model-repository/choose-role.png)

### <a name="upload-a-model"></a>Bir modeli karşıya yükleyin

Bir modeli şirket modeli deposuna yüklemek için depo kiracının **Oluşturucu** rolünün bir üyesi olmanız gerekir.

Bu modeller yayımlanmaz ve yalnızca kuruluşunuzdaki kullanıcılar tarafından varsayılan olarak erişilebilir. Ayrıca, bir veya daha fazla yayımdan kaldırılmış modeli dış kullanıcılarla paylaşabilirsiniz.

Karşıya yüklenen modeller sabittir.

Bu modellerin model kimlikleri, tüm karşıya yüklenen modellerin tüm depo kiracılar genelinde genel olarak benzersiz olmalıdır.

Portalı kullanarak bir modeli karşıya yüklemek için:

1. [Azure IoT model deposu portalında](https://aka.ms/iotmodelrepo)oturum açın.

1. Sol bölmedeki **Şirket modelleri** ' ni genişletin ve **model oluştur**' u seçin. Ardından **JSON al**' ı seçin.

    ![Model oluştur](./media/concepts-model-repository/create-model.png)

1. Karşıya yüklemek istediğiniz dosyayı seçin. Portal modelinizi başarıyla doğrulaırsa **Kaydet**' i seçin.

REST API kullanarak bir modeli karşıya yüklemek için bkz. [model oluşturma](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/createorupdateasync/createorupdateasync) API 'si. HTTP isteğindeki bir JWT yetkilendirme üst bilgisine geçme hakkında daha fazla bilgi için, bkz. [Şirket modellerine erişirken bir güvenlik belirteci geçirme REST API](#passing-a-security-token-when-accessing-company-models-with-a-rest-api) .

```csharp
var httpContent = new StringContent(jsonLdModel, Encoding.UTF8, "application/json");
var modelId = "dtmi:com:mxchip:model;1";
var response = await httpClient.PutAsync($"/models/{modelId}?api-version=2020-05-01-preview", httpContent).ConfigureAwait(false);
```

CLı kullanarak bir modeli karşıya yüklemek için bkz. Azure CLı [model oluşturma](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp/model?view=azure-cli-latest#ext-azure-iot-az-iot-pnp-model-create) komutu.

### <a name="publish-a-model"></a>Modeli yayımlama

Bir modeli yayımlamak için aşağıdaki gereksinimlerin karşılanması gerekir:

1. Kuruluşunuzun bir modeli yayımlamak için [Microsoft iş ortağı ağı](https://docs.microsoft.com/partner-center/) bir üyesi olması gerekir. Bir iş ortağı Merkezi hesabı oluşturmak için bkz. [Iş Ortağı Merkezi hesabı oluşturma](https://docs.microsoft.com/partner-center/mpn-create-a-partner-center-account). Hesabınız onaylandıktan sonra, modellerinizi yayımlayabilirsiniz. Daha fazla bilgi için bkz. [Iş ortağı MERKEZI SSS](https://support.microsoft.com/help/4340639/partner-center-account-faqs).

2. Kullanıcı, depo kiracının *Yayımcı* rolünün bir üyesi olmalıdır.

Kuruluşunuzdaki kullanıcılar tarafından oluşturulan ve yayımlanan modeller *yayımlanmış modeller*olarak görülebilir. Bu modeller geneldir ve **ortak modeller**altındaki herkes tarafından bulunabilir.

Portalı kullanarak bir model yayımlamak için:

1. [Azure IoT model deposu portalında](https://aka.ms/iotmodelrepo)oturum açın.

2. Sol bölmedeki **Şirket modelleri** ' ni genişletin ve yayımlamak istediğiniz modeli seçin. Ardından **Yayımla**’yı seçin.

    ![Model yayımlama](./media/concepts-model-repository/publish-model.png)

> [!NOTE]
> Bir Microsoft Iş ortağı (MPN) KIMLIĞINIZ olmadığını söyleyen bir bildirim alırsanız, bildirimdeki kayıt adımlarını izleyin. Daha fazla bilgi için bu bölümün başındaki gereksinimlere bakın.

REST API kullanarak bir modeli yayımlamak için bkz. [model yayımlama](https://docs.microsoft.com/rest/api/iothub/digitaltwinmodelrepositoryservice/createorupdateasync/createorupdateasync) REST API belgeleri. `update-metadata=true`REST API kullanarak bir modeli yayımlamak için sorgu dizesi parametresini sağlayın. HTTP isteğindeki bir JWT yetkilendirme üst bilgisine geçme hakkında daha fazla bilgi için, bkz. [Şirket modellerine erişirken bir güvenlik belirteci geçirme REST API](#passing-a-security-token-when-accessing-company-models-with-a-rest-api) .

CLı kullanarak bir model yayımlamak için bkz. Azure CLı [bir model yayımlama](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp/model?view=azure-cli-latest#ext-azure-iot-az-iot-pnp-model-publish) komutu.

### <a name="share-a-model"></a>Modeli paylaşma

Dış kuruluşların kullanıcılarıyla oluşturduğunuz şirket modellerini paylaşabilirsiniz. Bu şekilde, ortak çalışanların özel şirket modellerinizle çözümleri görüntülemesine ve geliştirmesine izin verebilirsiniz.

Örneğin, bir cihaz üreticisi şirket veya kuruluş için özel modelleri sürdürmek isteyebilir. Bunların cihaz yeteneklerinin gizli kalmasını gerektiren müşterileri olabilir.

Şirketlerin veya kuruluşların genelinde model paylaşımı, ortak olmayan modellere güvenli erişim sağlar.

Portalı kullanarak bir şirket modelini paylaşmak için:

- Bir modeli oluştursanız, modeli **Şirket modelleri** bölümünde görüntülediğinizde **Paylaşım** ve **paylaşılan** düğmeleri de etkindir.

    ![Model paylaşma](./media/concepts-model-repository/share-model.png)

- Modeli bir dış kullanıcıyla paylaşmak için, **paylaşma**' yı seçin. **Model paylaşma** bölmesinde, dış kullanıcının e-posta adresini girin ve **Kaydet**' i seçin.

- Modeli paylaştığınız kullanıcıları görmek için, **Ile paylaşılan**' ı seçin.

- Modeli belirli bir kullanıcıyla paylaşmayı durdurmak için, **paylaşılan** bölmesinde bulunan kullanıcılar listesinden kullanıcıyı seçin. Ardından **Kaldır** ' ı seçin ve sorulduğunda seçiminizi onaylayın.

    ![Paylaşmayı durdur](./media/concepts-model-repository/stop-sharing.png)

## <a name="additional-information"></a>Ek bilgiler

Azure AD ile çalışırken aşağıdaki konuları yararlı bulabilirsiniz:

- Yeni bir Azure AD kiracısı oluşturmak için bkz. [Azure AD 'de yeni kiracı oluşturma](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant). Çoğu kuruluş zaten Azure AD kiracılarına sahip olur.

- Bir Azure AD kiracısına Kullanıcı veya Konuk kullanıcıları eklemek için bkz. [Azure ad kullanarak Kullanıcı ekleme veya silme](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory).

- Bir Azure AD kiracısına hizmet sorumlusu eklemek için, [kaynaklara erişebilen bir Azure AD uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)konusuna bakın.

- Azure AD 'den REST API 'Leri çağırırken kullanılacak JWT belirtecini alma hakkında bilgi edinmek için bkz. [bir istemci uygulamasından istekleri yetkilendirmek Için Azure AD 'den belirteç alma](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-app).

## <a name="next-steps"></a>Sonraki adımlar

Önerilen sonraki adım [ıot Tak ve kullan mimarisini](concepts-architecture.md)gözden geçirmeye yönelik olur.