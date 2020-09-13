---
title: Logic Apps ile tümleştirme
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS 'e Logic Apps bağlama, özel bağlayıcı kullanma
author: baanders
ms.author: baanders
ms.date: 8/14/2020
ms.topic: how-to
ms.service: digital-twins
ms.reviewer: baanders
ms.openlocfilehash: 20959709854f8366cc067437fe86c245fcbc3ef0
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89401070"
---
# <a name="integrate-with-logic-apps-using-a-custom-connector"></a>Özel bağlayıcı kullanarak Logic Apps tümleştirme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) , uygulamalar ve hizmetler arasında iş akışlarını otomatikleştirmenize yardımcı olan bir bulut hizmetidir. Azure dijital TWINS API 'Lerine Logic Apps bağlayarak, Azure dijital TWINS ve bunların verileri etrafında bu tür otomatikleştirilmiş akışlar oluşturabilirsiniz.

Azure dijital TWINS 'in Şu anda Logic Apps için Sertifikalı (önceden oluşturulmuş) bir Bağlayıcısı yoktur. Bunun yerine, Azure Digital TWINS ile Logic Apps kullanmaya yönelik geçerli işlem, Logic Apps ile çalışmak üzere değiştirilmiş [özel bir Azure dijital TWINS Swagger](https://docs.microsoft.com/samples/azure-samples/digital-twins-custom-swaggers/azure-digital-twins-custom-swaggers/) kullanarak [**özel bir Logic Apps Bağlayıcısı**](../logic-apps/custom-connector-overview.md)oluşturmaktır.

