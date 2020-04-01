---
title: İş sürekliliği ve olağanüstü durum kurtarma
description: Verileri korumak, yıkıcı olaylardan hızla kurtarmak, kritik iş işlevlerinin gerektirdiği kaynakları geri yüklemek ve Azure Mantık Uygulamaları için iş sürekliliğini korumak için stratejinizi tasarlayın
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: 7bf71ce7c44229ccf19022e9cfb0162f9d77cd97
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437710"
---
# <a name="business-continuity-and-disaster-recovery-for-azure-logic-apps"></a>Azure Mantık Uygulamaları için iş sürekliliği ve olağanüstü durum kurtarma

Öngörülemeyen olayların işletmeniz ve müşterileriniz üzerindeki etkisini ve etkilerini azaltmaya yardımcı olmak için, verileri koruyabilmek, kritik iş işlevlerini destekleyen kaynakları hızla geri yükleyebilmeniz ve [ *iş sürekliliğini* (BC)](https://en.wikipedia.org/wiki/Business_continuity_planning)korumak için işlemleri yürütmeye devam etmek için bir [ *olağanüstü durum kurtarma* (DR)](https://en.wikipedia.org/wiki/Disaster_recovery) çözümüne sahip olduğundan emin olun. Örneğin, kesintiler kesintiler, altta yatan altyapıdaki kayıplar veya depolama, ağ veya bilgi işlem kaynakları gibi bileşenlerdeki kayıplar, kurtarılamayan uygulama hataları ve hatta tam veri merkezi kaybı içerebilir. İşletmeniz insadansı ve olağanüstü durum kurtarma (BCDR) çözümünüzü hazır layarak, kuruluşunuz kesintilere, planlanmış veya planlanmamış kesintilere daha hızlı yanıt verebilir ve müşterilerinizin kapalı kalma süresini azaltabilir.

Bu makalede, [Azure Mantık Uygulamaları](../logic-apps/logic-apps-overview.md)kullanarak otomatik iş akışları oluştururken uygulayabileceğiniz BCDR kılavuzu ve stratejileri sağlanmaktadır. Mantıksal uygulama iş akışları, ne kadar kod yazmanız gerektiğini azaltarak uygulamalar, bulut hizmetleri ve şirket içi sistemler arasındaki verileri daha kolay entegre etmenize ve düzenlemenize yardımcı olur. BCDR için plan yaptığınızda, yalnızca mantıksal uygulamalarınızı değil, mantık uygulamalarınızla kullandığınız Azure kaynaklarını da göz önünde bulundurun:

* [Connections](../connectors/apis-list.md) Mantık uygulamalarından diğer uygulamalara, hizmetlere ve sistemlere oluşturduğunuz bağlantılar. Daha fazla bilgi için, bu konunun ilerleyen saatlerinde [kaynaklara bağlantılar](#resource-connections) abakın.

* Şirket içi sistemlerdeki verilere erişmek için mantık uygulamalarınızda oluşturduğunuz ve kullandığınız Azure kaynakları olan [şirket içi veri ağ geçitleri.](../logic-apps/logic-apps-gateway-connection.md) Her ağ geçidi kaynağı, yerel bir bilgisayarda ayrı bir [veri ağ geçidi yüklemesi](../logic-apps/logic-apps-gateway-install.md) temsil eder. Daha fazla bilgi için, bu konuyla ilgili daha sonra [şirket içi veri ağ geçitlerine](#on-premises-data-gateways) bakın.

* Mantıksal uygulamaların [işletmeden işletmeye (B2B) kurumsal tümleştirme](../logic-apps/logic-apps-enterprise-integration-overview.md) senaryoları için kullandığı yapıları tanımladığınız ve depoladığınız [tümleştirme hesapları.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) Örneğin, [tümleştirme hesapları için bölgeler arası olağanüstü durum kurtarma](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md)ayarlayabilirsiniz.

* Azure sanal ağında yalıtılmış Logic Apps çalışma zamanı örneğinde çalışan mantık uygulamaları oluşturduğunuz [tümleştirme hizmet ortamları (ISS'ler).](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) Bu mantıksal uygulamalar daha sonra bu sanal ağdaki bir güvenlik duvarının arkasında korunan kaynaklara erişebilir.

<a name="primary-secondary-locations"></a>

## <a name="primary-and-secondary-locations"></a>Birincil ve ikincil konumlar

Her mantık uygulamasının dağıtım için kullanmak istediğiniz konumu belirtmesi gerekir. Bu konum, "Batı ABD" gibi genel çok kiracılı Azure'da veya daha önce oluşturduğunuz ve bir Azure sanal ağına dağıtıldığınız bir tümleştirme hizmet ortamı (Ise) gibi genel bir bölgedir. Bir İmKB'de mantık uygulamaları çalıştırmak, genel bir Azure bölgesinde mantık uygulamaları çalıştırmaya benzer, bu da olağanüstü durum kurtarma stratejinizin her iki senaryoiçin de geçerli olabileceği anlamına gelir. Ancak, ISEs'in yalnızca ISE'ler tarafından kullanılabilen kaynaklara erişimi yapılandırma gibi başka hususları vardır.

> [!NOTE]
> Mantık uygulamanız, bir entegrasyon hesabında depolanan ticaret ortakları, anlaşmalar, şemalar, haritalar ve sertifikalar gibi B2B yapıları ile de çalışıyorsa, hem entegrasyon hesabınız hem de mantık uygulamalarınızın aynı konumu belirtmesi gerekir.

Bu olağanüstü durum kurtarma stratejisi, azure mantık uygulamalarının da kullanılabildiği alternatif bir konumda, birincil mantık uygulamanızı bekleme veya yedekleme mantığı uygulamasına [*geçemeyecek*](https://en.wikipedia.org/wiki/Failover) şekilde ayarlamaya odaklanır. Bu şekilde, birincil kayıplar, kesintiler veya başarısızlıklar uğrarsa, ikincil iş alabilir. Bu strateji, ikincil mantık uygulamanızın ve bağımlı kaynaklarınızın alternatif konumda zaten dağıtılmış ve hazır olduğunu gerektirir.

İyi DevOps uygulamalarını takip ediyorsanız, mantıksal uygulamalarınızı ve bunların bağımlı kaynaklarını tanımlamak ve dağıtmak için [Azure Kaynak Yöneticisi şablonlarını](../azure-resource-manager/management/overview.md) zaten kullanırsınız. Kaynak Yöneticisi şablonları, size tek bir dağıtım tanımı nı kullanma ve ardından her dağıtım hedefi için kullanılacak yapılandırma değerlerini sağlamak için parametre dosyalarını kullanma olanağı sağlar. Bu özellik, aynı mantık uygulamasını geliştirme, test ve üretim gibi farklı ortamlara dağıtabileceğiniz anlamına gelir. Aynı mantık uygulamasını, eşleştirilmiş bölgeleri kullanan olağanüstü durum kurtarma stratejilerini destekleyen farklı Azure bölgelerine veya ISE'lere de [dağıtabilirsiniz.](../best-practices-availability-paired-regions.md)

Başarısız strateji için, mantık uygulamalarınız ve konumlarınız aşağıdaki gereksinimleri karşılamalıdır:

* İkincil mantık uygulaması örneği, birincil mantık uygulaması örneğiyle aynı uygulamalara, hizmetlere ve sistemlere erişebilir.

* Her iki mantık uygulaması örnekleri aynı ana bilgisayar türüne sahiptir. Bu nedenle, her iki örnek de küresel çok kiracılı Azure'daki bölgelere dağıtılır veya her iki örnek de, mantık uygulamalarınızın bir Azure sanal ağındaki kaynaklara doğrudan erişmelerine izin veren ISE'lere dağıtılır. En iyi uygulamalar ve BCDR için eşleştirilmiş bölgeler hakkında daha fazla bilgi için Bkz. [İş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeler.](../best-practices-availability-paired-regions.md)

  Örneğin, birincil mantık uygulaması Bir İmKB'de çalıştığında ve [İmKB sürümünde konektörler](../connectors/apis-list.md#ise-connectors), Azure sanal ağındaki kaynakları aramak için HTTP eylemleri veya her ikisi de kullandığında, hem birincil hem de ikincil konumlar ISE olmalıdır. Bu senaryoda, ikincil mantık uygulamanız da birincil mantık uygulaması olarak ikincil konumda benzer bir kuruluma sahip olmalıdır.

  > [!NOTE]
  > Daha gelişmiş senaryolar için, hem çok kiracılı Azure'u hem de Birİm'yi konum olarak karıştırabilirsiniz. Ancak, [bir İmKB'de ve çok kiracılı Azure'da mantık uygulamalarının nasıl çalıştırışyaptığı arasındaki farkları](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#difference)göz önünde bulundurun ve anladığınızdan emin olun.

* ISE kullanıyorsanız, bunların ölçeklendirildiklerinden veya yükü işlemek için [yeterli kapasiteye sahip olduğundan emin olun.](../logic-apps/ise-manage-integration-service-environment.md#add-capacity)

#### <a name="example-multi-tenant-azure"></a>Örnek: Çok kiracılı Azure

Bu örnek, bu senaryo için genel çok kiracılı Azure'daki ayrı bölgelere dağıtılan birincil ve ikincil mantık uygulaması örneklerini gösterir. Tek bir [Kaynak Yöneticisi şablonu,](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) hem mantık uygulama örneklerini hem de bu mantık uygulamalarının gerektirdiği bağımlı kaynakları tanımlar. Ayrı parametre dosyaları, her dağıtım konumu için kullanılacak yapılandırma değerlerini belirtir:

![Ayrı konumlarda birincil ve ikincil mantık uygulaması örnekleri](./media/business-continuity-disaster-recovery-guidance/primary-secondary-locations.png)

#### <a name="example-integration-service-environment"></a>Örnek: Entegrasyon hizmet ortamı

Bu örnek, önceki birincil ve ikincil mantık uygulama örneklerini gösterir, ancak ISEs'leri ayırmak için dağıtılır. Tek bir Kaynak Yöneticisi şablonu hem mantık uygulama örneklerini, bu mantık uygulamalarının gerektirdiği bağımlı kaynakları ve dağıtım konumları olarak ISE'leri tanımlar. Ayrı parametre dosyaları, her konumda dağıtım için kullanılacak yapılandırma değerlerini tanımlar:

![Farklı konumlardaki birincil ve ikincil mantık uygulamaları](./media/business-continuity-disaster-recovery-guidance/primary-secondary-locations-ise.png)

<a name="resource-connections"></a>

## <a name="connections-to-resources"></a>Kaynaklara bağlantılar

Azure Logic Apps [yerleşik tetikleyiciler ve eylemlerin yanı sıra](../connectors/apis-list.md) mantık uygulamanızın Azure Depolama hesapları, SQL Server veritabanları, Office 365 Outlook e-posta hesapları gibi diğer uygulamalar, hizmetler, sistemler ve diğer kaynaklarla çalışmak için kullanabileceği yüzlerce yönetilen bağlayıcı sağlar. Mantık uygulamanızın bu kaynaklara erişmesi gerekiyorsa, bu kaynaklara erişimi doğrulayan bağlantılar oluşturursunuz. Her bağlantı, belirli bir konumda bulunan ve diğer konumlardaki kaynaklar tarafından kullanılabilen ayrı bir Azure kaynağıdır.

Olağanüstü durum kurtarma stratejiniz için, mantık uygulama örneklerinize göre bağımlı kaynakların bulunduğu yerleri göz önünde bulundurun:

* Birincil örneğiniz ve bağımlı kaynaklarınız farklı konumlarda bulunur. Bu durumda, ikincil örneğiniz aynı bağımlı kaynaklara veya uç noktalara bağlanabilir. Ancak, ikincil örneğiniz için özel olarak bağlantılar oluşturmanız gerekir. Bu şekilde, birincil konumunuz kullanılamıyorsa, ikincil bağlantınız etkilenmez.

  Örneğin, birincil mantık uygulamanızın Salesforce gibi harici bir hizmete bağladığını varsayalım. Genellikle, harici hizmetin kullanılabilirliği ve konumu mantık uygulamanızın kullanılabilirliğinden bağımsızdır. Bu durumda, ikincil örneğiniz aynı hizmete bağlanabilir, ancak kendi bağlantısı olmalıdır.

* Hem birincil örneğiniz hem de bağımlı kaynaklarınız aynı konumda bulunur. Bu durumda, ikincil örneğinizin bu kaynaklara erişebilmeleri için bağımlı kaynakların farklı bir konumda yedeklemeleri veya çoğaltılmış sürümleri olmalıdır.

  Örneğin, birincil mantık uygulamanızın Azure SQL Veritabanı gibi aynı konum veya bölgede bulunan bir hizmete bağladığını varsayalım. Bu bölgenin tamamı kullanılamıyorsa, o bölgedeki Azure SQL Veritabanı hizmeti de büyük olasılıkla kullanılamaz. Bu durumda, ikincil örneğinizin bu veritabanına ayrı bir bağlantıyla birlikte çoğaltılmış veya yedek bir veritabanı kullanmasını istersiniz.

<a name="on-premises-data-gateways"></a>

## <a name="on-premises-data-gateways"></a>Şirket içi veri ağ geçitleri

Mantık uygulamanız çok kiracılı Azure'da çalışıyorsa ve SQL Server veritabanları gibi şirket içi kaynaklara erişmesi gerekiyorsa, [şirket içi veri ağ geçidini](../logic-apps/logic-apps-gateway-install.md) yerel bir bilgisayara yüklemeniz gerekir. Daha sonra Azure portalında bir veri ağ geçidi kaynağı oluşturabilirsiniz, böylece kaynakla bağlantı oluşturduğunuzda mantık uygulamanız ağ geçidini kullanabilir.

Veri ağ geçidi kaynağı, tıpkı mantık uygulama kaynağınız gibi bir konum veya Azure bölgesiyle ilişkilidir. Olağanüstü durum kurtarma stratejinizde, veri ağ geçidinin mantık uygulamanızın kullanabileceği şekilde kullanılabilir kaldığından emin olun. Birden çok ağ geçidi yüklemeniz olduğunda [ağ geçidiniz için yüksek kullanılabilirlik sağlayabilirsiniz.](../logic-apps/logic-apps-gateway-install.md#high-availability)

> [!NOTE]
> Mantık uygulamanız bir entegrasyon hizmeti ortamında (Ise) çalışıyorsa ve şirket içi veri kaynakları için yalnızca Ise sürümünde konektörler kullanıyorsa, İmKB bağlayıcıları şirket içi kaynağa doğrudan erişim sağladığından veri ağ geçidine ihtiyacınız yoktur.
>
> İstediğiniz şirket içi kaynak için İmKB sürümünde konnektör yoksa, mantık uygulamanız İmKB'niz değil, küresel çok kiracılı Azure'da çalışan bir İmKB olmayan bağlayıcıyı kullanarak bağlantıyı oluşturmaya devam edebilir. Ancak, bu bağlantı şirket içi veri ağ geçidi gerektirir.

<a name="roles"></a>

## <a name="active-active-versus-active-passive-roles"></a>Etkin-aktif ve aktif-pasif roller

Birincil ve ikincil konumlarınızı, bu konumlardaki mantık uygulama örneklerinizin bu rolleri oynayabilmesi için ayarlayabilirsiniz:

| Birincil-ikincil rol | Açıklama |
|------------------------|-------------|
| *Etkin* | Her iki konumdaki birincil ve ikincil mantık uygulaması örnekleri, aşağıdaki desenlerden birini izleyerek istekleri etkin bir şekilde işler: <p><p>- *Yük dengesi*: Her iki örneğin de bir bitiş noktası nı dinlemesini ve her bir örneğin yük bakiyesi trafiğini gerektiği gibi dinlemesini sağlayabilirsiniz. <p>- *Rakip tüketiciler*: Her iki örneğin de rakip tüketici gibi davranmasını sağlayabilirsiniz, böylece örnekler kuyruktaki iletiler için rekabet eder. Bir örnek başarısız olursa, diğer örnek iş yükünü devralır. |
| *Aktif-pasif* | Birincil mantık uygulaması örneği etkin olarak tüm iş yükünü işlerken, ikincil örnek pasif (devre dışı veya etkin değil) ise. İkincil, birincil etkin veya arıza nedeniyle çalışmıyor ve etkin örnek olarak iş yükünü devralır bir sinyal bekler. |
| Kombinasyon | Bazı mantık uygulamaları etkin-etkin bir rol oynarken, diğer mantık uygulamaları etkin-pasif bir rol oynar. |
|||

<a name="active-active-examples"></a>

### <a name="active-active-examples"></a>Etkin-etkin örnekler

Bu örnekler, her iki mantık uygulaması örneğinin de istekleri veya iletileri etkin olarak işlediği etkin etkin kurulumu gösterir. Başka bir sistem veya hizmet, istekleri veya iletileri örneğin aşağıdaki seçeneklerden biri gibi örnekler arasında dağıtır:

* Trafiği yönlendirir donanım parçası gibi "fiziksel" yük dengeleyicisi

* [Azure Yük Dengeleyicisi](../load-balancer/load-balancer-overview.md) veya Azure API Yönetimi gibi "yumuşak" bir yük dengeleyicisi. [Azure API Management](../api-management/api-management-key-concepts.md) API Yönetimi ile, gelen trafiği nasıl yükleyebileceğinizi belirleyen ilkeler belirtebilirsiniz. Veya, örneğin [Azure Hizmet Veri Servisi](../service-bus-messaging/service-bus-messaging-overview.md)gibi durum izlemeyi destekleyen bir hizmeti kullanabilirsiniz.

  Bu örnekte öncelikle Azure Yük Dengeleyicisi gösterse de, senaryonuzun gereksinimlerine en uygun seçeneği kullanabilirsiniz:

  ![Yük dengeleyicisi veya devlet hizmeti kullanan "etkin" kurulum](./media/business-continuity-disaster-recovery-guidance/active-active-setup-load-balancer.png)

* Her mantık uygulaması örneği bir tüketici gibi davranır ve her iki örneğin de kuyruktan gelen iletiler için rekabet var:

  !["Rakip tüketiciler" kullanan "etkin" kurulum](./media/business-continuity-disaster-recovery-guidance/active-active-competing-consumers-pattern.png)

<a name="active-passive-examples"></a>

### <a name="active-passive-examples"></a>Etkin-pasif örnekler

Bu örnek, birincil mantık uygulaması örneğinin bir konumda etkin olduğu etkin-pasif kurulumu, ikincil örneğin ise başka bir konumda etkin liğini koruduğunu gösterir. Birincil bir aksaklık veya başarısızlıkla karşılaşılırsa, iş yükünü üstlenmek için ikincil komut dosyasını etkinleştiren bir komut dosyası çalıştıran bir işleç çalıştırabilirsiniz.

!["Rakip tüketicileri" kullanan "aktif-pasif" kurulum](./media/business-continuity-disaster-recovery-guidance/active-passive-setup.png)

<a name="active-active-active-passive-examples"></a>

### <a name="combination-with-active-active-and-active-passive"></a>Aktif-aktif ve aktif-pasif ile kombinasyon

Bu örnek, birincil konumun hem etkin mantık uygulaması örneklerine sahip olduğu, ikincil konumun ise etkin-pasif mantık uygulama örneklerine sahip olduğu birleşik bir kurulum gösterir. Birincil konum bir aksaklık veya başarısızlıkla karşılanırsa, zaten kısmi bir iş yükünü işleyen ikincil konumdaki etkin mantık uygulaması tüm iş yükünü devralabilir.

* Birincil konumda, etkin bir mantık uygulaması iletiler için Azure Hizmet Veri Hizmeti Veri Mes'i sırasını dinlerken, başka bir etkin mantık uygulaması Office 365 Outlook yoklama tetikleyicisini kullanarak e-postaları denetler.

* İkincil konumda, etkin bir mantık uygulaması, aynı Servis Veri Servisi kuyruğundan gelen iletileri dinleyerek ve rekabet ederek birincil konumdaki mantık uygulamasıyla çalışır. Bu arada, pasif bir etkin olmayan mantık uygulaması, birincil konum kullanılamadığında ancak e-postaları yeniden okumaktan kaçınmak için *devre dışı* bırakıldığında e-postaları denetlemek için beklemede bekler.

![Nüks tetikleyicilerini kullanan "aktif-pasif" ve "aktif-pasif" kombinasyonu](./media/business-continuity-disaster-recovery-guidance/combo-active-active-active-passive-setup.png)

<a name="state-history"></a>

## <a name="logic-app-state-and-history"></a>Mantık uygulaması durumu ve geçmişi

Mantık uygulamanız tetiklendiğinde ve çalışmaya başladığında, uygulamanın durumu uygulamanın başladığı konumda depolanır ve başka bir konuma aktarılamaz. Bir hata veya bozulma olursa, devam eden iş akışı örnekleri terk edilir. Birincil ve ikincil konumlar ayarlandığında, ikincil konumda yeni iş akışı örnekleri çalışmaya başlar.

<a name="reduce-abandoned-in-progress"></a>

### <a name="reduce-abandoned-in-progress-instances"></a>Terk edilmiş devam eden örnekleri azaltın

Terk edilmiş devam eden iş akışı örneklerinin sayısını en aza indirmek için, uygulayabileceğiniz çeşitli ileti desenleri arasından seçim yapabilirsiniz:

* [Sabit yönlendirme kayma deseni](https://docs.microsoft.com/biztalk/esb-toolkit/message-routing-patterns#routing-slip)

  İş sürecini daha küçük aşamalara ayıran bu kurumsal ileti deseni. Her aşama için, o aşamanın iş yükünü işleyen bir mantık uygulaması ayarlarsınız. Mantık uygulamalarınız birbirleriyle iletişim kurmak için Azure Servis Veri Hizmetleri kuyrukları veya konuları gibi eşzamanlı bir ileti protokolü kullanır. Bir işlemi daha küçük aşamalara böldüğünüzde, başarısız bir mantık uygulaması örneğine takılıp kalabilecek iş işlemlerinin sayısını azaltırsınız. Bu desen hakkında daha genel bilgi için, [Bkz. Kurumsal tümleştirme desenleri - Yönlendirme fişi.](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RoutingTable.html)

  Bu örnek, her mantık uygulamasının bir aşamayı temsil ettiği ve işlemdeki bir sonraki mantık uygulamasıyla iletişim kurmak için Bir Hizmet Veri Servisi kuyruğu kullandığı bir yönlendirme fişi deseni gösterir.

  ![Azure Servis Veri Hizmetleri sıralarını kullanarak birbirleriyle iletişim kuran mantıksal uygulamalar tarafından temsil edilen aşamalara bir iş sürecini bölme](./media/business-continuity-disaster-recovery-guidance/fixed-routing-slip-pattern.png)

  Hem birincil hem de ikincil mantık uygulaması örnekleri konumlarında aynı yönlendirme fişi deseni takip ediyorsa, bu örnekler için [etkin-etkin roller](#roles) ayarlayarak [rakip tüketici deseni](https://docs.microsoft.com/azure/architecture/patterns/competing-consumers) uygulayabilirsiniz.

* [İşlem yöneticisi (broker) deseni](https://www.enterpriseintegrationpatterns.com/patterns/messaging/ProcessManager.html)

* [Zaman ayarı deseni olmadan peek-lock](https://social.technet.microsoft.com/wiki/contents/articles/50022.azure-service-bus-how-to-peek-lock-a-message-from-queue-using-azure-logic-apps.aspx)

<a name="access-trigger-runs-history"></a>

### <a name="access-to-trigger-and-runs-history"></a>Tetikleyiciye erişim ve geçmişi çalıştırın

Mantık uygulamanızın geçmiş iş akışı yürütmeleri hakkında daha fazla bilgi almak için uygulamanın tetikleyicisini inceleyebilir ve geçmişi çalıştırabilirsiniz. Bir mantık uygulamasının geçmiş yürütme geçmişi, mantık uygulamasının çalıştığı aynı konumda veya bölgede depolanır, bu da bu geçmişi farklı bir konuma geçiremezsiniz anlamına gelir. Birincil örneğiniz ikincil bir örnek üzerinde başarısız olursa, yalnızca her örneğin tetikleyicisine erişebilir ve bu örneklerin çalıştığı ilgili konumlarda geçmişi çalıştırabilirsiniz. Ancak, mantıksal uygulamalarınızı bir Azure Log Analytics çalışma alanına tanılama etkinlikleri göndermek üzere ayarlayarak mantık uygulamanızın geçmişi hakkında konum-agnostik bilgiler alabilirsiniz. Daha sonra, birden çok konumda çalışan mantık uygulamaları arasında sistem durumunu ve geçmişi gözden geçirebilirsiniz.

<a name="trigger-types-guidance"></a>

## <a name="trigger-type-guidance"></a>Tetikleyici türü kılavuzu

Mantıksal uygulamalarınızda kullandığınız tetikleyici türü, olağanüstü durum kurtarma stratejinizde konumlar arasında mantık uygulamaları nasıl ayarlayabileceğinize dair seçeneklerinizi belirler. Mantık uygulamalarında kullanabileceğiniz tetikleyici türleri şunlardır:

* [Yineleme tetikleyicisi](#recurrence-trigger)
* [Yoklama tetikleyicisi](#polling-trigger)
* [İstek tetikleyicisi](#request-trigger)
* [Webhook tetikleme](#webhook-trigger)

<a name="recurrence-trigger"></a>

### <a name="recurrence-trigger"></a>Yineleme tetikleyicisi

**Yineleme** tetikleyicisi belirli bir hizmetten veya bitiş noktasından bağımsızdır ve yalnızca belirli bir zamanlamaya ve örneğin başka bir ölçüte dayalı olarak yangınları kolaylaştırır:

* Her 10 dakikada bir olduğu gibi sabit bir frekans ve aralık
* Her ayın son Pazartesi günü saat 17:00'de olduğu gibi daha gelişmiş bir program

Mantık uygulamanız bir Yineleme tetikleyicisi ile başladığında, birincil ve ikincil mantık uygulama örneklerinizi [etkin-pasif rollerle](#roles)ayarlamanız gerekir. Bir aksaklık veya felaketten sonra bir iş sürecini geri getirmek için hedef süreyi ifade eden *kurtarma süresi hedefini* (RTO) azaltmak için, mantık uygulama örneklerinizi [etkin-pasif roller](#roles) ve [pasif-aktif rollerin](#roles)bir kombinasyonuyla ayarlayabilirsiniz. Bu kurulumda, zamanlamayı konumlara bölersiniz.

Örneğin, her 10 dakikada bir çalışması gereken bir mantık uygulamanız olduğunu varsayalım. Mantık uygulamalarınızı ve konumlarınızı, birincil konum kullanılamaz hale gelirse, ikincil konumun çalışmayı devralabilmesi için ayarlayabilirsiniz:

![Nüks tetikleyicilerini kullanan "aktif-pasif" ve "pasif-aktif" kombinasyonu](./media/business-continuity-disaster-recovery-guidance/combo-active-passive-passive-active-setup.png)

* Birincil konumda, bu mantık uygulamaları için [etkin-pasif roller](#roles) ayarlayın:

  * *Etkin* etkin leştirilmiş mantık uygulaması için, Yineleme tetikleyicisini saatin en üstünden başlayıp her 20 dakikada bir tekrarlayacak şekilde ayarlayın, örneğin, örneğin, 09:00, 09:20, ve benzeri.

  * *Pasif* devre dışı bırakılmış mantık uygulaması için, Yineleme tetikleyicisini aynı zamanlamaya ayarlayın, ancak saatin 10 dakika sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağısı ayarlayın ve örneğin, 09:10, 09:30 ve benzeri her 20 dakikada bir tekrarlayın.

* İkincil konumda, bu mantık uygulamaları için [pasif-etkin](#roles) ayarlayın:

  * *Pasif* devre dışı bırakılmış mantık uygulaması için, Yineleme tetikleyicisini birincil konumdaki etkin mantık uygulamasıyla aynı zamanlamaya ayarlayın, bu da saatin en üstündedir ve örneğin her 20 dakikada bir tekrarlayın, örneğin, 09:00, 09:10 ve benzeri.

  * *Etkin* etkin leştirilmiş mantık uygulaması için, Yineleme tetikleyicisini birincil konumdaki pasif mantık uygulamasıyla aynı zamanlamaya ayarlayın, bu da saatin 10 dakika sında başlayıp her 20 dakikada bir tekrarlamaktır, örneğin, örneğin, 9:10, 09:20, ve benzeri.

Şimdi, birincil konumda rahatsız edici bir olay gerçekleşirse, alternatif konumdaki pasif mantık uygulamasını etkinleştirin. Bu şekilde, hatanın bulunması zaman alıyorsa, bu kurulum bu gecikme sırasında kaçırılan yineleme sayısını sınırlar.

<a name="polling-trigger"></a>

### <a name="polling-trigger"></a>Yoklama tetikleyicisi

İşleme için yeni verilerin belirli bir hizmetten veya bitiş noktasından kullanılabilir olup olmadığını düzenli olarak denetlemek için, mantık uygulamanız sabit bir yineleme zamanlamasına bağlı olarak hizmeti veya bitiş noktasını tekrar tekrar çağıran bir *yoklama* tetikleyicisi kullanabilir. Hizmetin veya bitiş noktasının sağladığı veriler aşağıdakitürlerden herhangi biri olabilir:

* Her zaman okunabilen verileri açıklayan statik veriler
* Okuduktan sonra artık kullanılamayan verileri açıklayan geçici veriler

Aynı verileri tekrar tekrar okumaktan kaçınmak için, mantık uygulamanızın istemci tarafında veya sunucu, hizmet veya sistem tarafında durumu koruyarak hangi verilerin daha önce okunduğunu hatırlaması gerekir.

* İstemci tarafı durum kullanımıyla çalışan mantıksal uygulamalar, durumu koruyabilen tetikleyiciler.

  Örneğin, bir e-posta gelen kutusundan yeni bir ileti okuyan bir tetikleyici, tetikleyicinin en son okunan iletiyi hatırlayabını gerektirir. Bu şekilde, tetikleyici mantık uygulamasını yalnızca bir sonraki okunmamış ileti geldiğinde başlatır.

* Sunucu, hizmet veya sistem tarafı durumuyla çalışan mantıksal uygulamalar, sunucu, hizmet veya sistem tarafında bulunan özellik değerlerini veya ayarlarını kullanır.

  Örneğin, veritabanından bir satır okuyan sorgu tabanlı tetikleyici, satırın `isRead` `FALSE`'da ' olarak ayarlanmış bir sütuna sahip olmasını gerektirir. Tetikleyici her satırı okuduğunda, mantık uygulaması sütunu `isRead` 'dan `FALSE` 'a `TRUE`değiştirerek bu satırı güncelleştirir.

  Bu sunucu tarafı yaklaşımı, mantık uygulaması iletiyi işlerken bir tetikleyicinin bir iletiyi okuyup kilitleyebileceği semantik sıraya giren Servis Veri Servisi kuyrukları veya konular için de benzer şekilde çalışır. Mantık uygulaması işleme yi bitirdiğinde, tetikleyici sıraveya konudaki iletiyi siler.

Olağanüstü durum kurtarma açısından bakıldığında, mantık uygulamanızın birincil ve ikincil örneklerini ayarladığınızda, mantık uygulamanızın istemci tarafında veya sunucu tarafında durumunu izleyip izlemediğine bağlı olarak bu davranışları hesaba kattığından emin olun:

* İstemci tarafı durumuyla çalışan bir mantık uygulaması için, mantık uygulamanızın aynı iletiyi birden fazla kez okumadığından emin olun. Belirli bir zamanda yalnızca bir konum etkin bir mantık uygulaması örneğine sahip olabilir. Birincil örnek alternatif konuma geçene kadar alternatif konumdaki mantık uygulaması örneğinin etkin olmadığından veya devre dışı bırakıldığından emin olun.

  Örneğin, Office 365 Outlook tetikleyicisi istemci tarafı durumunu korur ve yinelenen bir okumayı önlemek için en son okunan e-postanın zaman damgasını izler.

* Sunucu tarafı durumuyla çalışan bir mantık uygulaması için, mantıksal uygulama örneklerinizi, rakip tüketiciler olarak çalıştıkları [etkin etkin rolleri](#roles) veya alternatif örneğin alternatif konuma başarısız olana kadar beklediği [etkin-pasif rolleri](#roles) oynayacak şekilde ayarlayabilirsiniz.

  Örneğin, Azure Hizmet Veri Hizmeti sırası gibi bir ileti kuyruğundan okuma, diğer istemcilerin aynı iletileri okumasını önlemek için iletilerde kilitler içerdiğinden, sunucu tarafı durumunu kullanır.

  > [!NOTE]
  > Mantık uygulamanızın iletileri belirli bir sırada okuması gerekiyorsa, örneğin, Bir Hizmet Veri Servisi kuyruğundan, rakip tüketici deseni ancak [ *sıralı konvoy* deseni](https://docs.microsoft.com/azure/architecture/patterns/sequential-convoy)olarak da bilinen Servis Veri Servisi oturumlarıyla birleştirildiğinde kullanabilirsiniz. Aksi takdirde, mantık uygulama örneklerinizi etkin-pasif rollerle ayarlamanız gerekir.

<a name="request-trigger"></a>

### <a name="request-trigger"></a>İstek tetikleyicisi

**İstek** tetikleyicisi, mantık uygulamanızı diğer uygulamalardan, hizmetlerden ve sistemlerden çağrılabilir hale getirir ve genellikle bu özellikleri sağlamak için kullanılır:

* Başkalarının arayabildiği mantık uygulamanız için doğrudan BIR REST API

  Örneğin, diğer mantık uygulamalarının **Çağrı iş akışını** - Mantık Uygulamaları eylemini kullanarak tetikleyiciyi çağırabilmesi için mantık uygulamanızı başlatmak için İstek tetikleyicisini kullanın.

* Mantık uygulamanız için [bir webhook](#webhook-trigger) veya geri arama mekanizması

* Belirli bir görevi gerçekleştiren bir PowerShell komut dosyası kullanarak, örneğin mantık uygulamanızı aramak için kullanıcı işlemlerini veya yordamları el ile çalıştırabileceğiniz bir yol

Olağanüstü durum kurtarma açısından bakıldığında, İstek tetikleyicisi pasif bir alıcıdır, çünkü mantık uygulaması herhangi bir iş yapmaz ve başka bir hizmet veya sistem tetikleyiciyi açıkça çağırana kadar bekler. Pasif bir bitiş noktası olarak, birincil ve ikincil örneklerinizi şu şekilde ayarlayabilirsiniz:

* [Etkin- etkin](#roles): Her iki örnekde de istekleri veya çağrıları etkin bir şekilde işler. Arayan veya yönlendirici, bu örnekler arasındaki trafiği dengeler veya dağıtır.

* [Etkin-pasif](#roles): Yalnızca birincil örnek etkindir ve tüm işi işlerken, ikincil örnek birincil bozulma veya başarısızlık la karşılanıncaya kadar bekler. Arayan veya yönlendirici ikincil örneği ne zaman arayacağını belirler.

Önerilen bir mimari olarak, İstek tetikleyicilerini kullanan mantık uygulamaları için Azure API Yönetimi'ni proxy olarak kullanabilirsiniz. API Yönetimi [yerleşik bölgeler arası esneklik ve trafiği birden çok uç noktaya yönlendirme olanağı](https://docs.microsoft.com/azure/api-management/api-management-howto-deploy-multi-region)sağlar.

<a name="webhook-trigger"></a>

### <a name="webhook-trigger"></a>Webhook tetikleme

*Webhook* tetikleyicisi, mantık uygulamanızın bu hizmete bir *geri arama URL'si* geçirerek bir hizmete abone olmasını sağlar. Mantık uygulamanız daha sonra bu hizmet bitiş noktasında belirli bir olayın gerçekleşmesini dinleyebilir ve bekleyebilir. Olay gerçekleştiğinde, servis, daha sonra mantık uygulamasını çalıştıran geri arama URL'sini kullanarak webhook tetikleyicisini çağırır. Etkinleştirildiğinde, webhook tetikleyicisi hizmete abone dir. Devre dışı bırakıldığında, tetikleyici hizmetten aboneliğini iptal eder.

Olağanüstü durum kurtarma açısından bakıldığında, yalnızca bir örnek abone bitiş noktasından olay veya ileti almalısınız, çünkü etkin-pasif rolleri oynamak için webhook tetikleyicileri kullanan birincil ve ikincil örnekleri ayarlayın.

## <a name="assess-primary-instance-health"></a>Birincil örnek sağlık değerlendirmesini

Olağanüstü durum kurtarma stratejinizin çalışması için, çözümünüz bu görevleri gerçekleştirmek için yollara ihtiyaç duyar:

* [Birincil örneğin kullanılabilirliğini denetleme](#check-primary-availability)
* [Birincil örneğin durumunu izleme](#monitor-primary-health)
* [İkincil örneği etkinleştirme](#activate-secondary)

Bu bölümde, doğrudan veya kendi tasarımınız için temel olarak kullanabileceğiniz bir çözüm açıklanmaktadır. Bu çözüm için üst düzey bir görsel bakış:

![Birincil konumda ki sistem durumu denetimi mantığı uygulamasını izleyen izleme örgütü mantığı uygulaması oluşturun](./media/business-continuity-disaster-recovery-guidance/check-location-health-watchdog.png)

<a name="check-primary-availability"></a>

### <a name="check-primary-instance-availability"></a>Birincil örnek kullanılabilirliğini denetleme

Birincil örneğin kullanılabilir olup olmadığını, çalıştırılıp çalışılmadığını ve çalışabildiğini belirlemek için, birincil örnekle aynı konumda bulunan bir "sistem durumu denetimi" mantık uygulaması oluşturabilirsiniz. Daha sonra bu sistem durumu denetimi uygulamasını alternatif bir konumdan arayabilirsiniz. Sistem durumu denetimi uygulaması başarılı bir şekilde yanıt verirse, o bölgedeki Azure Logic Apps hizmetinin temel altyapısı kullanılabilir ve çalışır. Sistem durumu denetimi uygulaması yanıt alamazsa, konumun artık sağlıklı olmadığını varsayabilirsiniz.

Bu görev için, bu görevleri gerçekleştiren temel bir sistem durumu denetimi mantığı uygulaması oluşturun:

1. İstek tetikleyicisini kullanarak izleme örgütü uygulamasından bir çağrı alır.

1. Yanıt eylemini kullanarak denetlenen mantık uygulamasının hala çalışıp çalışmadığını belirten bir durumla yanıt verin.

   > [!IMPORTANT]
   > Sistem durumu denetimi mantığı uygulaması, uygulamanın eşzamanlı olarak değil, eşzamanlı olarak yanıt vermesi için bir Yanıt eylemi kullanmalıdır.

1. İsteğe bağlı olarak, birincil konumun sağlıklı olup olmadığını daha fazla belirlemek için, bu konumdaki hedef mantık uygulamasıyla etkileşimde bulunan diğer hizmetlerin durumunu da hesaba katabilirsiniz. Sadece bu diğer hizmetler için de sağlık değerlendirmek için sağlık kontrol mantığı uygulaması genişletin.

<a name="monitor-primary-health"></a>

### <a name="create-a-watchdog-logic-app"></a>Watchdog mantık uygulaması oluşturma

Birincil örneğin durumunu izlemek ve sistem durumu denetimi mantığı uygulamasını aramak için alternatif bir *konumda*bir "izleme" mantığı uygulaması oluşturun. Örneğin, izleme izleme aracı mantığı uygulamasını, sistem durumu denetimi mantığını aramanın başarısız olması durumunda, izleme ekibinin hatayı ve birincil örneğin neden yanıt vermemesini araştırabilmeleri için operasyon ekibinize bir uyarı gönderebileceği şekilde ayarlayabilirsiniz.

> [!IMPORTANT]
> İzleme izleme mantığı uygulamanızın *birincil konumdan farklı*bir konumda olduğundan emin olun. Birincil konumdaki Logic Apps hizmeti sorunlarla karşılanırsa, izleme izleme mantıksal uygulamanız çalışmayabilir.

Bu görev için, ikincil konumda, bu görevleri gerçekleştiren bir izleme izleme mantığı uygulaması oluşturun:

1. Yineleme tetikleyicisini kullanarak sabit veya zamanlanmış bir yinelemeye göre çalıştırın.

   Yinelemeyi, kurtarma süresi hedefiniz (RTO) için tolerans düzeyinin altında bir değere ayarlayabilirsiniz.

1. Örneğin, HTTP eylemini kullanarak birincil konumdaki sistem durumu denetimi mantığı uygulamasını arayın:

<a name="activate-secondary"></a>

### <a name="activate-your-secondary-instance"></a>İkincil örneğini etkinleştirme

İkincil örneği otomatik olarak etkinleştirmek için, ikincil konumdaki uygun mantık uygulamalarını etkinleştirmek için [Azure Kaynak Yöneticisi bağlayıcısı](https://docs.microsoft.com/connectors/arm/) gibi yönetim API'sini çağıran bir mantık uygulaması oluşturabilirsiniz. Belirli sayıda hata olduktan sonra bu etkinleştirme mantığı uygulamasını aramak için izleme izleme uygulamanızı genişletebilirsiniz.

<a name="collect-diagnostic-data"></a>

## <a name="collect-diagnostic-data"></a>Tanılama verilerini toplama

Mantık uygulama çalıştırmalarınız için günlük işlemlerini ayarlayabilir ve ortaya çıkan tanı verilerini daha fazla işleme ve işleme için Azure Depolama, Azure Etkinlik Hub'ları ve Azure Log Analytics gibi hizmetlere gönderebilirsiniz.

* Bu verileri Azure Log Analytics ile kullanmak istiyorsanız, mantık uygulamanızın **Tanılama ayarlarını** ayarlayarak ve verileri birden çok Log Analytics çalışma alanlarına göndererek verileri hem birincil hem de ikincil konumlar için kullanılabilir hale getirebilirsiniz. Daha fazla bilgi için azure [monitör günlükleri ayarlama ve Azure Mantık Uygulamaları için tanılama verileri toplama](../logic-apps/monitor-logic-apps-log-analytics.md)'na bakın.

* Verileri Azure Depolama veya Azure Etkinlik Hub'larına göndermek istiyorsanız, coğrafi artıklık ayarlayarak verileri hem birincil hem de ikincil konumlar için kullanılabilir hale getirebilirsiniz. Daha fazla bilgi için şu makalelere bakın:<p>

  * [Azure Blob Depolama olağanüstü kurtarma ve hesap başarısızlığı](../storage/common/storage-disaster-recovery-guidance.md)
  * [Azure Olay Hub'ları coğrafi felaket kurtarma](../event-hubs/event-hubs-geo-dr.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure için esnekliğe genel bakış](https://docs.microsoft.com/azure/architecture/framework/resiliency/overview)
* [Belirli Azure hizmetleri için dayanıklılık denetim listesi](https://docs.microsoft.com/azure/architecture/checklist/resiliency-per-service)
* [Azure'da esneklik için veri yönetimi](https://docs.microsoft.com/azure/architecture/framework/resiliency/data-management)
* [Azure uygulamaları için yedekleme ve olağanüstü durum kurtarma](https://docs.microsoft.com/azure/architecture/framework/resiliency/backup-and-recovery)
* [Bölge genelindeki hizmet kesintisinden kurtarma](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region)
* [Azure hizmetleri için Microsoft Hizmet Düzeyi Sözleşmeleri (SLA'lar)](https://azure.microsoft.com/support/legal/sla/)
