---
title: Dosya yüklemesini yapılandırmak için Azure PowerShell'i kullanın | Microsoft Dokümanlar
description: IoT hub'ınızı bağlı aygıtlardan dosya yüklemelerini etkinleştirmek için yapılandırmak için Azure PowerShell cmdlets'i nasıl kullanılır? Hedef Azure depolama hesabını yapılandırma yla ilgili bilgileri içerir.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: robinsh
ms.openlocfilehash: c8fc0393e0961b46fbb8031d735f27e9ad785031
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60318470"
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>PowerShell kullanarak IoT Hub dosya yüklemelerini yapılandırma

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

[IoT Hub'da dosya yükleme işlevini](iot-hub-devguide-file-upload.md)kullanmak için öncelikle bir Azure depolama hesabını IoT hub'ınızla ilişkilendirmeniz gerekir. Varolan bir depolama hesabı kullanabilir veya yeni bir depo oluşturabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* [Azure PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-Az-ps).

* Azure IoT hub'ı. Bir IoT hub'ınyoksa, [yeni-AzIoTHub cmdlet'ini](https://docs.microsoft.com/powershell/module/az.iothub/new-aziothub) kullanarak bir hub oluşturabilir veya portalı [IoT hub'ı oluşturmak](iot-hub-create-through-portal.md)için kullanabilirsiniz.

* Bir Azure depolama hesabı. Azure depolama hesabınız yoksa, azure [depolama powershell cmdlets'ini](https://docs.microsoft.com/powershell/module/az.storage/) kullanarak bir tane oluşturabilir veya bir [depolama hesabı oluşturmak](../storage/common/storage-create-storage-account.md) için portalı kullanabilirsiniz

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açın ve Azure hesabınızı ayarlayın

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. PowerShell komut isteminde **Connect-AzAccount** cmdlet'i çalıştırın:

    ```powershell
    Connect-AzAccount
    ```

2. Birden fazla Azure aboneliğiniz varsa, Azure'da oturum açmak kimlik bilgilerinizle ilişkili tüm Azure aboneliklerine erişmenizi tanır. Kullanabileceğiniz Azure aboneliklerini listelemek için aşağıdaki komutu kullanın:

    ```powershell
    Get-AzSubscription
    ```

    IoT hub'ınızı yönetmek için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```powershell
    Select-AzSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>Depolama hesabı bilgilerinizi alma

Aşağıdaki adımlar, Depolama hesabınızı **Klasik** dağıtım modelini değil, **Kaynak Yöneticisi** dağıtım modelini kullanarak oluşturduğunuzu varsayar.

Aygıtlarınızdan dosya yüklemelerini yapılandırmak için bir Azure depolama hesabının bağlantı dizesine ihtiyacınız vardır. Depolama hesabı, IoT hub'ınızla aynı abonelikte olmalıdır. Ayrıca depolama hesabında bir blob konteyner adı gerekir. Depolama hesabı anahtarlarınızı almak için aşağıdaki komutu kullanın:

```powershell
Get-AzStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

**Key1** depolama hesabı anahtar değerini not alın. Aşağıdaki adımlarda ihtiyacınız var.

Dosya yüklemeleriniz için varolan bir blob kapsayıcısı kullanabilir veya yeni bir tane oluşturabilirsiniz:

* Depolama hesabınızdaki varolan blob kapsayıcılarını listelemek için aşağıdaki komutları kullanın:

    ```powershell
    $ctx = New-AzStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzStorageContainer -Context $ctx
    ```

* Depolama hesabınızda bir blob kapsayıcısı oluşturmak için aşağıdaki komutları kullanın:

    ```powershell
    $ctx = New-AzStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>IoT hub'ınızı yapılandırın

Artık IoT hub'ınızı, depolama hesabı bilgilerinizi kullanarak [IoT hub'ına dosya yükecek](iot-hub-devguide-file-upload.md) şekilde yapılandırabilirsiniz.

Yapılandırma aşağıdaki değerleri gerektirir:

* **Depolama kapsayıcısı**: IoT hub'ınızla ilişkilendirmek için geçerli Azure aboneliğinizdeki bir Azure depolama hesabındaki blob kapsayıcısı. Önceki bölümde gerekli depolama hesabı bilgilerini aldınız. IoT Hub, aygıtların dosya yüklerken kullanmaları için bu blob kapsayıcısına yazma izinleri ile otomatik olarak SAS URI'leri oluşturur.

* **Yüklenen dosyalar için bildirimler alın**: Dosya yükleme bildirimlerini etkinleştirin veya devre dışı.

* **SAS TTL**: Bu ayar, IoT Hub tarafından cihaza döndürülen SAS URI'lerinin canlı olarak zaman dilimidir. Varsayılan olarak bir saatolarak ayarlayın.

* **Dosya bildirim ayarları varsayılan TTL**: Süresi dolmadan önce dosya yükleme bildiriminin zaman aşımısüresi. Varsayılan olarak bir güne ayarlayın.

* **Dosya bildirimi maksimum teslimat sayısı**: IoT Hub'ının dosya yükleme bildirimini kaç kez sunmaya çalıştığı. Varsayılan olarak 10 olarak ayarlayın.

IoT hub'ınızdaki dosya yükleme ayarlarını yapılandırmak için aşağıdaki PowerShell cmdlet'i kullanın:

```powershell
Set-AzIotHub `
    -ResourceGroupName "{your iot hub resource group}" `
    -Name "{your iot hub name}" `
    -FileUploadNotificationTtl "01:00:00" `
    -FileUploadSasUriTtl "01:00:00" `
    -EnableFileUploadNotifications $true `
    -FileUploadStorageConnectionString "DefaultEndpointsProtocol=https;AccountName={your storage account name};AccountKey={your storage account key};EndpointSuffix=core.windows.net" `
    -FileUploadContainerName "{your blob container name}" `
    -FileUploadNotificationMaxDeliveryCount 10
```

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub'ın dosya yükleme özellikleri hakkında daha fazla bilgi için bir [aygıttan dosya yükleme](iot-hub-devguide-file-upload.md)bölümüne bakın.

Azure IoT Hub'ı yönetme hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [IoT cihazlarını toplu yönetme](iot-hub-bulk-identity-mgmt.md)
* [IoT Hub ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
* [IoT çözümünüzü sıfırdan emniyete](../iot-fundamentals/iot-security-ground-up.md)
