---
title: Azure’da Kuruluş için yönetici rollerini anlama | Microsoft Docs
description: Azure’da Kuruluş yöneticisi rolleri hakkında bilgi edinin.
author: bandersmsft
ms.reviewer: adwise
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: banders
ms.openlocfilehash: 9fab4d8ba0cf2e6f684a1b9de177084f8ce31604
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462199"
---
# <a name="understand-azure-enterprise-agreement-administrative-roles-in-azure"></a>Azure'daki Azure Kurumsal Anlaşma yönetici rollerini anlama

Kuruluşunuzun kullanım ve harcamasının yönetilmesine yardımcı olmak için Kurumsal Anlaşması (EA) olan Azure müşterileri beş farklı yönetici rolü atayabilir:

- Kuruluş Yöneticisi
- Kuruluş Yöneticisi (salt okunur)<sup>1</sup>
- Bölüm Yöneticisi
- Bölüm Yöneticisi (salt okunur)
- Hesap Sahibi

<sup>1</sup> EA sözleşmesinin Fatura ilgili kişisi bu rolde olacaktır.

Bu roller, Azure Kurumsal Anlaşmaları yönetmeye özgüdür ve kaynaklara erişimi denetlemeye yönelik Azure’da yer alan yerleşik rollere ek olarak sunulmuştur. Daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](../../role-based-access-control/built-in-roles.md).

Aşağıdaki bölümlerde her bir rolün sınırlamaları ve özellikleri açıklanmaktadır.

## <a name="user-limit-for-admin-roles"></a>Yönetici rolleri için kullanıcı sınırı

|Rol| Kullanıcı sınırı|
|---|---|
|Kuruluş Yöneticisi|Sınırsız|
|Kuruluş Yöneticisi (salt okunur)|Sınırsız|
|Bölüm Yöneticisi|Sınırsız|
|Bölüm Yöneticisi (salt okunur)|Sınırsız|
|Hesap Sahibi|Hesap başına 1<sup>2</sup>|

<sup>2</sup> Her hesap için benzersiz bir Microsoft hesabı veya iş ya da okul hesabı gerekir.

## <a name="organization-structure-and-permissions-by-role"></a>Role göre kuruluş yapısı ve izinleri

|Görevler| Kuruluş Yöneticisi|Kuruluş Yöneticisi (salt okunur)|Bölüm Yöneticisi|Bölüm Yöneticisi (salt okunur)|Hesap Sahibi|
|---|---|---|---|---|---|
|Kuruluş Yöneticilerini görüntüleme|✔|✔|✘|✘|✘|
|Kuruluş Yöneticileri ekleme veya kaldırma|✔|✘|✘|✘|✘|
|Bildirim İlgili Kişilerini görüntüleme<sup>3</sup> |✔|✔|✘|✘|✘|
|Bildirim İlgili Kişilerini ekleme veya kaldırma<sup>3</sup> |✔|✘|✘|✘|✘|
|Bölümler oluşturma ve yönetme |✔|✘|✘|✘|✘|
|Bölüm Yöneticilerini görüntüleme|✔|✔|✔|✔|✘|
|Bölüm Yöneticileri ekleme veya kaldırma|✔|✘|✔|✘|✘|
|Kayıttaki Hesapları görüntüleme |✔|✔|✔<sup>4</sup>|✔<sup>4</sup>|✘|
|Kayda Hesaplar ekleme ve Hesap Sahibini değiştirme|✔|✘|✔<sup>4</sup>|✘|✘|
|Abonelikler ve abonelik izinleri oluşturma ve yönetme|✘|✘|✘|✘|✔|

- <sup>3</sup> Bildirim ilgili kişilerine Azure Kurumsal Anlaşma ile ilgili e-posta iletişimleri gönderilir.
- <sup>4</sup> Görev, bölümünüzdeki hesaplarla sınırlıdır.


## <a name="usage-and-costs-access-by-role"></a>Role göre kullanım ve maliyet erişimi

