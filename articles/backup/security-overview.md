---
title: Güvenlik özelliklerine genel bakış
description: Yedekleme verilerinizi korumanıza ve işletmenizin güvenlik gereksinimlerini karşılamanıza yardımcı olan Azure Yedekleme'deki güvenlik yetenekleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 91a0f29862b0c9c35e562c143e28ebbc6c39cf94
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80423190"
---
# <a name="overview-of-security-features-in-azure-backup"></a>Azure Yedekleme'de güvenlik özelliklerine genel bakış

Verilerinizi korumak için atabileceğiniz en önemli adımlardan biri güvenilir bir yedekleme altyapısına sahip olmaktır. Ancak verilerinizin güvenli bir şekilde yedeklenmesini ve yedeklemelerinizin her zaman korunduğundan emin olmak da aynı derecede önemlidir. Azure Yedekleme, hem verileriniz akiş halinde yken hem de dinlenirken yedekleme ortamınıza güvenlik sağlar. Bu makalede, Yedekleme verilerinizi korumanıza ve işletmenizin güvenlik gereksinimlerini karşılamanıza yardımcı olan Azure Yedekleme'deki güvenlik yetenekleri listelenebilmelidir.

## <a name="management-and-control-of-identity-and-user-access"></a>Kimlik ve kullanıcı erişiminin yönetimi ve kontrolü

