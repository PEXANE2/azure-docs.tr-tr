---
title: Azure IoT Merkezi uygulama ayarlarını değiştirme | Microsoft Dokümanlar
description: Yönetici olarak, uygulama adını, URL'sini, yükleme resmini değiştirerek azure IoT Merkezi uygulamanızı nasıl yönetebilirsiniz ve bir uygulamayı silme
author: viv-liu
ms.author: viviali
ms.date: 11/27/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 7b77ea9e0e1f322cb5ef0bc63885c3ccce1b76f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158699"
---
# <a name="change-iot-central-application-settings"></a>IoT Merkezi uygulama ayarlarını değiştirme



Bu makalede, bir yönetici olarak uygulama adını ve URL'yi değiştirerek, resim yükleyerek ve Azure IoT Central uygulamanızdaki bir uygulamayı silerek uygulamayı nasıl yönetebileceğiniz açıklanmaktadır.

Yönetim bölümüne erişmek ve **bunları** kullanmak için, Bir Azure IoT Merkezi uygulaması için **Yönetici** rolünde olmalısınız. Bir Azure IoT Merkezi uygulaması oluşturursanız, otomatik olarak bu uygulama için **Yönetici** rolüne atanırsınız.

## <a name="change-application-name-and-url"></a>Uygulama adını ve URL'yi değiştirme

Uygulama **Ayarları** sayfasında, uygulamanızın adını ve URL'sini değiştirebilir, ardından **Kaydet'i**seçebilirsiniz.

![Uygulama ayarları sayfası](media/howto-administer/image0-a.png)

Yöneticiniz uygulamanız için özel bir tema oluşturursa, bu sayfada Kullanıcı Arabirimi'nde **Uygulama Adını** gizleme seçeneği de yer almaktadır. Bu seçenek, özel temadaki uygulama logosu uygulama adını içeriyorsa yararlıdır. Daha fazla bilgi için Azure [IoT Merkezi Kullanıcı Arabirimi'ni özelleştir'e](./howto-customize-ui.md)bakın.

> [!Note]
> URL'nizi değiştirirseniz, eski URL'niz başka bir Azure IoT Central müşterisi tarafından alınabilir. Bu durumda, artık kullanmak için kullanılabilir. URL'nizi değiştirdiğinizde, eski URL artık çalışmıyor ve kullanıcılarınızı kullanmak üzere yeni URL hakkında bilgilendirmeniz gerekir.

## <a name="delete-an-application"></a>Uygulamayı silme

IoT Merkezi uygulamanızı kalıcı olarak silmek için **Sil** düğmesini kullanın. Bu eylem, uygulamayla ilişkili tüm verileri kalıcı olarak siler.

> [!Note]
> Bir uygulamayı silmek için, uygulamayı oluşturduğunuzda seçtiğiniz Azure aboneliğindeki kaynakları silme iznine de sahip olmalısınız. Daha fazla bilgi edinmek için Azure [abonelik kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimini kullan'a](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)bakın.

## <a name="manage-programmatically"></a>Programlama yoluyla yönetme

Düğüm, Python, C#, Ruby, Java ve Go için IoT Central Azure Kaynak Yöneticisi SDK paketleri mevcuttur. Bu paketleri IoT Merkezi uygulamalarını oluşturmak, listelemek, güncelleştirmek veya silmek için kullanabilirsiniz. Paketler, kimlik doğrulama ve hata işlemeyi yönetmek için yardımcıları içerir.

Azure Kaynak Yöneticisi SDK'larını nasıl kullanacağınızla [https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples)ilgili örnekler bulabilirsiniz.

Daha fazla bilgi edinmek için aşağıdaki GitHub depolarına ve paketlerine bakın:

| Dil | Depo | Paket |
| ---------| ---------- | ------- |
| Node | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Başlayın | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızı nasıl yöneteceğimize ilişkin bilgi aldığınıza göre, önerilen bir sonraki adım Azure IoT Central'daki [kullanıcıları ve rolleri yönet](howto-manage-users-roles.md) hakkında bilgi edinmektir.
