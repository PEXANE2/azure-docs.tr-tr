---
title: Azure 'da verileri işlemek için bir işlev ayarlama
titleSuffix: Azure Digital Twins
description: Azure 'da dijital TWINS 'e erişebilen ve tetiklenebilecek bir işlev oluşturma bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 8/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: f1ed4b9beda9848bba8fb12783f49dcf8016d3dd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104590628"
---
# <a name="connect-function-apps-in-azure-for-processing-data"></a>Verileri işlemek için Azure 'da işlev uygulamalarına bağlanma

Verileri temel alarak dijital TWINS 'nin güncelleştirilmesi, [Azure işlevleri](../azure-functions/functions-overview.md)kullanılarak yapılan bir işlev gibi işlem kaynakları aracılığıyla [**olay rotaları**](concepts-route-events.md) kullanılarak işlenir. İşlevleri, yanıt olarak bir dijital ikizi güncelleştirmek için kullanılabilir:
* IoT Hub gelen cihaz telemetri verileri
* ikizi grafiğinin içindeki başka bir dijital ikizi gelen özellik değişikliği veya diğer veriler

Bu makalede, Azure 'da Azure dijital TWINS ile kullanım için bir işlev oluşturma işlemi adım adım açıklanmaktadır. 

İçerdiği adımlara genel bir bakış aşağıda verilmiştir:

