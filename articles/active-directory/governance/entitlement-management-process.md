---
title: İşlem & bildirimleri isteği-Azure AD Yetkilendirme Yönetimi
description: Erişim paketi için istek süreci ve Azure Active Directory yetkilendirme yönetimi 'nde e-posta bildirimleri gönderildiğinde öğrenin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4ff270977449bb80f97073342dc0c726a3f2316
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80128516"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde işlem ve e-posta bildirimleri iste

Bir Kullanıcı bir erişim paketine istek gönderdiğinde, bir işlem o erişim isteğini sunmaya başlar. Azure AD Yetkilendirme Yönetimi, işlem sırasında önemli olaylar gerçekleştiğinde onaylayanlara ve istek Lara e-posta bildirimleri gönderir. Bu makalede, istek işlemi ve gönderilen e-posta bildirimleri açıklanmaktadır.

## <a name="request-process"></a>İstek işlemi

Erişim paketine erişmesi gereken bir Kullanıcı, erişim isteği gönderebilir. İlkenin yapılandırmasına bağlı olarak istek için onay gerekebilir. Bir istek onaylandığında bir işlem, erişim paketindeki her kaynağa Kullanıcı erişimini atamaya başlar. Aşağıdaki diyagramda, işleme ve farklı durumlara genel bir bakış gösterilmektedir:

![Onay işlemi diyagramı](./media/entitlement-management-process/request-process.png)

| Durum | Açıklama |
| --- | --- |
| Gönderildi | Kullanıcı bir istek gönderir. |
| Onay bekleniyor | Bir erişim paketinin ilkesi onay gerektiriyorsa, bir istek bekleyen onaya gider. |
| Süresi doldu | Onay isteği zaman aşımı içinde bir onaylayan isteği onaylamadıysanız, isteğin süresi dolar. Yeniden denemek için, kullanıcının isteklerini yeniden göndermek zorunda kalır. |
| Reddedildi | Onaylayan bir isteği reddeder. |
| Onaylandı | Onaylayan bir isteği onaylar. |
| Sunma | Kullanıcıya erişim paketindeki tüm kaynaklara **erişim atanmamış.** Bu bir dış Kullanıcı ise, Kullanıcı kaynak dizinine henüz erişmemiş olabilir. Ayrıca onay isteğini kabul etmeyebilir. |
| Teslim Edildi | Kullanıcıya erişim paketindeki tüm kaynaklara erişim atandı. |
| Genişletilmiş erişim | İlkede uzantılara izin veriliyorsa Kullanıcı atamayı genişletti. |
| Erişim zaman aşımına uğradı | Kullanıcının erişim paketine erişiminin süresi doldu. Yeniden erişim sağlamak için kullanıcının bir istek göndermesi gerekir. |

## <a name="email-notifications"></a>E-posta bildirimleri

Bir onaylıyorsanız, bir erişim isteğini onaylamanız gerektiğinde e-posta bildirimleri gönderilir. Ayrıca, bir erişim isteği tamamlandığında da bildirim alırsınız. Ayrıca, istek sahibi olduğunuzda isteğiniz durumunu belirten e-posta bildirimleri de gönderdiniz.

