---
title: Ticari özellikli bir Azure uzaktan Işleme uygulaması oluşturma
description: Azure uzaktan Işleme kullanarak ticari özellikli bir uygulama oluşturma stratejileri ve konuları
author: m-the-hoff
ms.author: v-michof
ms.date: 06/15/2020
ms.topic: tutorial
ms.openlocfilehash: fa1a49aeef8b86230dc1d5ea898832cfb1cee852
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85570000"
---
# <a name="tutorial-creating-a-commercial-ready-azure-remote-rendering-application"></a>Öğretici: ticari özellikli bir Azure uzaktan Işleme uygulaması oluşturma

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Ticari uygulamalar için oturum yönetimi
> * Faturalandırma için oturumları izleme
> * Oturum yükleme süresi etrafında Kullanıcı deneyimini iyileştirme
> * Ağ gecikmesi ile ilgili önemli noktalar

## <a name="prerequisites"></a>Önkoşullar

* Bu öğretici [öğretici: Azure uzaktan işleme ve model depolamanın güvenliğini sağlama](../security/security.md).

## <a name="introduction-to-commercial-readiness"></a>Ticari kullanıma hazır olma

Azure uzaktan Işleme, karma gerçeklik ile mümkün olan şeyleri genişletir. Temel bilgiler çözümünüze tümleştirildiğinde, çözümünüzün güvenli, ölçeklenebilir ve değer sunmaya hazırsa emin olmak için bazı ek hususlar vardır.

Bu modül size ticari uygulamanız için göz önünde bulundurmanız gerekebilecek bazı ek yetenekler sunar.

Sistemlerin genelinde mimari en iyi uygulamalarına yönelik kapsamlı bir genel bakış için şu adresi ziyaret edin:

