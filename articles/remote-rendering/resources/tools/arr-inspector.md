---
title: ArrInspector denetim aracı
description: ArrInspector aracının kullanım kılavuzu
author: florianborn71
ms.author: flborn
ms.date: 03/09/2020
ms.topic: article
ms.openlocfilehash: e3acfc15b0c12822e48009bef4aabadac701fb2d
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680082"
---
# <a name="the-arrinspector-inspection-tool"></a>ArrInspector denetim aracı

ArrInspector, çalışan bir Azure Uzaktan İşlem oturumunu denetlemek için kullanılan web tabanlı bir araçtır. Hata ayıklama amacıyla, işlenen sahnenin yapısını incelemek, günlük iletilerini göstermek ve sunucudaki canlı performansı izlemek için kullanılacaktır.

![ArrInspector](./media/arr-inspector.png)

## <a name="connecting-to-the-arrinspector"></a>ArrInspector'e Bağlanma

ARR sunucunuzun ana `mixedreality.azure.com`bilgisayar adını (bitişle biteni) aldıktan sonra [ConnectToArrInspectorAsync](../../how-tos/frontend-apis.md#connect-to-arr-inspector)kullanarak bağlanın. Bu işlev, `StartArrInspector.html` uygulamanın çalıştığı aygıtta bir işlev oluşturur. ArrInspector'i başlatmak için, bu dosyayı bir bilgisayarda bir tarayıcı (Kenar, Firefox veya Chrome) ile açın. Dosya yalnızca 24 saat için geçerlidir.

Arama `ConnectToArrInspectorAsync` yapan uygulama zaten bir bilgisayarda çalışıyorsa:

* Unity tümleştirmesini kullanıyorsanız, otomatik olarak sizin için başlatılabilir.
* Aksi takdirde, dosyayı *Kullanıcı Klasörleri\\LocalAppData\\\\[your_app]\\AC Temp'te*bulabilirsiniz.

Uygulama HoloLens'te çalışıyorsa:

1. [Windows Aygıt Portalı'nı](https://docs.microsoft.com/windows/mixed-reality/using-the-windows-device-portal)kullanarak HoloLens'e erişin.
1. Dosya *Gezgini > Sisteme*gidin.
1. Kullanıcı *Klasörlerine\\Gidin LocalAppData\\[your_app]\\AC\\Temp*.
1. *StartArrInspector.html'i* bilgisayarınıza kaydedin.
1. Oturumun ArrInspector yüklemek için *StartArrInspector.html* açın.

## <a name="the-performance-panel"></a>Performans paneli

![Performans Paneli](./media/performance-panel.png)

Bu panel, sunucu tarafından açığa çıkarılan tüm kare başına performans değerlerinin grafiklerini gösterir. Değerleri şu anda çerçeve süresi, FPS, CPU ve bellek kullanımı, genel RAM kullanımı, nesne sayıları, vb gibi bellek istatistikleri içerir.

Bu parametrelerden birini görselleştirmek için **Yeni Ekle** düğmesini tıklatın ve iletişim kutusunda gösterilen kullanılabilir değerlerden birini seçin. Bu eylem, değerleri gerçek zamanlı olarak izleyerek panele yeni bir kaydırma grafiği ekler. Sağında *minimum,* *maksimum* ve *geçerli* değeri görebilirsiniz.

Ancak, içeriğini fareyle sürükleyerek grafiği kaydırabilirsiniz, ancak yatay kaydırma yalnızca ArrInspector duraklatılmış durumda olduğunda mümkündür.

Sürükleme sırasında CTRL'yi basılı tutmak, yakınlaştırmanızı sağlar. Yatay yakınlaştırma, alttaki kaydırıcı ile de kontrol edilebilir.

Dikey aralık varsayılan olarak görüntülenen değerlere göre hesaplanır ve sağdaki metin kutularında min ve max değerleri gösterilir. Değerler el ile ayarlandığında, doğrudan textbox'a yazarak veya kaydırma/yakınlaştırma ile grafik bu değerleri kullanır. Otomatik dikey çerçeveyi geri yüklemek için sağ üst köşedeki simgeyi tıklatın.

![dikey aralık](./media/vertical-range.png)

## <a name="the-log-panel"></a>Günlük paneli

![Günlük Paneli](./media/log-panel.png)

Günlük paneli, sunucu tarafında oluşturulan günlük iletilerinin listesini gösterir. Bağlantıda, 200'e kadar önceki günlük iletileri gösterir ve yenilerini olduğu gibi yazdırır.

Üstteki düğmeleri kullanarak listeyi `[Error/Warning/Info/Debug]` günlük türüne göre filtreleyebilirsiniz.
![Günlük Filtre Düğmeleri](./media/log-filter.png)

## <a name="the-timing-data-capture-panel"></a>Zamanlama Veri Yakalama paneli

![Zamanlama Veri Yakalama](./media/timing-data-capture.png)

Bu panel, zamanlama bilgilerini sunucudan yakalamak ve indirmek için kullanılır. Dosya, [Chrome İzleme JSON biçimini](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/edit)kullanır. Verileri incelemek için URL'de `Chrome://tracing` Chrome'u açın ve indirilen dosyayı sayfaya sürükleyip bırakın. Zamanlama verileri sürekli olarak sabit boyutlu bir zil arabelleğinde toplanır. Yazıldığında, yakalama yalnızca yakın geçmiş hakkında bilgi içerir, birkaç dakika birkaç saniye anlamına gelir.

## <a name="the-scene-inspection-panel"></a>Sahne İnceleme paneli

![Sahne Denetim Paneli](./media/scene-inspection-panel.png)

Bu panel, işlenen sahnenin yapısını gösterir. Nesne hiyerarşisi solda, seçili nesnenin içeriği sağdadır. Panel salt okunur ve gerçek zamanlı olarak güncellenir.

## <a name="the-vm-debug-information-panel"></a>VM Hata Ayıklama Bilgi paneli

![VM Hata Ayıklama Bilgi Paneli](./media/state-debugger-panel.png)

Bu panel bazı hata ayıklama işlevleri sunar.

### <a name="restart-service"></a>Hizmeti yeniden başlatın

**Hizmeti Yeniden Başlat** düğmesi, arrInspector'in bağlı olduğu sanal makinede çalışma süresini yeniden başlatır. Ekli istemcibağlantısı kesilir ve yeniden başlatılan hizmete bağlanmak için arrInspector sayfasının yeniden yüklenmesi gerekir.

### <a name="collect-debug-information"></a>Hata ayıklama bilgilerini toplama

**VM için Hata Ayıklama Bilgilerini Topla** düğmesi, VM'de hata ayıklama bilgileri toplamak için ARR örneğini tetiklemenize olanak tanıyan bir iletişim kutusu açar:

![VM Hata Ayıklama Bilgi İletişim](./media/state-debugger-dialog.png)

Hata ayıklama bilgileri, Azure Uzaktan İşlem ekibe çalışan bir ARR örneğinde oluşan sorunları çözümlemesine yardımcı olur. İletişim kutusunda ek ayrıntılar sağlamak için bir metin alanı vardır( örneğin bir sorunu yeniden oluşturma adımları.

**Toplamabaşlat** düğmesini tıklattıktan sonra iletişim kapatılacak ve toplama işlemi başlar. VM hakkında bilgi toplama birkaç dakika sürebilir.

![VM Hata Ayıklama Bilgi toplama devam ediyor](./media/state-debugger-panel-in-progress.png)

Koleksiyon tamamlandıktan sonra, ArrInspector penceresinde bir bildirim alırsınız. Bu bildirim, bu özel koleksiyonu tanımlayan bir kimlik içerir. Azure Uzaktan İşleme ekibine aktarmak için bu kimliği kaydettiğinizi unutmayın.

![VM Debug Bilgi toplama başarısı](./media/state-debugger-snackbar-success.png)

> [!IMPORTANT]
> VM hata ayıklama bilgilerini indiremez veya başka bir şekilde erişemezsiniz. Toplanan verilere yalnızca Azure Uzaktan İşlem ekibi erişebilir. Gördüğünüz sorunu araştırmamız için bizimle irtibata geçmeniz ve tahsilat kimliğini göndermeniz gerekmektedir.

## <a name="pause-mode"></a>Duraklatma modu

Sağ üst köşede, bir anahtar panellerin canlı güncellemesini duraklatmanızı sağlar. Bu mod, belirli bir durumu dikkatle incelemek için yararlı olabilir.

![Duraklatma Modu](./media/pause-mode.png)

Canlı güncelleştirmeyi yeniden etkinleştirirken, tüm paneller sıfırlanır.

## <a name="host-configuration"></a>Ana bilgisayar yapılandırması

Varsayılan olarak araç, ArrInspector'e hizmet veren aynı ana bilgisayarda çalışan ARR sunucusuna bağlanır. Ancak, araç bağlantı noktası açık bir ARR örneği çalıştırdığınızı varsayarak, başka bir sunucuyu incelemek üzere yapılandırabilirsiniz.

Bunu yapmak için, üstbilgi çubuğunun solundaki ana menüye erişin ve *Ana Bilgisayar yapılandırmasını*seçin. **Yeni ana bilgisayar ekle'yi**tıklatın ve adını ve ana bilgisayar adını girin. *Ana bilgisayar adı* için yalnızca `.mixedreality.azure.com`, eklemeyin `http://` veya bağlantı noktası olarak biten ana bilgisayar adını kullanın.

![Ana Bilgisayar Yapılandırması](./media/host-configuration.png)

Bir ana bilgisayardan diğerine hızlı bir şekilde geçiş yapmak için sağ üstteki açılır bırakmayı kullanın.

![Ev Sahibi Combo](./media/host-switch-combo.png)

Ana bilgisayar listesi tarayıcı yerel depolama sında depolanır, böylece aynı tarayıcıyı yeniden açarken korunur.
