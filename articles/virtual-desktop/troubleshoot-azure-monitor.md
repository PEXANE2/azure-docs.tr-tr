---
title: Windows sanal masaüstü önizlemesi Izleme sorunlarını giderme-Azure
description: Windows sanal masaüstü için Azure Izleyici ile ilgili sorunları giderme.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 91cf6729911cdb674c5451f172e76a2e9d5943e4
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96468025"
---
# <a name="troubleshoot-azure-monitor-for-windows-virtual-desktop-preview"></a>Windows sanal masaüstü için Azure Izleyici sorunlarını giderme (Önizleme)

>[!IMPORTANT]
>Windows sanal masaüstü için Azure Izleyici Şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Windows sanal masaüstü (Önizleme) için Azure Izleyici 'de yaygın sorunlara yönelik bilinen sorunlar ve çözümler sunulmaktadır.

## <a name="the-configuration-workbook-isnt-working-properly"></a>Yapılandırma çalışma kitabı düzgün çalışmıyor

Azure Izleyici yapılandırma çalışma kitabı çalışmıyorsa, parçalarını el ile ayarlamak için bu kaynakları kullanabilirsiniz:

- Tanılamayı el ile etkinleştirmek veya Log Analytics çalışma alanına erişmek için bkz. [Log Analytics Için Windows sanal masaüstü tanılaması gönderme](diagnostics-log-analytics.md).
- Log Analytics uzantısını bir konağa el ile yüklemek için, bkz. [Windows için Log Analytics sanal makine uzantısı](../virtual-machines/extensions/oms-windows.md).
- Yeni bir Log Analytics çalışma alanı ayarlamak için, bkz. [Azure portal Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md).
- Performans sayaçlarını eklemek veya kaldırmak için bkz. [performans sayaçlarını yapılandırma](../azure-monitor/platform/data-sources-performance-counters.md).
- Log Analytics çalışma alanı için olayları yapılandırmak için, bkz. [Log Analytics aracısıyla Windows olay günlüğü veri kaynakları toplama](../azure-monitor/platform/data-sources-windows-events.md).

Bunun yerine, sorun kaynağın bulunmaması veya gerekli izinlere sahip olmamasından kaynaklanabilir.

Abonelikte herhangi bir Windows sanal masaüstü kaynağı yoksa, *abonelik* parametresinde gösterilmez.

Doğru abonelikler için okuma erişiminiz yoksa, *abonelik* parametresinde gösterilmez ve verileri panoda göremezsiniz. Bu sorunu gidermek için abonelik sahibiyle iletişime geçin ve okuma erişimi isteyin.

## <a name="my-data-isnt-displaying-properly"></a>Verilerim doğru görüntülenmiyor

