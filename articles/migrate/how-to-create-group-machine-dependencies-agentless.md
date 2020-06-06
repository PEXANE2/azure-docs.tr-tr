---
title: Azure geçişi sunucu değerlendirmesi ' nde aracısız bağımlılık analizini ayarlama
description: Azure geçişi sunucu değerlendirmesi ' nde aracısız bağımlılık analizini ayarlayın.
ms.topic: how-to
ms.date: 2/24/2020
ms.openlocfilehash: 3259c861b0e64b560eb2a17a832a02b87855bebf
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449201"
---
# <a name="set-up-agentless-dependency-visualization"></a>Aracısız bağımlılık görselleştirmesini ayarlama 

Bu makalede, Azure geçişi: Sunucu değerlendirmesi ' nde aracısız bağımlılık analizinin nasıl ayarlanacağı açıklanır. [Bağımlılık Analizi](concepts-dependency-visualization.md) , değerlendirmek ve Azure 'a geçirmek istediğiniz makineler arasında bağımlılıkları belirlemenize ve anlamanıza yardımcı olur.


> [!IMPORTANT]
> Aracısız bağımlılık görselleştirmesi Şu anda yalnızca VMware VM 'Leri için Azure geçişi: Sunucu değerlendirmesi aracı 'yla birlikte bulunan önizlemededir.
> Özellikler sınırlı veya tamamlanmamış olabilir.
> Bu önizleme müşteri desteği kapsamında ele alınmıştır ve üretim iş yükleri için kullanılabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



## <a name="before-you-start"></a>Başlamadan önce

- Aracısız bağımlılık Analizi [hakkında bilgi edinin](concepts-dependency-visualization.md#agentless-analysis) .
- VMware VM 'Leri için aracısız bağımlılık görselleştirmesini ayarlamaya yönelik önkoşulları ve destek gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements)
- Bir Azure geçişi projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
- Zaten bir proje oluşturduysanız Azure geçişi: Sunucu değerlendirmesi [aracını eklediğinizden emin](how-to-assess.md) olun.
- Şirket içi makinelerinizi keşfetmeye yönelik bir [Azure geçiş](migrate-appliance.md) gereci ayarladığınızdan emin olun. [VMware](how-to-set-up-appliance-vmware.md) VM 'leri için bir gereç ayarlamayı öğrenin. Gereç, şirket içi makineleri bulur ve Azure geçişi: Sunucu değerlendirmesi ' ne meta veri ve performans verileri gönderir.


## <a name="current-limitations"></a>Geçerli sınırlamalar

- Artık, bağımlılık analizi görünümünde bir gruba sunucu ekleyemez veya gruptan sunucu kaldıramazsınız.
- Bir sunucu grubu için bağımlılık eşlemesi Şu anda kullanılamıyor.
- Şu anda, bağımlılık verileri tablolu biçimde indirilemez.

## <a name="create-a-user-account-for-discovery"></a>Bulma için bir kullanıcı hesabı oluşturma

Sunucu değerlendirmesinin, bulma için VM 'ye erişebilmesi için bir kullanıcı hesabı ayarlayın. Hesap gereksinimleri hakkında [bilgi edinin](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) .


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

    ![Export dependencies](./media/how-to-create-group-machine-dependencies-agentless/export.png)


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
