---
title: Azure Yedekleme korumalı iş yüklerini izleyin
description: Bu makalede, Azure portalını kullanarak Azure Yedekleme iş yüklerinin izleme ve bildirim yetenekleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 03/05/2019
ms.assetid: 86ebeb03-f5fa-4794-8a5f-aa5cbbf68a81
ms.openlocfilehash: de5a82f5ad1d8113b27c07484f2f08f4cf97c759
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294932"
---
# <a name="monitoring-azure-backup-workloads"></a>Azure Yedekleme iş yüklerini izleme

Azure Yedekleme, yedekleme gereksinimine ve altyapı topolojisini (Şirket içi vs Azure) temel alan birden çok yedekleme çözümü sağlar. Herhangi bir yedek kullanıcı veya yönetici tüm çözümler arasında neler olup bittiğini görmelidir ve önemli senaryolarda bildirilmesi bekleniyor. Bu makalede, Azure Yedekleme hizmeti tarafından sağlanan izleme ve bildirim özellikleri ayrıntılı olarak anlatılıyor.

## <a name="backup-jobs-in-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında Yedekleme İşleri

Azure Yedekleme, Azure Yedekleme tarafından korunan iş yükleri için dahili izleme ve uyarı özellikleri sağlar. Kurtarma Hizmetleri kasa ayarlarında, **İzleme** bölümü yerleşik işler ve uyarılar sağlar.

![RS kasa dahili izleme](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltmonitoring.png)

Yedekleme, yedekleme, geri yükleme, yedeklemeyi silme ve benzeri işlemler gerçekleştirildiğinde işler oluşturulur.

Aşağıdaki Azure Yedekleme çözümlerinin işleri burada gösterilmiştir:

- Azure VM yedeklemesi
- Azure Dosya yedekleme
- SQL ve SAP HANA gibi azure iş yükü yedeklemesi
- Azure Backup aracısı (MAB)

Sistem Merkezi Veri Koruma Yöneticisi (SC-DPM), Microsoft Azure Yedekleme Sunucusu (MABS) İşlerinden işler GÖRÜNTÜLENMEZ.

> [!NOTE]
> Azure VM'lerindeki SQL ve SAP HANA yedeklemeleri gibi Azure iş yüklerinin çok sayıda yedekleme işi vardır. Örneğin, günlük yedeklemeleri her 15 dakikada bir çalıştırılabilir. Bu nedenle, bu tür DB iş yükleri için yalnızca kullanıcı tetiklenen işlemler görüntülenir. Zamanlanan yedekleme işlemleri GÖRÜNTÜLENMEZ.

## <a name="backup-alerts-in-recovery-services-vault"></a>Kurtarma Hizmetleri kasasında Yedek Uyarılar

Uyarılar, öncelikle kullanıcıların ilgili eylemi yapabilmeleri için bilgilendirildiği senaryolardır. **Yedekleme Uyarıları** bölümünde Azure Yedekleme hizmeti tarafından oluşturulan uyarıları gösterir. Bu uyarılar hizmet tarafından tanımlanır ve kullanıcı özel olarak herhangi bir uyarı oluşturamaz.

### <a name="alert-scenarios"></a>Uyarı senaryoları

Aşağıdaki senaryolar hizmet tarafından uyarılabilir senaryolar olarak tanımlanır.

- Yedekleme/Geri Yükleme hataları
- Azure Backup Aracısı (MAB) için yedekleme uyarılarla başarılı oldu
- Verileri koruma yla korumayı durdurma/Verileri silile korumayı durdur

### <a name="alerts-from-the-following-azure-backup-solutions-are-shown-here"></a>Aşağıdaki Azure Yedekleme çözümlerinden gelen uyarılar burada gösterilmiştir

- Azure VM yedeklemeleri
- Azure Dosya yedeklemeleri
- SQL, SAP HANA gibi Azure iş yükü yedeklemeleri
- Azure Backup aracısı (MAB)

> [!NOTE]
> System Center Data Protection Manager (SC-DPM), Microsoft Azure Backup Server (MABS) uyarıları burada görüntülenmez.

### <a name="consolidated-alerts"></a>Konsolide Uyarılar

SQL ve SAP HANA gibi Azure iş yükü yedekleme çözümleri için günlük yedeklemeleri çok sık oluşturulabilir (ilkeye göre her 15 dakikada bir). Bu nedenle günlük yedekleme hataları da çok sık (her 15 dakikada bir kadar) mümkündür. Bu senaryoda, her hata oluşumu için bir uyarı yükseltilirse, son kullanıcı bunalmış olur. Bu nedenle ilk olay için bir uyarı gönderilir ve sonraki hatalar aynı kök nedenden kaynaklanıyorsa, daha fazla uyarı oluşturulmaz. İlk uyarı hata sayısı ile güncelleştirilir. Ancak uyarı kullanıcı tarafından devre dışı bırakılırsa, bir sonraki olay başka bir uyarı tetikler ve bu durum için ilk uyarı olarak kabul edilir. Azure Yedekleme, SQL ve SAP HANA yedeklemeleri için uyarı konsolidasyonu'nun bu şekilde gerçekleştirilmesidir.

