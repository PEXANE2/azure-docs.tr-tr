---
title: Azure Monitör Günlükleri dağıtımınızı tasarlama | Microsoft Dokümanlar
description: Bu makalede, Azure Monitor'da bir çalışma alanı dağıtmaya hazırlanan müşteriler için göz önünde bulundurulması gereken hususlar ve öneriler açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/20/2019
ms.openlocfilehash: e493b07814821496f941a4b81402ba0b49acbede
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248833"
---
# <a name="designing-your-azure-monitor-logs-deployment"></a>Azure Monitör Günlükleri dağıtımınızı tasarlama

Azure Monitor [günlük](data-platform-logs.md) verilerini, bir Azure kaynağı olan Günlük Analizi çalışma alanında ve verilerin toplandığı, toplandığı ve yönetim sınırı olarak hizmet ettiği bir kapsayıcıda depolar. Azure aboneliğinizde bir veya daha fazla çalışma alanı dağıtabilmenize eklenebilirken, ilk dağıtımınızın size uygun maliyetli, yönetilebilir ve ölçeklenebilir bir çözüm sağlamak için yönergelerimize uyduğundan emin olmak için anlamanız gereken birkaç husus vardır kuruluşlarınızın gereksinimlerini karşılayan dağıtım.

Çalışma alanındaki veriler, her biri farklı türde verileri depolayan ve verileri oluşturan kaynağa dayalı kendi benzersiz özellikleri kümesine sahip tablolar halinde düzenlenir. Çoğu veri kaynağı, Log Analytics çalışma alanında kendi tablolarına yazar.

![Örnek çalışma alanı veri modeli](./media/design-logs-deployment/logs-data-model-01.png)

Günlük Analizi çalışma alanı şunları sağlar:

* Veri depolama için coğrafi bir konum.
* Önerilen tasarım stratejilerimizden birini izleyerek farklı kullanıcılara erişim hakları vererek veri yalıtımı.
* [Fiyatlandırma katmanı,](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier) [bekletme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)ve veri [kapama](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#manage-your-maximum-daily-data-volume)gibi ayarların yapılandırması için kapsam.

Bu makalede, tasarım ve geçiş hususları, erişim denetimine genel bakış ve BT kuruluşunuz için önerdiğimiz tasarım uygulamalarının anlaşılması hakkında ayrıntılı bir genel bakış sağlanması sağlanıp, bu uygulamanın anlaşılması sağlanıp, bu tür konulara ilişkin bilgiler verilmektedir.



## <a name="important-considerations-for-an-access-control-strategy"></a>Erişim kontrol stratejisi için önemli noktalar

İhtiyacınız olan çalışma alanı sayısını belirlemek aşağıdaki gereksinimlerden biri veya birkaçı tarafından etkilenir:

* Küresel bir şirketsiniz ve veri egemenliği veya uyumluluk nedenleriyle belirli bölgelerde depolanan günlük verilerine ihtiyacınız vardır.
* Azure kullanıyorsanız ve çalışma alanını, yönettiği Azure kaynaklarıyla aynı bölgede bulundurarak giden veri aktarımı ücretlerini ortadan kaldırmak istiyorsanız.
* Birden çok departmanı veya iş gruplarını yönetirsiniz ve her birinin kendi verilerini görmesini istersiniz, ancak başkalarından gelen verileri görmezsiniz. Ayrıca, konsolide bir çapraz departman veya iş grubu görünümü için iş gereksinimi yoktur.

Günümüzde BT kuruluşları, her iki yapının merkezileştirilmiş, merkezi olmayan veya arada bir melezi olarak modellenmiştir. Sonuç olarak, aşağıdaki çalışma alanı dağıtım modelleri genellikle aşağıdaki kuruluş yapılarından birine eşlemek için kullanılmıştır:

* **Merkezi :** Tüm günlükler merkezi bir çalışma alanında depolanır ve tek bir ekip tarafından yönetilir ve Azure Monitor takım başına farklı erişim sağlar. Bu senaryoda, günlükleri yönetmek, kaynaklar arasında arama yapmak ve günlükleri çapraz ilişkilendirmek kolaydır. Çalışma alanı, aboneliğinizdeki birden çok kaynaktan toplanan veri miktarına bağlı olarak önemli ölçüde büyüyebilir ve farklı kullanıcılara erişim denetimini sağlamak için ek yönetim yükü eklenir. Bu model "hub ve spoke" olarak bilinir.
* **Merkezi olmayan**: Her takımın kendi sahip oldukları ve yönettiği bir kaynak grubunda oluşturulan kendi çalışma alanı vardır ve günlük verileri kaynak başına ayrılmıştır. Bu senaryoda, çalışma alanı güvenli tutulabilir ve erişim denetimi kaynak erişimiyle tutarlıdır, ancak günlükleri ilişkilendirmek zordur. Birçok kaynağın geniş bir görünümüne ihtiyaç duyan kullanıcılar verileri anlamlı bir şekilde çözümleyemez.
* **Karma**: Birçok kuruluş her iki dağıtım modelini de paralel olarak uyguladığından, güvenlik denetimi uyumluluk gereksinimleri bu senaryoyu daha da karmaşık hale getirmektedir. Bu genellikle günlükleri kapsama boşlukları ile karmaşık, pahalı ve bakımı zor yapılandırma ile sonuçlanır.

Veri toplamak için Log Analytics aracılarını kullanırken, aracı dağıtımınızı planlamak için aşağıdakileri anlamanız gerekir:

* Windows aracılarından veri toplamak için, sistem merkezi operasyon yöneticisi yönetim grubuna rapor verirken bile [her aracıbir veya daha fazla çalışma alanlarına rapor layacak](../../azure-monitor/platform/agent-windows.md)şekilde yapılandırabilirsiniz. Windows aracısı en fazla dört çalışma alanı bildirebilir.
* Linux aracısı çoklu homing'i desteklemez ve yalnızca tek bir çalışma alanına rapor verebilir.

System Center Operations Manager 2012 R2 veya sonraki lerini kullanıyorsanız:

* Her Operasyon Yöneticisi yönetim grubu [yalnızca bir çalışma alanına bağlanabilir.](../platform/om-agents.md) 
* Bir yönetim grubuna rapor eden Linux bilgisayarları, doğrudan bir Log Analytics çalışma alanına raporlanacak şekilde yapılandırılmalıdır. Linux bilgisayarlarınız zaten doğrudan bir çalışma alanına rapor veriyorsa ve bunları Operations Manager ile izlemek istiyorsanız, [bir Operasyon Yöneticisi yönetim grubuna rapor](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group)vermek için aşağıdaki adımları izleyin. 
* Windows bilgisayarına Log Analytics Windows aracısını yükleyebilir ve hem çalışma alanıyla tümleşik operations manager'a hem de farklı bir çalışma alanına rapor etmesini sağlayabilirsiniz.

## <a name="access-control-overview"></a>Erişim denetimine genel bakış

Rol tabanlı erişim denetimi (RBAC) ile, kullanıcılara ve gruplara yalnızca çalışma alanındaki verileri izlemeyle çalışmak için gereken erişim miktarını verebilirsiniz. Bu, toplanan verileri tüm kaynaklarınızda depolamak için tek bir çalışma alanı kullanarak BT kuruluş işletim modeliyle hizalamanızı sağlar. Örneğin, Azure sanal makinelerinde (VM) barındırılan altyapı hizmetlerinden sorumlu ekibinize erişim izni verirsiniz ve sonuç olarak yalnızca Sanal Makineler tarafından oluşturulan günlüklere erişebilirler. Bu, yeni kaynak bağlam günlüğü modelimizi takip ediyor. Bu modelin temeli, bir Azure kaynağı tarafından yayılan her günlük kaydı içindir, otomatik olarak bu kaynakla ilişkilidir. Günlükler, kaynaklara dayalı kapsam ve RBAC'a saygı gösteren merkezi bir çalışma alanına iletilir.

Bir kullanıcının erişebilen verileri, aşağıdaki tabloda listelenen etkenlerin bir birleşimi tarafından belirlenir. Her biri aşağıdaki bölümlerde açıklanmıştır.

| Faktörü | Açıklama |
|:---|:---|
| [Erişim modu](#access-mode) | Kullanıcının çalışma alanına erişmek için kullandığı yöntem.  Kullanılabilir verilerin kapsamını ve uygulanan erişim denetim modunu tanımlar. |
| [Erişim kontrol modu](#access-control-mode) | İzinlerin çalışma alanında mı yoksa kaynak düzeyinde mi uygulandığını tanımlayan çalışma alanını ayarlama. |
| [Izin](manage-access.md) | Çalışma alanı veya kaynak için tek tek veya kullanıcı gruplarına uygulanan izinler. Kullanıcının hangi verilere erişeceğini tanımlar. |
| [Tablo seviyesi RBAC](manage-access.md#table-level-rbac) | Erişim modlarından veya erişim denetim moduna bakılmaksızın tüm kullanıcılar için geçerli olan isteğe bağlı parçalı izinler. Bir kullanıcının erişebileceği veri türlerini tanımlar. |

## <a name="access-mode"></a>Erişim modu

*Erişim modu,* kullanıcının Bir Log Analytics çalışma alanına nasıl eriştiğini ve erişebileceği verilerin kapsamını nasıl tanımladığını ifade eder. 

Kullanıcıların verilere erişmek için iki seçeneği vardır:

* **Çalışma alanı bağlamı**: İzin aldığınız çalışma alanındaki tüm günlükleri görüntüleyebilirsiniz. Bu moddaki sorgular, çalışma alanındaki tüm tablolardaki tüm verilere kapsamlıdır. Azure portalındaki **Azure Monitör** menüsünden **Günlükler'i** seçtiğinizde olduğu gibi, günlüklere kapsam olarak çalışma alanıyla erişildiğinde kullanılan erişim modudur.

    ![Çalışma alanından Günlük Analizi bağlamı](./media/design-logs-deployment/query-from-workspace.png)

* **Kaynak bağlamı**: Azure portalındaki bir kaynak menüsünden **Günlükler'i** seçtiğinizde olduğu gibi belirli bir kaynak, kaynak grubu veya abonelik için çalışma alanına erişdiğinizde, erişebildiğiniz tüm tablolardaki tüm kaynaklar için günlükleri görüntüleyebilirsiniz. Bu moddaki sorgular yalnızca bu kaynakla ilişkili verilere göre kapsamdadır. Bu mod aynı zamanda parçalı RBAC sağlar.

    ![Kaynaktan Günlük Analizi bağlamı](./media/design-logs-deployment/query-from-resource.png)

    > [!NOTE]
    > Günlükler, kaynak bağlam sorguları için yalnızca ilgili kaynakla düzgün şekilde ilişkilendirildiyse kullanılabilir. Şu anda, aşağıdaki kaynakların sınırlamaları vardır:
    > - Azure dışındaki bilgisayarlar
    > - Service Fabric
    > - Application Insights
    >
    > Günlüklerin bir sorgu çalıştırarak ve ilgilendiğiniz kayıtları inceleyerek günlüklerin kaynaklarıyla doğru şekilde ilişkilendirilip ilişkilendirilmediklerini sınayabilirsiniz. Doğru kaynak kimliği [_ResourceId](log-standard-properties.md#_resourceid) özelliğindeyse, veriler kaynak merkezli sorgular için kullanılabilir.

Azure Monitor, günlük aramasını gerçekleştirdiğiniz içeriğe bağlı olarak doğru modu otomatik olarak belirler. Kapsam her zaman Log Analytics'in sol üst bölümünde sunulur.

### <a name="comparing-access-modes"></a>Erişim modlarını karşılaştırma

Aşağıdaki tabloda erişim modları özetlenebilir:

| | Çalışma alanı bağlamı | Kaynak bağlamı |
|:---|:---|:---|
| Her model kime yöneliktir? | Merkezi yönetim. Veri toplamayı yapılandırması gereken yöneticiler ve çok çeşitli kaynaklara erişmeye ihtiyacı olan kullanıcılar. Şu anda Azure dışındaki kaynaklar için günlüklere erişmesi gereken kullanıcılar için de gereklidir. | Uygulama ekipleri. Azure kaynaklarının yöneticileri izleniyor. |
| Bir kullanıcı günlükleri görüntülemek için ne gerektirir? | Çalışma alanına izinler. [Bkz. Çalışma alanı izinlerini kullanarak erişimi yönet'te](manage-access.md#manage-access-using-workspace-permissions)Çalışma Alanı **izinleri.** | Kaynağa erişimi okuyun. [Azure izinlerini kullanarak erişimi yönet'te](manage-access.md#manage-access-using-azure-permissions)Kaynak **izinlerine** bakın. İzinler devralınabilir (içeren kaynak grubundan gibi) veya doğrudan kaynağa atanabilir. Kaynak için günlüklere izin otomatik olarak atanır. |
| İzinlerin kapsamı nedir? | Çalışma alanı. Çalışma alanına erişimi olan kullanıcılar, çalışma alanındaki tüm günlükleri izinleri olan tablolardan sorgulayabilir. Bkz. [Tablo erişim denetimi](manage-access.md#table-level-rbac) | Azure kaynağı. Kullanıcı, herhangi bir çalışma alanından erişebildikleri belirli kaynaklar, kaynak grupları veya abonelik için günlükleri sorgulayabilir, ancak diğer kaynaklar için günlükleri sorgulayamamaktadır. |
| Kullanıcı günlüklerine nasıl erişebilir? | <ul><li>**Azure Monitor** menüsünden **Günlükleri** Başlat.</li></ul> <ul><li>Günlük Analiz **çalışma alanlarından** **Günlükleri** başlatın.</li></ul> <ul><li>Azure Monitör [Çalışma Kitaplarından.](../visualizations.md#workbooks)</li></ul> | <ul><li>Azure kaynağı için menüden **Günlükleri** Başlat</li></ul> <ul><li>**Azure Monitor** menüsünden **Günlükleri** Başlat.</li></ul> <ul><li>Günlük Analiz **çalışma alanlarından** **Günlükleri** başlatın.</li></ul> <ul><li>Azure Monitör [Çalışma Kitaplarından.](../visualizations.md#workbooks)</li></ul> |

## <a name="access-control-mode"></a>Erişim kontrol modu

*Access denetim modu,* her çalışma alanında, çalışma alanı için izinlerin nasıl belirlendiğini tanımlayan bir ayardır.

* **Çalışma alanı izinleri gerektirir**: Bu denetim modu parçalı RBAC'a izin vermez. Bir kullanıcının çalışma alanına erişmesi için, çalışma alanına veya belirli tablolara izin verilmesi gerekir.

    Bir kullanıcı çalışma alanı bağlamı modunu izleyerek çalışma alanına erişirilirse, erişim izni verilen herhangi bir tablodaki tüm verilere erişebilir. Bir kullanıcı kaynak bağlam modunu izleyerek çalışma alanına erişirilirse, yalnızca erişim izni verilen herhangi bir tablodaki bu kaynağa ait verilere erişebilir.

    Bu, Mart 2019'dan önce oluşturulan tüm çalışma alanları için varsayılan ayardır.

* **Kaynak veya çalışma alanı izinlerini kullanın**: Bu denetim modu parçalı RBAC'a izin verir. Kullanıcılara yalnızca Azure `read` izni atayarak görüntüleyebilecekleri kaynaklarla ilişkili verilere erişim izni verilebilir. 

    Bir kullanıcı çalışma alanı bağlamı modunda çalışma alanına erişiğinde, çalışma alanı izinleri uygulanır. Bir kullanıcı kaynak bağlam modunda çalışma alanına erişiğinde, yalnızca kaynak izinleri doğrulanır ve çalışma alanı izinleri yoksayılır. Çalışma alanı izinlerinden kaldırarak ve kaynak izinlerinin tanınmasına izin vererek kullanıcı için RBAC'ı etkinleştirin.

    Bu, Mart 2019'dan sonra oluşturulan tüm çalışma alanları için varsayılan ayardır.

    > [!NOTE]
    > Bir kullanıcının çalışma alanı için yalnızca kaynak izinleri varsa, yalnızca kaynak bağlamı modunu kullanarak çalışma alanına erişebilir, çalışma alanı erişim modunun **kaynak veya çalışma alanı izinlerini kullanacak**şekilde ayarlı olduğunu varsayarsak.

PowerShell ile portaldaki erişim denetim modunu nasıl değiştireceğinizi veya Kaynak Yöneticisi şablonunu kullanmayı öğrenmek için [erişim denetim modunu Yapılandır'a](manage-access.md#configure-access-control-mode)bakın.

## <a name="ingestion-volume-rate-limit"></a>Yutma hacim oranı sınırı

Azure Monitor, her ay terabaytlarce veri gönderen binlerce müşteriye artan bir hızla hizmet veren yüksek ölçekli bir veri hizmetidir. Varsayılan yutma oranı eşiği çalışma alanı başına **6 GB/dk** olarak ayarlanır. Bu, günlük uzunluğuna ve sıkıştırma oranına bağlı olarak veri türleri arasında değişebildiği için yaklaşık bir değerdir. Bu sınır aracılardan veya Veri Toplayıcı [API'sinden](data-collector-api.md)gönderilen veriler için geçerli değildir.

Verileri tek bir çalışma alanına daha yüksek bir hızda gönderirseniz, bazı veriler bırakılır ve eşik aşılması devam ederken çalışma alanınızdaki *Çalışma* tablosuna her 6 saatte bir olay gönderilir. Yutma hacminiz fiyat sınırını aşmaya devam ederse veya yakın bir zamanda bu sayıya ulaşmayı bekliyorsanız, bir destek isteği açarak çalışma alanınızda bir artış talep edebilirsiniz.
 
Çalışma alanınızda böyle bir olaydan haberdar olmak için, sıfırdan daha fazla sonuç sayısına ilişkin uyarı mantığı tabanına sahip aşağıdaki sorguyu kullanarak bir [günlük uyarı kuralı](alerts-log.md) oluşturun.

``` Kusto
Operation
|where OperationCategory == "Ingestion"
|where Detail startswith "The rate of data crossed the threshold"
``` 


## <a name="recommendations"></a>Öneriler

![Kaynak bağlamı tasarım örneği](./media/design-logs-deployment/workspace-design-resource-context-01.png)

Bu senaryo, BT kuruluşları aboneliğinizde veri egemenliği veya mevzuata uygunluk tarafından kısıtlanmayan veya kaynaklarınızın dağıtılan bölgelerineşlerini eşlemesi gereken tek bir çalışma alanı tasarımını kapsar. Kuruluşlarınuzun güvenliğini ve BT yöneticisi ekiplerinin Azure erişim yönetimi ve daha güvenli erişim denetimi ile geliştirilmiş tümleştirmeden yararlanmaolanağı sağlar.

Farklı ekipler tarafından tutulan altyapı ve uygulamaları destekleyen Tüm kaynaklar, izleme çözümleri ve Sanal Taşıtlar için Uygulama Öngörüleri ve Azure Monitörü gibi öngörüler, toplanan günlük verilerini BT kuruluşlarına iletecek şekilde yapılandırılır merkezi paylaşılan çalışma alanı. Her takımdaki kullanıcılara, kendilerine erişim izni verilen kaynaklar için günlüklere erişim izni verilir.

Çalışma alanı mimarinizi dağıttıktan sonra, [Azure İlkesi](../../governance/policy/overview.md)ile bunu Azure kaynaklarında uygulayabilirsiniz. Tüm kaynak günlüklerini belirli bir çalışma alanına göndermeleri için ilkeleri tanımlamanın ve Azure kaynaklarınızla uyumluluğu sağlamanın bir yolunu sağlar. Örneğin, Azure sanal makineleri veya sanal makine ölçek kümeleriyle, çalışma alanı uyumluluğunu değerlendiren ve sonuçları bildiren varolan ilkeleri kullanabilir veya uyumlu değilse düzeltecek şekilde özelleştirebilirsiniz.  

## <a name="workspace-consolidation-migration-strategy"></a>Çalışma alanı konsolidasyonu geçiş stratejisi

Birden çok çalışma alanı dağıtmış olan ve kaynak bağlamerişim modeline birleştirmek isteyen müşteriler için, önerilen erişim modeline geçiş yapmak için artımlı bir yaklaşım almanızı öneririz ve bunu başarmaya çalışmazsanız, hızlı veya agresif. Makul bir zaman çizelgesini izleyerek planlamak, göç etmek, doğrulamak ve emekli olmak için aşamalı bir yaklaşım izleyen, bulut işlemlerinizde planlanmamış olayların veya beklenmeyen etkilerin önlenmesine yardımcı olur. Uyumluluk veya iş nedenleriyle ilgili bir veri saklama ilkesiniz yoksa, işlem sırasında geçiş yaptığınız çalışma alanında verileri tutmak için uygun süreyi değerlendirmeniz gerekir. Paylaşılan çalışma alanına rapor vermek üzere kaynakları yeniden yapılandırırken, gerektiğinde özgün çalışma alanındaki verileri çözümleyebilirsiniz. Geçiş tamamlandıktan sonra, bekletme süresinin bitiminden önce orijinal çalışma alanında veri tutmak için yönetilirseniz, silmeyin.

Bu modele geçişinizi planlarken aşağıdakileri göz önünde bulundurun:

* Veri saklamayla ilgili endüstri yönetmeliklerinin ve dahili politikalarına uymanız gerekenleri anlayın.
* Uygulama ekiplerinizin varolan kaynak bağlamı işlevselliği içinde çalışabilmesini unutmayın.
* Uygulama ekipleriniz için kaynaklara verilen erişimi belirleyin ve üretimde uygulamadan önce bir geliştirme ortamında test edin.
* **Kaynak veya çalışma alanı izinlerini kullan'ı**etkinleştirecek şekilde çalışma alanını yapılandırın.
* Uygulama ekiplerinin çalışma alanını okuma ve sorgulama iznini kaldırın.
* Kapsayıcılar için Azure Monitor ve/veya VM'ler için Azure Monitor, Otomasyon hesabınız(lar) ve özgün çalışma alanında dağıtılan Update Management, Start/Stop VM'ler gibi yönetim çözümlerini etkinleştirin ve yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda önerilen güvenlik izinlerini ve denetimlerini uygulamak [için günlüklere erişimi gözden geçirin.](manage-access.md)
