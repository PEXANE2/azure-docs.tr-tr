---
author: tanmaygore
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: tagore
ms.openlocfilehash: b86e0d784d26e9e483dd12e20d45189ae8bfb9bd
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81866210"
---
## <a name="migrate-iaas-resources-from-the-classic-deployment-model-to-azure-resource-manager"></a>IaaS kaynaklarını klasik dağıtım modelinden Azure Kaynak Yöneticisi'ne geçirin
İlk olarak, bir hizmet (IaaS) kaynakları olarak altyapıdaki veri düzlemi ve yönetim-düzlem operasyonları arasındaki farkı anlamak önemlidir.

* *Yönetim/denetim düzlemi,* kaynakları değiştirmek için yönetim/denetim düzlemine veya API'ye gelen çağrıları açıklar. Örneğin, VM oluşturma, bir sanal makineyi yeniden başlatma ve çalışmakta olan kaynakları yönetmek için bir sanal ağı yeni alt ağla güncelleştirme gibi işlemler. VM'lere bağlanmayı doğrudan etkilemezler.
* *Veri düzlemi* (uygulama) uygulamanın çalışma zamanını açıklar ve Azure API'den geçmeyen örneklerle etkileşim içerir. Örneğin, web sitenize erişmek veya çalışan bir SQL Server örneğinden veya MongoDB sunucusundan veri çekmek veri düzlemi veya uygulama etkileşimleridir. Diğer örnekler arasında bir depolama hesabından bir blob kopyalama ve sanal makineye Uzak Masaüstü Protokolü (RDP) veya Güvenli Kabuk (SSH) kullanmak için ortak bir IP adresine erişmek sayılabilir. Bu işlemler, uygulamanın işlem, ağ ve depolama kaynaklarında çalışmaya devam etmesini sağlar.

Veri düzlemi, klasik dağıtım modeli ile Kaynak Yöneticisi yığınları arasında aynıdır. Aradaki fark, geçiş işlemi sırasında Microsoft'un kaynakların genel dağıtım modelindeki tümevarı Kaynak Yöneticisi yığınında bu şekilde çevirmesidir. Sonuç olarak, Kaynak Yöneticisi yığınındaki kaynaklarınızı yönetmek için yeni araçlar, API'ler ve SDK'lar kullanmanız gerekir.

