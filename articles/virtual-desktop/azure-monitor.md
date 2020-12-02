---
title: Windows sanal masaüstü Izleyici önizlemeyi kullan-Azure
description: Windows sanal masaüstü için Azure Izleyicisi 'ni kullanma.
author: Heidilohr
ms.topic: how-to
ms.date: 12/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: e0be6decf28fcbb2edacd5019f567d26403b1f31
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96468034"
---
# <a name="use-azure-monitor-for-windows-virtual-desktop-to-monitor-your-deployment-preview"></a>Dağıtımınızı izlemek için Windows sanal masaüstü için Azure Izleyicisini kullanın (Önizleme)

>[!IMPORTANT]
>Windows sanal masaüstü için Azure Izleyici Şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows sanal masaüstü için Azure Izleyici (Önizleme), BT uzmanlarının Windows sanal masaüstü ortamlarını anlamasına yardımcı olan Azure Izleyici çalışma kitaplarında oluşturulmuş bir panodur. Bu konu, Windows sanal masaüstü ortamlarınızı izlemek üzere Windows sanal masaüstü için Azure Izleyici 'yi ayarlama konusunda size kılavuzluk eder.

## <a name="requirements"></a>Gereksinimler

Windows sanal masaüstü için Azure Izleyici 'yi kullanmaya başlamadan önce, aşağıdaki şeyleri ayarlamanız gerekir:

- İzlemeniz gereken tüm Windows sanal masaüstü ortamları, Azure Resource Manager ile uyumlu olan en son Windows sanal masaüstü sürümünü temel almalıdır.

- En az bir yapılandırılmış Log Analytics çalışma alanı.

- Log Analytics çalışma alanınızdaki şu şeyler için veri toplamayı etkinleştirin:
    - Tüm gerekli performans sayaçları
    - Windows sanal masaüstü için Azure Izleyici 'de kullanılan tüm performans sayaçları veya olayları
    - İzlemek istediğiniz ortamdaki tüm nesneler için tanılama aracı verileri.
    - İzlemek istediğiniz ortamdaki sanal makineler (VM).

Ortamınız için Windows sanal masaüstü için Azure Izleyicisini izleyen herkesin, aşağıdaki okuma erişim izinlerine de ihtiyacı vardır:

- Ortamın kaynaklarının bulunduğu kaynak grubuna yönelik okuma erişimi.

- Ortamın oturum ana bilgisayarlarının bulunduğu kaynak gruplarına yönelik okuma erişimi

>[!NOTE]
> Okuma erişimi yalnızca yöneticilerin verileri görüntülemesini sağlar. Windows sanal masaüstü portalındaki kaynakları yönetmek için farklı izinlere sahip olmaları gerekir.

## <a name="open-azure-monitor-for-windows-virtual-desktop"></a>Windows sanal masaüstü için Azure Izleyicisini açın

Windows sanal masaüstü için Azure Izleyici 'yi aşağıdaki yöntemlerden biriyle açabilirsiniz:

- [Aka.MS/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks)adresine gidin.

- Azure portal **Windows sanal masaüstü** araması yapın ve seçin ve ardından **Öngörüler**' i seçin.

- Azure portal **Azure izleyici** araması yapın ve seçin. **Öngörüler**' ın altında **Öngörüler hub** ' ı seçin ve **diğer** **Windows sanal masaüstü** ' nün altında panoyu Azure İzleyici sayfasında açmak için seçin.

Windows sanal masaüstü için Azure Izleyicisi 'ni açık olduktan sonra, izlemek istediğiniz ortama göre **abonelik**, **kaynak grubu**, **ana bilgisayar havuzu** ve **zaman aralığı** etiketli bir veya daha fazla onay kutusunu seçin.

>[!NOTE]
>Windows sanal masaüstü Şu anda yalnızca bir abonelik, kaynak grubu ve konak havuzunun aynı anda izlenmesini destekliyor. İzlemek istediğiniz ortamı bulamıyorsanız, bilinen özellik istekleri ve sorunları için [sorun giderme belgelerimize](troubleshoot-azure-monitor.md) bakın.!

