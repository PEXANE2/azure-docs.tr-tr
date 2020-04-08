---
title: Microsoft Defender Gelişmiş Tehdit Koruması - Azure Güvenlik Merkezi
description: Bu belge, Azure Güvenlik Merkezi ile Microsoft Defender Gelişmiş Tehdit Koruması arasındaki tümleştirmeyi tanıtır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2020
ms.author: memildin
ms.openlocfilehash: a502541a5a01ec3304338054239fc3b50f4d3c61
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80810297"
---
# <a name="microsoft-defender-advanced-threat-protection-with-azure-security-center"></a>Azure Güvenlik Merkezi ile Microsoft Defender Gelişmiş Tehdit Koruması

Azure Güvenlik Merkezi, [Microsoft Defender Gelişmiş Tehdit Koruması](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp) (ATP) ile tümleştirerek Bulut İş Yükü Koruma Platformları teklifini genişletiyor.
Bu değişiklik, kapsamlı Uç Nokta Koruma ve Yanıt (EDR) özelliklerini de beraberinde getirmektedir. Microsoft Defender ATP tümleştirmesi ile anormallikleri fark edebilirsiniz. Azure Güvenlik Merkezi tarafından izlenen sunucu uç noktalarına yapılan gelişmiş saldırıları algılayabilir ve yanıtlayabilirsiniz.

## <a name="microsoft-defender-atp-features-in-security-center"></a>Güvenlik Merkezi'ndeki Microsoft Defender ATP özellikleri

Microsoft Defender ATP'yi kullandığınızda şunları elde eeesiniz:

- **Gelişmiş ihlal sonrası algılama sensörleri**: Windows sunucuları için Microsoft Defender ATP sensörleri çok çeşitli davranış sinyalleri toplar.

- **Analitik tabanlı, bulut destekli posta ihlali algılama**: Microsoft Defender ATP değişen tehditlere hızla uyum sağlar. Gelişmiş analitik ve büyük veri kullanır. Microsoft Defender ATP, Bilinmeyen tehditleri algılamak için Windows, Azure ve Office'teki sinyallerle Akıllı Güvenlik Grafiği'nin gücüyle güçlenir. Eyleme geçirilebilir uyarılar sağlar ve hızlı bir şekilde yanıt vermenizi sağlar.

- **Tehdit istihbaratı**: Microsoft Defender ATP, saldırgan araçlarını, tekniklerini ve yordamlarını tanımladığında uyarılar üretir. Microsoft tehdit avcıları ve güvenlik ekipleri tarafından oluşturulan ve ortaklar tarafından sağlanan istihbaratla artırılan verileri kullanır.

Aşağıdaki özellikler artık Azure Güvenlik Merkezi'nde kullanılabilir:

- **Otomatik onboarding**: Microsoft Defender ATP sensörü, Azure Güvenlik Merkezi'ne dahil edilen Windows sunucuları için otomatik olarak etkinleştirilir (Windows Server 2019 çalıştıranlar hariç).

- **Tek cam bölme**: Azure Güvenlik Merkezi konsolu Microsoft Defender ATP uyarılarını görüntüler.

- **Ayrıntılı makine araştırması**: Azure Güvenlik Merkezi müşterileri, ihlalin kapsamını ortaya çıkarmak için ayrıntılı bir araştırma yapmak için Microsoft Defender ATP konsolunu kullanabilir.

![Azure Güvenlik Merkezi, her uyarı yla ilgili uyarıların ve genel bilgilerin listesini görüntüler](media/security-center-wdatp/image1.png)

Daha fazla araştırma yapmak için Microsoft Defender ATP'yi kullanın. Microsoft Defender ATP, uyarı işlem ağacı ve olay grafiği gibi ek bilgiler sağlar. Ayrıca, altı aya kadar olan bir tarihsel dönem için her davranışı gösteren ayrıntılı bir makine zaman çizelgesi de görebilirsiniz.

