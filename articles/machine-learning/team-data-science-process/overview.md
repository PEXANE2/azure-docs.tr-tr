---
title: Ekip Veri Bilimi Süreci Nedir?
description: Tahmine dayalı analitik çözümleri ve akıllı uygulamalar sunmak için veri bilimi metodolojisi sağlar.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: overview
ms.date: 1/10/2020
ms.author: tdsp
ms.custom: previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 10d6e562301e089700940ac5dfb212bcc4e09653
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79088076"
---
# <a name="what-is-the-team-data-science-process"></a>Ekip Veri Bilimi Süreci Nedir?

Ekip Veri Bilimi Süreci (TDSP), tahmine dayalı analitik çözümleri ve akıllı uygulamaları verimli bir şekilde sunmak için çevik, yinelemeli bir veri bilimi metodolojisidir. TDSP, ekip rollerinin birlikte en iyi nasıl çalıştığını önererek ekip işbirliğini ve öğrenmeyi geliştirmeye yardımcı olur. TDSP, veri bilimi girişimlerinin başarılı bir şekilde uygulanmasına yardımcı olmak için Microsoft ve diğer endüstri liderlerinin en iyi uygulamalarını ve yapılarını içerir. Hedef, şirketlerin kendi analiz programlarının avantajlarını tam olarak hayata geçirmesine yardımcı olmaktır.

Bu makalede, TDSP ve ana bileşenlerine genel bir bakış sağlanmış. Burada farklı türde araçlarla uygulanabilen sürecin genel bir açıklamasını salıyoruz. Projenin yaşam döngüsünde yer alan proje görevlerinin ve rollerinin daha ayrıntılı bir açıklaması, bağlantılı ek konularda sağlanır. TDSP'yi ekiplerimizde uygulamak için kullandığımız belirli bir Microsoft araçları ve altyapısı kümesini kullanarak TDSP'nin nasıl uygulanacağı konusunda da rehberlik sağlanır.

## <a name="key-components-of-the-tdsp"></a>TDSP'nin temel bileşenleri

TDSP aşağıdaki temel bileşenlerden oluşur:

- Veri **bilimi yaşam döngüsü** tanımı
- **Standartlaştırılmış proje yapısı**
- Veri bilimi projeleri için **altyapı ve kaynaklar**
- Proje yürütme için **araçlar ve yardımcı programlar**


## <a name="data-science-lifecycle"></a>Veri bilimi yaşam döngüsü

Ekip Veri Bilimi Süreci (TDSP), veri bilimi projelerinizin gelişimini yapılandırmak için bir yaşam döngüsü sağlar. Yaşam döngüsü, başarılı projelerin izlediği tüm adımları özetler.

