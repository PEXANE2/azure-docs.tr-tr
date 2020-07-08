---
title: Rol tabanlı Access Control yedeklemeleri yönetme
description: Kurtarma Hizmetleri kasasındaki yedekleme yönetimi işlemlerine erişimi yönetmek için rol tabanlı Access Control kullanın.
ms.reviewer: utraghuv
ms.topic: conceptual
ms.date: 06/24/2019
ms.openlocfilehash: e2e32ac6981635e3b9885119fdf397783ac32cc9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79273208"
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Azure Backup kurtarma noktalarını yönetmek için rol tabanlı Access Control kullanma

Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere sadece erişim miktarını verebilirsiniz.

> [!IMPORTANT]
> Azure Backup tarafından sunulan roller Azure portal veya REST API ya da kurtarma hizmetleri Kasası PowerShell veya CLı cmdlet 'leri aracılığıyla gerçekleştirilebilecek eylemlerle sınırlıdır. Azure Backup Aracısı Istemci kullanıcı arabirimi veya System Center Data Protection Manager Kullanıcı arabirimi veya Azure Backup Sunucusu Kullanıcı arabirimi ' nde gerçekleştirilen eylemler bu rollerin denetimini dışındadır.

Azure Backup, yedekleme yönetimi işlemlerini denetlemek için üç yerleşik rol sağlar. [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

* [Yedek katılımcısı](../role-based-access-control/built-in-roles.md#backup-contributor) -bu rolün, kurtarma hizmetleri kasasını silme ve başkalarına erişim verme dışında yedekleme oluşturma ve yönetme izinleri vardır. Bu rolü, her yedekleme yönetimi işlemini yapan yedekleme yönetiminin Yöneticisi olarak düşünün.
* [Yedekleme işletmeni](../role-based-access-control/built-in-roles.md#backup-operator) -bu rolün, yedeklemenin kaldırılması ve yedekleme ilkelerinin yönetilmesi dışında, katkıda bulunan her şeye yönelik izinleri vardır. Bu rol katkıda bulunan, verileri silme veya şirket içi kaynakların kaydını kaldırma gibi bozucu işlemler gerçekleştiremedikçe, katkıda buluna eşdeğerdir.
* [Yedekleme okuyucusu](../role-based-access-control/built-in-roles.md#backup-reader) -bu rolün tüm yedekleme yönetimi işlemlerini görüntüleme izinleri vardır. Bu rolü bir izleme kişisi olarak düşünün.

Daha da fazla denetim için kendi rollerinizi tanımlamak istiyorsanız bkz. Azure RBAC 'de [özel roller oluşturma](../role-based-access-control/custom-roles.md) .

## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Yedekleme yerleşik rollerini yedekleme yönetim eylemlerine eşleme

Aşağıdaki tabloda, bu işlemi gerçekleştirmek için gereken yedekleme yönetim eylemleri ve karşılık gelen minimum RBAC rolü yer alır.

| Yönetim Işlemi | Minimum RBAC rolü gerekli | Kapsam gerekli |
| --- | --- | --- |
| Kurtarma Hizmetleri kasası oluşturma | Yedek Katılımcısı | Kasayı içeren kaynak grubu |
| Azure VM 'lerinin yedeklenmesini etkinleştirme | Yedekleme Işletmeni | Kasayı içeren kaynak grubu |
| | Sanal Makine Katılımcısı | VM kaynağı |
| VM 'nin isteğe bağlı yedeklemesi | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| Sanal makineyi geri yükleme | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| | Katılımcı | VM 'nin dağıtılacağı kaynak grubu |
| | Sanal Makine Katılımcısı | Yedeklenen kaynak VM |
| Yönetilmeyen diskleri geri yükleme VM yedeklemesi | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| | Sanal Makine Katılımcısı | Yedeklenen kaynak VM |
| | Depolama Hesabı Katılımcısı | Disklerin geri yükleneceği depolama hesabı kaynağı |
| Yönetilen diskleri VM yedeklemesinden geri yükleme | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| | Sanal Makine Katılımcısı | Yedeklenen kaynak VM |
| | Depolama Hesabı Katılımcısı | Geri yükleme 'nin bir parçası olarak seçilen geçici depolama hesabı, verileri yönetilen disklere dönüştürmeden önce kasadan bekletme |
| | Katılımcı | Yönetilen disklerin geri yükleneceği kaynak grubu |
| Tek tek dosyaları VM yedeklemesinden geri yükleme | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| | Sanal Makine Katılımcısı | Yedeklenen kaynak VM |
| Azure VM yedeklemesi için yedekleme ilkesi oluşturma | Yedek Katılımcısı | Kurtarma Hizmetleri kasası |
| Azure VM yedeklemesi 'nin yedekleme ilkesini değiştirme | Yedek Katılımcısı | Kurtarma Hizmetleri kasası |
| Azure VM yedeklemesi 'nin yedekleme ilkesini silme | Yedek Katılımcısı | Kurtarma Hizmetleri kasası |
| VM yedeklemede Yedeklemeyi Durdur (verileri sakla veya verileri Sil) | Yedek Katılımcısı | Kurtarma Hizmetleri kasası |
| Şirket içi Windows Server/Client/SCDPM veya Azure Backup Sunucusu Kaydet | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| Kayıtlı şirket içi Windows Server/Client/SCDPM veya Azure Backup Sunucusu Sil | Yedek Katılımcısı | Kurtarma Hizmetleri kasası |

> [!IMPORTANT]
> Sanal makine kaynak kapsamında VM katılımcısı belirtirseniz ve VM ayarlarının bir parçası olarak yedekle ' ye tıklarsanız, yedekleme durumunu doğrulama çağrısı yalnızca abonelik düzeyinde çalışır durumda olsa da VM zaten yedeklense bile ' yedeklemeyi etkinleştir ' ekranını açar. Bunu önlemek için kasa 'ya gidin ve VM 'nin yedekleme öğesi görünümünü açın ya da bir abonelik düzeyinde VM katılımcısı rolünü belirtin.

## <a name="minimum-role-requirements-for-the-azure-file-share-backup"></a>Azure dosya paylaşımının yedeklenmesi için en düşük rol gereksinimleri

Aşağıdaki tabloda, yedekleme yönetim eylemleri ve Azure dosya paylaşma işlemini gerçekleştirmek için gereken ilgili rol yer alır.

| Yönetim Işlemi | Rol gerekli | Kaynaklar |
| --- | --- | --- |
| Azure dosya paylaşımlarının yedeklenmesini etkinleştir | Yedek Katılımcısı |Kurtarma Hizmetleri kasası |
| |Depolama Hesabı | Katkıda bulunan depolama hesabı kaynağı |
| VM 'nin isteğe bağlı yedeklemesi | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| Dosya payını geri yükle | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| | Depolama Hesabı Katılımcısı | Geri yükleme kaynağı ve hedef dosya paylaşımlarının mevcut olduğu depolama hesabı kaynakları |
| Tek tek dosyaları geri yükle | Yedekleme Işletmeni | Kurtarma Hizmetleri kasası |
| |Depolama Hesabı Katılımcısı|Geri yükleme kaynağı ve hedef dosya paylaşımlarının mevcut olduğu depolama hesabı kaynakları |
| Korumayı durdurma |Yedek Katılımcısı | Kurtarma Hizmetleri kasası |
| Depolama hesabının kasadan kaydını sil |Yedek Katılımcısı | Kurtarma Hizmetleri kasası |
| |Depolama Hesabı Katılımcısı | Depolama hesabı kaynağı|

## <a name="next-steps"></a>Sonraki adımlar

* [Rol tabanlı Access Control](../role-based-access-control/role-assignments-portal.md): Azure Portal RBAC ile çalışmaya başlayın.
* İle erişimin nasıl yönetileceğini öğrenin:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Rol tabanlı Access Control sorun giderme](../role-based-access-control/troubleshooting.md): yaygın sorunları gidermeye yönelik öneriler alın.
