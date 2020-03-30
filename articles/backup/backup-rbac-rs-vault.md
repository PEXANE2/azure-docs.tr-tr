---
title: Rol Tabanlı Erişim Denetimi ile Yedeklemeleri Yönetme
description: Kurtarma Hizmetleri kasasındaki yedekleme yönetimi işlemlerine erişimi yönetmek için Rol Tabanlı Erişim Denetimi'ni kullanın.
ms.reviewer: utraghuv
ms.topic: conceptual
ms.date: 06/24/2019
ms.openlocfilehash: e2e32ac6981635e3b9885119fdf397783ac32cc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273208"
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Azure Yedekleme kurtarma noktalarını yönetmek için Rol Tabanlı Erişim Denetimini kullanma

Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere sadece erişim miktarını verebilirsiniz.

> [!IMPORTANT]
> Azure Yedekleme tarafından sağlanan roller, Azure portalında veya REST API veya Recovery Services vault PowerShell veya CLI cmdlets aracılığıyla gerçekleştirilebilecek eylemlerle sınırlıdır. Azure yedekleme Aracısı İstemci UI veya Sistem merkezi Veri Koruma Yöneticisi UI veya Azure Yedekleme Sunucusu UI'de gerçekleştirilen eylemler bu rollerin denetiminde değildir.

