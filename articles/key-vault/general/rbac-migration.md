---
title: Azure rol tabanlı erişim denetimine geçiş | Microsoft Docs
description: Kasa erişim ilkelerinden Azure rollerine geçiş yapmayı öğrenin.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 8/30/2020
ms.author: mbaldwin
ms.openlocfilehash: e06a7a759c712b47f3a725a3c49a660226da6a09
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90064195"
---
# <a name="migrate-from-vault-access-policy-to-an-azure-role-based-access-control-preview-permission-model"></a>Kasa erişim ilkesinden Azure rol tabanlı erişim denetimi (Önizleme) izin modeline geçiş

Kasa erişim ilkesi modeli, anahtarlar, gizlilikler ve sertifikalara erişim sağlamak için Key Vault yerleşik olarak bulunan mevcut bir yetkilendirme sistemidir. Key Vault kapsamındaki güvenlik sorumlusuna (Kullanıcı, Grup, hizmet sorumlusu, yönetilen kimlik) bireysel izinler atayarak erişimi denetleyebilirsiniz. 

Azure rol tabanlı erişim denetimi (Azure RBAC), Azure kaynakları üzerinde ayrıntılı erişim yönetimi sağlayan [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) yerleşik bir yetkilendirme sistemidir. Key Vault anahtarlar, gizlilikler ve sertifikalar erişim yönetimi için Azure RBAC Şu anda genel önizlemededir. Azure RBAC ile, üç öğeden oluşan, rol atamaları oluşturarak kaynaklara erişimi kontrol edersiniz: güvenlik sorumlusu, rol tanımı (önceden tanımlanmış izinler kümesi) ve kapsam (kaynak grubu veya ayrı kaynak). Daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview).

Azure RBAC 'e geçmeden önce avantajlarının ve sınırlamaların anlaşılması önemlidir.

