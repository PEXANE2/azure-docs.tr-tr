---
title: 'Hızlı başlangıç: Azure Veri Gezgini bir Synapse çalışma alanına bağlama'
description: Azure SYNAPSE kullanarak bir Azure Veri Gezgini kümesini bir Synapse çalışma alanına bağlama Apache Spark
services: synapse-analytics
author: manojraheja
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 10/07/2020
ms.author: maraheja
ms.reviewer: jrasnick
ms.openlocfilehash: 53092f5c49073098f28d2fd06f38391e858b2b9d
ms.sourcegitcommit: ba7fafe5b3f84b053ecbeeddfb0d3ff07e509e40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91946426"
---
# <a name="connect-to-azure-data-explorer-using-synapse-apache-spark"></a>Apache Spark SYNAPSE kullanarak Azure Veri Gezgini bağlanma

Bu makalede, SYNAPSE Apache Spark ile SYNAPSE Studio 'dan Azure Veri Gezgini veritabanlarına nasıl erişebileceğiniz açıklanır. 

## <a name="prerequisites"></a>Önkoşullar

* [Azure Veri Gezgini kümesi ve veritabanı oluşturun](/azure/data-explorer/create-cluster-database-portal).
* Bu [hızlı](./quickstart-create-workspace.md) başlangıcı izleyerek var olan SYNAPSE çalışma alanı veya yeni bir çalışma alanı oluştur 
* Mevcut SYNAPSE Apache Spark havuzu veya bu [hızlı](./quickstart-create-apache-spark-pool-portal.md) başlangıcı izleyerek yeni bir havuz oluştur
* [Azure AD uygulaması sağlayarak Azure AD uygulaması oluşturun.](/azure/data-explorer/kusto/management/access-control/how-to-provision-aad-app)
* Azure AD uygulamanıza [azure Veri Gezgini veritabanı Izinlerini yönetin](/azure/data-explorer/manage-database-permissions) ve sonra veritabanınıza erişin

## <a name="navigate-to-synapse-studio"></a>SYNAPSE Studio 'ya gidin

Bir Synapse çalışma alanından **Launch SYNAPSE Studio 'yu**seçin. SYNAPSE Studio giriş sayfasında, verileri **Nesne Gezgini verileri** **seçin.**

## <a name="connect-an-azure-data-explorer-database-to-a-synapse-workspace"></a>Azure Veri Gezgini veritabanını bir Synapse çalışma alanına bağlama

Azure Veri Gezgini veritabanını bir çalışma alanına bağlamak, bağlantılı hizmet aracılığıyla yapılır. Azure Data, bağlı hizmeti araştır, kullanıcıların Azure SYNAPSE Analytics için Apache Spark verilere gözatmasına, okumasına ve yazmasına ve bir işlem hattında tümleştirme işlerini çalıştırmasına olanak sağlar.

Veri Nesne Gezgini, bir Azure Veri Gezgini kümesini doğrudan bağlamak için aşağıdaki adımları izleyin:

1. **+** Verilerin yakınında simge Seç
2. Dış verilere **Bağlan** ' ı seçin
3. **Azure Veri Gezgini seçin (kusto)**
5. **Devam**'ı seçin
6. Bağlı hizmeti adlandırın. Ad Nesne Gezgini görüntülenir ve veritabanına bağlanmak için SYNAPSE çalışma zamanları tarafından kullanılır. Kolay bir ad kullanmanızı öneririz
7. Aboneliğinizden Azure Data keşfet kümesini seçin veya URI 'yi girin.
8. "Hizmet sorumlusu KIMLIĞI" ve "hizmet sorumlusu anahtarını" girin (Bu hizmet sorumlusunun, okuma işlemi için veritabanında görünüm erişiminin olduğundan ve verilerin içe inmek için erişim izni olduğundan emin olun)
9. Azure Veri Gezgini veritabanı adını girin
10. Doğru izinlere sahip olduğunuzdan emin olmak için **Bağlantıyı Sına** ' ya tıklayın
11. **Oluştur**’u seçin

    ![Yeni Bağlı Hizmet](./media/quickstart-connect-azure-data-explorer/003-new-linked-service.png)

    > [!NOTE]
    > Seçim Sınama bağlantısı yazma erişimini doğrulamaz, hizmet sorumlusu kimliğinizin Azure Veri Gezgini veritabanına yazma erişimi olduğundan emin olun.

12. Azure Veri Gezgini kümeleri ve veritabanları, Azure Veri Gezgini bölümündeki  **bağlantılı** sekmenin altında görünür. 

    ![Kümelere gözatamıyorum](./media/quickstart-connect-azure-data-explorer/004-browse-clusters.png)

    > [!NOTE] 
    > Geçerli sürümde, veritabanı nesneleri Azure Veri Gezgini veritabanlarında AAD hesabınızın izinlerine göre doldurulur. Apache Spark not defterlerini veya tümleştirme işlerini çalıştırdığınızda, bağlantı hizmetindeki kimlik bilgileri kullanılır (Yani hizmet sorumlusu).


## <a name="quickly-interact-with-code-generated-actions"></a>Kod tarafından oluşturulan eylemlerle hızlıca etkileşim kurun

* Bir veritabanına veya tabloya sağ tıkladığınızda, verileri okumak, veri yazmak, verileri Azure Veri Gezgini 'ye aktarmak için örnek bir Spark Not defteri tetiklenecek hareketlerle ilgili bir listeniz olacaktır. 
    [![Yeni örnek Not defterleri](./media/quickstart-connect-azure-data-explorer/005-new-notebook.png)](./media/quickstart-connect-azure-data-explorer/005-new-notebook.png#lightbox)

* Veri okuma hakkında bir örnek aşağıda verilmiştir. Not defterini Spark havuzunuza eklendi ve [ ![ Yeni bir not defteri oku](./media/quickstart-connect-azure-data-explorer/006-read-data.png)](./media/quickstart-connect-azure-data-explorer/006-read-data.png#lightbox)

   > [!NOTE] 
   > İlk kez yürütmenin Spark oturumunun başlatılması üç dakikadan fazla sürebilir. Sonraki yürütmeler önemli ölçüde daha hızlı olacaktır.  


## <a name="limitations"></a>Sınırlamalar
Azure Veri Gezgini Bağlayıcısı Şu anda Azure SYNAPSE Managed VNET ile desteklenmiyor.


## <a name="next-steps"></a>Sonraki adımlar

* [Gelişmiş seçeneklere sahip örnek kod](https://github.com/Azure/azure-kusto-spark/blob/master/samples/src/main/python/SynapseSample.py)
* [Azure Veri Gezgini (kusto) Spark Bağlayıcısı](https://github.com/Azure/azure-kusto-spark)