---
title: Azure Site Kurtarma ile Azure'da VMware olağanüstü durum kurtarma için kaynak ayarları ayarlama
description: Bu makalede, VMware VM'leri Azure'da Azure Site Kurtarma ile çoğaltmak için şirket içi ortamınızı nasıl ayarlayacağınız açıklanmaktadır.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/14/2019
ms.author: ramamill
ms.openlocfilehash: ff01aed92669acb193ff149ea9298550134f42a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257062"
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>VMware için kaynak ortamını Azure çoğaltmasına ayarlama

Bu makalede, VMware VM'leri Azure'a çoğaltmak için kaynak şirket içi ortamınızı nasıl ayarlayacağınız açıklanmaktadır. Makale, çoğaltma senaryonuzu seçmek, Site Kurtarma yapılandırma sunucusu olarak şirket içi bir makine ayarlamak ve şirket içi VM'leri otomatik olarak keşfetmek için adımlar içerir.

## <a name="prerequisites"></a>Ön koşullar

Makale, zaten var varsayar:

- Azure [Site Kurtarma Dağıtım Planlayıcısı](site-recovery-deployment-planner.md)yardımıyla dağıtımınızı planla. Bu, istediğiniz kurtarma noktası hedefini (RPO) karşılamak için günlük veri değiştirme oranınıza göre yeterli bant genişliği ayırmanıza yardımcı olur.
- [Azure portalında](https://portal.azure.com) [kaynak ayarlama.](tutorial-prepare-azure.md)
- Otomatik keşif için özel bir hesap da dahil olmak üzere [şirket içi VMware'i ayarlayın.](vmware-azure-tutorial-prepare-on-premises.md)

## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçin

1. **Kurtarma Hizmetleri kasaları** bölümünde kasa adını seçin. Bu senaryo için**ContosoVMVault**’u kullanıyoruz.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Daha sonra **Altyapıyı Hazırlama**’yı seçin.
3. **Koruma hedefiNde** > **Makineleriniz nerede bulunur**, şirket **içi**seçin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.

## <a name="set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarlama

Açık Sanallaştırma Uygulaması (OVA) şablonu aracılığıyla yapılandırma sunucusunu şirket içi VMware VM olarak ayarlayabilirsiniz. VMware VM'de yüklenecek bileşenler hakkında [daha fazla bilgi edinin.](concepts-vmware-to-azure-architecture.md)

1. Yapılandırma sunucusu dağıtımı için [ön koşullar](vmware-azure-deploy-configuration-server.md#prerequisites) hakkında bilgi edinin.
2. Dağıtım için [kapasite numaralarını denetleyin.](vmware-azure-deploy-configuration-server.md#sizing-and-capacity-requirements)
3. Yapılandırma sunucusunu çalıştıran şirket içi Bir VMware VM'yi ayarlamak için OVA şablonuna [indirin](vmware-azure-deploy-configuration-server.md#download-the-template) ve [aktarın.](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) Şablonla sağlanan lisans bir değerlendirme lisansıdır ve 180 gün boyunca geçerlidir. Bu süre sonrası, müşteri bir satın lisans ile pencereleri etkinleştirmek gerekir.
4. VMware VM'yi açın ve Kurtarma Hizmetleri kasasına [kaydedin.](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services)

## <a name="azure-site-recovery-folder-exclusions-from-antivirus-program"></a>Azure Site Kurtarma klasörü Antivirus programından hariç tutma

### <a name="if-antivirus-software-is-active-on-source-machine"></a>Virüsten Koruma yazılımı Kaynak makinede etkinse

Kaynak makinede virüsten koruma yazılımı etkinse, yükleme klasörü hariç tutulmalıdır. Bu nedenle, düzgün çoğaltma için *C:\ProgramData\ASR\agent* klasörünü hariç tut.

### <a name="if-antivirus-software-is-active-on-configuration-server"></a>Antivirus Software Configuration sunucusunda etkinse

Düzgün çoğaltma ve bağlantı sorunlarını önlemek için Antivirus yazılımından aşağıdaki klasörleri hariç tutma

- C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı.
- C:\Program Files\Microsoft Azure Site Kurtarma Sağlayıcısı
- C:\Program Files\Microsoft Azure Site Kurtarma Yapılandırma Yöneticisi 
- C:\Program Files\Microsoft Azure Sitesi Kurtarma Hata Toplama Aracı 
  - C:\üçüncü taraf
  - C:\temp
  - C:\çilek
  - C:\ProgramData\MySQL
  - C:\Program Dosyaları (x86)\MySQL
  - C:\ProgramData\ASR
  - C:\ProgramData\Microsoft Azure Site Kurtarma
  - C:\ProgramData\ASRLogs
  - C:\ProgramData\ASRSetupLogs
  - C:\ProgramData\LogUploadServiceLogs
  - C:\inetpub
  - Site Kurtarma sunucu yükleme dizini. Örneğin: E:\Program Dosyaları (x86)\Microsoft Azure Site Kurtarma

### <a name="if-antivirus-software-is-active-on-scale-out-process-servermaster-target"></a>Virüsten Koruma Yazılımı ölçeklendirme İşlemi sunucusunda/Ana Hedef'te etkinse

Antivirus yazılımından aşağıdaki klasörleri hariç tutma

1. C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı
2. C:\ProgramData\ASR
3. C:\ProgramData\ASRLogs
4. C:\ProgramData\ASRSetupLogs
5. C:\ProgramData\LogUploadServiceLogs
6. C:\ProgramData\Microsoft Azure Site Kurtarma
7. Azure Site Kurtarma yük dengeli işlem sunucusu yükleme dizini, Örnek: C:\Program Files (x86)\Microsoft Azure Site Kurtarma


## <a name="next-steps"></a>Sonraki adımlar
[Hedef ortamınızı ayarlama](./vmware-azure-set-up-target.md) 
