---
title: Yapılandırma-Azure Event Grid IoT Edge | Microsoft Docs
description: IoT Edge Event Grid yapılandırma.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 841b5092775353bbe3340dbbd55610026f998a15
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76846479"
---
# <a name="event-grid-configuration"></a>Event Grid yapılandırması

Event Grid, ortam başına değiştirilebilen birçok yapılandırma sağlar. Aşağıdaki bölüm, kullanılabilir tüm seçeneklere ve bunların varsayılanlarına yönelik bir başvurudur.

## <a name="tls-configuration"></a>TLS yapılandırması

İstemci kimlik doğrulaması hakkında genel bilgi edinmek için bkz. [güvenlik ve kimlik doğrulaması](security-authentication.md). Kullanım örnekleri [Bu makalede](configure-api-protocol.md)bulunabilir.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
|`inbound__serverAuth__tlsPolicy`| Event Grid modülünün TLS Ilkesi. Varsayılan değer yalnızca HTTPS 'dir.
|`inbound__serverAuth__serverCert__source`| Event Grid modülü tarafından TLS yapılandırması için kullanılan sunucu sertifikasının kaynağı. Varsayılan değer IoT Edge.

## <a name="incoming-client-authentication"></a>Gelen istemci kimlik doğrulaması

İstemci kimlik doğrulaması hakkında genel bilgi edinmek için bkz. [güvenlik ve kimlik doğrulaması](security-authentication.md). [Bu makalede](configure-client-auth.md)örnekleri bulabilirsiniz.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
|`inbound__clientAuth__clientCert__enabled`| Sertifika tabanlı istemci kimlik doğrulamasını açmak/devre dışı bırakmak için. Varsayılan değer true 'dur.
|`inbound__clientAuth__clientCert__source`| İstemci sertifikalarının doğrulanması için kaynak. Varsayılan değer IoT Edge.
|`inbound__clientAuth__clientCert__allowUnknownCA`| Otomatik olarak imzalanan bir istemci sertifikasına izin verme ilkesi. Varsayılan değer true 'dur.
|`inbound__clientAuth__sasKeys__enabled`| SAS anahtar tabanlı istemci kimlik doğrulamasını açmak/devre dışı bırakmak için. Varsayılan değer kapalı.
|`inbound__clientAuth__sasKeys__key1`| Gelen istekleri doğrulamak için değerlerden biri.
|`inbound__clientAuth__sasKeys__key2`| Gelen istekleri doğrulamak için isteğe bağlı ikinci değer.

## <a name="outgoing-client-authentication"></a>Giden istemci kimlik doğrulaması
İstemci kimlik doğrulaması hakkında genel bilgi edinmek için bkz. [güvenlik ve kimlik doğrulaması](security-authentication.md). [Bu makalede](configure-identity-auth.md)örnekleri bulabilirsiniz.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
|`outbound__clientAuth__clientCert__enabled`| Giden istekler için kimlik sertifikasını ekleme/kapatma. Varsayılan değer true 'dur.
|`outbound__clientAuth__clientCert__source`| Event Grid modülün giden sertifikasını alma kaynağı. Varsayılan değer IoT Edge.

## <a name="webhook-event-handlers"></a>Web kancası olay işleyicileri

İstemci kimlik doğrulaması hakkında genel bilgi edinmek için bkz. [güvenlik ve kimlik doğrulaması](security-authentication.md). [Bu makalede](configure-webhook-subscriber-auth.md)örnekleri bulabilirsiniz.

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
|`outbound__webhook__httpsOnly`| Yalnızca HTTPS abonelerine izin verilip verilmeyeceğini denetleyen ilke. Varsayılan değer true 'dur (yalnızca HTTPS).
|`outbound__webhook__skipServerCertValidation`| Abonenin sertifikasının doğrulanıp doğrulanmayacağını denetleyen bayrak. Varsayılan değer true 'dur.
|`outbound__webhook__allowUnknownCA`| Otomatik olarak imzalanan bir sertifikanın bir abone tarafından sunulup sunulmadığını denetleyen ilke. Varsayılan değer true 'dur. 

## <a name="delivery-and-retry"></a>Teslim ve yeniden deneme

Bu özellik hakkında genel bilgi edinmek için bkz. [teslim ve yeniden deneme](delivery-retry.md).

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
| `broker__defaultMaxDeliveryAttempts` | Bir olayı teslim etmeye yönelik deneme sayısı üst sınırı. Varsayılan değer 30’dur.
| `broker__defaultEventTimeToLiveInSeconds` | Saniyeler içinde, bir olayın teslim edilmeden önce bırakılma yaşam süresi (TTL). Varsayılan değer **7200** saniyedir

## <a name="output-batching"></a>Çıkışı toplu işleme

Bu özellik hakkında genel bilgi edinmek için bkz. [teslim ve çıkış toplu işleme](delivery-output-batching.md).

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
| `api__deliveryPolicyLimits__maxBatchSizeInBytes` | `ApproxBatchSizeInBytes` Düğme için izin verilen en büyük değer. Varsayılan değer `1_058_576`.
| `api__deliveryPolicyLimits__maxEventsPerBatch` | `MaxEventsPerBatch` Düğme için izin verilen en büyük değer. Varsayılan değer `50`.
| `broker__defaultMaxBatchSizeInBytes` | Yalnızca `MaxEventsPerBatch` belirtilen en fazla teslim isteği boyutu. Varsayılan değer `1_058_576`.
| `broker__defaultMaxEventsPerBatch` | Yalnızca `MaxBatchSizeInBytes` belirtildiğinde bir toplu işe eklenecek olay sayısı üst sınırı. Varsayılan değer `10`.

## <a name="metrics"></a>Ölçümler

IoT Edge Event Grid ile ölçümleri kullanma hakkında bilgi edinmek için bkz. [izleme konuları ve abonelikleri](monitor-topics-subscriptions.md)

| Özellik Adı | Açıklama |
| ---------------- | ------------ |
| `metrics__reporterType` | Ölçüm enpoint için Raporlayıcı türü. Varsayılan değer `none` , ölçümleri devre dışı bırakır. Prometheus Exposition biçimindeki ölçümleri etkinleştirmeye olanak sağlamak için `prometheus` ayarı.