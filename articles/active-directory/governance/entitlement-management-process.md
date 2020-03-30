---
title: Bildirim & alma işlemi isteği - Azure AD yetkilendirme yönetimi
description: Bir erişim paketi için istek süreci ve Azure Active Directory yetkilendirme yönetiminde e-posta bildirimleri gönderildiği hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128516"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde işlem ve e-posta bildirimleri isteme

Bir kullanıcı bir erişim paketine istek gönderdiğinde, bir işlem bu erişim isteğini teslim etmeye başlar. Azure AD yetkilendirme yönetimi, işlem sırasında önemli olaylar meydana geldiğinde onaylayanlara ve istekte bulunduranlara e-posta bildirimleri gönderir. Bu makalede, istek süreci ve gönderilen e-posta bildirimleri açıklanmaktadır.

## <a name="request-process"></a>İstek süreci

Bir erişim paketine erişmesi gereken bir kullanıcı erişim isteği gönderebilir. İlkenin yapılandırması bağlı olarak, istek bir onay gerektirebilir. Bir istek onaylandığında, bir işlem kullanıcı erişimini erişim paketindeki her kaynağa atamaya başlar. Aşağıdaki diyagram, sürecin ve farklı durumların genel bir özetini gösterir:

![Onay süreci diyagramı](./media/entitlement-management-process/request-process.png)

| Durum | Açıklama |
| --- | --- |
| Gönderildi | Kullanıcı bir istek gönderir. |
| Bekleyen onay | Bir erişim paketi için ilke onay gerektiriyorsa, istek bekleyen onaya taşınır. |
| Süresi doldu | Onaylayanlar onay isteği zaman aşımı içinde bir isteği onaylamazsa, istek sona erer. Yeniden denemek için, kullanıcının isteğini yeniden göndermesi gerekir. |
| Reddedildi | Onaylayan bir isteği reddeder. |
| Onaylandı | Onaylayan bir isteği onaylar. |
| Teslim | Kullanıcıya erişim paketindeki tüm kaynaklara erişim **atanmadı.** Bu harici bir kullanıcıysa, kullanıcı kaynak dizinine henüz erişmiş olmayabilir. Ayrıca onay istemini de kabul etmemiş olabilirler. |
| Teslim Edildi | Kullanıcıya erişim paketindeki tüm kaynaklara erişim atandı. |
| Erişim uzatıldı | İlkede uzantılara izin verilirse, kullanıcı atamayı uzattı. |
| Erişimsüresi doldu | Kullanıcının erişim paketine erişiminin süresi doldu. Yeniden erişim elde etmek için, kullanıcının bir istek göndermesi gerekir. |

## <a name="email-notifications"></a>E-posta bildirimleri

Onaylayıcıysanız, bir erişim isteğini onaylamanız gerektiğinde size e-posta bildirimleri gönderilir. Ayrıca, bir erişim isteği tamamlandığında bildirimler ilerlersiniz. Ayrıca, istekte bulunduğunuzda isteğinizin durumunu gösteren e-posta bildirimleri de gönderilir.

