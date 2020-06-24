---
title: 'Hızlı başlangıç: iş yükü yalıtımını yapılandırma-Portal'
description: İş yükü yalıtımını yapılandırmak için Azure portal kullanın.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 05/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 30862a0c16995e143df72f2a243419819941f54e
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213049"
---
# <a name="quickstart-configure-synapse-sql-pool-workload-isolation-using-a-workload-group-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal bir iş yükü grubu kullanarak SYNAPSE SQL havuzu iş yükü yalıtımını yapılandırma

Bu hızlı başlangıçta, kaynak ayırma için bir iş yükü grubu oluşturarak [iş yükü yalıtımını](sql-data-warehouse-workload-isolation.md) yapılandıracaksınız.  Bu öğreticinin amaçları doğrultusunda, çağıran veri yükleme için iş yükü grubunu oluşturacağız `DataLoads` . İş yükü grubu, sistem kaynaklarının %20 ' sini ayıracaktır.  Veri yükleri için %20 yalıtımıyla, bunlar SLA 'Lara vurmasına izin veren garantili kaynaklardır.  İş yükü grubunu oluşturduktan sonra, bu iş yükü grubuna sorgular atamak için [bir iş yükü Sınıflandırıcısı oluşturun](quickstart-create-a-workload-classifier-portal.md) .


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.


## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

> [!NOTE]
> Azure SYNAPSE Analytics 'te bir SQL havuzu örneği oluşturmak, yeni bir faturalanabilir hizmetle sonuçlanabilir.  Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç, SYNAPSE SQL 'de zaten bir SQL havuzu örneğiniz olduğunu ve DENETIM VERITABANı izinlerine sahip olduğunuzu varsayar. Gerekiyorsa **mySampleDataWarehouse** adlı bir veri ambarı oluşturmak için [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanabilirsiniz.

>[!IMPORTANT] 
>İş yükü yönetimini yapılandırmak için SQL havuzunuzun çevrimiçi olması gerekir. 

## <a name="configure-workload-isolation"></a>İş yükü yalıtımını yapılandırma
SQL havuzu kaynakları, iş yükü grupları oluşturularak belirli iş yükleri için yalıtılabilir ve ayrılabilir.  İş yükü gruplarının iş yükünüzü yönetme konusunda nasıl yardımcı olduğunu öğrenmek için [Iş yükü yalıtımı](sql-data-warehouse-workload-isolation.md) kavram belgelerine bakın.  [Oluşturma ve bağlanma-Portal](create-data-warehouse-portal.md) hızlı başlangıcı, **mysampledatawarehouse** oluşturdu ve 400 dwus ile başlatıldı. Aşağıdaki adımlarda, **Mysampledatawarehouse**içinde bir iş yükü grubu oluşturulur.

%20 yalıtımına sahip bir iş yükü grubu oluşturmak için:
1.  Azure portal sol sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
2.  **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. SQL Havuzu açılır.
3.  **Iş yükü yönetimi**' ne tıklayın.
4.  **Yeni iş yükü grubu**' na tıklayın.
5.  **Özel ' e**tıklayın.

    ![Özel 'e tıklayın](./media/quickstart-configure-workload-isolation-portal/create-wg.png)

6.  `DataLoads` **İş yükü grubu**için girin.
7.  `20` **Min. resources**için girin%.
8.  `5` **Istek başına min. resources%** için girin.
9.  `100` **Cap kaynakları için%** girin.
10.   **Kaydet**’e tıklayın.

   ![Kaydet’e tıklayın.](./media/quickstart-configure-workload-isolation-portal/configure-wg.png)

İş yükü grubu oluşturulduğunda bir portal bildirimi görüntülenir.  İş yükü grubu kaynakları, yapılandırılan değerlerin altındaki grafiklerde görüntülenir.

   ![Son ' a tıklayın](./media/quickstart-configure-workload-isolation-portal/display-wg.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

`DataLoads`Bu öğreticide oluşturulan iş yükü grubunu silmek için:
1. **`...`** `DataLoads` İş yükü grubunun sağındaki öğesine tıklayın.
2. **İş yükü grubunu sil**' e tıklayın.
3. İş yükü grubunun silinmesini onaylamanız istendiğinde **Evet** ' e tıklayın.
4. **Kaydet**'e tıklayın.

   ![Sil 'e tıklayın](./media/quickstart-configure-workload-isolation-portal/delete-wg.png)



Veri ambarı birimleri ve veri Ambarınızda depolanan veriler için ücret ödersiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depoda tutmak istiyorsanız, veri ambarını kullanmadığınız zamanlarda işlemi duraklatabilirsiniz. İşlem duraklatıldığında yalnızca veri depolama alanı için ücret ödersiniz. Verilerle çalışmaya hazırsanız, işlem işlemini sürdürmeniz gerekir.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz.

Kaynakları temizlemek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com)oturum açın, veri Ambarınızda öğesini seçin.

    ![Kaynakları temizleme](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. İşlem duraklatmak için **Duraklat** düğmesini seçin. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlem işlemini sürdürmesini sağlamak için **Başlat**' ı seçin.

3. İşlem veya depolama için ücretlendirilmemek üzere veri ambarını kaldırmak için **Sil**' i seçin.

4. Oluşturduğunuz SQL Server 'ı kaldırmak için önceki görüntüde **sqlpoolservername.Database.Windows.net** ' ı seçin ve **Sil**' i seçin.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için **Myresourcegroup**' ı seçin ve **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

İş yükü grubunu kullanmak için `DataLoads` , istekleri iş yükü grubuna yönlendirmek üzere bir [iş yükü sınıflandırıcının](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) oluşturulması gerekir.  İçin bir iş yükü Sınıflandırıcısı oluşturmak üzere [iş yükü Sınıflandırıcısı oluşturma](quickstart-create-a-workload-classifier-portal.md) öğreticisine geçin `DataLoads` .

## <a name="see-also"></a>Ayrıca bkz.
İş yükü yönetimi için iş yüklerini izleme hakkında ayrıntılı bilgi için bkz. nasıl yapılır makalesi [Iş yükü yönetimini yönetme ve izleme](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md) .
