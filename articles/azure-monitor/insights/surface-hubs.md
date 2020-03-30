---
title: Azure Monitör ile Yüzey Hub'larını Izleyin | Microsoft Dokümanlar
description: Yüzey Hub'larınızın durumunu izlemek ve bunların nasıl kullanıldığını anlamak için Surface Hub çözümünün kullanın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/16/2018
ms.openlocfilehash: 7999735bf2d182b2811d01172adcfc89cba27dc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77662510"
---
# <a name="monitor-surface-hubs-with-azure-monitor-to-track-their-health"></a>Sistem durumunu izlemek için Azure Monitörü ile Surface Hub'larını izleyin

![Yüzey Hub simgesi](./media/surface-hubs/surface-hub-symbol.png)

Bu makalede, Microsoft Surface Hub aygıtlarını izlemek için Azure Monitor'daki Surface Hub çözümünü nasıl kullanabileceğiniz açıklanmaktadır. Çözüm, Yüzey Hub'larınızın sistem durumunu izlemenize ve bunların nasıl kullanıldığını anlamanıza yardımcı olur.

Her Surface Hub'da Microsoft İzleme Aracısı yüklü. Surface Hub'ınızdan Azure Monitor'daki Bir Günlük Analizi çalışma alanına veri gönderebileceğiniz aracı aracılığıyla. Günlük dosyaları Surface Hub'larınızdan okunur ve Azure Monitor'a gönderilir. Sunucuların çevrimdışı olması, takvimin eşitlenmemesi veya cihaz hesabının Skype'ta oturum açamaması gibi sorunlar Azure Monitor'daki Surface Hub panosunda gösterilir. Panodaki verileri kullanarak, çalışmayan veya başka sorunlarla karşı çıkan aygıtları belirleyebilir ve algılanan sorunlar için düzeltmeleri uygulayabilirsiniz.

## <a name="install-and-configure-the-solution"></a>Çözümü yükleme ve yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın. Azure Monitör'de Surface Hub'larınızı yönetmek için aşağıdakilere ihtiyacınız vardır:

