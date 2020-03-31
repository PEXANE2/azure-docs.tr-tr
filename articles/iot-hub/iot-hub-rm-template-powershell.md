---
title: Şablon (PowerShell) kullanarak Azure IoT Hub'ı oluşturma | Microsoft Dokümanlar
description: Azure PowerShell'e sahip bir IoT Hub'ı oluşturmak için Azure Kaynak Yöneticisi şablonu nasıl kullanılır?
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: a1f878361a96c5584f43b31135d79ff799f66efa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75976618"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Azure Kaynak Yöneticisi şablonu (PowerShell) kullanarak bir IoT hub'ı oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bir IoT Hub'ı ve bir tüketici grubu oluşturmak için Azure Kaynak Yöneticisi şablonu nasıl kullanacağınızı öğrenin. Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Kaynak Yöneticisi şablonları geliştirme hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi belgelerine](https://docs.microsoft.com/azure/azure-resource-manager/)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Bu hızlı başlatmada kullanılan Kaynak Yöneticisi şablonu [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-iothub-with-consumergroup-create/) Şablonun bir kopyası aşağıda verilmiştir:

[!code-json[iothub-creation](~/quickstart-templates/101-iothub-with-consumergroup-create/azuredeploy.json)]

Şablon, üç uç noktası (eventhub, bulut-aygıt ve mesajlaşma) ve bir tüketici grubu içeren bir Azure Iot hub'ı oluşturur. Daha fazla şablon örneği için [Azure Quickstart şablonlarına](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Devices&pageNumber=1&sort=Popular)bakın. Iot Hub şablon şeması [burada](https://docs.microsoft.com/azure/templates/microsoft.devices/iothub-allversions)bulunabilir.

Şablon dağıtmak için çeşitli yöntemler vardır.  Bu eğitimde Azure PowerShell'i kullanırsınız.

PowerShell komut dosyasını çalıştırmak için Azure Bulutu kabuğunu açmak için **deneyin'i** seçin. Komut dosyasını yapıştırmak için kabuğu sağ tıklatın ve ardından Yapıştır'ı seçin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$iotHubName = Read-Host -Prompt "Enter the IoT Hub name"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-iothub-with-consumergroup-create/azuredeploy.json" `
    -iotHubName $iotHubName
```

PowerShell komut dosyasından da görebileceğiniz gibi, kullanılan şablon Azure Quickstart şablonlarından dır. Kendi dosyanızı kullanmak için önce şablon dosyasını Bulut kabuğuna yüklemeniz ve ardından dosya adını belirtmek için `-TemplateFile` anahtarı kullanmanız gerekir.  Örneğin, [bkz. şablonu dağıt.](../azure-resource-manager/templates/quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell#deploy-the-template)

## <a name="next-steps"></a>Sonraki adımlar

Artık bir Azure Kaynak Yöneticisi şablonu kullanarak bir IoT hub'ı dağıttınız, daha fazla araştırma yapmak isteyebilirsiniz:

* [IoT Hub kaynak sağlayıcısı REST API'nin][lnk-rest-api]yetenekleri hakkında bilgi edinin.
* Azure Kaynak Yöneticisi'nin yetenekleri hakkında daha fazla bilgi edinmek için [Azure Kaynak Yöneticisi'ne genel bakışı][lnk-azure-rm-overview] okuyun.
* Şablonlarda kullanılacak JSON sözdizimi ve özellikleri için [Microsoft.Devices kaynak türlerine](/azure/templates/microsoft.devices/iothub-allversions)bakın.

IoT Hub için geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya Giriş][lnk-c-sdk]
* [Azure IoT SDK’ları][lnk-sdks]

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-Az-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/management/overview.md
[lnk-powershell-arm]: ../azure-resource-manager/management/manage-resources-powershell.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
