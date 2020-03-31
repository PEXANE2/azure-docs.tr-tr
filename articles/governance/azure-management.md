---
title: Azure Yönetimine Genel Bakış - Azure Yönetimi
description: Azure yönetim araçlarındaki içeriğe bağlantılar içeren Azure uygulamaları ve kaynakları için yönetim alanlarına genel bakış.
ms.date: 12/06/2018
ms.topic: overview
ms.openlocfilehash: 644f4fc3a04f86426c2eb04b02c29882fa39fb88
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75980910"
---
# <a name="overview-of-management-services-in-azure"></a>Azure'da Yönetim hizmetlerine Genel Bakış

Azure'da yönetim, Azure Yönetimi'nin bir yönüdür. Bu makale, kaynaklarınızı Azure'da dağıtmak ve korumak için farklı yönetim alanlarını kapsar.

Yönetim, iş uygulamalarınızı ve onları destekleyen kaynaklarınızı korumak için gereken görevleri ve süreçleri ifade eder. Azure'da tam yönetim sağlamak için birlikte çalışan birçok hizmet ve araç vardır. Bu hizmetler yalnızca Azure'daki kaynaklar için değil, diğer bulutlarda ve şirket içi kaynaklar için de dir. Farklı araçları ve birlikte nasıl çalıştıklarını anlamak, tam bir yönetim ortamı tasarlamanın ilk adımıdır.

Aşağıdaki diyagramda herhangi bir uygulamayı veya kaynağı korumak için gereken farklı yönetim alanları gösterilmektedir. Bu farklı alanlar bir yaşam döngüsü olarak düşünülebilir. Her alan, bir kaynağın ömrü boyunca sürekli olarak art arda gereklidir. Bu kaynak yaşam döngüsü, devam eden çalışma yoluyla ve son olarak kullanımdan kaldırılınca ilk dağıtımla başlar.

![Azure'da Yönetim Disiplinleri](../monitoring/media/management-overview/management-capabilities.png)

Tek bir Azure hizmeti, belirli bir yönetim alanının gereksinimlerini tamamen doldurmaz. Bunun yerine, her biri birlikte çalışan çeşitli hizmetler tarafından gerçekleştirilir. Uygulama Öngörüleri gibi bazı hizmetler, web uygulamaları için hedefli izleme işlevi sağlar. Azure Monitor günlükleri gibi diğerleri, diğer hizmetler için yönetim verilerini depolar. Bu özellik, farklı hizmetler tarafından toplanan farklı türdeki verileri çözümlemenize olanak tanır.

Aşağıdaki bölümlerde farklı yönetim alanları kısaca açıklanır ve ilgili temel Azure hizmetlerindeki ayrıntılı içeriklere yönelik bağlantılar sunulur.

## <a name="monitor"></a>İzleme

İzleme, kaynaklarınızın performansını, durumunu ve kullanılabilirliğini denetlemek için veri toplama ve çözümleme eylemidir. Etkili bir izleme stratejisi, bileşenlerin çalışmasını anlamanıza ve bildirimlerle çalışma sürenizi artırmanıza yardımcı olur. [Azure uygulamalarını ve kaynaklarını izlemede](../monitoring/monitoring-overview.md)kullanılan farklı hizmetleri kapsayan İzleme'ye genel bir bakış okuyun.

## <a name="configure"></a>Yapılandırma

Yapılandırma, kaynakların ilk dağıtımı ve yapılandırması ve devam eden bakım anlamına gelir.
Bu görevlerin otomasyonu artıklığı ortadan kaldırmanızı, zaman ınızı ve çabanızı en aza indirmenizi ve doğruluğunuzu ve verimliliğinizi artırmanızı sağlar. [Azure Otomasyonu](../automation/automation-intro.md), yapılandırma görevlerini otomatikleştirmek için toplu hizmetler sunar. Runbook'lar proses otomasyonu, yapılandırma ve güncelleştirme yönetimi yapılandırmayı yönetmede yardımcı olur.

## <a name="govern"></a>İdare

İdare, Azure’daki uygulama ve kaynaklarınız üzerindeki denetimi sürdürmenize yönelik mekanizmalar ve süreçler sağlar. Girişimlerinizi planlama ve stratejik öncelikleri belirleme de bu kapsama dahildir.
Azure’da İdare, temelde iki hizmet ile uygulanır. [Azure İlkesi,](./policy/overview.md) kaynaklarınız için kurallar uygulamak için ilke tanımları oluşturmanıza, atamanıza ve yönetmenize olanak tanır. Bu özellik, bu kaynakları şirket standartlarınıza uygun tutar. [Azure Maliyet Yönetimi,](../cost-management-billing/cost-management-billing-overview.md) Azure kaynaklarınız ve diğer bulut sağlayıcılarınız için bulut kullanımını ve harcamalarını izlemenize olanak tanır.

## <a name="secure"></a>Güvenlik

Kaynaklarınızın ve verilerinizin güvenliğini yönetin. Bir güvenlik programı tehditleri değerlendirmeyi, veri toplamayı ve çözümlemeyi ve uygulamalarınızın ve kaynaklarınız için uygunluğu içerir. Güvenlik izleme ve tehdit çözümlemesi, karma bulut iş yüklerinde birleşik güvenlik yönetimi ve gelişmiş tehdit koruması içeren [Azure Güvenlik Merkezi](../security-center/security-center-intro.md)tarafından sağlanır. Azure kaynaklarının güvenliğini sağlama konusunda kapsamlı bilgi ve rehberlik için [Azure Güvenliğine Giriş'e](../security/fundamentals/overview.md) bakın.

## <a name="protect"></a>Koruma

Koruma, uygulamalarınızın ve verilerinizin kontrolünüz dışında olan kesintilerle bile kullanılabilir olmasını sağlamak anlamına gelir. Azure’da Koruma, iki hizmetle sağlanır. [Azure Backup](../backup/backup-introduction-to-azure-backup.md), hem buluttaki hem de şirket içindeki verilerinize yönelik yedekleme ve kurtarma sağlar. [Azure Site Kurtarma,](../site-recovery/site-recovery-overview.md) bir felaket sırasında iş sürekliliği ve anında kurtarma sağlar.

## <a name="migrate"></a>Geçiş

Geçiş, şirket içinde çalışan mevcut iş yüklerini Azure bulut ortamına geçirmeyi ifade eder.
[Azure Geçiş,](../migrate/migrate-overview.md) şirket içi sanal makinelerin Azure'a geçiş uygunluğunu değerlendirmenize yardımcı olan bir hizmettir. Azure Site Kurtarma, sanal makineleri şirket [içinden veya Amazon Web Hizmetleri'nden](../site-recovery/migrate-tutorial-aws-azure.md)aktarır. [from on-premises](../site-recovery/migrate-tutorial-on-premises-azure.md) [Azure Veritabanı Geçişi,](../dms/dms-overview.md) veritabanı kaynaklarını Azure Veri platformlarına geçirmenize yardımcı olur.