Azure Yedekleme, Azure Role Tabanlı Erişim [Denetimi 'ni (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)kullanarak ince taneli erişimi yönetmenize olanak tanır. RBAC, ekibiniz içindeki görevleri ayırmanızı ve yalnızca işlerini yapmak için gerekli kullanıcılara erişim miktarını vermenizi sağlar.

Azure Yedekleme, yedekleme yönetimi işlemlerini denetlemek için üç yerleşik rol sağlar:

* Yedekleme Katılımcısı - Kurtarma Hizmetleri kasasını siletmek ve başkalarına erişim vermek dışında yedeklemeleri oluşturmak ve yönetmek için
* Yedekleme Operatörü - yedekleme yi kaldırma ve yedekleme ilkelerini yönetme dışında katkıda bulunanın yaptığı her şey
* Backup Reader - tüm yedekleme yönetimi işlemlerini görüntüleme izinleri

[Azure Yedekleme'yi yönetmek için Role Tabanlı Erişim denetimi](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault)hakkında daha fazla bilgi edinin.

Azure Yedekleme'de güvenlik açıklarını önlemek, algılamak ve yanıt vermek için hizmette yerleşik çeşitli güvenlik denetimleri vardır. [Azure Yedekleme için güvenlik denetimleri](https://docs.microsoft.com/azure/backup/backup-security-controls)hakkında daha fazla bilgi edinin.

## <a name="separation-between-guest-and-azure-storage"></a>Konuk ve Azure depolama arasında ayrım

VM yedeklemesinde sanal makine yedeklemesi ve SQL ve SAP HANA'yı içeren Azure Yedekleme ile yedekleme verileri Azure depolama alanında depolanır ve konuğun yedekleme depolamasına veya içeriğine doğrudan erişimi yoktur.  Sanal makine yedeklemesi ile yedekleme anlık görüntüsü oluşturma ve depolama, konuğun uygulama tutarlı yedeklemeleri için iş yükünü sorgulamaktan başka bir ilgisi olmadığı Azure kumaşı tarafından yapılır.  SQL ve SAP HANA ile yedekleme uzantısı belirli blobs yazmak için geçici erişim alır.  Bu şekilde, gizliliği ihlal edilmiş bir ortamda bile, varolan yedeklemeler konuk tarafından değiştirilemez veya silinemez.

## <a name="internet-connectivity-not-required-for-azure-vm-backup"></a>Azure VM yedeklemesi için Internet bağlantısı gerekli değildir

Azure Sanal Makinelerinin yedeklemesi, sanal makinenizin diskinden Kurtarma Hizmetleri kasasına veri hareketi gerektirir. Ancak, gerekli tüm iletişim ve veri aktarımı, sanal ağınıza erişmeye gerek kalmadan yalnızca Azure omurga ağında gerçekleşir. Bu nedenle, güvenli ağlara yerleştirilen Azure VM'lerinin yedeklemesi, herhangi bir IP'ye veya FQDN'ye erişime izin vermenizi gerektirmez.

## <a name="private-endpoints-for-azure-backup"></a>Azure yedeklemesi için özel Uç Noktalar

Artık verilerinizi sanal ağ daki sunuculardan Kurtarma Hizmetleri kasanıza güvenli bir şekilde yedeklemek için [Özel Uç Noktaları'nı](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) kullanabilirsiniz. Özel bitiş noktası kasanız için VNET adres alanından bir IP kullanır, böylece sanal ağlarınızı herkese açık IP'lere maruz bırakmanız gerekmez. Azure VM'lerinizin içinde çalışan SQL ve SAP HANA veritabanlarınızı yedeklemek ve geri almak için özel Uç Noktalar kullanılabilir. Mars aracısını kullanarak şirket içi sunucularınız için de kullanılabilir.

>[!NOTE]
> Bu özellik şu anda sınırlı kullanılabilirlik tedir. Azure Yedekleme için Özel Bitiş Noktaları'nı kullanmak istiyorsanız lütfen [bu anketi](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUQk5EQ1QxRzVOWDNDS1Y1Q0xLTkdLQ0U0RC4u) doldurun ve [bize e-posta gönderin.](mailto:azbackupnetsec@microsoft.com) Bu özelliği kullanabilme özelliği Azure Yedekleme hizmetinin onayına tabidir.

## <a name="encryption-of-data-in-transit-and-at-rest"></a>Aktarım ve istirahatte verilerin şifrelemesi

Şifreleme verilerinizi korur ve kuruluş güvenliği ve uyumluluk taahhütlerinizi yerine getirmenize yardımcı olur. Azure'da, Azure depolama alanı ile kasa arasındaki aktarımdaki veriler HTTPS tarafından korunur. Bu veriler Azure omurga ağında kalır.

* Yedekleme verileri Microsoft tarafından yönetilen anahtarlar kullanılarak otomatik olarak şifrelenir. Azure Anahtar Kasası'nda depolanan [müşteri yönetilen anahtarlarını](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#encryption-of-backup-data-using-customer-managed-keys) kullanarak, Yedeklenmiş yönetilen disk VM'lerinizi Kurtarma Hizmetleri Kasası'nda da şifreleyebilirsiniz. Bu şifrelemeyi etkinleştirmek için herhangi bir açık eylemde bulunmanız gerekmez. Kurtarma Hizmetleri kasanıza yedeklenen tüm iş yükleri için geçerlidir.

* Azure Yedekleme, işletim sistemi/veri diskleri Azure Disk Şifreleme (ADE) ile şifrelenmiş Olan Azure VM'lerinin yedeklenmesini ve geri yüklenmesini destekler. [Şifreli Azure VM'ler ve Azure Yedekleme hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption)

## <a name="protection-of-backup-data-from-unintentional-deletes"></a>Yedekleme verilerinin kasıtsız silmelerden korunması

Azure Yedekleme, silindikten sonra bile yedekleme verilerinin korunmasına yardımcı olacak güvenlik özellikleri sağlar. Yumuşak silme ile, kullanıcı bir VM'nin yedeğini silerse, yedekleme verileri 14 gün daha korunur ve bu da yedekleme öğesinin veri kaybı olmadan kurtarılmasına olanak sağlar. Yedekleme verilerinin "yumuşak silme" durumunda ki ek 14 gün tutulması, müşteriye herhangi bir maliyet ememez. [Yumuşak silme hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#soft-delete)

## <a name="monitoring-and-alerts-of-suspicious-activity"></a>Şüpheli etkinliğin izlenmesi ve uyarıları

Azure Yedekleme, Azure Yedekleme ile ilgili etkinlikleriçin eylemleri görüntülemek ve yapılandırmak için [yerleşik izleme ve uyarı özellikleri](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) sağlar. [Yedekleme Raporları,](https://docs.microsoft.com/azure/backup/configure-reports) kullanımı izlemek, yedeklemeleri ve geri yüklemeleri denetlemek ve farklı parçalı lık düzeylerindeki önemli eğilimleri tanımlamak için tek noktadan bir hedef olarak hizmet vermektedir. Azure Yedekleme'nin izleme ve raporlama araçlarını kullanmak, yetkisiz, şüpheli veya kötü amaçlı etkinlikler oluşur oluşmaz sizi uyarabilir.

## <a name="security-features-to-help-protect-hybrid-backups"></a>Karma yedeklemelerin korunmasına yardımcı olacak güvenlik özellikleri

Azure Yedekleme hizmeti, dosyaları, klasörleri ve birim durumunu şirket içi bir bilgisayardan Azure'a yedeklemek ve geri yüklemek için Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını kullanır. MARS artık hibrit yedeklemelerin korunmasına yardımcı olacak güvenlik özellikleri sağlar. Bu özellikler şunlardır:

* Parolayı değiştirmek gibi kritik bir işlem yapıldığında ek bir kimlik doğrulama katmanı eklenir. Bu doğrulama, bu tür işlemlerin yalnızca geçerli Azure kimlik bilgilerine sahip kullanıcılar tarafından gerçekleştirilebilmesini sağlamak içindir. [Saldırıları önleyen özellikler hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#prevent-attacks)

* Silinen yedekleme verileri silinme tarihinden itibaren 14 gün daha saklanır. Bu, belirli bir süre içinde verilerin kurtarılabilirliğini sağlar, böylece bir saldırı gerçekleşse bile veri kaybı olmaz. Ayrıca, bozuk verilere karşı korumak için daha fazla sayıda minimum kurtarma noktası korunur. [Silinen yedekleme verilerini kurtarma hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#recover-deleted-backup-data)

## <a name="compliance-with-standardized-security-requirements"></a>Standart güvenlik gereksinimlerine uygunluk

Kuruluşların bireylerin verilerinin toplanması ve kullanımıyla ilgili ulusal, bölgesel ve sektöre özel gereksinimlere uymasına yardımcı olmak için, Microsoft Azure & Azure Yedekleme kapsamlı bir sertifika seti ve attestation sunar. [Uyumluluk sertifikaları listesine bakın](compliance-offerings.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Yedekleme kullanan bulut iş yüklerinin korunmasına yardımcı olacak güvenlik özellikleri](backup-azure-security-feature-cloud.md)
* [Azure Yedekleme kullanan karma yedeklemeleri korumaya yardımcı olacak güvenlik özellikleri](backup-azure-security-feature.md)