![Bir uyarı hakkında ayrıntılı bilgi içeren Microsoft Defender ATP sayfası](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Platform desteği

Güvenlik Merkezi'ndeki Microsoft Defender ATP, Windows Server 2016, 2012 R2 ve 2008 R2 SP1'de algılamayı destekler, Azure VM'ler için Standart katman aboneliğine ihtiyacınız vardır ve Azure olmayan VM'ler için yalnızca çalışma alanı düzeyinde Standart katmana ihtiyacınız vardır.

> [!NOTE]
> Sunucuları izlemek için Azure Güvenlik Merkezi'ni kullandığınızda, bir Microsoft Defender ATP kiracısı otomatik olarak oluşturulur ve Microsoft Defender ATP verileri varsayılan olarak Avrupa'da depolanır. Verilerinizi başka bir konuma taşımanız gerekiyorsa, kiracıyı sıfırlamak için Microsoft Destek'e başvurmanız gerekir. Bu tümleştirmeyi kullanan sunucu uç nokta izleme, Office 365 GCC müşterileri için devre dışı bırakıldı.

## <a name="onboarding-servers-to-security-center"></a>Sunucuları Güvenlik Merkezi'ne ekleme 

Güvenlik Merkezi'ndeki sunucular için, Microsoft Defender ATP sunucusundaki yerleşik sunuculara gitmek **için Azure Güvenlik Merkezi'ne gidin'i** tıklatın.

1. **Onboarding** alanında, verileri depolamak için bir çalışma alanı seçin veya oluşturun. <br>
2. Tüm çalışma alanlarınızı göremiyorsanız, bunun nedeni izin eksikliği olabilir, çalışma alanınızın Azure Güvenlik Standardı katmanıolarak ayarlandıklarından emin olun. Daha fazla bilgi için, [gelişmiş güvenlik için Güvenlik Merkezi'nin Standart katmanına Yükseltme](security-center-pricing.md)konusuna bakın.
    
3. Log Analytics aracısını nasıl yükleyin yönergeleri görüntülemek için **sunucu ekle'yi** seçin. 

4. Onboarding sonra, **İşlem ve uygulamalar**altında makineleri izleyebilirsiniz.

   ![Yerleşik bilgisayarlar](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-microsoft-defender-atp-integration"></a>Microsoft Defender ATP tümleştirmesini etkinleştirme

Microsoft Defender ATP tümleştirmesi etkinleştirilip etkinleştirilmesini görüntülemek için, aboneliğinizi tıklatmak > **Güvenlik merkezi** > **Fiyatlandırma & ayarlarını** seçin.
Burada şu anda etkin leştirilmiş tümleştirmeleri görebilirsiniz.

  ![Microsoft Defender ATP tümleştirmesi etkinleştirilmiş Azure Güvenlik Merkezi tehdit algılama ayarları sayfası](media/security-center-wdatp/enable-integrations.png)

- Sunucuları Azure Güvenlik Merkezi standart katmanına zaten bindirdiyseniz, başka bir işlem yapmanız gerekmez. Azure Güvenlik Merkezi, sunucularda otomatik olarak Microsoft Defender ATP'ye sunulur. Onboarding 24 saat kadar sürebilir.

- Sunucuları Azure Güvenlik Merkezi standart katmanına hiç binmediyseniz, her zamanki gibi Azure Güvenlik Merkezi'ne binin.

- Sunucuları Microsoft Defender ATP aracılığıyla gemiye battıysanız:
  - [Sunucu makinelerinin nasıl offboard'a](https://go.microsoft.com/fwlink/p/?linkid=852906)yapılacağını gösteren kılavuzlar için belgelere bakın.
  - Bu sunucularda Azure Güvenlik Merkezi'ne inin.

## <a name="access-to-the-microsoft-defender-atp-portal"></a>Microsoft Defender ATP portalına erişim

[Portala kullanıcı erişimini atay](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access)yönergeleri izleyin.

## <a name="set-the-firewall-configuration"></a>Güvenlik duvarı yapılandırmasını ayarlama

Bir Microsoft Defender ATP sensörü sistem bağlamından bağlandığından, anonim trafiği engelleyen bir proxy'niz veya güvenlik duvarınız varsa, anonim trafiğe izin verildiğinden emin olun. [Proxy sunucusundaki Microsoft Defender ATP hizmet URL'lerine erişimi etkinleştirme'deki](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server)yönergeleri izleyin.

## <a name="test-the-feature"></a>Özelliği test edin

İyi huylu bir Microsoft Defender ATP test uyarısı oluşturmak için:

1. 'C:\test-MDATP-test' klasörü oluşturun.

1. Windows Server 2012 R2 VM veya Windows Server 2016 VM'e erişmek için Uzak Masaüstü'nü kullanın. Komut satırı pencereyi açın.

1. İstemde, aşağıdaki komutu kopyalayın ve çalıştırın. Komut İstemi penceresi otomatik olarak kapanır.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```

   ![Yukarıdaki komutu içeren Komut İstemi penceresi](media/security-center-wdatp/image4.jpeg)

1. Komut başarılı olursa, Azure Güvenlik Merkezi panosunda ve Microsoft Defender ATP portalında yeni bir uyarı görürsünüz. Bu uyarının görünmesi birkaç dakika sürebilir.

1. Güvenlik Merkezi'ndeki uyarıyı incelemek **için, Güvenlik uyarılarışüpheli** > **PowerShell CommandLine'a**gidin.

1. Araştırma penceresinden Microsoft Defender ATP portalına gitmek için bağlantıyı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi tarafından desteklenen platformlar ve özellikler](security-center-os-coverage.md)
- [Azure Güvenlik Merkezi'nde güvenlik ilkeleri belirleme](tutorial-security-policy.md): Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini nasıl yapılandırıştıracaklarınızı öğrenin.
- [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md): Önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.