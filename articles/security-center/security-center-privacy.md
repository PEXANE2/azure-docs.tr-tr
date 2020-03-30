---
title: Azure Güvenlik Merkezi'nde kullanıcı verilerini yönetme | Microsoft Dokümanlar
description: Azure Güvenlik Merkezi'nde kullanıcı verilerini nasıl yöneteceğini öğrenin. Kullanıcı verilerini yönetmek, verilere erişme, silme veya dışa aktarma olanağını içerir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: memildin
ms.openlocfilehash: 6edea1d0de53e2dc9f764de26209dc1f3110556e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978612"
---
# <a name="manage-user-data-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki kullanıcı verilerini yönetme
Bu makalede, Azure Güvenlik Merkezi'nde kullanıcı verilerini nasıl yönetebileceğiniz hakkında bilgi verilmektedir. Kullanıcı verilerini yönetmek, verilere erişme, silme veya dışa aktarma olanağını içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

Okuyucu, Sahip, Katkıda Bulunan veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı, araç içindeki müşteri verilerine erişebilir. Hesap Yöneticisi rolü hakkında daha fazla bilgi edinmek için, Okuyucu, Sahip ve Katılımcı rolleri hakkında daha fazla bilgi edinmek [için Azure rol tabanlı erişim denetimi için Yerleşik rollere](../role-based-access-control/built-in-roles.md) bakın. Bkz. [Azure abonelik yöneticileri.](../cost-management-billing/manage/add-change-subscription-administrator.md)

