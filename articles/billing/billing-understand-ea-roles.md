---
title: Azure 'da Enterprise için yönetici rollerini anlama | Microsoft Docs
description: Azure 'da Kurumsal Yönetici rolleri hakkında bilgi edinin.
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2018
ms.author: banders
ms.openlocfilehash: 39526defb8f41ddacb0a26d7ad852f820ca6ea77
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034536"
---
# <a name="understand-azure-enterprise-agreement-administrative-roles-in-azure"></a>Azure 'da Azure Kurumsal Anlaşma yönetim rollerini anlama

Kuruluşunuzun kullanımını ve harcamalarınızı yönetmeye yardımcı olmak için bir Kurumsal Anlaşma (EA) olan Azure müşterileri beş ayrı yönetim rolü atayabilir:

- Kuruluş Yöneticisi
- Kuruluş Yöneticisi (salt okuma)
- Bölüm Yöneticisi
- Bölüm Yöneticisi (salt okuma)
- Hesap Sahibi
 
Bu roller, Azure Kurumsal sözleşmelerini yönetmeye özgüdür ve Azure 'un kaynaklara erişimi denetleyecek yerleşik rollere ek niteliğindedir. Daha fazla bilgi için bkz. [Azure kaynakları Için yerleşik roller](../role-based-access-control/built-in-roles.md).

Aşağıdaki bölümler her rolün sınırlamalarını ve yeteneklerini anlatmaktadır.

## <a name="user-limit-for-admin-roles"></a>Yönetici rolleri için Kullanıcı sınırı

|Role| Kullanıcı sınırı|
|---|---|
|Kuruluş Yöneticisi|Sınırsız|
|Kuruluş Yöneticisi (salt okuma)|Sınırsız|
|Bölüm Yöneticisi|Sınırsız|
|Bölüm Yöneticisi (salt okuma)|Sınırsız|
|Hesap Sahibi|Hesap başına 1<sup>1</sup>|

<sup>1</sup> her hesap için benzersiz bir Microsoft hesabı veya iş veya okul hesabı gerekir.

## <a name="organization-structure-and-permissions-by-role"></a>Role göre kuruluş yapısı ve izinleri

|Görevler| Kuruluş Yöneticisi|Kuruluş Yöneticisi (salt okuma)|Bölüm Yöneticisi|Bölüm Yöneticisi (salt okuma)|Hesap Sahibi|
|---|---|---|---|---|---|
|Kuruluş yöneticilerini görüntüle|✔|✔|✘|✘|✘|
|Kuruluş yöneticileri ekleme veya kaldırma|✔|✘|✘|✘|✘|
|Bildirim kişilerini görüntüle<sup>2</sup> |✔|✔|✘|✘|✘|
|Bildirim kişilerini Ekle veya Kaldır<sup>2</sup> |✔|✘|✘|✘|✘|
|Departmanlar oluşturma ve yönetme |✔|✘|✘|✘|✘|
|Bölüm yöneticilerini görüntüle|✔|✔|✔|✔|✘|
|Bölüm yöneticileri ekleme veya kaldırma|✔|✘|✔|✘|✘|
|Kayıt içindeki hesapları görüntüleme |✔|✔|✔<sup>3</sup>|✔<sup>3</sup>|✘|
|Kayıt hesabına Hesap ekleme ve hesap sahibini değiştirme|✔|✘|✔<sup>3</sup>|✘|✘|
|Abonelikler ve abonelik izinleri oluşturma ve yönetme|✘|✘|✘|✘|✔|

- <sup>2</sup> bildirim kişileri Azure Kurumsal Anlaşma ile ilgili e-posta iletişimleri gönderilir.
- <sup>3</sup> görev, departmanınızdaki hesaplarla sınırlıdır.


## <a name="usage-and-costs-access-by-role"></a>Role göre kullanım ve maliyet erişimi

