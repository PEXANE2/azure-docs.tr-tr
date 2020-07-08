---
title: Azure Kinect oynatma API 'SI
description: Kayıttan yürütme API 'sini kullanarak bir kayıt dosyasını açmak için Azure Kinect algılayıcı SDK 'sını nasıl kullanacağınızı öğrenin.
author: xthexder
ms.author: jawirth
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, algılayıcı, SDK, derinlik, RGB, kayıt, kayıttan yürütme, Matroska, MKV
ms.openlocfilehash: fe403f314c1df415537d090433f34627eb1249e7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85277653"
---
# <a name="the-azure-kinect-playback-api"></a>Azure Kinect oynatma API 'SI

Algılayıcı SDK 'sı, cihaz verilerini bir Matroska (. MKV) dosyasına kaydetmek için bir API sağlar. Matroska kapsayıcı biçimi video izlemelerini, ıMU örneklerini ve cihaz ayarlama 'yı depolar. Kayıtlar, belirtilen [k4arecorder](record-sensor-streams-file.md) komut satırı yardımcı programı kullanılarak oluşturulabilir. Kayıtlar Ayrıca kayıt API 'SI kullanılarak özelleştirilebilir ve doğrudan kaydedilebilir.

Kayıt API 'SI hakkında daha fazla bilgi için bkz [`k4a_record_create()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gae14f4181e9688e710d1c80b215413831.html#gae14f4181e9688e710d1c80b215413831) ..

Matroska dosya biçimi belirtimleri hakkında daha fazla bilgi için bkz. [kayıt dosyası biçimi](record-file-format.md) sayfası.

## <a name="use-the-playback-api"></a>Kayıttan yürütme API 'sini kullanma

Kayıt dosyaları, kayıttan yürütme API 'SI kullanılarak açılabilir. Kayıttan yürütme API 'SI, algılayıcı SDK 'sının geri kalanı ile aynı biçimdeki algılayıcı verilerine erişim sağlar.

### <a name="open-a-record-file"></a>Bir kayıt dosyası açın

