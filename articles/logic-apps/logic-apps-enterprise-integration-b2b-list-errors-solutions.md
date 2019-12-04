---
title: B2B senaryoları için hatalar ve çözümler
description: Azure Logic Apps B2B senaryolarına yönelik hataları ve çözümleri bulma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/02/2017
ms.openlocfilehash: 176bc3642f7c923b50cbf2f7a97096f88dc02817
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74790701"
---
# <a name="b2b-errors-and-solutions-for-azure-logic-apps"></a>Azure Logic Apps için B2B hataları ve çözümleri

Bu makale, Logic Apps B2B senaryolarında oluşabilecek hataların giderilmesine ve bu hataları düzeltmek için uygun eylemleri önermenize yardımcı olur.

## <a name="agreement-resolution"></a>Sözleşme çözümleme

### <a name="no-agreement-found"></a>Anlaşma bulunamadı 

|   |   |  
|---|---|
| Hata açıklaması | Anlaşma çözümleme parametreleriyle anlaşma bulunamadı. | 
| Kullanıcı eylemi | Anlaşma, kabul edilen iş kimlikleriyle tümleştirme hesabına eklenmelidir. </br>İş kimlikleri, giriş iletisi kimlikleriyle eşleşmelidir. |  
|   |   |

### <a name="no-agreement-found-with-identities"></a>Kimliklere sahip hiçbir anlaşma bulunamadı

|   |   | 
|---|---|
| Hata açıklaması | ' AS2Identity ':: ' Partner1 ' ve 'As2ıdentity ':: ' Partner3 ' kimliklerine sahip bir anlaşma bulunamadı | 
| Kullanıcı eylemi | Sözleşme için geçersiz AS2-from veya AS2-to yapılandırması. </br>AS2 iletisinin "AS2-from" veya "AS2-to" üst bilgilerini veya anlaşmayı, AS2 ileti başlıklarındaki AS2 kimliklerini sözleşme yapılandırmalarına uyacak şekilde düzeltin. |
|   |   |     

## <a name="as2"></a>AS2

### <a name="missing-as2-message-headers"></a>Eksik AS2 ileti üstbilgileri  

|   |   |  
|---|---|
| Hata açıklaması | Geçersiz AS2 üstbilgileri. "AS2-to" veya "AS2-from" başlıklarından biri boş. | 
| Kullanıcı eylemi | AS2-from veya AS2-to veya her iki üst bilgi içermeyen bir AS2 iletisi alındı. </br> AS2 Message AS2-from ve AS2-to üst bilgilerini denetleyip anlaşma yapılandırmasına göre düzeltin. |
|  |  | 

### <a name="missing-as2-message-body-and-headers"></a>Eksik AS2 ileti gövdesi ve başlıkları    

|   |   |  
|---|---|
| Hata açıklaması | İstek içeriği null veya boş. | 
| Kullanıcı eylemi | İleti gövdesini içermeyen bir AS2 iletisi alındı. |
|  |  | 

### <a name="as2-message-decryption-failure"></a>AS2 ileti şifre çözme hatası

|   |   | 
|---|---|
| Hata açıklaması |  [işlendi/hata: şifre çözme başarısız oldu] | 
| Kullanıcı eylemi | İş ortağına göndermeden önce AS2Message 'e @base64ToBinary ekleyin. |
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
| Hata açıklaması |  [işlendi/hata: şifre çözme başarısız oldu] | 
| Kullanıcı eylemi | İş ortağına göndermeden önce MDN 'ye @base64ToBinary ekleyin. | 
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

### <a name="missing-signing-certificate"></a>İmza sertifikası eksik

|   |   |  
|---|---|
| Hata açıklaması| Imzalama sertifikası AS2 partisi için yapılandırılmamış. </br>AS2-From: partner1 AS2-to: partner2 | 
| Kullanıcı eylemi | İmza için doğru sertifikayla AS2 anlaşma ayarlarını yapılandırın. |
|  |  | 

## <a name="x12-and-edifact"></a>X12 ve EDIOLGU

### <a name="leading-or-trailing-space-found"></a>Baştaki veya sondaki boşluk bulundu    
    
|   |   | 
|---|---|
| Hata açıklaması | Ayrıştırma sırasında hatayla karşılaşıldı. Kimliği ' 123456 ' olan EDIOLGU işlem kümesi, kimlik ' 987654 ' olan, ' Partner1 ' Gönderen KIMLIĞI, ' Partner2 ' alıcı KIMLIĞI, aşağıdaki hatalarla askıya alındı: <p>"Baştaki sonda ayırıcı bulundu" |
| Kullanıcı eylemi | Baştaki ve sondaki alana izin verecek şekilde yapılandırılacak anlaşma ayarları. </br>Önde gelen ve sondaki alana izin vermek için anlaşma ayarlarını düzenleyin. |
|   |   |

![alana izin ver](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="duplicate-check-has-enabled-in-the-agreement"></a>Sözleşmede yinelenen denetim etkinleştirildi

|   |   | 
|---|---| 
| Hata açıklaması | Yinelenen denetim numarası |
| Kullanıcı eylemi | Bu hata, alınan iletinin yinelenen denetim numaralarına sahip olduğunu gösterir. </br>Denetim numarasını düzeltin ve iletiyi yeniden gönderin. |
|   |   |

### <a name="missing-schema-in-the-agreement"></a>Sözleşmede şema eksik

|   |   | 
|---|---| 
| Hata açıklaması | Ayrıştırma sırasında hatayla karşılaşıldı. Kimliği ' 564220001 ' olan x12 işlem kümesi, kimliği ' 56422 ' olan işlevsel grupta yer alan ' 000056422 ' kimlikli değişim içinde, ' 12345678 ' Gönderen KIMLIĞI, alıcı KIMLIĞI ' 87654321 ' Şu hatalarla askıya alındı: <p>"İleti bilinmeyen bir belge türüne sahip ve anlaşmada yapılandırılmış mevcut şemalardan hiçbirine çözümlenmedi" |
| Kullanıcı eylemi | Sözleşme ayarları ' nda şemayı yapılandırın.  |
|   |   |

### <a name="incorrect-schema-in-the-agreement"></a>Sözleşmede yanlış şema

|   |   | 
|---|---| 
| Hata açıklaması | İleti, bilinmeyen bir belge türüne sahip ve anlaşmada yapılandırılmış mevcut şemalardan hiçbirine çözümlenmedi. |
| Kullanıcı eylemi | Anlaşma ayarlarında doğru şemayı yapılandırın. |
|   |   |

## <a name="flat-file"></a>Düz dosya

### <a name="input-message-with-no-body"></a>Gövde olmadan giriş iletisi

|   |   | 
|---|---|
| Hata açıklaması | Invalidtemplate. ' Flat_File_Decoding ' eyleminde, ' 1 ' satırı ve ' 1902 ' sütununda şablon dili ifadeleri işlenemiyor: ' gerekli ' content ' özelliği bir değer bekliyor ancak null değeri aldı. ' '. ' Yolu. |
| Kullanıcı eylemi | Bu hata giriş iletisinin bir gövde içermediğini gösterir. |
|   |   | 

