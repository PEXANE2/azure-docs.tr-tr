---
title: Azure 'da verileri işlemek için bir işlev ayarlama
titleSuffix: Azure Digital Twins
description: Azure 'da dijital TWINS 'e erişebilen ve tetiklenebilecek bir işlev oluşturma bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 8/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 1178b0ab5af3642026fe78c7de788f354691b13a
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98701176"
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

İşlev uygulamanız oluşturulduktan sonra Visual Studio, proje klasörünüzdeki bir **function1.cs** dosyasında kod örneği oluşturur. Bu kısa işlev olayları günlüğe kaydetmek için kullanılır.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-sample-code.png" alt-text="Oluşturulan yeni proje için proje penceresinde Visual Studio 'nun ekran görüntüsü. Işlev1 adlı örnek bir işlev için kod vardır." lightbox="media/how-to-create-azure-function/visual-studio-sample-code.png":::

## <a name="write-a-function-with-an-event-grid-trigger"></a>Event Grid tetikleyicisiyle bir işlev yazma

İşlev uygulamanıza SDK ekleyerek bir işlev yazabilirsiniz. İşlev uygulaması, [.net Için Azure dijital TWINS SDK 'sını (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true)kullanarak Azure Digital TWINS ile etkileşime girer. 

SDK 'yı kullanmak için aşağıdaki paketleri projenize eklemeniz gerekir. Paketleri Visual Studio 'nun NuGet Paket Yöneticisi 'ni kullanarak yükleyebilir veya `dotnet` bir komut satırı aracında kullanarak paketleri ekleyebilirsiniz. Tercih ettiğiniz Yöntem için aşağıdaki adımları izleyin.

**1. seçenek. Visual Studio Package Manager 'ı kullanarak paket ekleme:**
    
Projenizi sağ seçin ve listeden _NuGet Paketlerini Yönet_ ' i seçin. Sonra, açılan pencerede, _tarayıcı_ sekmesini seçin ve aşağıdaki paketleri arayın. Paketi yüklemek için, _yüklemeyi_ seçin ve lisans sözleşmesini _kabul edin_ .

* `Azure.DigitalTwins.Core`
* `Azure.Identity`
* `System.Net.Http`
* `Azure.Core`

**Seçenek 2. `dotnet` Komut satırı aracını kullanarak paket ekleyin:**

Alternatif olarak, `dotnet add` bir komut satırı aracında aşağıdaki komutları kullanabilirsiniz:

```cmd/sh
dotnet add package Azure.DigitalTwins.Core
dotnet add package Azure.Identity
dotnet add package System.Net.Http
dotnet add package Azure.Core
```

Ardından, Visual Studio Çözüm Gezgini, örnek kodunuzun bulunduğu _function1.cs_ dosyasını açın ve `using` işlevinizin aşağıdaki deyimlerini ekleyin. 

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="Function_dependencies":::

## <a name="add-authentication-code-to-the-function"></a>İşleve kimlik doğrulama kodu ekleyin

Artık sınıf düzeyi değişkenleri bildirecektir ve işlevin Azure dijital TWINS 'e erişmesine izin verecek kimlik doğrulama kodu eklersiniz. _Function1.cs_ dosyasına aşağıdaki işlevi ekleyin.

* Bir ortam değişkeni olarak Azure Digital TWINS hizmet URL 'sini okumak için kod. Hizmet URL 'sini, işlevinde sabit kodlamak yerine, bir ortam değişkeninden okumak iyi bir uygulamadır.

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

## <a name="set-up-security-access-for-the-function-app"></a>İşlev uygulaması için güvenlik erişimini ayarlama

Azure CLı veya Azure portal kullanarak işlev uygulaması için güvenlik erişimi ayarlayabilirsiniz. Aşağıdaki tercih ettiğiniz seçeneğe yönelik adımları izleyin.

### <a name="option-1-set-up-security-access-for-the-function-app-using-cli"></a>Seçenek 1: CLı kullanarak işlev uygulaması için güvenlik erişimini ayarlama

Önceki örneklerden iskelet işlevi, Azure Digital TWINS ile kimlik doğrulaması yapabilmek için bir taşıyıcı belirtecinin kendisine geçirilmesini gerektirir. Bu taşıyıcı belirtecinin geçirildiğinden emin olmak için, işlev uygulaması için [yönetilen hizmet kimliği (MSI)](../active-directory/managed-identities-azure-resources/overview.md) ayarlamanız gerekir. Bu, her bir işlev uygulaması için yalnızca bir kez yapılmalıdır.

Azure dijital TWINS örneğiniz için sistem tarafından yönetilen kimlik oluşturabilir ve işlev uygulamasının kimliğini _**Azure Digital TWINS veri sahibi**_ rolüne atayabilirsiniz. Bu, veri düzlemi etkinliklerini gerçekleştirmek için örnekte işlev uygulamasına izin verir. Daha sonra, bir ortam değişkeni ayarlayarak Azure dijital TWINS örneğinin URL 'sini işlevinizle erişilebilir yapın.

[!INCLUDE [digital-twins-role-rename-note.md](../../includes/digital-twins-role-rename-note.md)]

Komutları çalıştırmak için [Azure Cloud Shell](https://shell.azure.com) kullanın.

Sistem tarafından yönetilen kimliği oluşturmak için aşağıdaki komutu kullanın. Çıktıda _PrincipalId_ alanını bir yere göz atın.

```azurecli-interactive 
az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>   
```
_principalId_ değerini aşağıdaki komutta kullanarak işlev uygulamasının kimliğini Azure Digital Twins örneğinizin _Azure Digital Twins Veri Sahibi_ rolüne atayın.

```azurecli-interactive 
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Data Owner"
```
Son olarak, bir ortam değişkeni ayarlayarak Azure dijital TWINS örneğinizin URL 'sini işleviniz için erişilebilir hale getirebilirsiniz. Ortam değişkenlerini ayarlama hakkında daha fazla bilgi için bkz. [*ortam değişkenleri*](/sandbox/functions-recipes/environment-variables). 

> [!TIP]
> Azure Digital TWINS örneğinin URL 'SI, Azure Digital TWINS örneğinizin *ana bilgisayar adının* başlangıcına *https://* eklenerek yapılır. Ana bilgisayar adını görmek için, örneğinizin tüm özellikleriyle birlikte çalıştırabilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

```azurecli-interactive 
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-hostname>"
```
### <a name="option-2-set-up-security-access-for-the-function-app-using-azure-portal"></a>Seçenek 2: Azure portal kullanarak işlev uygulaması için güvenlik erişimini ayarlama

Bir sistem tarafından atanmış yönetilen kimlik, Azure kaynaklarının kimlik bilgilerini kodda depolamadan (örneğin, Azure Key Vault) kimlik doğrulaması yapmasına olanak sağlar. Etkinleştirildikten sonra, tüm gerekli izinler Azure rol tabanlı erişim denetimi aracılığıyla verilebilir. Bu tür yönetilen kimliğin yaşam döngüsü, bu kaynağın yaşam döngüsüne bağlıdır. Ayrıca, her kaynak (örneğin, sanal makine) yalnızca bir sistem tarafından atanmış yönetilen kimliğe sahip olabilir.

[Azure Portal](https://portal.azure.com/), arama çubuğunda _işlev uygulaması_ ' nı daha önce oluşturduğunuz işlev uygulaması adı ile arayın. Listeden *işlev uygulaması* seçin. 

:::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Azure portal ekran görüntüsü: işlev uygulamasının adı, Portal arama çubuğunda aranır ve arama sonucu vurgulanır.":::

İşlev uygulaması penceresinde, yönetilen kimliği etkinleştirmek için sol taraftaki Gezinti çubuğundan _kimlik_ ' i seçin.
_Sistem atandı_ sekmesinde _durumu_ açık olarak değiştirin ve _kaydedin_ . _Sistem tarafından atanan yönetilen kimliği etkinleştirmek_ için bir açılır pencere görürsünüz.
_Evet_ düğmesini seçin. 

:::image type="content" source="media/how-to-create-azure-function/enable-system-managed-identity.png" alt-text="Azure portal ekran görüntüsü: işlev uygulamasının kimlik sayfasında, sistem tarafından atanan yönetilen kimliği etkinleştirme seçeneği Evet olarak ayarlanmıştır. Durum seçeneği açık olarak ayarlanır.":::

İşlevinizin Azure Active Directory başarıyla kaydedildiğini bildirimleri doğrulayabilirsiniz.

:::image type="content" source="media/how-to-create-azure-function/notifications-enable-managed-identity.png" alt-text="Azure portal ekran görüntüsü: portalın üst çubuğunda Bell-şekillendirilmiş simgesini seçip bildirimler listesi. Kullanıcının sistem tarafından atanmış yönetilen kimliği etkinleştirmiş olduğunu belirten bir bildirim vardır.":::

Ayrıca, _kimlik_ sayfasında GÖSTERILEN **nesne kimliğini** bir sonraki bölümde kullanılacak şekilde aklınızda bulabilirsiniz.

:::image type="content" source="media/how-to-create-azure-function/object-id.png" alt-text="Azure portal ekran görüntüsü: Azure Işlevinin kimlik sayfasından nesne KIMLIĞI alanının etrafında bir vurgu.":::

### <a name="assign-access-roles-using-azure-portal"></a>Azure portal kullanarak erişim rolleri atama

Azure *rol atamaları sayfasını açmak* için _Azure rol atamaları_ düğmesini seçin. Ardından _+ rol ataması Ekle (Önizleme)_ seçeneğini belirleyin.

:::image type="content" source="media/how-to-create-azure-function/add-role-assignments.png" alt-text="Azure portal ekran görüntüsü: Azure Işlevinin kimlik sayfasındaki Izinler altında Azure rol atamaları düğmesinin etrafında vurgu.":::

Açılan _rol ataması Ekle (Önizleme)_ sayfasında şunları seçin:

* _Kapsam_: Kaynak grubu
* _Abonelik_: Azure aboneliğinizi seçin
* _Kaynak grubu_: açılan listeden kaynak grubunuzu seçin
* _Rol_: açılan listeden _Azure dijital TWINS veri sahibini_ seçin

Sonra, _Kaydet_ düğmesine basarak ayrıntılarınızı kaydedin.

:::image type="content" source="media/how-to-create-azure-function/add-role-assignment.png" alt-text="Yeni bir rol ataması (Önizleme) eklemek için Azure portal: Iletişim kutusunun ekran görüntüsü. Kapsam, abonelik, kaynak grubu ve rol için alanlar vardır.":::

### <a name="configure-application-settings-using-azure-portal"></a>Azure portal kullanarak uygulama ayarlarını yapılandırma

Bir ortam değişkeni ayarlayarak, Azure dijital TWINS örneğinizin URL 'sini işleviniz için erişilebilir hale getirebilirsiniz. Bunun hakkında daha fazla bilgi için bkz. [*ortam değişkenleri*](/sandbox/functions-recipes/environment-variables). Uygulama ayarları, dijital TWINS örneğine erişmek için ortam değişkenleri olarak sunulur. 

Bir ortam değişkenini örneğinizin URL 'siyle birlikte ayarlamak için, önce Azure Digital TWINS örneğinizin ana bilgisayar adını bularak URL 'YI alın. [Azure Portal](https://portal.azure.com) arama çubuğunda örneğiniz için arama yapın. Ardından, _ana bilgisayar adını_ görüntülemek için sol gezinti çubuğundaki _genel bakış_ ' ı seçin. Bu değeri kopyalayın.

:::image type="content" source="media/how-to-create-azure-function/adt-hostname.png" alt-text="Azure portal ekran görüntüsü: Azure dijital TWINS örneği için genel bakış sayfasından ana bilgisayar adı değeri vurgulanır.":::

Artık aşağıdaki adımları izleyerek bir uygulama ayarı oluşturabilirsiniz:

1. Portal arama çubuğunda işlev uygulamanızı arayın ve sonuçlardan seçin
1. Yeni uygulama ayarı oluşturmak için sol taraftaki Gezinti çubuğunda _yapılandırma_ ' yı seçin.
1. _Uygulama ayarları_ sekmesinde _+ Yeni uygulama ayarı_ ' nı seçin.

:::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Azure portal ekran görüntüsü: işlev uygulamasının adı, Portal arama çubuğunda aranır ve arama sonucu vurgulanır.":::

:::image type="content" source="media/how-to-create-azure-function/application-setting.png" alt-text="Azure portal ekran görüntüsü: işlev uygulamasının yapılandırma sayfasında, yeni bir uygulama ayarı oluşturmak için düğme vurgulanır.":::

Açılan pencerede, bir uygulama ayarı oluşturmak için yukarıda kopyalanmış ana bilgisayar adı değerini kullanın.
* **Ad**: ADT_SERVICE_URL
* **Değer**: https://{-Azure-dijital-TWINS-ana bilgisayar-adı}

Bir uygulama ayarı oluşturmak için _Tamam ' ı_ seçin.

:::image type="content" source="media/how-to-create-azure-function/add-application-setting.png" alt-text="Azure portal ekran görüntüsü: Tamam düğmesi, uygulama ayarı Ekle/Düzenle sayfasında ad ve değer alanları doldurulduktan sonra vurgulanır.":::

Uygulama ayarlarınızı _ad_ alanı altında uygulama adı ile görüntüleyebilirsiniz. Sonra, _Kaydet_ düğmesini seçerek uygulama ayarlarınızı kaydedin.

:::image type="content" source="media/how-to-create-azure-function/application-setting-save-details.png" alt-text="Azure portal ekran görüntüsü: yeni ADT_SERVICE_URL ayarı vurgulanmış şekilde uygulama ayarları sayfası. Kaydet düğmesi de vurgulanır.":::

Uygulama ayarlarında yapılan tüm değişiklikler, uygulamanın yeniden başlatılmasını gerektirir. Uygulamanızı yeniden başlatmak için _devam_ ' ı seçin.

:::image type="content" source="media/how-to-create-azure-function/save-application-setting.png" alt-text="Azure portal ekran görüntüsü: uygulama ayarlarında, uygulamanızı yeniden başlatma ile ilgili herhangi bir değişiklik olduğuna ilişkin bir uyarı vardır. Devam düğmesi vurgulanır.":::

_Bildirimler_ simgesini seçerek uygulama ayarlarının güncelleştirildiğini görebilirsiniz. Uygulama ayarınız oluşturulmadıysa, yukarıdaki işlemi izleyerek bir uygulama ayarı eklemeyi yeniden deneyebilirsiniz.

:::image type="content" source="media/how-to-create-azure-function/notifications-update-web-app-settings.png" alt-text="Azure portal ekran görüntüsü: portalın üst çubuğunda Bell-şekillendirilmiş simgesini seçip bildirimler listesi. Web uygulaması ayarlarının başarıyla güncelleştirildiğini belirten bir bildirim vardır.":::

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure dijital TWINS ile kullanmak üzere Azure 'da bir işlev uygulaması ayarlama adımları izlenir.

Daha sonra, Azure dijital TWINS 'e IoT Hub verileri almak için temel işlevinizde nasıl derleme yapılacağını öğrenin:
* [*Nasıl yapılır: IoT Hub 'dan alma telemetrisi*](how-to-ingest-iot-hub-data.md)