* İzlemek istediğiniz cihaz sayısını destekleyecek bir [Log Analytics abonelik](https://azure.microsoft.com/pricing/details/log-analytics/) düzeyi. Günlük Analizi fiyatlandırması, kaç cihazın kayıtlı olduğuna ve ne kadar veri işlediğine bağlı olarak değişir. Surface Hub ürününüzü planlarken bunu göz önünde bulundurmak isteyeceksiniz.

Ardından, varolan bir Log Analytics çalışma alanı ekler veya yeni bir çalışma alanı oluşturursunuz. Her iki yöntemi de kullanmak için ayrıntılı talimatlar [Azure portalında Bir Günlük Analizi oluştur çalışma alanındadır.](../learn/quick-create-workspace.md) Log Analytics çalışma alanı yapılandırıldıktan sonra Surface Hub aygıtlarınızı kaydetmenin iki yolu vardır:

* Intune ile otomatik olarak
* Surface Hub cihazınızdaki **Ayarlar** aracılığıyla el ile.

## <a name="set-up-monitoring"></a>İzlemeyi ayarlama
Azure Monitörünü kullanarak Surface Hub'ınızın durumunu ve etkinliğini izleyebilirsiniz. Intune'u kullanarak veya Yüzey Hub'ındaki **Ayarlar'ı** kullanarak Yüzey Hub'ını yerel olarak kaydedebilirsiniz.

## <a name="connect-surface-hubs-to-azure-monitor-through-intune"></a>Intune aracılığıyla Yüzey Hub'larını Azure Monitörüne bağlayın
Surface Hub'larınızı yönetecek Log Analytics çalışma alanı için çalışma alanı kimliğine ve çalışma alanı anahtarına ihtiyacınız vardır. Bunları Azure portalındaki çalışma alanı ayarlarından alabilirsiniz.

Intune, bir veya daha fazla aygıtınıza uygulanan Log Analytics çalışma alanı yapılandırma ayarlarını merkezi olarak yönetmenize olanak tanıyan bir Microsoft ürünüdür. Aygıtlarınızı Intune aracılığıyla yapılandırmak için aşağıdaki adımları izleyin:

1. Intune'da oturum açın.
2. **Ayarlara** > **Bağlı Kaynaklara**gidin.
3. Surface Hub şablonuna dayalı bir ilke oluşturun veya edin.
4. İlkenin Azure Operasyonel Öngörüleri bölümüne gidin ve ilkenin Giriş Analizi *Çalışma Alanı Kimliği* ve Çalışma Alanı *Anahtarını* ekleyin.
5. İlkeyi kaydedin.
6. İlkeyi uygun aygıt grubuyla ilişkilendirin.

   ![Intune İlkesi](./media/surface-hubs/intune.png)

Intune daha sonra Log Analytics ayarlarını hedef gruptaki cihazlarla senkronize ederek Log Analytics çalışma alanınıza kaydeder.

## <a name="connect-surface-hubs-to-azure-monitor-using-the-settings-app"></a>Ayarlar uygulamasını kullanarak Yüzey Hub'larını Azure Monitörüne bağlayın
Surface Hub'larınızı yönetecek Log Analytics çalışma alanı için çalışma alanı kimliğine ve çalışma alanı anahtarına ihtiyacınız vardır. Bunları Azure portalındaki Log Analytics çalışma alanı ayarlarından alabilirsiniz.

Ortamınızı yönetmek için Intune'u kullanmıyorsanız, aygıtları her Surface Hub'ındaki **Ayarlar** üzerinden el ile kaydedebilirsiniz:

1. Surface Hub'ınızdan **Ayarlar'ı**açın.
2. İstendiğinde aygıt yöneticisi kimlik bilgilerini girin.
3. **Bu aygıtı**ve altında **İzleme'yi**tıklatın, Günlük Analizi **Ayarlarını Yapılandır'ı**tıklatın.
4. **İzlemeyi Etkinleştir'i**seçin.
5. Günlük Analizi ayarları iletişim kutusunda, Günlük Analitiği **Çalışma Alanı Kimliğini** yazın ve Çalışma Alanı **Anahtarı'nı**yazın.  
   ![Ayarlar](./media/surface-hubs/settings.png)
6. Yapılandırmayı tamamlamak için **Tamam'ı** tıklatın.

Yapılandırmanın aygıta başarıyla uygulanıp uygulanmadığını bildiren bir onay görüntülenir. Varsa, aracının Azure Monitor'a başarıyla bağlandığını belirten bir ileti görüntülenir. Aygıt daha sonra görüntüleyip hareket edebileceğiniz Azure Monitor'a veri göndermeye başlar.

## <a name="monitor-surface-hubs"></a>Yüzey Hub'larını Monitörle
Azure Monitör'ü kullanarak Surface Hub'larınızı izlemek, kayıtlı diğer aygıtları izlemeye çok benzer.

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

Surface Hub döşemesini tıklattığınızda cihazınızın durumu görüntülenir.

   ![Yüzey Hub panosu](./media/surface-hubs/surface-hub-dashboard.png)

Varolan veya özel günlük aramalarını temel alan [uyarılar](../platform/alerts-overview.md) oluşturabilirsiniz. Azure Monitor'un Yüzey Hub'larınızdan topladığı verileri kullanarak, sorunlarını arayabilir ve aygıtlarınız için tanımladığınız koşullarda uyarıda bulunabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı Surface Hub verilerini görüntülemek için [Azure Monitor'da Günlük sorgularını](../log-query/log-query-overview.md) kullanın.
* Yüzey [Hub'larınızla](../platform/alerts-overview.md) ilgili sorunlar oluştuğunda sizi bilgilendirmek için uyarılar oluşturun.
