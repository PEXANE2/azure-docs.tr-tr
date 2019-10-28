---
title: Azure IoT Central uygulama ayarlarını değiştirme | Microsoft Docs
description: Yönetici olarak, uygulama adını, URL 'yi değiştirerek, görüntüyü karşıya yükleyerek ve bir uygulamayı silerek Azure IoT Central uygulamanızı yönetme
author: viv-liu
ms.author: viviali
ms.date: 10/10/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 7396b7dc22f0be77b9fc62a4dea379cf03a6befd
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72951815"
---
# <a name="change-iot-central-application-settings"></a>IoT Central uygulama ayarlarını değiştir

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu makalede, yönetici olarak uygulama adı ve URL 'yi değiştirerek, görüntüyü karşıya yükleyerek ve Azure IoT Central uygulamanızda bir uygulamayı silerek uygulamayı nasıl yönetebileceğinizi açıklanmaktadır.

**Yönetim** bölümüne erişmek ve bunları kullanmak için, bir Azure IoT Central uygulaması için **yönetici** rolünde olmanız gerekir. Azure IoT Central uygulaması oluşturursanız, bu uygulama için otomatik olarak **yönetici** rolüne atanırsınız.

## <a name="change-application-name-and-url"></a>Uygulama adını ve URL 'YI değiştirme

**Uygulama ayarları** sayfasında uygulamanızın adını ve URL 'Sini değiştirip **Kaydet**' i seçebilirsiniz.

![Uygulama ayarları sayfası](media/howto-administer-pnp/image0-a.png)

Yöneticiniz uygulamanız için özel bir tema oluşturursa, Bu sayfa Kullanıcı arabirimindeki **uygulama adını** gizlemek için bir seçenek içerir. Özel temadaki uygulama logosu uygulama adını içeriyorsa bu seçenek faydalıdır. Daha fazla bilgi için bkz. [Azure IoT Central kullanıcı arabirimini özelleştirme](./howto-customize-ui-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

> [!Note]
> URL 'nizi değiştirirseniz, eski URL 'niz başka bir Azure IoT Central müşterisi tarafından alınabilir. Bu durumda, artık kullanılamaz. URL 'nizi değiştirdiğinizde eski URL artık çalışmaz ve kullanıcılarınıza kullanılacak yeni URL 'YI bilgilendirmesini isteyebilirsiniz.

## <a name="delete-an-application"></a>Bir uygulamayı silme

IoT Central uygulamanızı kalıcı olarak silmek için **Sil** düğmesini kullanın. Bu eylem uygulamayla ilişkili tüm verileri kalıcı olarak siler.

> [!Note]
> Bir uygulamayı silmek için, uygulamayı oluştururken seçtiğiniz Azure aboneliğindeki kaynakları silme izinlerine de sahip olmanız gerekir. Daha fazla bilgi edinmek için bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimi kullanma](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

## <a name="manage-programmatically"></a>Programlama yoluyla yönetme

IoT Central Azure Resource Manager SDK paketleri Node, Python, C#, Ruby, Java ve go için kullanılabilir. Bu paketleri IoT Central uygulamalar oluşturmak, listelemek, güncelleştirmek veya silmek için kullanabilirsiniz. Paketler, kimlik doğrulama ve hata işlemeyi yönetmek için yardımcıları içerir.

[https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples)Azure Resource Manager SDK 'larının nasıl kullanılacağına ilişkin örnekler bulabilirsiniz.

Daha fazla bilgi edinmek için aşağıdaki GitHub depoları ve paketlerine bakın:

| Dil | Depo | Paket |
| ---------| ---------- | ------- |
| Düğüm | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Go | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızı yönetme hakkında bilgi edindiğinize göre, önerilen sonraki adım Azure IoT Central [kullanıcıları ve rolleri yönetme](howto-manage-users-roles-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında bilgi almanız gerekir.
