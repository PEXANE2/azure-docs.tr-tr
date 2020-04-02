---
title: Portalda bir Azure AD uygulaması & hizmet sorumlusu oluşturma
titleSuffix: Microsoft identity platform
description: Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimine sahip kaynaklara erişimi yönetmek için hizmet sorumlusu & yeni bir Azure Active Directory uygulaması oluşturun.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: ryanwi
ms.reviewer: tomfitz
ms.custom: aaddev, seoapril2019, identityplatformtop40
ms.openlocfilehash: d1ee8e90d1d690315b2727a050e0383d7d28dc03
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546148"
---
# <a name="how-to-use-the-portal-to-create-an-azure-ad-application-and-service-principal-that-can-access-resources"></a>Nasıl olunur: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın

Bu makalede, rol tabanlı erişim denetimiile kullanılabilecek yeni bir Azure Etkin Dizin (Azure AD) uygulaması ve hizmet ilkesi nasıl oluşturulabileceğiniz gösterilmektedir. Kaynaklara erişmesi veya değiştirmesi gereken kodunuz olduğunda, uygulama için bir kimlik oluşturabilirsiniz. Bu kimlik, hizmet sorumlusu olarak bilinir. Daha sonra gerekli izinleri hizmet sorumlusuna atayabilirsiniz. Bu makalede, hizmet ilkesini oluşturmak için portalı nasıl kullanacağınızı gösterir. Uygulamanın yalnızca bir kuruluş içinde çalışması amaçlandığı tek kiracılı bir uygulamaya odaklanır. Genellikle kuruluşunuz içinde çalışan iş yeri uygulamaları için tek kiracılı uygulamalar kullanırsınız.

> [!IMPORTANT]
> Hizmet ilkesi oluşturmak yerine, uygulama kimliğiniz için Azure kaynakları için yönetilen kimlikler kullanmayı düşünün. Kodunuz yönetilen kimlikleri destekleyen bir hizmette çalışıyorsa ve Azure AD kimlik doğrulamasını destekleyen kaynaklara erişiyorsa, yönetilen kimlikler sizin için daha iyi bir seçenektir. Şu anda hangi hizmetlerin desteklediği de dahil olmak üzere Azure kaynaklarıiçin yönetilen kimlikler hakkında daha fazla bilgi edinmek için Azure [kaynakları için yönetilen kimlikler nedir?](../managed-identities-azure-resources/overview.md)

## <a name="create-an-azure-active-directory-application"></a>Azure Etkin Dizin uygulaması oluşturma

