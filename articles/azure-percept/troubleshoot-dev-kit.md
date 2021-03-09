---
title: Azure Percept DK ve IoT Edge genel sorunlarını giderme
description: Taslak deneyimi sırasında bulunan daha yaygın sorunlardan bazıları için sorun giderme ipuçları alın
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: a6d099e8d267c9fe03e0bb676276e7a4ab8157ab
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102521535"
---
# <a name="azure-percept-dk-dev-kit-troubleshooting"></a>Azure Percept DK (geliştirme seti) sorunlarını giderme

Azure Percept DK için genel sorun giderme ipuçları için aşağıdaki kılavuza bakın.

## <a name="general-troubleshooting-commands"></a>Genel sorun giderme komutları

Bu komutları çalıştırmak için 
1. [Dev kit Wi-Fi AP](./quickstart-percept-dk-set-up.md) 'ye bağlanma
1. [Geliştirme Seti içine SSH](./how-to-ssh-into-percept-dk.md)
1. SSH terminalinde komutları girin

Daha fazla analiz için herhangi bir çıktıyı bir. txt dosyasına yönlendirmek için aşağıdaki sözdizimini kullanın:

```console
[command] > [file name].txt
```

Çıktıyı bir. txt dosyasına yönlendirdikten sonra SCP aracılığıyla dosyayı ana bilgisayara kopyalayın:

```console
scp [remote username]@[IP address]:[remote file path]/[file name].txt [local host file path]
```

```[local host file path]``` ,. txt dosyasını kopyalamak istediğiniz ana bilgisayar bilgisayarınızdaki konuma başvurur. ```[remote username]``` , [kurulum deneyimi](./quickstart-percept-dk-set-up.md)sırasında SSH Kullanıcı adı seçilir. OOBE sırasında bir SSH oturum açma ayarı yapmadıysanız, uzak kullanıcı adınız olur ```root``` .