1. Visual Studio'da bir Azure İşlevleri projesi oluşturma
2. [Event Grid](../event-grid/overview.md) tetikleyicisiyle bir işlev yazma
3. İşleve kimlik doğrulama kodu ekleme (Azure dijital TWINS 'ye erişebilmek için)
4. İşlev uygulamasını Azure'da yayımlama
5. İşlev uygulaması için [güvenlik](concepts-security.md) erişimini ayarlama

## <a name="prerequisite-set-up-azure-digital-twins-instance"></a>Önkoşul: Azure dijital TWINS örneğini ayarlama

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

## <a name="create-a-function-app-in-visual-studio"></a>Visual Studio 'da işlev uygulaması oluşturma

Visual Studio 2019 ' de _dosya > yeni > proje_ ' yi seçin ve _Azure işlevleri_ şablonunu arayın. _İleri_’yi seçin.

:::image type="content" source="media/how-to-create-azure-function/create-azure-function-project.png" alt-text="Yeni proje iletişim kutusunu gösteren Visual Studio ekran görüntüsü. Azure Işlevleri proje şablonu vurgulanır.":::

İşlev uygulaması için bir ad belirtin ve _Oluştur_' u seçin.

:::image type="content" source="media/how-to-create-azure-function/configure-new-project.png" alt-text="Proje adı, konum Kaydet, yeni çözüm oluşturma seçimi ve çözüm adı dahil olmak üzere yeni bir proje yapılandırma iletişim kutusunu gösteren Visual Studio ekran görüntüsü.":::

*Event Grid tetikleyicisi* işlev uygulaması türünü seçin ve _Oluştur_' u seçin.

:::image type="content" source="media/how-to-create-azure-function/event-grid-trigger-function.png" alt-text="Yeni bir Azure Işlevleri uygulaması oluşturmak için iletişim kutusunu gösteren Visual Studio ekran görüntüsü. Event Grid tetikleyici seçeneği vurgulanır.":::

İşlev uygulamanız oluşturulduktan sonra Visual Studio, proje klasörünüzdeki bir **işlev1. cs** dosyasında bir kod örneği oluşturur. Bu kısa işlev olayları günlüğe kaydetmek için kullanılır.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-sample-code.png" alt-text="Oluşturulan yeni proje için proje penceresinde Visual Studio 'nun ekran görüntüsü. Işlev1 adlı örnek bir işlev için kod vardır." lightbox="media/how-to-create-azure-function/visual-studio-sample-code.png":::

## <a name="write-a-function-with-an-event-grid-trigger"></a>Event Grid tetikleyicisiyle bir işlev yazma

İşlev uygulamanıza SDK ekleyerek bir işlev yazabilirsiniz. İşlev uygulaması, [.net Için Azure dijital TWINS SDK 'sını (C#)](/dotnet/api/overview/azure/digitaltwins/client)kullanarak Azure Digital TWINS ile etkileşime girer. 

SDK 'yı kullanmak için aşağıdaki paketleri projenize eklemeniz gerekir. Paketleri Visual Studio 'nun NuGet Paket Yöneticisi 'ni kullanarak yükleyebilir veya `dotnet` bir komut satırı aracında kullanarak paketleri ekleyebilirsiniz.

* [Azure. DigitalTwins. Core](https://www.nuget.org/packages/Azure.DigitalTwins.Core/)
* [Azure. Identity](https://www.nuget.org/packages/Azure.Identity/)
* [System .net. http](https://www.nuget.org/packages/System.Net.Http/)
* [Azure. Core](https://www.nuget.org/packages/Azure.Core/)

Ardından, Visual Studio Çözüm Gezgini, örnek kodunuzun bulunduğu _işlev1. cs_ dosyasını açın ve `using` Bu paketler için aşağıdaki deyimlerini işlevlerinize ekleyin.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="Function_dependencies":::

## <a name="add-authentication-code-to-the-function"></a>İşleve kimlik doğrulama kodu ekleyin

Artık sınıf düzeyi değişkenleri bildirecektir ve işlevin Azure dijital TWINS 'e erişmesine izin verecek kimlik doğrulama kodu eklersiniz. _İşlev1. cs_ dosyasındaki işlevinizi aşağıda bulabilirsiniz.

* Bir **ortam değişkeni** olarak Azure Digital TWINS hizmet URL 'sini okumak için kod. Hizmet URL 'sini, işlevinde sabit kodlamak yerine, bir ortam değişkeninden okumak iyi bir uygulamadır. Bu ortam değişkeninin değerini [daha sonra bu makalede](#set-up-security-access-for-the-function-app)ayarlayacaksınız. Ortam değişkenleri hakkında daha fazla bilgi için bkz. [*işlev uygulamanızı yönetme*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal).

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ADT_service_URL":::

* HttpClient örneğini tutacak statik bir değişken. HttpClient, oluşturmak için nispeten pahalıdır ve her işlev çağrısı için bunu yapmak zorunda kalmamak istiyoruz.

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="HTTP_client":::

* Azure Işlevleri 'nde yönetilen kimlik kimlik bilgilerini kullanabilirsiniz.
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ManagedIdentityCredential":::

* Azure dijital TWINS istemci örneğinizi tutmak için işlevinizin içine _DigitalTwinsClient_ yerel bir değişken ekleyin. Bu değişkeni sınıfınız içinde *statik yapmayın.*
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="DigitalTwinsClient":::

* Özel durumları yakalamak için bir try/catch bloğunda _adtInstanceUrl_ için null denetimi ekleyin ve işlev mantığınızı sarın.

Bu değişikliklerden sonra, işlev kodunuz aşağıdakine benzer olacaktır:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs":::

Uygulamanız yazıldığına göre, sonraki bölümde yer alan adımları kullanarak Azure 'da yayımlayabilirsiniz.

## <a name="publish-the-function-app-to-azure"></a>İşlev uygulamasını Azure'da yayımlama

[!INCLUDE [digital-twins-publish-azure-function.md](../../includes/digital-twins-publish-azure-function.md)]

### <a name="verify-function-publish"></a>İşlev yayımlamayı doğrula

1. [Azure Portal](https://portal.azure.com/)kimlik bilgilerinizle oturum açın.
2. Pencerenin üst kısmındaki arama çubuğunda, **işlev uygulamanızın adını** arayın.

    :::image type="content" source="media/how-to-create-azure-function/search-function-app.png" alt-text="Azure portal, işlev uygulamanızı adıyla arama yapın." lightbox="media/how-to-create-azure-function/search-function-app.png":::

3. Açılan *işlev uygulaması* sayfasında, sol taraftaki menü seçeneklerinde *işlevler* ' i seçin. İşleviniz başarıyla yayımlanıyorsa, işlevinizin adını listede görürsünüz.
İşlevinizi yayımlanmış işlevler listesinde görebilmeniz için birkaç dakika beklemeniz veya sayfayı birkaç kez yenilemeniz gerekebileceğini unutmayın.

    :::image type="content" source="media/how-to-create-azure-function/view-published-functions.png" alt-text="Azure portal yayınlanan işlevleri görüntüleyin." lightbox="media/how-to-create-azure-function/view-published-functions.png":::

İşlev uygulamanızın Azure dijital TWINS 'e erişebilmesi için, Azure dijital TWINS örneğinizi erişim izinleri olan sistem tarafından yönetilen bir kimliğe sahip olması gerekir. Bunu daha sonra ayarlayacaksınız.

## <a name="set-up-security-access-for-the-function-app"></a>İşlev uygulaması için güvenlik erişimini ayarlama

Azure CLı veya Azure portal kullanarak işlev uygulaması için güvenlik erişimi ayarlayabilirsiniz. Aşağıdaki tercih ettiğiniz seçeneğe yönelik adımları izleyin.

# <a name="cli"></a>[CLI](#tab/cli)

Bu komutları, [Azure Cloud Shell](https://shell.azure.com) veya [yerel bir Azure CLI yüklemesinde](/cli/azure/install-azure-cli)çalıştırabilirsiniz.
Azure dijital TWINS örneğiniz için _**Azure dijital TWINS veri sahibi**_ rolünü sağlamak üzere, uygulamanın sistem tarafından yönetilen kimliğini kullanabilirsiniz. Bu, veri düzlemi etkinliklerini gerçekleştirmek için örnekte işlev uygulamasına izin verir. Daha sonra, bir ortam değişkeni ayarlayarak Azure dijital TWINS örneğinin URL 'sini işlevinizle erişilebilir yapın.

### <a name="assign-access-role"></a>Erişim rolü ata

[!INCLUDE [digital-twins-permissions-required.md](../../includes/digital-twins-permissions-required.md)]

Önceki örneklerden iskelet işlevi, Azure Digital TWINS ile kimlik doğrulaması yapabilmek için bir taşıyıcı belirtecinin kendisine geçirilmesini gerektirir. Bu taşıyıcı belirtecin geçirildiğinden emin olmak için, işlev uygulaması için Azure dijital TWINS 'e erişmek üzere [yönetilen hizmet kimliği (MSI)](../active-directory/managed-identities-azure-resources/overview.md) izinleri ayarlamanız gerekir. Bu, her bir işlev uygulaması için yalnızca bir kez yapılmalıdır.


1. İşlev için sistem tarafından yönetilen kimliğin ayrıntılarını görmek için aşağıdaki komutu kullanın. Çıktıda _PrincipalId_ alanını bir yere göz atın.

    ```azurecli-interactive 
    az functionapp identity show -g <your-resource-group> -n <your-App-Service-(function-app)-name> 
    ```

    >[!NOTE]
    > Bir kimliğin ayrıntılarını göstermek yerine sonuç boşsa, bu komutu kullanarak işlev için yeni bir sistem tarafından yönetilen kimlik oluşturun:
    > 
    >```azurecli-interactive    
    >az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>  
    >```
    >
    > Daha sonra çıktı, sonraki adım için gereken _PrincipalId_ değeri de dahil olmak üzere kimliğin ayrıntılarını görüntüler. 

1. _principalId_ değerini aşağıdaki komutta kullanarak işlev uygulamasının kimliğini Azure Digital Twins örneğinizin _Azure Digital Twins Veri Sahibi_ rolüne atayın.

    ```azurecli-interactive 
    az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Data Owner"
    ```

### <a name="configure-application-settings"></a>Uygulama ayarlarını yapılandırma

Son olarak, Azure dijital TWINS örneğinizin URL 'sini, bir **ortam değişkenini** ayarlayarak işlevinizin erişimine açık hale getirin. Ortam değişkenleri hakkında daha fazla bilgi için bkz. [*işlev uygulamanızı yönetme*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). 

> [!TIP]
> Azure Digital TWINS örneğinin URL 'SI, Azure Digital TWINS örneğinizin *ana bilgisayar adının* başlangıcına *https://* eklenerek yapılır. Ana bilgisayar adını, örneğinizin tüm özellikleriyle birlikte görmek için çalıştırabilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

```azurecli-interactive 
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-host-name>"
```

# <a name="azure-portal"></a>[Azure portalı](#tab/portal)

[Azure Portal](https://portal.azure.com/)aşağıdaki adımları izleyin.

### <a name="assign-access-role"></a>Erişim rolü ata

[!INCLUDE [digital-twins-permissions-required.md](../../includes/digital-twins-permissions-required.md)]

Bir sistem tarafından atanmış yönetilen kimlik, Azure kaynaklarının kimlik bilgilerini kodda depolamadan (örneğin, Azure Key Vault) kimlik doğrulaması yapmasına olanak sağlar. Etkinleştirildikten sonra, tüm gerekli izinler Azure rol tabanlı erişim denetimi aracılığıyla verilebilir. Bu tür yönetilen kimliğin yaşam döngüsü, bu kaynağın yaşam döngüsüne bağlıdır. Ayrıca, her kaynağa yalnızca bir sistem tarafından atanmış yönetilen kimlik olabilir.

1. [Azure Portal](https://portal.azure.com/), arama çubuğuna adını yazarak işlev uygulamanızı arayın. Sonuçlardan uygulamanızı seçin. 

    :::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Azure portal ekran görüntüsü: işlev uygulamasının adı, Portal arama çubuğunda aranır ve arama sonucu vurgulanır.":::

1. İşlev uygulaması sayfasında, işlevin yönetilen kimliğiyle çalışmak için sol taraftaki gezinti çubuğundaki _kimlik_ ' i seçin. _Sistem atandı_ sayfasında, _durumun_ **Açık** olarak ayarlandığını doğrulayın (yoksa, şimdi ayarlayın ve değişikliği *kaydedin* ).

    :::image type="content" source="media/how-to-create-azure-function/verify-system-managed-identity.png" alt-text="Azure portal ekran görüntüsü: işlev uygulamasının kimlik sayfasında, durum seçeneği açık olarak ayarlanır." lightbox="media/how-to-create-azure-function/verify-system-managed-identity.png":::

1. Azure *rol atamaları sayfasını açmak* için _Azure rol atamaları_ düğmesini seçin.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-1.png" alt-text="Azure portal ekran görüntüsü: Azure Işlevinin kimlik sayfasındaki Izinler altında Azure rol atamaları düğmesinin etrafında vurgu." lightbox="media/how-to-create-azure-function/add-role-assignment-1.png":::

    _+ Rol ataması Ekle (Önizleme)_ seçeneğini belirleyin.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-2.png" alt-text="Azure portal ekran görüntüsü: Azure rol atamaları sayfasında, bir vurgu + rol ataması Ekle (Önizleme)." lightbox="media/how-to-create-azure-function/add-role-assignment-2.png":::

1. Açılan _rol ataması Ekle (Önizleme)_ sayfasında aşağıdaki değerleri seçin:

    * **Kapsam**: Kaynak grubu
    * **Abonelik**: Azure aboneliğinizi seçin
    * **Kaynak grubu**: açılan listeden kaynak grubunuzu seçin
    * **Rol**: açılan listeden _Azure dijital TWINS veri sahibini_ seçin

    Sonra, _Kaydet_ düğmesine basarak ayrıntılarınızı kaydedin.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-3.png" alt-text="Yeni bir rol ataması (Önizleme) eklemek için Azure portal: Iletişim kutusunun ekran görüntüsü. Kapsam, abonelik, kaynak grubu ve rol için alanlar vardır.":::

### <a name="configure-application-settings"></a>Uygulama ayarlarını yapılandırma

Azure dijital TWINS örneğinizin URL 'sini işlevinizin erişimine açık hale getirmek için bir **ortam değişkeni** ayarlayabilirsiniz. Ortam değişkenleri hakkında daha fazla bilgi için bkz. [*işlev uygulamanızı yönetme*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). Uygulama ayarları, Azure dijital TWINS örneğine erişmek için ortam değişkenleri olarak sunulur. 

Bir ortam değişkenini örneğinizin URL 'siyle birlikte ayarlamak için, önce Azure Digital TWINS örneğinizin ana bilgisayar adını bularak URL 'YI alın. [Azure Portal](https://portal.azure.com) arama çubuğunda örneğiniz için arama yapın. Ardından, _ana bilgisayar adını_ görüntülemek için sol gezinti çubuğundaki _genel bakış_ ' ı seçin. Bu değeri kopyalayın.

:::image type="content" source="media/how-to-create-azure-function/instance-host-name.png" alt-text="Azure portal ekran görüntüsü: Azure dijital TWINS örneği için genel bakış sayfasından ana bilgisayar adı değeri vurgulanır.":::

Artık şu adımlarla bir uygulama ayarı oluşturabilirsiniz:

1. Portal arama çubuğunda işlev uygulamanızı arayın ve sonuçlardan seçim yapın.

    :::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Azure portal ekran görüntüsü: işlev uygulamasının adı, Portal arama çubuğunda aranır ve arama sonucu vurgulanır.":::

1. Sol taraftaki Gezinti çubuğunda _yapılandırma_ ' yı seçin. _Uygulama ayarları_ sekmesinde _+ Yeni uygulama ayarı_' nı seçin.

    :::image type="content" source="media/how-to-create-azure-function/application-setting.png" alt-text="Azure portal ekran görüntüsü: işlev uygulamasının yapılandırma sayfasında, yeni bir uygulama ayarı oluşturmak için düğme vurgulanır.":::

1. Açılan pencerede, bir uygulama ayarı oluşturmak için yukarıda kopyalanmış ana bilgisayar adı değerini kullanın.
    * **Ad**: ADT_SERVICE_URL
    * **Değer**: https://{-Azure-dijital-TWINS-ana bilgisayar-adı}
    
    Bir uygulama ayarı oluşturmak için _Tamam ' ı_ seçin.
    
    :::image type="content" source="media/how-to-create-azure-function/add-application-setting.png" alt-text="Azure portal ekran görüntüsü: Tamam düğmesi, uygulama ayarı Ekle/Düzenle sayfasında ad ve değer alanları doldurulduktan sonra vurgulanır.":::

1. Ayarı oluşturduktan sonra, _uygulama ayarları_ sekmesinde geri göründüğünü görmeniz gerekir. Listede *ADT_SERVICE_URL* göründüğünü doğrulayın ve ardından _Kaydet_ düğmesini seçerek yeni uygulama ayarını kaydedin.

    :::image type="content" source="media/how-to-create-azure-function/application-setting-save-details.png" alt-text="Azure portal ekran görüntüsü: yeni ADT_SERVICE_URL ayarı vurgulanmış şekilde uygulama ayarları sayfası. Kaydet düğmesi de vurgulanır.":::

1. Uygulama ayarlarında yapılan değişikliklerin etkili olması için uygulama yeniden başlatması gerekir, bu nedenle istendiğinde uygulamanızı yeniden başlatmak için _devam_ ' ı seçin.

    :::image type="content" source="media/how-to-create-azure-function/save-application-setting.png" alt-text="Azure portal ekran görüntüsü: uygulama ayarlarında, uygulamanızı yeniden başlatma ile ilgili herhangi bir değişiklik olduğuna ilişkin bir uyarı vardır. Devam düğmesi vurgulanır.":::

---

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure dijital TWINS ile kullanmak üzere Azure 'da bir işlev uygulaması ayarlama adımları izlenir.

Daha sonra, Azure dijital TWINS 'e IoT Hub verileri almak için temel işlevinizde nasıl derleme yapılacağını öğrenin:
* [*Nasıl yapılır: IoT Hub 'dan alma telemetrisi*](how-to-ingest-iot-hub-data.md)