Bu makalede, bir Azure dijital TWINS örneğine Logic Apps bağlamak için kullanılabilecek **özel bir bağlayıcı oluşturmak** için [Azure Portal](https://portal.azure.com) kullanacaksınız. Daha sonra bu bağlantıyı bir örnek senaryo için kullanan **bir mantıksal uygulama oluşturacaksınız** . Bu, bir Zamanlayıcı tarafından tetiklenen olayların Azure dijital TWINS örneğindeki bir ikizi otomatik olarak güncelleştirilmesini sağlayacaktır. 

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce ** [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun** .

[Azure Portal](https://portal.azure.com) bu hesapla oturum açın.

### <a name="set-up-azure-digital-twins-instance"></a>Azure dijital TWINS örneğini ayarlama

Bu makaledeki Logic Apps bir Azure dijital TWINS örneğini bağlamak için **Azure Digital TWINS örneğinin** zaten ayarlanmış olması gerekir. 

Şimdi yeni bir örnek ayarlamanız gerekiyorsa, bunu yapmanın en kolay yolu bir otomatik dağıtım betik örneği çalıştırmalıdır. Yeni bir örnek ve gerekli Azure AD uygulama kaydını ayarlamak için [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama (komut dosyalı)*](how-to-set-up-instance-scripted.md) konusundaki yönergeleri izleyin. Yönergeler Ayrıca, her adımı başarıyla tamamladığınızı ve yeni örneğinizi kullanarak üzerine geçmeye başlamaya yönelik adımları da içerir.

Bu öğreticide, örneğinizi ayarlarken aşağıdaki değerlere ihtiyaç duyarsınız. Bu değerleri yeniden toplamanız gerekiyorsa, [Azure Portal](https://portal.azure.com)için kurulum makalesindeki ilgili bölümlere aşağıdaki bağlantıları kullanın.
* Azure Digital TWINS örnek **_ana bilgisayar adı_** ([portalda bul](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values))
* Azure AD uygulama kayıt **_uygulaması (istemci) kimliği_** ([portalda bul](how-to-set-up-instance-portal.md#collect-important-values))
* Azure AD uygulama kayıt **_dizini (kiracı) kimliği_** ([portalda bul](how-to-set-up-instance-portal.md#collect-important-values))

### <a name="get-app-registration-client-secret"></a>Uygulama kaydı istemci gizliliğini al

Ayrıca, Azure AD uygulama kaydınız için bir **_istemci gizli anahtarı_** oluşturmanız gerekir. Bunu yapmak için, Azure portal [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) sayfasına gidin (Bu bağlantıyı kullanabilir veya Portal arama çubuğunda bulabilirsiniz). Ayrıntılarını açmak için listeden kaydınızı seçin. 

Kayıt menüsündeki *Sertifikalar ve gizli* dizileri vurun ve *+ yeni istemci parolası*' nı seçin.

:::image type="content" source="media/how-to-integrate-logic-apps/client-secret.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Açıklama ve süre sonu için istediğiniz değerleri girin ve *Ekle*'ye basın.
Gizli dizi, *Sertifikalar ve gizlilikler* sayfasında istemci gizli dizileri listesine eklenir. Daha sonra kullanmak için değerini (kopyalama simgesiyle birlikte Pano 'ya de kopyalayabilirsiniz) göz önünde bulabilirsiniz.

### <a name="add-a-digital-twin"></a>Dijital ikizi ekleme

Bu makalede, Azure dijital TWINS örneğiniz içindeki bir ikizi güncelleştirmek için Logic Apps kullanılır. Devam etmek için, örneğinize en az bir ikizi eklemeniz gerekir. 

[Digitaltwıns API 'lerini](how-to-use-apis-sdks.md), [.net (C#) SDK 'Sını](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)veya [Azure dijital TWINS CLI](how-to-use-cli.md)'yi kullanarak TWINS ekleyebilirsiniz. Bu yöntemleri kullanarak TWINS oluşturma hakkında ayrıntılı adımlar için bkz. [*nasıl yapılır: dijital TWINS 'ı yönetme*](how-to-manage-twin.md).

Örneğiniz içinde oluşturduğunuz bir ikizi **_IKIZI ID_** 'ye ihtiyacınız olacaktır.

## <a name="create-custom-logic-apps-connector"></a>Özel Logic Apps Bağlayıcısı oluştur

Bu adımda, Azure dijital TWINS API 'Leri için [özel bir Logic Apps Bağlayıcısı](../logic-apps/custom-connector-overview.md) oluşturacaksınız. Bunu yaptıktan sonra, bir sonraki bölümde bir mantıksal uygulama oluştururken Azure dijital TWINS 'i yedeklenebilir.

Azure portal [Logic Apps özel bağlayıcı](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2FcustomApis) sayfasına gidin (Bu bağlantıyı kullanabilir veya Portal arama çubuğunda arama yapabilirsiniz). İsabet *+ Ekle*.

:::image type="content" source="media/how-to-integrate-logic-apps/logic-apps-custom-connector.png" alt-text="Azure portal ' Logic Apps özel bağlayıcı ' sayfası. ' Ekle ' düğmesinin etrafında Vurgula":::

Aşağıdaki *Logic Apps özel bağlayıcı oluştur* sayfasında, aboneliğiniz ve kaynak grubunuz ' ı ve yeni Bağlayıcınız için bir ad ve dağıtım konumunu seçin. *İnceleme ve oluşturma*düğmesine basın. Bu işlem sizi, kaynağı oluşturmak için en altta *Oluştur* ' u vuran *İnceleme + oluştur* sekmesine götürür.

:::image type="content" source="media/how-to-integrate-logic-apps/create-logic-apps-custom-connector.png" alt-text="Azure portal ' Logic Apps özel bağlayıcı oluşturma ' sayfasının ' gözden geçir + oluştur ' sekmesi. ' Oluştur ' düğmesinin etrafında Vurgula":::

Bağlayıcının dağıtım sayfasına yönlendirilirsiniz. Dağıtım tamamlandığında, bağlayıcının portalda ayrıntılarını görüntülemek için *Kaynağa Git* düğmesine basın.

### <a name="configure-connector-for-azure-digital-twins"></a>Azure dijital TWINS için bağlayıcı yapılandırma

Daha sonra, oluşturduğunuz bağlayıcıyı Azure dijital TWINS 'e ulaşmak için yapılandıracaksınız.

İlk olarak, Logic Apps çalışmak üzere değiştirilmiş olan özel bir Azure dijital TWINS Swagger 'yi indirin. *Posta indirme* düğmesine basarak **Azure Digital Twins özel Swaggers** örneğini [Bu bağlantıdan](https://docs.microsoft.com/samples/azure-samples/digital-twins-custom-swaggers/azure-digital-twins-custom-swaggers/) indirin. İndirilen *Azure_Digital_Twins_Custom_Swaggers.zip* klasörüne gidin ve sıkıştırmayı açın. Bu öğretici için özel Swagger, * üzerindeAzure_Digital_Twins_Custom_Swaggers\LogicApps\preview\2020-05-31-preview\digitaltwins.js*bulunmaktadır.

Ardından, [Azure Portal](https://portal.azure.com) bağlayıcının genel bakış sayfasına gidin ve *Düzenle*'ye basın.

:::image type="content" source="media/how-to-integrate-logic-apps/edit-connector.png" alt-text="Önceki adımda oluşturulan bağlayıcının ' genel bakış ' sayfası. ' Düzenle ' düğmesinin etrafında Vurgula":::

Aşağıdaki *Logic Apps özel bağlayıcıyı Düzenle* sayfasında, bu bilgileri yapılandırın:
* **Özel bağlayıcılar**
    - API uç noktası: REST (varsayılan olarak bırakın)
    - İçeri aktarma modu: Openapı dosyası (varsayılanı bırak)
    - Dosya: Bu, daha önce indirdiğiniz özel Swagger dosyası olacaktır. *Içeri aktar*' a basın, makinenizde dosyayı bulun (*Azure_Digital_Twins_Custom_Swaggers\LogicApps\preview\2020-05-31-preview\digitaltwins.js*) ve *Aç*' a basın.
* **Genel bilgiler**
    - Simge, simge arka plan rengi, Açıklama: istediğiniz değerleri girin.
    - Düzen: HTTPS (varsayılan olarak bırakın)
    - Ana bilgisayar: Azure dijital TWINS örneğinizin *konak adı* .
    - Temel URL:/(varsayılanı bırak)

Ardından, bir sonraki yapılandırma adımına devam etmek için pencerenin alt kısmındaki *güvenlik* düğmesine basın.

:::image type="content" source="media/how-to-integrate-logic-apps/configure-next.png" alt-text="' Logic Apps özel bağlayıcısını Düzenle ' sayfasının en alt ekran görüntüsü. Güvenliğe devam etmek için etrafında vurgu düğmesi":::

Güvenlik adımında, bu bilgileri *Düzenle* ve Yapılandır ' a basın.
* **Kimlik doğrulama türü**: OAuth 2,0
* **OAuth 2,0**:
    - Kimlik sağlayıcısı: Azure Active Directory
    - İstemci KIMLIĞI: Azure AD uygulama kaydınız için *uygulama (istemci) kimliği*
    - İstemci gizli dizisi: Azure AD uygulama kaydınız için [*Önkoşullar*](#prerequisites) bölümünde oluşturduğunuz *istemci gizli* dizisi
    - Oturum açma URL 'SI: https://login.windows.net (varsayılanı bırak)
    - Kiracı KIMLIĞI: Azure AD uygulama kaydınız için *Dizin (kiracı) kimliği*
    - Kaynak URL 'SI: 0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    - Kapsam: Directory. AccessAsUser. All
    - Yeniden yönlendirme URL 'SI: (şimdilik varsayılan olarak bırakın)

Yeniden yönlendirme URL 'SI alanının, *YÖNLENDIRME URL 'sini oluşturmak için özel bağlayıcıyı kaydetdüğüne*emin olduğunu unutmayın. Bunu şimdi, bağlayıcı ayarlarınızı onaylamak için bölmenin üst kısmında bulunan *güncelleştirme bağlayıcısını* vurarak yapın.

:::image type="content" source="media/how-to-integrate-logic-apps/update-connector.png" alt-text="' Logic Apps özel bağlayıcısını Düzenle ' sayfasının en üstündeki ekran görüntüsü. ' Güncelleştirme Bağlayıcısı ' düğmesinin etrafında Vurgula":::

<!-- Success message? didn't see one -->

Yeniden yönlendirme URL 'SI alanına dönün ve oluşturulan değeri kopyalayın. Bunu bir sonraki adımda kullanacaksınız.

:::image type="content" source="media/how-to-integrate-logic-apps/copy-redirect-url.png" alt-text="' Logic Apps özel bağlayıcısını Düzenle ' sayfasındaki yeniden yönlendirme URL 'SI alanı artık ' ' değerine sahip https://logic-apis-westus2.consent.azure-apim.net/redirect . Değeri kopyalama düğmesi vurgulanır.":::

Bu, bağlayıcınızı oluşturmak için gereken tüm bilgiler (tanım adımına geçmiş güvenlik 'e devam etmeniz gerekmez). *Düzenle Logic Apps özel bağlayıcı* bölmesini kapatabilirsiniz.

>[!NOTE]
>İlk olarak *düzenlemeye*vurduysanız bağlayıcının genel bakış sayfasına geri dönmek için, yeniden *Düzenle* ' nin, yapılandırma seçimlerinizi girme işleminin tamamını yeniden başlatdığına göz atın. Bu, sizin tarafınızdan son yaptığınız zamandan itibaren doldurmayacak, bu nedenle güncelleştirilmiş bir yapılandırmayı değiştirilen değerlerle kaydetmek istiyorsanız, diğer tüm değerleri yeniden girmeniz ve bunların üzerine yazılmaları önlenir.

### <a name="grant-connector-permissions-in-the-azure-ad-app"></a>Azure AD uygulamasında bağlayıcı izinleri verme

Daha sonra, Azure AD uygulama kaydınıza bağlayıcı izinleri vermek için son adımda kopyaladığınız özel bağlayıcının *yeniden YÖNLENDIRME URL* değerini kullanın.

Azure portal [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) sayfasına gidin ve listeden kaydınızı seçin. 

Kayıt menüsündeki *kimlik doğrulaması* altında bir URI ekleyin.

:::image type="content" source="media/how-to-integrate-logic-apps/add-uri.png" alt-text="Azure portal uygulama kaydı için kimlik doğrulama sayfası. Menüdeki ' Authentication ' vurgulanmıştır ve sayfada ' URI Ekle ' düğmesi vurgulanır."::: 

Özel bağlayıcının *yeniden YÖNLENDIRME URL* 'sini yeni alana girin ve *Kaydet* simgesine basın.

:::image type="content" source="media/how-to-integrate-logic-apps/save-uri.png" alt-text="Azure portal uygulama kaydı için kimlik doğrulama sayfası. Yeni yeniden yönlendirme URL 'SI vurgulanır ve sayfanın ' Kaydet ' düğmesi.":::

Artık Azure dijital TWINS API 'Lerine erişebilen bir özel bağlayıcı ayarlamayı tamamladınız. 

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

Ardından, Azure dijital TWINS güncelleştirmelerini otomatikleştirmek için yeni bağlayıcınızı kullanacak bir mantıksal uygulama oluşturacaksınız.

Azure portal [Logic Apps](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Logic%2Fworkflows) sayfasına gidin (Bu bağlantıyı kullanabilir veya Portal arama çubuğunda bulabilirsiniz). *Mantıksal uygulama oluştur ' a*basın.

:::image type="content" source="media/how-to-integrate-logic-apps/create-logic-app.png" alt-text="Azure portal ' Logic Apps ' sayfası. ' Mantıksal uygulama oluştur ' düğmesinin etrafında Vurgula":::

Aşağıdaki *mantıksal uygulama* sayfasında, aboneliğiniz ve kaynak grubunuz ' ı ve yeni mantıksal uygulamanız için bir ad ve dağıtım konumunu seçin. *İnceleme ve oluşturma*düğmesine basın. Bu işlem sizi, kaynağı oluşturmak için en altta *Oluştur* ' u vuran *İnceleme + oluştur* sekmesine götürür.

Mantıksal uygulama için dağıtım sayfasına yönlendirilirsiniz. Dağıtım tamamlandığında, *Logic Apps tasarımcısına*devam etmek Için *Kaynağa Git* düğmesine basın. Bu, iş akışının mantığını dolduracaktır.

### <a name="design-workflow"></a>Tasarım iş akışı

*Logic Apps tasarımcısında*, *ortak bir tetikleyiciden başla*' nın altında _**yinelenme**_' yi seçin.

:::image type="content" source="media/how-to-integrate-logic-apps/logic-apps-designer-recurrence.png" alt-text="Azure portal ' Logic Apps Designer ' sayfası. ' Yinelenme ' ortak tetikleyicisinin etrafında Vurgula":::

Aşağıdaki *Logic Apps tasarımcı* sayfasında **yinelenme** sıklığını *ikinci*olarak değiştirin, böylece olay 3 saniyede bir tetiklenir. Bu, daha sonra çok uzun süre beklemek zorunda kalmadan sonuçları görmeyi kolaylaştırır.

*Yeni adım*' a basın.

Bu *işlem bir eylem Seç* kutusunu açar. *Özel* sekmeye geçiş yapın. Özel bağlayıcınızı daha önce üstteki kutuda görmeniz gerekir.

:::image type="content" source="media/how-to-integrate-logic-apps/custom-action.png" alt-text="Azure portal Logic Apps tasarımcısında akış oluşturma. ' Eylem Seç ' kutusunda ' özel ' sekmesi seçilidir. Kullanıcının daha önce özel Bağlayıcısı, etrafında vurgulanacak şekilde kutusunda gösterilir.":::

Bu bağlayıcının içerdiği API 'lerin listesini göstermek için bunu seçin. **DigitalTwins_Add**seçmek için arama çubuğunu kullanın veya listede ilerleyin. (Bu makalede kullanılan API budur, ancak aynı zamanda başka bir API 'YI Logic Apps bağlantı için geçerli bir seçenek olarak seçebilirsiniz).

Bağlayıcıya bağlanmak için Azure kimlik bilgilerinizle oturum açmanız istenebilir. *Istenen izinlere* sahip bir iletişim kutusu alırsanız, uygulamanıza izin vermek ve kabul etmek için istemleri izleyin.

Yeni *DigitalTwinsAdd* kutusunda, alanları aşağıdaki gibi girin:
* Kimlik: örnekte mantıksal uygulamanın güncelleştirilmesini istediğiniz dijital ikizi 'ın *IKIZI kimliğini* doldurursunuz.
* Öğe-1: Bu alan, seçilen API isteğinin gerektirdiği gövdeye girebileceğiniz yerdir. *Digitaltwınsupdate*için bu gövde JSON Patch kodu biçimindedir. İkizi 'nizi güncelleştirmek için bir JSON Patch yapılandırma hakkında daha fazla bilgi için *nasıl yapılır: dijital TWINS yönetme*konusunun [dijital ikizi güncelleştirme](how-to-manage-twin.md#update-a-digital-twin) bölümüne bakın.
* api sürümü: geçerli genel önizlemede, bu değer *2020-05-31-Preview* ' dir

Logic Apps tasarımcısında *Kaydet* ' i ziyaret edin.

:::image type="content" source="media/how-to-integrate-logic-apps/save-logic-app.png" alt-text="Mantıksal uygulama bağlayıcısı 'nda uygulamanın tamamlanmış görünümü. DigitalTwinsAdd kutusu, örnek bir JSON yama gövdesi dahil olmak üzere yukarıda açıklanan değerlerle doldurulur. Pencerenin ' Kaydet ' düğmesi vurgulanır.":::

## <a name="query-twin-to-see-the-update"></a>Güncelleştirmeyi görmek için ikizi sorgulama

Mantıksal uygulamanız oluşturulduktan sonra, Logic Apps tasarımcısında tanımladığınız ikizi Update olayı her üç saniyede bir tekrarda gerçekleşmelidir. Bu, üç saniyeden sonra, ikizi sorgunuzu sorgulayabilmeniz ve yeni düzeltme eki uygulanmış değerlerinizin yansıtıldığını görmeniz gerekir.

İkizi 'nizi tercih ettiğiniz Yöntem (örneğin, [özel bir istemci uygulaması](tutorial-command-line-app.md), [Azure Digital TWINS Explorer örnek uygulaması](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/), [SDK 'Lar ve API 'ler](how-to-use-apis-sdks.md)veya [CLI](how-to-use-cli.md)) aracılığıyla sorgulayabilirsiniz. 

Azure dijital TWINS örneğinizi sorgulama hakkında daha fazla bilgi için bkz. [*nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure dijital TWINS örneğiniz içindeki bir ikizi, verdiğiniz bir düzeltme ekiyle düzenli olarak güncelleştiren bir mantıksal uygulama oluşturdunuz. Örneğiniz üzerinde çeşitli eylemler için Logic Apps oluşturmak üzere özel bağlayıcıdaki diğer API 'Leri seçmeyi deneyebilirsiniz.

Kullanılabilir API işlemleri ve ihtiyaç duydukları ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [*nasıl yapılır: Azure Digital TWINS API 'leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md).