[CRISP-DM,](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining) [KDD](https://wikipedia.org/wiki/Data_mining#Process) veya kuruluşunuzun kendi özel işlemi gibi başka bir veri bilimi yaşam döngüsü kullanıyorsanız, bu geliştirme yaşam döngüleri bağlamında görev tabanlı TDSP'yi kullanmaya devam edebilirsiniz. Yüksek düzeyde, bu farklı metodolojilerin çok ortak noktaları var. 

Bu yaşam döngüsü, akıllı uygulamaların bir parçası olarak gönderi yapan veri bilimi projeleri için tasarlanmıştır. Bu uygulamalar, tahmine dayalı analitik için makine öğrenimi veya yapay zeka modellerini devreye sokar. Araştırmacı veri bilimi projeleri veya doğaçlama analiz projeleri de bu süreci kullanarak yararlanabilir. Ancak bu gibi durumlarda açıklanan bazı adımlargerekli olmayabilir.    

Yaşam döngüsü, projelerin genellikle yineleyici olarak yürüttüğü başlıca aşamaları özetler:

* **İş Anlayışı**
* **Veri Toplama ve Anlama**
* **Modelleme**
* **Dağıtım**
* **Müşteri Kabulü**

Burada **Takım Veri Bilimi Süreci yaşam döngüsü**görsel bir temsilidir. 

![TDSP-Yaşam Döngüsü2](./media/overview/tdsp-lifecycle2.png) 

TDSP'de yaşam döngüsünün her aşamasıiçin hedefler, görevler ve dokümantasyon yapıları [Team Data Science Process yaşam döngüsü](lifecycle.md) konusunda açıklanmıştır. Bu görevler ve yapılar proje rolleri ile ilişkilidir:

- Çözüm mimarı
- Proje yöneticisi
- Veri bilimcisi
- Proje lideri 

Aşağıdaki diyagram, bu roller için (dikey eksende) yaşam döngüsünün her aşamasıyla (yatay eksende) ilişkili görevlerin (mavi renkte) ve yapıtların (yeşil renkte) Bir ızgara görünümünü sağlar. 

[![TDSP-roller ve görevleri](./media/overview/tdsp-tasks-by-roles.png)](./media/overview/tdsp-tasks-by-roles.png#lightbox)

## <a name="standardized-project-structure"></a>Standartlaştırılmış proje yapısı

Tüm projelerin bir dizin yapısını paylaşması ve proje belgeleri için şablonlar kullanması, ekip üyelerinin projeleri hakkında bilgi bulmalarını kolaylaştırır. Tüm kod ve belgeler, ekip işbirliğini etkinleştirmek için Git, TFS veya Subversion gibi bir sürüm kontrol sisteminde (VCS) depolanır. Jira, Rally ve Azure DevOps gibi çevik bir proje izleme sistemindeki görevleri ve özellikleri izleme, tek tek özellikler için kodun daha yakından izlenmesine olanak tanır. Bu tür izleme, ekiplerin daha iyi maliyet tahminleri elde etmesini de sağlar. TDSP, VCS'deki her proje için sürüm oluşturma, bilgi güvenliği ve işbirliği için ayrı bir depo oluşturmanızı önerir. Tüm projeler için standartlaştırılmış yapı, kuruluş genelinde kurumsal bilgi nin oluşturulmasına yardımcı olur.

Klasör yapısı için şablonlar ve standart konumlarda gerekli belgeler sağlarız. Bu klasör yapısı, veri arama ve özellik ayıklama için kod içeren dosyaları düzenler ve bu kayıt modeli yinelemeler. Bu şablonlar, ekip üyelerinin başkaları tarafından yapılan işleri anlamasını ve takımlara yeni üyeler eklemesini kolaylaştırır. Belge şablonlarını işaretleme biçiminde görüntülemek ve güncelleştirmek kolaydır. Sorunun iyi tanımlandığını ve teslim edilebilirlerin beklenen kaliteyi karşıladığını sağlamak için her proje için anahtar soruları içeren denetim listeleri sağlamak için şablonları kullanın. Örneklere şunlar dahildir:

- iş sorununu ve projenin kapsamını belgelemek için bir proje tüzüğü
- ham verilerin yapısını ve istatistiklerini belgelemek için veri raporları
- türetilen özellikleri belgelemek için model raporları
- ROC eğrileri veya MSE gibi model performans ölçümleri


[![TDSP dizinleri](./media/overview/tdsp-dir-structure.png)](./media/overview/tdsp-dir-structure.png#lightbox)

Dizin yapısı [GitHub](https://github.com/Azure/Azure-TDSP-ProjectTemplate)klonlanabilir.

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Veri bilimi projeleri için altyapı ve kaynaklar  

TDSP, paylaşılan analiz ve depolama altyapılarını yönetmek için öneriler sunar:

- veri kümelerini depolamak için bulut dosya sistemleri 
- veritabanları
- büyük veri (Hadoop veya Spark) kümeleri 
- makine öğrenimi hizmeti 

Ham ve işlenmiş veri kümelerinin depolandığı analitik ve depolama altyapısı bulutta veya şirket içinde olabilir. Bu altyapı tekrarlanabilir analiz sağlar. Ayrıca, tutarsızlıklara ve gereksiz altyapı maliyetlerine yol açabilecek yinelemeleri de önler. Paylaşılan kaynakları sağlamak, onları izlemek ve her ekip üyesinin bu kaynaklara güvenli bir şekilde bağlanmasına izin vermek için araçlar sağlanır. Ayrıca, proje üyelerinin tutarlı bir işlem ortamı oluşturması da iyi bir uygulamadır. Daha sonra farklı ekip üyeleri denemeleri çoğaltabilir ve doğrulayabilir.

Aşağıda, birden çok proje üzerinde çalışan ve çeşitli bulut analizi altyapı bileşenlerini paylaşan bir ekibin bir örneği verilmiştir.

[![TDSP altyapısı](./media/overview/tdsp-analytics-infra.png)](./media/overview/tdsp-analytics-infra.png#lightbox) 


## <a name="tools-and-utilities-for-project-execution"></a>Proje yürütme için araçlar ve yardımcı programlar

Çoğu kuruluşta süreçleri tanıtmak zordur. Veri bilimi süreci ve yaşam döngüsünü uygulamak için sağlanan araçlar, bunların benimsenmesinin önündeki engelleri düşürmeye ve tutarlılığının artmasına yardımcı olur. TDSP, TDSP'nin bir takım içinde benimsenmesini hızlandırmak için bir başlangıç araçları ve komut dosyaları sağlar. Ayrıca, veri arama ve temel modelleme gibi veri bilimi yaşam döngüsündeki bazı ortak görevlerin otomatiklenmesine yardımcı olur. Bireylerin, takımlarının paylaşılan kod deposuna paylaşılan araçlara ve yardımcı programlara katkıda bulunmaları için iyi tanımlanmış bir yapı sağlanır. Bu kaynaklar daha sonra takım veya kuruluş içindeki diğer projeler tarafından kullanılabilir. TDSP ayrıca araçların ve yardımcı programların tüm topluma katkılarını sağlamayı planlıyor. TDSP yardımcı programları [GitHub](https://github.com/Azure/Azure-TDSP-Utilities)klonlanabilir.


## <a name="next-steps"></a>Sonraki adımlar

[Ekip Veri Bilimi Süreci: Roller ve görevler](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) Bu işlemi standartlaştıran bir veri bilimi ekibinin temel personel rollerini ve ilişkili görevlerini özetler. 