Aşağıdaki örnekte, kullanarak bir kayıt açıyoruz [`k4a_playback_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gacb254ac941b2ab3c202ca68f4537f368.html#gacb254ac941b2ab3c202ca68f4537f368) , kayıt uzunluğunu yazdırdık ve sonra dosyayı ile kapattık [`k4a_playback_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga76f415f2076f1c8c544e094a649306ff.html#ga76f415f2076f1c8c544e094a649306ff) .

```C
k4a_playback_t playback_handle = NULL;
if (k4a_playback_open("recording.mkv", &playback_handle) != K4A_RESULT_SUCCEEDED)
{
    printf("Failed to open recording\n");
    return 1;
}

uint64_t recording_length = k4a_playback_get_last_timestamp_usec(playback_handle);
printf("Recording is %lld seconds long\n", recording_length / 1000000);

k4a_playback_close(playback_handle);
```

### <a name="read-captures"></a>Yakalamaları okuyun

Dosya açıldıktan sonra kayıttan yakalamaları okumaya başlayabiliriz. Bu sonraki örnek dosyadaki yakalamalarından her birini okur.

```C
k4a_capture_t capture = NULL;
k4a_stream_result_t result = K4A_STREAM_RESULT_SUCCEEDED;
while (result == K4A_STREAM_RESULT_SUCCEEDED)
{
    result = k4a_playback_get_next_capture(playback_handle, &capture);
    if (result == K4A_STREAM_RESULT_SUCCEEDED)
    {
        // Process capture here
        k4a_capture_release(capture);
    }
    else if (result == K4A_STREAM_RESULT_EOF)
    {
        // End of file reached
        break;
    }
}
if (result == K4A_STREAM_RESULT_FAILED)
{
    printf("Failed to read entire recording\n");
    return 1;
}
```

### <a name="seek-within-a-recording"></a>Kayıt içinde ara

Dosyanın sonuna ulaştıktan sonra geri dönüp yeniden okumak isteyebilirsiniz. Bu işlem, ile geriye doğru okunarak yapılabilir [`k4a_playback_get_previous_capture()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga54732e3aa0717e1ca4eb76ee385e878c.html#ga54732e3aa0717e1ca4eb76ee385e878c) , ancak kaydın uzunluğuna bağlı olarak çok yavaş olabilir.
Bunun yerine, bu [`k4a_playback_seek_timestamp()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaea748994a121543bd77f90417cf428f6.html#gaea748994a121543bd77f90417cf428f6) işlevi, dosyadaki belirli bir noktaya gitmek için kullanabiliriz.

Bu örnekte, dosyadaki çeşitli noktaları aramak için mikrosaniye cinsinden zaman damgaları belirttik.

```C
// Seek to the beginning of the file
if (k4a_playback_seek_timestamp(playback_handle, 0, K4A_PLAYBACK_SEEK_BEGIN) != K4A_RESULT_SUCCEEDED)
{
    return 1;
}

// Seek to the end of the file
if (k4a_playback_seek_timestamp(playback_handle, 0, K4A_PLAYBACK_SEEK_END) != K4A_RESULT_SUCCEEDED)
{
    return 1;
}

// Seek to 10 seconds from the start
if (k4a_playback_seek_timestamp(playback_handle, 10 * 1000000, K4A_PLAYBACK_SEEK_BEGIN) != K4A_RESULT_SUCCEEDED)
{
    return 1;
}

// Seek to 10 seconds from the end
if (k4a_playback_seek_timestamp(playback_handle, -10 * 1000000, K4A_PLAYBACK_SEEK_END) != K4A_RESULT_SUCCEEDED)
{
    return 1;
}
```

### <a name="read-tag-information"></a>Etiket bilgisini oku

Kayıtlar, cihaz seri numarası ve bellenim sürümleri gibi çeşitli meta verileri de içerebilir. Bu meta veriler, işlevi kullanılarak erişilebilen kayıt etiketlerinde depolanır [`k4a_playback_get_tag()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga320f966fc89b4ba0d758f787f70d5143.html#ga320f966fc89b4ba0d758f787f70d5143) .

```C
// Print the serial number of the device used to record
char serial_number[256];
size_t serial_number_size = 256;
k4a_buffer_result_t buffer_result = k4a_playback_get_tag(playback_handle, "K4A_DEVICE_SERIAL_NUMBER", &serial_number, &serial_number_size);
if (buffer_result == K4A_BUFFER_RESULT_SUCCEEDED)
{
    printf("Device serial number: %s\n", serial_number);
}
else if (buffer_result == K4A_BUFFER_RESULT_TOO_SMALL)
{
    printf("Device serial number too long.\n");
}
else
{
    printf("Tag does not exist. Device serial number was not recorded.\n");
}
```

### <a name="record-tag-list"></a>Etiket listesini Kaydet

Aşağıda bir kayıt dosyasına dahil olabilecek tüm varsayılan etiketlerin listesi verilmiştir. Bu değerlerin birçoğu yapının bir parçası olarak kullanılabilir [`k4a_record_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__record__configuration__t.html) ve [`k4a_playback_get_record_configuration()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaab54a85c1f1e98d170d009042b449255.html#gaab54a85c1f1e98d170d009042b449255) işleviyle okunabilir.

Bir etiket yoksa, varsayılan değere sahip olduğu varsayılır.

| Etiket Adı                     | Varsayılan değer      | [`k4a_record_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__record__configuration__t.html)Alanla | Notlar     |
|------------------------------|--------------------|--------------------------------------|----------------------------------------------------------------------------------------------------------------|
| `K4A_COLOR_MODE`             | DıŞıNA              | `color_format` / `color_resolution`  | Olası değerler: "OFF", "MJPG_1080P", "NV12_720P", "YUY2_720P" vb.                                      |
| `K4A_DEPTH_MODE`             | DıŞıNA              | `depth_mode` / `depth_track_enabled` | Olası değerler: "OFF," NFOV_UNBINNED "," PASSIVE_IR ", vb.                                                |
| `K4A_IR_MODE`                | DıŞıNA              | `depth_mode` / `ir_track_enabled`    | Olası değerler: "OFF", "ETKIN", "pasıf"                                                                    |
| `K4A_IMU_MODE`               | DıŞıNA              | `imu_track_enabled`                  | Olası değerler: "açık", "kapalı"                                                                                   |
| `K4A_CALIBRATION_FILE`       | "Üzerinde calibration.js" | YOK                                  | Bakýn[`k4a_device_get_raw_calibration()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga8c4e46642cee3115aeb0b33e2b43b24f.html#ga8c4e46642cee3115aeb0b33e2b43b24f) |
| `K4A_DEPTH_DELAY_NS`         | "0"                | `depth_delay_off_color_usec`         | Değer nanosaniye olarak depolandı, API mikrosaniye sağlar.                                                        |
| `K4A_WIRED_SYNC_MODE`        | ETIKETE       | `wired_sync_mode`                    | Olası değerler: "STANDALONE", "MASTER", "AST"                                                         |
| `K4A_SUBORDINATE_DELAY_NS`   | "0"                | `subordinate_delay_off_master_usec`  | Değer nanosaniye olarak depolandı, API mikrosaniye sağlar.                                                        |
| `K4A_COLOR_FIRMWARE_VERSION` | ""                 | YOK                                  | Cihaz rengi üretici yazılımı sürümü, örneğin "1. x. xx"                                                            |
| `K4A_DEPTH_FIRMWARE_VERSION` | ""                 | YOK                                  | Cihaz derinliği üretici yazılımı sürümü, örneğin "1. x. xx"                                                            |
| `K4A_DEVICE_SERIAL_NUMBER`   | ""                 | YOK                                  | Cihaz seri numarasını kaydetme                                                                                 |
| `K4A_START_OFFSET_NS`        | "0"                | `start_timestamp_offset_usec`        | Aşağıdaki [zaman damgası eşitleme](record-playback-api.md#timestamp-synchronization) bölümüne bakın.                       |
| `K4A_COLOR_TRACK`            | Hiçbiri               | YOK                                  | Bkz. [kayıt dosyası biçim-parçaları belirleme](record-file-format.md#identifying-tracks).                     |
| `K4A_DEPTH_TRACK`            | Hiçbiri               | YOK                                  | Bkz. [kayıt dosyası biçim-parçaları belirleme](record-file-format.md#identifying-tracks).                     |
| `K4A_IR_TRACK`               | Hiçbiri               | YOK                                  | Bkz. [kayıt dosyası biçim-parçaları belirleme](record-file-format.md#identifying-tracks).                     |
| `K4A_IMU_TRACK`              | Hiçbiri               | YOK                                  | Bkz. [kayıt dosyası biçim-parçaları belirleme](record-file-format.md#identifying-tracks).                     |

## <a name="timestamp-synchronization"></a>Zaman damgası eşitleme

Matroska biçimi kayıtların sıfır zaman damgasıyla başlaması gerekir. [Kameraları dışarıdan eşitlerken](record-external-synchronized-units.md), her cihazdan alınan ilk zaman damgası sıfır dışında olabilir.

Kayıt ve kayıttan yürütme arasındaki cihazlardan gelen özgün zaman damgalarını korumak için dosya, zaman damgalarına uygulanacak bir konum depolar.

`K4A_START_OFFSET_NS`Etiketi, dosyanın kaydedildikten sonra yeniden eşitlenmesi için zaman damgası uzaklığı belirtmek için kullanılır. Bu zaman damgası, özgün cihaz zaman damgalarını yeniden oluşturmak için dosyadaki her bir zaman damgasına eklenebilir.

Başlangıç boşluğu yapı içinde de kullanılabilir [`k4a_record_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__record__configuration__t.html) .