### <a name="exceptions-when-an-alert-is-not-raised"></a>Uyarı yükseltilmediğinde özel durumlar

Bir hata da bir uyarı yükseltilmediği zaman birkaç özel durum vardır. Bunlar:

- Kullanıcı çalışan işi açıkça iptal etti
- Başka bir yedekleme işi devam ediyor çünkü iş başarısız olur (biz sadece önceki işin tamamlanması nı beklemek zorunda beri burada hareket etmek için hiçbir şey)
- Yedeklenmiş Azure VM artık yok olduğundan VM yedekleme işi başarısız olur
- [Konsolide Uyarılar](#consolidated-alerts)

Yukarıdaki özel durumlar, bu işlemlerin sonucunun (öncelikle kullanıcı tetiklenen) portal/PS/CLI istemcilerinde hemen ortaya çıkması yla tasarlanmıştır. Bu nedenle kullanıcı hemen haberdar olur ve bildirime ihtiyaç duymaz.

### <a name="alert-types"></a>Uyarı türleri

Uyarı şiddetine bağlı olarak, uyarılar üç türde tanımlanabilir:

- **Kritik**: Prensipte, herhangi bir yedekleme veya kurtarma hatası (planlanmış veya kullanıcı tetiklenir) bir uyarının oluşumuna yol açar ve kritik uyarı olarak gösterilir ve yedeklemeyi silme gibi yıkıcı işlemler de gösterilir.
- **Uyarı**: Yedekleme işlemi başarılı ancak birkaç uyarı yla birlikte uyarı uyarıları olarak listelenir.
- **Bilgilendirme**: Bugün itibariyle Azure Yedekleme hizmeti tarafından hiçbir bilgilendirme uyarısı oluşturulmaz.

## <a name="notification-for-backup-alerts"></a>Yedekleme Uyarıları için Bildirim

> [!NOTE]
> Bildirim yapılandırması yalnızca Azure Portalı üzerinden yapılabilir. PS/CLI/REST API/Azure Kaynak Yöneticisi Şablonu desteği desteklenmez.

Bir uyarı yükseltildikten sonra, kullanıcılara bildirilir. Azure Yedekleme, e-posta yoluyla yerleşik bir bildirim mekanizması sağlar. Bir uyarı oluşturulduğunda bildirilecek tek tek e-posta adresleri veya dağıtım listeleri belirtilebilir. Ayrıca, her bir uyarı için bildirim alınıp alınmayacağı nızı veya bunları saatlik özette gruplandırmayı ve ardından bilgilendirilmeyi de seçebilirsiniz.

![RS Vault dahili e-posta bildirimi](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltnotification.png)

Bildirim yapılandırıldığı zaman, bir karşılama veya tanıtım e-postası alırsınız. Bu, Bir uyarı yükseltildiğinde Azure Yedekleme'nin bu adreslere e-posta gönderebileceğini doğrular.<br>

Frekans saatlik bir özet olarak ayarlanmışsa ve bir saat içinde bir uyarı yükseltilip çözülürse, bu, yaklaşan saatlik özetin bir parçası olmayacaktır.

> [!NOTE]
>
> - **Silme verileriyle korumayı durdurma** gibi yıkıcı bir işlem gerçekleştirilirse, bir uyarı yükseltilir ve bildirimler Hizmeti Kurtarma kasası için yapılandırılmamış olsa bile abonelik sahiplerine, yöneticilere ve yardımcı yöneticilere bir e-posta gönderilir.
> - Başarılı işler için bildirimi yapılandırmak için [Log Analytics'i](backup-azure-monitoring-use-azuremonitor.md#using-log-analytics-workspace)kullanın.

## <a name="inactivating-alerts"></a>Uyarıları devre dışı

Etkin bir uyarıyı etkinleştirmek/çözmek için, etkinleştirmek istediğiniz uyarıya karşılık gelen liste öğesini tıklatabilirsiniz. Bu, üstte 'Devre dışı bırak' düğmesi yle uyarı yla ilgili ayrıntılı bilgileri görüntüleyen bir ekran açılır. Bu düğmeyi tıklatmak, uyarının durumunu 'Etkin olmayan' olarak değiştirir. Ayrıca, bu uyarıya karşılık gelen liste öğesine sağ tıklayarak ve 'Etkinle'yi seçerek bir uyarıyı devre dışı bırakabilirsiniz.

![RS Vault uyarısı inaktivasyon](media/backup-azure-monitoring-laworkspace/vault-alert-inactivation.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Monitörünü kullanarak Azure yedekleme iş yüklerini izleme](backup-azure-monitoring-use-azuremonitor.md)
