---
title: Azure 'daki grup makinelerini aracısız bağımlılık görselleştirmesini kullanarak geçirme
description: Makine bağımlılıklarını aracısız bir şekilde kullanarak grupların nasıl oluşturulacağını açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/18/2019
ms.author: hamusa
ms.openlocfilehash: 4bc2e67d276deeea01989ed76be7352ce27abd71
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2020
ms.locfileid: "76280824"
---
# <a name="set-up-agentless-dependency-visualization-for-assessment"></a>Değerlendirme için aracısız bağımlılık görselleştirmesini ayarlama

Bu makalede, Azure geçişi: Sunucu değerlendirmesi ' nde aracısız bağımlılık eşlemesinin nasıl ayarlanacağı açıklanır. 

> [!IMPORTANT]
> Aracısız bağımlılık görselleştirmesi Şu anda Azure geçiş gereci kullanılarak bulunan Azure VMware VM 'Leri için önizleme aşamasındadır.
> Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Bu önizleme müşteri desteği kapsamında ele alınmıştır ve üretim iş yükleri için kullanılabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="about-dependency-mapping"></a>Bağımlılık eşleme hakkında

Bağımlılık eşleme, değerlendirmek ve geçirmek istediğiniz makineler arasında bağımlılıkları görselleştirmenize yardımcı olur. Daha yüksek düzeyde güvenle makineler değerlendirmek istediğinizde genellikle bağımlılık eşlemesini kullanırsınız.

- Azure geçişi: Sunucu değerlendirmesi ' nde, makineleri değerlendirme için gruplar halinde toplayın. Gruplar genellikle birlikte geçirmek istediğiniz makinelerden oluşur ve bağımlılık eşleme, makineleri doğru bir şekilde gruplandırabilmeniz için makine bağımlılıklarını çapraz denetlemenize yardımcı olur.
- Eşleme kullanarak, birlikte geçirilmesi gereken bağımlı sistemleri bulabilirsiniz. Ayrıca, çalışan bir sistemin hala kullanıcılara hizmet verip vermediğini veya geçiş yerine yetki alma için bir aday olduğunu belirleyebilirsiniz.
- Bağımlılıkların görselleştirilmesi, hiçbir şeyin geri ayrılmaması ve geçiş sırasında beklenmedik kesintilerden kaçınılması sağlar.

## <a name="about-agentless-visualization"></a>Aracısız görselleştirme hakkında

Aracısız bağımlılık görselleştirmesi, makinelere herhangi bir aracı yüklemenizi gerektirmez. Etkin olduğu makinelerden gelen TCP bağlantısı verilerini yakalayıp işe yarar.

- Bağımlılık keşfi başlatıldıktan sonra, Gereç, beş dakikalık yoklama aralığındaki makinelerden veri toplar.
- Aşağıdaki veriler toplanır:
    - TCP bağlantıları
    - Etkin bağlantıları olan işlemlerin adları
    - Yukarıdaki işlemlerin çalıştırıldığı yüklü uygulamaların adları
    - Hayır. her yoklama aralığında algılanan bağlantıların sayısı

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Aracısız bağımlılık görselleştirmesi Şu anda yalnızca VMware VM 'Leri için kullanılabilir.
- Artık, bağımlılık analizi görünümünde bir gruba sunucu ekleyemez veya gruptan sunucu kaldıramazsınız.
- Bir sunucu grubu için bağımlılık eşlemesi Şu anda kullanılamıyor.
- Şu anda, bağımlılık verileri tablolu biçimde indirilemez.

## <a name="before-you-start"></a>Başlamadan önce

