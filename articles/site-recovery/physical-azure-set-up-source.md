---
title: Azure Site Kurtarma 'yı kullanarak fiziksel sunucuların Azure'a olağanüstü durum kurtarması için yapılandırma sunucusunu ayarlama | Microsoft Dokümanlar'
description: Bu makalede, şirket içi fiziksel sunucuların Azure'da olağanüstü durum kurtarması için şirket içi yapılandırma sunucusunun nasıl ayarlanır.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: ramamill
ms.openlocfilehash: 902c14211e91a1500c6b50cd790b347e337c4f70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257881"
---
# <a name="set-up-the-configuration-server-for-disaster-recovery-of-physical-servers-to-azure"></a>Azure'da fiziksel sunucuların olağanüstü kurtarma durumu için yapılandırma sunucusunu ayarlama

Bu makalede, Windows veya Linux çalıştıran fiziksel sunucuları Azure'da çoğaltmaya başlamak için şirket içi ortamınızı nasıl ayarlayacağı açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Makale zaten var varsayar:
- [Azure portalında](https://portal.azure.com "Azure portalında")bir Kurtarma Hizmetleri kasası.
- Yapılandırma sunucusunun yüklendiği fiziksel bir bilgisayar.
- Yapılandırma sunucusunu yüklediğiniz makinede TLS 1.0'ı devre dışı bıraktıysanız, TLs 1.2'nin etkin olduğundan ve .NET Framework sürümü 4.6 veya sonraki lerinin makineye (güçlü şifreleme etkinleştirilmiş) yüklendiğinden emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/help/4033999/how-to-resolve-azure-site-recovery-agent-issues-after-disabling-tls-1).

### <a name="configuration-server-minimum-requirements"></a>Yapılandırma sunucusu minimum gereksinimleri
Aşağıdaki tabloda, yapılandırma sunucusu için minimum donanım, yazılım ve ağ gereksinimleri listelenilmektedir.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> HTTPS tabanlı proxy sunucuları yapılandırma sunucusu tarafından desteklenmez.

## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçin

1. Azure portalında, Kurtarma **Hizmetleri** kasaları bıçağına gidin ve kasanızı seçin.
2. Kasanın **Kaynak** menüsünde, **Başlangıç** > **Sitesi Kurtarma** > Adımı 1: Altyapı > Koruma hedefini**hazırla'yı**tıklatın.**Protection goal**

    ![Hedefleri seçme](./media/physical-azure-set-up-source/choose-goals.png)
3. **Koruma hedefinde,** Azure'a ve **Sanallaştırılan/Diğer'e'yi**seçin ve ardından **Tamam'ı**tıklatın. **To Azure**

    ![Hedefleri seçme](./media/physical-azure-set-up-source/physical-protection-goal.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

1. **Kaynak Hazırla'da,** yapılandırma sunucunuz yoksa, eklemek için **+Configuration sunucusuna** tıklayın.

   ![Kaynağı ayarlama](./media/physical-azure-set-up-source/plus-config-srv.png)
2. Sunucu **Ekle** durumunda, **Configuration Server'ın** **Sunucu türünde**görünip görünmediğini denetleyin.
4. Site Kurtarma Birleşik Kurulum yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indirin. Birleşik Kurulum'u çalıştırDığınızda kayıt anahtarına ihtiyacınız vardır. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/physical-azure-set-up-source/set-source2.png)
6. Yapılandırma sunucusu olarak kullandığınız makinede, yapılandırma sunucusunu, işlem sunucusunu ve ana hedef sunucuyu yüklemek için **Azure Site Kurtarma Tümleşek Kurulumu'nu** çalıştırın.

#### <a name="run-azure-site-recovery-unified-setup"></a>Azure Site Kurtarma Birleşik Kurulum'u çalıştırın

> [!TIP]
> Bilgisayarınızın sistem saatindeki süre yerel saate beş dakikadan fazla uzaklıktaysa yapılandırma sunucusu kaydı başarısız olur. Yüklemeyi başlatmadan önce sistem saatinizi bir [zaman sunucusuyla](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) senkronize edin.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Yapılandırma sunucusu bir komut satırı üzerinden yüklenebilir. [Daha fazla bilgi edinin](physical-manage-configuration-server.md#install-from-the-command-line).


## <a name="common-issues"></a>Genel sorunlar

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım, [hedef ortamınızı](physical-azure-set-up-target.md) Azure'da ayarlamakla ilgilidir.
