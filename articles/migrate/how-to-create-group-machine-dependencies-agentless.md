---
title: Azure Geçir Sunucusu Değerlendirmesi'nde aracısız bağımlılık çözümlemesi ayarlama
description: Azure Geçir Sunucusu Değerlendirmesi'nde aracısız bağımlılık çözümlemesi ayarlayın.
ms.topic: how-to
ms.date: 2/24/2020
ms.openlocfilehash: af767bf73a3b9a6f2a91298987f11974499fd694
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455715"
---
# <a name="set-up-agentless-dependency-visualization"></a>Aracısız bağımlılık görselleştirmeyi ayarlama 

Bu makalede, Azure Geçiş:Sunucu Değerlendirmesi'nde aracısız bağımlılık çözümlemesi nasıl ayarlanır. [Bağımlılık çözümlemesi,](concepts-dependency-visualization.md) değerlendirmek ve Azure'a geçirmek istediğiniz makinelerdeki bağımlılıkları belirlemenize ve anlamanıza yardımcı olur.


> [!IMPORTANT]
> Aracısız bağımlılık görselleştirmesi şu anda yalnızca Azure Geçiş:Sunucu Değerlendirmesi aracıyla keşfedilen yalnızca VMware VM'leri için önizlemededir.
> Özellikler sınırlı veya eksik olabilir.
> Bu önizleme müşteri desteği kapsamındadır ve üretim iş yükleri için kullanılabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.



## <a name="before-you-start"></a>Başlamadan önce

- Aracısız bağımlılık çözümlemesi [hakkında bilgi edinin.](concepts-dependency-visualization.md#agentless-analysis)
- VMware VM'ler için aracısız bağımlılık görselleştirmesi ayarlamaya yönelik ön koşulları ve destek gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements)
- Bir Azure Geçiş projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
- Bir proje zaten oluşturduysanız, Azure Geçir:Sunucu Değerlendirmesi aracını [eklediğinizden](how-to-assess.md) emin olun.
- Şirket içi makinelerinizi keşfetmek için bir [Azure Geçiş cihazı](migrate-appliance.md) ayarladığınızdan emin olun. [VMware VM'ler](how-to-set-up-appliance-vmware.md) için nasıl bir cihaz ayarlayamaize devam edin. Cihaz şirket içi makineleri keşfeder ve Azure Geçiş:Sunucu Değerlendirmesi'ne meta veri ve performans verileri gönderir.


## <a name="current-limitations"></a>Geçerli sınırlamalar

- Şu anda bağımlılık çözümlemesi görünümünde bir sunucuyu gruptan ekleyip kaldıramam.
- Bir sunucu grubu için bağımlılık haritası şu anda kullanılamıyor.
- Şu anda, bağımlılık verileri tabular biçiminde indirilenemiyor.

## <a name="create-a-user-account-for-discovery"></a>Keşif için bir kullanıcı hesabı oluşturma

Sunucu Değerlendirmesi'nin keşif için VM'ye erişebileceği bir kullanıcı hesabı ayarlayın. Hesap gereksinimleri hakkında [bilgi edinin.](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements)


## <a name="add-the-user-account-to-the-appliance"></a>Kullanıcı hesabını cihaza ekleme

Kullanıcı hesabını cihaza ekleyin.

1. Cihaz yönetimi uygulamasını açın. 
2. **VCenter ayrıntıları sağla** paneline gidin.
3. **VM'lere uygulama ve bağımlılıkları keşfedin,** **kimlik bilgileri ekle'yi** tıklatın
3. **İşletim sistemini**seçin, hesap için uygun bir ad sağlayın ve **Kullanıcı adı**/**Parolası**
6. **Kaydet**'e tıklayın.
7. **Kaydet'i tıklatın ve keşfetmeyi başlatın.**

    ![VM kullanıcı hesabı ekleme](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Bağımlılık bulmayı başlatma

Bağımlılık bulmayı etkinleştirmek istediğiniz makineleri seçin.

1. **Azure Geçir: Sunucu**Değerlendirmesi'nde, **Keşfedilen sunucular'ı**tıklatın.
2. Bağımlılık **çözümlemesi** simgesini tıklatın.
3. **Sunucu ekle'yi**tıklatın.
3. **Sunucuekle** sayfasında, ilgili makineleri keşfeden cihazı seçin.
4. Makine listesinden makineleri seçin.
5. **Sunucu ekle'yi**tıklatın.

    ![Bağımlılık bulmayı başlatma](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Bağımlılık keşfine başladıktan yaklaşık altı saat sonra bağımlılıkları görselleştirebilirsiniz.

## <a name="visualize-dependencies"></a>Bağımlılıkları görselleştirin

1. **Azure Geçir: Sunucu**Değerlendirmesi'nde, **Keşfedilen sunucular'ı**tıklatın.
2. Görüntülemek istediğiniz makineyi arayın.
3. **Bağımlılıklar** sütununda, **bağımlılıkları görüntüle'yi** tıklatın
4. **Zaman süresi** açılır düşüşünü kullanarak haritayı görüntülemek istediğiniz süreyi değiştirin.
5. **İstemci** grubunu, seçili makineye bağımlı makineleri listelemek için genişletin.
6. Seçili makineden bağımlı olan makineleri listelemek için **Bağlantı Noktası** grubunu genişletin.
7. Bağımlı makinelerden herhangi birinin harita görünümüne gitmek için, makine adı > **Yükle sunucu haritasına** tıklayın

    ![Sunucu bağlantı noktası grubunu genişletin ve sunucu haritasını yükleyin](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![İstemci grubunu genişletme ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Her bağımlılık için işlem düzeyi ayrıntılarını görüntülemek için seçili makineyi genişletin.

    ![İşlemleri göstermek için sunucuya genişletme](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> Bağımlılık için işlem bilgileri her zaman kullanılabilir değildir. Kullanılabilir değilse, bağımlılık "Bilinmeyen işlem" olarak işaretlenmiş işlemle betimlenir.

## <a name="stop-dependency-discovery"></a>Bağımlılık bulmayı durdurma

Bağımlılık keşfini durdurmak istediğiniz makineleri seçin.

1. **Azure Geçir: Sunucu**Değerlendirmesi'nde, **Keşfedilen sunucular'ı**tıklatın.
2. Bağımlılık **çözümlemesi** simgesini tıklatın.
3. **Sunucuları Kaldır'ı**tıklatın.
3. **Sunucuları Kaldır** sayfasında, bağımlılık keşfini durdurmak için baktığınız VM'leri keşfeden **cihazı** seçin.
4. Makine listesinden makineleri seçin.
5. **Sunucuları Kaldır'ı**tıklatın.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirme için [makineleri gruplandırma.](how-to-create-a-group.md)