|Görevler| Kuruluş Yöneticisi|Kuruluş Yöneticisi (salt okuma)|Bölüm Yöneticisi|Bölüm Yöneticisi (salt okuma) |Hesap Sahibi|
|---|---|---|---|---|---|
|Parasal taahhüt dahil kredi bakiyesini görüntüleme|✔|✔|✘|✘|✘|
|Bölüm harcama kotalarını görüntüle|✔|✔|✘|✘|✘|
|Departman harcama kotalarını ayarlama|✔|✘|✘|✘|✘|
|Kuruluşun EA fiyat listesini görüntüle|✔|✔|✘|✘|✘|
|Kullanım ve maliyet ayrıntılarını görüntüleme|✔|✔|✔<sup>4</sup>|✔<sup>4</sup>|✔<sup>5</sup>|
|Azure portal kaynaklarını yönetme|✘|✘|✘|✘|✔|

- <sup>4</sup> Enterprise Portal 'Da da kuruluş yöneticisinin **da görüntüleme ücretleri** ilkesini etkinleştirmesi gerekir. Departman Yöneticisi daha sonra departmanın maliyet ayrıntılarını görebilir.
- <sup>5</sup> Enterprise Portal 'Da, kuruluş yöneticisinin **Ao görüntüleme ücretleri** ilkesini etkinleştirmesi gerekir. Hesap sahibi daha sonra hesabın maliyet ayrıntılarını görebilir.


## <a name="pricing-in-azure-portal"></a>Azure portal fiyatlandırma

Yönetim rolünüze ve görüntüleme ücretleri ilkelerinin Kurumsal Yönetici tarafından nasıl ayarlandığına bağlı olarak Azure portal farklı fiyatlar görebilirsiniz. Enterprise Portal 'daki Azure portal gördüğünüz fiyatlandırmayı etkileyen iki ilke şunlardır:

- DA görüntüleme ücretleri
- AO görüntüleme ücretleri

Bu ilkeleri ayarlamayı öğrenmek için bkz. [Azure için faturalandırma bilgilerine erişimi yönetme](billing-manage-access.md).

Aşağıdaki tabloda Kurumsal Anlaşma yönetici rolleri, görüntüleme ücretleri ilkesi, Azure portal rol tabanlı erişim denetimi (RBAC) rolü ve Azure portal gördüğünüz fiyatlandırma arasındaki ilişki gösterilmektedir. Kurumsal Yönetici, her zaman kuruluşun EA fiyatlandırmasına göre kullanım ayrıntılarını görür. Ancak, Departman Yöneticisi ve hesap sahibi, görünüm ücreti ilkesini ve bunların RBAC rolünü temel alan farklı fiyatlandırma görünümlerini görür. Aşağıdaki tabloda listelenen Departman Yöneticisi rolü, hem bölüm yöneticisi hem de Departman Yöneticisi (salt okuma) rollerine başvurur.

|Yönetici rolü Kurumsal Anlaşma|Rol için ücret ilkesini görüntüle|RBAC rolü|Fiyatlandırma görünümü|
|---|---|---|---|
|Hesap sahibi veya Departman Yöneticisi|✔ Etkin|Sahip|Kuruluşun EA fiyatlandırması|
|Hesap sahibi veya Departman Yöneticisi|✘ Devre dışı|Sahip|Perakende fiyatlandırması|
|Hesap sahibi veya Departman Yöneticisi|✔ Etkin |yok|Fiyatlandırma yok|
|Hesap sahibi veya Departman Yöneticisi|✘ Devre dışı |yok|Fiyatlandırma yok|
|Yok.|Geçerli değil |Sahip|Perakende fiyatlandırması|

Kurumsal yönetici rolünü ve görüntüleme ücretleri ilkelerini Enterprise Portal ' da ayarlarsınız. RBAC rolü Azure portal güncelleştirilebilen olabilir. Daha fazla bilgi için bkz. [RBAC kullanarak erişimi yönetme ve Azure Portal](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure için fatura bilgilerini erişimi yönetme](billing-manage-access.md)
- [RBAC ve Azure portalı kullanarak erişimi yönetme](../role-based-access-control/role-assignments-portal.md)
- [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md)
