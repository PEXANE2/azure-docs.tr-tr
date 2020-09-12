---
title: Azure geçişi sunucu değerlendirmesi ' nde aracısız bağımlılık analizini ayarlama
description: Azure geçişi sunucu değerlendirmesi ' nde aracısız bağımlılık analizini ayarlayın.
ms.topic: how-to
ms.date: 6/08/2020
ms.openlocfilehash: 2e6e562a18fa2ee0b89416ea67cc15394e760ada
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536447"
---
# <a name="analyze-machine-dependencies-agentless"></a>Makine bağımlılıklarını analiz etme (aracısız)

Bu makalede, Azure geçişi: Sunucu değerlendirmesi ' nde aracısız bağımlılık analizinin nasıl ayarlanacağı açıklanır. [Bağımlılık Analizi](concepts-dependency-visualization.md) , değerlendirme ve Azure 'a geçiş için makineler genelinde bağımlılıkları belirlemenize ve anlamanıza yardımcı olur.


> [!IMPORTANT]
> Aracısız bağımlılık görselleştirmesi Şu anda Azure geçişi: Sunucu değerlendirmesi aracında bulunan VMware VM 'Leri için önizleme aşamasındadır.
> Özellikler sınırlı veya tamamlanmamış olabilir.
> Bu önizleme müşteri desteği kapsamında ele alınmıştır ve üretim iş yükleri için kullanılabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Bağımlılık analizi görünümünde, şu anda bir gruptan bir sunucu ekleyemez veya kaldırabilirsiniz.
- Bir sunucu grubu için bağımlılık eşlemesi Şu anda kullanılamıyor.
- Bağımlılık verileri toplama, 400 sunucuları için aynı anda ayarlanabilir. 400 toplu işler halinde sıralama yaparak daha yüksek sayıda sunucu analiz etme sağlayabilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce

- Desteklenen işletim sistemlerini ve gerekli izinleri [gözden geçirin](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) .
- Şunları yaptığınızdan emin olun:
    - Bir Azure geçişi projesi vardır. Bunu yapmazsanız, şimdi bir tane [oluşturun](how-to-add-tool-first-time.md) .
    - Projeye Azure geçişi: Sunucu değerlendirmesi aracını [eklemişseniz](how-to-assess.md) ' i kontrol edin.
    - Şirket içi makineleri keşfetmeye yönelik bir [Azure geçiş](migrate-appliance.md) gereci ayarlayın. VMware VM 'Leri için [bir gereç ayarlayın](how-to-set-up-appliance-vmware.md) . Gereç, şirket içi makineleri bulur ve Azure geçişi: Sunucu değerlendirmesi ' ne meta veri ve performans verileri gönderir.
- Çözümlemek istediğiniz her VM 'de VMware araçlarının (10,2 ' den sonraki bir sürüm) yüklü olduğundan emin olun.


## <a name="create-a-user-account-for-discovery"></a>Bulma için bir kullanıcı hesabı oluşturma

Sunucu değerlendirmesinin bağımlılıkları saptamak için VM 'ye erişebilmeleri için bir kullanıcı hesabı ayarlayın. Windows ve Linux VM 'Leri için hesap gereksinimleri hakkında [bilgi edinin](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) .


## <a name="add-the-user-account-to-the-appliance"></a>Kullanıcı hesabını gereç 'e ekleme

Kullanıcı hesabını gereç öğesine ekleyin.

