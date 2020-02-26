---
title: Azure geçişi 'nde aracısız bağımlılık görselleştirmesini ayarlama
description: Azure geçişi sunucu değerlendirmesi ' nde aracısız bağımlılık görselleştirmesini kullanarak grupları ayarlayın.
ms.topic: article
ms.date: 2/24/2020
ms.openlocfilehash: c9425ad1fa78f14a194d3fe13c259dadf4eb5eb6
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589139"
---
# <a name="set-up-agentless-dependency-visualization"></a>Aracısız bağımlılık görselleştirmesini ayarlama 

Bu makalede, Azure geçişi: Sunucu değerlendirmesi ' nde bağımlılık görselleştirmesinin nasıl ayarlanacağı açıklanır. [Bağımlılık görselleştirme](concepts-dependency-visualization.md#what-is-dependency-visualization) , değerlendirmek ve Azure 'a geçirmek istediğiniz makineler genelinde bağımlılıkları belirlemenize ve anlamanıza yardımcı olur.

Aracısız bağımlılık görselleştirme, makinelere hiçbir aracı yüklemeden makine bağımlılıklarını tanımlamanızı sağlar. Etkin olduğu makinelerden gelen TCP bağlantısı verilerini yakalayıp işe yarar.

> [!IMPORTANT]
> Aracısız bağımlılık görselleştirmesi şu an için Azure geçişi: Sunucu değerlendirmesi aracı ile keşfedilen yalnızca Azure VMware VM 'Leri için önizleme aşamasındadır.
> Özellikler sınırlı veya tamamlanmamış olabilir.
> Bu önizleme müşteri desteği kapsamında ele alınmıştır ve üretim iş yükleri için kullanılabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Artık, bağımlılık analizi görünümünde bir gruba sunucu ekleyemez veya gruptan sunucu kaldıramazsınız.
- Bir sunucu grubu için bağımlılık eşlemesi Şu anda kullanılamıyor.
- Şu anda, bağımlılık verileri tablolu biçimde indirilemez.

## <a name="before-you-start"></a>Başlamadan önce

- Aracısız bağımlılık görselleştirmesi ile ilişkili gereksinimleri ve maliyetleri [gözden geçirin](concepts-dependency-visualization.md#agentless-visualization) .
- Aracısız bağımlılık görselleştirmesini ayarlamaya yönelik [destek gereksinimlerini](migrate-support-matrix-vmware.md#agentless-dependency-visualization) gözden geçirin.
- Bir Azure geçişi projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
- Zaten bir proje oluşturduysanız Azure geçişi: Sunucu değerlendirmesi [aracını eklediğinizden emin](how-to-assess.md) olun.
- Şirket içi makinelerinizi keşfetmeye yönelik bir [Azure geçiş](migrate-appliance.md) gereci ayarladığınızdan emin olun. [VMware](how-to-set-up-appliance-vmware.md) VM 'leri için bir gereç ayarlamayı öğrenin. Gereç, şirket içi makineleri bulur ve Azure geçişi: Sunucu değerlendirmesi ' ne meta veri ve performans verileri gönderir.


## <a name="create-a-user-account-for-discovery"></a>Bulma için bir kullanıcı hesabı oluşturma

Sunucu değerlendirmesinin, bulma için VM 'ye erişebilmesi için bir kullanıcı hesabı ayarlayın. Bir kullanıcı hesabı belirtebilirsiniz.

- **Windows VM 'leri**: Kullanıcı hesabının yerel veya etki alanı yöneticisi olması gerekir.
- **Linux VM 'leri**: hesapta kök ayrıcalık gereklidir. Alternatif olarak, Kullanıcı hesabı/bin/netstat ve/bin/ls dosyalarında şu iki özelliği gerektirir: CAP_DAC_READ_SEARCH ve CAP_SYS_PTRACE.

## <a name="add-the-user-account-to-the-appliance"></a>Kullanıcı hesabını gereç 'e ekleme

Kullanıcı hesabını gereç öğesine ekleyin.

1. Gereç Yönetimi uygulamasını açın. 
2. **VCenter ayrıntıları sağla** paneline gidin.
3. **VM 'lerde uygulama ve bağımlılıkları keşfet**bölümünde **kimlik bilgileri ekle** ' ye tıklayın.
3. **İşletim sistemini**seçin, hesap için bir kolay ad ve **Kullanıcı adı**/**parolayı** girin
6. **Kaydet**’e tıklayın.
7. **Kaydet ve bulmayı Başlat**' a tıklayın.

    ![VM Kullanıcı hesabı ekle](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Bağımlılık bulmayı Başlat

Üzerinde bağımlılık bulmayı etkinleştirmek istediğiniz makineleri seçin.

1. **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular**' a tıklayın.
2. **Bağımlılık Analizi** simgesine tıklayın.
3. **Sunucu Ekle**' ye tıklayın.
3. **Sunucu Ekle** sayfasında, ilgili makineleri bulan gereci seçin.
4. Makine listesinden makineleri seçin.
5. **Sunucu Ekle**' ye tıklayın.

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
