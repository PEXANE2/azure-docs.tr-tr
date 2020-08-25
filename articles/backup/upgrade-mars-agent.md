---
title: Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını yükseltme
description: Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını nasıl yükselteceğinizi öğrenin.
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: 782975583547bb5b51c3816d815ae1129c979509
ms.sourcegitcommit: afa1411c3fb2084cccc4262860aab4f0b5c994ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2020
ms.locfileid: "88757345"
---
# <a name="upgrade-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını yükseltme

Bu makalede şunları yapmayı öğreneceksiniz:

* MARS aracısının önceki sürümleriyle sunucuları tanımla
* Bu sunuculardaki MARS yüklemelerini güncelleştirme

## <a name="identify-servers-with-earlier-versions-of-the-mars-agent"></a>MARS aracısının önceki sürümleriyle sunucuları tanımla

Azure Backup Aracısı ve Azure Backup sunucusu yüklemeleri için:

1. Aracının daha eski sürümleri tarafından yedeklenmekte olan sunucuları kayıtlı olan kurtarma hizmetleri kasasına gidin. Azure 'daki Azure Backup güncelleştirme uyarılarında daha eski Azure Backup aracılarıyla temsili bir kasa listesi bulabilirsiniz.
1. Kurtarma Hizmetleri kasasının sol taraftaki **Ayarlar** bölümünde, **Yönet** bölümünde **Yedekleme altyapısı** ' nı seçin.
1. Azure Backup sunucu yüklemelerinin bir parçası olarak yüklenen Azure Backup aracılarını öğrenmek için **Yönetim sunucuları**altındaki **yedekleme yönetim sunucuları** ' na gidin. Bu, ilişkili Azure Backup aracısının sürüm numarasıyla birlikte Azure Backup Server yüklemelerine sahip sunucuları listeler.

    ![Azure Backup Server yüklemelerinin bir parçası olarak yüklenen MARS aracılarının listesi](./media/upgrade-mars-agent/backup-management-servers.png)

1. Microsoft Azure Kurtarma Hizmetleri (MARS) aracı yüklemelerinin veya Azure Backup aracısının aracı sürümlerini denetlemek için **Yönetim sunucuları**altındaki **korumalı sunucular** ' a gidin. Ardından yedekleme yönetimi türü altında **Azure Backup Aracısı** ' nı seçin. Bu, yükleme için sürüm numarasıyla birlikte Azure Backup aracı yüklemelerine sahip sunucuları listeler.

    ![MARS aracısının yüklü olduğu sunucu listesi](./media/upgrade-mars-agent/protected-servers.png)

1. Azure Backup Aracısı sürümü sütununu, MARS Aracısı yüklemelerinin **Aracı sürümü** sütununa veya Azure Backup sunucusu yüklemeleri Için **Azure Backup Aracısı sürümü** sütununa tıklayarak sıralayın.

1. Önceki adımda, 2.0.9083.0 ' den daha düşük sürümler veya boşluk olarak listelenen aracı sürümlerinden Azure Backup aracılarıyla birlikte sunucu listesi sunulacaktır. Bunlar, Azure Backup aracılarının güncellenmesi gereken sunuculardır.

## <a name="update-the-mars-agent-installation-on-the-server"></a>Sunucuda MARS Aracısı yüklemesini güncelleştirme

