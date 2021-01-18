---
title: Sık karşılaşılan hatalar
description: Bu belge, panoda bulunan yaygın hatalar hakkında bilgi içerir
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/18/2021
ms.openlocfilehash: 5e12ca3bf626ae212f44fe0378ccb6649738753c
ms.sourcegitcommit: 61d2b2211f3cc18f1be203c1bc12068fc678b584
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2021
ms.locfileid: "98562872"
---
# <a name="errors-in-the-connector-status"></a>Bağlayıcı durumundaki hatalar

Bağlayıcı durum listesinde, ıTSM bağlayıcınızı düzeltmenize yardımcı olabilecek hataları bulabilirsiniz.

## <a name="status-common-errors"></a>Durum yaygın hataları

Bu bölümde, durum listesinde bulabileceğiniz ortak hatayı ve nasıl çözümlebileceğinizi bulabilirsiniz:

*  **Hata**: "ServiceNow 'dan beklenmeyen yanıt durum kodu ile birlikte. Yanıt: {"import_set": "{import_set_id}", "staging_table": "x_mioms_microsoft_oms_incident", "sonuç": [{"transform_map": "OMS olayı", "Table": "olay", "durum": "hata", "error_message": "{hedef kayıt bulunamadı | Geçersiz tablo | "}" Hazırlama tablosu geçersiz

    **Neden**: Bu tür bir hata şu durumlarda ServiceNow 'dan döndürülür:
    * ServiceNow örneğinde dağıtılan özel bir betik olayların yoksayılmasına neden olur.
    * "OMS tümleştirici uygulaması" kodu ServiceNow tarafında değiştirilmiştir, örneğin, onBefore betiği.

    **Çözüm**: veri içeri aktarma yolundaki tüm özel betikleri veya kod değişikliklerini devre dışı bırakın.

* **Hata**: "{" Error ": {" Message ":" Işlem başarısız oldu "," ayrıntı ":" güvenlik KıSıTLAMALARı nedeniyle ACL özel durum güncelleştirmesi başarısız oldu "}"

    **Neden**: ServiceNow izinleri yanlış yapılandırma

    **Çözüm**: tüm rollerin [belirtilen](itsmc-connections-servicenow.md#install-the-user-app-and-create-the-user-role)şekilde doğru şekilde atandığını denetleyin.

* **Hata**: "Istek gönderilirken bir hata oluştu."

    **Neden**: "ServiceNow örneği kullanılamıyor"

    **Çözüm**: ServiceNow 'da örneğinizi kontrol edin veya kullanılamıyor olabilir.

* **Hata**: "Servicedeskhttpbadrequestexception: StatusCode = 429"

    **Neden**: ServiceNow hız sınırları çok düşük.

    **Çözüm**: [burada](https://docs.servicenow.com/bundle/london-application-development/page/integrate/inbound-rest/task/investigate-rate-limit-violations.html)açıklandığı gibi ServiceNow örneğindeki hız sınırlarını artırın veya iptal edin.

* **Hata**: "accesstoken ve refreshtoken geçersiz. Kullanıcının yeniden kimlik doğrulaması yapması gerekiyor. "

    **Neden**: yenileme belirtecinin zaman aşımına uğradı.

    **Çözüm**: ITSM Bağlayıcısı, [burada](./itsmc-resync-servicenow.md)açıklandığı gibi yeni bir yenileme belirteci oluşturacak şekilde eşitleyin.

* **Hata**: "{alertname} uyarısı için iş öğesi oluşturulamadı/güncelleştirilemedi. {Connectionıdentifier} ITSM Bağlayıcısı yok veya silindi. "

    **Neden**: ITSM Bağlayıcısı silindi.

    **Çözüm**: ITSM Bağlayıcısı silinmiş ancak yine de onu kullanmak için tanımlanmış ıtssm işlemleri var. Bu sorunu gidermek için 2 seçenek vardır:
  * Bu eylemi bul ve devre dışı bırak veya Sil
  * [Eylem grubunu](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts) mevcut bir ITSM Bağlayıcısı kullanacak şekilde yeniden yapılandırın.
  * [Yeni BIR ITSM Bağlayıcısı oluşturun](./itsmc-definition.md#create-an-itsm-connection) ve [Eylem grubunu kullanmak için yeniden yapılandırın](itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

## <a name="ui-common-errors"></a>UI ortak hataları

* **Hata**: "bir sorun oluştu. Bağlantı ayrıntıları alınamadı. "

    **Neden**: yeni oluşturulan ITSM Bağlayıcısı ilk eşitlemeyi henüz bitiremedi.

    **Çözüm**: yenı bir ITSM bağlayıcısı oluşturulduğunda ITSM Bağlayıcısı, çalışma öğesi şablonları ve iş ÖĞELERI gibi ITSM sisteminden bilgi eşitlemeye başlar. [Burada](./itsmc-resync-servicenow.md)açıklandığı gibi yeni bir yenileme belirteci oluşturmak için ITSM Bağlayıcısı eşitleyin.
