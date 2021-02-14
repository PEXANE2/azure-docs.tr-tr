---
title: Windows sanal masaüstü önizlemesi Izleme sorunlarını giderme-Azure
description: Windows sanal masaüstü için Azure Izleyici ile ilgili sorunları giderme.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 1818dc558ba45e318b71e1443556cc48feaede8b
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100367682"
---
# <a name="troubleshoot-azure-monitor-for-windows-virtual-desktop-preview"></a>Windows sanal masaüstü için Azure Izleyici sorunlarını giderme (Önizleme)

>[!IMPORTANT]
>Windows sanal masaüstü için Azure Izleyici Şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Windows sanal masaüstü (Önizleme) için Azure Izleyici 'de yaygın sorunlara yönelik bilinen sorunlar ve çözümler sunulmaktadır.

## <a name="issues-with-configuration-and-setup"></a>Yapılandırma ve kurulum ile ilgili sorunlar

Yapılandırma çalışma kitabı kurulumu otomatik hale getirmek için düzgün çalışmıyorsa, ortamınızı el ile ayarlamak için bu kaynakları kullanabilirsiniz:

- Tanılamayı el ile etkinleştirmek veya Log Analytics çalışma alanına erişmek için bkz. [Log Analytics Için Windows sanal masaüstü tanılaması gönderme](diagnostics-log-analytics.md).
- Log Analytics uzantısını bir konağa el ile yüklemek için, bkz. [Windows için Log Analytics sanal makine uzantısı](../virtual-machines/extensions/oms-windows.md).
- Yeni bir Log Analytics çalışma alanı ayarlamak için, bkz. [Azure portal Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md).
- Performans sayaçlarını eklemek veya kaldırmak için bkz. [performans sayaçlarını yapılandırma](../azure-monitor/platform/data-sources-performance-counters.md).
- Log Analytics çalışma alanı için olayları yapılandırmak için, bkz. [Log Analytics aracısıyla Windows olay günlüğü veri kaynakları toplama](../azure-monitor/platform/data-sources-windows-events.md).

## <a name="my-data-isnt-displaying-properly"></a>Verilerim doğru görüntülenmiyor

Verileriniz doğru görüntülenmiyorsa, yapılandırmanızı, izinlerinizi denetleyin ve gerekli IP adreslerinin engellemesi kaldırılmış olup olmadığını denetleyin. 

