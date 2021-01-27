---
title: En iyi performans uygulamaları-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'nın performansını izlemeye ve ayarlamaya yönelik en iyi uygulamalar açıklanmaktadır.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 11/23/2020
ms.openlocfilehash: 30176e2df850e6d2794ab9c1542bcb6a89d8f89f
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98880415"
---
# <a name="best-practices-for-optimal-performance-of-your-azure-database-for-mysql---single-server"></a>MySQL için Azure veritabanınızın en iyi performansı için en iyi uygulamalar-tek sunucu

MySQL için Azure veritabanı-tek sunucu ile çalışırken en iyi performansı elde etmek için en iyi uygulamalar hakkında bilgi edinin. Platforma yeni yetenekler eklediğimiz için, bu bölümde ayrıntılı olarak açıklanan en iyi uygulamaları geliştirmeye devam edeceğiz.

## <a name="physical-proximity"></a>Fiziksel yakınlık

 Bir uygulamayı ve veritabanını aynı bölgede dağıttığınızdan emin olun. İstemci ve veritabanı arasındaki ağ gecikmesini basit bir SELECT 1 sorgusu kullanarak belirlemek, herhangi bir performans sınama çalıştırması başlatmadan önce hızlı bir denetim sağlar. 

## <a name="accelerated-networking"></a>Hızlandırılmış Ağ

Azure sanal makinesi, Azure Kubernetes veya App Services kullanıyorsanız, uygulama sunucusu için hızlandırılmış ağ kullanın. Hızlandırılmış ağ, bir VM 'ye tek köklü g/ç Sanallaştırması (SR-ıOV) sağlar ve ağ performansını büyük ölçüde geliştirir. Bu yüksek performanslı yol, desteklenen VM türlerinde en zorlu ağ iş yükleri ile kullanım için, gecikme süresi, değişim ve CPU kullanımını azaltan ana bilgisayarı veri yolundan atlar.

## <a name="connection-efficiency"></a>Bağlantı verimliliği

Yeni bir bağlantı kurmak her zaman pahalı ve zaman alıcı bir görevdir. Bir uygulama bir veritabanı bağlantısı istediğinde, yeni bir tane oluşturmak yerine var olan boştaki veritabanı bağlantılarının ayrılmasını önceliklendirir.  İyi bağlantı uygulamaları için bazı seçenekler şunlardır:

- **Proxysql**: yerleşik bağlantı havuzu sağlayan [proxysql](https://proxysql.com/) kullanın ve uygulama kodundaki değişikliklerle isteğe bağlı olarak birden çok okuma çoğaltmasına [Yük Dengelemesi](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042) yapın.

- **Heimdall Data proxy**: Alternatif olarak, satıcının bağımsız özel proxy çözümü olan Heımdall veri proxy 'sinin da yararlanabilirsiniz. Sorgu önbelleğe alma ve çoğaltma gecikmesi algılaması ile okuma/yazma bölme işlemlerini destekler. Ayrıca [, Heımdall proxy Ile MySQL performansını hızlandırma](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/accelerate-mysql-performance-with-the-heimdall-proxy/ba-p/1063349)bölümüne de başvurabilirsiniz.  

- **Kalıcı veya Long-Lived bağlantısı**: uygulamanızda genellikle 5-10 MS < yürütme süresi ile kısa işlemler veya sorgular varsa, kısa bağlantıları kalıcı bağlantılarla değiştirin. Kısa bağlantıları kalıcı bağlantılarla değiştirme yalnızca kodda küçük değişiklikler yapılmasını gerektirir, ancak birçok tipik uygulama senaryosunda performansı iyileştirmek açısından önemli bir etkiye sahiptir. İşlem tamamlandığında zaman aşımını ayarladığınızdan emin olun veya bağlantıyı kapatın.

- **Çoğaltma**: çoğaltma kullanıyorsanız, birincil sunucu ile okunabilir ikincil çoğaltma sunucusu arasındaki yükü dengelemek Için [proxysql](https://proxysql.com/) kullanın. [ProxySQL ayarlamayı](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/scaling-an-azure-database-for-mysql-workload-running-on/ba-p/1105847)öğrenin.

## <a name="data-import-configurations"></a>Veri Içeri aktarma yapılandırması

- Bir veri içeri aktarma işlemine başlamadan önce örneğinizi daha yüksek SKU boyutuna ölçeklendirebilir ve içeri aktarma başarılı olduğunda ölçeği devre dışı bırakabilirsiniz.
- Çevrimiçi veya çevrimdışı geçişler için [Azure veritabanı geçiş hizmeti 'ni (DMS)](https://datamigration.microsoft.com/) kullanarak verilerinizi en az kapalı kalma süresiyle içeri aktarabilirsiniz. 

## <a name="azure-database-for-mysql-memory-recommendations"></a>MySQL için Azure veritabanı bellek önerileri

MySQL için Azure veritabanı performansı en iyi uygulaması, çalışma kümesi neredeyse tamamen bellekte yer alacak şekilde yeterli miktarda RAM ayırmaya yöneliktir. 

- [MySQL sunucusunun ölçümlerini](./concepts-monitoring.md)kullanarak [sınırlara](./concepts-pricing-tiers.md) ulaşılması halinde kullanılan bellek yüzdesinin olup olmadığını denetleyin. 
- Sunucuların sınırlara ulaştığı şekilde bu tür uyarılar için uyarı ayarlayın. Tanımlanan sınırlara bağlı olarak, veritabanı SKU 'sunun ölçeğini, daha yüksek işlem boyutuna veya daha iyi bir fiyatlandırma katmanına göre ölçeklendirerek, performansı önemli ölçüde artarak elde edin. 
- Ölçek işleminden sonra performans numaralarınızı önemli ölçüde düşene kadar ölçeği ölçeklendirin. Bir VERITABANı örneğinin ölçümlerini izleme hakkında bilgi için bkz. [MySQL db ölçümleri](./concepts-monitoring.md#metrics).

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL için Azure veritabanı 'nı kullanarak sunucu işlemleri için en iyi uygulama](concept-operation-excellence-best-practices.md) <br/>
- [MySQL için Azure veritabanınızı izlemek için en iyi uygulama](concept-monitoring-best-practices.md)<br/>
- [MySQL için Azure veritabanı 'nı kullanmaya başlama](quickstart-create-mysql-server-database-using-azure-portal.md)<br/>