Verileriniz doğru şekilde görüntülenmiyorsa Azure Izleyici yapılandırma sürecinde bir sorun oluşmuş olabilir. İlk olarak, [dağıtımınızı izlemek Için Windows sanal masaüstü Için Azure İzleyicisi 'Ni kullanma](azure-monitor.md)başlığı altında açıklandığı gibi yapılandırma çalışma kitabındaki tüm alanları doldurduğunuzdan emin olun. Her zaman hem yeni hem de mevcut ortamların ayarlarını değiştirebilirsiniz. Herhangi bir sayaç veya olay eksikse, bunlarla ilişkili veriler Azure portal görünmez.

Herhangi bir bilgi eksik değilse ancak verileriniz doğru şekilde görüntülenmiyorsa, sorguda veya veri kaynaklarında bir sorun olabilir. 

Herhangi bir kurulum hatası görmüyorsanız ve istediğiniz verileri göremiyorsanız, 15 dakika beklemek ve akışı yenilemek isteyebilirsiniz. Azure Izleyici, günlük verilerini doldurmak için 15 dakikalık bir gecikme süresine sahiptir. Daha fazla bilgi edinmek için bkz. [Azure izleyici 'de günlük verisi alma süresi](../azure-monitor/platform/data-ingestion-time.md).

Son olarak, herhangi bir bilgi eksik değilse ancak verileriniz hala görünmüyorsa, sorguda veya veri kaynaklarında bir sorun olabilir. Bu durumda, sorunu çözmek için desteğe başvurmanız gerekebilir.

## <a name="i-want-to-customize-azure-monitor-for-windows-virtual-desktop"></a>Windows sanal masaüstü için Azure Izleyicisini özelleştirmek istiyorum

Windows sanal masaüstü için Azure Izleyici, Azure Izleyici çalışma kitaplarını kullanır. Çalışma kitapları, Windows sanal masaüstü çalışma kitabı şablonunun bir kopyasını kaydetmenizi ve kendi özelleştirmelerinizi yapmanızı sağlar.

Özelleştirilmiş şablonlar, ürün grubu özgün şablonu güncelleştirdikleri zaman güncellemiyor. Bu, çalışma kitapları aracında tasarım gereği, güncelleştirilmiş şablonun bir kopyasını kaydetmeniz ve güncelleştirmeleri benimsemek için özelleştirmelerinizi yeniden oluşturmanız gerekecektir. Daha fazla bilgi için bkz. [çalışma kitabı tabanlı Öngörüler sorunlarını giderme](../azure-monitor/insights/troubleshoot-workbooks.md) ve [çalışma kitaplarına genel bakış](../azure-monitor/platform/workbooks-overview.md).

## <a name="i-cant-interpret-the-data"></a>Verileri yorumlayamıyorum

[Windows sanal masaüstü sözlüğü Için Azure izleyici](azure-monitor-glossary.md)'de veri koşulları hakkında daha fazla bilgi edinin.

## <a name="the-data-i-need-isnt-available"></a>İhtiyacım olan veriler kullanılamıyor

Bir sorunu tanılamaya yardımcı olmak için bir veri noktası bulunamıyor musunuz? Bize geri bildirim gönderin!

- Geri bildirimleri nasıl bırakacağınızı öğrenmek için bkz. [sorun giderme genel bakış, geri bildirim ve Windows sanal masaüstü desteği](troubleshoot-set-up-overview.md).
- Windows sanal masaüstü [Geri Bildirim Hub 'ında](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app) veya [UserVoice forumumuzdan](https://windowsvirtualdesktop.uservoice.com/forums/921118-general)Windows sanal masaüstü için geri bildirim de bırakabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar

Bunlar şu anda farkında olduğumuz ve düzeltilmesi için çalışıyoruz:

- Şu anda yalnızca bir abonelik, kaynak grubu ve ana bilgisayar havuzunu tek seferde izlemek üzere seçebilirsiniz. Bu nedenle, bir kullanıcının deneyimini anlamak için Kullanıcı raporları sayfasını kullanırken, kullanıcının kullanmakta olduğu doğru konak havuzuna sahip olduğunuzu doğrulamanız veya verileri görselleri doldurmayacak şekilde emin olmanız gerekir.

- Çalışma kitabının özel bir şablonunu kaydetmediğiniz takdirde, sık kullanılan ayarları Azure Izleyici 'ye kaydetmek mümkün değildir. Bu, BT yöneticilerinin Windows sanal masaüstü için Azure Izleyicisini her açtıklarında abonelik adı, kaynak grubu adları ve konak havuzu tercihlerini girmesi gerektiği anlamına gelir.

- Şu anda Windows sanal masaüstü için Azure Izleyici 'den Excel 'e veri aktarmanın bir yolu yoktur.

- Seçili abonelik içindeki tüm ürünlerin tüm önem derecesi 1 Azure Izleyici uyarıları Genel Bakış sayfasında görünür. Bu, aboneliğin diğer ürünlerden gelen uyarılar Windows sanal masaüstü 'Nü etkileyebileceğinden tasarım ile yapılır. Şu anda sorgu, genel bakış sayfasından yüksek öncelikli önem derecesi 0 uyarılarını dışlayarak önem derecesi 1 uyarılarla sınırlıdır.

- Bazı hata iletileri, Kullanıcı dostu bir şekilde phrased değildir ve tüm hata iletileri belgelerde açıklanmaz.

## <a name="next-steps"></a>Sonraki adımlar

Verilerin nasıl yorumlanacağı konusunda emin değilseniz veya ortak terimler hakkında daha fazla bilgi edinmek istiyorsanız, [Windows sanal masaüstü sözlüğü Için Azure İzleyicisi](azure-monitor-glossary.md)'ne göz atın. Windows sanal masaüstü için Azure Izleyicisini ayarlama ve kullanma hakkında bilgi edinmek istiyorsanız, bkz. [dağıtımınızı izlemek Için Windows sanal masaüstü Için Azure İzleyicisi 'Ni kullanma](azure-monitor.md).