|Görevler| Kuruluş Yöneticisi|Kuruluş Yöneticisi (salt okunur)|Bölüm Yöneticisi|Bölüm Yöneticisi (salt okunur) |Hesap Sahibi|
|---|---|---|---|---|---|
|Parasal taahhüt dahil kredi bakiyesini görüntüleme|✔|✔|✘|✘|✘|
|Bölüm harcaması kotalarını görüntüleme|✔|✔|✘|✘|✘|
|Bölüm harcaması kotalarını ayarlama|✔|✘|✘|✘|✘|
|Kuruluşun Kurumsal Anlaşma fiyat listesini görüntüleme|✔|✔|✘|✘|✘|
|Kullanım ve maliyet ayrıntılarını görüntüleme|✔|✔|✔<sup>5</sup>|✔<sup>5</sup>|✔<sup>6</sup>|
|Azure portalında kaynakları yönetme|✘|✘|✘|✘|✔|

- <sup>5</sup> Kuruluş Yöneticisinin Enterprise portalında **DA ücretleri görüntüleme** ilkesini etkinleştirmesini gerektirir. Bölüm Yöneticisi daha sonra bölüme ait maliyet ayrıntılarını görebilir.
- <sup>6</sup> Kuruluş Yöneticisinin Enterprise portalında **AO ücretleri görüntüleme** ilkesini etkinleştirmesini gerektirir. Hesap Sahibi daha sonra hesaba ait maliyet ayrıntılarını görebilir.


## <a name="pricing-in-azure-portal"></a>Azure portalında fiyatlandırma

Yönetici rolünüze ve Kuruluş Yöneticisi tarafından ücretleri görüntüleme ilkelerinin nasıl ayarlandığına bağlı olarak Azure portalında farklı fiyatlandırma görebilirsiniz. Azure portalında gördüğünüz fiyatlandırmayı etkileyen Enterprise Portal’daki iki ilke şunlardır:

- DA ücretleri görüntüleme
- AO ücretleri görüntüleme

Bu ilkelerin nasıl ayarlanacağı hakkında bilgi edinmek için bkz. [Azure için faturalama bilgilerine erişimi yönetme](manage-billing-access.md).

Aşağıdaki tabloda, Azure portalında gördüğünüz fiyatlandırma, Azure portalındaki rol tabanlı erişim denetimi (RBAC) rolü, ücretleri görüntüleme ilkesi ve Kurumsal Anlaşma yönetici rolleri arasındaki ilişki gösterilmektedir. Kuruluş Yöneticisi her zaman kuruluşun Kurumsal Anlaşma fiyatlandırmasına göre kullanım ayrıntılarını görür. Ancak Bölüm Yöneticisi ve Hesap Sahibi, ücreti görüntüleme ilkesine ve RBAC rolüne göre farklı fiyatlandırma görünümleri görür. Aşağıdaki tabloda listelenen Bölüm Yöneticisi rolü hem Bölüm Yöneticisi hem de Bölüm Yöneticisi (salt okunur) rolünü ifade eder.

|Kurumsal Anlaşma yönetici rolü|Rol için ücretleri görüntüleme ilkesi|RBAC rolü|Fiyatlandırma görünümü|
|---|---|---|---|
|Hesap Sahibi VEYA Bölüm Yöneticisi|✔ Etkin|Sahip|Kuruluşun Kurumsal Anlaşma fiyatlandırması|
|Hesap Sahibi VEYA Bölüm Yöneticisi|✘ Devre dışı|Sahip|Perakende fiyatlandırması|
|Hesap Sahibi VEYA Bölüm Yöneticisi|✔ Etkin |yok|Fiyatlandırma yok|
|Hesap Sahibi VEYA Bölüm Yöneticisi|✘ Devre dışı |yok|Fiyatlandırma yok|
|Hiçbiri|Uygulanamaz |Sahip|Perakende fiyatlandırması|

Kuruluş yöneticisi rolünü ve ücretleri görüntüleme ilkelerini Enterprise portal’da ayarlarsınız. RBAC rolü, Azure portalında güncelleştirilebilir. Daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure için faturalama bilgilerine erişimi yönetme](manage-billing-access.md)
- [RBAC ve Azure portalı kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md)
- [Azure kaynakları için yerleşik roller](../../role-based-access-control/built-in-roles.md)