Azure Backup aracısının güncelleştirilmesi gereken sunucuları tanımladıktan sonra, tanımlanan her sunucu (Azure Backup sunucusu veya MARS Aracısı kullanarak) için aşağıdaki adımları gerçekleştirin. Aşağıdaki adımları uygulamadan önce [Azure Backup aracısının en son sürümünü indirin](https://aka.ms/azurebackup_agent) .

1. Azure Backup Aracısına 2.0.9083.0 veya Blank 'tan daha düşük bir satıra tıklayın. Bu, sunucu ayrıntısı ekranını açar.

    ![Güncel olmayan aracı sürümlerine sahip korumalı sunucular](./media/upgrade-mars-agent/old-agent-version.png)

    ![Güncel olmayan aracı sürümlerine sahip Azure Backup sunucuları](./media/upgrade-mars-agent/backup-management-servers-old-versions.png)

1. Sunucuya bağlanmak için bir Uzak Masaüstü bağlantı dosyası almak veya sunucunuzdaki Uzak Masaüstü bağlantısı aracılığıyla sunucuya doğrudan bağlanmak için **Bağlan** ' a tıklayın.

    ![Uzak Masaüstü bağlantısı ile sunucuya bağlanma](./media/upgrade-mars-agent/connect-to-server.png)

    >[!NOTE]
    > Listelenen sunucu yoksa veya kullanımdan kaldırılmıştır, aşağıdaki kalan adımları yoksayabilirsiniz ve sonraki sunucuya atlayın.

1. Yönetici oturum açma ayrıntılarınızı girin ve oturum açın.

1. Sunucunuzun veya sunucunuzun proxy 'sinin internet erişimi sınırlı ise, sunucu/ara sunucu üzerindeki güvenlik duvarı ayarlarının, kullanmakta olduğunuz Azure bulutuna uygun URL 'ye izin verecek şekilde yapılandırıldığından emin olun:

    Azure bulutu | URL
    -- | ---
    Azure bulutu (genel) |   `https://login.windows.net`
    Azure Çin 21Vianet bulutu   | `https://login.chinacloudapi.cn`
    Azure ABD kamu bulutu |   `https://login.microsoftonline.us`
    Azure Almanya bulutu  |  `https://login.microsoftonline.de`

1. Azure Backup Aracısı güncelleştirme yükleyicisini sunucuya kopyalayın.

    ![Azure Backup Aracısı güncelleştirme yükleyicisini sunucuya Kopyala](./media/upgrade-mars-agent/copy-agent-installer.png)

1. Yükleyiciyi çalıştırın. Microsoft Azure Kurtarma Hizmetleri Aracısı Yükseltme Sihirbazı açılır.

    ![Microsoft Azure Kurtarma Hizmetleri Aracısı Yükseltme Sihirbazı](./media/upgrade-mars-agent/agent-upgrade-wizard.png)

1. **İleri**’ye tıklayın.

1. **Yükselt**’e tıklayın.

    ![Microsoft Azure Kurtarma Hizmetleri aracısının yüklenmesi](./media/upgrade-mars-agent/upgrade-installation.png)

1. Son onay ekranı, Azure Backup aracısının başarıyla güncelleştirildiğini gösterir.

## <a name="for-system-center-data-protection-manager-sc-dpm-customers"></a>System Center Data Protection Manager (SC DPM) müşterileri için

System Center Data Protection Manager (SC DPM) sunucularına Azure Backup aracıları yüklediyseniz, DPM sunucularınızın bir Azure Backup Aracısı güncelleştirmesi gerekip gerekmediğini belirlemek için aşağıdaki adımları izlemeniz gerekir:

1. SC DPM sunucunuzda yönetici olarak oturum açın.
2. DPM konsolunu açın.
3. Konsolun sol alt gezinti bölmesinde **Yönetim** ' e tıklayın.
4. Sol gezinti çubuğunda görüntülenen bilgiler içinde Azure Backup Aracısı sürüm bilgilerini arayın.
5. Sürüm 2.0.9083.0 'den düşükse, en son Azure Backup Aracısı yükleyicisini indirin ve Azure Backup aracısını güncelleştirmek için DPM sunucusunda yükleyiciyi çalıştırın.

Ortamınızdaki tüm DPM sunucuları için yukarıdaki adımları tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Backup Mars Aracısı 'nı kullanarak Windows makinelerini nasıl yedekleyeceğinizi](backup-windows-with-mars-agent.md) öğrenin