Kasa erişim ilkeleri üzerinde Azure RBAC önemli avantajları:
- Azure hizmetleri genelinde aynı API ile Azure kaynakları için Birleşik erişim denetimi modeli sağlar
- Yöneticiler için merkezi erişim yönetimi-tüm Azure kaynaklarını tek bir görünümde yönetin
- Zamana dayalı erişim denetimi için [Privileged Identity Management](../../active-directory/privileged-identity-management/pim-configure.md) ile tümleşik
- Atamaları Reddet-belirli bir kapsamdaki güvenlik sorumlusunu dışarıda bırakma özelliği. Daha fazla bilgi için bkz. [Azure reddetme atamalarını anlama](https://docs.microsoft.com/azure/role-based-access-control/deny-assignments)

Azure RBAC dezavantajları:
- Rol atamaları için gecikme-rol atamasının uygulanması birkaç dakika sürebilir. Kasa erişim ilkeleri anında atanır.
- Key Vault başına sınırlı sayıda rol ataması-abonelik başına 2000 rol ataması ve her biri için 1024 erişim ilkeleri

## <a name="access-policies-to-azure-roles-mapping"></a>Azure rollerine eşleme erişimi ilkeleri

Azure RBAC, kullanıcılara, gruplara, hizmet sorumlularına ve yönetilen kimliklere atayabileceğiniz birkaç Azure yerleşik rolüne sahiptir. Yerleşik roller kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi [Azure özel rollerinizi](https://docs.microsoft.com/azure/role-based-access-control/custom-roles)de oluşturabilirsiniz.

Anahtarlar, sertifikalar ve gizli dizi erişim yönetimi için yerleşik roller Key Vault:
- Key Vault Yöneticisi (Önizleme)
- Key Vault okuyucu (Önizleme)
- Key Vault sertifikası Müdürü (Önizleme)
- Key Vault şifre Müdürü (Önizleme)
- Key Vault şifreleme kullanıcısı (Önizleme)
- Key Vault gizli bilgileri Müdürü (Önizleme)
- Key Vault gizli dizi kullanıcısı (Önizleme)

Mevcut yerleşik roller hakkında daha fazla bilgi için bkz. [Azure yerleşik rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)

Kasa erişim ilkeleri, tek tek seçili izinlerle veya önceden tanımlanmış izin şablonlarıyla atanabilir.

Erişim ilkeleri önceden tanımlanmış izin şablonları:
- Anahtar, gizli, sertifika yönetimi
- Anahtar & gizli dizi yönetimi
- Gizli & sertifika yönetimi
- Anahtar yönetimi
- Gizli dizi yönetimi
- Sertifika Yönetimi
- SQL Server Bağlayıcısı
- Azure Data Lake Storage veya Azure depolama
- Azure Backup
- Exchange Online müşteri anahtarı
- SharePoint Online müşteri anahtarı
- Azure bilgileri BYOK

### <a name="access-policies-templates-to-azure-roles-mapping"></a>İlke şablonlarına Azure rolleri eşleme erişimi
| Erişim ilkesi şablonu | Operations | Azure rolü |
| --- | --- | --- |
| Anahtar, gizli, sertifika yönetimi | Anahtarlar: tüm işlemler <br>Sertifikalar: tüm işlemler<br>Gizlilikler: tüm işlemler | Key Vault Yöneticisi (Önizleme) |
| Anahtar & gizli dizi yönetimi | Anahtarlar: tüm işlemler <br>Gizlilikler: tüm işlemler| Key Vault şifre Müdürü (Önizleme)<br> Key Vault gizli bilgileri Müdürü (Önizleme)|
| Gizli & sertifika yönetimi | Sertifikalar: tüm işlemler <br>Gizlilikler: tüm işlemler| Key Vault sertifikaları Müdürü (Önizleme)<br> Key Vault gizli bilgileri Müdürü (Önizleme)|
| Anahtar yönetimi | Anahtarlar: tüm işlemler| Key Vault şifre Müdürü (Önizleme)|
| Gizli dizi yönetimi | Gizlilikler: tüm işlemler| Key Vault gizli bilgileri Müdürü (Önizleme)|
| Sertifika Yönetimi | Sertifikalar: tüm işlemler | Key Vault sertifikaları Müdürü (Önizleme)|
| SQL Server Bağlayıcısı | Anahtarlar: Al, Listele, sarmalama tuşu, sarmalama tuşu | Key Vault şifreleme hizmeti şifrelemesi (Önizleme)|
| Azure Data Lake Storage veya Azure depolama | Anahtarlar: get, List, Unwrap tuşu | Yok<br> Özel rol gerekli|
| Azure Backup | Anahtarlar: Al, Listele, Yedekle<br> Sertifika: get, List, Backup | Yok<br> Özel rol gerekli|
| Exchange Online müşteri anahtarı | Anahtarlar: Al, Listele, sarmalama tuşu, sarmalama tuşu | Key Vault şifreleme hizmeti şifrelemesi (Önizleme)|
| Exchange Online müşteri anahtarı | Anahtarlar: Al, Listele, sarmalama tuşu, sarmalama tuşu | Key Vault şifreleme hizmeti şifrelemesi (Önizleme)|
| Azure bilgileri BYOK | Anahtarlar: get, şifre çöz, imzala | Yok<br>Özel rol gerekli|


## <a name="assignment-scopes-mapping"></a>Atama kapsamları eşleme  

Key Vault için Azure RBAC, aşağıdaki kapsamlardaki rol atamasına izin verir:
- Yönetim grubu
- Abonelik
- Kaynak grubu
- Key Vault kaynağı
- Bireysel anahtar, gizli ve sertifika

Kasa erişimi ilkesi izin modeli, ilke yalnızca Key Vault kaynak düzeyinde atama ile sınırlıdır ve bu 

Genel olarak, uygulama başına bir Anahtar Kasası olması ve Anahtar Kasası düzeyinde erişimi yönetmeniz en iyi uygulamadır. Diğer kapsamlardaki erişimi yönetirken, erişim yönetimini basitleştirecek senaryolar vardır.

- * * Altyapı, güvenlik yöneticileri ve işleçler: yönetim grubu, abonelik veya kaynak grubu düzeyinde, kasa erişim ilkeleriyle ilgili anahtar kasalarının grubunu yönetmek, her Anahtar Kasası için ilkelerin bakım yapılmasını gerektirir. Azure RBAC, yönetim grubunda, abonelikte veya kaynak grubunda bir rol ataması oluşturulmasına olanak sağlar. Bu atama, aynı kapsam altında oluşturulan tüm yeni anahtar kasaları için de geçerlidir. Bu senaryoda, kalıcı erişim sağlamak için tam zamanında erişim ile Privileged Identity Management kullanılması önerilir.
 
- * * Uygulamalar: uygulamanın diğer uygulamayla gizli anahtar paylaşması gerektiğinde senaryolar vardır. Kasa erişimi ilkelerinin kullanılması, tüm gizli anahtarlara erişim vermemek için ayrı anahtar kasasının oluşturulması gerekiyordu. Azure RBAC tek bir anahtar kasası kullanmak yerine bireysel gizlilik için kapsama sahip rol atamaya izin verir.

## <a name="vault-access-policy-to-azure-rbac-migration-steps"></a>Azure RBAC geçiş adımlarına kasa erişimi ilkesi
Azure RBAC ve kasa erişim ilkesi izin modeli arasında birçok fark vardır. Sırasıyla, geçiş sırasında kesintilerden kaçınmak için adımların aşağıdaki adımları izlemeniz önerilir.
 
1. **Rolleri tanımla ve ata**: yukarıdaki eşleme tablosuna göre yerleşik rolleri tanımla ve gerektiğinde özel roller oluştur. Kapsamlara kapsamlar eşleme kılavuzlarına göre roller atayın. Anahtar kasasına rol atama hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi ile Key Vault erişim sağlama (Önizleme)](rbac-guide.md)
1. **Rol atamasını doğrula**: Azure RBAC 'de rol atamalarının yayılması birkaç dakika sürebilir. Rol atamalarını denetleme Kılavuzu için bkz. [kapsamdaki rol atamalarını listeleme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-list-portal#list-role-assignments-for-a-user-at-a-scope)
1. **Anahtar kasasında izleme ve uyarı yapılandırma**: erişim reddedildi özel durumları için günlüğe kaydetmeyi etkinleştirmek ve uyarı ayarlamak önemlidir. Daha fazla bilgi için bkz. [Azure Key Vault Için izleme ve uyarma](https://docs.microsoft.com/azure/key-vault/general/alert)
1. **Key Vault Azure rol tabanlı erişim denetimi izin modelini ayarlama**: Azure RBAC izin modelinin etkinleştirilmesi, mevcut tüm erişim ilkelerini geçersiz kılar. Bir hata varsa, izin modeli mevcut tüm erişim ilkeleriyle birlikte geri dönebilir.

> [!NOTE]
> Azure RBAC izin modeli etkinleştirildiğinde, erişim ilkelerini güncelleştirmeyi deneyen tüm betikler başarısız olur. Bu betiklerin Azure RBAC kullanmak için güncelleştirilmesi önemlidir.

## <a name="troubleshooting"></a>Sorun giderme
-  Rol ataması birkaç dakika sonra çalışmıyor-rol atamalarının daha uzun sürebileceği durumlar vardır. Bu servis taleplerini kapsayan kodda yeniden deneme mantığı yazmak önemlidir.
- Key Vault silindiğinde rol atamaları yok (geçici silme) ve kurtarılan-bu, şu anda tüm Azure hizmetlerinde geçici silme özelliği kısıtlamasıdır. Kurtarma sonrasında tüm rol atamalarının yeniden oluşturulması gerekir.    

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Azure RBAC genel bakış](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Özel roller öğreticisi](https://docs.microsoft.com/azure/role-based-access-control/tutorial-custom-role-cli)
- [Privileged Identity Management](../../active-directory/privileged-identity-management/pim-configure.md)