Doğruca kimliği yaratmaya başlayalım. Bir sorunla karşılaştıysanız, hesabınızın kimliği oluşturabilmesini sağlamak için [gerekli izinleri](#required-permissions) denetleyin.

1. [Azure portalı](https://portal.azure.com)üzerinden Azure Hesabınızda oturum açın.
1. **Azure Etkin Dizini'ni**seçin.
1. **Uygulama kayıtları**'nı seçin.
1. **Yeni kayıt**seçin.
1. Başvuruyu adlandırın. Uygulamayı kimlerin kullanabileceğini belirleyen desteklenen bir hesap türünü seçin. **Uri'yi Yönlendirme**altında, oluşturmak istediğiniz uygulama türü için **Web'i** seçin. Erişim belirtecinin gönderildiği URI'yi girin. [Yerel bir uygulama](../manage-apps/application-proxy-configure-native-client-application.md)için kimlik bilgileri oluşturamazsınız. Bu türü otomatik bir uygulama için kullanamazsınız. Değerleri ayarladıktan sonra **Kaydol'u**seçin.

   ![Başvurunuz için bir ad yazın](./media/howto-create-service-principal-portal/create-app.png)

Azure AD uygulamanızı ve hizmet anaparanızı oluşturdunuz.

## <a name="assign-a-role-to-the-application"></a>Uygulamaya rol atama

Aboneliğinizdeki kaynaklara erişmek için uygulamaya bir rol atamanız gerekir. Hangi rolün uygulama için doğru izinleri sunduğuna karar verin. Kullanılabilir roller hakkında bilgi edinmek için Bkz. [RBAC: Built in Roles](../../role-based-access-control/built-in-roles.md).

Kapsamı abonelik, kaynak grubu veya kaynak düzeyinde ayarlayabilirsiniz. İzinler daha düşük kapsam düzeylerine devralınır. Örneğin, bir kaynak grubu için Okuyucu rolüne bir uygulama eklemek, kaynak grubunu ve içerdiği kaynakları okuyabileceği anlamına gelir.

1. Azure portalında, uygulamayı atamak istediğiniz kapsam düzeyini seçin. Örneğin, abonelik kapsamında bir rol atamak için **Abonelikleri**arayın ve seçin veya **Ana** Sayfa'daki **Abonelikleri** seçin.

   ![Örneğin, abonelik kapsamında bir rol atama](./media/howto-create-service-principal-portal/select-subscription.png)

1. Uygulamayı atamak için belirli bir aboneliği seçin.

   ![Atama için abonelik seçin](./media/howto-create-service-principal-portal/select-one-subscription.png)

   Aradığınız aboneliği görmüyorsanız, **genel abonelik filtresini**seçin. Portal için istediğiniz aboneliğin seçildiğinden emin olun.

1. **Erişim denetimi (IAM) seçeneğini**belirleyin.
1. **Rol ataması ekle**’yi seçin.
1. Uygulamaya atamak istediğiniz rolü seçin. Örneğin, uygulamanın **örnekleri yeniden başlatma,** **başlatma** ve **durdurma** gibi eylemleri yürütmesine izin vermek için **Katılımcı** rolünü seçin.  [Kullanılabilir roller](../../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi Varsayılan olarak, Azure AD uygulamaları kullanılabilir seçeneklerde görüntülenmez. Başvurunuzu bulmak için, adı arayın ve seçin.

   ![Uygulamaya atamak için rolü seçin](./media/howto-create-service-principal-portal/select-role.png)

1. Rolü atamayı bitirmek için **Kaydet'i** seçin. Uygulamanızı bu kapsamda bir rolü olan kullanıcılar listesinde görürsünüz.

Hizmet müdürünüz ayarlandı. Komut dosyalarınızı veya uygulamalarınızı çalıştırmak için kullanmaya başlayabilirsiniz. Sonraki bölümde programlı oturum alırken gereken değerleri nasıl elde edilen gösterir.

## <a name="get-values-for-signing-in"></a>Oturum açma değerleri alın

Programlı olarak oturum alırken, kimlik doğrulama isteğinizle birlikte kiracı kimliğini geçirmeniz gerekir. Ayrıca, başvurunuz için kimlik ve kimlik doğrulama anahtarına da ihtiyacınız vardır. Bu değerleri almak için aşağıdaki adımları kullanın:

1. **Azure Etkin Dizini'ni**seçin.
1. Azure AD'deki **Uygulama kayıtlarından** uygulamanızı seçin.
1. Dizin (kiracı) kimliğini kopyalayın ve uygulama kodunuzda saklayın.

    ![Dizini (kiracı kimliği) kopyalayın ve uygulama kodunuzda saklayın](./media/howto-create-service-principal-portal/copy-tenant-id.png)

1. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın.

   ![Başvuru (istemci) kimliğini kopyalama](./media/howto-create-service-principal-portal/copy-app-id.png)

## <a name="certificates-and-secrets"></a>Sertifikalar ve sırlar
Daemon uygulamaları, Azure AD ile kimlik doğrulama yapmak için iki kimlik belgesi türü kullanabilir: sertifikalar ve uygulama sırları.  Sertifika kullanmanızı öneririz, ancak yeni bir uygulama sırrı da oluşturabilirsiniz.

### <a name="upload-a-certificate"></a>Sertifika yükleme

Varsa varolan bir sertifikayı kullanabilirsiniz.  İsteğe bağlı olarak, *yalnızca sınama amacıyla*kendi imzalı bir sertifika oluşturabilirsiniz. PowerShell'i açın ve bilgisayarınızdaki kullanıcı sertifikası deposunda kendi imzalı bir sertifika oluşturmak için aşağıdaki parametrelerle [Yeni İmzalı Sertifika](/powershell/module/pkiclient/new-selfsignedcertificate) çalıştırın: 

```powershell
$cert=New-SelfSignedCertificate -Subject "CN=DaemonConsoleCert" -CertStoreLocation "Cert:\CurrentUser\My"  -KeyExportPolicy Exportable -KeySpec Signature
```

Bu sertifikayı, Windows Denetim Masası'ndan erişilebilen [Kullanıcı Sertifikası](/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) MMC snap-in'i kullanarak bir dosyaya dışa aktarın.

1. **Başlat** menüsünden **Çalıştır'ı** seçin ve ardından **certmgr.msc'yi**girin.

   Geçerli kullanıcı için Sertifika Yöneticisi aracı görüntülenir.

1. Sertifikalarınızı görüntülemek **için, Sertifikalar altında - Sol** bölmedeki Geçerli Kullanıcı, **Kişisel** dizini genişletin.
1. Oluşturduğunuz sertifikaya sağ tıklayın, **Tüm görevler->Dışa Aktarma'yı**seçin.
1. Sertifika Dışa Aktarma sihirbazını izleyin.  Özel anahtarı dışa aktarmayın ve bir . CER dosyası.

Sertifikayı yüklemek için:

1. **Azure Etkin Dizini'ni**seçin.
1. Azure AD'deki **Uygulama kayıtlarından** uygulamanızı seçin.
1. **Sertifikalar & sırları**seçin.
1. **Yükle sertifikasını** seçin ve sertifikayı (varolan bir sertifika veya dışa aktarılan kendi imzasını taşıyan sertifika) seçin.

    ![Yükle sertifikasını seçin ve eklemek istediğiniz sertifikayı seçin](./media/howto-create-service-principal-portal/upload-cert.png)

1. **Ekle'yi**seçin.

Başvuru kayıt portalında sertifikayı başvurunuzla birlikte kaydettikten sonra, istemci başvuru kodunun sertifikayı kullanmasını sağlamanız gerekir.

### <a name="create-a-new-application-secret"></a>Yeni bir uygulama sırrı oluşturma

Sertifika kullanmamayı seçerseniz, yeni bir uygulama sırrı oluşturabilirsiniz.

1. **Sertifikalar & sırları**seçin.
1. **İstemci sırlarını**seçin -> Yeni istemci sırrı .
1. Sırrın açıklamasını ve bir süreyi sağlayın. Bittiğinde **Ekle'yi**seçin.

   İstemci sırrını kurtardıktan sonra istemci sırrının değeri görüntülenir. Anahtarı daha sonra alamayacağından bu değeri kopyalayın. Uygulama olarak oturum açacak uygulama kimliği ile anahtar değeri sağlayacaktır. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

   ![Bunu daha sonra alamadığınız için gizli değeri kopyalayın](./media/howto-create-service-principal-portal/copy-secret.png)

## <a name="configure-access-policies-on-resources"></a>Kaynaklardaki erişim ilkelerini yapılandırma
Uygulamanızın erişmesi gereken kaynaklarüzerinde ek izinleri yapılandırmanız gerekebileceğini unutmayın. Örneğin, uygulamanızın anahtarlara, sırlara veya sertifikalara erişimini sağlamak için [önemli bir kasanın erişim ilkelerini](/azure/key-vault/key-vault-secure-your-key-vault#data-plane-and-access-policies) de güncelleştirmeniz gerekir.  

1. Azure [portalında,](https://portal.azure.com)anahtar kasanıza gidin ve **Access ilkelerini**seçin.  
1. **Erişim ilkesi ekle'yi**seçin, ardından başvurunuzu vermek istediğiniz anahtar, gizli ve sertifika izinlerini seçin.  Daha önce oluşturduğunuz hizmet ilkesini seçin.
1. Erişim ilkesini eklemek için **Ekle'yi** seçin, ardından değişikliklerinizi gerçekleştirmek için **Kaydet'i** seçin.
    ![Erişim ilkesi ekleme](./media/howto-create-service-principal-portal/add-access-policy.png)

## <a name="required-permissions"></a>Gerekli izinler

Bir uygulamayı Azure AD kiracınıza kaydetmek ve uygulamaya Azure aboneliğinizde bir rol atamak için yeterli izine sahip olmanız gerekir.

### <a name="check-azure-ad-permissions"></a>Azure REKLAM izinlerini denetleme

1. **Azure Etkin Dizini'ni**seçin.
1. Rolünüze dikkat edin. **Kullanıcı** rolüne sahipseniz, yönetici olmayanların uygulamaları kaydedebilmesini sağlamalısınız.

   ![Rolünü bul. Kullanıcıysanız, yönetici olmayanların uygulamaları kaydedebilmesini sağlayın](./media/howto-create-service-principal-portal/view-user-info.png)

1. Sol bölmede Kullanıcı **ayarlarını**seçin.
1. Uygulama **kayıtları** ayarını kontrol edin. Bu değer yalnızca bir yönetici tarafından ayarlanabilir. **Evet**olarak ayarlanırsa, Azure AD kiracısındaki herhangi bir kullanıcı bir uygulamayı kaydedebilir.

Uygulama kayıtları ayarı **Hayır**olarak ayarlanmışsa, yalnızca yönetici rolü olan kullanıcılar bu tür uygulamaları kaydedebilir. Kullanılabilir yönetici rolleri ve Azure AD'de her role verilen belirli izinler hakkında bilgi edinmek için [kullanılabilir rolleri](../users-groups-roles/directory-assign-admin-roles.md#available-roles) ve [rol izinlerini](../users-groups-roles/directory-assign-admin-roles.md#role-permissions) görün. Hesabınıza Kullanıcı rolü atanmışsa, ancak uygulama kayıt ayarı yönetici kullanıcılarla sınırlıysa, yöneticinizden uygulama kayıtlarının tüm yönlerini oluşturup yönetebilecek yönetici rollerinden birini atamasını veya kullanıcıların uygulamaları kaydetmesini sağlamasını isteyin.

### <a name="check-azure-subscription-permissions"></a>Azure abonelik izinlerini denetleme

Azure aboneliğinizde, hesabınızın `Microsoft.Authorization/*/Write` bir AD uygulamasına rol atama erişimine sahip olması gerekir. Bu eylemin izni, [Sahip](../../role-based-access-control/built-in-roles.md#owner) rolüyle veya [Kullanıcı Erişimi Yöneticisi](../../role-based-access-control/built-in-roles.md#user-access-administrator) rolüyle verilir. Hesabınıza **Katılımcı** rolü atanmışsa, yeterli izniniz yoktur. Hizmet sorumlusuna bir rol atamaya çalışırken bir hata alırsınız.

Abonelik izinlerinizi kontrol etmek için:

1. **Abonelikleri**arayın ve seçin veya **Ana** Sayfa'daki **Abonelikleri** seçin.

   ![Ara](./media/howto-create-service-principal-portal/select-subscription.png)

1. Hizmet ilkesini oluşturmak istediğiniz aboneliği seçin.

   ![Atama için abonelik seçin](./media/howto-create-service-principal-portal/select-one-subscription.png)

   Aradığınız aboneliği görmüyorsanız, **genel abonelik filtresini**seçin. Portal için istediğiniz aboneliğin seçildiğinden emin olun.

1. **İzinlerimi**seçin. Ardından, **bu abonelik için tam erişim ayrıntılarını görüntülemek için buraya tıklayın'ı**seçin.

   ![Hizmet ilkesini oluşturmak istediğiniz aboneliği seçin](./media/howto-create-service-principal-portal/view-details.png)

1. Atanan rollerinizi görüntülemek ve bir AD uygulamasına rol atamak için yeterli izine sahip olup olmadığını belirlemek için **Rol atamalarında** **Görünüm'u** seçin. Değilse, abonelik yöneticinizden sizi Kullanıcı Erişim Yöneticisi rolüne eklemesini isteyin. Aşağıdaki resimde, kullanıcıya Sahip rolü atanır, bu da kullanıcının yeterli izinlere sahip olduğu anlamına gelir.

   ![Bu örnek, kullanıcıya Sahip rolünün atandığını gösterir](./media/howto-create-service-principal-portal/view-user-role.png)

## <a name="next-steps"></a>Sonraki adımlar

* Güvenlik ilkeleri belirtme hakkında bilgi edinmek için [Azure Rol Tabanlı Erişim Denetimi'ne](../../role-based-access-control/role-assignments-portal.md)bakın.  
* Kullanıcılara verilebilecek veya reddedilebilen kullanılabilir eylemlerin listesi için Azure [Kaynak Yöneticisi Kaynak Sağlayıcısı işlemlerine](../../role-based-access-control/resource-provider-operations.md)bakın.