Azure Yedekleme, yedekleme yönetimi işlemlerini denetlemek için üç yerleşik rol sağlar. [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

* [Yedekleme Katılımcısı](../role-based-access-control/built-in-roles.md#backup-contributor) - Bu rol, Kurtarma Hizmetleri kasasını siletmek ve başkalarına erişim vermek dışında yedekleme oluşturmak ve yönetmek için tüm izinlere sahiptir. Bu rolü, her yedekleme yönetimi işlemini yapabilen yedekleme yönetiminin yöneticisi olarak düşünün.
* [Yedekleme Operatörü](../role-based-access-control/built-in-roles.md#backup-operator) - Bu rol, destek ilkelerini kaldırmak ve yedekleme ilkelerini yönetmek dışında katkıda bulunanın yaptığı her şeye izin verir. Bu rol, verileri silme veya şirket içi kaynakların kaydını kaldırma gibi yıkıcı işlemleri gerçekleştirememesi dışında katkıda bulunana eşdeğerdir.
* [Yedekleme Okuyucu](../role-based-access-control/built-in-roles.md#backup-reader) - Bu rolün tüm yedekleme yönetimi işlemlerini görüntüleme izinleri vardır. Bu rolü bir izleme kişi olarak düşünün.

Daha fazla denetim için kendi rollerinizi tanımlamak istiyorsanız, Azure RBAC'da Özel rollerin nasıl [oluşturabileceğinize](../role-based-access-control/custom-roles.md) bakın.

## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Yedekleme yerleşik rollerini yedekleme yönetimi eylemleriyle eşleme

Aşağıdaki tablo, Yedekleme yönetim eylemlerini ve bu işlemi gerçekleştirmek için gereken ilgili minimum RBAC rolünü yakalar.

| Yönetim Operasyonu | Minimum RBAC rolü gerekli | Kapsam Gerekli |
| --- | --- | --- |
| Kurtarma Hizmetleri kasası oluşturma | Yedekleme Katılımcısı | Kasaiçeren kaynak grubu |
| Azure VM'lerinin yedeklemesini etkinleştirme | Yedekleme Operatörü | Kasaiçeren kaynak grubu |
| | Sanal Makine Katılımcısı | VM kaynağı |
| VM'nin isteğe bağlı yedeklemesi | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| Sanal makineyi geri yükleme | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| | Katılımcı | VM'nin dağıtılacayacağı kaynak grubu |
| | Sanal Makine Katılımcısı | Yedeklenen kaynak VM |
| Yönetilmeyen diskler VM yedeklemegeri yükleme | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| | Sanal Makine Katılımcısı | Yedeklenen kaynak VM |
| | Depolama Hesabı Katılımcısı | Disklerin geri yüklendiği depolama hesabı kaynağı |
| VM yedeklemesinden yönetilen diskleri geri yükleme | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| | Sanal Makine Katılımcısı | Yedeklenen kaynak VM |
| | Depolama Hesabı Katılımcısı | Verileri yönetilen disklere dönüştürmeden önce kasadan veri tutmak için geri yüklemenin bir parçası olarak seçilen Geçici Depolama hesabı |
| | Katılımcı | Yönetilen disk(lerin geri yükleneceği kaynak grubu |
| VM yedeklemesinden tek tek dosyaları geri yükleme | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| | Sanal Makine Katılımcısı | Yedeklenen kaynak VM |
| Azure VM yedeklemesi için yedekleme ilkesi oluşturma | Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |
| Azure VM yedeklemesinin yedekleme ilkesini değiştirme | Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |
| Azure VM yedeklemesinin yedekleme ilkesini silme | Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |
| VM yedeklemede yedeklemeyi durdurma (verileri sakla veya verileri sil) | Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |
| Şirket içi Windows Server/istemci/SCDPM veya Azure Yedekleme Sunucusu'na kaydolun | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| Şirket içinde kayıtlı Windows Server/istemci/SCDPM veya Azure Yedekleme Sunucusu'nı silme | Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |

> [!IMPORTANT]
> VM kaynak kapsamında VM Katılımcısı belirtir ve VM ayarlarının bir parçası olarak Yedekleme'yi tıklattığınızda, yedekleme durumunu doğrulamak için çağrı yalnızca abonelik düzeyinde çalışsa da VM zaten yedeklenmiş olsa bile 'Yedeklemeyi Etkinleştir' ekranını açar. Bunu önlemek için, tonozgidin ve VM'nin yedek madde görünümünü açın veya abonelik düzeyinde VM Katılımcısı rolünü belirtin.

## <a name="minimum-role-requirements-for-the-azure-file-share-backup"></a>Azure Dosya paylaşım yedeklemesi için minimum rol gereksinimleri

Aşağıdaki tablo, Azure Dosyası paylaşımı işlemini gerçekleştirmek için gereken Yedekleme yönetim eylemlerini ve ilgili rolü yakalar.

| Yönetim Operasyonu | Gerekli Rol | Kaynaklar |
| --- | --- | --- |
| Azure Dosyası paylaşımlarının yedeklemesini etkinleştirme | Yedekleme Katılımcısı |Kurtarma Hizmetleri kasası |
| |Depolama Hesabı | Katılımcı Depolama hesabı kaynağı |
| VM'nin isteğe bağlı yedeklemesi | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| Dosya paylaşımını geri yükle | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| | Depolama Hesabı Katılımcısı | Geri yükleme kaynağı ve Hedef dosya paylaşımlarının bulunduğu depolama hesabı kaynakları |
| Tek tek Dosyaları Geri Yükleme | Yedekleme Operatörü | Kurtarma Hizmetleri kasası |
| |Depolama Hesabı Katılımcısı|Geri yükleme kaynağı ve Hedef dosya paylaşımlarının bulunduğu depolama hesabı kaynakları |
| Korumayı durdurma |Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |
| Kasadan kayıt dışı depolama hesabı |Yedekleme Katılımcısı | Kurtarma Hizmetleri kasası |
| |Depolama Hesabı Katılımcısı | Depolama hesabı kaynağı|

## <a name="next-steps"></a>Sonraki adımlar

* [Role Based Access Control](../role-based-access-control/role-assignments-portal.md): Azure portalında RBAC ile başlayın.
* Şularla erişimi nasıl yöneteceklerini öğrenin:
  * [Powershell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Rol Tabanlı Erişim Denetimi sorun giderme](../role-based-access-control/troubleshooting.md): Sık karşılaşılan sorunları gidermek için öneriler alın.