1. Gereç Yönetimi uygulamasını açın. 
2. **VCenter ayrıntıları sağla** paneline gidin.
3. **VM 'lerde uygulama ve bağımlılıkları keşfet**bölümünde **kimlik bilgileri ekle** ' ye tıklayın.
3. **İşletim sistemini**seçin, hesap için bir kolay ad ve **Kullanıcı adı** / **parolasını** girin
6. **Kaydet**’e tıklayın.
7. **Kaydet ve bulmayı Başlat**' a tıklayın.

    ![VM Kullanıcı hesabı ekle](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Bağımlılık bulmayı Başlat

Üzerinde bağımlılık bulmayı etkinleştirmek istediğiniz makineleri seçin.

1. **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular**' a tıklayın.
2. **Bağımlılık Analizi** simgesine tıklayın.
3. **Sunucu Ekle**' ye tıklayın.
4. **Sunucu Ekle** sayfasında, ilgili makineleri bulan gereci seçin.
5. Makine listesinden makineleri seçin.
6. **Sunucu Ekle**' ye tıklayın.

    ![Bağımlılık bulmayı Başlat](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Bağımlılık bulmayı başlattıktan sonra altı saat etrafında bağımlılıkları görselleştirebilirsiniz.

## <a name="visualize-dependencies"></a>Bağımlılıkları görselleştirin

1. **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular**' a tıklayın.
2. Görüntülemek istediğiniz makineyi arayın.
3. **Bağımlılıklar** sütununda, **bağımlılıkları görüntüle** ' ye tıklayın.
4. **Zaman süresi** açılan listesini kullanarak Haritayı görüntülemek istediğiniz zaman aralığını değiştirin.
5. Seçilen makineye bağımlılığı olan makineleri listelemek için **istemci** grubunu genişletin.
6. Seçili makineden bağımlılığı olan makineleri listelemek için **bağlantı noktası** grubunu genişletin.
7. Bağımlı makinelerin herhangi birinin harita görünümüne gitmek için, makine adına tıklayın > **sunucu haritasını yükle**

    ![Sunucu bağlantı noktası grubunu ve yükleme sunucusu haritasını Genişlet](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![İstemci grubunu Genişlet ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Her bağımlılığın işlem düzeyi ayrıntılarını görüntülemek için seçili makineyi genişletin.

    ![İşlem göstermek için sunucuyu genişlet](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> Bağımlılık için işlem bilgileri her zaman kullanılabilir değildir. Kullanılabilir değilse, bağımlılık "Bilinmeyen işlem" olarak işaretlenen işlemle birlikte gösterilmiştir.

## <a name="export-dependency-data"></a>Bağımlılık verilerini dışarı aktar

1. **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular**' a tıklayın.
2. **Bağımlılık Analizi** simgesine tıklayın.
3. **Uygulama bağımlılıklarını dışarı aktar**' a tıklayın.
4. **Uygulama bağımlılıklarını dışarı aktar** sayfasında, ilgili makineleri bulan gereci seçin.
5. Başlangıç saatini ve bitiş saatini seçin. Verileri yalnızca son 30 güne indirebileceğinizi unutmayın.
6. **Bağımlılığı dışarı aktar**öğesine tıklayın.

Bağımlılık verileri bir CSV biçiminde verilir ve indirilir. İndirilen dosya, bağımlılık analizi için etkinleştirilen tüm makinelerde bağımlılık verilerini içerir. 

![Bağımlılıkları dışarı aktar](./media/how-to-create-group-machine-dependencies-agentless/export.png)

### <a name="dependency-information"></a>Bağımlılık bilgileri

İçe aktarılmış CSV içindeki her satır, belirtilen zaman yuvasında gözlemlenen bir bağımlılığa karşılık gelir. 

Aşağıdaki tablo, dışarıya aktarılmış CSV içindeki alanları özetler. Sunucu adı, uygulama ve işlem alanlarının yalnızca aracısız bağımlılık Analizi etkin olan sunucular için doldurulduğunu unutmayın.

**Alan adı** | **Ayrıntılar**
--- | --- 
Timeslot | Bağımlılığın gözlemlendiği zaman lotu. <br/> Bağımlılık verileri, şu anda 6 saatlik yuva üzerinden yakalanır.
Kaynak sunucu adı | Kaynak makinenin adı 
Kaynak uygulama | Kaynak makinedeki uygulamanın adı 
Kaynak işlem | Kaynak makinedeki işlemin adı 
Hedef sunucu adı | Hedef makinenin adı
Hedef IP | Hedef makinenin IP adresi
Hedef uygulama | Hedef makinedeki uygulamanın adı
Hedef işlem | Hedef makinedeki işlemin adı 
Hedef bağlantı noktası | Hedef makinedeki bağlantı noktası numarası


## <a name="stop-dependency-discovery"></a>Bağımlılık bulmayı durdur

Üzerinde bağımlılık bulmayı durdurmak istediğiniz makineleri seçin.

1. **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular**' a tıklayın.
2. **Bağımlılık Analizi** simgesine tıklayın.
3. **Sunucuları kaldır**' a tıklayın.
3. **Sunucuları kaldır** sayfasında, bağımlılık bulmayı durdurmayı planladığınız VM 'leri bulmak **için gereken gereci** seçin.
4. Makine listesinden makineleri seçin.
5. **Sunucuları kaldır**' a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

[Makineleri](how-to-create-a-group.md) değerlendirme için gruplandırın.
