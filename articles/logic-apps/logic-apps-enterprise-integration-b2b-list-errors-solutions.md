---
title: B2B senaryolarında sık karşılaşılan hatalar ve sorunlar için çözümler
description: Azure Logic Apps'ta B2B senaryolarını sorun giderdiğinizde sık karşılaşılan hatalar alabildikleri ve karşılaşılan sorunlara çözüm bulun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/02/2017
ms.openlocfilehash: 38e281ce3d8117bff719b1bb572f09acbbb89669
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666695"
---
# <a name="b2b-errors-and-solutions-for-azure-logic-apps"></a>Azure Mantık Uygulamaları için B2B hataları ve çözümleri

Bu makale, Logic Apps B2B senaryolarında meydana gelebilecek hataları gidermenize yardımcı olur ve bu hataları düzeltmek için uygun eylemleri önerir.

## <a name="agreement-resolution"></a>Anlaşma çözümü

### <a name="no-agreement-found"></a>Anlaşma bulunamadı 

|   |   |  
|---|---|
| Hata açıklaması | Anlaşma Çözüm Parametreleri ile anlaşma bulunamadı. | 
| Kullanıcı eylemi | Anlaşma, kararlaştırılan iş kimlikleri ile entegrasyon hesabına eklenmelidir. </br>İş kimlikleri giriş iletisi kimliklerle eşleşmelidir. |  
|   |   |

### <a name="no-agreement-found-with-identities"></a>Kimliklerle anlaşma bulunamadı

|   |   | 
|---|---|
| Hata açıklaması | Kimliklerle anlaşma bulunamadı: 'AS2Identity'::'Partner1' ve 'AS2Identity':'Partner3' | 
| Kullanıcı eylemi | Geçersiz AS2-From veya AS2-Anlaşma için yapılandırılan. </br>AS2 iletisinin "AS2-From" veya "AS2-To" üstbilgilerini veya AS2 ileti üstbilgindeki AS2 işlemi işlemi' ndeki as2 işlisi işlemianlaçısınızı anlaşma konfigürasyonlarıyla eşleştirme anlaşmasını sızdır. |
|   |   |     

## <a name="as2"></a>AS2

### <a name="missing-as2-message-headers"></a>Eksik AS2 ileti üstbilgisi  

|   |   |  
|---|---|
| Hata açıklaması | Geçersiz AS2 üstbilgi. "AS2-To" veya "AS2-From" üstbilgilerinden biri boştur. | 
| Kullanıcı eylemi | AS2-From veya AS2-To veya her iki üstbilgi içermeyen bir AS2 iletisi alındı. </br> AS2 iletisi AS2-From ve AS2-To üstbilgilerini kontrol edin ve anlaşma yapılandırmasına göre düzeltin. |
|  |  | 

### <a name="missing-as2-message-body-and-headers"></a>Eksik AS2 ileti gövdesi ve üstbilgi    

|   |   |  
|---|---|
| Hata açıklaması | İstek içeriği boş veya boştur. | 
| Kullanıcı eylemi | İleti gövdesini içermeyen bir AS2 iletisi alındı. |
|  |  | 

### <a name="as2-message-decryption-failure"></a>AS2 ileti şifre çözme hatası

|   |   | 
|---|---|
| Hata açıklaması |  [işlendi/Hata: şifre çözme-başarısız] | 
| Kullanıcı eylemi | İş @base64ToBinary ortağına göndermeden önce AS2Message'a ekleyin. |
|||

Örnek:

```json
"HTTP": {
   "inputs": {
   "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
   "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
   "method": "POST",
   "uri": "xxxxx.xxx"
},
``` 

### <a name="mdn-decryption-failure"></a>MDN şifre çözme hatası

|   |   | 
|---|---|
| Hata açıklaması |  [işlendi/Hata: şifre çözme-başarısız] | 
| Kullanıcı eylemi | İş @base64ToBinary ortağına göndermeden önce MDN'ye ekleyin. | 
|||

Örnek:

```json
"Response": {
   "inputs": {
   "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
   "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
   "statusCode": 200
},               
``` 

### <a name="missing-signing-certificate"></a>Eksik imza sertifikası

|   |   |  
|---|---|
| Hata açıklaması| İmzalama Sertifikası AS2 partisi için yapılandırılmamıştır. </br>AS2-Gönderen: partner1 AS2-To: partner2 | 
| Kullanıcı eylemi | AS2 anlaşması ayarlarını imza için doğru sertifikayla yapılandırın. |
|  |  | 

## <a name="x12-and-edifact"></a>X12 ve EDIFACT

### <a name="leading-or-trailing-space-found"></a>Bulunan satır aralığı veya öncü alan    
    
|   |   | 
|---|---|
| Hata açıklaması | Ayrışma sırasında karşılaşılan hata. '987654' kimliğiyle (grupsuz) kavşakta yer alan '123456' kimlikli EDIFACT işlem kümesi, gönderen kimliği 'Ortak1', alıcı kimliği 'Partner2' aşağıdaki hatalarla askıya alınmıştır: <p>"Önde gelen Trailing ayırıcı bulundu" |
| Kullanıcı eylemi | Anlaşma ayarları, satır aralığı ve izleme alanı sağlayacak şekilde yapılandırılacak. </br>Satır aralığına ve öncü alana izin verecek şekilde anlaşma ayarlarını edin. |
|   |   |

![alan izin](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="duplicate-check-has-enabled-in-the-agreement"></a>Yinelenen denetim sözleşmede etkin

|   |   | 
|---|---| 
| Hata açıklaması | Yinelenen Kontrol Numarası |
| Kullanıcı eylemi | Bu hata, alınan iletinin yinelenen denetim numaralarına sahip olduğunu gösterir. </br>Denetim numarasını düzeltin ve iletiyi yeniden gönderin. |
|   |   |

### <a name="missing-schema-in-the-agreement"></a>Anlaşmada eksik şema

|   |   | 
|---|---| 
| Hata açıklaması | Ayrışma sırasında karşılaşılan hata. '564220001' kimlikli fonksiyonel grupta yer alan '564220001' kimlikli X12 işlem seti, '000056422' kimliği ile, gönderen kimliği '12345678', alıcı kimliği '87654321' ile değiştirilme de aşağıdaki hatalarla askıya alınmıştır: <p>"İleti bilinmeyen bir belge türüne sahiptir ve anlaşmada yapılandırılan mevcut şemaların hiçbirine çözümlenmemiştir." |
| Kullanıcı eylemi | Anlaşma ayarlarında şema yapılandırın.  |
|   |   |

### <a name="incorrect-schema-in-the-agreement"></a>Anlaşmada yanlış şema

|   |   | 
|---|---| 
| Hata açıklaması | İleti bilinmeyen bir belge türüne sahiptir ve anlaşmada yapılandırılan varolan şemaların hiçbirine çözümlenmedi. |
| Kullanıcı eylemi | Anlaşma ayarlarında doğru şema yapılandırın. |
|   |   |

## <a name="flat-file"></a>Düz dosya

### <a name="input-message-with-no-body"></a>Gövdesiz giriş iletisi

|   |   | 
|---|---|
| Hata açıklaması | Geçersiz Şablon. '1' ve sütun '1902' sütununda 'Flat_File_Decoding' satırındaki eylemdeki şablon dil ifadelerini işleyemiyor: 'Gerekli özellik 'içerik' bir değer bekliyor ama geçersiz oldu. Yol ''.'. |
| Kullanıcı eylemi | Bu hata, giriş iletisinin gövde içermediğini gösterir. |
|   |   | 

