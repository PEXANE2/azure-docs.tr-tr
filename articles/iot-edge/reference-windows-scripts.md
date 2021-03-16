---
title: Windows kapsayıcıları ile Azure IoT Edge için betikler | Microsoft Docs
description: Windows cihazlarında yükleme, kaldırma veya güncelleştirme yapmak için IoT Edge PowerShell betikleri için başvuru bilgileri
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/06/2020
ms.topic: reference
ms.service: iot-edge
services: iot-edge
monikerRange: =iotedge-2018-06
ms.openlocfilehash: a919238e4a62ae8954e101cb21a2fd4943191f6a
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103489533"
---
# <a name="powershell-scripts-for-iot-edge-with-windows-containers"></a>Windows kapsayıcıları ile IoT Edge için PowerShell betikleri

[!INCLUDE [iot-edge-version-201806](../../includes/iot-edge-version-201806.md)]

Windows cihazlarında IoT Edge yükleyen, güncelleştiren veya kaldırabileceği PowerShell betiklerini anlayın.

Bu makalede açıklanan komutlar, `IoTEdgeSecurityDaemon.ps1` her [IoT Edge sürümünde](https://github.com/Azure/azure-iotedge/releases)yayınlanan dosyadan alınır. Betiğin en son sürümü aka.ms/iotedge-win adresinde her zaman kullanılabilir.

`Invoke-WebRequest`En son betik sürümüne erişmek için cmdlet 'ini kullanarak komutlardan herhangi birini çalıştırabilirsiniz. Örnek:

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge
```

Komutları çalıştırmak için bu betiği veya belirli bir sürümdeki betiğin bir sürümünü de indirebilirsiniz. Örnek:

```powershell
. <path>\IoTEdgeSecurityDaemon.ps1
Deploy-IoTEdge
```

Güvenliği artırmak için belirtilen komut dosyası imzalandı. Betiği cihazınıza indirerek ve sonra aşağıdaki PowerShell komutunu çalıştırarak imzayı doğrulayabilirsiniz:

```powershell
Get-AuthenticodeSignature "C:\<path>\IotEdgeSecurityDaemon.ps1"
```

İmza doğrulanırsa çıkış durumu **geçerli** olur.

## <a name="deploy-iotedge"></a>Deploy-IoTEdge

Deploy-IoTEdge komutu IoT Edge güvenlik cini ve bağımlılıklarını indirir ve dağıtır. Dağıtım komutu, diğerleri arasında bu ortak parametreleri kabul eder. Tam liste için komutunu kullanın `Get-Help Deploy-IoTEdge -full` .  

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** veya **Linux** | Bir kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IoT Edge, yüklemeye dahil edilen Moby kapsayıcı altyapısını kullanır. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı altyapısı yüklemeniz gerekir. |
| **Proxy** | Proxy URL 'SI | Cihazınızın internet 'e erişmek için bir proxy sunucusu üzerinden gitmesi gerekiyorsa bu parametreyi ekleyin. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Offlineınstallationpath** | Dizin yolu | Bu parametre dahil ise, yükleyici, yükleme için gereken IoT Edge cab ve VC çalışma zamanı MSI dosyaları için listelenen dizini kontrol eder. Dizinde bulunmayan tüm dosyalar indirilir. Her iki dosya da dizinde ise, internet bağlantısı olmadan IoT Edge yükleyebilirsiniz. Belirli bir sürümü kullanmak için bu parametreyi de kullanabilirsiniz. |
| **Invokewebrequestparameters** | Parametrelerin ve değerlerin Hashtable 'ı | Yükleme sırasında bazı Web istekleri yapılır. Bu Web isteklerinin parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Restartifgerekliyse** | yok | Bu bayrak dağıtım betiğinin, gerekirse, makineyi sormadan yeniden başlatmasını sağlar. |

## <a name="initialize-iotedge"></a>Initialize-IoTEdge

Initialize-IoTEdge komutu, IoT Edge cihaz bağlantı dizeniz ve işletimsel ayrıntılarla yapılandırır. Bu komutla oluşturulan bilgilerin çoğu daha sonra ıotedge\config.exe dosyasında depolanır. Başlatma komutu, diğerleri arasında bu ortak parametreleri kabul eder. Tam liste için komutunu kullanın `Get-Help Initialize-IoTEdge -full` .

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **ManualConnectionString** | Yok | **Anahtar parametresi**. **Varsayılan değer**. Hiçbir sağlama türü belirtilmemişse, bir bağlantı dizesiyle el ile sağlama varsayılan değerdir.<br><br>Cihazı el ile sağlamak için bir cihaz bağlantı dizesi sağlayacağınızı bildirir. |
| **ManualX509** | Yok | **Anahtar parametresi**. Hiçbir sağlama türü belirtilmemişse, bir bağlantı dizesiyle el ile sağlama varsayılan değerdir.<br><br>Cihazı el ile sağlamak için bir kimlik sertifikası ve özel anahtar sağlayacağınızı bildirir.
| **DpsTpm** | Yok | **Anahtar parametresi**. Hiçbir sağlama türü belirtilmemişse, bir bağlantı dizesiyle el ile sağlama varsayılan değerdir.<br><br>Bir cihaz sağlama hizmeti (DPS) kapsam KIMLIĞI ve bu cihazın, DPS aracılığıyla sağlamak üzere cihazınızın kayıt KIMLIĞI sağlayacağınızı bildirir.  |
| **DpsSymmetricKey** | Yok | **Anahtar parametresi**. Hiçbir sağlama türü belirtilmemişse, bir bağlantı dizesiyle el ile sağlama varsayılan değerdir.<br><br>Bir cihaz sağlama hizmeti (DPS) kapsam KIMLIĞI ve bir cihaz kayıt KIMLIĞININ, DPS aracılığıyla sağlaması için, kanıtlama için bir simetrik anahtarla birlikte sağlayacağınızı bildirir. |
| **DpsX509** | Yok | **Anahtar parametresi**. Hiçbir sağlama türü belirtilmemişse, bir bağlantı dizesiyle el ile sağlama varsayılan değerdir.<br><br>Bir cihaz sağlama hizmeti (DPS) kapsam KIMLIĞI ve bir X. 509.440 kimlik sertifikası ve kanıtlama için özel anahtarla birlikte, DPS aracılığıyla sağlamak üzere cihazınızın kayıt KIMLIĞI sağlayacağınızı bildirir.  |
| **DeviceConnectionString** | Tek tırnak içinde bir IoT Hub kayıtlı IoT Edge cihazdan bağlantı dizesi | Bir bağlantı dizesiyle el ile sağlama için **gereklidir** . Betik parametrelerinde bir bağlantı dizesi sağlamazsanız sizden bir tane istenir. |
| **IotHubHostName** | Bir cihazın bağlandığı IoT Hub 'ın ana bilgisayar adı. | X. 509.440 sertifikaları ile el ile sağlama için **gereklidir** . *{Hub Name}. Azure-Devices.net* biçimini alır. |
| **DeviceID** | IoT Hub içindeki kayıtlı bir cihaz kimliğinden cihaz KIMLIĞI. | X. 509.440 sertifikaları ile el ile sağlama için **gereklidir** . |
| **KML** | IoT Hub ilişkili cihaz sağlama hizmeti örneğinden bir kapsam KIMLIĞI. | DPS sağlaması için **gereklidir** . Komut dosyası parametrelerinde bir kapsam kimliği sağlamazsanız sizden bir kapsam kimliği belirtmeniz gerekir. |
| **RegistrationId** | Cihazınız tarafından oluşturulan bir kayıt KIMLIĞI | TPM veya simetrik anahtar kanıtlama kullanılıyorsa, DPS sağlaması için **gereklidir** . X. 509.440 sertifika kanıtlama kullanılıyorsa **Isteğe bağlıdır** . |
| **X509IdentityCertificate** | Cihazdaki X. 509.440 cihaz kimliği sertifikasının URI yolu. | X. 509.440 sertifika kanıtlama kullanılıyorsa el ile veya DPS sağlama için **gereklidir** . |
| **X509IdentityPrivateKey** | Cihazdaki X. 509.440 cihaz kimliği sertifika anahtarının URI yolu. | X. 509.440 sertifika kanıtlama kullanılıyorsa el ile veya DPS sağlama için **gereklidir** . |
| **SymmetricKey** | DPS kullanılırken IoT Edge cihaz kimliğini sağlamak için kullanılan simetrik anahtar | Simetrik anahtar kanıtlama kullanılıyorsa, DPS sağlaması için **gereklidir** . |
| **ContainerOs** | **Windows** veya **Linux** | Bir kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IoT Edge, yüklemeye dahil edilen Moby kapsayıcı altyapısını kullanır. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı altyapısı yüklemeniz gerekir. |
| **DeviceCACertificate** | Cihazdaki X. 509.952 cihaz CA sertifikasının URI yolu. | Ayrıca, dosyasında da yapılandırılabilir `C:\ProgramData\iotedge\config.yaml` . Daha fazla bilgi için bkz. [IoT Edge cihazda sertifikaları yönetme](how-to-manage-device-certificates.md). |
| **DeviceCAPrivateKey** | Cihazdaki X. 509.440 cihaz CA özel anahtarının URI yolu. | Ayrıca, dosyasında da yapılandırılabilir `C:\ProgramData\iotedge\config.yaml` . Daha fazla bilgi için bkz. [IoT Edge cihazda sertifikaları yönetme](how-to-manage-device-certificates.md). |
| **Invokewebrequestparameters** | Parametrelerin ve değerlerin Hashtable 'ı | Yükleme sırasında bazı Web istekleri yapılır. Bu Web isteklerinin parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Dil** | IoT Edge aracı görüntüsü URI 'SI | Varsayılan olarak, yeni bir IoT Edge yüklemesi IoT Edge Aracısı görüntüsü için en son kayan etiketi kullanır. Görüntü sürümü için belirli bir etiket ayarlamak veya kendi aracı görüntünüzü sağlamak için bu parametreyi kullanın. Daha fazla bilgi için bkz. [IoT Edge etiketlerini anlama](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Kullanıcı adı** | Kapsayıcı kayıt defteri Kullanıcı adı | Bu parametreyi yalnızca,-Tımage parametresini özel bir kayıt defterindeki bir kapsayıcıya ayarlarsanız kullanın. Kayıt defterine erişimi olan bir Kullanıcı adı belirtin. |
| **Parola** | Güvenli parola dizesi | Bu parametreyi yalnızca,-Tımage parametresini özel bir kayıt defterindeki bir kapsayıcıya ayarlarsanız kullanın. Kayıt defterine erişmek için parolayı girin. |

## <a name="update-iotedge"></a>Update-IoTEdge

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** veya **Linux** | Bir kapsayıcı işletim sistemi belirtilmemişse, Windows varsayılan değerdir. Windows kapsayıcıları için, yüklemeye bir kapsayıcı altyapısı dahil edilir. Linux kapsayıcıları için yüklemeyi başlatmadan önce bir kapsayıcı altyapısı yüklemeniz gerekir. |
| **Proxy** | Proxy URL 'SI | Cihazınızın internet 'e erişmek için bir proxy sunucusu üzerinden gitmesi gerekiyorsa bu parametreyi ekleyin. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Invokewebrequestparameters** | Parametrelerin ve değerlerin Hashtable 'ı | Yükleme sırasında bazı Web istekleri yapılır. Bu Web isteklerinin parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak için yararlıdır. Daha fazla bilgi için bkz. [bir IoT Edge cihazını bir ara sunucu üzerinden iletişim kurmak Için yapılandırma](how-to-configure-proxy-support.md). |
| **Offlineınstallationpath** | Dizin yolu | Bu parametre dahil ise, yükleyici, yükleme için gereken IoT Edge cab ve VC çalışma zamanı MSI dosyaları için listelenen dizini kontrol eder. Dizinde bulunmayan tüm dosyalar indirilir. Her iki dosya da dizinde ise, internet bağlantısı olmadan IoT Edge yükleyebilirsiniz. Belirli bir sürümü kullanmak için bu parametreyi de kullanabilirsiniz. |
| **Restartifgerekliyse** | yok | Bu bayrak dağıtım betiğinin, gerekirse, makineyi sormadan yeniden başlatmasını sağlar. |

## <a name="uninstall-iotedge"></a>Uninstall-IoTEdge

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **Force** | yok | Bu bayrak, önceki kaldırma girişimi başarısız olursa kaldırma işlemini zorlar.
| **Restartifgerekliyse** | yok | Bu bayrak, gerekirse, kaldırma komut dosyasının makineyi sormadan yeniden başlatmasını sağlar. |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalede bu komutları kullanmayı öğrenin:

* [Windows için Azure IoT Edge yükleme veya kaldırma](how-to-install-iot-edge-windows-on-windows.md)
