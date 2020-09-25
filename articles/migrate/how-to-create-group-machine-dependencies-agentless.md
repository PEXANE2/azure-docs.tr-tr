---
title: Azure geçişi sunucu değerlendirmesi ' nde aracısız bağımlılık analizini ayarlama
description: Azure geçişi sunucu değerlendirmesi ' nde aracısız bağımlılık analizini ayarlayın.
ms.topic: how-to
ms.date: 6/08/2020
ms.openlocfilehash: 164cc20632faa1d444d06da6688000e9b40d7e76
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91275600"
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
- Bağımlılık verileri toplama, 1000 sunucuları için aynı anda ayarlanabilir. 1000 toplu işler halinde sıralama yaparak daha yüksek sayıda sunucu çözümleyebilirsiniz.

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

Bağımlılık bulmayı başlattıktan sonra altı saat etrafında bağımlılıkları görselleştirebilirsiniz. Çeşitli makineleri etkinleştirmek istiyorsanız, [PowerShell](#start-or-stop-dependency-discovery-using-powershell) 'i kullanarak bunu yapabilirsiniz.

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

Birden çok makinede bağımlılığı durdurmak istiyorsanız, [PowerShell](#start-or-stop-dependency-discovery-using-powershell) 'i kullanarak bunu yapabilirsiniz.


### <a name="start-or-stop-dependency-discovery-using-powershell"></a>PowerShell kullanarak bağımlılık bulmayı başlatma veya durdurma

GitHub 'daki [Azure PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/dependencies-at-scale) deposundan PowerShell modülünü indirin.


#### <a name="log-in-to-azure"></a>Azure'da oturum açma

1. Connect-AzAccount cmdlet 'ini kullanarak Azure aboneliğinizde oturum açın.

    ```PowerShell
    Connect-AzAccount
    ```
    Azure Kamu kullanıyorsanız, aşağıdaki komutu kullanın.
    ```PowerShell
    Connect-AzAccount -EnvironmentName AzureUSGovernment
    ```

2. Azure geçişi projesini oluşturduğunuz aboneliği seçin 

    ```PowerShell
    select-azsubscription -subscription "Fabrikam Demo Subscription"
    ```

3. İndirilen AzMig_Dependencies PowerShell modülünü içeri aktarma

    ```PowerShell
    Import-Module .\AzMig_Dependencies.psm1
    ```

#### <a name="enable-or-disable-dependency-data-collection"></a>Bağımlılık veri toplamayı etkinleştir veya devre dışı bırak

1. Aşağıdaki komutları kullanarak, Azure geçişi projenizde bulunan VMware VM 'lerinin listesini alın. Aşağıdaki örnekte, proje adı FabrikamDemoProject ve ait olduğu kaynak grubu FabrikamDemoRG. Makinelerin listesi FabrikamDemo_VMs.csv ' de kaydedilecek

    ```PowerShell
    Get-AzMigDiscoveredVMwareVMs -ResourceGroupName "FabrikamDemoRG" -ProjectName "FabrikamDemoProject" -OutputCsvFile "FabrikamDemo_VMs.csv"
    ```

    Dosyasında, VM görünen adını, bağımlılık koleksiyonunun geçerli durumunu ve bulunan tüm VM 'lerin ARM KIMLIĞINI görebilirsiniz. 

2. Bağımlılıkları etkinleştirmek veya devre dışı bırakmak için bir giriş CSV dosyası oluşturun. Dosyanın "ARM KIMLIĞI" üst bilgisine sahip bir sütunu olması gerekir. CSV dosyasındaki ek üstbilgiler yok sayılır. Önceki adımda oluşturulan dosyayı kullanarak CSV oluşturabilirsiniz. Bağımlılıkları etkinleştirmek veya devre dışı bırakmak istediğiniz VM 'Leri koruyarak dosyanın bir kopyasını oluşturun. 

    Aşağıdaki örnekte, FabrikamDemo_VMs_Enable.csv giriş dosyasındaki VM 'Ler listesinde bağımlılık Analizi etkinleştiriliyor.

    ```PowerShell
    Set-AzMigDependencyMappingAgentless -InputCsvFile .\FabrikamDemo_VMs_Enable.csv -Enable
    ```

    Aşağıdaki örnekte, FabrikamDemo_VMs_Disable.csv giriş dosyasındaki VM 'Ler listesinde bağımlılık Analizi devre dışı bırakılıyor.

    ```PowerShell
    Set-AzMigDependencyMappingAgentless -InputCsvFile .\FabrikamDemo_VMs_Disable.csv -Disable
    ```

## <a name="visualize-network-connections-in-power-bi"></a>Power BI 'de ağ bağlantılarını görselleştirin

Azure geçişi, aynı anda birçok sunucunun ağ bağlantılarını görselleştirmek ve işlem ve sunucuya göre filtrelemek için kullanabileceğiniz bir Power BI şablonu sunar. Görselleştirmek için, aşağıdaki yönergelere göre Power BI bağımlılık verileriyle yükleyin.

1. GitHub 'daki [Azure PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/dependencies-at-scale) deposundan PowerShell modülünü ve Power BI şablonunu indirin.

2. Aşağıdaki yönergeleri kullanarak Azure 'da oturum açın: 
- Connect-AzAccount cmdlet 'ini kullanarak Azure aboneliğinizde oturum açın.

    ```PowerShell
    Connect-AzAccount
    ```

- Azure Kamu kullanıyorsanız, aşağıdaki komutu kullanın.

    ```PowerShell
    Connect-AzAccount -EnvironmentName AzureUSGovernment
    ```

- Azure geçişi projesini oluşturduğunuz aboneliği seçin 

    ```PowerShell
    select-azsubscription -subscription "Fabrikam Demo Subscription"
    ```

3. İndirilen AzMig_Dependencies PowerShell modülünü içeri aktarma

    ```PowerShell
    Import-Module .\AzMig_Dependencies.psm1
    ```

4. Aşağıdaki komutu çalıştırın. Bu komut, bir CSV 'deki bağımlılıklar verilerini indirir ve Power BI görselleştirme için kullanılabilecek benzersiz bağımlılıkların bir listesini oluşturmak üzere işler. Aşağıdaki örnekte, proje adı FabrikamDemoProject ve ait olduğu kaynak grubu FabrikamDemoRG. Bağımlılıklar, FabrikamAppliance tarafından bulunan makineler için indirilir. Benzersiz bağımlılıklar FabrikamDemo_Dependencies.csv kaydedilir

    ```PowerShell
    Get-AzMigDependenciesAgentless -ResourceGroup FabrikamDemoRG -Appliance FabrikamAppliance -ProjectName FabrikamDemoProject -OutputCsvFile "FabrikamDemo_Dependencies.csv"
    ```

5. İndirilen Power BI şablonunu açın

6. Power BI indirilen bağımlılık verilerini yükleyin.
    - Şablonu Power BI açın.
    - Araç çubuğundan **veri al** ' a tıklayın. 
    - Ortak veri kaynaklarından **metin/CSV** ' yi seçin.
    - İndirilen bağımlılıklar dosyasını seçin.
    - **Yükle**' ye tıklayın.
    - CSV dosyasının adıyla bir tablo alındığını görürsünüz. Tabloyu sağ taraftaki alanlar çubuğunda görebilirsiniz. AzMig_Dependencies olarak yeniden adlandırın
    - Araç çubuğundan Yenile ' ye tıklayın.

    Ağ bağlantıları grafiği ve kaynak sunucu adı, hedef sunucu adı, kaynak işlem adı, hedef işlem adı Dilimleyicileri içeri aktarılan verilerle birlikte çalışır olmalıdır.

7. Sunucu ve işlemlere göre filtreleme ağ bağlantılarının haritasını görselleştirin. Dosyanızı kaydedin.


## <a name="next-steps"></a>Sonraki adımlar

[Makineleri](how-to-create-a-group.md) değerlendirme için gruplandırın.
