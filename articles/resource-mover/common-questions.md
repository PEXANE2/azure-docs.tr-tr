---
title: Azure Kaynak taşıyıcısı hakkında sık sorulan sorular
description: Azure Kaynak taşıyıcısı hakkında sık sorulan soruların yanıtlarını alın
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 09/07/2020
ms.author: raynew
ms.openlocfilehash: 520c2d4fd258bfab5a5a1e0abf890d58bb98fbdc
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653185"
---
# <a name="common-questions"></a>Sık sorulan sorular

Bu makalede, [Azure Kaynak taşıyıcısı](overview.md)hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="general"></a>Genel

### <a name="is-resource-mover-generally-available"></a>Kaynak taşıyıcısı genel olarak kullanılabilir mi?

Kaynak taşıyıcısı Şu anda genel önizleme aşamasındadır. Üretim iş yükleri desteklenir.

## <a name="region-move"></a>Bölge taşıma

### <a name="what-resources-can-i-move-across-regions-using-resource-mover"></a>Kaynak taşıyıcısı kullanarak bölgeler arasında hangi kaynakları taşıyabilirim?

Kaynak taşıyıcısı kullanarak şu anda bölgeler arasında şu kaynakları taşıyabilirsiniz:

- Azure VM 'Leri ve ilişkili diskler
- NIC’ler
- Kullanılabilirlik kümeleri 
- Azure sanal ağları 
- Genel IP adresleri ağ güvenlik grupları (NSG 'ler)
- İç ve genel yük dengeleyiciler 
- Azure SQL veritabanları ve elastik havuzlar

### <a name="where-is-the-metadata-about-a-region-move-stored"></a>Bölge taşıma hakkındaki meta veriler nerede depolanır?

Bir Microsoft aboneliğinde [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) veritabanında ve [Azure Blob depolamada](../storage/common/storage-service-encryption.md)depolanır.

### <a name="is-the-collected-metadata-encrypted"></a>Toplanan meta veriler şifrelendi mı?

Evet, hem aktarım hem de bekleyen.
- Aktarım sırasında, meta veriler HTTPS kullanılarak internet üzerinden kaynak taşıyıcısı hizmetine güvenli bir şekilde gönderilir.
- Depolama alanında meta veriler şifrelenir.

### <a name="how-is-managed-identity-used-in-resource-mover"></a>Kaynak taşıyıcısı içinde yönetilen kimlik nasıl kullanılır?

[Yönetilen kimlik](../active-directory/managed-identities-azure-resources/overview.md) (eski adıyla YÖNETILEN HIZMET KIMLIĞI (MIS)), Azure hizmetleri 'NI Azure AD 'de otomatik olarak yönetilen bir kimlikle sağlar.
- Kaynak taşıyıcısı, kaynakları bölgeler arasında taşımak için Azure aboneliklerine erişebilmek üzere yönetilen kimlik kullanır.
- Taşıma koleksiyonu, taşıdığınız kaynakları içeren aboneliğe erişimi olan sistem tarafından atanan bir kimliğe ihtiyaç duyuyor.

- Portaldaki bölgeler arasında kaynakları taşırsanız, bu işlem otomatik olarak gerçekleşir.
- PowerShell kullanarak kaynakları taşırsanız, koleksiyona sistem tarafından atanan bir kimlik atamak için cmdlet 'leri çalıştırır ve ardından kimlik sorumlusu için doğru abonelik izinlerine sahip bir rol atamalısınız. 

### <a name="what-managed-identity-permissions-does-resource-mover-need"></a>Kaynak taşıyıcısı hangi yönetilen kimlik izinlerine ihtiyaç duyuyor?

Azure Kaynak taşıyıcısı yönetilen kimliği, en az şu izinlere ihtiyaç duyuyor: 

- Kullanıcı aboneliğine kaynak yazma/oluşturma izni, *katkıda* bulunan rolüyle kullanılabilir. 
- Rol atamaları oluşturma izni. Genellikle *sahip* veya *Kullanıcı erişimi yönetici* rolleriyle veya *Microsoft. Authorization/role atama/yazma izni* atanmış özel bir rolle kullanılabilir. Veri paylaşımının yönetilen kimliği zaten Azure veri deposuna erişim izni verildiyse bu izin gerekli değildir. 
 
Portal 'da kaynak taşıyıcısı hub 'ına kaynak eklediğinizde, Kullanıcı yukarıda açıklanan izinlere sahip olduğu sürece izinler otomatik olarak işlenir. PowerShell 'e kaynak eklerseniz, izinleri el ile atarsınız.

> [!IMPORTANT]
> Kimlik rolü atamalarını değiştirmemenizi veya kaldırmanızı kesinlikle öneririz. 

### <a name="what-should-i-do-if-i-dont-have-permissions-to-assign-role-identity"></a>Rol kimliği atama iznim yoksa ne yapmam gerekir?

**Olası nedeni** | **Öneri**
--- | ---
İlk kez bir kaynak eklediğinizde *katkıda bulunan* ve *Kullanıcı erişimi Yöneticisi* (veya *sahibi*) değilsiniz. | Abonelik için *katkıda* bulunan ve *Kullanıcı erişimi Yöneticisi* (veya *sahibi*) izinleri olan bir hesap kullanın.
Kaynak taşıyıcısı tarafından yönetilen kimliğin gerekli rolü yok. | ' Katkıda bulunan ' ve ' Kullanıcı erişimi Yöneticisi ' rollerini ekleyin.
Kaynak taşıyıcısı yönetilen kimliği *none*olarak sıfırlandı. | Taşıma koleksiyonunda sistem tarafından atanan bir kimliği yeniden etkinleştirin > **kimliği**. Alternatif olarak, kaynağı Ekle ' ye aynı şeyi sağlayan **kaynakları**yeniden ekleyin.  
Abonelik farklı bir kiracıya taşındı. | Taşıma koleksiyonu için yönetilen kimliği devre dışı bırakıp etkinleştirin.


## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı bileşenleri ve taşıma işlemi hakkında [daha fazla bilgi edinin](about-move-process.md) .