Aşağıdaki diyagramlarda, bu e-posta bildirimlerinin onaylayanlara veya istek sahibine ne zaman gönderildiği gösterilmektedir. Diyagramda görüntülenecek e-posta bildirimlerine karşılık gelen sayıyı bulmak için [e-posta bildirimleri tablosuna](entitlement-management-process.md#email-notifications-table) başvurun.

### <a name="first-approvers-and-alternate-approvers"></a>İlk onaylayanlar ve alternatif onaylayanlar
Aşağıdaki diyagramda, ilk onaylayanlar ve alternatif onaylayanlar deneyimi ve istek işlemi sırasında aldıkları e-posta bildirimleri gösterilmektedir:

![İlk ve alternatif onaylayanlar işlem akışı](./media/entitlement-management-process/first-approvers-and-alternate-with-escalation-flow.png)

### <a name="requestors"></a>İstek sahipleri
Aşağıdaki diyagramda istek süreci sırasında istek sahipleri ve aldıkları e-posta bildirimleri gösterilmektedir:

![İstek sahibi işlem akışı](./media/entitlement-management-process/requestor-approval-request-flow.png)

### <a name="2-stage-approval"></a>2 aşamalı onay
Aşağıdaki diyagramda, 1. aşama ve 2. aşama ve istek sürecinde aldıkları e-posta bildirimlerinin deneyimi gösterilmektedir:

![2 aşamalı onay süreci akışı](./media/entitlement-management-process/2stage-approval-with-request-timeout-flow.png)

### <a name="email-notifications-table"></a>E-posta bildirimleri tablosu
Aşağıdaki tabloda, bu e-posta bildirimlerinin her biri hakkında daha ayrıntılı bilgi verilmektedir. Bu e-postaları yönetmek için kurallarını kullanabilirsiniz. Örneğin, Outlook 'ta, konu bu tablodan sözcükler içeriyorsa e-postaları bir klasöre taşımak için kurallar oluşturabilirsiniz:

| # | E-posta konusu | Gönderildiğinde | Gönderildiği yer |
| --- | --- | --- | --- |
| 1 | Eylem gerekiyor: iletilen isteği *[Date]* tarafından onayla veya Reddet | Bu e-posta, 1. aşama (isteğin ilerletilmiş olduktan sonra) işlem yapması için gönderilir. | Aşama-1 alternatif onaylayanlar |
| 2 | Eylem gerekli: *[Date]* tarafından istekleri onaylama veya reddetme | Bu e-posta, yükseltme devre dışı bırakılmışsa, işlem yapmak için ilk onaylayana gönderilir. | İlk onaylayan |
| 3 | Anımsatıcı: [ *istek sahibi]* için *[Date]* isteği onaylama veya reddetme | Bu anımsatıcı e-postası, yükseltme devre dışı bırakılmışsa ilk onaylayana gönderilir. E-posta, uygulamadıklarında işlem yapması istenir. | İlk onaylayan |
| 4 | [ *Date* ] tarihinde *[Time]* isteği onayla veya Reddet | Bu e-posta, ilk onaylayana (yükseltme etkinse) işlem yapması için gönderilir. | İlk onaylayan |
| 5 | Eylem gerekiyor anımsatıcı: *[istek sahibi]* için *[Date]* isteği onaylama veya reddetme | Bu anımsatıcı e-postası, yükseltme etkinse ilk onaylayana gönderilir. E-posta, uygulamadıklarında işlem yapması istenir. | İlk onaylayan |
| 6 | *[Access_package]* için isteğin süresi doldu | Bu e-posta, isteğin süresi dolduktan sonra ilk onaylayana ve Aşama 1 alternatif onaylayanlara gönderilir. | İlk onaylayan, Aşama 1 alternatif onaylayanlar |
| 7 | *[İstek sahibi]* için *[access_package]* isteği onaylandı | Bu e-posta ilk onaylayana gönderilir ve istek tamamlandığında 1 alternatif onaylayanlara gönderilir. | İlk onaylayan, Aşama 1 alternatif onaylayanlar |
| 8 | *[İstek sahibi]* için *[access_package]* isteği onaylandı | Bu e-posta, 1. aşama isteği onaylandığında bir 2 aşamalı isteğin ilk onaylayanlara ve 1. aşamasına gönderilir. | İlk onaylayan, Aşama 1 alternatif onaylayanlar |
| 9 | *[Access_package]* isteği reddedildi | Bu e-posta, istekleri reddedildiğinde istek sahibine gönderilir | Requestor |
| 10 | İsteğiniz *[access_package]* için sona ermiştir | Bu e-posta, tek veya 2 aşamalı isteğin sonunda istek sahibine gönderilir. E-posta, isteğin zaman aşımına uğradığı istek sahibine bildirir. | Requestor |
| 11 | Eylem gerekli: *[Date]* tarafından istekleri onaylama veya reddetme | Bu e-posta, yükseltme devre dışıysa, işlem yapmak için ikinci onaylayana gönderilir. | İkinci onaylayan |
| 12 | Eylem gerekiyor anımsatıcısı: *[Date]* tarafından isteği onaylayın veya reddedin | Yükseltme devre dışıysa, bu anımsatıcı e-postası ikinci onaylayana gönderilir. Bildirim, henüz yoksa işlem yapması istenir. | İkinci onaylayan |
| 13 | Eylem gerekiyor: *[istek sahibi]* için *[Date]* isteği onayla veya Reddet | Bu e-posta, yükseltme etkinse, işlem yapmak için ikinci onaylayana gönderilir. | İkinci onaylayan |
| 14 | Eylem gerekiyor anımsatıcı: *[istek sahibi]* için *[Date]* isteği onaylama veya reddetme | Bu anımsatıcı e-postası, yükseltme etkinse ikinci onaylayana gönderilir. Bildirim, henüz yoksa işlem yapması istenir. | İkinci onaylayan |
| 15 | Eylem gerekiyor: iletilen isteği *[Date]* tarafından onayla veya Reddet | Bu e-posta, yükseltme etkinse, işlem yapmak için aşama 2 alternatif onaylayanlara gönderilir. | 2. aşama alternatif onaylayanlar |
| 16 | *[İstek sahibi]* için *[access_package]* isteği onaylandı | Bu e-posta ikinci onaylayana gönderilir ve istek onaylanınca 2. aşama diğer onaylayanlara gönderilir. | İkinci onaylayan, Aşama 2 alternatif onaylayanlar |
| 17 | *[Access_package]* için bir isteğin süresi doldu | Bu e-posta, isteğin süresi dolduktan sonra ikinci onaylayana veya alternatif onaylayanlara gönderilir. | İkinci onaylayan, Aşama 2 alternatif onaylayanlar |
| 18 | Artık *[access_package]* erişimine sahipsiniz | Bu e-posta, son kullanıcılara erişimini kullanmaya başlamak için gönderilir. | Requestor |
| 19 | [ *Date]* tarafından *[access_package]* için erişimi uzat | Bu e-posta, erişiminin süresi dolmadan son kullanıcılara gönderilir. | Requestor |
| 20 | *[Access_package]* için erişim sona erdi | Bu e-posta, erişiminin süresi dolduktan sonra son kullanıcılara gönderilir. | Requestor |

### <a name="access-request-emails"></a>Erişim isteği e-postaları

Bir istek sahibi onay gerektirecek şekilde yapılandırılmış bir erişim paketi için erişim isteği gönderdiğinde, ilkeye eklenen tüm onaylayanlar isteğin ayrıntılarını içeren bir e-posta bildirimi alır. E-postadaki Ayrıntılar şunları içerir: istek sahibinin adı organizasyonu ve iş doğrulaması; ve istenen erişim başlangıç ve bitiş tarihi (sağlanmışsa). Ayrıntılar Ayrıca isteğin ne zaman gönderildiğini ve isteğin ne zaman dolacağını da içerir.

E-posta, erişim isteğini onaylamak veya reddetmek için erişimime gitmek üzere açık ' a tıklayarak bir bağlantı onaylayanları içerir. Aşağıda, bir erişim isteğini tamamlaması için ilk onaylayana veya ikinci onaylayana (2 aşamalı onay etkinse) gönderilen örnek bir e-posta bildirimi verilmiştir:

![Paket e-postasına erişim isteğini onayla](./media/entitlement-management-shared/approver-request-email.png)

Onaylayanlar Ayrıca bir anımsatıcı e-postası alabilir. E-posta, onaylayana istek hakkında bir karar vermesini ister. Onaylayanın işlem yapması için bu bildirimin aldığı örnek bir e-posta bildirimi aşağıda verilmiştir:

![Anımsatıcı erişim isteği e-postası](./media/entitlement-management-process/approver-access-request-reminder-email.png)

### <a name="alternate-approvers-request-emails"></a>Alternatif onaylayanlar isteği e-postaları

Alternatif onaylayanlar ayarı etkinse ve istek hala beklendiyse iletilir. Alternatif onaylayanlar, isteği onaylamak veya reddetmek için bir e-posta alır. 1. ve 2. aşama 'da alternatif onaylayanları etkinleştirebilirsiniz. Diğer onaylayanların aldığı bildirimin örnek bir e-postası aşağıda verilmiştir:

![Alternatif onaylayanlar isteği e-postası](./media/entitlement-management-process/alternate-approver-email-fwd-request.png)

Onaylayan ve alternatif onaylayanlar, isteği onaylayabilir veya reddedebilir.

### <a name="approved-or-denied-emails"></a>Onaylanan veya reddedilen e-postalar

 Bir onaylayan, istek sahibi tarafından gönderilen bir erişim isteği aldığında, erişim isteğini onaylayabilir veya reddedebilirler. Onaylayanın kararlarına yönelik bir iş gerekçe eklemesi gerekir. Bir istek onaylandıktan sonra onaylayanlara ve alternatif onaylayanlara gönderilen örnek bir e-posta aşağıda verilmiştir:

![Paket e-postasına erişim onaylandı isteği](./media/entitlement-management-process/approver-request-email-approved.png)

Bir erişim isteği onaylandığında ve erişimleri sağlandığında, istek sahibine artık erişim paketine erişimi olan bir e-posta bildirimi gönderilir. Bir erişim paketine erişim verildiğinde bir istek sahibine gönderilen örnek bir e-posta bildirimi aşağıda verilmiştir:

![Onaylanan istek sahibi erişim isteği e-postası](./media/entitlement-management-process/requestor-email-approved.png)

Erişim isteği reddedildiğinde, istek sahibine bir e-posta bildirimi gönderilir. Erişim istekleri reddedildiğinde bir istek sahibine gönderilen örnek bir e-posta bildirimi aşağıda verilmiştir:

![İstek sahibi isteği reddedildi e-postası](./media/entitlement-management-process/requestor-email-denied.png)

### <a name="2-stage-approval-access-request-emails"></a>2 aşamalı onay erişim isteği e-postaları

2 aşamalı onay etkinse, istek sahibinin erişim alabilmesi için en az iki onaylayan tarafından her aşamadan biri olan isteği onaylaması gerekir.

İlk onaylayan, 1. aşama sırasında erişim isteği e-postasını alır ve bir karar vermez. İsteği onaylarsa, 1. aşama içindeki tüm ilk onaylayanlar ve alternatif onaylayanlar (yükseltme etkinse), Aşama 1 ' in tamamlandığını belirten bir bildirim alır. İşte, 1. aşama tamamlandığında gönderilen bildirimin örnek bir e-postası aşağıda verilmiştir:

![2 aşamalı erişim isteği e-postası](./media/entitlement-management-process/approver-request-email-2stage.png)

İlk veya alternatif onaylayanlar, 1. aşama içindeki isteği onayladıktan sonra 2. aşama başlar. 2. aşama sırasında ikinci onaylayan, erişim isteği bildirim e-postasını alacaktır. İkinci onaylayan veya 2. aşama (yükseltme etkinse), isteği onaylamaya veya reddetmeye karar verirse, bildirim e-postalarının ilk ve ikinci onaylayanlara ve 1. aşama ve 2. Aşama ' daki tüm alternatif onaylayanlara ve istek sahibine yönelik olarak gönderilmesini sağlar.

### <a name="expired-access-request-emails"></a>Son erişim isteği e-postaları

Bir onaylayan, isteği onaylamamışsa veya reddetmezse erişim istekleri zaman sona erecektir. 

İstek, yapılandırılan süre sonu tarihine ulaştığında ve süresi dolduğunda, bu, artık onaylayanlar tarafından onaylanamaz veya reddedilmez. İlk, ikinci (2 aşamalı onay etkinse) ve alternatif onaylayanlar için gönderilen bildirimin örnek bir e-postası aşağıda verilmiştir:

![Onaylayanların süre dolma erişimi isteği e-postası](./media/entitlement-management-process/approver-request-email-expired.png)

Ayrıca, istek sahibine bir e-posta bildirimi gönderilir, bunlara erişim istekleri sona ermiştir ve erişim isteğini yeniden gönderebilmeleri gerekir. Aşağıdaki diyagramda, istek sahibinin deneyimi ve erişim genişletmesi istediklerinde aldıkları e-posta bildirimleri gösterilmektedir:

![İstek sahibi uzatma erişimi işlem akışı](./media/entitlement-management-process/requestor-expiration-request-flow.png) 

Erişim istekleri süresi dolduğunda, istek sahibine gönderilen örnek bir e-posta bildirimi aşağıda verilmiştir:

![İstek sahibinin süre dolduğunda erişim isteği e-postası](./media/entitlement-management-process/requestor-email-request-expired.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketine erişim isteme](entitlement-management-request-access.md)
- [Erişim isteklerini onaylama veya reddetme](entitlement-management-request-approve.md)
