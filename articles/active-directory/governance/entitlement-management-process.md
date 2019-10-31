---
title: Azure AD Yetkilendirme Yönetimi 'nde işlem ve e-posta bildirimleri isteme-Azure Active Directory
description: Erişim paketi için istek süreci ve Azure Active Directory yetkilendirme yönetimi 'nde e-posta bildirimleri gönderildiğinde öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/30/2019
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a1ce3b2cb72a7b57e556e09264cb5bd421eda0f
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73173748"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde işlem ve e-posta bildirimleri iste

Bir Kullanıcı bir erişim paketine istek gönderdiğinde, bu isteği sunmaya yönelik bir işlem başlatılır. Azure AD Yetkilendirme Yönetimi, işlem sırasında önemli olaylar gerçekleştiğinde onaylayanlara ve istek Lara e-posta bildirimleri de gönderir.

Bu makalede, istek işlemi ve gönderilen e-posta bildirimleri açıklanmaktadır.

## <a name="request-process"></a>İstek işlemi

Erişim paketine erişmesi gereken bir Kullanıcı, erişim isteği gönderebilir. İlkenin yapılandırmasına bağlı olarak istek için onay gerekebilir. Bir istek onaylandığında bir işlem, erişim paketindeki her kaynağa Kullanıcı erişimini atamaya başlar. Aşağıdaki diyagramda, işleme ve farklı durumlara genel bir bakış gösterilmektedir.

![Onay işlemi diyagramı](./media/entitlement-management-process/request-process.png)

| Eyalet | Açıklama |
| --- | --- |
| Gönderme | Kullanıcı bir istek gönderir. |
| Onay bekleniyor | Bir erişim paketinin ilkesi onay gerektiriyorsa, bir istek bekleyen onaya gider. |
| Süresi doldu | Onay isteği zaman aşımı içinde bir onaylayan isteği onaylamadıysanız, isteğin süresi dolar. Yeniden denemek için, kullanıcının isteklerini yeniden göndermek zorunda kalır. |
| Reddedildiğinde | Onaylayan bir isteği reddeder. |
| Onaylandı | Onaylayan bir isteği onaylar. |
| Sunma | Kullanıcıya erişim paketindeki tüm kaynaklara **erişim atanmamış.** Bu bir dış Kullanıcı ise, Kullanıcı henüz kaynak dizinine erişmemiş ve onay isteğini kabul etmiş olabilir. |
| Teslim Edildi | Kullanıcıya erişim paketindeki tüm kaynaklara erişim atandı. |
| Genişletilmiş erişim | İlkede uzantılara izin veriliyorsa Kullanıcı atamayı genişletti. |
| Erişim zaman aşımına uğradı | Kullanıcının erişim paketine erişiminin süresi doldu. Yeniden erişim sağlamak için kullanıcının bir istek göndermesi gerekir. |

## <a name="email-notifications"></a>E-posta bildirimleri

Bir onaylıyorsanız, bir erişim isteğini onaylamanız gerektiğinde ve bir erişim isteği tamamlandığında e-posta bildirimleri gönderilir. İstek sahibi olduğunuzda, isteğiniz durumunu belirten e-posta bildirimleri gönderilir. Aşağıdaki diyagramda, bu e-posta bildirimlerinin ne zaman gönderildiği gösterilmektedir.

![Yetkilendirme Yönetimi e-posta işlemi](./media/entitlement-management-process/email-notifications.png)

Aşağıdaki tabloda, bu e-posta bildirimlerinin her biri hakkında daha ayrıntılı bilgi verilmektedir.