![Yönetim/kontrol düzlemi ile veri düzlemi arasındaki farkı gösteren diyagram](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> Bazı geçiş senaryolarında, Azure platformu sanal makinenizi durdurur, serbest bırakır ve yeniden başlatır. Bu, kısa bir veri düzlemi kapalı kalma süresine neden olur.
>

## <a name="the-migration-experience"></a>Geçiş deneyimi
Geçişe başlamadan önce:

* Geçirmek istediğiniz kaynakların desteklenmeyen özellikleri veya yapılandırmaları olmadığından emin olun. Bu sorunlar genellikle platform tarafından algılanır ve bir hata oluşturulur.
* Sanal ağda olmayan VM'leriniz varsa, bunlar durdurulur ve hazırlama işleminin bir parçası olarak ayrılır. Genel IP adresini kaybetmek istemiyorsanız, hazırlama işlemini tetiklemeden önce IP adresini ayırmayı düşünün. VM'ler sanal bir ağdaysa, durdurulup ayrılmaz.
* Geçiş sırasında gerçekleşebilecek beklenmeyen hataları göz önünde bulundurarak geçişinizi çalışma saatleri dışına gerçekleşecek şekilde planlayın.
* Hazırlama adımı tamamlandıktan sonra doğrulamayı kolaylaştırmak için PowerShell’i, komut satırı arabirimi (CLI) komutlarını veya REST API’leri kullanarak VM’lerinizin geçerli yapılandırmasını indirin.
* Geçişe başlamadan önce Kaynak Yöneticisi dağıtım modelini işlemek için otomasyon ve operasyonelleştirme komut dosyalarınızı güncelleştirin. İsteğe bağlı olarak, kaynaklar hazırlanmış durumdayken GET işlemleri gerçekleştirebilirsiniz.
* Klasik dağıtım modelinde IaaS kaynaklarında yapılandırılan Rol Tabanlı Erişim Denetimi (RBAC) ilkelerini değerlendirin ve geçiş tamamlandıktan sonra planlayın.

Geçiş iş akışı aşağıdaki gibidir:

![Geçiş iş akışını gösteren diyagram](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Aşağıdaki bölümlerde açıklanan işlemlerin tümü idempotenttir. Desteklenmeyen bir özellik veya yapılandırma hatası dışında bir sorununuz varsa, işlemi hazırlamayı, iptal etmeyi veya işlemeyi yeniden deneyin. Azure eylemi yeniden dener.
>
>

### <a name="validate"></a>Doğrulama
Doğrulama işlemi, geçiş sürecinin ilk adımıdır. Bu adımın amacı, klasik dağıtım modelinde geçirmek istediğiniz kaynakların durumunu çözümlemektir. İşlem, kaynakların geçiş (başarı veya başarısızlık) yeteneğine sahip olup olmadığını değerlendirir.

Geçiş için doğrulamak istediğiniz sanal ağı veya bulut hizmetini (sanal ağda değilse) seçersiniz. Kaynak geçiş yapamazsa, Azure bunun nedenlerini listeler.

#### <a name="checks-not-done-in-the-validate-operation"></a>Doğrulama işleminde yapılamayan denetimler

Doğrulama işlemi yalnızca klasik dağıtım modelindeki kaynakların durumunu analiz eder. Klasik dağıtım modelindeki çeşitli yapılandırmalar nedeniyle tüm hataları ve desteklenmeyen senaryoları denetleyebilir. Azure Kaynak Yöneticisi yığınının geçiş sırasında kaynaklara dayatabileceği tüm sorunları denetlemek mümkün değildir. Bu sorunlar yalnızca kaynaklar geçişin bir sonraki adımında (hazırlama işlemi) dönüşümden geçtiğinde denetlenir. Aşağıdaki tablo, doğrulama işleminde denetlenmiyor tüm sorunları listeler:


|Doğrulama işleminde ağ denetimleri|
|-|
|Hem ER hem de VPN ağ geçidine sahip bir sanal ağ.|
|Bağlantısı kesilen durumda sanal ağ ağ geçidi bağlantısı.|
|Tüm ER devreleri Azure Kaynak Yöneticisi yığınına önceden taşınır.|
|Azure Kaynak Yöneticisi kotası ağ kaynaklarını denetler. Örneğin: statik genel IP, dinamik genel IP'ler, yük dengeleyicisi, ağ güvenlik grupları, rota tabloları ve ağ arabirimleri. |
| Tüm yük dengeleyici kuralları dağıtım ve sanal ağ arasında geçerlidir. |
| Aynı sanal ağda durdurulan VM'ler arasında çakışan özel IP'ler. |

### <a name="prepare"></a>Hazırlama
Hazırlama işlemi, geçiş sürecinin ikinci adımıdır. Bu adımın amacı, IaaS kaynaklarının klasik dağıtım modelinden Kaynak Yöneticisi kaynaklarına dönüşümunu simüle etmektir. Ayrıca, hazırlama işlemi bu yan yana görselleştirmeniz için sunar.

> [!NOTE] 
> Klasik dağıtım modelindeki kaynaklarınız bu adımda değiştirilmez. Eğer göçü deniyorsanız, bu güvenli bir adımdır. 

Geçiş için hazırlamak istediğiniz sanal ağı veya bulut hizmetini (sanal ağ değilse) seçersiniz.

* Kaynak geçiş yapamazsa, Azure geçiş işlemini durdurur ve hazırlama işleminin başarısız olmasının nedenini listeler.
* Kaynak geçiş yapabiliyorsa, Azure geçiş altındaki kaynaklar için yönetim düzlemi işlemlerini kilitler. Örneğin, geçirilmekte olan bir VM’ye veri diski ekleyemezsiniz.

Azure daha sonra, geçiş teki kaynaklar için klasik dağıtım modelinden Kaynak Yöneticisi'ne meta verilerin geçişini başlatır.

Hazırlama işlemi tamamlandıktan sonra, hem klasik dağıtım modelinde hem de Kaynak Yöneticisi'nde kaynakları görselleştirme seçeneğiniz vardır. Azure platformu, klasik dağıtım modelindeki her bulut hizmeti için `cloud-service-name>-Migrated` deseninde bir kaynak grubu adı oluşturur.

> [!NOTE]
> Geçirilen kaynaklar için oluşturulan bir kaynak grubunun adını seçmek mümkün değildir (yani "-Geçirilen"). Ancak geçiş tamamlandıktan sonra, kaynakları istediğiniz kaynak grubuna taşımak için Azure Kaynak Yöneticisi'nin taşıma özelliğini kullanabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../articles/resource-group-move-resources.md).

Aşağıdaki iki ekran görüntüsü, başarılı bir hazırlama işleminden sonra sonucu gösterir. İlki, özgün bulut hizmetini içeren bir kaynak grubunu gösterir. İkincisi, eşdeğer Azure Kaynak Yöneticisi kaynaklarını içeren yeni "-Geçirilen" kaynak grubunu gösterir.

![Özgün bulut hizmetini gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Hazırlama işleminde Azure Kaynak Yöneticisi kaynaklarını gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Burada hazırlık aşaması tamamlandıktan sonra kaynaklarınızı bir perde arkası bir bakış. Veri düzlemindeki kaynağın aynı olduğunu unutmayın. Hem yönetim düzleminde (klasik dağıtım modeli) hem de denetim düzleminde (Kaynak Yöneticisi) temsil edilir.

![Hazırlama aşamasının diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Klasik dağıtım modelinde sanal ağda olmayan VM'ler durdurulur ve geçişin bu aşamasında ayrılır.
>

### <a name="check-manual-or-scripted"></a>Denetim (el ile veya betikle)
Denetim adımında, geçişin doğru göründüğünü doğrulamak için daha önce karşıdan yüklediğiniz yapılandırmayı kullanma seçeneğiniz var. Alternatif olarak, portalda oturum açabilir ve meta veri geçişinin iyi göründüğünü doğrulamak için özellikleri ve kaynakları denetleyebilirsiniz.

Sanal ağ geçişi yapıyorsanız, sanal makinelerin çoğu yapılandırması yeniden başlatılmaz. Bu VM'ler üzerindeki uygulamalar için, uygulamanın hala çalıştığını doğrulayabilirsiniz.

VM'lerin beklendiği gibi çalışıp çalışmadığınızı ve güncelleştirilmiş komut dosyalarınızın doğru çalışıp çalışmadığınızı görmek için izleme ve işletim komut dosyalarınızı sınayabilirsiniz. Kaynaklar hazırlanmış durumdayken yalnızca GET işlemleri desteklenir.

Geçiş için gereken zaman kümesi yoktur. Bu durumdayken dilediğiniz kadar bekleyebilirsiniz. Bununla birlikte, bu kaynaklar için geçişi durdurana veya işleyene kadar yönetim düzlemi kilitli kalır.

Herhangi bir sorun yaşarsanız dilediğiniz zaman geçişi durdurabilir ve klasik dağıtım modeline dönebilirsiniz. Geri döndükten sonra Azure, klasik dağıtım modelinde bu VM'lerde normal işlemleri yeniden başlatabilmeniz için kaynaklardaki yönetim düzlemi işlemlerini açar.

### <a name="abort"></a>Durdurma
Değişikliklerinizi klasik dağıtım modeline geri almak ve geçişi durdurmak istiyorsanız, bu isteğe bağlı bir adımdır. Bu işlem, kaynaklarınız için Kaynak Yöneticisi meta verilerini (hazırlama adımında oluşturulan) siler. 

![İptal adımı diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Bu işlem, işleme işlemini tetikledikten sonra yapılamaz.     
>

### <a name="commit"></a>İşleme
Doğrulama adımını tamamladıktan sonra geçişi işleyebilirsiniz. Kaynaklar artık klasik dağıtım modelinde görünmez ve yalnızca Kaynak Yöneticisi dağıtım modelinde kullanılabilir. Geçirilen kaynaklar yalnızca yeni portalda yönetilebilir.

> [!NOTE]
> Bu, bir kere etkili olan bir işlemdir. Başarısız olursa, işlemi yeniden deneyin. Başarısız olmaya devam ederse, bir destek bileti oluşturun veya [Microsoft Q&A'da](https://docs.microsoft.com/answers/index.html) bir forum oluşturun
>
>

![Commit adım diyagramı](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="migration-flowchart"></a>Geçiş akış şeması

Geçişle nasıl devam edileceğine dair bir akış şeması aşağıda veda edebilirsiniz:

![Geçiş adımlarını gösteren ekran görüntüsü](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-the-classic-deployment-model-to-resource-manager-resources"></a>Klasik dağıtım modelinin Kaynak Yöneticisi kaynaklarına çevirisi
Kaynakların klasik dağıtım modelini ve Kaynak Yöneticisi temsillerini aşağıdaki tabloda bulabilirsiniz. Diğer özellikler ve kaynaklar şu an desteklenmemektedir.

| Klasik gösterim | Resource Manager gösterimi | Notlar |
| --- | --- | --- |
| Bulut hizmeti adı |DNS adı |Geçiş sırasında, her bulut hizmeti için `<cloudservicename>-migrated` adlandırma deseni kullanılarak yeni bir kaynak grubu oluşturulur. Bu kaynak grubu tüm kaynaklarınızı içerir. Bulut hizmeti adı, genel IP adresiyle ilişkili bir DNS adı olur. |
| Sanal makine |Sanal makine |Sanal makineye özgü özellikler değiştirilmeden geçirilir. Bilgisayar adı gibi belirli osProfile bilgileri klasik dağıtım modelinde depolanmaz ve geçişten sonra boş kalır. |
| Sanal makineye bağlı disk kaynakları |Sanal makineye bağlı örtük diskler |Resource Manager dağıtım modelinde diskler en üst düzey kaynaklar olarak modellenmez. Diskler, VM altındaki örtük diskler olarak geçirilir. Şu anda yalnızca sanal makineye bağlı diskler desteklenmektedir. Kaynak Yöneticisi VM'ler artık, disklerin herhangi bir güncelleştirme olmadan kolayca geçirilebilen klasik dağıtım modelinde depolama hesaplarını kullanabilir. |
| VM uzantıları |VM uzantıları |Klasik dağıtım modelinden XML uzantıları dışındaki tüm kaynak uzantıları geçirilir. |
| Sanal makine sertifikaları |Azure Key Vault’ta Sertifikalar |Bir bulut hizmeti hizmet sertifikaları içeriyorsa, geçiş bulut hizmeti başına yeni bir Azure anahtar kasası oluşturur ve sertifikaları anahtar kasasına taşır. VM’ler, anahtar kasasındaki sertifikalara başvuracak şekilde güncelleştirilir. <br><br> Anahtar kasasını silmeyin. Bu, VM'nin başarısız bir duruma girmesine neden olabilir. |
| WinRM yapılandırması |osProfile altındaki WinRM yapılandırması |Windows Uzaktan Yönetimi yapılandırması, geçiş kapsamında değiştirilmeden taşınır. |
| Kullanılabilirlik kümesi özelliği |Kullanılabilirlik kümesi kaynağı | Kullanılabilirlik kümesi belirtimi, klasik dağıtım modelinde VM'de bir özelliktir. Kullanılabilirlik kümeleri, geçiş kapsamında en üst düzey kaynağa dönüşür. Şu yapılandırmalar desteklenmez: bulut hizmeti başına birden çok kullanılabilirlik kümesi veya bir bulut hizmetindeki herhangi bir kullanılabilirlik kümesinde olmayan VM’lerle birlikte bir veya daha fazla kullanılabilirlik kümesi. |
| Bir VM’deki ağ yapılandırması |Birincil ağ arabirimi |Bir VM’deki ağ yapılandırması, geçişten sonra birincil ağ arabirimi kaynağı olarak gösterilir. Bir sanal ağda olmayan VM’lerin iç IP adresi geçiş sırasında değişir. |
| Bir VM’de birden çok ağ arabirimi |Ağ arabirimleri |Bir VM'de onunla ilişkili birden çok ağ arabirimi varsa, her ağ arabirimi tüm özellikleriyle birlikte geçişin bir parçası olarak üst düzey bir kaynak haline gelir. |
| Yük dengeli uç nokta kümesi |Yük dengeleyici |Klasik dağıtım modelinde, platform tarafından her bulut hizmetine örtük bir yük dengeleyici atanıyordu. Geçiş sırasında yeni bir yük dengeleyici kaynağı oluşturulur ve yük dengeleme uç noktası kümesi, yük dengeleyici kurallarına dönüşür. |
| Gelen NAT kuralları |Gelen NAT kuralları |VM’de tanımlanmış giriş uç noktaları, geçiş sırasında yük dengeleyici altındaki gelen ağ adresi çevirisi kurallarına dönüştürülür. |
| VIP adresi |DNS adına sahip genel IP adresi |Sanal IP adresi genel bir IP adresi haline gelir ve yük dengeleyicisi ile ilişkilidir. Yalnızca giriş uç noktası atanmış sanal IP’ler geçirilebilir. |
| Sanal ağ |Sanal ağ |Sanal ağ, tüm özellikleriyle birlikte Resource Manager dağıtım modeline geçirilir. `-migrated` adlı yeni bir kaynak grubu oluşturulur. |
| Ayrılmış IP’ler |Statik ayırma yöntemi kullanan genel IP adresi |Yük dengeleyiciyle ilişkili ayrılmış IP’ler, bulut hizmetinin veya sanal makinenin geçişiyle birlikte geçirilir. İlişkili olmayan ayrılmış IP geçişi şu an desteklenmemektedir. |
| VM başına genel IP adresi |Dinamik ayırma yöntemi kullanan genel IP adresi |VM’le ilişkili genel IP adresi, ayırma yöntemi statik olarak ayarlanarak bir genel IP adresi kaynağı olarak dönüştürülür. |
| NSG'ler |NSG'ler |Bir alt ağla ilişkili ağ güvenlik grupları, geçiş kapsamında Resource Manager dağıtım modeline kopyalanır. Geçiş sırasında, klasik dağıtım modelindeki NSG kaldırılmaz. Bununla birlikte, geçiş sürdüğü sırada NSG için yönetim düzlemi işlemleri engellenir. |
| DNS sunucuları |DNS sunucuları |Bir sanal ağ veya VM’le ilişkili DNS sunucuları, kendilerine karşılık gelen kaynağın geçişi kapsamında, tüm özellikleriyle birlikte geçirilir. |
| UDR’ler |UDR’ler |Bir alt ağla ilişkili kullanıcı tanımlı rotalar, geçiş kapsamında Resource Manager dağıtım modeline kopyalanır. Geçiş sırasında, klasik dağıtım modelindeki UDR kaldırılmaz. Geçiş sürdüğü sırada UDR için yönetim düzlemi işlemleri engellenir. |
| Bir sanal makinenin ağ yapılandırmasında IP iletme özelliği |NIC’de IP iletme özelliği |Bir VM’deki IP iletme özelliği, geçiş sırasında ağ arabirimindeki bir özelliğe dönüştürülür. |
| Birden çok IP’si olan yük dengeleyici |Birden çok genel IP kaynağı olan yük dengeleyici |Yük dengeleyicisiyle ilişkili her genel IP, ortak bir IP kaynağına dönüştürülür ve geçişten sonra yük dengeleyicisiyle ilişkilendirilir. |
| VM’deki iç DNS adları |NIC’deki iç DNS adları |Geçiş sırasında, VM’lerin iç DNS son ekleri NIC’deki “InternalDomainNameSuffix” adlı salt okunur bir özelliğe geçirilir. Geçişten sonra sonek değişmeden kalır ve VM çözünürlüğü daha önce olduğu gibi çalışmaya devam etmelidir. |
| Sanal ağ geçidi |Sanal ağ geçidi |Sanal ağ ağ geçidi özellikleri değişmeden geçirilir. Ağ geçidiyle ilişkili VIP de değişmez. |
| Yerel ağ sitesi |Yerel ağ geçidi |Yerel ağ sitesi özellikleri değişmeden yerel ağ ağ geçidi adı verilen yeni bir kaynağa geçirilir. Bu, şirket içi adres önekleri ve uzak ağ geçidi IP'sini temsil eder. |
| Bağlantı başvuruları |Bağlantı |Ağ yapılandırmasında ağ geçidi ve yerel ağ sitesi arasındaki bağlantı başvuruları Bağlantı adlı yeni bir kaynak tarafından temsil edilir. Ağ yapılandırma dosyalarındaki bağlantı başvurusu tüm özellikleri Bağlantı kaynağına değişmeden kopyalanır. Klasik dağıtım modelinde sanal ağlar arasındaki bağlantı, sanal ağları temsil eden yerel ağ sitelerine iki IPsec tüneli oluşturularak elde edilir. Bu, yerel ağ ağ geçitlerine gerek kalmadan Kaynak Yöneticisi modelinde sanal ağdan sanal ağa bağlantı türüne dönüştürülür. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Geçiş sonrası otomasyon ve araçlarınızda gerçekleşecek değişiklikler
Kaynaklarınızı klasik dağıtım modelinden Kaynak Yöneticisi dağıtım modeline geçirmenin bir parçası olarak, geçişten sonra çalışmaya devam etmesini sağlamak için varolan otomasyonunuzu veya araç kullanımınızı güncelleştirmeniz gerekir.
