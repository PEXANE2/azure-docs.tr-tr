---
title: AS2 ileti ayarları
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta AS2 gönderme ve alma ayarları için başvuru kılavuzu
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: ad047a30b901d71604c775e9882b0f242f094638
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74793036"
---
# <a name="reference-for-as2-message-settings-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta AS2 ileti ayarlarına başvuru

Bu başvuru, bir AS2 anlaşmasının ticaret ortakları arasında gönderilen ve alınan iletileri nasıl işleyeceğini belirtmek için ayarlayabildiğiniz özellikleri açıklar. Bu özellikleri, sizinle ileti alışverişinde bulunan iş ortağıyla yaptığınız anlaşmaya göre ayarlayın.

<a name="AS2-incoming-messages"></a>

## <a name="as2-receive-settings"></a>AS2 Alma ayarları

!["Ayarları Al" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-as2-message-settings/receive-settings.png)

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **İleti özelliklerini geçersiz kılma** | Hayır | Özellik ayarlarınızla gelen iletilerin özelliklerini geçersiz kılar. |
| **İleti imzalanmalıdır** | Hayır | Gelen tüm iletilerin dijital olarak imzalanması gerekip gerekmediğini belirtir. **Sertifika** listesinden imzalamayı istiyorsanız, iletilerin imzasını doğrulamak için varolan bir konuk ortak ortak sertifikası seçin. Sertifikanız yoksa, [sertifika ekleme](../logic-apps/logic-apps-enterprise-integration-certificates.md)hakkında daha fazla bilgi edinin. |
| **İleti şifrelenmeli** | Hayır | Gelen tüm iletilerin şifrelenip şifrelenmemesi gerektiğini belirtir. Şifrelenmemiş iletiler reddedilir. **Sertifika** listesinden şifreleme ye ihtiyacınız varsa, gelen iletilerin şifresini çözmek için varolan bir ana bilgisayar ortağı özel sertifikasını seçin. Sertifikanız yoksa, [sertifika ekleme](../logic-apps/logic-apps-enterprise-integration-certificates.md)hakkında daha fazla bilgi edinin. |
| **İleti sıkıştırılmalı** | Hayır | Gelen tüm iletilerin sıkıştırılıp sıkıştırılmayacağını belirtir. Sıkıştırılmamış iletiler reddedilir. |
| **İleti Kimliği yinelenenlerine izin verme** | Hayır | Yinelenen kimlikleri olan iletilere izin verip vermemeyi belirtir. Yinelenen bilgisayarlara izin vermiyorsanız, denetimler arasındaki gün sayısını seçin. Yinelenenleri askıya alıp almayacağınızı da seçebilirsiniz. |
| **MDN Metni** | Hayır | İleti gönderene gönderilmesini istediğiniz varsayılan ileti verme bildirimini (MDN) belirtir. |
| **MDN Gönder** | Hayır | Alınan iletiler için eşzamanlı MDN'ler gönderip göndermeyeceğini belirtir.  |
| **İmzalı MDN gönder** | Hayır | Alınan iletiler için imzalı MDN'lerin gönderip gönderilmeyeceğini belirtir. **MIC Algoritması** listesinden imzalamayı gerekiyorsa, iletileri imzalamak için kullanılacak algoritmayı seçin. |
| **Eşkron MDN gönder** | Hayır | MDN'lerin eş senkronize olarak gönderip gönderilmeyeceğini belirtir. **URL** kutusunda eşzamanlı MDN'ler seçerseniz, MDN'lerin nereye gönderileceğine dair URL'yi belirtin. |
||||

<a name="AS2-outgoing-messages"></a>

## <a name="as2-send-settings"></a>AS2 Gönderme ayarları

!["Ayarları Gönder"i seçin](./media/logic-apps-enterprise-integration-as2-message-settings/send-settings.png)

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **İleti imzalamayı etkinleştirme** | Hayır | Giden tüm iletilerin dijital olarak imzalanması gerekip gerekmediğini belirtir. İmzalama nız gerekiyorsa, şu değerleri seçin: <p>- **İmza Algoritması** listesinden, iletileri imzalamak için kullanılacak algoritmayı seçin. <br>- **Sertifika** listesinden, iletileri imzalamak için varolan ana bilgisayar ortağı özel sertifikasını seçin. Sertifikanız yoksa, [sertifika ekleme](../logic-apps/logic-apps-enterprise-integration-certificates.md)hakkında daha fazla bilgi edinin. |
| **İleti şifrelemesini etkinleştirme** | Hayır | Giden tüm iletilerin şifrelenip şifrelenmemesi gerektiğini belirtir. Şifreleme ye ihtiyacınız varsa, şu değerleri seçin: <p>- **Şifreleme Algoritması** listesinden, iletileri şifrelemek için kullanmak üzere konuk ortak ortak sertifika algoritmasını seçin. <br>- **Sertifika** listesinden, giden iletileri şifrelemek için varolan bir konuk ortak özel sertifikasını seçin. Sertifikanız yoksa, [sertifika ekleme](../logic-apps/logic-apps-enterprise-integration-certificates.md)hakkında daha fazla bilgi edinin. |
| **İleti sıkıştırmayı etkinleştirme** | Hayır | Giden tüm iletilerin sıkıştırılıp sıkıştırılmayacağını belirtir. |
| **HTTP üstbilgilerini aç** | Hayır | HTTP `content-type` üstbilgisini tek bir satıra koyar. |
| **MIME üstbilgisinde dosya adı iletme** | Hayır | MIME üstbilgisine dosya adının eklenip eklenmeyeceğini belirtir. |
| **İstek MDN** | Hayır | Giden tüm iletiler için ileti verme bildirimleri (MDN' ler) alınıp alınmayacağını belirtir. |
| **İstek imzalı MDN** | Hayır | Giden tüm iletiler için imzalı MDN'lerin alınıp alınmayacağını belirtir. **MIC Algoritması** listesinden imzalamayı gerekiyorsa, iletileri imzalamak için kullanılacak algoritmayı seçin. |
| **Asynchronous MDN isteyin** | Hayır | MDN'lerin eş senkronize olarak alınıp alınmayacağını belirtir. **URL** kutusunda eşzamanlı MDN'ler seçerseniz, MDN'lerin nereye gönderileceğine dair URL'yi belirtin. |
| **NRR'yi etkinleştir** | Hayır | Reddetme makbuzu (NRR) gerekip gerekmediğini belirtir. Bu iletişim özniteliği, verilerin ele alındığına dair kanıt sağlar. |
| **SHA2 Algoritma formatı** | Hayır | Giden AS2 iletileri veya MDN için üstbilgide oturum açma için kullanılacak MIC algoritma biçimini belirtir |
||||

## <a name="next-steps"></a>Sonraki adımlar

[AS2 iletilerini paylaşma](../logic-apps/logic-apps-enterprise-integration-as2.md)
