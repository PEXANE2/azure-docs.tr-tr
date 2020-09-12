---
title: Microsoft Defender Gelişmiş tehdit koruması-Azure Güvenlik Merkezi
description: Bu belge, Azure Güvenlik Merkezi ile Microsoft Defender Gelişmiş tehdit koruması arasındaki tümleştirmeyi tanıtır.
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
ms.openlocfilehash: e1595d7e205c788f90177836f3c0370681b7747b
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569093"
---
# <a name="microsoft-defender-advanced-threat-protection-with-azure-security-center"></a>Azure Güvenlik Merkezi ile Microsoft Defender Gelişmiş tehdit koruması

Azure Güvenlik Merkezi, kapsamlı uç nokta algılama ve yanıt (EDR) özellikleri sağlamak için [Microsoft Defender Gelişmiş tehdit koruması (ATP)](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp) ile tümleşir.


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|**Genel kullanılabilirlik**|
|Fiyat|Standart katmanı|
|Desteklenen makineler:|![Yes](./media/icons/yes-icon.png) Windows çalıştıran Azure makineleri<br>![Yes](./media/icons/yes-icon.png) Windows çalıştıran Azure Arc makineleri|
|Gerekli roller ve izinler:|Tümleştirmeyi etkinleştirmek/devre dışı bırakmak için: **Güvenlik Yöneticisi** veya **sahibi**<br>Güvenlik Merkezi 'nde MDADTP uyarılarını görüntülemek için: **güvenlik okuyucu**, **okuyucu**, **kaynak grubu katılımcısı**, **kaynak grubu sahibi**, **Güvenlik Yöneticisi**, **abonelik sahibi**veya **abonelik katılımcısı**|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar.<br>![No](./media/icons/no-icon.png) Ortak Azure bulutlarında iş yüklerini çalıştıran GCC müşterileri<br>![Yes](./media/icons/yes-icon.png) US Gov<br>![No](./media/icons/no-icon.png) Çin gov, diğer gov|
|||


## <a name="microsoft-defender-atp-features-in-security-center"></a>Güvenlik Merkezi 'ndeki Microsoft Defender ATP özellikleri

Microsoft Defender ATP şunları sağlar:

- **Gelişmiş ihlal sonrası algılama algılayıcı**: Windows Server Için MICROSOFT Defender ATP sensörleri çok sayıda davranış sinyali toplar.

- **Analitik tabanlı, bulut destekli gönderi ihlali algılama**: MICROSOFT Defender ATP, değişen tehditlere hızlı bir şekilde uyum sağlar. Gelişmiş analiz ve büyük veri kullanır. Microsoft Defender ATP, bilinmeyen tehditleri algılamak için Windows, Azure ve Office 'teki sinyallerle Intelligent Security Graph güçlerinin korunmasını sağlar. İşlem yapılabilir uyarılar sağlar ve hızla yanıt vermenize olanak tanır.

- **Tehdit bilgileri**: MICROSOFT Defender ATP, saldırgan araçlarını, tekniklerini ve yordamlarını belirlediğinde uyarı oluşturur. Microsoft tehdit arayıcılar ve güvenlik ekipleri tarafından oluşturulan ve iş ortakları tarafından sunulan zeka tarafından düzenlenen verileri kullanır.


Defender ATP 'yi Azure Güvenlik Merkezi ile tümleştirerek aşağıdaki ek yetenekler de yararlı olabilir:

- **Otomatik ekleme**: Tümleştirme, Azure Güvenlik Merkezi tarafından izlenen Windows sunucuları Için MICROSOFT Defender ATP algılayıcısı 'nı otomatik olarak (windows Server 2019 çalıştırmadıkları müddetçe) otomatik olarak sunar.