## <a name="set-up-with-the-configuration-workbook"></a>Yapılandırma çalışma kitabıyla birlikte ayarlama

Windows sanal masaüstü için Azure Izleyici 'yi ilk kez açtığınızda, Windows sanal masaüstü kaynaklarınız için Azure Izleyici 'yi yapılandırmanız gerekir. Kaynaklarınızı yapılandırmak için:

1. Çalışma kitabınızı Azure portal açın.
2. **Yapılandırma çalışma kitabını aç**' ı seçin.

Yapılandırma çalışma kitabı, izleme ortamınızı ayarlar ve kurulum işlemini tamamladıktan sonra yapılandırmayı denetlemenizi sağlar. Panodaki öğeler doğru görüntülenmiyorsa veya ürün grubu ek veri noktaları gerektiren güncelleştirmeler yayımladığında, yapılandırmanızı denetlemeniz önemlidir.

## <a name="host-pool-diagnostic-settings"></a>Konak havuzu tanılama ayarları

Bu özelliği destekleyen Windows sanal masaüstü ortamı içindeki tüm nesnelerde Azure Izleyici tanılama ayarlarını etkinleştirmeniz gerekir.

1. Windows sanal masaüstü için Azure Izleyici [aka.MS/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks)adresinde açın ve sonra **yapılandırma çalışma kitabı**' nı seçin.

2. **Abonelik**, **kaynak grubu** ve **konak havuzu** altında izlenecek bir ortam seçin.

3. **Konak havuzu Tanılama ayarları** altında, konak havuzu Için Windows sanal masaüstü tanılama 'nın etkin olup olmadığını denetleyin. Bu değillerse, "seçili konak havuzu için var olan tanılama yapılandırması bulunamadı" ifadesini içeren bir hata iletisi görüntülenir. 
   
   Aşağıdaki tablolar etkinleştirilmelidir:

    - Checkpoint
    - Hata
    - Yönetim
    - Bağlantı
    - HostRegistration

    >[!NOTE]
    > Hata iletisini görmüyorsanız 4. adımı uygulamanız gerekmez.

4. **Konak havuzunu Yapılandır**' ı seçin.

5. **Dağıt**'ı seçin.

6. Çalışma kitabını yenileyin.

