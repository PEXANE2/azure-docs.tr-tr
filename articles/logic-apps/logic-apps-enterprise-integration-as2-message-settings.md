---
title: AS2 ileti ayarları
description: Enterprise Integration Pack Azure Logic Apps içindeki AS2 gönderme ve alma ayarları için başvuru kılavuzu
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: ad047a30b901d71604c775e9882b0f242f094638
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "74793036"
---
# <a name="reference-for-as2-message-settings-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack Azure Logic Apps içindeki AS2 ileti ayarları için başvuru

Bu başvuru, AS2 sözleşmesinin ticari iş ortakları arasında gönderilen ve alınan iletileri nasıl işleyeceğini belirlemek için ayarlayabileceğiniz özellikleri açıklar. Sizinle ileti alışverişi yapan iş ortağıyla anlaşmanıza bağlı olarak bu özellikleri ayarlayın.

<a name="AS2-incoming-messages"></a>

## <a name="as2-receive-settings"></a>AS2 alma ayarları

!["Ayarları al" ı seçin](./media/logic-apps-enterprise-integration-as2-message-settings/receive-settings.png)

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **İleti özelliklerini geçersiz kıl** | No | Özellik ayarlarınızdaki gelen iletilerde özellikleri geçersiz kılar. |
| **İleti imzalanmalıdır** | No | Tüm gelen iletilerin dijital olarak imzalanıp imzalanmayacağını belirtir. İmza gerekiyorsa, **sertifika** listesinden, iletilerdeki imzayı doğrulamak için mevcut bir konuk iş ortağı genel sertifikası seçin. Sertifikanız yoksa, sertifika [ekleme](../logic-apps/logic-apps-enterprise-integration-certificates.md)hakkında daha fazla bilgi edinin. |
| **İleti şifrelenmelidir** | No | Tüm gelen iletilerin şifrelenmesinin gerekip gerekmediğini belirtir. Şifrelenmemiş iletiler reddedilir. Şifreleme gerekiyorsa, **sertifika** listesinden, gelen iletilerin şifresini çözmek için var olan bir konak ortağı özel sertifikası seçin. Sertifikanız yoksa, sertifika [ekleme](../logic-apps/logic-apps-enterprise-integration-certificates.md)hakkında daha fazla bilgi edinin. |
| **İleti sıkıştırılmış olmalıdır** | No | Tüm gelen iletilerin sıkıştırılması gerekip gerekmediğini belirtir. Sıkıştırılmış olmayan iletiler reddedilir. |
| **Ileti KIMLIĞI yinelemelerine izin verme** | No | Yinelenen kimlikleri olan iletilere izin verilip verilmeyeceğini belirtir. Yinelenen kimliklere izin verirseniz, denetimler arasındaki gün sayısını seçin. Ayrıca, yinelenenleri askıya alıp almamayı seçebilirsiniz. |
| **MDN metni** | No | İleti göndericisine göndermek istediğiniz varsayılan ileti değerlendirmesi bildirimini (MDN) belirtir. |
| **MDN gönder** | No | Alınan iletiler için zaman uyumlu MDNs gönderileceğini belirtir.  |
| **İmzalı MDN gönder** | No | Alınan iletiler için imzalanmış MDNs gönderileceğini belirtir. İmzalama gerekirse, **MIC algoritması** listesinden, ileti imzalama için kullanılacak algoritmayı seçin. |
| **Zaman uyumsuz MDN gönder** | No | MDNs 'in zaman uyumsuz olarak gönderileceğini belirtir. Zaman uyumsuz MDNs ' yi seçerseniz, **URL** kutusunda, MDNs 'nin gönderileceği URL 'yi belirtin. |
||||

<a name="AS2-outgoing-messages"></a>

## <a name="as2-send-settings"></a>AS2 gönderme ayarları

!["Ayarları gönder" i seçin](./media/logic-apps-enterprise-integration-as2-message-settings/send-settings.png)

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **İleti imzalamayı etkinleştir** | No | Tüm giden iletilerin dijital olarak imzalanıp imzalanmayacağını belirtir. İmzalama gerekliyse şu değerleri seçin: <p>- **Imzalama algoritması** listesinden, ileti imzalama için kullanılacak algoritmayı seçin. <br>- **Sertifika** listesinden, iletileri imzalamak için mevcut bir ana bilgisayar ortağı özel sertifikası seçin. Sertifikanız yoksa, sertifika [ekleme](../logic-apps/logic-apps-enterprise-integration-certificates.md)hakkında daha fazla bilgi edinin. |
| **İleti şifrelemesini etkinleştir** | No | Tüm giden iletilerin şifrelenmesinin gerekip gerekmediğini belirtir. Şifrelemeye ihtiyacınız varsa şu değerleri seçin: <p>- **Şifreleme algoritması** listesinden, iletileri şifrelemek için kullanılacak Konuk iş ortağı ortak sertifika algoritmasını seçin. <br>- **Sertifika** listesinden, giden iletileri şifrelemek için mevcut bir konuk iş ortağı özel sertifikası seçin. Sertifikanız yoksa, sertifika [ekleme](../logic-apps/logic-apps-enterprise-integration-certificates.md)hakkında daha fazla bilgi edinin. |
| **İleti sıkıştırmayı etkinleştir** | No | Tüm giden iletilerin sıkıştırılıp sıkıştırılmayacağını belirtir. |
| **HTTP üst bilgilerinin katlamayı kaldır** | No | HTTP `content-type` üstbilgisini tek bir satıra koyar. |
| **MIME üstbilgisinde dosya adı ilet** | No | MIME üstbilgisine dosya adının eklenip eklenmeyeceğini belirtir. |
| **MDN iste** | No | Tüm giden iletiler için ileti değerlendirmesi bildirimleri (MDNs) alıp almamayacağını belirtir. |
| **İmzalı MDN iste** | No | Tüm giden iletiler için imzalanan MDNs alıp almadığını belirtir. İmzalama gerekirse, **MIC algoritması** listesinden, ileti imzalama için kullanılacak algoritmayı seçin. |
| **Zaman uyumsuz MDN iste** | No | MDNs 'nin zaman uyumsuz olarak verilip verilmeyeceğini belirtir. Zaman uyumsuz MDNs ' yi seçerseniz, **URL** kutusunda, MDNs 'nin gönderileceği URL 'yi belirtin. |
| **NRR 'YI etkinleştir** | No | Red olmayan giriş (NRR) isteyip istemediğinizi belirtir. Bu iletişim özniteliği, verilerin çözümlenmiş olarak alındığını kanıtlandı. |
| **SHA2 algoritması biçimi** | No | Giden AS2 iletileri veya MDN 'nin üst bilgilerinde oturum açmak için kullanılacak MıC algoritması biçimini belirtir |
||||

## <a name="next-steps"></a>Sonraki adımlar

[AS2 iletilerini paylaşma](../logic-apps/logic-apps-enterprise-integration-as2.md)