Aşağıdaki diyagramlar, bu e-posta bildirimlerinin onaylayanlara veya istekte bulundurucuya ne zaman gönderildiğini gösterir. Diyagramlarda görüntülenen e-posta bildirimlerine karşılık gelen numarayı bulmak için [e-posta bildirimleri tablosuna](entitlement-management-process.md#email-notifications-table) başvurun.

### <a name="first-approvers-and-alternate-approvers"></a>İlk onaylayıcılar ve alternatif onaylayıcılar
Aşağıdaki diyagram, ilk onaylayanların ve alternatif onaylayıcıların deneyimini ve istek işlemi sırasında aldıkları e-posta bildirimlerini gösterir:

![İlk ve alternatif onaylayıcılar işlem akışı](./media/entitlement-management-process/first-approvers-and-alternate-with-escalation-flow.png)

### <a name="requestors"></a>İstek sahipleri
Aşağıdaki diyagram, istekte bulunduranların deneyimlerini ve istek sürecinde aldıkları e-posta bildirimlerini gösterir:

![İstek veya süreç akışı](./media/entitlement-management-process/requestor-approval-request-flow.png)

### <a name="2-stage-approval"></a>2 aşamalı onay
Aşağıdaki diyagram, aşama-1 ve aşama-2 onaylayıcılarının deneyimlerini ve istek işlemi sırasında aldıkları e-posta bildirimlerini gösterir:

![2 aşamalı onay süreci akışı](./media/entitlement-management-process/2stage-approval-with-request-timeout-flow.png)

### <a name="email-notifications-table"></a>E-posta bildirimleri tablosu
Aşağıdaki tablo, bu e-posta bildirimlerinin her biri hakkında daha fazla ayrıntı sağlar. Bu e-postaları yönetmek için kuralları kullanabilirsiniz. Örneğin, Outlook'ta, özne bu tablodan sözcükler içeriyorsa, e-postaları bir klasöre taşımak için kurallar oluşturabilirsiniz:

| # | E-posta konusu | Gönderildiğinde | Gönderilen |
| --- | --- | --- | --- |
| 1 | Gerekli eylem: *[tarih]* tarafından iletilen isteği onaylama veya reddetme | Bu e-posta, harekete geçmek üzere Aşama-1 alternatif onaylayıcılarına (istek arttıktan sonra) gönderilir. | Aşama-1 alternatif onaylayıcılar |
| 2 | Gerekli eylem: *[tarihe göre]* isteği onaylama veya reddetme | Bu e-posta, yükseltme devre dışı bırakılırsa, harekete geçmek üzere ilk onaylayana gönderilir. | İlk onaylayan |
| 3 | Hatırlatma: *[İstekçi]* için *[tarihe]* göre isteği onaylayın veya reddedin | Bu anımsatıcı e-postası, yükseltme devre dışı bırakılırsa ilk onaylayana gönderilir. E-posta, yapmadılarsa harekete geçmelerini ister. | İlk onaylayan |
| 4 | *[tarihte]* *[time]* isteği onaylama veya reddetme | Bu e-posta, harekete geçmek için ilk onaylayıcıya (yükseltme etkinse) gönderilir. | İlk onaylayan |
| 5 | Eylem gerekli hatırlatma: *[İstekçi]* için *[tarih]* isteği onaylamak veya reddetmek | Yükseltme etkinse, bu anımsatıcı e-postası ilk onaylayana gönderilir. E-posta, yapmadılarsa harekete geçmelerini ister. | İlk onaylayan |
| 6 | *[access_package]* için istek süresi doldu | Bu e-posta, istek sona erdikten sonra ilk onaylayıcıya ve aşama-1 alternatif onaylayıcılarına gönderilir. | İlk onaylayıcı, aşama-1 alternatif onaylayıcılar |
| 7 | *[access_package]* için *[istekte bulunduran]* için istenen istek onaylandı | Bu e-posta, istek tamamlandıktan sonra ilk onaylayıcıya ve aşama-1 alternatif onaylayıcılarına gönderilecektir. | İlk onaylayıcı, aşama-1 alternatif onaylayıcılar |
| 8 | *[access_package]* için *[istekte bulunduran]* için istenen istek onaylandı | Bu e-posta, aşama-1 isteği onaylandığında 2 aşamalı bir isteğin ilk onaylayıcısına ve aşama-1 alternatif onaylayıcılarına gönderilir. | İlk onaylayıcı, aşama-1 alternatif onaylayıcılar |
| 9 | İstek *[access_package]* reddedildi | Bu e-posta, istekleri reddedildiğinde istekte bulunulduğunda istekte bulunulacaktır | Requestor |
| 10 | *[access_package]* için isteğiniz in süresi doldu | Bu e-posta, tek veya 2 aşamalı bir isteğin sonunda istekte bulununacaktır. E-posta, istek verene isteğin süresinin dolduğunu belirtir. | Requestor |
| 11 | Gerekli eylem: *[tarihe göre]* isteği onaylama veya reddetme | Bu e-posta, yükseltme devre dışı bırakılırsa, harekete geçmek üzere ikinci onaylayıcıya gönderilir. | İkinci onaylayıcı |
| 12 | Eylem gerekli hatırlatma: *[tarihe* göre] isteği onaylama veya reddetme | Bu anımsatıcı e-postası, yükseltme devre dışı bırakılırsa ikinci onaylayıcıya gönderilir. Bildirim, henüz yapmadılarsa harekete geçmelerini ister. | İkinci onaylayıcı |
| 13 | Gerekli eylem: *[İstekçi]* için *[tarihe]* göre isteği onaylamak veya reddetmek | Bu e-posta, yükseltme etkinse, harekete geçmek üzere ikinci onaylayıcıya gönderilir. | İkinci onaylayıcı |
| 14 | Eylem gerekli hatırlatma: *[İstekçi]* için *[tarih]* isteği onaylamak veya reddetmek | Bu anımsatıcı e-postası, yükseltme etkinse ikinci onaylayıcıya gönderilir. Bildirim, henüz yapmadılarsa harekete geçmelerini ister. | İkinci onaylayıcı |
| 15 | Gerekli eylem: *[tarih]* tarafından iletilen isteği onaylama veya reddetme | Bu e-posta, yükseltme etkinse, harekete geçmek üzere aşama-2 alternatif onaylayıcılarına gönderilir. | Aşama-2 alternatif onaylayıcılar |
| 16 | *[access_package]* için *[istekte bulunduran]* için istenen istek onaylandı | Bu e-posta, isteği onayladıktan sonra ikinci onaylayıcıya ve aşama-2 alternatif onaylayıcılarına gönderilecektir. | İkinci onaylayıcı, Aşama-2 alternatif onaylayıcılar |
| 17 | *[access_package]* için bir istek süresi doldu | Bu e-posta, istek sona erdikten sonra ikinci onaylayıcıya veya alternatif onaylayıcılara gönderilir. | İkinci onaylayıcı, sahne-2 alternatif onaylayıcılar |
| 18 | Artık *[access_package]* erişiminiz var | Bu e-posta, erişimlerini kullanmaya başlamak için son kullanıcılara gönderilir. | Requestor |
| 19 | *[access_package]* için erişimi *[tarihe]* göre genişletin | Bu e-posta, erişimsüresi dolmadan önce son kullanıcılara gönderilir. | Requestor |
| 20 | *[access_package]* için erişim sona erdi | Bu e-posta, erişimlerinin süresi dolduktan sonra son kullanıcılara gönderilir. | Requestor |

### <a name="access-request-emails"></a>İstek e-postalarını erişin

İstekçi onay gerektirecek şekilde yapılandırılan bir erişim paketi için erişim isteği gönderdiğinde, politikaya eklenen tüm onaylayıcılar isteğin ayrıntılarını içeren bir e-posta bildirimi alır. E-postadaki ayrıntılar şunlardır: istekte bulunanın adı organizasyonu ve iş gerekçesi; ve istenen erişim başlangıç ve bitiş tarihi (eğer sağlanmışsa). Ayrıntılar, isteğin ne zaman gönderileceğin ve isteğin ne zaman sona ereceğini de içerir.

E-posta, onaylayanların erişim isteğini onaylamak veya reddetmek için Erişimim'e gitmek için tıklayabilecekleri bir bağlantı içerir. Bir erişim isteğini tamamlamak için ilk onaylayana veya ikinci onaylayana (2 aşamalı onay etkinse) gönderilen örnek bir e-posta bildirimi aşağıda veda edilir:

![Paket e-postasına erişmek için isteği onaylama](./media/entitlement-management-shared/approver-request-email.png)

Onaylayanlar da bir hatırlatma e-postası alabilir. E-posta, onaylayandan istek le ilgili bir karar vermesini ister. Burada, onaylayanın eylemde bulunmalarını hatırlatmak için aldığı örnek bir e-posta bildirimi verem:

![Anımsatıcı erişim isteği e-postası](./media/entitlement-management-process/approver-access-request-reminder-email.png)

### <a name="alternate-approvers-request-emails"></a>Alternatif onaylayanlar e-posta isteğinde

Alternatif onaylayıcılar ayarı etkinse ve istek beklemedeyse, iletilecektir. Alternatif onaylayanlar, isteği onaylamak veya reddetmek için bir e-posta alır. Aşama-1 ve aşama-2'de alternatif onaylayıcıları etkinleştirebilirsiniz. Alternatif onaycıların aldığı bildirimin örnek bir e-postası aşağıda veda edebilirsiniz:

![Alternatif onaylayanlar e-posta isteğinde](./media/entitlement-management-process/alternate-approver-email-fwd-request.png)

Hem onaylayan hem de alternatif onaylayanlar isteği onaylayabilir veya reddedebilir.

### <a name="approved-or-denied-emails"></a>Onaylanan veya reddedilen e-postalar

 Bir onaylayıcı, bir istekçi tarafından gönderilen bir erişim isteğini aldığında, erişim isteğini onaylayabilir veya reddedebilir. Onaylayanın kararı için bir iş gerekçesi eklemesi gerekir. Bir istek onaylandıktan sonra onaylayanlara ve alternatif onaylayıcılara gönderilen örnek bir e-posta aşağıda veda edilir:

![Paket e-postasına erişmek için onaylı istek](./media/entitlement-management-process/approver-request-email-approved.png)

Bir erişim isteği onaylandığında ve erişimleri sağlandığında, istek sahibine artık erişim paketine erişebildiklerine dair bir e-posta bildirimi gönderilir. Bir erişim paketine erişim izni verildiğinde bir istekte bulunulan örnek bir e-posta bildirimi aşağıda verilmiştir:

![Onaylı istek veya erişim isteği e-posta](./media/entitlement-management-process/requestor-email-approved.png)

Bir erişim isteği reddedildiğinde, istekte bulununcaya bir e-posta bildirimi gönderilir. Erişim isteği reddedildiğinde bir istekte bulunulan örnek bir e-posta bildirimi aşağıda veda edilmiştir:

![İstek li istek reddedilen e-posta](./media/entitlement-management-process/requestor-email-denied.png)

### <a name="2-stage-approval-access-request-emails"></a>2 aşamalı onay erişim isteği e-postaları

2 aşamalı onay etkinleştirilirse, istekçi erişim alabilmek için en az iki onaylayıcının her aşamadan olmak üzere isteği onaylaması gerekir.

Aşama-1 sırasında, ilk onaylayan erişim isteği e-postasını alır ve bir karar verir. İsteği onaylarsa, aşama-1'deki tüm ilk onaylayıcılar ve alternatif onaylayıcılar (yükseltme etkinse) aşama-1'in tamamladığına dair bildirim alır. Aşama-1 tamamlandığında gönderilen bildirimin örnek bir e-postası aşağıda veda edebilirsiniz:

![2 aşamalı erişim isteği e-postası](./media/entitlement-management-process/approver-request-email-2stage.png)

İlk veya alternatif onaylayıcılar, aşama-1'de isteği onayladıktan sonra, aşama-2 başlar. Aşama-2 sırasında, ikinci onaylayıcı erişim isteği bildirim e-postasını alır. Aşama-2'deki ikinci onaylayıcı veya alternatif onaylayıcılar (yükseltme etkinse) isteği onaylamaya veya reddetmeye karar verdikten sonra, bildirim e-postaları birinci ve ikinci onaylayıcılara ve aşama-1 ve aşama-2'deki tüm alternatif onaylayıcılara ve talepte bulunanlara gönderilir.

### <a name="expired-access-request-emails"></a>Süresi dolmuş erişim isteği e-postaları

Hiçbir onaylayıcı isteği onaylamamış veya reddetmişse erişim isteklerinin süresi dolabilir. 

İstek yapılandırılmış son kullanma tarihine ulaştığında ve süresi dolduğunda, onaylayanlar tarafından artık onaylanamaz veya reddedilemez. Aşağıda, birinci, ikinci (2 aşamalı onay etkinse) ve alternatif onaylayıcıların tümüne gönderilen bildirimin örnek bir e-postası ve alternatif onaylayıcılar ve aşağıda ve

![Onaylayanlar süresi dolmuş erişim isteği e-posta](./media/entitlement-management-process/approver-request-email-expired.png)

İstekçiye, erişim isteklerinin süresinin dolduğunu ve erişim isteğini yeniden göndermeleri gerektiğini bildiren bir e-posta bildirimi de gönderilir. Aşağıdaki diyagram, isteği olanın deneyimini ve erişimi genişletmek istediklerinde aldıkları e-posta bildirimlerini gösterir:

![İstek veya erişim süreci akışını genişletmek](./media/entitlement-management-process/requestor-expiration-request-flow.png) 

Erişim isteğinin süresi dolduğunda bir istekçiye gönderilen örnek bir e-posta bildirimi aşağıda veda edilmiştir:

![İstek süresi dolmuş erişim isteği e-posta](./media/entitlement-management-process/requestor-email-request-expired.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketine erişim isteği](entitlement-management-request-access.md)
- [Erişim isteklerini onaylama veya reddetme](entitlement-management-request-approve.md)
