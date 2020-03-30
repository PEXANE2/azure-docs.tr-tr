---
title: Bir modülden IoT Edge aygıtının yerel depolama alanını kullanma - Azure IoT Edge | Microsoft Dokümanlar
description: IoT Edge aygıt yerel depolama modül erişimi sağlamak için ortam değişkenleri kullanın ve seçenekler oluşturun.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/18/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 079d5845917e63fadcf0466e5a744ed637d704ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75434537"
---
# <a name="give-modules-access-to-a-devices-local-storage"></a>Modüllerin bir cihazın yerel depolama alanına erişmesine izin ver

Azure depolama hizmetlerini kullanarak veya cihazınızın kapsayıcı depolama sında veri depolamanın yanı sıra, depolamayı özellikle çevrimdışı çalışırken daha iyi güvenilirlik için ana bilgisayar IoT Edge aygıtında da ayırabilirsiniz.

## <a name="link-module-storage-to-device-storage"></a>Modül depolamasını aygıt depolamasına bağlama

Modül depolamadan ana bilgisayar sistemindeki depolamaya bir bağlantı sağlamak için, modülünüz için kapsayıcıdaki bir depolama klasörünü işaret eden bir ortam değişkeni oluşturun. Ardından, bu depolama klasörünü ana bilgisayar makinesindeki bir klasöre bağlamak için oluşturma seçeneklerini kullanın.

Örneğin, IoT Edge hub'ın iletileri cihazınızın yerel depolama alanında depolamasını ve daha sonra geri almalarını sağlamak istiyorsanız, **Çalışma Zamanı Ayarları** bölümünde ki Azure portalındaki ortam değişkenlerini ve oluşturma seçeneklerini yapılandırabilirsiniz.

1. Hem IoT Edge hub'ı hem de IoT Edge aracısı için, modüldeki bir dizine işaret eden **storageFolder** adlı bir ortam değişkeni ekleyin.
1. Hem IoT Edge hub'ı hem de IoT Edge aracısı için, ana bilgisayar makinesindeki yerel bir dizini modüldeki bir dizine bağlamak için bağlamalar ekleyin. Örnek:

   ![Yerel depolama için seçenekler ve ortam değişkenleri oluşturma ekleme](./media/how-to-access-host-storage-from-module/offline-storage.png)

Veya yerel depolama alanını doğrudan dağıtım bildiriminde yapılandırabilirsiniz. Örnek:

```json
"systemModules": {
    "edgeAgent": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath>"]
                }
            }
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        }
    },
    "edgeHub": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath>"],
                    "PortBindings":{"5671/tcp":[{"HostPort":"5671"}],"8883/tcp":[{"HostPort":"8883"}],"443/tcp":[{"HostPort":"443"}]}}}
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        },
        "status": "running",
        "restartPolicy": "always"
    }
}
```

Ana `<HostStoragePath>` `<ModuleStoragePath>` bilgisayar ve modül depolama yolunudeğiştirin ve değiştirin; her iki değer de mutlak bir yol olmalıdır.

Örneğin, bir Linux sisteminde, `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` ana bilgisayar sisteminizdeki dizin **/etc/iotedge/depolama,** kapsayıcıdaki **/iotedge/depolama/** dizine eşlenir. Bir Windows sisteminde, başka `"Binds":["C:\\temp:C:\\contemp"]` bir örnek olarak, dizin **C:\\** ana bilgisayar sistemi üzerinde temp dizin C eşlenir: **\\** kapsayıcıda contemp.

Ayrıca, Linux cihazlarında, modülünüzün kullanıcı profilinin ana bilgisayar sistem dizinine yönelik gerekli okuma, yazma ve yürütme izinlerine sahip olduğundan emin olun. IoT Edge hub'ının iletileri cihazınızın yerel depolama alanında depolamasını etkinleştirme önceki örneğine dönersek, kullanıcı profili uID 1000'e izin vermeniz gerekir. (IoT Edge aracısı kök olarak çalışır, bu nedenle ek izinlere gerek duymaz.) Dizin sahibini değiştirmek ve ardından `chown` `chmod` izinleri değiştirmek gibi aşağıdakileri yapmak da dahil olmak üzere Linux sistemlerinde dizin izinlerini yönetmenin çeşitli yolları vardır:

```bash
sudo chown 1000 <HostStoragePath>
sudo chmod 700 <HostStoragePath>
```

[Docker dokümanlarından](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate)seçenekler oluşturma hakkında daha fazla ayrıntı bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir modülden ana bilgisayar depolamasına erişmenin ek bir örneği için, [IoT Edge'deki Azure Blob Depolama ile verileri en uçta](how-to-store-data-blob.md)depolayın'a bakın.