- İlk olarak, [dağıtımınızı izlemek Için Windows sanal masaüstü Için Azure İzleyicisi 'Ni kullanma](azure-monitor.md)başlığı altında açıklandığı gibi yapılandırma çalışma kitabındaki tüm alanları doldurduğunuzdan emin olun. Herhangi bir sayaç veya olay eksikse, bunlarla ilişkili veriler Azure portal görünmez.

- Erişim izinlerinizi denetleyin & eksik izinler istemek için kaynak sahipleriyle iletişime geçin; Windows sanal masaüstünü izleyen herkes aşağıdaki izinleri gerektirir:

    - Windows sanal masaüstü kaynaklarınızı tutan Azure aboneliklerine yönelik okuma erişimi
    - Windows sanal masaüstü oturumu konaklarınızı tutan aboneliğin kaynak gruplarına okuma erişimi 
    - Log Analytics çalışma alanına okuma erişimi

- Azure Izleyici 'nin portala veri göndermesini sağlamak için sunucunuzun güvenlik duvarında giden bağlantı noktalarını açmanız gerekebilir. bkz. [giden bağlantı noktaları](https://docs.microsoft.com/azure/azure-monitor/app/ip-addresses). 

- Son etkinlikten veriler görmüyor musunuz? 15 dakika beklemek ve akışı yenilemek isteyebilirsiniz. Azure Izleyici, günlük verilerinin doldurulmasına yönelik 15 dakikalık bir gecikme süresine sahiptir. Daha fazla bilgi edinmek için bkz. [Azure izleyici 'de günlük verisi alma süresi](../azure-monitor/platform/data-ingestion-time.md).

Herhangi bir bilgi eksik değilse ancak verileriniz doğru şekilde görüntülenmiyorsa, sorguda veya veri kaynaklarında bir sorun olabilir. Bilinen sorunlarınızı ve sınırlamaları gözden geçirin. 

## <a name="i-want-to-customize-azure-monitor-for-windows-virtual-desktop"></a>Windows sanal masaüstü için Azure Izleyicisini özelleştirmek istiyorum

Windows sanal masaüstü için Azure Izleyici, Azure Izleyici çalışma kitaplarını kullanır. Çalışma kitapları, Windows sanal masaüstü çalışma kitabı şablonunun bir kopyasını kaydetmenizi ve kendi özelleştirmelerinizi yapmanızı sağlar.

Tasarım yaparak, özel çalışma kitabı şablonları ürün grubundan güncelleştirmeleri otomatik olarak benimsemez. Daha fazla bilgi için bkz. [çalışma kitabı tabanlı Öngörüler sorunlarını giderme](../azure-monitor/insights/troubleshoot-workbooks.md) ve [çalışma kitaplarına genel bakış](../azure-monitor/platform/workbooks-overview.md).

## <a name="i-cant-interpret-the-data"></a>Verileri yorumlayamıyorum

[Windows sanal masaüstü sözlüğü Için Azure izleyici](azure-monitor-glossary.md)'de veri koşulları hakkında daha fazla bilgi edinin.

## <a name="the-data-i-need-isnt-available"></a>İhtiyacım olan veriler kullanılamıyor

Daha fazla performans sayacını veya olayını izlemek isterseniz, bu kullanıcıların Log Analytics çalışma alanınıza gönderilmesini ve bunları konak Tanılama: konak tarayıcısı 'nda izlemenizi sağlayabilirsiniz. 

- Performans sayaçlarını eklemek için bkz. [performans sayaçlarını yapılandırma](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters#configuring-performance-counters)
- Windows olayları eklemek için bkz. [Windows olay günlüklerini yapılandırma](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-windows-events#configuring-windows-event-logs)

Bir sorunu tanılamaya yardımcı olmak için bir veri noktası bulunamıyor musunuz? Bize geri bildirim gönderin!

- Geri bildirimleri nasıl bırakacağınızı öğrenmek için bkz. [sorun giderme genel bakış, geri bildirim ve Windows sanal masaüstü desteği](troubleshoot-set-up-overview.md).
- Windows sanal masaüstü [Geri Bildirim Hub 'ında](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app) veya [UserVoice forumumuzdan](https://windowsvirtualdesktop.uservoice.com/forums/921118-general)Windows sanal masaüstü için geri bildirim de bırakabilirsiniz.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

Bunlar, şu anda farkında olduğumuz ve düzeltiyoruz olan sorunlar ve kısıtlamalardır:

- Tek seferde yalnızca bir konak havuzunu izleyebilirsiniz. 

- Sık kullanılan ayarları kaydetmek için çalışma kitabının özel bir şablonunu kaydetmeniz gerekir. Özel şablonlar ürün grubundan güncelleştirmeleri otomatik olarak benimsemez.

- Bazı hata iletileri, Kullanıcı dostu bir şekilde phrased değildir ve tüm hata iletileri belgelerde açıklanmaz.

- Toplam oturumlar performans sayacı oturumları küçük bir sayıya göre sayabilir ve toplam oturumlarınız, en fazla oturum sınırınız üzerinde bulunabilir.

- Kullanılabilir oturum sayısı, konak havuzundaki ölçeklendirme ilkelerini yansıtmıyor. 
    
- Nadir olarak bir bağlantının tamamlanma olayı eksik olabilir ve bu, zaman içindeki bağlantılar ve kullanıcının bağlantı durumu gibi bazı görselleri etkileyebilir.  
    
- Yapılandırma çalışma kitabı yalnızca kaynak grubuyla aynı bölgedeki Konakları yapılandırmayı destekler. 

- Bağlanma süresi, kullanıcıların kimlik bilgilerini girmesi için gereken süreyi içerir; Bu deneyimle ilişkili olmakla kalmaz, bazı durumlarda yanlış Peaks gösterilebilir. 
    

## <a name="next-steps"></a>Sonraki adımlar

Verilerin nasıl yorumlanacağı konusunda emin değilseniz veya ortak terimler hakkında daha fazla bilgi edinmek istiyorsanız, [Windows sanal masaüstü sözlüğü Için Azure İzleyicisi](azure-monitor-glossary.md)'ne göz atın. Windows sanal masaüstü için Azure Izleyicisini ayarlama ve kullanma hakkında bilgi edinmek istiyorsanız, bkz. [dağıtımınızı izlemek Için Windows sanal masaüstü Için Azure İzleyicisi 'Ni kullanma](azure-monitor.md).