| # | E-posta konusu | Gönderildiğinde | Gönderildiği yer |
| --- | --- | --- | --- |
| 1 | Eylem gerekli: [ *Date] tarihine* kadar [ *istek sahibi]* kaynağından *[erişim paketine]* erişim isteğini gözden geçirin | İstek sahibi bir erişim paketi isteği gönderdiğinde | Tüm onaylayanlar |
| 2 | Eylem gerekli: [ *Date] tarihine* kadar [ *istek sahibi]* kaynağından *[erişim paketine]* erişim isteğini gözden geçirin | Onay isteği zaman aşımından önceki X gün | Tüm onaylayanlar |
| 3 | Durum bildirimi: [ *istek sahibi]* için *[erişim paketi]* erişim isteğinin süresi doldu | Onaylayanlar istek süresi içinde bir erişim isteğini onaylamaz veya reddetmez | Sahibinin |
| 4 | Durum bildirimi: [ *istek sahibi]* *[erişim paketi]* için erişim isteği tamamlandı | İlk onaylayan bir erişim isteğini onayladığında veya reddettiğinde | Tüm onaylayanlar |
| 5 | *[Erişim paketi]* erişimi reddedildi | Bir istek sahibinin erişim paketine erişimi reddedildiğinde | Sahibinin |
| 6 | Artık *[erişim paketi]* erişiminiz var  | İstek sahibine erişim paketindeki her kaynağa erişim verildiğinde | Sahibinin |
| 7 | *[Erişim paketi]* erişiminizin süresi X gün içinde doluyor | İstek sahibinin erişim paketine erişiminin süresi dolmadan X gün önce | Sahibinin |
| 8 | *[Erişim paketi]* erişiminizin süresi doldu | İstek sahibinin bir erişim paketine erişimi sona erdiğinde | Sahibinin |

### <a name="access-request-emails"></a>Erişim isteği e-postaları

Bir istek sahibi onay gerektirecek şekilde yapılandırılmış bir erişim paketi için erişim isteği gönderdiğinde, ilkede yapılandırılan tüm onaylayanlar isteğin ayrıntılarına sahip bir e-posta bildirimi alır. Ayrıntılar, istekte bulunan kişinin adı, kuruluş, erişim başlangıç ve bitiş tarihi, iş gerekçe, isteğin gönderildiği zaman ve isteğin ne zaman dolacağını içerir. E-posta, onaylayanların erişim isteğini onaylayabildiği veya reddedebileceği bir bağlantı içerir. İstek sahibi bir erişim isteği gönderdiğinde onaylayana gönderilen örnek bir e-posta bildirimi aşağıda verilmiştir.

![Erişim isteği e-postasını gözden geçir](./media/entitlement-management-shared/email-approve-request.png)

### <a name="approved-or-denied-emails"></a>Onaylanan veya reddedilen e-postalar

İstek sahipleri, erişim istekleri onaylandığında veya erişim için kullanılabilir olduğunda veya erişim istekleri reddedildiğinde bilgilendirilir. Bir onaylayan, istek sahibi tarafından gönderilen bir erişim isteği aldığında, erişim isteğini onaylayabilir veya reddedebilirler. Onaylayanın kararlarına yönelik bir iş gerekçe eklemesi gerekir.

Bir erişim isteği onaylandığında, Yetkilendirme Yönetimi, erişim paketindeki her bir kaynağa istek sahibine erişim verme işlemini başlatır. İstek sahibine erişim paketindeki her kaynağa erişim verildikten sonra, istek sahibine erişim isteğinin onaylandığı ve erişim paketine erişimi olan bir e-posta bildirimi gönderilir. Bir erişim paketine erişim verildiğinde bir istek sahibine gönderilen örnek bir e-posta bildirimi aşağıda verilmiştir.

Erişim isteği reddedildiğinde, istek sahibine bir e-posta bildirimi gönderilir. Erişim istekleri reddedildiğinde bir istek sahibine gönderilen örnek bir e-posta bildirimi aşağıda verilmiştir.

### <a name="expired-access-request-emails"></a>Son erişim isteği e-postaları

Erişim istekleri süresi dolduğunda, istek sahipleri bilgilendirilir. Bir istek sahibi bir erişim isteği gönderdiğinde, isteğin süresi dolduktan sonra istek süresi olur. Onaylama/reddetme kararı gönderen bir onaylayan yoksa, istek bekleyen bir onay durumunda kalmaya devam eder. İstek, yapılandırılan süre sonu süresine ulaştığında, isteğin süresi dolar ve artık onaylayanlar tarafından onaylanamaz veya reddedilmez. Bu durumda istek, zaman aşımına uğradı. Süre biten bir istek artık onaylanamaz veya reddedilmez. İstek sahibine, erişim isteğinin süresi dolduğunda ve erişim isteğini yeniden göndermelidir olmaları için bir e-posta bildirimi gönderilir. Erişim istekleri süresi dolduğunda, istek sahibine gönderilen örnek bir e-posta bildirimi aşağıda verilmiştir.

![Son erişim isteği e-postası](./media/entitlement-management-process/email-expired-access-request.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketine erişim isteme](entitlement-management-request-access.md)
- [Erişim isteklerini onaylama veya reddetme](entitlement-management-request-approve.md)
