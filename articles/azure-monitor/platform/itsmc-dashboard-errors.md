---
title: ISMC panosundaki bağlayıcı durumu hataları
description: BT Hizmet Yönetimi Bağlayıcısı panosunda bulunan yaygın hatalar hakkında bilgi edinin.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/18/2021
ms.openlocfilehash: d1ba698cd95a074c021aa351a98eb12fc8ae0fc3
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99492527"
---
# <a name="connector-status-errors-in-the-itsmc-dashboard"></a>ISMC panosundaki bağlayıcı durumu hataları

BT Hizmet Yönetimi Bağlayıcısı (ıSMC) panosunda, bağlayıcıdaki sorunları gidermenize yardımcı olabilecek hatalar sunulmaktadır.

Aşağıdaki bölümlerde, panonun bağlayıcı durumu bölümünde görünen yaygın hatalar ve bunları nasıl çözekullanabileceğiniz açıklanır.

## <a name="unexpected-response"></a>Beklenmeyen yanıt

**Hata**: "ServiceNow 'dan beklenmeyen yanıt durum kodu ile birlikte. Yanıt: {"import_set": "{import_set_id}", "staging_table": "x_mioms_microsoft_oms_incident", "sonuç": [{"transform_map": "OMS olayı", "Table": "olay", "durum": "hata", "error_message": "{hedef kayıt bulunamadı | Geçersiz tablo | "}" Hazırlama tablosu geçersiz

**Neden**: ServiceNow şu durumlarda bu hatayı döndürür:

* ServiceNow örneğinde dağıtılan özel bir betik olayların yoksayılmasına neden olur.
* ServiceNow tarafında "OMS tümleştirici uygulaması" kodu değiştirilmiştir (örneğin, `onBefore` komut dosyası aracılığıyla).

**Çözüm**: tüm özel betikleri veya kod değişikliklerini devre dışı bırakın.

## <a name="exception-update-failure"></a>Özel durum güncelleştirme hatası

**Hata**: "{" Error ": {" Message ":" Işlem başarısız oldu "," ayrıntı ":" güvenlik KıSıTLAMALARı nedeniyle ACL özel durum güncelleştirmesi başarısız oldu "}"

**Neden**: ServiceNow izinleri yanlış yapılandırılmış.

**Çözüm**: tüm rollerin [belirtilen](itsmc-connections-servicenow.md#install-the-user-app-and-create-the-user-role)şekilde doğru şekilde atandığını denetleyin.

## <a name="problem-sending-a-request"></a>İstek gönderilirken sorun oluştu

**Hata**: "Istek gönderilirken bir hata oluştu."

**Neden**: ServiceNow örneği kullanılamıyor.

**Çözüm**: ServiceNow içindeki örneğinizi denetleyin. Silinmiş veya kullanılamıyor olabilir.

## <a name="servicenow-rate-problem"></a>ServiceNow oranı sorunu

**Hata**: "Servicedeskhttpbadrequestexception: StatusCode = 429"

**Neden**: ServiceNow hız limitleri çok yüksek veya çok düşük.

**Çözüm**: [ServiceNow belgelerinde](https://docs.servicenow.com/bundle/london-application-development/page/integrate/inbound-rest/task/investigate-rate-limit-violations.html)açıklandığı şekilde ServiceNow örneğindeki hız sınırlarını artırın veya iptal edin.

## <a name="invalid-refresh-token"></a>Geçersiz yenileme belirteci

**Hata**: "accesstoken ve refreshtoken geçersiz. Kullanıcının yeniden kimlik doğrulaması yapması gerekiyor. "

**Neden**: yenileme belirtecinin zaman aşımına uğradı.

**Çözüm**: [eşitleme sorunlarını el ile çözme](./itsmc-resync-servicenow.md)bölümünde açıklandığı gibi, yeni bir yenileme belirteci oluşturmak için ısmc ' ı eşitleyin.

## <a name="missing-connector"></a>Eksik bağlayıcı

**Hata**: "{alertname} uyarısı için iş öğesi oluşturulamadı/güncelleştirilemedi. {Connectionıdentifier} ITSM Bağlayıcısı yok veya silindi. "

**Neden**: ısmc silindi.

**Çözüm**: ısmc silindi, ancak tanımlı BT hizmeti yönetimi (ISM) eylem grupları hala onunla ilişkili. Bu sorunu çözmek için üç seçenek vardır:

* Bu tür eylem gruplarını bulun ve devre dışı bırakın veya silin.
* Mevcut bir ıSMC örneğini kullanmak için [eylem gruplarını yeniden yapılandırın](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts) .
* [Yeni BIR ıSMC örneği oluşturun](./itsmc-definition.md#create-an-itsm-connection) ve [eylem gruplarını kullanmak üzere yeniden yapılandırın](itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

## <a name="lack-of-connection-details"></a>Bağlantı olmaması ayrıntıları

**Hata**: "bir sorun oluştu. Bağlantı ayrıntıları alınamadı. " Bir ıTSM eylem grubunu tanımladığınızda bu hata görüntülenir.

**Neden**: böyle bir hata, bu durumlardan birinde görünür:

* Yeni oluşturulan bir ITSM Bağlayıcısı örneği, ilk eşitlemeyi henüz bitiremedi.
* Bağlayıcı doğru tanımlanmadı.

**Çözüm**: 

* Yeni bir ıSMC örneği oluşturulduğunda, çalışma öğesi şablonları ve iş öğeleri gibi ıTSM sisteminden bilgi eşitlemeye başlar. [Yeni bir yenileme belirteci oluşturmak IÇIN ITSMC eşitlemesini](./itsmc-resync-servicenow.md)yapın.
* [Ismc 'daki bağlantı ayrıntılarınızı gözden geçirin](./itsmc-connections-servicenow.md#create-a-connection) ve ısmc 'ın başarıyla [eşitleme](./itsmc-resync-servicenow.md)yapabilir olup olmadığını denetleyin.