Windows Sanal Masaüstü ortamındaki tüm nesnelerde tanılamayı etkinleştirme hakkında daha fazla bilgi edinebilirsiniz veya [Windows sanal masaüstü tanılama 'yı Log Analytics gönderme](diagnostics-log-analytics.md)sırasında Log Analytics çalışma alanına erişebilirsiniz.

## <a name="configure-log-analytics"></a>Log Analytics’i Yapılandır

Windows sanal masaüstü için Azure Izleyici 'yi kullanmaya başlamak için, izlemeyi planladığınız ortamdan veri toplamak ve çalışma kitabına sağlamak için en az bir Log Analytics çalışma alanına ihtiyacınız olacaktır. Zaten bir ayarladıysanız, [performans sayaçlarını ayarlamaya](#set-up-performance-counters)devam edin. Windows sanal masaüstü ortamınızı içeren Azure aboneliği için yeni bir Log Analytics çalışma alanı ayarlamak için, bkz. [Azure portal Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md).

>[!NOTE]
>Log Analytics için standart veri depolama ücretleri uygulanır. Başlamak için Kullandıkça Öde modelini seçmenizi ve dağıtımınızı ölçeklendirdiğiniz ve daha fazla veri alacak şekilde ayarlamanız önerilir. Daha fazla bilgi için bkz. [Azure izleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/).

### <a name="set-up-performance-counters"></a>Performans sayaçlarını ayarlama

Log Analytics çalışma alanındaki karşılık gelen örnek aralıktaki koleksiyon için belirli performans sayaçlarını etkinleştirmeniz gerekir. Bu performans sayaçları, Windows sanal masaüstünü izlemeniz için gereken tek sayaçlardır. Maliyetleri kaydetmek için diğerlerinin tümünü devre dışı bırakabilirsiniz.

Performans sayaçlarınız zaten etkinse ve bunları kaldırmak istiyorsanız, performans Sayaçlarınızı yeniden yapılandırmak için [performans sayaçlarını yapılandırma](../azure-monitor/platform/data-sources-performance-counters.md) konusundaki yönergeleri izleyin. Bu makale sayaçların nasıl ekleneceğini açıklar ancak aynı konumda da kaldırabilirsiniz.

Daha önce performans sayaçlarını ayarlamadıysanız, Windows sanal masaüstü için Azure Izleyici için nasıl yapılandırılacağı aşağıda verilmiştir:

1. [Aka.MS/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks)adresine gidin ve ardından pencerenin **yapılandırma çalışma kitabını** seçin.

2. **Log Analytics yapılandırma** altında, aboneliğiniz için ayarladığınız çalışma alanını seçin.

3. **Çalışma alanı performans sayaçları**' nda, izleme için gereken sayaçların listesini görürsünüz. Bu listenin sağ tarafında, çalışma alanınızı izlemeye başlamak için ihtiyacınız olan sayaçları etkinleştirmek üzere **eksik sayaçlar** listesindeki öğeleri kontrol edin.

4. **Performans sayaçlarını Yapılandır**' ı seçin.

5. **Yapılandırma Uygula**' yı seçin.

6. Yapılandırma çalışma kitabını yenileyin ve ortamınızı ayarlamaya devam edin.

Ayrıca, hizmet her güncelleştirildiğinde ilk yapılandırmadan sonra yeni performans sayaçları ekleyebilir ve yeni izleme araçları gerekir. Tüm gerekli sayaçların devre dışı olduğunu, **eksik sayaçlar** listesinden seçerek doğrulayabilirsiniz.

>[!NOTE]
>Giriş gecikmesi performans sayaçları yalnızca Windows 10 RS5 ve üzeri ya da Windows Server 2019 ve üzeri sürümlerle uyumludur.

Koleksiyon için önceden etkinleştirilmemiş performans sayaçlarını el ile ekleme hakkında daha fazla bilgi edinmek için bkz. [performans sayaçlarını yapılandırma](../azure-monitor/platform/data-sources-performance-counters.md).

### <a name="set-up-windows-events"></a>Windows olaylarını ayarlama

Daha sonra, Log Analytics çalışma alanında koleksiyon için belirli Windows olaylarını etkinleştirmeniz gerekir. Bu bölümde açıklanan olaylar, Windows sanal masaüstü ihtiyaçlarına yönelik tek Azure Izleyicisine yöneliktir. Maliyetleri kaydetmek için diğerlerinin tümünü devre dışı bırakabilirsiniz.

Windows olaylarını ayarlamak için:

1. Zaten etkinleştirilmiş Windows olayları varsa ve bunları kaldırmak istiyorsanız, izleme için gerekli kümeyi etkinleştirmek üzere yapılandırma çalışma kitabını kullanmadan önce istemediğiniz olayları kaldırın.

2. [Aka.MS/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks)adresindeki Windows sanal masaüstü Için Azure İzleyicisi ' ne gidin ve ardından pencerenin alt kısmındaki **yapılandırma çalışma kitabı** ' nı seçin.

3. **Windows olayları yapılandırması**'nda, izleme için gerekli olan Windows olaylarının bir listesi vardır. Bu listenin sağ tarafında, gerekli olay adlarını ve şu anda çalışma alanınız için etkin olmayan olay türlerini bulacağınız **eksik olaylar** listesi bulunur. Bu adların her birini daha sonra kaydedin.

4. **Açık çalışma alanları yapılandırması**' nı seçin.

5. **Veri** seçin.

6. **Windows olay günlükleri**' ni seçin.

7. Adım 3 ' ten eksik olay adlarını ve her biri için gerekli olay türünü ekleyin.

8. Yapılandırma çalışma kitabını yenileyin ve ortamınızı ayarlamaya devam edin.

İzleme aracı güncelleştirmeleri yeni olayların etkinleştirilmesini gerektiriyorsa, ilk yapılandırmadan sonra yeni Windows olayları ekleyebilirsiniz. Tüm gerekli olaylarınızın etkin olduğundan emin olmak için **eksik olaylar** listesine geri dönün ve burada gördüğünüz eksik olayları etkinleştirin.

## <a name="install-the-log-analytics-agent-on-all-hosts"></a>Log Analytics aracısını tüm konaklara yükler

Son olarak, konaklardan seçili çalışma alanına veri göndermek için konak havuzundaki tüm konaklara Log Analytics aracısını yüklemeniz gerekir.

Log Analytics aracısını yüklemek için:

1. [Aka.MS/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks)adresindeki Windows sanal masaüstü Için Azure İzleyicisi ' ne gidin ve ardından pencerenin alt kısmındaki **yapılandırma çalışma kitabı** ' nı seçin.

2. Konak havuzundaki tüm konaklar için Log Analytics yapılandırılmamışsa, "konak havuzundaki bazı konaklar seçili Log Analytics çalışma alanına veri göndermiyor" iletisiyle birlikte Log Analytics yapılandırma bölümünün en altında bir hata görürsünüz. Seçilen konakları eklemek için **çalışma alanına konaklar Ekle** ' yi seçin. Hata iletisini görmüyorsanız buradan durun.

3. Yapılandırma çalışma kitabını yenileyin.

>[!NOTE]
>Log Analytics uzantısını yüklemek için konak makinenin çalışıyor olması gerekir. Bir konakta otomatik dağıtım başarısız olursa, uzantıyı her zaman bir konağa el ile yükleyebilirsiniz. Uzantıyı el ile yüklemeyi öğrenmek için bkz. [Windows için sanal makine uzantısı Log Analytics](../virtual-machines/extensions/oms-windows.md).

## <a name="optional-configure-alerts"></a>İsteğe bağlı: uyarıları yapılandırma

Windows sanal masaüstü için Azure Izleyici 'yi, seçtiğiniz aboneliğinizde önemli bir Azure Izleyici uyarısı gerçekleştiğinde size bildirecek şekilde yapılandırabilirsiniz. Bunu yapmak için [Azure Izleyici uyarıları ile olaylara yanıt verme](../azure-monitor/learn/tutorial-response.md)bölümündeki yönergeleri izleyin.

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft, Azure Izleyici hizmetini kullanımınız aracılığıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft bu verileri hizmetin kalitesini, güvenliğini ve bütünlüğünü geliştirmek için kullanır.

Doğru ve verimli sorun giderme özellikleri sağlamak için, toplanan veriler Portal oturum KIMLIĞI, Azure Active Directory Kullanıcı KIMLIĞI ve olayın gerçekleştiği Portal sekmesinin adını içerir. Microsoft ad, adres veya diğer iletişim bilgileri toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için [Microsoft Online Services gizlilik bildirimi](https://privacy.microsoft.com/privacystatement)' ne bakın.

>[!NOTE]
>Hizmet tarafından toplanan kişisel verilerinizi görüntüleme veya silme hakkında bilgi edinmek için bkz. [GDPR Için Azure veri konu istekleri](/microsoft-365/compliance/gdpr-dsr-azure). GDPR hakkında daha fazla bilgi için, [hizmet güven PORTALıNıN GDPR bölümüne](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal masaüstü Azure portal yapılandırdığınıza göre, aşağıda size yardımcı olabilecek bazı kaynaklar verilmiştir:

- Windows sanal masaüstü için Azure Izleyici ile ilgili hüküm ve kavramlar hakkında daha fazla bilgi edinmek için [sözlüğümüze](azure-monitor-glossary.md) göz atın.
- Bir sorunla karşılaşırsanız yardım için [sorun giderme kılavuzumuzu](troubleshoot-azure-monitor.md) inceleyin.