---
title: Azure depolama ile kullanılabilen güvenlik özellikleri | Microsoft Docs
description: Bu makalede, Azure depolama ile kullanılabilecek çekirdek Azure Güvenlik özelliklerine genel bir bakış sunulmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2019
ms.author: terrylan
ms.openlocfilehash: 249380b5bd9d95e969a9c7a812102b694b9d1e3b
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726537"
---
# <a name="azure-storage-security-overview"></a>Azure depolama güvenliğine genel bakış

Bu makalede, Azure depolama ile kullanabileceğiniz Azure Güvenlik özelliklerine genel bir bakış sunulmaktadır. Azure Depolama, müşterilerin ihtiyaçlarını karşılamak üzere sağlamlık, kullanılabilirlik ve ölçeklenebilirliğe dayanan modern uygulamalara yönelik bulut depolama çözümüdür. Azure depolama, kapsamlı bir güvenlik özellikleri kümesi sağlar. Şunları yapabilirsiniz:

* Rol tabanlı Access Control (RBAC) ve Azure Active Directory kullanarak depolama hesabını güvenli hale getirin.
* İstemci tarafı şifreleme, HTTPS veya SMB 3,0 kullanarak bir uygulama ile Azure arasında geçiş sırasında verileri güvenli hale getirin.
* Depolama Hizmeti Şifrelemesi kullanarak Azure depolama 'ya yazıldığında verileri otomatik olarak şifrelenecek şekilde ayarlayın.
* Sanal makineler (VM 'Ler) tarafından kullanılan işletim sistemi ve veri disklerini Azure disk şifrelemesi kullanılarak şifrelenecek şekilde ayarlayın.
* Paylaşılan erişim imzalarını (SASs) kullanarak Azure Storage 'daki veri nesnelerine temsilci erişimi verin.
* Bir kullanıcının depolamaya erişirken kullandığı kimlik doğrulama yöntemini izlemek için Analytics kullanın.

Azure depolama 'nın güvenliğine daha ayrıntılı bir bakış için bkz. [Azure depolama Güvenlik Kılavuzu](../../storage/common/storage-security-guide.md). Bu kılavuzda, Azure depolama 'nın güvenlik özellikleri hakkında ayrıntılı bilgi verilmektedir. Bu özellikler depolama hesabı anahtarlarını, aktarım ve bekleyen veri şifrelemesini ve depolama analizlerini içerir.

## <a name="role-based-access-control"></a>Rol Tabanlı Access Control

Rol tabanlı Access Control kullanarak depolama hesabınızın güvenliğini sağlamaya yardımcı olabilirsiniz. Erişimi, veri erişimi için güvenlik ilkeleri zorlamak isteyen kuruluşlar için [bilmelidir](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkelerine göre kısıtlamak zorunludur. Bu erişim hakları, belirli bir kapsamdaki gruplara ve uygulamalara uygun RBAC rolü atanarak verilir. Kullanıcılara ayrıcalık atamak için, depolama hesabı katılımcısı gibi [YERLEŞIK RBAC rollerini](/azure/role-based-access-control/built-in-roles)kullanabilirsiniz.

Daha fazla bilgi:

* [Rol tabanlı Access Control Azure Active Directory](/azure/role-based-access-control/role-assignments-portal)

## <a name="delegated-access-to-storage-objects"></a>Depolama nesnelerine atanan erişim

Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. SAS, belirli bir süre ve belirli bir izin kümesi için Depolama hesabınızdaki nesnelere sınırlı bir istemci izinleri vermeyeceğiniz anlamına gelir. Hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan bu sınırlı izinleri verebilirsiniz.

SAS, bir depolama kaynağına kimlik doğrulamalı erişim için gereken tüm bilgileri sorgu parametrelerinde kapsayan bir URI 'dir. SAS ile depolama kaynaklarına erişmek için, istemcinin yalnızca SAS 'yi uygun oluşturucuya veya yönteme sağlaması gerekir.

Daha fazla bilgi:

* [SAS modelini anlama](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
* [BLOB depolama ile SAS oluşturma ve kullanma](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)

## <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme

Aktarım sırasında şifreleme, ağlar arasında iletilirken verilerin korunmasında bir mekanizmadır. Azure depolama ile, aşağıdakileri kullanarak verileri güvenli hale getirebilirsiniz:

* Azure depolama içine veya dışına veri aktarırken, HTTPS gibi [Aktarım düzeyi şifreleme](/azure/storage/common/storage-security-guide#encryption-in-transit).
* Azure dosya paylaşımları için SMB 3,0 Şifrelemesi gibi [kablo şifreleme](/azure/storage/common/storage-security-guide#using-encryption-during-transit-with-azure-file-shares).
* Depolama alanına aktarılmadan önce verileri şifrelemek ve depolama alanı dışına aktarıldıktan sonra verilerin şifresini çözmek için [istemci tarafı şifreleme](/azure/storage/common/storage-security-guide#using-client-side-encryption-to-secure-data-that-you-send-to-storage).

İstemci tarafı şifreleme hakkında daha fazla bilgi edinin:

* [Microsoft Azure Depolama için istemci tarafı şifreleme](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Cloud Security denetimleri serisi: Yoldaki verileri şifreleme](https://cloudblogs.microsoft.com/microsoftsecure/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Birçok kuruluş için, [bekleyen veri şifreleme](https://cloudblogs.microsoft.com/microsoftsecure/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) , veri gizliliği, uyumluluk ve veri egemenlik 'e yönelik zorunlu bir adımdır. Üç Azure özelliği, bekleyen verilerin şifrelenmesini sağlar:

* [Depolama hizmeti şifrelemesi](/azure/storage/common/storage-security-guide#encryption-at-rest) her zaman etkindir ve Azure depolama alanına yazılırken depolama hizmeti verilerini otomatik olarak şifreler.
* [İstemci tarafı şifreleme](/azure/storage/common/storage-security-guide#client-side-encryption) , bekleyen şifreleme özelliğini de sağlar.
* [Azure disk şifrelemesi](/azure/storage/common/storage-security-guide#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) , bir IaaS sanal makinesi tarafından kullanılan işletim sistemi disklerini ve veri disklerini şifrelemenizi sağlar.

Depolama Hizmeti Şifrelemesi hakkında daha fazla bilgi edinin:

* Azure [depolama hizmeti şifrelemesi](https://azure.microsoft.com/services/storage/) , [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/)için kullanılabilir. Diğer Azure depolama türleriyle ilgili ayrıntılar için bkz. [Azure dosyaları](https://azure.microsoft.com/services/storage/files/), [Tablo depolama](https://azure.microsoft.com/services/storage/tables/)ve [kuyruk depolama](https://azure.microsoft.com/services/storage/queues/).
* [Bekleyen veriler için Azure Depolama Hizmeti Şifrelemesi](/azure/storage/common/storage-service-encryption)

## <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi

Sanal makineler için Azure disk şifrelemesi, kurumsal güvenlik ve uyumluluk gereksinimlerini karşılamanıza yardımcı olur. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)' de denetlediğiniz anahtar ve ILKELERI kullanarak VM disklerinizi (önyükleme ve veri diskleri dahil) şifreler.

VM 'Ler için disk şifrelemesi, Linux ve Windows işletim sistemleri için geçerlidir. Ayrıca, disk şifreleme anahtarlarınızın korunmasına, yönetilmesine ve kullanımını denetlemeye yardımcı olması için Key Vault kullanır. VM disklerinizdeki tüm veriler, Azure depolama hesaplarınızdaki sektör standardı şifreleme teknolojisini kullanarak geri kalanında şifrelenir. Windows için disk şifreleme çözümü, [Microsoft BitLocker Sürücü Şifrelemesi](https://technet.microsoft.com/library/cc732774.aspx)tabanlıdır ve Linux çözümü [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt)' i temel alır.

Daha fazla bilgi edinin

* [Windows ve Linux IaaS sanal makineleri için Azure disk şifrelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="firewalls-and-virtual-networks"></a>Güvenlik duvarları ve sanal ağlar

Azure depolama, depolama hesaplarınız için güvenlik duvarı kurallarını etkinleştirmenizi sağlar. Bu işlem etkinleştirildikten sonra, diğer Azure hizmetlerinden gelen istekler dahil olmak üzere veri isteklerini engeller. Trafiğe izin vermek için özel durumlar yapılandırabilirsiniz. Güvenlik duvarı kuralları, mevcut depolama hesaplarında veya oluşturulma zamanı sırasında etkinleştirilebilir.

Depolama hesaplarınızı belirli bir izin verilen ağlar kümesine güvenli hale getirmek için bu işlevi kullanmanız gerekir.

Azure Storage güvenlik duvarları ve sanal ağlar hakkında daha fazla bilgi için [Azure Storage güvenlik duvarlarını ve sanal ağları yapılandırma](/azure/storage/common/storage-network-security) makalesini gözden geçirin

## <a name="azure-data-box"></a>Azure Data Box

Data Box, Data Box Disk ve Data Box Heavy cihazları, ağ seçeneğinin olmadığı durumlarda Azure’a büyük boyutlu veri aktarmanıza yardımcı olur. Bu çevrimdışı veri aktarımı cihazları, kuruluşunuz ve Azure veri merkezi arasında gönderilir. Bunlar aktarım sırasında verilerinizin korunmasına yardımcı olmak için AES şifrelemesi kullanır ve karşıya yükleme sonrası temizlik işlemine tabi tutularak verileriniz cihazdan silinir.

Data Box Edge ve Data Box Gateway, verilerin siteniz ile Azure arasında yönetilmesi için ağ depolama geçitleri olarak çalışan çevrimiçi veri aktarımı ürünleridir. Şirket içi bir ağ cihazı olan Data Box Edge, Azure’ın içine ve dışına veri aktarımı gerçekleştirmesinin yanı sıra verileri işlemek için yapay zeka (AI) özellikli uç işlemini kullanır. Data Box Gateway, depolama ağ geçidi özelliklerine sahip sanal bir gereçtir.

Daha fazla bilgi:

* [Azure Data Box](https://azure.microsoft.com/services/storage/databox/)
* [Azure Data Box Edge](/azure/databox-online/data-box-edge-overview)
* [Azure Data Box Gateway](/azure/databox-online/data-box-gateway-overview)

## <a name="advanced-threat-protection"></a>Gelişmiş Tehdit Koruması

Azure depolama, depolama hesabınıza erişmek veya onları yararlanmak için olağandışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik bilgileri katmanı için Gelişmiş tehdit koruması sağlar. Gelişmiş tehdit koruması, blob depolamaya şüpheli okuma, yazma veya silme istekleri için Azure depolama tanılama günlüklerini izler.

Gelişmiş tehdit koruması uyarıları, [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)'nden görüntülenebilir. Azure Güvenlik Merkezi, algılanan şüpheli etkinliklerin ayrıntılarını sağlar ve olası tehdidi araştırmak ve düzeltmek için Eylemler önerir.

Daha fazla bilgi:

* [Azure depolama Gelişmiş tehdit korumasına genel bakış](/azure/storage/common/storage-advanced-threat-protection)

## <a name="azure-key-vault"></a>Azure Key Vault

Azure disk şifrelemesi, Anahtar Kasası aboneliğinizdeki disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) kullanır. Ayrıca, sanal makine disklerindeki tüm verilerin Azure Storage 'da bekleyen olarak şifrelenmesini de sağlar. Anahtarları ve ilke kullanımını denetlemek için Key Vault kullanmanız gerekir.

Daha fazla bilgi edinin

* [Azure Key Vault nedir?](/azure/key-vault/key-vault-overview)
