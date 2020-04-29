---
title: Azure yönetimine genel bakış-Azure Idare
description: Azure Yönetim Araçları 'ndaki içeriklere yönelik bağlantılarla Azure uygulamaları ve kaynakları için yönetim bölümlerine genel bakış.
ms.date: 12/06/2018
ms.topic: overview
ms.openlocfilehash: 644f4fc3a04f86426c2eb04b02c29882fa39fb88
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75980910"
---
# <a name="overview-of-management-services-in-azure"></a>Azure 'da Yönetim Hizmetleri 'ne genel bakış

Azure 'da idare, Azure yönetiminin bir yönüdür. Bu makalede, Azure 'da kaynaklarınızı dağıtmaya ve korumaya yönelik farklı yönetim bölgeleri ele alınmaktadır.

Yönetim, iş uygulamalarınızı ve onları destekleyen kaynaklarınızı korumak için gereken görevleri ve süreçleri ifade eder. Azure 'da, tüm yönetimi sağlamak için birlikte çalışan birçok hizmet ve araç vardır. Bu hizmetler yalnızca Azure 'daki kaynaklar ve ayrıca diğer bulutlarda ve şirket içinde değildir. Farklı araçları ve bunların birlikte nasıl çalıştığını anlamak, tüm yönetim ortamlarını tasarlamanın ilk adımıdır.

Aşağıdaki diyagramda herhangi bir uygulamayı veya kaynağı korumak için gereken farklı yönetim alanları gösterilmektedir. Bu farklı bölgeler yaşam döngüsü olarak düşünülebilir. Her alan, bir kaynağın kullanım ömrü boyunca sürekli olarak her bir şekilde gereklidir. Bu kaynak yaşam döngüsü ilk dağıtım ile başlar, devam eden işlem ve son kullanımdan kalkmıştır.

![Azure 'da disiplindeki yönetim hatları](../monitoring/media/management-overview/management-capabilities.png)

Tek bir Azure hizmeti belirli bir yönetim alanının gereksinimlerini tamamen doldurmıyor. Bunun yerine, her biri birlikte çalışan çeşitli hizmetler tarafından gerçekleştirilir. Application Insights gibi bazı hizmetler, Web uygulamaları için hedeflenen izleme işlevlerini sağlar. Azure Izleyici günlükleri gibi diğerleri, diğer hizmetler için yönetim verilerini depolar. Bu özellik farklı hizmetler tarafından toplanan farklı türlerdeki verileri analiz etmenizi sağlar.

Aşağıdaki bölümlerde farklı yönetim alanları kısaca açıklanır ve ilgili temel Azure hizmetlerindeki ayrıntılı içeriklere yönelik bağlantılar sunulur.

## <a name="monitor"></a>İzleme

İzleme, kaynaklarınızın performansını, sistem durumunu ve kullanılabilirliğini denetlemek için veri toplama ve çözümleme işlemidir. Etkili bir izleme stratejisi, bileşenlerin çalışmasını anlamanıza ve kullanım süresini bildirimlerle artırmaya yardımcı olur. [Azure uygulamalarını ve kaynaklarını izlerken](../monitoring/monitoring-overview.md)kullanılan farklı Hizmetleri kapsamakta olan izlemeye genel bakış konusunu okuyun.

## <a name="configure"></a>Yapılandırma

Yapılandırma, kaynakların ilk dağıtımını ve yapılandırmasını ve devam eden bakım sayısını ifade eder.
Bu görevlerin otomasyonu, artıklığı ortadan kaldırmanıza, zaman ve çabalarınızı en aza indirmenizi ve doğruluğu ve verimliliğin artırılmasını sağlar. [Azure Otomasyonu](../automation/automation-intro.md), yapılandırma görevlerini otomatikleştirmek için toplu hizmetler sunar. Runbook 'lar süreç otomasyonu, yapılandırma ve güncelleştirme yönetimi ile yapılandırmayı yönetme konusunda yardımcı olur.

## <a name="govern"></a>İdare

İdare, Azure’daki uygulama ve kaynaklarınız üzerindeki denetimi sürdürmenize yönelik mekanizmalar ve süreçler sağlar. Girişimlerinizi planlama ve stratejik öncelikleri belirleme de bu kapsama dahildir.
Azure’da İdare, temelde iki hizmet ile uygulanır. [Azure ilkesi](./policy/overview.md) , kaynaklarınız için kuralları zorlamak üzere ilke tanımları oluşturmanıza, atamanıza ve yönetmenize olanak sağlar. Bu özellik, bu kaynakları kurumsal standartlarınızla uyumlu tutar. [Azure maliyet yönetimi](../cost-management-billing/cost-management-billing-overview.md) , Azure kaynaklarınız ve diğer bulut sağlayıcıları için bulut kullanımını ve harcamalarınızı izlemenize olanak sağlar.

## <a name="secure"></a>Güvenlik

Kaynaklarınızın ve verilerinizin güvenliğini yönetin. Bir güvenlik programı tehditleri değerlendirmek, verileri toplamak ve analiz etmek ve uygulamalarınızın ve kaynaklarınızın uyumluluğunu içerir. Güvenlik izleme ve tehdit analizi, [Azure Güvenlik Merkezi](../security-center/security-center-intro.md)tarafından sağlanır ve karma bulut iş yükleri arasında Birleşik güvenlik yönetimi ve Gelişmiş tehdit koruması içerir. Azure kaynaklarını güvenli hale getirme hakkında kapsamlı bilgi ve yönergeler için bkz. [Azure güvenliğine giriş](../security/fundamentals/overview.md) .

## <a name="protect"></a>Koruma

Koruma, sizin denetiminizin ötesinde olan kesintilerle birlikte uygulamalarınızın ve verilerinizin kullanılabilir tutulması anlamına gelir. Azure’da Koruma, iki hizmetle sağlanır. [Azure Backup](../backup/backup-introduction-to-azure-backup.md), hem buluttaki hem de şirket içindeki verilerinize yönelik yedekleme ve kurtarma sağlar. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) olağanüstü durum sırasında iş sürekliliği ve anında kurtarma sağlar.

## <a name="migrate"></a>Geçiş

Geçiş, şirket içinde çalışan mevcut iş yüklerini Azure bulut ortamına geçirmeyi ifade eder.
[Azure geçişi](../migrate/migrate-overview.md) , şirket içi sanal makinelerin Azure 'a geçiş uygunluğunu değerlendirmenize yardımcı olan bir hizmettir. Azure Site Recovery sanal makineleri [Şirket içinden](../site-recovery/migrate-tutorial-on-premises-azure.md) veya [Amazon Web Services](../site-recovery/migrate-tutorial-aws-azure.md)geçirir. [Azure veritabanı geçişi](../dms/dms-overview.md) , veritabanı kaynaklarını Azure veri platformları 'na geçirmeye yardımcı olur.