Azure IoT Edge komutları hakkında daha fazla bilgi için bkz. [Azure IoT Edge cihaz sorun giderme belgeleri](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

|Kategori:         |Komutundaki                    |Çalışmayacaktır                  |
|------------------|----------------------------|---------------------------|
|İşletim Sistemi                |```cat /etc/os-release```         |Mariner görüntü sürümünü denetle |
|İşletim Sistemi                |```cat /etc/os-subrelease```      |Türev görüntü sürümünü denetle |
|İşletim Sistemi                |```cat /etc/adu-version```        |ADU sürümünü denetle |
|Sıcaklık       |```cat /sys/class/thermal/thermal_zone0/temp``` |devkit 'in sıcaklığını denetleyin |
|Wi-Fi             |```journalctl -u hostapd.service``` |SoftAP günlüklerine bakın|
|Wi-Fi             |```journalctl -u wpa_supplicant.service``` |Wi-Fi Services günlüklerine bakın |
|Wi-Fi             |```journalctl -u ztpd.service```  |Wi-Fi sıfır dokunma sağlama hizmeti günlüklerine bakın |
|Wi-Fi             |```journalctl -u systemd-networkd``` |Mariner ağ yığını günlüklerine bakın |
|Wi-Fi             |```/data/misc/wifi/hostapd_virtual.conf``` |WiFi erişim noktası yapılandırma ayrıntılarını denetle |
|OOBE              |```journalctl -u oobe -b```       |OOBE günlüklerini denetleyin |
|Telemetri         |```azure-device-health-id```      |benzersiz telemetri HW_ID bulun |
|Azure IoT Edge          |```sudo iotedge check```          |sık karşılaşılan sorunlar için yapılandırma ve bağlantı denetimlerini çalıştır |
|Azure IoT Edge          |```sudo iotedge logs [container name]``` |konuşma ve vizyon modülleri gibi kapsayıcı günlüklerini denetleyin |
|Azure IoT Edge          |```sudo iotedge support-bundle --since 1h``` |Son saatten modül günlüklerini, Azure IoT Edge Güvenlik Yöneticisi günlüklerini, kapsayıcı altyapısı günlüklerini, ```iotedge check``` JSON çıkışını ve diğer yararlı hata ayıklama bilgilerini toplayın |
|Azure IoT Edge          |```sudo journalctl -u iotedge -f``` |Azure IoT Edge Güvenlik Yöneticisi 'nin günlüklerini görüntüleme |
|Azure IoT Edge          |```sudo systemctl restart iotedge``` |Azure IoT Edge güvenlik cini 'nı yeniden başlatın |
|Azure IoT Edge          |```sudo iotedge list```           |dağıtılan Azure IoT Edge modüllerini listeleyin |
|Diğer             |```df [option] [file]```          |Belirtilen dosya sistemlerinde kullanılabilir/toplam alan bilgilerini görüntüle |
|Diğer             |```ip route get 1.1.1.1```        |Cihaz IP ve arabirim bilgilerini görüntüle |
|Diğer             |```ip route get 1.1.1.1 \| awk '{print $7}'``` <br> ```ifconfig [interface]``` |yalnızca cihaz IP adresini görüntüle |


```journalctl```Wi-Fi komutları aşağıdaki tek komutla birleştirilebilir:

```console
journalctl -u hostapd.service -u wpa_supplicant.service -u ztpd.service -u systemd-networkd -b
```

## <a name="docker-troubleshooting-commands"></a>Docker sorun giderme komutları

|Komutundaki                        |Çalışmayacaktır                  |
|--------------------------------|---------------------------|
|```docker ps``` |[Hangi kapsayıcıların çalıştığını gösterir](https://docs.docker.com/engine/reference/commandline/ps/) |
|```docker images``` |[cihazda hangi görüntülerin olduğunu gösterir](https://docs.docker.com/engine/reference/commandline/images/)|
|```docker rmi [image id] -f``` |[cihazdan bir görüntüyü siler](https://docs.docker.com/engine/reference/commandline/rmi/) |
|```docker logs -f edgeAgent``` <br> ```docker logs -f [module_name]``` |[Belirtilen modülün kapsayıcı günlüklerini alır](https://docs.docker.com/engine/reference/commandline/logs/) |
|```docker image prune``` |[Tüm sallaştırılmış görüntüleri kaldırır](https://docs.docker.com/engine/reference/commandline/image_prune/) |
|```watch docker ps``` <br> ```watch ifconfig [interface]``` |Docker kapsayıcısı indirme durumunu denetle |

## <a name="usb-updating"></a>USB güncelleştirme

|Hata:                                    |Çözüm:                                               |
|------------------------------------------|--------------------------------------------------------|
|UıUU aracılığıyla USB Flash sırasında LIBUSB_ERROR_XXX |Bu hata, UıUU güncelleştirmesi sırasında USB bağlantı hatasının sonucudur. USB kablosu PC veya PE-10X üzerindeki USB bağlantı noktalarına düzgün bir şekilde bağlı değilse, bu formdan bir hata meydana gelir. USB kablosunun her iki ucunu da sökmeyi ve güvenli bir bağlantı sağlamak için kabloyu jiggze yapmayı deneyin. Bu neredeyse her zaman sorunu çözer. |

## <a name="azure-percept-dk-carrier-board-led-states"></a>Azure Percept DK taşıyıcı panosu LED durumları

Taşıyıcı Pano Muhafazası üzerinde üç küçük LED vardır. 1. LED 'in yanına bir bulut simgesi yazdırılır, LED 2 ' nin yanında bir Wi-Fi simgesi yazdırılır ve LED 3 ' ün yanında ünlem işareti simgesi yazdırılır. Her LED durumu hakkında bilgi için aşağıdaki tabloya bakın.

|GELIŞTIRMESINE             |Durum      |Açıklama                      |
|----------------|-----------|---------------------------------|
|LED 1 (IoT Hub) |Açık (Solid) |Cihaz bir IoT Hub bağlı. |
|LED 2 (Wi-Fi)   |Yavaş yanıp sönme |Cihaz, Wi-Fi kolay bağlantı tarafından yapılandırılmaya hazırlanıyor ve bir yapılandırıcıda varlığını duyuruyor. |
|LED 2 (Wi-Fi)   |Hızlı yanıp sönen |Kimlik doğrulama başarılı oldu, cihaz ilişkilendirmesi devam ediyor. |
|LED 2 (Wi-Fi)   |Açık (Solid) |Kimlik doğrulama ve ilişkilendirme başarılı oldu; cihaz bir Wi-Fi ağa bağlı. |
|LED 3           |NA         |LED kullanımda değil. |