* [Azure Mimari Merkezi](https://docs.microsoft.com/azure/architecture/)
* [Azure geliştiricileri için Başlarken Kılavuzu](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)

## <a name="analytics"></a>Analiz

Analiz araçlarını tümleştirmek, çözümünüzü yönetmenize, izlemenize ve iyileştirmenize yardımcı olabilir.

Kullanabileceğiniz analiz kaynaklarının kapsamlı bir listesi için şu adresi ziyaret edin:

* [Azure Analytics hizmetleri](https://azure.microsoft.com/product-categories/analytics/)

### <a name="tracking-usage-for-billing"></a>Faturalandırma için kullanımı izleme

Birden çok iç ekip veya dış istemci tarafından Azure uzaktan Işlemenin tüketimini izlemek, özellikle de çok kiracılı durumlarda önemli bir konudur.

Azure, bunu başarmak için kaynak etiketleme adlı bir hizmet sunarak Azure uzaktan Işleme hizmeti 'nin tüketimini her istemciyle ilişkilendirir.

Kaynak adlandırma ve etiketleme hakkında daha fazla bilgi için başlamak için iyi bir yer vardır:

* [Kaynak adlandırma ve etiketleme karar Kılavuzu](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

### <a name="diagnostics"></a>Tanılama

Windows için olay Izleme (ETW) ve olay Izleme günlüğü (ETL) gibi güçlü araçlar, uygulamanız içinde izleme olayları oluşturmayı kolaylaştırır ve ağ, içerik alımı, oturum, uygulama ve ticari bir çözüm dağıtımında ortaya çıkabilecek diğer sorunları tanılamanıza yardımcı olabilir.

Daha fazla bilgi için şu adresi ziyaret edin:

* [Istemci tarafı performans Izlemeleri oluşturma](https://docs.microsoft.com/azure/remote-rendering/how-tos/performance-tracing)
* [Windows için olay Izleme (ETW) verilerini toplama](https://docs.microsoft.com/visualstudio/profiling/how-to-collect-event-tracing-for-windows-etw-data)
* [Windows cihaz portalını kullanma: günlüğe kaydetme](https://docs.microsoft.com/windows/mixed-reality/using-the-windows-device-portal)

### <a name="usage-analysis"></a>Kullanım analizi

Azure Application Insights, kişilerin Azure uzaktan Işleme uygulamanızı nasıl kullandığını anlamanıza yardımcı olur. Uygulamanızı her güncelleştirdiğinizde, kullanıcıların ne kadar iyi çalıştığını değerlendirebilir ve çözümünüzü buna göre geliştirebilirsiniz. Bu bilgi ile, bir sonraki geliştirme döngüleriniz hakkında veri odaklı kararlar verebilirsiniz.

Daha fazla bilgi için şu adresi ziyaret edin:

* [Application Insights ile kullanım Analizi](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview)

## <a name="fast-startup-time-strategies"></a>Hızlı başlangıç zamanı stratejileri

Kullanım durumu, uygulama başlatmasının 3B model görüntülemeye hızlı bir şekilde başlatılmasını gerektirebilir. Örneğin, her şeyin güncel ve çalışır hale geldiğinden önemli bir toplantıda yer vardır. Bir CAD uygulaması ve karma gerçeklik arasındaki hızlı tasarım yinelemesinin, verimlilik açısından önemli olduğu bir CAD 3D model incelemesi, başka bir örnektir.

Azure uzaktan Işleme için önceden işlenmiş 3B modeller gerekir ve Azure Şu anda bir VM oluşturmak ve işleme için bir model yüklemek üzere birkaç dakika sürer. Bu işlemi olabildiğince sorunsuz ve hızlı hale getirmek, 3D model verilerinin ve ARR oturumunun bir süre önce hazırlanmasını gerektirir.

Burada paylaşılan öneriler şu anda standart Azure uzaktan Işleme 'nin bir parçası değildir, ancak daha hızlı başlangıç süreleri için bunları kendi kendinize uygulayabilirsiniz.

### <a name="initiate-early"></a>Erken Başlat

Başlangıç süresini azaltmak için, en basit çözüm, sanal makinenin oluşturulmasını ve başlatılmasını Kullanıcı iş akışında mümkün olduğunca erken taşıyamaktır. Bir strateji, bir ARR oturumunun gerekli olacağı bilindiğinde oturumu başlatmaktır. Bu, genellikle kullanıcının Azure uzaktan Işleme ile kullanmak üzere bir 3B modeli Azure Blob depolamaya yüklemeye başladığı zaman olur. Bu durumda, oturum oluşturma ve VM başlatması, 3D model karşıya yüklenirken her iki iş akışı da paralel olarak çalışacak şekilde aynı anda başlatılabilir.

Bu işlem, seçilen Azure Blob depolama giriş ve çıkış kapsayıcılarının Azure uzaktan Işleme oturumuyla aynı bölgesel veri merkezinde olduğundan emin olarak kolaylaştırılabilir.

### <a name="scheduling"></a>Zamanlama

Azure uzaktan Işleme için gelecekteki bir ihtiyacınız olduğunu biliyorsanız, Azure uzaktan Işleme oturumunun başlatılması için belirli bir tarih ve saat zamanlayabilirsiniz.

Bu seçenek, insanların her ikisi de bir 3B modeli yükleyebilecekleri ve gelecekte görüntülemek için zaman zamanlabileceği bir Web portalı üzerinden sunulabilir. Bu, standart veya Premium işleme gibi diğer tercihleri sormak için de iyi bir yerdir. Premium işleme, ideal boyutun otomatik olarak belirlenmesi veya Azure bölgesinin belirtilen sürede kullanılabilir VM 'Lere sahip olduğundan emin olmak için ihtiyaç duyduğu bir varlık karışımı göstermek için uygun olabilir.

### <a name="session-pooling"></a>Oturum havuzu oluşturma

En zorlu koşullarda, bir veya daha fazla oturumun her zaman oluşturulduğu ve başlatıldığı başka bir seçenek de oturum havuzdur. Bu işlem, isteyen bir kullanıcı tarafından hemen kullanılmak üzere bir oturum havuzu oluşturur. Bu yaklaşımın aşağı tarafında, VM başlatıldıktan sonra hizmet için faturalandırma başlatılır. Bir oturum havuzunu her zaman çalışır durumda tutmak, ancak analizler temel alınarak, en yüksek yükleri tahmin etmek ve oturum gerektiğinde tahmin etmek ve oturum havuzunu uygun şekilde artırma ve azaltma olanağı sağlamak için yukarıdaki zamanlama stratejisiyle birleştirilebilir.

Bu strateji aynı zamanda standart ve Premium oturumlardaki iki tür arasında geçiş yapmak için çok daha hızlı bir şekilde bir Premium karmaşıklık modelinin görüntülendiği, ardından bir standart içinde çalışıdığı durumlar gibi tek bir kullanıcı oturumunda bu seçimi iyileştirmenize yardımcı olur. Bu Kullanıcı oturumları oldukça uzunsa, önemli maliyet tasarrufları olabilir.

Azure uzaktan Işleme oturumları hakkında daha fazla bilgi için şu işlemleri inceleyin:

* [Remote Rendering Oturumları](https://docs.microsoft.com/azure/remote-rendering/concepts/sessions)

## <a name="standard-vs-premium-vm-routing-strategies"></a>Standart ve Premium VM yönlendirme stratejileri

Standart veya Premium bir sanal makinenin oluşturulup oluşturulmayacağını seçme gereksinimi, Kullanıcı deneyiminizi ve uçtan uca sisteminizin tasarlanmasıyla bir zorluk gösterir. Yalnızca Premium oturumlarını kullanmak bir seçenektir, ancak standart oturumlar çok daha az Azure işlem kaynaklarını kullanır ve Premium 'dan daha ucuz olur. Bu, mümkün olduğunda standart oturumları kullanmak için güçlü bir işlem sağlar ve yalnızca gerektiğinde Premium kullanır.

Burada, oturum seçimlerini yönetme isteğini ele almak için birkaç seçeneği en çok kapsamlı olarak paylaşıyoruz.

### <a name="use-only-standard-or-premium"></a>Yalnızca standart veya Premium kullanın

Gereksinimleriniz *her zaman* standart ve Premium arasındaki eşiğin altına düşecek şekilde, bu, kararınızı önemli ölçüde basitleştirir. Yalnızca standart kullanın. Yüklenen varlıkların toplam karmaşıklığı, standart bir oturum için çok karmaşık olarak reddedildiğinde, kullanıcı deneyiminin etkisinin önemli olmasına dikkat edin.

Benzer şekilde, standart ve Premium arasındaki eşiği aşmamak için kullanılan büyük bir bölümü beklediğinizi veya maliyet kullanım ihtimalinde bir anahtar faktörü değilse, her zaman Premium ' u seçerek basit tutma seçeneği de vardır.

### <a name="ask-the-user"></a>Kullanıcıdan sorun

Hem standart hem de Premium desteklemek istiyorsanız, örnek oluşturulacak sanal makine oturumu türünü belirlemenin en kolay yolu kullanıcıdan görüntülenecek 3D varlıkları seçtiğinde bunu sormasını belirlemektir. Bu yaklaşımdaki zorluk, kullanıcının 3B varlığın karmaşıklığını ve hatta görüntülenecek birden çok varlığı anlamasına gerek duyduğu bir sorundur. Genellikle, bu nedenle önerilmez. Kullanıcı yanlış seçerse ve standart seçerse, sonuçta elde edilen kullanıcı deneyimi inopportune bir anda tehlikeye girebilir.

### <a name="analyze-the-3d-model"></a>3B modeli çözümle

Başka bir görece basit yaklaşım, seçili 3B varlıkların karmaşıklığını analiz etmek değildir. Model karmaşıklığı standart için eşiğin altındaysa standart bir oturum başlatın, aksi takdirde Premium oturumu başlatın. Burada, sınama, bir standart oturumun karmaşıklık eşiğini aşabileceğinden birden çok modeli görüntülemek için son kullanılan bir oturum, farklı bir 3B varlık sırası için aynı oturumu sorunsuz bir şekilde kullanmayabilir.

### <a name="automatic-switching"></a>Otomatik anahtarlama

Standart ve Premium oturumlar arasında otomatik geçiş yapmak, bir sistem tasarımında oturum havuzunu de içeren çok sayıda fikir verebilir. Bu strateji, kaynak kullanımının daha iyi duruma getirilmesini sağlar. Kullanıcı modelleri görüntülenmek üzere yüklerken karmaşıklık belirlenir ve oturum havuzu hizmetinden doğru oturum boyutu istenir.

## <a name="working-with-networks"></a>Ağlarla çalışma

### <a name="diagnostics"></a>Tanılama

Azure uzaktan Işleme, düşük gecikme süresine sahip hızlı bir internet bağlantısı gerektirir. Kullanıcının ağının kalitesi, deneyim kalitesi üzerinde önemli bir etkiye sahip olabilir. İstemcilerinizin farklı ağ yapılandırmalarına sahip olma olasılığı yüksektir ve yalnızca ara sıra kötü ağ gecikme süresi, tanılama araçları anahtarıdır.  

Düzenli olarak yüksek kaliteli bir deneyim sunabildiğinizden emin olmak için, sunucu tarafı ve istemci tarafı analiz araçlarını Azure uzaktan Işleme uygulamalarınızla tümleştirmenizi öneririz. Bunun yapılması, istemcilerinizin karşılaştığı ağ sorunlarını tanılamak ve azaltmak için ihtiyacınız olan bilgileri sağlar.

### <a name="client-network-configurations"></a>İstemci ağ yapılandırması

Çok çeşitli kurumsal ortamlara dağıtılmış güçlü işbirliği çözümleri geliştirmenin en büyük zorluklarından biri, istemcilerinizin kullanabileceği farklı ağ topolojisi ve kurumsal güvenlik duvarı yapılandırmalarına hazırlanmaktadır.

Birçok kuruluş, LAN içindeki tüm eşler arası trafiği engeller. Bu, karma gerçeklik uygulamanızın bulunan tüm örnekleri arasında yerel bir paylaşılan oturum oluşturmak için otomatik LAN bulmanın basitleşme ve kolaylaştırılmış UX avantajlarından yararlanmanıza olanak sağlar.

Diğer olası hata noktaları, TCP/IP bağlantı noktalarının çoğunu engelleyen bant genişliği ve güvenlik duvarlarını kasıtlı olarak kısıtlamak için yapılandırılmış yönlendiricilerdir.

Bilinmeyen bir ağ üzerinde Azure uzaktan Işleme kullanmayı planlarken, şunları öneririz:

* Ağ hazırlığını değerlendirmek için Toplantı öncesi denetim listesi sağlayın.
* Uygun bölgesel veri merkezinin isteğe hizmet verebilir olduğundan emin olun.
* Her türlü sorunu tanılamak için çok zaman izin verin.
* Yüksek bant genişliğine sahip bir veri planına sahip bir mobil etkin noktayı yedekleme olarak taşıyın.

### <a name="end-to-end-bandwidth"></a>Uçtan uca bant genişliği

Azure uzaktan Işleme VM 'si ile son istemci arasında mevcut olabilecek her bir ağ için bant genişliği özelliklerini değerlendirmek önemlidir. Azure veri merkezi 'nden istemcinin ISS 'sine kadar olan ağ segmentinin, ISS 'den istemciye kadar bir sınırlandırma faktörüyle daha fazla olabileceğini aklınızda bulundurun. Blob Indirme hızı testi, bu tür sorunları tanılamaya yardımcı olmak için kullanılabilir.

### <a name="bandwidth-competition"></a>Bant genişliği yarışma

Karma Gerçeklik uygulamanızı tasarlarken, uygulamanın farklı özelliklerinin bant genişliği için Azure uzaktan Işleme ile rekabet edebilir olduğunu aklınızda bulundurun. En olası beklenmeyen örnek, tek bir odada bulunan çok sayıda katılımcının bir 3B varlığı görüntülemek için aynı anda ARR 'yi kullanması beklenen bir örnektir. Ağ veri akışının her bir bamasının, birleştirilmiş tüm ARR veri akışlarının toplam toplamını taşıması için kapasiteye sahip olması gerekir.

Diğer örneklerde, özellikle çok sayıda katılımcı olduğu ve sistem, orta yaklaşımda bir ses karıştırma sunucusunun aksine dağıtılmış eşler arası bir yaklaşım kullanan, diğer ilgili içerik ve sesli sohbet gibi akan video, eşzamanlı arka planda karşıya yükleme ve ses sohbeti sayılabilir.

Ağ Analizi hakkında daha fazla bilgi için şu görünümü görüntüleyin:

* [Azure Depolama Blobu Indirme hızı testi](https://www.azurespeed.com/Azure/Download)
* [Azure ağ gidiş dönüş gecikme süresi Istatistikleri](https://docs.microsoft.com/azure/networking/azure-network-latency)
* [Sunucu tarafı performans Izlemeleri](https://docs.microsoft.com/azure/remote-rendering/overview/features/performance-queries)
* [İstemci tarafı performans Izlemeleri](https://docs.microsoft.com/azure/remote-rendering/how-tos/performance-tracing)

## <a name="collaboration-considerations"></a>İşbirliği konuları

Azure uzaktan Işlemenin en değerli kullanımlardan bazıları aynı anda aynı 3B deneyimi görüntüleyen birden çok katılımcı arasındaki işbirliğini içerir. Bu paylaşılan oturumlarda, her katılımcının aynı ağ üzerinde aynı yerde olup olmadıklarından bağımsız olarak, her katılımcının benzersiz bir Azure uzaktan Işleme oturumuna ihtiyacı olacağını bilmek önemlidir.

Bu, her katılımcının farklı vanın noktalarından aynı deneyimi görebildiğinden, aynı 3B varlıkların her bir perspektiften aynı anda oluşturulmasını gerektiren bu doğru bir şeydir.

### <a name="multiple-azure-remote-rendering-sessions"></a>Birden çok Azure uzaktan Işleme oturumu

Azure uzaktan Işleme ile paylaşılan deneyimleri desteklemek istiyorsanız, ARR oturumlarını oluşturup yönetmek için yerleştirdiğiniz sistemlerin birden çok oturumu başlatmak için hazırlanması gerekir. Katılımcılar coğrafi olarak dağınık ise, bu oturumların farklı Azure veri merkezlerinde başlatılması gerekebilir.

Ayrıca sisteminiz, bir veya daha fazla katılımcının Azure uzaktan Işleme tarafından desteklenmeyen bir coğrafi bölgede olma olasılığını da Yönetmeli veya şu anda Azure uzaktan Işleme VM örnekleri mevcut değil.

Birden çok eş zamanlı oturumun bu yönetimi, oturum havuzu ve bu belgede ele alınan diğer stratejiler ile birleştirildiğinde daha kolay olabilir.

### <a name="azure-blob-storage-considerations"></a>Azure Blob depolama konuları

Tüm eşzamanlı ARR oturumları, dönüştürülen modelin görüntülenebilmesi için aynı SAS URI 'sine başvurabilir. Bu, istenen 3B varlıkların bir kez karşıya yüklenmesini ve dönüştürülmesini ve sonra tüm oturumlarda paylaşılmasını mümkün kılar. Bu durum özellikle katılımcılar birlikte konumlandırıldığında ve Azure Blob depolama ile ilgili performans endişeleri Azure uzaktan Işleme sunucusu ve Kullanıcı dışında farklı bir veri merkezinde yer alan aynı veri merkezini kullanırken geçerlidir.

3B varlıklar genellikle tek bir görüntüleme oturumu için karşıya yüklenirse ve sonra atıldığı gibi, bir tasarım incelemesi oturumunda, Azure Blob depolama alanının coğrafi bölgesi de daha az önemlidir.

Ancak, bir eğitim kullanım örneği gibi sürekli olarak kullanılacak 3B varlıklar için, Azure uzaktan Işleme kullanmayı planladığınız her bölgesel veri merkezinde blob depolamada kullanıma almaya yönelik 3B varlıkların tutulması önerilir. Bu, Azure depolama yedekliliği kullanılarak otomatikleştirilebilir. CDN genellikle bu amaçla kullanılır, ancak bu henüz Azure uzaktan Işleme için bir seçenek değildir.

Daha fazla bilgi için:

* [Karma Gerçeklik 'te paylaşılan deneyimler](https://docs.microsoft.com/windows/mixed-reality/shared-experiences-in-mixed-reality)
* [Azure depolama artıklığı](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

## <a name="managing-model-access"></a>Model erişimini yönetme

Azure uzaktan Işlemesini tamamen kullanmak 3B modellerin yönetilmesi için uçtan uca altyapıya dikkat etmeniz gerekir.

Azure uzaktan oluşturma kullanmanın bir avantajı, büyük 3B varlıkların görüntülenmeden önce doğrudan karma gerçeklik cihazına aktarılması gerekmez.  Ayrıca, 3B bir varlık karşıya yüklenip Azure uzaktan Işleme ile kullanılmak üzere dönüştürüldükten sonra, herhangi bir sayıda kullanıcı 3B modelin bu tek örneğini paylaşabilir.

### <a name="considerations-for-3d-model-access"></a>3B model erişimi ile ilgili konular

Model erişim stratejinize karar verirken bazı önemli noktalar aşağıda verilmiştir.

Tahmin edilen kullanım örneğine bağlı olarak, bir kullanıcının görüntüleme için 3B varlıkları seçmesine izin vermek için en iyi yeri veya yerlerin birleşimini belirleyin. Bazı yaygın seçenekler şunlardır:

* Doğrudan karma gerçeklik deneyimi içinde
* Yardımcı Web portalı aracılığıyla
* Bir yardımcı masaüstü veya mobil uygulamada

Kullanım örneği, aynı 3B varlığın birden çok kez karşıya yüklenebildiği kullanım desenlerine sahipse, arka uç, bir modelin birden çok sonraki seçim için yalnızca bir kez önceden işleneceği şekilde, ARR ile kullanım için dönüştürülen modelleri izler. Bir tasarım incelemesi örneği, bir ekibin ortak bir orijinal 3B varlığa erişebileceği bir örnektir. Her ekip üyesinin, çalışma akışındaki bir noktada ARR 'yi kullanarak modeli incelemesi beklenir. Daha sonra yalnızca ilk görünüm, önceden işleme adımını tetikler. Sonraki görünümler, SAS çıkış kapsayıcısındaki ilişkili işleme sonrası dosyasını arayacaktır.

Kullanım örneğine bağlı olarak, aynı oturumda birlikte görüntülenecek her bir 3B varlık veya varlık grubu için doğru Azure uzaktan Işleme VM boyutunu, standart veya Premium 'u belirlenmesi ve potansiyel olarak kalıcı hale getirmek isteyeceksiniz.  

### <a name="on-device-model-selection-list"></a>Cihaz üzerinde model seçim listesi

Eğitim, görev Kılavuzu veya pazarlama uygulaması gibi birçok kullanım durumunda, Azure uzaktan Işlemede yaygın olarak görüntülenmek üzere olan 3B varlıkların kümesi oldukça statik olabilir. Bu durumlarda, seçkin bir 3B varlık kümesi önceden dönüştürülebilir ve seçkin varlıkların seçim listesini doldurmak için gerekli bilgileri içeren bir veritabanı aracılığıyla kullanılabilir hale getirilebilir. Bu veriler daha sonra karma gerçeklik uygulamasından alınarak bir seçim menüsünü doldurabilir.

Bu, her bir kişiye veya gruba özgü özel 3B varlıkları karşıya yüklemek için bir yol sunarak daha fazla bir adım yapılabilir. Bu özel varlık listesi daha sonra, görüntülenecek 3B varlıkların çekilmesi için kullanıcı deneyiminde ortak, seçkin varlıkların listesi ile birleştirilebilir.

### <a name="on-device-onedrive-access"></a>Cihaz içi OneDrive erişimi

OneDrive dosya seçicisinin Microsoft 'un karma gerçeklik cihazlarına yerel olarak oluşturulduğu için, OneDrive 'dan cihaz üzerinde 3B varlıklar seçmek, özellikle de farklı veya değiştirilmiş 3B modeller yüklemek için yaygın olarak kullanılan kullanım durumları için yararlıdır. Bu senaryoda, Kullanıcı karma gerçeklik uygulamanızdaki OneDrive dosya Seçicisi aracılığıyla bir veya daha fazla 3B varlık seçer. Daha sonra 3B varlıklar bir SAS giriş kapsayıcısına geçirilir, SAS çıkış kapsayıcısına dönüştürülür ve ARR oturumuna eklenir. İdeal olarak, karma gerçeklik uygulaması, tüm bitleri OneDrive 'dan cihaza taşımak ve Azure Blob depolama alanına geri dönmek için bu adımları gerçekleştirmek üzere bulut tabanlı bir işlem çağıracaktır.

Bu yaklaşım, daha önce görüntülenmiş olan 3B varlıklar arasındaki ilişkiyi kalıcı hale getirerek, aynı modelin OneDrive 'dan yeniden seçildiği durumlarda, uygulamanın dönüştürme işlemini atlayabilir ve ilgili dönüştürülmüş 3B varlığı, SAS URI 'SI aracılığıyla doğrudan yükleyebilmesini sağlayabilir.

Daha fazla bilgi için:

* [OneDrive 'dan Azure 'a depolama çoğaltması için Microsoft Power otomatikleştirir şablonu](https://flow.microsoft.com/galleries/public/templates/2f90b5d3-029b-4e2e-ad37-1c0fe6d187fe/when-a-file-is-uploaded-to-onedrive-copy-it-to-azure-storage-container/)
* [OneDrive dosya depolama API 'sine genel bakış](https://docs.microsoft.com/graph/onedrive-concept-overview)

### <a name="direct-cad-access"></a>Doğrudan CAD erişimi

Karma Gerçeklik için etkileyici bir kullanım örneği, CAD çalışmanın tasarım incelemeleriyle devam etmektedir. Bu senaryoda, masaüstünden karma gerçeklik 'e en hızlı yükleme süresi anahtardır. İdeal bir çözüm, belirli CAD uygulamaları için eklenti geliştirmeyi gerektirebilir. Bu eklentiler, yükleme, dönüştürme ve görüntüleme işleminin her yönünü doğrudan yönetebilir:

* Bir UX girin:
  * CAD uygulamasını belirli bir karma gerçeklik cihazından (bir kez) eşleştirin.
  * Seçili geometrinin, bu karma gerçeklik cihazında görüntülenmesini isteyin.
* Zaten çalışmıyorsa, CAD dosyasını karşıya yüklerken ve dönüştürürken paralel olarak işleyebilmesi için Azure uzaktan Işleme oturumunu çalıştırın
* CAD geometrisi verilerini Azure uzaktan Işleme tarafından desteklenen biçimlerden biriyle normalleştirin
* Normalleştirilmiş verileri doğrudan Azure Blob depolama giriş kapsayıcısına iletme
* Model dönüştürme işlemini Başlat
* Modelin çıkış kapsayıcısı SAS URI 'sini Azure uzaktan Işleme oturumuna bağlama
* Modelin kullanılabilir olduğunu ve görüntüleme için hazır olduğunu eşleştirilmiş karma gerçeklik uygulamasına bildirin ve uygulamanın onu oturuma iliştirebilmesi için çıkış kapsayıcısı SAS URI 'sini sağlayın.

Çok daha basit ancak biraz daha kolay bir yaklaşım, 3D modeli yerel bir sabit sürücüye kaydetme sürecini otomatikleştirebilir ve ardından kaydedilen dosyayı SAS giriş kapsayıcısına aktarmak için bir işlem başlatabilir.

### <a name="azure-marketplace"></a>Azure Market

Birçok kurumsal istemci, Azure Stack güvenlik nedenleriyle kendi Azure hesapları ve kimlik bilgileri kapsamında dağıtılabilecek. Bunu mümkün kılmak için Azure yönetilen uygulamanızın Azure Market 'te Azure Uygulama teklifi olarak yayınlanabileceği şekilde paketlenmesi önerilir.

Daha fazla bilgi için:

* [Azure Market](https://azure.microsoft.com/marketplace/)
* [Öğretici: market 'te Azure yönetilen uygulamaları yayımlama](https://docs.microsoft.com/azure/azure-resource-manager/managed-applications/publish-marketplace-app)

### <a name="security"></a>Güvenlik

Baştan sona güvenlik için uçtan uca Azure uzaktan Işleme çözümünüzü oluşturmak önemlidir. Aşağıdakiler dahil olmak üzere uçtan uca çözümünüzün tasarımında dikkate alınması gereken birçok güvenlik yönü vardır:

* Kimlik doğrulaması stratejileri
* Erişim yönetimi – gruplar, ilkeler ve izinler
* Çok kiracılı
* Veri depolama ve aktarım şifreleme
* Geçici kullanım belirteçleri
* Dağıtılmış hizmet reddi (DDoS) saldırıları
* Tehdit algılama
* VPN 'Ler ve güvenli ağlar
* Güvenlik Duvarları
* Sertifika ve gizli anahtar yönetimi
* Uygulama güvenlik açığı ve yararlanma

Kimlik doğrulaması için, ARR kimlik doğrulaması ve oturum yönetiminin olabildiğince büyük bir Azure Web hizmetine taşınması önerilir. Bu, daha iyi yönetilen ve daha güvenli bir çözüme neden olur.

Daha fazla bilgi için:

* [Azure AD hizmeti kimlik doğrulaması](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp#azure-ad-service-authentication)
* [Azure ile güvenlik duruşunuzu güçlendirin](https://azure.microsoft.com/overview/security/)
* [Bulut güvenliği](https://azure.microsoft.com/product-categories/security/)
