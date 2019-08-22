---
title: Azure IoT Central uygulamanızı yönetme | Microsoft Docs
description: Yönetici olarak, uygulama adını, URL 'yi değiştirerek, görüntüyü karşıya yükleme, kopyalama ve uygulamayı silme ile Azure IoT Central uygulamanızı yönetme
author: viv-liu
ms.author: viviali
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: bdb13ef0d4f05802e4ab0f383c1bf3d82d9ba632
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69880975"
---
# <a name="manage-your-iot-central-application"></a>IoT Central uygulamanızı yönetme

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu makalede, yönetici olarak uygulama adı ve URL 'yi değiştirerek uygulamayı nasıl yönetebileceğinizi, görüntüyü karşıya yüklemeyi ve ayrıca Azure IoT Central uygulamanızda bir uygulamayı kopyalamayı ve silmeyi öğreneceksiniz.

**Yönetim** bölümüne erişmek ve bunları kullanmak için, bir Azure IoT Central uygulaması için **yönetici** rolünde olmanız gerekir. Azure IoT Central uygulaması oluşturursanız, bu uygulama için otomatik olarak **yönetici** rolüne atanırsınız. 

## <a name="change-application-name-and-url"></a>Uygulama adını ve URL 'YI değiştirme

**Uygulama ayarları** sayfasında uygulamanızın adını ve URL 'Sini değiştirip **Kaydet**' i seçebilirsiniz.

![Uygulama ayarları sayfası](media/howto-administer-pnp/image0-a.png)

Yöneticiniz uygulamanız için özel bir tema oluşturursa, Bu sayfa Kullanıcı arabirimindeki **uygulama adını** gizlemek için bir seçenek içerir. Bu, özel temadaki uygulama logosu uygulama adını içeriyorsa yararlıdır. Daha fazla bilgi için bkz. [Azure IoT Central kullanıcı arabirimini özelleştirme](./howto-customize-ui-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

> [!Note]
> URL 'nizi değiştirirseniz, eski URL 'niz başka bir Azure IoT Central müşterisi tarafından alınabilir. Bu durumda, artık kullanılamaz. URL 'nizi değiştirdiğinizde eski URL artık çalışmaz ve kullanıcılarınıza kullanılacak yeni URL 'YI bilgilendirmesini isteyebilirsiniz.

## <a name="prepare-and-upload-image"></a>Görüntüleri hazırlama ve karşıya yükleme

Uygulama görüntüsünü değiştirmek için bkz. [Azure IoT Central uygulamanıza görüntü hazırlama ve yükleme](howto-prepare-images.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="copy-an-application"></a>Bir uygulamayı kopyalama

Herhangi bir uygulamanın bir kopyasını, eksi herhangi bir cihaz örneğini, cihaz veri geçmişini ve Kullanıcı verilerini oluşturabilirsiniz. Kopya, ücretlendirilebilecek Kullandıkça Öde uygulamasıdır. Bu şekilde bir deneme uygulaması oluşturamazsınız.

**Kopyala**' yı seçin. İletişim kutusunda yeni Kullandıkça Öde uygulamasının ayrıntılarını girin. Sonra devam etmek istediğinizi onaylamak için **Kopyala** ' yı seçin. [Uygulama oluşturma](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hızlı başlangıcı ' nda bu formdaki alanlar hakkında daha fazla bilgi edinin.

![Uygulama ayarları sayfası](media/howto-administer-pnp/appcopy2.png)

Uygulama kopyalama işlemi başarılı olduktan sonra, bağlantısını kullanarak yeni uygulamaya gidebilirsiniz.

![Uygulama ayarları sayfası](media/howto-administer-pnp/appcopy3a.png)

Bir uygulamayı kopyalamak kuralların ve e-posta eyleminin tanımını da kopyalar. Flow, Logic Apps vb. gibi bazı eylemler kural KIMLIĞI aracılığıyla belirli kurallara bağlıdır. Bir kural farklı bir uygulamaya kopyalandığında kendi kural KIMLIĞINI alır. Bu durumda, kullanıcıların yeni bir eylem oluşturması ve yeni kuralı onunla ilişkilendirilmesi gerekir. Genel olarak, yeni uygulamada güncel olduklarından emin olmak için kuralları ve eylemleri denetlemeniz iyi bir fikirdir.

> [!WARNING]
> Bir pano belirli cihazlarla ilgili bilgileri görüntüleyen kutucuklar içeriyorsa, bu kutucuklar **istenen kaynağı** yeni uygulamada bulamadı. Yeni uygulamanızdaki cihazlarla ilgili bilgileri göstermek için bu kutucukları yeniden yapılandırmanız gerekir.

## <a name="delete-an-application"></a>Bir uygulamayı silme

IoT Central uygulamanızı kalıcı olarak silmek için **Sil** düğmesini kullanın. Bu eylem uygulamayla ilişkili tüm verileri kalıcı olarak siler.

> [!Note]
> Bir uygulamayı silmek için, uygulamayı oluştururken seçtiğiniz Azure aboneliğindeki kaynakları silme izinlerine de sahip olmanız gerekir. Daha fazla bilgi edinmek için bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimi kullanma](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).


## <a name="manage-programatically"></a>Program aracılığıyla yönetme

IoT Central Azure Resource Manager SDK paketleri Node, Python, C#, Ruby, Java ve go için kullanılabilir. Bu paketleri IoT Central uygulamalar oluşturmak, listelemek, güncelleştirmek veya silmek için kullanabilirsiniz. Paketler, kimlik doğrulama ve hata işlemeyi yönetmek için yardımcıları içerir.

Azure Resource Manager SDK [https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples)'larının nasıl kullanılacağına ilişkin örnekler bulabilirsiniz.

Daha fazla bilgi edinmek için aşağıdaki GitHub depoları ve paketlerine bakın:

| Dil | Havuz | Paket |
| ---------| ---------- | ------- |
| Düğüm | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Git | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Sonraki adımlar
 
Azure IoT Central uygulamanızı yönetme hakkında bilgi edindiğinize göre, önerilen sonraki adım Azure IoT Central [kullanıcıları ve rolleri yönetme](howto-manage-users-roles-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında bilgi almanız gerekir.