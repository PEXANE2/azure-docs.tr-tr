---
title: Azure’da Kuruluş için yönetici rollerini anlama
description: Azure’da Kuruluş yöneticisi rolleri hakkında bilgi edinin.
author: bandersmsft
ms.reviewer: adwise
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 06/01/2020
ms.author: banders
ms.openlocfilehash: e38271b3a56894380ca5456146a7ab36fb09e08c
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84678027"
---
# <a name="managing-azure-enterprise-roles"></a>Azure kurumsal rollerini yönetme

Kuruluşunuzun kullanım ve harcamasının yönetilmesine yardımcı olmak için Kurumsal Anlaşması (EA) olan Azure müşterileri beş farklı yönetici rolü atayabilir:

- Kuruluş Yöneticisi
- Kuruluş Yöneticisi (salt okunur)<sup>1</sup>
- Bölüm Yöneticisi
- Bölüm Yöneticisi (salt okunur)
- Hesap sahibi<sup>2</sup>

<sup>1</sup> EA sözleşmesinin Fatura ilgili kişisi bu rolde olacaktır.

<sup>2</sup> Fatura ilgili kişisi Azure EA portalına eklenemez veya burada değiştirilemez. Bu kişi, anlaşma düzeyinde fatura ilgili kişisi olarak ayarlanmış kullanıcıya göre EA kaydına eklenir. Fatura ilgili kişisini değiştirmek için Bölgesel İşlem Merkezi’nin (ROC) iş ortağı/yazılım danışmanı aracılığıyla istek oluşturmanız gerekir.

Kayıt sağlama sırasında ayarlanan ilk kayıt yöneticisi, Fatura ilgili kişisi hesabının kimlik doğrulaması türünü belirler. Fatura ilgili kişisi EA Portal’a salt okunur bir yönetici olarak eklendiğinde, bu kişiye Microsoft hesabı kimlik doğrulaması verilir. 

Örneğin ilk kimlik doğrulaması türü Karma olarak ayarlandıysa, EA bir Microsoft hesabı olarak eklenir ve Fatura ilgili kişisi salt okunur EA yönetici ayrıcalıklarına sahip olur. EA yöneticisi mevcut Fatura ilgili kişisi için Microsoft hesabı yetkilendirmesini onaylamazsa, EA yöneticisi söz konusu kullanıcıyı silebilir ve müşteriden kullanıcıyı EA portalında yalnızca kayıt düzeyinde ayarlanan bir İş veya Okul hesabıyla salt okunur yönetici olarak yeniden eklemesini isteyebilir.

Bu roller, Azure Kurumsal Anlaşmaları yönetmeye özgüdür ve kaynaklara erişimi denetlemeye yönelik Azure’da yer alan yerleşik rollere ek olarak sunulmuştur. Daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](../../role-based-access-control/built-in-roles.md).

Aşağıdaki bölümlerde her bir rolün sınırlamaları ve özellikleri açıklanmaktadır.

## <a name="user-limit-for-admin-roles"></a>Yönetici rolleri için kullanıcı sınırı

|Rol| Kullanıcı sınırı|
|---|---|
|Kuruluş Yöneticisi|Sınırsız|
|Kuruluş Yöneticisi (salt okunur)|Sınırsız|
|Bölüm Yöneticisi|Sınırsız|
|Bölüm Yöneticisi (salt okunur)|Sınırsız|
|Hesap Sahibi|Hesap başına 1<sup>3</sup>|

<sup>3</sup> Her hesap için benzersiz bir Microsoft hesabı veya iş ya da okul hesabı gerekir.

## <a name="organization-structure-and-permissions-by-role"></a>Role göre kuruluş yapısı ve izinleri

|Görevler| Kuruluş Yöneticisi|Kuruluş Yöneticisi (salt okunur)|Bölüm Yöneticisi|Bölüm Yöneticisi (salt okunur)|Hesap Sahibi| İş Ortağı|
|---|---|---|---|---|---|---|
|Kuruluş Yöneticilerini görüntüleme|✔|✔|✘|✘|✘|✔|
|Kuruluş Yöneticileri ekleme veya kaldırma|✔|✘|✘|✘|✘|✘|
|Bildirim İlgili Kişilerini görüntüleme<sup>4</sup> |✔|✔|✘|✘|✘|✔|
|Bildirim İlgili Kişilerini ekleme veya kaldırma<sup>4</sup> |✔|✘|✘|✘|✘|✘|
|Bölümler oluşturma ve yönetme |✔|✘|✘|✘|✘|✘|
|Bölüm Yöneticilerini görüntüleme|✔|✔|✔|✔|✘|✔|
|Bölüm Yöneticileri ekleme veya kaldırma|✔|✘|✔|✘|✘|✘|
|Kayıttaki Hesapları görüntüleme |✔|✔|✔<sup>5</sup>|✔<sup>5</sup>|✘|✔|
|Kayda Hesaplar ekleme ve Hesap Sahibini değiştirme|✔|✘|✔<sup>5</sup>|✘|✘|✘|
|Abonelikler ve abonelik izinleri oluşturma ve yönetme|✘|✘|✘|✘|✔|✘|