- Bir Azure geçişi projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
- Aracısız bağımlılık Analizi Şu anda yalnızca VMware makineleri için kullanılabilir.
- Zaten bir proje oluşturduysanız Azure geçişi: Sunucu değerlendirmesi [aracını eklediğinizden emin](how-to-assess.md) olun.
- Azure geçişi 'nde VMware makinelerinizi keşfetdiğinizden emin olun; Bunu, [VMware](how-to-set-up-appliance-vmware.md)Için bir Azure geçiş gereci ayarlayarak yapabilirsiniz. Gereç, şirket içi makineleri bulur ve Azure geçişi: Sunucu değerlendirmesi ' ne meta veri ve performans verileri gönderir. [Daha fazla bilgi edinin](migrate-appliance.md).
- Aracısız bağımlılık görselleştirmesini ayarlama [gereksinimlerini gözden geçirin](migrate-support-matrix-vmware.md#agentless-dependency-visualization) .



## <a name="create-a-user-account-for-discovery"></a>Bulma için bir kullanıcı hesabı oluşturma

Sunucu değerlendirmesinin, bulma için VM 'ye erişebilmesi için gerekli izinlere sahip bir kullanıcı hesabı ayarlayın. Bir kullanıcı hesabı belirtebilirsiniz.

- **Windows VM 'Lerde gerekli izin**: Kullanıcı hesabı ' Konuk ' erişimi gerektirir.
- **Linux VM 'Lerde gerekli izin**: hesapta kök ayrıcalığı gereklidir. Alternatif olarak, Kullanıcı hesabı/bin/netstat ve/bin/ls dosyalarında şu iki özelliği gerektirir: CAP_DAC_READ_SEARCH ve CAP_SYS_PTRACE.

## <a name="add-the-user-account-to-the-appliance"></a>Kullanıcı hesabını gereç 'e ekleme

Kullanıcı hesabını gereç öğesine eklemeniz gerekir.

Hesabı aşağıdaki gibi ekleyin:

1. Gereç Yönetimi uygulamasını açın. **VCenter ayrıntıları sağla** paneline gidin.
2. **VM 'lerde uygulama ve bağımlılıkları bul** bölümünde **kimlik bilgileri ekle** ' ye tıklayın.
3. **İşletim sistemini**seçin.
4. Hesap için bir kolay ad belirtin.
5. **Kullanıcı adı** ve **parola** belirtin
6. **Save (Kaydet)** düğmesine tıklayın.
7. **Kaydet ve bulmayı Başlat**' a tıklayın.

    ![VM Kullanıcı hesabı ekle](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Bağımlılık bulmayı Başlat

Üzerinde bağımlılık bulmayı etkinleştirmek istediğiniz makineleri seçin.

1. **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular**' a tıklayın.
2. **Bağımlılık Analizi** simgesine tıklayın.
3. **Bağımlılık bulmayı Başlat**' a tıklayın.
3. **Bağımlılık bulmayı Başlat** sayfasında, ilgili makineleri bulan gereci seçin.
4. Makine listesinden makineleri seçin.
5. **Bağımlılık bulmayı Başlat**' a tıklayın.

    ![Bağımlılık bulmayı Başlat](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Bağımlılık bulmayı başlattıktan sonra 6 saat bağımlılıklarını görselleştirebileceksiniz.

## <a name="visualize-dependencies"></a>Bağımlılıkları görselleştirin

1. **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular**' a tıklayın.
2. Bağımlılık eşlemesini görüntülemek istediğiniz makineyi arayın.
3. **Bağımlılıklar** sütununda **bağımlılıkları görüntüle** ' ye tıklayın.
4. **Zaman süresi** açılan listesini kullanarak Haritayı görüntülemek istediğiniz zaman aralığını değiştirin.
5. Seçili makinede bağımlılığı olan makineleri listelemek için **istemci** grubunu genişletin.
6. Seçili makineden bağımlılığı olan makineleri listelemek için **bağlantı noktası** grubunu genişletin.
7. Bağımlı makinelerin herhangi birinin harita görünümüne gitmek için makine adına tıklayın ve ardından **sunucu haritasını yükle** ' ye tıklayın.

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
3. **Bağımlılık bulmayı durdur**' a tıklayın.
3. **Bağımlılık bulmayı durdur** **sayfasında, bağımlılık** bulmayı durdurmak Için bakaplanladığınız VM 'leri bulan gereci seçin.
4. Makine listesinden makineleri seçin.
5. **Bağımlılık bulmayı durdur** ' a tıklayın


## <a name="next-steps"></a>Sonraki adımlar

[Makineleri gruplandırın](how-to-create-a-group.md)
