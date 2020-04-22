---
title: Azure İlkesi'ni kullanarak VM'ler için Azure Monitörünü etkinleştirme
description: Bu makalede, Azure İlkesi'ni kullanarak birden çok Azure sanal makine veya sanal makine ölçeği kümeleri için Sanal Makineler için Azure Monitor'u nasıl etkinleştirdiğiniz açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 73c18d45136eea90ad29dc1bd40c4539dddc0ee6
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767261"
---
# <a name="enable-azure-monitor-for-vms-by-using-azure-policy"></a>Azure İlkesi'ni kullanarak VM'ler için Azure Monitörünü etkinleştirme

Bu makalede, Azure İlkesi'ni kullanarak Azure sanal makineler veya sanal makine ölçek kümeleri için Sanal Ayarlar için Azure Monitor'un nasıl etkinleştirilen olduğu açıklanmaktadır. Bu işlemin sonunda, Log Analytics ve Bağımlılık aracılarını etkinleştirerek başarılı bir şekilde yapılandırmış ve uyumlu olmayan sanal makineleri tanımlamış olursunuz.

Tüm Azure sanal makineleriniz veya sanal makine ölçek kümeleriniz için Azure Monitor'u keşfetmek, yönetmek ve etkinleştirmek için Azure İlkesi veya Azure PowerShell'i kullanabilirsiniz. Azure İlkesi, tutarlı uyumluluk ve yeni sağlanan VM'lerin otomatik olarak etkinleştirilmesini sağlamak için aboneliklerinizi etkili bir şekilde yönetmek için ilke tanımlarını yönetebildiğiniz için önerdiğimiz yöntemdir. Bu ilke tanımları:

* Log Analytics aracısını ve Bağımlılık aracısını dağıtın.
* Uyumluluk sonuçlarını rapor edin.
* Uyumlu olmayan VM'ler için düzeltin.

