---
title: B2B senaryolarında yaygın hatalara ve sorunlara yönelik çözümler
description: Azure Logic Apps 'de B2B senaryolarında sorun giderirken sık karşılaşılan hatalar ve sorunlar için çözümler bulun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/02/2017
ms.openlocfilehash: 6400cfe7e524dcc16e08c2bba7dfba4a62d00b2e
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232568"
---
# <a name="b2b-errors-and-solutions-for-azure-logic-apps"></a>Azure Logic Apps için B2B hataları ve çözümleri

Bu makale, Logic Apps B2B senaryolarında oluşabilecek hataların giderilmesine ve bu hataları düzeltmek için uygun eylemleri önermenize yardımcı olur.

## <a name="agreement-resolution"></a>Sözleşme çözümleme

### <a name="no-agreement-found"></a>Anlaşma bulunamadı 

**Hata açıklaması**: anlaşma çözümleme parametreleriyle anlaşma bulunamadı.

**Kullanıcı eylemi**: anlaşma, kabul edilen iş kimlikleriyle tümleştirme hesabına eklenmelidir. İş kimlikleri, giriş iletisi kimlikleriyle eşleşmelidir.

### <a name="no-agreement-found-with-identities"></a>Kimliklere sahip hiçbir anlaşma bulunamadı

**Hata açıklaması**: ' AS2Identity ':: ' Partner1 ' ve 'As2ıdentity ':: ' Partner3 ' kimliklerine sahip hiçbir anlaşma bulunamadı

**Kullanıcı eylemi**: anlaşma IÇIN geçersiz AS2-from veya AS2-to yapılandırması. AS2 iletisinin "AS2-from" veya "AS2-to" üst bilgilerini veya anlaşmayı, AS2 ileti başlıklarındaki AS2 kimliklerini sözleşme yapılandırmalarına uyacak şekilde düzeltin.

## <a name="as2"></a>AS2

### <a name="missing-as2-message-headers"></a>Eksik AS2 ileti üstbilgileri  

**Hata açıklaması**: geçersiz AS2 üstbilgileri. "AS2-to" veya "AS2-from" başlıklarından biri boş.

**Kullanıcı eylemi**: AS2-from veya AS2-to veya both üst bilgilerini IÇERMEYEN bir AS2 iletisi alındı. AS2 Message AS2-from ve AS2-to üst bilgilerini denetleyip anlaşma yapılandırmasına göre düzeltin.

### <a name="missing-as2-message-body-and-headers"></a>Eksik AS2 ileti gövdesi ve başlıkları    

**Hata açıklaması**: istek içeriği null veya boş.

**Kullanıcı eylemi**: ileti gövdesini IÇERMEYEN bir AS2 iletisi alındı.

### <a name="as2-message-decryption-failure"></a>AS2 ileti şifre çözme hatası

**Hata açıklaması**: [Işlendi/hata: şifre çözme başarısız oldu]

**Kullanıcı eylemi**: @base64ToBinary iş ortağına göndermeden önce AS2Message öğesine ekleyin.

Örneğin:

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

**Hata açıklaması**: [Işlendi/hata: şifre çözme başarısız oldu]

**Kullanıcı eylemi**: @base64ToBinary iş ortağına göndermeden önce MDN 'ye ekleyin.

Örneğin:

```json
"Response": {
   "inputs": {
   "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
   "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
   "statusCode": 200
},               
``` 

### <a name="missing-signing-certificate"></a>İmza sertifikası eksik

**Hata açıklaması**: IMZALAMA sertifikası AS2 partisi için yapılandırılmamış. AS2-From: partner1 AS2-to: partner2

**Kullanıcı eylemi**: imza için doğru sertifikayla AS2 anlaşma ayarlarını yapılandırın.

## <a name="x12-and-edifact"></a>X12 ve EDIOLGU

### <a name="leading-or-trailing-space-found"></a>Baştaki veya sondaki boşluk bulundu    

**Hata açıklaması**: ayrıştırma sırasında hatayla karşılaşıldı. Kimliği ' 123456 ' olan EDIOLGU işlem kümesi, kimlik ' 987654 ' olan, ' Partner1 ' Gönderen KIMLIĞI, ' Partner2 ' alıcı KIMLIĞI, şu hatalarla askıya alındı: "baştaki sonda ayırıcı bulundu"

**Kullanıcı eylemi**: baştaki ve sondaki alana izin verecek şekilde yapılandırılacak anlaşma ayarları. Önde gelen ve sondaki alana izin vermek için anlaşma ayarlarını düzenleyin.

![alana izin ver](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="duplicate-check-has-enabled-in-the-agreement"></a>Sözleşmede yinelenen denetim etkinleştirildi

**Hata açıklaması**: yinelenen denetim numarası

**Kullanıcı eylemi**: Bu hata, alınan iletinin yinelenen denetim numaralarına sahip olduğunu gösterir. Denetim numarasını düzeltin ve iletiyi yeniden gönderin.

### <a name="missing-schema-in-the-agreement"></a>Sözleşmede şema eksik

**Hata açıklaması**: ayrıştırma sırasında hatayla karşılaşıldı. Kimliği ' 564220001 ' olan x12 işlem kümesi, kimliği ' 56422 ' olan işlevsel grupta yer alan ' 000056422 ' kimlikli değişim içinde, ' 12345678 ' Gönderen KIMLIĞI, alıcı KIMLIĞI ' 87654321 ' Şu hatalarla askıya alındı: "ileti bilinmeyen bir belge türüne sahip ve anlaşmada yapılandırılan mevcut şemalardan hiçbirine çözümlenmedi"

**Kullanıcı eylemi**: anlaşma ayarlarında şemayı yapılandırın.

### <a name="incorrect-schema-in-the-agreement"></a>Sözleşmede yanlış şema

**Hata açıklaması**: ileti bilinmeyen bir belge türüne sahip ve anlaşmada yapılandırılmış mevcut şemalardan hiçbirine çözümlenmedi.

**Kullanıcı eylemi**: anlaşma ayarlarında doğru şemayı yapılandırın.

## <a name="flat-file"></a>Düz dosya

### <a name="input-message-with-no-body"></a>Gövde olmadan giriş iletisi

**Hata açıklaması**: ınvalidtemplate. ' Flat_File_Decoding ' eyleminde, ' 1 ' satırı ve ' 1902 ' sütununda şablon dili ifadeleri işlenemiyor: ' gerekli ' content ' özelliği bir değer bekliyor ancak null değeri aldı. ' '. ' Yolu.

**Kullanıcı eylemi**: Bu hata, giriş iletisinin bir gövde içermediğini gösterir.