- <sup>4</sup> Bildirim ilgili kişilerine Azure Kurumsal Anlaşma ile ilgili e-posta iletişimleri gönderilir.
- <sup>5</sup> Görev, bölümünüzdeki hesaplarla sınırlıdır.

## <a name="add-a-new-enterprise-administrator"></a>Yeni bir kuruluş yöneticisi ekleme

Kuruluş yöneticileri bir Azure EA kaydını yönetirken en fazla ayrıcalığa sahip olan kullanıcılardır. İlk Azure EA yöneticisi, EA sözleşmesi ayarlanırken oluşturulmuştur. Ancak, dilediğiniz zaman yeni yöneticiler ekleyebilir veya kaldırabilirsiniz. Yeni yöneticiler yalnızca var olan yöneticiler tarafından eklenir. Ek kuruluş yöneticileri ekleme hakkında daha fazla bilgi için bkz. [Başka bir kuruluş yöneticisi oluşturma](ea-portal-get-started.md#create-another-enterprise-administrator). Faturalama profili rolleri ve görevleri hakkında daha fazla bilgi için bkz. [Faturalama profili rolleri ve görevleri](understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="update-account-owner-state-from-pending-to-active"></a>Beklemede olan hesap sahibi durumunu etkin olarak güncelleştirme

Bir Azure EA kaydına ilk kez yeni Hesap Sahipleri (AO) eklendiğinde durumları _beklemede_ olarak görünür. Yeni bir hesap sahibi etkinleştirme karşılama e-postasını aldığında, hesabını etkinleştirmek için oturum açabilir. Hesabını etkinleştirdiğinde, _beklemede_ olan hesap durumu _etkin_ olarak güncelleştirilir. Hesap sahibinin "Uyarı" iletisini okuması ve **Devam**'ı seçmesi gerekir. Yeni kullanıcılardan Bir Ticari Hesap oluşturmak için ad ve soyadı girmeleri istenebilir. Bu durumda, devam etmek için gerekli bilgileri eklemesi gerekir. Bundan sonra hesap etkinleştirilir.

## <a name="add-a-department-admin"></a>Departman yöneticisi ekleme

Azure EA yöneticisi bir departman oluşturduktan sonra, Azure Kuruluş yöneticisi departman yöneticileri ekleyebilir ve her birini bir departmanla ilişkilendirebilir. Bir departman yöneticisi yeni hesaplar oluşturabilir. Azure EA aboneliklerinin oluşturulabilmesi için yeni hesaplar gereklidir.

Departman yöneticisi ekleme hakkında daha fazla bilgi için bkz. [Azure EA departman yöneticisi oluşturma](ea-portal-get-started.md#add-a-department-administrator).

## <a name="usage-and-costs-access-by-role"></a>Role göre kullanım ve maliyet erişimi

|Görevler| Kuruluş Yöneticisi|Kuruluş Yöneticisi (salt okunur)|Bölüm Yöneticisi|Bölüm Yöneticisi (salt okunur) |Hesap Sahibi| İş Ortağı|
|---|---|---|---|---|---|---|
|Parasal taahhüt dahil kredi bakiyesini görüntüleme|✔|✔|✘|✘|✘|✔|
|Bölüm harcaması kotalarını görüntüleme|✔|✔|✘|✘|✘|✔|
|Bölüm harcaması kotalarını ayarlama|✔|✘|✘|✘|✘|✘|
|Kuruluşun Kurumsal Anlaşma fiyat listesini görüntüleme|✔|✔|✘|✘|✘|✔|
|Kullanım ve maliyet ayrıntılarını görüntüleme|✔|✔|✔<sup>6</sup>|✔<sup>6</sup>|✔<sup>7</sup>|✔|
|Azure portalında kaynakları yönetme|✘|✘|✘|✘|✔|✘|

- <sup>6</sup> Enterprise Portal’da **DA ücretleri görüntüleme** ilkesinin Kuruluş Yöneticisi tarafından etkinleştirilmesini gerektirir. Bölüm Yöneticisi daha sonra bölüme ait maliyet ayrıntılarını görebilir.
- <sup>7</sup> Enterprise Portal’da **AO ücretleri görüntüleme** ilkesinin Kuruluş Yöneticisi tarafından etkinleştirilmesini gerektirir. Hesap Sahibi daha sonra hesaba ait maliyet ayrıntılarını görebilir.

## <a name="see-pricing-for-different-user-roles"></a>Farklı kullanıcı rolleri için fiyatlandırmayı görüntüleyin

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
