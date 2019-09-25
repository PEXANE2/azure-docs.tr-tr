---
title: Notification Hubs güvenliği
description: Bu konuda Azure Bildirim Hub 'ları için güvenlik açıklanmaktadır.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 05/31/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 05/31/2019
ms.openlocfilehash: 753493100bbdb34255574656a47217560e2d321a
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213048"
---
# <a name="notification-hubs-security"></a>Notification Hubs güvenliği

## <a name="overview"></a>Genel Bakış

Bu konuda Azure Notification Hubs 'in güvenlik modeli açıklanmaktadır.

## <a name="shared-access-signature-security-sas"></a>Paylaşılan erişim Imzası güvenliği (SAS)

Notification Hubs SAS (paylaşılan erişim Imzası) adlı varlık düzeyinde bir güvenlik şeması uygular. Her kural, [güvenlik talepleri](#security-claims)bölümünde açıklandığı gibi bir ad, anahtar değer (paylaşılan gizlilik) ve bir haklar kümesi içerir. Bir Bildirim Hub 'ı oluştururken, otomatik olarak iki kural oluşturulur: bir **dinleme** haklarıyla (istemci uygulamanın kullandığı) ve biri **Tüm** haklara sahip (uygulamanın arka ucu tarafından kullanılır).

İstemci uygulamalarından kayıt yönetimi gerçekleştirirken, bildirimler aracılığıyla gönderilen bilgiler hassas değilse (örneğin, hava durumu güncelleştirmeleri), bir Bildirim Hub 'ına erişmenin yaygın bir yolu, kuralın anahtar değerini istemci uygulamasına yalnızca dinleme erişimine vermektir. kuralın anahtar değerine, uygulama arka ucuna tam erişim vermek için.

Uygulamalar, Windows Mağazası istemci uygulamalarına anahtar değerini katıştırmamalıdır, bunun yerine, istemci uygulamasının başlangıçta uygulama arka ucuna alabilmesi gerekir.

**Dinleme** erişimi olan anahtar, bir istemci uygulamanın herhangi bir etikete kaydolmaya izin verir. Uygulamanızın kayıtları belirli istemcilerle kısıtlanması gerekiyorsa (örneğin, Etiketler Kullanıcı kimliklerini temsil ediyorsa), uygulamanızın arka ucunuzun kayıtları gerçekleştirmesi gerekir. Daha fazla bilgi için bkz. [kayıt yönetimi](notification-hubs-push-notification-registration-management.md). Bu şekilde, istemci uygulamanın Notification Hubs doğrudan erişimine sahip olamayacağını unutmayın.

## <a name="security-claims"></a>Güvenlik talepleri

Diğer varlıklara benzer şekilde, üç güvenlik talebi için Bildirim Hub 'ı işlemlerine izin verilir: **Dinleyin**, **gönderin**ve **yönetin**.

| Talep   | Açıklama                                          | İzin verilen işlemler |
| ------- | ---------------------------------------------------- | ------------------ |
| Dinle  | Tek kayıt oluşturma/güncelleştirme, okuma ve silme | Kayıt oluştur/güncelleştir<br><br>Kaydı oku<br><br>Bir tanıtıcı için tüm kayıtları okuma<br><br>Kaydı Sil |
| Gönder    | Bildirim Hub 'ına ileti gönderme                | İleti gönder |
| Yönet  | Notification Hubs CRUDs (PNS kimlik bilgilerini güncelleştirme ve güvenlik anahtarları dahil) ve etiketlere göre kayıtları okuma |Bildirim Hub 'larını oluştur/güncelleştir/oku/Sil<br><br>Kayıtları etikete göre oku |

Notification Hubs, doğrudan Bildirim Hub 'ında yapılandırılmış paylaşılan anahtarlarla oluşturulan imza belirteçlerini kabul eder.

Birden fazla ad alanına bildirim göndermek mümkün değildir. Ad alanları, Bildirim Hub 'ları için mantıksal kapsayıcıdır ve bildirim göndermeye dahil değildir.
Ad alanı düzeyinde erişim ilkeleri (kimlik bilgileri), ad alanı düzeyindeki işlemler için kullanılabilir (örneğin, Bildirim Hub 'ları listeleme, Bildirim Hub 'ları oluşturma veya silme vb.) Yalnızca hub düzeyi erişim ilkeleri bildirim göndermenizi sağlar.
