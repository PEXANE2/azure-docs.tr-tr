---
title: Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını yükseltme
description: Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını nasıl yükselteceklerini öğrenin.
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: 49daf438b855d19961519d93b6c3ec535de4756f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672857"
---
# <a name="upgrade-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını yükseltme

Bu makalede nasıl öğreneceksiniz:

* MARS aracısının önceki sürümlerine sahip sunucuları tanımlama
* Bu sunucularda MARS yüklemelerini güncelleştirme

## <a name="identify-servers-with-earlier-versions-of-the-mars-agent"></a>MARS aracısının önceki sürümlerine sahip sunucuları tanımlama

Azure Yedekleme aracısı ve Azure yedekleme sunucusuyüklemeleri için:

1. Aracının eski sürümleri tarafından yedeklenmiş olabilecek kayıtlı sunucularınız olan Kurtarma Hizmetleri Kasası'na gidin. Azure Yedekleme Güncelleştirmesi uyarılarında eski Azure Yedekleme aracılarına sahip kasaların temsili bir listesini bulabilirsiniz.
1. Kurtarma Hizmetleri Kasası'nın sol taraftaki **Ayarlar** **bölümünde, Yönet** bölümü altında **Yedek Altyapı'yı** seçin.
1. Azure Yedekleme sunucu yüklemelerinin bir parçası olarak yüklenen Azure yedekleme aracılarını keşfetmek için **Yönetim Sunucuları**altında **Yedekleme Yönetim Sunucuları'na** gidin. Bu, ilgili Azure Yedekleme aracısının sürüm numarasıyla birlikte Azure Yedekleme sunucu yüklemelerine sahip sunucuları listeler.

    ![Azure Yedekleme sunucu yüklemelerinin bir parçası olarak yüklenen MARS aracılarının listesi](./media/upgrade-mars-agent/backup-management-servers.png)

1. Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı yüklemeleri veya Azure Yedekleme aracısı için aracı sürümlerini denetlemek **için, Yönetim Sunucuları**altında **Korumalı Sunucular'a** gidin. Ardından Yedekleme Yönetimi Türü altında **Azure Yedekleme aracısı** seçin. Bu, Azure Yedekleme aracıyüklemelerine sahip sunucuları ve yüklemenin sürüm numarasını listeler.

    ![MARS aracısının yüklü sunucuları listesi](./media/upgrade-mars-agent/protected-servers.png)

1. MARS Aracısı yüklemeleri için **Aracı** Sürümü sütununa veya Azure yedekleme sunucusu yüklemeleri için **Azure Yedekleme Aracısı Sürümü** sütununa tıklayarak Azure Yedekleme Aracısı sürüm sütununu sıralayın.

1. Önceki adım, 2.0.9083.0'dan daha düşük sürümleri olan Azure Yedekleme aracılarına sahip sunucuların veya boş olarak listelenen aracı sürümlerinin listesini verir. Bunlar, Azure Yedekleme aracılarının güncellenmesi gereken sunuculardır.

## <a name="update-the-mars-agent-installation-on-the-server"></a>Sunucudaki MARS aracı yüklemesini güncelleştirme

Azure Yedekleme aracısı güncelleştirmesi gerektiren sunucuları tanımladıktan sonra, tanımlanan her sunucu için (Azure yedekleme sunucusu veya MARS Aracısı kullanarak) aşağıdaki adımları gerçekleştirin. Aşağıdaki adımları takip etmeden önce [Azure yedekleme aracısının en son sürümünü indirin.](https://aka.ms/azurebackup_agent)

1. Azure Yedekleme aracısı 2.0.9083.0'dan düşük veya boş olan bir satırı tıklatın. Bu sunucu ayrıntı ekranı açılır.

    ![Güncel olmayan aracı sürümlerine sahip korumalı sunucular](./media/upgrade-mars-agent/old-agent-version.png)

    ![Güncel olmayan aracı sürümlerine sahip Azure yedekleme sunucuları](./media/upgrade-mars-agent/backup-management-servers-old-versions.png)

1. Sunucuya bağlanmak veya sunucunuzdaki Uzak Masaüstü bağlantısı üzerinden doğrudan sunucuya bağlanmak için uzak bir masaüstü bağlantı dosyası almak için **Bağlan'ı** tıklatın.

    ![Uzak masaüstü bağlantısı üzerinden sunucuya bağlanma](./media/upgrade-mars-agent/connect-to-server.png)

    >[!NOTE]
    > Listelenen sunucu yoksa veya kullanımdan kaldırılmışsa, aşağıdaki kalan adımları yoksayabilir ve bir sonraki sunucuya atlayabilirsiniz.

1. İdari giriş bilgilerinizi girin ve oturum açın.

1. Sunucunuzun veya sunucunuzun proxy'sinin internet erişimi sınırlıysa, sunucu/proxy'deki güvenlik duvarı ayarlarının, kullanmakta olduğunuz Azure bulutuna uygun URL'ye izin verecek şekilde yapılandırıldığından emin olun:

    Azure Bulutu | URL'si
    -- | ---
    Azure Bulutu (Genel) |   `https://login.windows.net`
    Azure China 21Vianet Bulut   | `https://login.chinacloudapi.cn`
    Azure ABD Devlet Bulutu |   `https://login.microsoftonline.us`
    Azure Alman Bulutu  |  `https://login.microsoftonline.de`

1. Azure yedekleme aracısı güncelleyicisini sunucuya kopyalayın.

    ![Azure yedekleme aracısı güncelleştirme yükleyicisini sunucuya kopyalama](./media/upgrade-mars-agent/copy-agent-installer.png)

1. Yükleyiciyi çalıştırın. Microsoft Azure Kurtarma Hizmetleri Aracısı Yükseltme sihirbazı açılır.

    ![Microsoft Azure Kurtarma Hizmetleri Aracıyükseltme Sihirbazı](./media/upgrade-mars-agent/agent-upgrade-wizard.png)

1. **İleri**'ye tıklayın.

1. **Yükselt**’e tıklayın.

    ![Microsoft Azure Kurtarma Hizmetleri Aracısı'nın yüklenmesi](./media/upgrade-mars-agent/upgrade-installation.png)

1. Son onay ekranı, Azure Yedekleme aracısının başarıyla güncellendiğini gösterir.

## <a name="for-system-center-data-protection-manager-sc-dpm-customers"></a>Sistem Merkezi Veri Koruma Yöneticisi (SC DPM) müşterileri için

Sistem Merkezi Veri Koruma Yöneticisi (SC DPM) sunucularına Azure yedekleme aracıları yüklediyseniz, DPM sunucularınızın bir Azure yedekleme aracısı güncelleştirmesine gerek olup olmadığını belirlemek için aşağıdaki adımları izlemeniz gerekir:

1. Yönetici olarak SC DPM sunucunuzda oturum açın.
2. DPM Konsolu'nu açın.
3. Konsolun sol alt gezintisinde **Yönetim'e** tıklayın.
4. Sol gezintide görünen bilgiler içinde Azure Yedekleme aracısı sürüm bilgilerini arayın.
5. Sürüm 2.0.9083.0'dan düşükse, en son Azure yedekleme aracısını indirin ve Azure Yedekleme aracısını güncelleştirmek için DPM sunucusunda yükleyiciyi çalıştırın.

Ortamınızdaki tüm DPM sunucuları için yukarıdaki adımları yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Yedekleme MARS aracısını kullanarak Windows makinelerini nasıl yedekleyin](backup-windows-with-mars-agent.md)
