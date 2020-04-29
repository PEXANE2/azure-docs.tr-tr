---
title: ArrInspector inceleme aracı
description: ArrInspector aracının Kullanıcı el kitabı
author: florianborn71
ms.author: flborn
ms.date: 03/09/2020
ms.topic: article
ms.openlocfilehash: e3acfc15b0c12822e48009bef4aabadac701fb2d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80680082"
---
# <a name="the-arrinspector-inspection-tool"></a>ArrInspector inceleme aracı

ArrInspector, çalışan bir Azure uzaktan Işleme oturumu denetlemek için kullanılan Web tabanlı bir araçtır. Hata ayıklama amacıyla, işlenen sahnenin yapısını incelemek, günlük iletilerini göstermek ve sunucudaki canlı performansı izlemek için kullanılması amaçlanmıştır.

![ArrInspector](./media/arr-inspector.png)

## <a name="connecting-to-the-arrinspector"></a>ArrInspector 'e bağlanma

ARR sunucunuzun ana bilgisayar adını (bitiş `mixedreality.azure.com`) edindikten sonra, [ConnectToArrInspectorAsync](../../how-tos/frontend-apis.md#connect-to-arr-inspector)kullanarak bağlanın. Bu işlev, uygulamanın `StartArrInspector.html` çalıştığı cihazda bir oluşturur. ArrInspector başlatmak için, bu dosyayı bir BILGISAYARDAKI tarayıcıyla (Edge, Firefox veya Chrome) açın. Dosya yalnızca 24 saat için geçerlidir.

Çağıran `ConnectToArrInspectorAsync` uygulama zaten bir bilgisayarda çalışıyorsa:

* Unity tümleştirmesi kullanıyorsanız, sizin için otomatik olarak başlatılabilir.
* Aksi takdirde, dosyayı *Kullanıcı klasörleri\\LocalAppData\\[your_app]\\AC\\geçici*bölümünde bulabilirsiniz.

Uygulama bir HoloLens üzerinde çalışıyorsa:

1. [Windows cihaz portalını](https://docs.microsoft.com/windows/mixed-reality/using-the-windows-device-portal)kullanarak HoloLens 'e erişin.
1. *System > dosya Gezgini*' ne gidin.
1. *Kullanıcı klasörleri\\LocalAppData\\[your_app]\\AC\\geçici*öğesine gidin.
1. *Startarrınspector. html* dosyasını bilgisayarınıza kaydedin.
1. Oturumun ArrInspector yüklemek için *Startarrınspector. html* dosyasını açın.

## <a name="the-performance-panel"></a>Performans paneli

![Performans paneli](./media/performance-panel.png)

Bu panelde, sunucu tarafından kullanıma sunulan tüm çerçeve başına performans değerlerinin grafikleri gösterilmektedir. Şu anda değerler çerçeve süresi, FPS, CPU ve bellek kullanımı, genel RAM kullanımı, nesne sayıları vb. gibi bellek istatistikleri içerir.

Bu parametrelerden birini görselleştirmek için **Yeni Ekle** düğmesine tıklayın ve iletişim kutusunda gösterilen kullanılabilir değerlerden birini seçin. Bu eylem panele yeni bir kayan grafik ekler ve değerleri gerçek zamanlı olarak izliyor. Sağ tarafta en *düşük*, *en yüksek* ve *geçerli* değeri görebilirsiniz.

Grafiğin içeriğini fareyle sürükleyerek grafiği kaydırabilirsiniz, ancak yatay kaydırma yalnızca ArrInspector duraklatılmış durumdaysa mümkündür.

Sürüklerken CTRL tuşunu basılı tutarak yakınlaştırmanızı sağlayabilirsiniz. Yatay yakınlaştırma, alt kısımdaki kaydırıcı ile de denetlenebilir.

Dikey Aralık varsayılan olarak, şu anda görüntülenen değerlere göre hesaplanır ve en az ve en büyük değerler sağdaki metin kutularında gösterilir. Değerler el ile ayarlandığında, metin kutusuna doğrudan yazılarak veya kaydırma/yakınlaştırma ile, grafik bu değerleri kullanır. Otomatik Dikey çerçeveleme geri yüklemek için sağ üst köşedeki simgeye tıklayın.

![dikey Aralık](./media/vertical-range.png)

## <a name="the-log-panel"></a>Günlük bölmesi

![Günlük bölmesi](./media/log-panel.png)

Günlük panelinde sunucu tarafında oluşturulan günlük iletilerinin bir listesi gösterilir. Bağlantıda, en fazla 200 önceki günlük iletisi görüntülenir ve bunlar, olduğu gibi yeni olanları yazdıracaktır.

Üstteki düğmeleri kullanarak listeyi günlük türüne `[Error/Warning/Info/Debug]` göre filtreleyebilirsiniz.
![Günlük Filtresi düğmeleri](./media/log-filter.png)

## <a name="the-timing-data-capture-panel"></a>Zamanlama verileri yakalama paneli

![Zamanlama verileri yakalama](./media/timing-data-capture.png)

Bu panel, sunucudan zamanlama bilgilerini yakalamak ve indirmek için kullanılır. Dosya [Chrome Izleme JSON biçimini](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/edit)kullanır. Verileri incelemek için, URL `Chrome://tracing` 'de Chrome ' u açın ve indirilen dosyayı sayfaya sürükleyip bırakın. Zamanlama verileri, sabit boyutlu bir halka arabelleğinde sürekli olarak toplanır. Bu sırada, yakalama yalnızca anında geçmiş hakkında bilgiler içerir ve birkaç dakika bekleyin.

## <a name="the-scene-inspection-panel"></a>Sahne Incelemesi bölmesi

![Sahne Inceleme paneli](./media/scene-inspection-panel.png)

Bu panel, işlenmiş sahnenin yapısını gösterir. Nesne hiyerarşisi sol tarafta, seçili nesnenin içeriği sağ tarafta. Panel salt okunurdur ve gerçek zamanlı olarak güncelleştirilir.

## <a name="the-vm-debug-information-panel"></a>VM hata ayıklama bilgileri bölmesi

![VM hata ayıklama bilgileri bölmesi](./media/state-debugger-panel.png)

Bu panelde bazı hata ayıklama işlevleri sunulmaktadır.

### <a name="restart-service"></a>Hizmeti yeniden Başlat

**Hizmeti yeniden Başlat** düğmesi, arrInspector 'in bağlandığı sanal makinede çalışma zamanını yeniden başlatır. Tüm bağlı istemcilerin bağlantısı kesilir ve yeniden başlatılan hizmete bağlanmak için arrInspector sayfasının yeniden yüklenmesi gerekir.

### <a name="collect-debug-information"></a>Hata ayıklama bilgilerini topla

**VM Için hata ayıklama bilgilerini topla** DÜĞMESI, sanal makinede hata ayıklama bilgilerini toplamak için ARR örneğini tetiklemeyi sağlayan bir iletişim kutusu açar:

![VM hata ayıklama bilgileri Iletişim kutusu](./media/state-debugger-dialog.png)

Hata ayıklama bilgileri, Azure uzaktan Işleme ekibinin çalışan bir ARR örneğinde oluşan sorunları çözümlemesine yardımcı olur. İletişim kutusunda ek ayrıntılar sağlayan bir metin alanı bulunur, örneğin bir sorunu yeniden oluşturma adımları.

**Toplamaya başla** düğmesine tıklandıktan sonra iletişim kutusu kapanır ve koleksiyon işlemi başlar. VM 'deki bilgilerin toplanması birkaç dakika sürebilir.

![VM hata ayıklama bilgileri toplama işlemi devam ediyor](./media/state-debugger-panel-in-progress.png)

Koleksiyon tamamlandıktan sonra ArrInspector penceresinde bir bildirim alırsınız. Bu bildirim, bu belirli koleksiyonu tanımlayan bir KIMLIK içerir. Azure uzaktan Işleme ekibine iletmek için bu KIMLIĞI kaydettiğinizden emin olun.

![VM hata ayıklama bilgileri toplama başarısı](./media/state-debugger-snackbar-success.png)

> [!IMPORTANT]
> VM hata ayıklama bilgilerine indiremez veya erişemezsiniz. Toplanan verilere yalnızca Azure uzaktan Işleme ekibinin erişimi vardır. Bizimle iletişime geçerek, gördüğünüz sorunu araştırmanız için koleksiyon KIMLIĞINI de göndermeniz gerekir.

## <a name="pause-mode"></a>Duraklatma modu

Sağ üst köşede, bir anahtar, panellerin canlı güncelleştirmesini duraklatmanızı sağlar. Bu mod, belirli bir durumu dikkatle incelemek için yararlı olabilir.

![Duraklatma modu](./media/pause-mode.png)

Canlı güncelleştirme yeniden etkinleştirildiğinde tüm paneller sıfırlanır.

## <a name="host-configuration"></a>Konak yapılandırması

Araç varsayılan olarak, ArrInspector hizmet veren ana bilgisayarda çalışan ARR sunucusuna bağlanır. Ancak, araç noktası açık olan bir ARR örneği çalıştırdığı varsayıldığında, başka bir sunucuyu incelemek üzere yapılandırabilirsiniz.

Bunu yapmak için, üst bilgi çubuğunun solundaki ana menüye erişin ve *konak yapılandırması*' nı seçin. **Yeni konak Ekle**' ye tıklayın ve adı ve ana bilgisayar adını girin. *Ana bilgisayar* adı için yalnızca içinde `.mixedreality.azure.com`biten ana bilgisayar adını kullanın `http://` , bir bağlantı noktası eklemeyin.

![Konak yapılandırması](./media/host-configuration.png)

Bir konaktan diğerine hızlıca geçiş yapmak için sağ üst köşedeki açılan eklentiyi kullanın.

![Konak açılan kutusu](./media/host-switch-combo.png)

Konak listesi tarayıcı yerel deposunda depolanır, bu nedenle aynı tarayıcı yeniden kullanılırken korunur.
