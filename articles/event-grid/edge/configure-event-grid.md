---
title: Yapılandırma - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: IoT Edge'deki Event Grid'de yapılandırma.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 841b5092775353bbe3340dbbd55610026f998a15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76846479"
---
# <a name="event-grid-configuration"></a>Olay Izgara Yapılandırması

Olay Grid, ortam başına değiştirilebilen birçok yapılandırma sağlar. Aşağıdaki bölüm, tüm kullanılabilir seçeneklere ve bunların varsayılanlarına bir başvurudur.

## <a name="tls-configuration"></a>TLS yapılandırması

Genel olarak istemci kimlik doğrulaması hakkında bilgi edinmek için [Bkz. Güvenlik ve Kimlik Doğrulama.](security-authentication.md) Kullanım örnekleri [bu makalede](configure-api-protocol.md)bulunabilir.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
|`inbound__serverAuth__tlsPolicy`| Olay Izgara modülüNÜN TLS Politikası. Varsayılan değer yalnızca HTTPS'dir.
|`inbound__serverAuth__serverCert__source`| TLS yapılandırması için Event Grid Modülü tarafından kullanılan sunucu sertifikası kaynağı. Varsayılan değer IoT Edge'dir.

## <a name="incoming-client-authentication"></a>Gelen istemci kimlik doğrulaması

Genel olarak istemci kimlik doğrulaması hakkında bilgi edinmek için [Bkz. Güvenlik ve Kimlik Doğrulama.](security-authentication.md) Örnekler [bu makalede](configure-client-auth.md)bulunabilir.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
|`inbound__clientAuth__clientCert__enabled`| Sertifika tabanlı istemci kimlik doğrulamasını açmak/kapatmak için. Varsayılan değer doğrudur.
|`inbound__clientAuth__clientCert__source`| İstemci sertifikalarını doğrulamak için kaynak. Varsayılan değer IoT Edge'dir.
|`inbound__clientAuth__clientCert__allowUnknownCA`| Kendi imzalı istemci sertifikasına izin verme ilkesi. Varsayılan değer doğrudur.
|`inbound__clientAuth__sasKeys__enabled`| SAS tuşu tabanlı istemci kimlik doğrulamasını açmak/kapatmak için. Varsayılan değer kapalıdır.
|`inbound__clientAuth__sasKeys__key1`| Gelen istekleri doğrulamak için değerlerden biri.
|`inbound__clientAuth__sasKeys__key2`| Gelen istekleri doğrulamak için isteğe bağlı ikinci değer.

## <a name="outgoing-client-authentication"></a>Giden istemci kimlik doğrulaması
Genel olarak istemci kimlik doğrulaması hakkında bilgi edinmek için [Bkz. Güvenlik ve Kimlik Doğrulama.](security-authentication.md) Örnekler [bu makalede](configure-identity-auth.md)bulunabilir.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
|`outbound__clientAuth__clientCert__enabled`| Giden istekler için kimlik sertifikası eklemeyi açmak/kapatmak için. Varsayılan değer doğrudur.
|`outbound__clientAuth__clientCert__source`| Olay Izgara modülün giden sertifikasını almak için kaynak. Varsayılan değer IoT Edge'dir.

## <a name="webhook-event-handlers"></a>Webhook olay işleyicileri

Genel olarak istemci kimlik doğrulaması hakkında bilgi edinmek için [Bkz. Güvenlik ve Kimlik Doğrulama.](security-authentication.md) Örnekler [bu makalede](configure-webhook-subscriber-auth.md)bulunabilir.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
|`outbound__webhook__httpsOnly`| Yalnızca HTTPS abonelerine izin verilip verilmeyeceğini denetleme politikası. Varsayılan değer doğrudur (yalnızca HTTPS).
|`outbound__webhook__skipServerCertValidation`| Abonenin sertifikasını doğrulayıp doğrulamayacağını denetlemek için bayrak işaretle. Varsayılan değer doğrudur.
|`outbound__webhook__allowUnknownCA`| Kendi imzalı bir sertifikanın bir abone tarafından sunulup sunulamayacağını denetleme ilkesi. Varsayılan değer doğrudur. 

## <a name="delivery-and-retry"></a>Teslim ve yeniden deneme

Genel olarak bu özellik hakkında bilgi edinmek [için, Bkz. Teslim ve Yeniden Deneme.](delivery-retry.md)

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
| `broker__defaultMaxDeliveryAttempts` | Bir olayı teslim etmek için maksimum sayıda deneme. Varsayılan değer 30’dur.
| `broker__defaultEventTimeToLiveInSeconds` | Saniyeler içinde canlı yayın (TTL) süresi, teslim edilmezse bir olay bırakılır. Varsayılan değer **7200** saniye

## <a name="output-batching"></a>Çıkışı toplu işleme

Genel olarak bu özellik hakkında bilgi edinmek için [Teslim ve Çıktı toplu işlemi'ne](delivery-output-batching.md)bakın.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
| `api__deliveryPolicyLimits__maxBatchSizeInBytes` | Tonlama `ApproxBatchSizeInBytes` için izin verilen maksimum değer. Varsayılan değer. `1_058_576`
| `api__deliveryPolicyLimits__maxEventsPerBatch` | Tonlama `MaxEventsPerBatch` için izin verilen maksimum değer. Varsayılan değer. `50`
| `broker__defaultMaxBatchSizeInBytes` | Yalnızca `MaxEventsPerBatch` belirtildiğinde maksimum teslimat isteği boyutu. Varsayılan değer. `1_058_576`
| `broker__defaultMaxEventsPerBatch` | Yalnızca `MaxBatchSizeInBytes` belirtildiğinde toplu iş partisine eklenecek maksimum olay sayısı. Varsayılan değer. `10`

## <a name="metrics"></a>Ölçümler

IoT Edge'deki Event Grid ile ölçümleri kullanma hakkında bilgi edinmek için [izleme konularına ve aboneliklere](monitor-topics-subscriptions.md) bakın

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
| `metrics__reporterType` | Ölçümler için muhabir türü. Varsayılan `none` dır ve ölçümleri devre dışı kılabilir. Prometheus fuar biçiminde ölçümleri etkinleştirmek için `prometheus` ayar.