Bu görevleri Azure PowerShell veya Azure Kaynak Yöneticisi şablonuyla gerçekleştirmek istiyorsanız, [Azure PowerShell veya Azure Kaynak Yöneticisi şablonlarını kullanarak VM'ler için Azure Monitörünü Etkinleştir'e](vminsights-enable-at-scale-powershell.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
Azure Sanal M'lerinizde ve sanal makine ölçek kümelerinde VM'ler için Azure İzleme'ye dahil etmek için İlke'yi kullanmadan önce, izleme verilerinizi depolamak için kullanacağınız çalışma alanında VMInsights çözümünü etkinleştirmeniz gerekir. Bu görev, **Diğer biniş seçenekleri** sekmesinde Azure Monitor'daki **Başlat'tan** tamamlanabilir.  Yapılandırılacak çalışma alanını seçmenizi gerektiren **bir çalışma alanını yapılandır'ı**seçin.

![Çalışma alanını yapılandırma](media/vminsights-enable-at-scale-policy/configure-workspace.png)

Ayrıca, **ilkeyi kullanarak Etkinleştir'i** seçerek çalışma alanınızı yapılandırabilir ve ardından Çalışma Alanı araç çubuğunu **Yapılandır'ı** seçebilirsiniz.  Bu, seçilen çalışma alanına VMInsights çözümünü yükler ve bu da çalışma alanının, Politika'yı kullanarak etkinleştirdiğiniz VM'ler ve sanal makine ölçeği kümeleri tarafından gönderilen izleme verilerini depolamasını sağlar. 

![İlkeyi kullanarak etkinleştirme](media/vminsights-enable-at-scale-policy/enable-using-policy.png)

## <a name="manage-policy-coverage-feature-overview"></a>İlke Kapsamı özelliğine genel bakışı yönet

VM İlke Kapsamı için Azure Monitor, daha önce bahsedilen ilke tanımlarını içeren **VM'ler için Azure Monitörü Etkinleştir** girişimini keşfetmeyi, yönetmeyi ve ölçeklendirmeyi kolaylaştırır. Bu özelliğe erişmek için, VM'ler için Azure Monitörü'nde **Başlat sekmesinden** **Diğer biniş seçeneklerini** seçin. **VM İlkesi Kapsamı** için Azure Monitörünü açmak için **İlke Kapsamını Yönet'i** seçin.

![VM'lerden Azure Monitör başlat sekmesi](./media/vminsights-enable-at-scale-policy/get-started-page.png)

Buradan, yönetim gruplarınız ve abonelikleriniz arasında girişimin kapsamını kontrol edebilir ve yönetebilirsiniz. Yönetim gruplarının ve aboneliklerin her birinde kaç VM bulunduğunu ve uyumluluk durumlarını anlayabilirsiniz.

![VM'ler için Azure Monitörü İlkeyi Yönet sayfası](media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Bu bilgiler, Azure Monitor için yönetim senaryonuzu tek bir merkezi konumdan planlamanıza ve yürütmenize yardımcı olmak için yararlıdır. Azure İlkesi bir ilke veya girişim bir kapsama atandığında bir uyumluluk görünümü sağlarken, bu yeni sayfayla ilkenin veya girişimin nerede atanmadığını bulabilir ve yerine atayabilirsiniz. Atama, görüntüleme ve değiştirme gibi tüm eylemler doğrudan Azure İlkesi'ne yönlendirilir. **VM İlke Kapsamı için Azure Monitörü,** yalnızca **VM'ler için Azure Monitörünü Etkinleştir**girişimi için genişletilmiş ve entegre bir deneyimdir.

Bu sayfadan, VM'ler için Azure Monitörü için Log Analytics çalışma alanınızı yapılandırabilirsiniz:

- Hizmet Haritası çözümlerini yükler.
- Performans grafikleri, çalışma kitapları ve özel günlük sorgularınız ve uyarılarınız tarafından kullanılan işletim sistemi performans sayaçlarını etkinleştirin.

![VM'ler için Azure Monitörçalışma alanını yapılandırma](media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Bu seçenek, herhangi bir ilke eylemiyle ilgili değildir. VM'ler için Azure Monitör'ü etkinleştirmek için gereken [ön koşulları](vminsights-enable-overview.md) karşılamak için kolay bir yol sağlayabilir.  

### <a name="what-information-is-available-on-this-page"></a>Bu sayfada hangi bilgiler mevcuttur?

Aşağıdaki tablo, ilke kapsamı sayfasında sunulan bilgilerin dökümünü ve nasıl yorumlanacağıdır.

| İşlev | Açıklama | 
|----------|-------------| 
| **Kapsam** | Yönetim grubu hiyerarşisi aracılığıyla ayrıntıya girebilme özelliğine sahip olduğunuz veya erişiminiz olan yönetim grubu ve abonelikler.|
| **Rol** | Okuyucu, sahip veya katkıda bulunan kapsamdaki rolünüz. Bazı durumlarda, aboneliğiniz olabileceğini, ancak ait olduğu yönetim grubuna erişmediğini belirtmek için boş görünebilir. Diğer sütunlarda yer alan bilgiler rolünüze bağlı olarak değişir. Bu rol, hangi verileri görebileceğinizi ve ilkeleri veya girişimleri atama (sahip) atama, bunları düzenleme veya uyumluluğu görüntüleme açısından gerçekleştirebileceğiniz eylemleri belirlemede önemlidir. |
| **Toplam VM** | Bu kapsamdaki VM sayısı. Bir yönetim grubu için, abonelikler veya alt yönetim grubu altında iç içe olan VM'lerin toplamıdır. |
| **Atama Kapsamı** | İlke veya girişim kapsamındaki VM'lerin yüzdesi. |
| **Atama Durumu** | İlkenizin veya girişim atamanızın durumu hakkında bilgi. |
| **Uyumlu VM'ler** | İlke veya girişim altında uyumlu vm sayısı. **VM'ler için Azure Monitörünü Etkinleştir**girişimi için bu, hem Log Analytics aracısı hem de Bağımlılık aracısı olan VM sayısıdır. Bazı durumlarda, atama, VM'ler veya yeterli izinler nedeniyle boş görünebilir. Bilgiler Uyumluluk **Durumu**altında sağlanmaktadır. |
| **Uyumluluk** | Genel uyumluluk numarası, uyumlu tüm farklı kaynakların toplamına bölünen ayrı kaynakların toplamıdır. |
| **Uyumluluk Durumu** | İlke veya girişim atamanız için uyumluluk durumu hakkında bilgi.|

İlke yi veya girişimi atadığınızda, atamada seçilen kapsam listelenen kapsam veya alt kümesi olabilir. Örneğin, bir yönetim grubu (kapsam kapsamı) değil, bir abonelik (ilke kapsamı) için bir atama oluşturmuş olabilirsiniz. Bu durumda, Atama **Kapsamı** değeri, kapsama kapsamındaKi VM'ler tarafından bölünmüş ilke veya girişim kapsamındaki VM'leri gösterir. Başka bir durumda, bazı VM'leri, kaynak gruplarını veya aboneliği ilke kapsamından dışlamış olabilirsiniz. Değer boşsa, ilke veya girişim inancın bulunmadığını veya izniniz olmadığını gösterir. Bilgiler Atama **Durumu**altında sağlanır.

## <a name="enable-by-using-azure-policy"></a>Azure İlkesi'ni kullanarak etkinleştirme

Kiracınızda Azure İlkesi'ni kullanarak VM'ler için Azure Monitörünü etkinleştirmek için:

- Girişimi bir kapsama atayın: yönetim grubu, abonelik veya kaynak grubu.
- Uyumluluk sonuçlarını gözden geçirin ve düzeltin.

Azure İlkesi atama hakkında daha fazla bilgi için [Azure İlkesi'ne genel bakış'a](../../governance/policy/overview.md#assignments) bakın ve devam etmeden önce [yönetim gruplarına genel bakışı](../../governance/management-groups/overview.md) inceleyin.

### <a name="policies-for-azure-vms"></a>Azure VM'leri için İlkeler

Azure VM'nin ilke tanımları aşağıdaki tabloda listelenmiştir.

|Adı |Açıklama |Tür |
|-----|------------|-----|
|VM'ler için Azure Monitörünü etkinleştirme |Belirtilen kapsamdaki sanal makineler (yönetim grubu, abonelik veya kaynak grubu) için Azure Monitörünü etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. |Girişimi |
|Denetim Bağımlılık aracısı dağıtımı – VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklenmiyorsa VM'leri uyumlu olmayan olarak raporlar. |İlke |
|Denetim Günlüğü Analytics aracısı dağıtımı – VM görüntüsü (OS) listedışı |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklenmiyorsa VM'leri uyumlu olmayan olarak raporlar. |İlke |
|Linux VM'leri için Bağımlılık aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse Linux VM'leri için Bağımlılık aracısını dağıtın. |İlke |
|Windows VM'leri için Bağımlılık aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows VM'leri için Bağımlılık aracısını dağıtın. |İlke |
|Linux VM'leri için Log Analytics aracısı dağıtın |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux VM'leri için Log Analytics aracısını dağıtın. |İlke |
|Windows VM'ler için Günlük Analizi aracısı dağıtma |Listede VM görüntüsü (OS) tanımlanmışsa ve aracı yüklü değilse, Windows VM'leri için Günlük Analizi aracısını dağıtın. |İlke |

### <a name="policies-for-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçeği ilkeleri kümeleri

Azure sanal makine ölçeği kümesinin ilke tanımları aşağıdaki tabloda listelenmiştir.

|Adı |Açıklama |Tür |
|-----|------------|-----|
|Sanal makine ölçek kümeleri için Azure Monitörünü etkinleştirme |Belirtilen kapsamdaki sanal makine ölçeği kümeleri (yönetim grubu, abonelik veya kaynak grubu) için Azure Monitörünü etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. Not: Ölçek ayarlama yükseltme ilkeniz El Ile ayarlanmışsa, uzantıyı, üzerlerine yükseltme çağrısında bulunarak dizideki tüm VM'lere uygulayın. CLI'de, bu. `az vmss update-instances` |Girişimi |
|Sanal makine ölçeği kümelerinde Denetim Bağımlılık aracısı dağıtımı – VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklenmiyorsa, sanal makine ölçeğini uyumlu olmayan olarak ayarlar. |İlke |
|Sanal makine ölçeği kümelerinde Denetim Günlüğü Analytics aracı dağıtımı – VM görüntüsü (OS) listelenmemiş |VM görüntüsü (OS) listede tanımlanmamışsa ve aracı yüklenmiyorsa, sanal makine ölçeğini uyumlu olmayan olarak ayarlar. |İlke |
|Linux sanal makine ölçek kümeleri için Bağımlılık aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux sanal makine ölçeği için Bağımlılık aracısını dağıtın. |İlke |
|Windows sanal makine ölçek kümeleri için Bağımlılık aracısı dağıtma |VM görüntüsü (OS) listede tanımlanmışsa ve aracı yüklü değilse, Windows sanal makine ölçeği için Bağımlılık aracısını dağıtın. |İlke |
|Linux sanal makine ölçek setleri için Log Analytics aracısı dağıtın |VM Image (OS) listede tanımlanmışsa ve aracı yüklü değilse, Linux sanal makine ölçeği için Log Analytics aracısını dağıtın. |İlke |
|Windows sanal makine ölçek kümeleri için Log Analytics aracısı dağıtma |Listede VM görüntüsü (OS) tanımlanmışsa ve aracı yüklenmezse, Windows sanal makine ölçeği için Log Analytics aracısını dağıtın. |İlke |

Bağımsız politika (girişime dahil değildir) burada açıklanmıştır:

|Adı |Açıklama |Tür |
|-----|------------|-----|
|VM için Denetim Günlüğü Analizi çalışma alanı – Rapor uyuşmazlığı |İlke veya girişim atamasında belirtilen Log Analytics çalışma alanına günlüğe kaydetmiyorlarsa, VM'leri uyumsuz olarak bildirin. |İlke |

### <a name="assign-the-azure-monitor-initiative"></a>Azure Monitörü girişimini atama

**VM İlkesi Kapsamı** için Azure Monitörü sayfasından ilke ataması oluşturmak için aşağıdaki adımları izleyin. Bu adımları nasıl tamamlayacağımı anlamak için Azure [portalından bir ilke ataması oluşturma'ya](../../governance/policy/assign-policy-portal.md)bakın.

İlke yi veya girişimi atadığınızda, atamada seçilen kapsam burada listelenen kapsam veya bunun bir alt kümesi olabilir. Örneğin, yönetim grubu (kapsam kapsamı) için değil, abonelik (ilke kapsamı) için bir atama oluşturmuş olabilirsiniz. Bu durumda, kapsama yüzdesi, kapsama kapsamındaKi VM'ler tarafından bölünmüş ilke veya girişim kapsamındaki VM'leri gösterir. Başka bir durumda, bazı VM'leri veya kaynak gruplarını veya bir aboneliği ilke kapsamından dışlamış olabilirsiniz. Boşsa, ilke veya girişim inancın bulunmadığını veya izinlerin olmadığını gösterir. Bilgiler Atama **Durumu**altında sağlanır.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Azure portalında **Monitör'ü**seçin. 

3. **Öngörüler** bölümünde **Sanal Makineler'i** seçin.
 
4. Başlat **sekmesini** seçin. Sayfada, **İlke Kapsamını Yönet'i**seçin.

5. Tablodan bir yönetim grubu veya abonelik seçin. Elipsleri seçerek **Kapsam'ı** seçin (...). Örnekte, bir kapsam ilke atamasını zorlama için sanal makinelerin gruplandırmasına sınırlar.

6. Azure **İlkesi atama** sayfasında, **VM'ler için Azure Monitörünü Etkinleştir**girişimiyle önceden doldurulur. 
    **Atama adı** kutusu otomatik olarak girişim adıyla doldurulur, ancak değiştirebilirsiniz. İsteğe bağlı bir açıklama da ekleyebilirsiniz. **Atanan** kutu, oturum açanların kim olduğuna bağlı olarak otomatik olarak doldurulur. Bu değer isteğe bağlıdır.

7. (İsteğe bağlı) Bir veya daha fazla kaynağı kapsamdan kaldırmak için **Dışlamalar'ı**seçin.

8. Desteklenen bölge için **Günlük Analitiği çalışma alanı** açılır listesinde bir çalışma alanı seçin.

   > [!NOTE]
   > Çalışma alanı atamakapsamının dışındaysa, *Giriş Analizi Katılımcısı* izinlerini ilke atamasının Asıl Kimliği'ne ver. Bunu yapmazsanız, erişim vermek gibi bir dağıtım `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` hatası görebilirsiniz, [yönetilen kimliği el ile yapılandırmak için nasıl](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity)gözden geçirin.
   > 
   >  **Yönetilen Kimlik** onay kutusu seçilir, çünkü atanan girişim *deployIfNotExists* efektine sahip bir ilke içerir.
    
9. Kimlik konumu açılır listesini **yönet'** de uygun bölgeyi seçin.

10. **Ata**'yı seçin.

Atamayı oluşturduktan sonra, **VM İlke Kapsamı** için Azure Monitörü sayfası **Atamaların Kapsamını, Atama** **Durumunu,** **Uyumlu VM'leri**ve Uyumluluk **Durumunu** değişiklikleri yansıtacak şekilde güncelleştirir. 

Aşağıdaki matris, girişim için olası her uyumluluk durumunu eşler.  

| Uyumluluk durumu | Açıklama | 
|------------------|-------------|
| **Uyumlu** | Kapsamdaki tüm VM'ler, kendilerine dağıtılan Log Analytics ve Bağımlılık aracılarına sahiptir.|
| **Uyumlu Değil** | Kapsamdaki tüm VM'ler, log analitiği ve bağımlılık aracılarına dağıtılmış değildir ve düzeltme gerektirebilir.|
| **Başlamadı** | Yeni bir atama eklendi. |
| **Kilit** | Yönetim grubuna yeterli ayrıcalığınız yok. <sup>1.1.2</sup> | 
| **Boş** | Hiçbir ilke atanmadı. | 

<sup>1</sup> Yönetim grubuna erişiminiz yoksa, bir sahipten erişim sağlamasını isteyin. Veya, alt yönetim grupları veya abonelikler aracılığıyla uyumluluğu görüntüleyin ve atamaları yönetin. 

Aşağıdaki tablo, girişim için olası her atama durumunu eşler.

| Atama durumu | Açıklama | 
|------------------|-------------|
| **Başarı** | Kapsamdaki tüm VM'ler, kendilerine dağıtılan Log Analytics ve Bağımlılık aracılarına sahiptir.|
| **Uyarı** | Abonelik bir yönetim grubu altında değildir.|
| **Başlamadı** | Yeni bir atama eklendi. |
| **Kilit** | Yönetim grubuna yeterli ayrıcalığınız yok. <sup>1.1.2</sup> | 
| **Boş** | VM'ler yok veya bir ilke atanmıyor. | 
| **Eylem** | Bir ilke atayın veya bir atamayı edin. | 

<sup>1</sup> Yönetim grubuna erişiminiz yoksa, bir sahipten erişim sağlamasını isteyin. Veya, alt yönetim grupları veya abonelikler aracılığıyla uyumluluğu görüntüleyin ve atamaları yönetin.

## <a name="review-and-remediate-the-compliance-results"></a>Uyumluluk sonuçlarını gözden geçirme ve düzeltin

Aşağıdaki örnek bir Azure VM içindir, ancak sanal makine ölçek kümeleri için de geçerlidir. Uyumluluk sonuçlarını nasıl gözden geçireceğini öğrenmek için [bkz.](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources) **VMs İlke Kapsamı için Azure Monitörü** sayfasında, tablodan bir yönetim grubu veya abonelik seçin. Elipsleri seçerek **Uyumluluğu** Görüntüle'yi seçin (...).   

![Azure VM'ler için ilke uyumluluğu](./media/vminsights-enable-at-scale-policy/policy-view-compliance.png)

Girişimle birlikte verilen politikaların sonuçlarına göre, VM'ler aşağıdaki senaryolarda uyumsuz olarak raporlanır:

* Günlük Analitiği aracısı veya Bağımlılık aracısı dağıtılmamadı.  
    Bu senaryo, varolan VM'leri olan bir kapsam için tipiktir. Bunu azaltmak için, uyumlu olmayan bir ilke [üzerinde düzeltme görevleri oluşturarak](../../governance/policy/how-to/remediate-resources.md) gerekli aracıları dağıtın.  
    - Linux VM'leri için Bağımlılık aracısı dağıtma
    - Windows VM'leri için Bağımlılık aracısı dağıtma
    - Linux VM'leri için Log Analytics aracısı dağıtın
    - Windows VM'ler için Günlük Analizi aracısı dağıtma

* VM görüntüsü (OS) ilke tanımında tanımlanmaz.  
    Dağıtım ilkesinin ölçütleri yalnızca tanınmış Azure VM görüntülerinden dağıtılan VM'leri içerir. VM OS'nin desteklenip desteklenmediğini görmek için belgeleri denetleyin. Desteklenmiyorsa, dağıtım ilkesini çoğaltın ve görüntüyü uyumlu hale getirmek için güncelleştirin veya değiştirin.  
    - Denetim Bağımlılık aracısı dağıtımı – VM görüntüsü (OS) listelenmemiş
    - Denetim Günlüğü Analytics aracısı dağıtımı – VM görüntüsü (OS) listedışı

* VM'ler belirtilen Log Analytics çalışma alanına giriş yapmıyor.  
    Girişim kapsamındaki bazı VM'ler, ilke atamasında belirtilenden başka bir Günlük Analizi çalışma alanında oturum açıyor olabilir. Bu ilke, hangi VM'lerin uyumlu olmayan bir çalışma alanına rapor larını belirlemek için bir araçtır.  
    - VM için Denetim Günlüğü Analizi çalışma alanı – Rapor uyuşmazlığı

## <a name="edit-an-initiative-assignment"></a>Girişim ataması'nı değiştirme

Bir yönetim grubuna veya aboneye bir girişim atadıktan sonra, aşağıdaki özellikleri değiştirmek için bu girişimi değiştirebilirsiniz:

- Atama adı
- Açıklama
- Tarafından atanan
- Log Analytics çalışma alanı
- Özel Durumlar

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için izleme etkinleştirildiğinden, bu bilgiler Sanal Makineler için Azure Monitor ile analiz edilebilir. 

- Keşfedilen uygulama bağımlılıklarını görüntülemek [için, VM'ler Haritası için Azure Monitörünü Görüntüle'ye](vminsights-maps.md)bakın. 

- VM'nizin performansıyla birlikte darboğazları ve genel kullanımı belirlemek için Azure [VM performansını görüntüleyin.](vminsights-performance.md) 
