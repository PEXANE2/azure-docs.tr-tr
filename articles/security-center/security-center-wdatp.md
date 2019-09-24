---
title: Azure Güvenlik Merkezi ile Windows Defender Gelişmiş tehdit koruması
description: Bu belge, Azure Güvenlik Merkezi ile Windows Defender Gelişmiş tehdit koruması arasındaki tümleştirmeyi tanıtır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2019
ms.author: memildin
ms.openlocfilehash: b1af4f99b1446599f53448884d3147601fba56a8
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200651"
---
# <a name="windows-defender-advanced-threat-protection-with-azure-security-center"></a>Azure Güvenlik Merkezi ile Windows Defender Gelişmiş tehdit koruması

Azure Güvenlik Merkezi, [Windows Defender Gelişmiş Tehdit Koruması](https://www.microsoft.com/en-us/WindowsForBusiness/windows-atp) (ATP) tümleştirmesiyle Bulut İş Yükü Koruma Platformlarını genişletmektedir.
Bu değişiklik, kapsamlı Uç Nokta Koruma ve Yanıt (EDR) özelliklerini de beraberinde getirmektedir. Windows Defender ATP tümleştirmesi ile, normalleştirimler oluşturabilirsiniz. Ayrıca, Azure Güvenlik Merkezi tarafından izlenen sunucu uç noktalarında gelişmiş saldırıları algılayabilir ve bunlara yanıt verebilirsiniz.

## <a name="windows-defender-atp-features-in-security-center"></a>Güvenlik Merkezi 'nde Windows Defender ATP özellikleri

Windows Defender ATP kullandığınızda şunları alırsınız:

- **Yeni nesil sonrası ihlali algılama algılayıcıları**: Windows Server için Windows Defender ATP sensörleri çok sayıda davranış sinyali toplar.

- **Analitik tabanlı, bulut destekli gönderi ihlali algılama**: Windows Defender ATP, değişen tehditlere hızlı bir şekilde uyum sağlar. Gelişmiş analiz ve büyük veri kullanır. Windows Defender ATP, bilinmeyen tehditleri algılamak için Windows, Azure ve Office 'teki sinyallerle Intelligent Security Graph güçlerinin korunmasını sağlar. İşlem yapılabilir uyarılar sağlar ve hızla yanıt vermenize olanak tanır.

- **Tehdit bilgileri**: Windows Defender ATP, saldırgan araçlarını, tekniklerini ve yordamlarını tanımlar. Bunları algıladığında uyarılar oluşturur. Microsoft tehdit arayıcılar ve güvenlik ekipleri tarafından oluşturulan ve iş ortakları tarafından sunulan zeka tarafından düzenlenen verileri kullanır.

Bu özellikler Azure Güvenlik Merkezi'nde kullanıma sunulmuştur:

- **Otomatik ekleme**: Windows Defender ATP algılayıcısı, Azure Güvenlik Merkezi 'Nde eklendi Windows Server 'lar için otomatik olarak etkinleştirilir.

- **Tek bir cam bölmesi**: Azure Güvenlik Merkezi konsolunda Windows Defender ATP uyarıları görüntülenir.

- **Ayrıntılı makine araştırması**: Azure Güvenlik Merkezi müşterileri, bir ihlalin kapsamını açmaya yönelik ayrıntılı bir araştırma gerçekleştirmek için Windows Defender ATP konsoluna erişebilir.

![Azure Güvenlik Merkezi, uyarıların bir listesini ve her uyarıyla ilgili genel bilgileri görüntüleme](media/security-center-wdatp/image1.png)

Windows Defender ATP 'ye özetleyerek uyarıyı daha fazla araştırıp inceleyebilirsiniz. Uyarı işlem ağacı ve olay grafiği gibi ek bilgileri görebilirsiniz. Ayrıca, altı aya kadar geçmiş bir dönem için her davranışı gösteren ayrıntılı bir makine zaman çizelgesi görebilirsiniz.

![Uyarı hakkında ayrıntılı bilgi içeren Windows Defender ATP sayfası](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Platform desteği

Güvenlik Merkezi 'nde Windows Defender ATP, standart bir hizmet aboneliğinde Windows Server 2016, 2012 R2 ve 2008 R2 SP1 işletim sistemlerinde algılamayı destekler.

> [!NOTE]
> Sunucuları izlemek için Azure Güvenlik Merkezi 'ni kullandığınızda, Windows Defender ATP kiracısı otomatik olarak oluşturulur ve Windows Defender ATP verileri varsayılan olarak Avrupa 'da depolanır. Verilerinizi başka bir konuma taşımanız gerekiyorsa, kiracıyı sıfırlamak için Microsoft Desteği başvurmanız gerekir.

## <a name="onboarding-servers-to-security-center"></a>Sunucuları güvenlik merkezi 'ne ekleme 

Sunucuları güvenlik merkezi 'ne eklemek için, Windows Defender ATP sunucu ekleme işleminden sunucu eklemek **üzere Azure Güvenlik Merkezi 'Ne git** ' e tıklayın.

1. **Ekleme** dikey penceresinde, verileri depolamak için bir çalışma alanı seçin veya oluşturun. <br>
2. Tüm çalışma alanlarınızı göremiyorsanız, bunun nedeni izinlerin bulunmaması olabilir, çalışma alanınızın Azure Güvenlik Standart katmanı olarak ayarlandığından emin olun. Daha fazla bilgi için bkz. [Gelişmiş güvenlik Için Güvenlik Merkezi 'Nin standart katmanına yükseltme](security-center-pricing.md).
    
3. Microsoft Monitoring Agent nasıl yükleneceğine ilişkin yönergeleri görüntülemek için **Sunucu Ekle** ' yi seçin. 

4. Ekleme işleminden sonra, **işlem ve uygulamalar**altında makineleri izleyebilirsiniz.

   ![Yerleşik bilgisayarlar](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-windows-defender-atp-integration"></a>Windows Defender ATP tümleştirmesini etkinleştir

Windows Defender ATP tümleştirmesi 'nin etkin olup olmadığını görmek için **Güvenlik Merkezi** > **fiyatlandırma & ayarları** ' nı seçin > aboneliğinize tıklayın.

  ![Azure Güvenlik Merkezi Ilke yönetimi](media/security-center-wdatp/policy-management.png)

Burada, şu anda etkin olan entegrasyona bakabilirsiniz.

  ![Windows Defender ATP tümleştirmesi etkin olan Azure Güvenlik Merkezi tehdit algılama ayarları sayfası](media/security-center-wdatp/enable-integrations.png)

- Sunucuları zaten Azure Güvenlik Merkezi Standart katmanına eklendi, başka bir işlem gerçekleştirmeniz gerekir. Azure Güvenlik Merkezi, sunucuları otomatik olarak Windows Defender ATP 'ye eklenecektir. Bu işlem 24 saate kadar sürebilir.

- Sunucuları hiçbir zaman Azure Güvenlik Merkezi Standart katmanına eklendi, bunları her zamanki gibi Azure Güvenlik Merkezi 'ne ekleyin.

- Eklendi Windows Defender ATP aracılığıyla sunucuları varsa:
  - [Sunucu makinelerini nasıl boşaltıkılabileceğine](https://go.microsoft.com/fwlink/p/?linkid=852906)ilişkin yönergeler için belgelere bakın.
  - Bu sunucuları Azure Güvenlik Merkezi 'ne ekleyin.

## <a name="access-to-the-windows-defender-atp-portal"></a>Windows Defender ATP portalına erişim

[Portala kullanıcı erişimi atama](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection)' daki yönergeleri izleyin.

## <a name="set-the-firewall-configuration"></a>Güvenlik duvarı yapılandırmasını ayarlama

Anonim trafiği engelleyen bir ara sunucu veya güvenlik duvarınız varsa, bir Windows Defender ATP algılayıcısı sistem bağlamından bağlandığında, anonim trafiğe izin verildiğini doğrulayın. [Proxy sunucusundaki Windows Defender ATP hizmeti URL 'lerine erişimi etkinleştirme](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server)' deki yönergeleri izleyin.

## <a name="test-the-feature"></a>Özelliği test etme

Bir zararsız Windows Defender ATP test uyarısı oluşturmak için:

1. Uzak Masaüstü 'Nü kullanarak bir Windows Server 2012 R2 VM 'sine veya Windows Server 2016 sanal makinesine erişin.  Bir komut istemi penceresi açın.

2. Komut isteminde aşağıdaki komutu kopyalayın ve çalıştırın. Komut Istemi penceresi otomatik olarak kapatılacak.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-WDATP-test\\invoice.exe'); Start-Process 'C:\\test-WDATP-test\\invoice.exe'
    ```

   ![Yukarıdaki komutla bir komut Istemi penceresi](media/security-center-wdatp/image4.jpeg)

3. Komut başarılı olursa, Azure Güvenlik Merkezi panosunda ve Windows Defender ATP portalında yeni bir uyarı görürsünüz. Bu uyarının görünmesi birkaç dakika sürebilir.

4. Güvenlik Merkezi 'nde uyarıyı gözden geçirmek için **güvenlik uyarıları** >  **şüpheli PowerShell komut satırı**bölümüne gidin.

5. Araştırma penceresinde, Windows Defender ATP portalına gitmek için bağlantıyı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi tarafından desteklenen platformlar ve özellikler](security-center-os-coverage.md)
- [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md): Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
- [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md): Önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
- [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