- **Tek bir cam bölmesi**: Azure Güvenlik Merkezi konsolunda MICROSOFT Defender ATP uyarıları görüntülenir. Daha fazla araştırmak için Microsoft Defender ATP 'yi kullanın. Microsoft Defender ATP, uyarı işlem ağacı ve olay grafiği gibi ek bilgiler sağlar. Ayrıca, altı aya kadar geçmiş bir dönem için her davranışı gösteren ayrıntılı bir makine zaman çizelgesi görebilirsiniz.

    ![Uyarı hakkında ayrıntılı bilgi içeren Microsoft Defender ATP sayfası](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Platform desteği

Güvenlik Merkezi 'ndeki Microsoft Defender ATP, Windows Server 2016, 2012 R2 ve 2008 R2 SP1 üzerinde algılamayı destekler. Azure VM 'Ler için Standart katman aboneliğine ve Azure dışı VM 'Ler için yalnızca çalışma alanı düzeyinde Standart katmana ihtiyacınız vardır.

Bu tümleştirme kullanılarak sunucu uç noktası izleme Office 365 GCC müşterileri için devre dışı bırakıldı.

## <a name="data-storage-location"></a>Veri depolama konumu

Sunucuları izlemek için Azure Güvenlik Merkezi 'ni kullandığınızda, bir Microsoft Defender ATP kiracısı otomatik olarak oluşturulur. Microsoft Defender ATP tarafından toplanan veriler, sağlama sırasında tanımlandığı şekilde kiracının coğrafi konumunda depolanır. Sahte bir biçimde bulunan müşteri verileri, Birleşik Devletler merkezi depolama ve işleme sistemlerine de depolanabilir. 

Yapılandırıldıktan sonra, verilerinizin depolandığı konumu değiştiremezsiniz. Verilerinizi başka bir konuma taşımanız gerekiyorsa, kiracıyı sıfırlamak için Microsoft Desteği başvurun.


## <a name="onboard-servers-to-security-center"></a>Sunucuları güvenlik merkezi 'ne ekleme 

Sunucuları güvenlik merkezi 'ne eklemek için **Azure Güvenlik Merkezi 'Ne git** ' e tıklayarak sunucuları MICROSOFT Defender ATP sunucu ekleme işleminden ekleyin.

1. **Ekleme** alanında, verileri depolamak için bir çalışma alanı seçin veya oluşturun.

2. Tüm çalışma alanlarınızı göremiyorsanız, bunun nedeni izin olmaması olabilir, çalışma alanınızın Azure Güvenlik Standart fiyatlandırma katmanına ayarlandığından emin olun. Daha fazla bilgi için bkz. [Gelişmiş güvenlik Için Güvenlik Merkezi 'nin standart katmanına yükseltme](security-center-pricing.md).
    
3. Log Analytics aracısının nasıl yükleneceğine ilişkin yönergeleri görüntülemek için **Sunucu Ekle** ' yi seçin. 

4. Ekleme işleminden sonra, **işlem ve uygulamalar**altında makineleri izleyebilirsiniz.

   ![Yerleşik bilgisayarlar](media/security-center-wdatp/onboard-computers.png)

## <a name="enable-microsoft-defender-atp-integration"></a>Microsoft Defender ATP tümleştirmesini etkinleştir

Microsoft Defender ATP tümleştirmesi 'nin etkin olup olmadığını görmek için **Güvenlik Merkezi**  >  **fiyatlandırma & ayarları** ' nı seçin > aboneliğinize tıklayın.
Burada, şu anda etkin olan entegrasyona bakabilirsiniz.

  ![Microsoft Defender ATP tümleştirmesi etkin olan Azure Güvenlik Merkezi tehdit algılama ayarları sayfası](media/security-center-wdatp/enable-integrations.png)

- Sunucuları zaten Azure Güvenlik Merkezi Standart katmanına eklendi, başka bir işlem gerçekleştirmeniz gerekir. Azure Güvenlik Merkezi, sunucuları otomatik olarak Microsoft Defender ATP 'ye eklenecektir. Ekleme, 24 saate kadar sürebilir.

- Sunucuları hiçbir zaman Azure Güvenlik Merkezi Standart katmanına eklendi, bunları her zamanki gibi Azure Güvenlik Merkezi 'ne ekleyin.

- Sunucuları Microsoft Defender ATP aracılığıyla eklendi:
  - [Sunucu makinelerini nasıl boşaltıkılabileceğine](https://go.microsoft.com/fwlink/p/?linkid=852906)ilişkin yönergeler için belgelere bakın.
  - Bu sunucuları Azure Güvenlik Merkezi 'ne ekleyin.

## <a name="access-to-the-microsoft-defender-atp-portal"></a>Microsoft Defender ATP portalına erişim

1. [Portala kullanıcı erişimi atama](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access)' daki yönergeleri izleyin.

1. Anonim trafiği engelleyen bir ara sunucuya veya güvenlik duvarına sahip olup olmadığınızı denetleyin. Defender ATP algılayıcısı sistem bağlamından bağlanır, bu nedenle anonim trafiğe izin verilmelidir. Microsoft Defender ATP portalına bir daha fazla erişim sağlamak için [proxy sunucusundaki Microsoft Defender ATP hizmeti URL 'lerine erişimi etkinleştirme](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server)' deki yönergeleri izleyin.

## <a name="test-the-feature"></a>Özelliği test etme

Bir zararsız Microsoft Defender ATP test uyarısı oluşturmak için:

1. ' C:\Test-mbatp-test ' klasörünü oluşturun.

1. Uzak Masaüstü 'Nü kullanarak bir Windows Server 2012 R2 VM 'sine veya Windows Server 2016 sanal makinesine erişin. Komut satırı penceresi açın.

1. Komut isteminde aşağıdaki komutu kopyalayın ve çalıştırın. Komut Istemi penceresi otomatik olarak kapatılacak.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```

   ![Yukarıdaki komutla bir komut Istemi penceresi](media/security-center-wdatp/image4.jpeg)

1. Komut başarılı olursa, Azure Güvenlik Merkezi panosunda ve Microsoft Defender ATP portalında yeni bir uyarı görürsünüz. Bu uyarının görünmesi birkaç dakika sürebilir.

1. Güvenlik Merkezi 'nde uyarıyı gözden geçirmek için **güvenlik uyarıları**  >  **şüpheli PowerShell komut satırı**bölümüne gidin.

1. Araştırma penceresinde, Microsoft Defender ATP portalına gitmek için bağlantıyı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi tarafından desteklenen platformlar ve özellikler](security-center-os-coverage.md)
- [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md): Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
- [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md): önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.