## <a name="searching-for-and-identifying-personal-data"></a>Kişisel verileri arama ve tanımlama
Güvenlik Merkezi kullanıcısı, azure portalı üzerinden kişisel verilerini görüntüleyebilir. Güvenlik Merkezi yalnızca e-posta adresleri ve telefon numaraları gibi güvenlik iletişim bilgilerini depolar. Daha fazla bilgi için azure [güvenlik merkezinde güvenlik iletişim bilgilerini sağlayın'a](security-center-provide-security-contact-details.md)bakın.

Azure portalında, bir kullanıcı Güvenlik Merkezi'nin tam zamanında VM erişim özelliğini kullanarak izin verilen IP yapılandırmalarını görüntüleyebilir. Daha fazla bilgi için bkz: [Tam zamanında kullanarak sanal makine erişimini yönet.](security-center-just-in-time.md)

Azure portalında, kullanıcı IP adresleri ve saldırgan ayrıntıları da dahil olmak üzere Güvenlik Merkezi tarafından sağlanan güvenlik uyarılarını görüntüleyebilir. Daha fazla bilgi için Azure [Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetme ve yanıtlatmaya](security-center-managing-and-responding-alerts.md)bakın.

## <a name="classifying-personal-data"></a>Kişisel verileri sınıflandırma
Güvenlik Merkezi'nin güvenlik iletişim özelliğinde bulunan kişisel verileri sınıflandırmanız gerekmez. Kaydedilen veriler bir e-posta adresi (veya birden çok e-posta adresi) ve bir telefon numarasıdır. [İlgili kişi verileri](security-center-provide-security-contact-details.md) Güvenlik Merkezi tarafından doğrulanır.

Güvenlik Merkezi'nin [tam zamanında](security-center-just-in-time.md) özelliği tarafından kaydedilen IP adreslerini ve bağlantı noktası numaralarını sınıflandırmanız gerekmez.

Güvenlik Merkezi'ndeki [uyarıları görüntüleyerek](security-center-managing-and-responding-alerts.md) kişisel verileri yalnızca Yönetici rolüne atanan bir kullanıcı sınıflandırabilir.

## <a name="securing-and-controlling-access-to-personal-data"></a>Kişisel verilere erişimi güvence altına alma ve kontrol etme
Okuyucu, Sahip, Katkıda Bulunan veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı [güvenlik kişiverilerine](security-center-provide-security-contact-details.md)erişebilir.

Okuyucu, Sahip, Katkıda Bulunan veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı, [tam zamanında](security-center-just-in-time.md) ilkelerine erişebilir.

Okuyucu, Sahip, Katkıda Bulunan veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı [uyarılarını](security-center-managing-and-responding-alerts.md)görüntüleyebilir.

## <a name="updating-personal-data"></a>Kişisel verileri güncelleştirme
Sahibi, Katılımcısı veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı, azure portalı üzerinden [güvenlik kişi verilerini](security-center-provide-security-contact-details.md) güncelleyebilir.

Sahip, Katılımcı veya Hesap Yöneticisi rolünü atadığı bir Güvenlik Merkezi kullanıcısı, [tam zamanında ilkelerini](security-center-just-in-time.md)güncelleyebilir.

Bir Hesap Yöneticisi uyarı olaylarını kaldıamaz. [Uyarı olayı](security-center-managing-and-responding-alerts.md) güvenlik verileri olarak kabul edilir ve yalnızca okunur.

## <a name="deleting-personal-data"></a>Kişisel verileri silme
Sahibi, Katılımcısı veya Hesap Yöneticisi rolünü atamıştır bir Güvenlik Merkezi kullanıcısı, Azure portalı üzerinden [güvenlik kişiverilerini](security-center-provide-security-contact-details.md) silebilir.

Sahip, Katılımcı veya Hesap Yöneticisi rolünü atamıştır bir Güvenlik Merkezi kullanıcısı, Azure portalı üzerinden [tam zamanında ilkeleri](security-center-just-in-time.md) silebilir.

Güvenlik Merkezi kullanıcısı uyarı olaylarını silemez. Güvenlik nedenleriyle, bir [uyarı olayı](security-center-managing-and-responding-alerts.md) salt okunur veri olarak kabul edilir.

## <a name="exporting-personal-data"></a>Kişisel verileri dışarı aktarma
Okuyucu, Sahip, Katkıda Bulunan veya Hesap Yöneticisi rolünü atayan bir Güvenlik Merkezi kullanıcısı [güvenlik kişiverilerini](security-center-provide-security-contact-details.md) şu şekilde dışa aktarabilir:

- Azure portalından kopyalama
- Azure REST API çağrısını yürütme, HTTP'Yi AL:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
  ```

Hesap Yöneticisi rolünü atayan bir Güvenlik Merkezi kullanıcısı, IP adreslerini içeren [tam zamanında ilkeleri](security-center-just-in-time.md) aşağıdakilere göre dışa aktarabilir:

- Azure portalından kopyalama
- Azure REST API çağrısını yürütme, HTTP'Yi AL:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
  ```

Bir Hesap Yöneticisi uyarı ayrıntılarını şu şekilde dışa aktarabilir:

- Azure portalından kopyalama
- Azure REST API çağrısını yürütme, HTTP'Yi AL:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
  ```

Daha fazla bilgi için [bkz: Güvenlik Uyarıları Al (GET Collection)](https://msdn.microsoft.com/library/mt704050.aspx).

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>İzin siz profil oluşturma veya pazarlama için kişisel verilerin kullanımını kısıtlamak
Güvenlik Merkezi [kullanıcısı, güvenlik kişi verilerini](security-center-provide-security-contact-details.md)silerek devre dışı bırakmayı seçebilir.

[Tam zamanında veri](security-center-just-in-time.md) tanımlanamayan veriler olarak kabul edilir ve 30 gün süreyle saklanır.

[Uyarı verileri](security-center-managing-and-responding-alerts.md) güvenlik verileri olarak kabul edilir ve iki yıl süreyle saklanır.

## <a name="auditing-and-reporting"></a>Denetim ve raporlama
Güvenlik kişisinin denetim günlükleri, tam zamanında ve uyarı [güncelleştirmeleri Azure Etkinlik Günlükleri'nde](../azure-monitor/platform/platform-logs-overview.md)tutulur.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcı verilerini yönetme hakkında daha fazla bilgi için [bkz.](security-center-investigation-user-data.md)
