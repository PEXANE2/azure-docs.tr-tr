---
title: Azure Geçişi projelerini oluşturma ve yönetme
description: Azure geçişi 'nde projeleri bulun, oluşturun, yönetin ve silin.
author: ms-psharma
ms.author: panshar
ms.manager: abhemraj
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: 445e08b255e5b4dd67dd1c6a47c8df6ce59df5bd
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96753833"
---
# <a name="create-and-manage-azure-migrate-projects"></a>Azure Geçişi projelerini oluşturma ve yönetme

Bu makalede, [Azure geçişi](migrate-services-overview.md) projelerini oluşturma, yönetme ve silme işlemlerinin nasıl yapılacağı açıklanır.

Değerlendirme veya geçiş yaptığınız ortamdan toplanan bulma, değerlendirme ve geçiş meta verilerini depolamak için bir Azure geçişi projesi kullanılır. Bir projede keşfedilen varlıkları izleyebilir, değerlendirmeler oluşturabilir ve geçişleri Azure 'da düzenleyebilirsiniz.  

## <a name="verify-permissions"></a>İzinleri doğrulama

Azure geçişi projesi oluşturmak için doğru izinlere sahip olup olmadığınızı kontrol edin:

1. Azure portal, ilgili aboneliği açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntüle ' yi seçin. *Katkıda bulunan* veya *sahip* izinlerinizin olması gerekir. 


## <a name="create-a-project-for-the-first-time"></a>İlk kez bir proje oluşturun

Azure aboneliğinde yeni bir Azure geçişi projesi ayarlayın.

1. Azure portal *Azure geçişi*' ni arayın.
2. **Hizmetler**' de **Azure geçişi**' ni seçin.
3. **Genel Bakış** bölümünde **Sunucuları değerlendir ve geçir**’i seçin.

    ![Sunucuları değerlendirmek ve geçirmek için genel bakış seçeneği](./media/create-manage-projects/assess-migrate-servers.png)

4. **Sunucular** bölümünde **proje oluştur**' u seçin.

    ![Proje oluşturmaya başlamak için düğme](./media/create-manage-projects/create-project.png)

5. **Proje oluştur**' da Azure aboneliğini ve kaynak grubunu seçin. Yoksa, bir kaynak grubu oluşturun.
6. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin.
    - Coğrafya yalnızca şirket içi makinelerden toplanan meta verileri depolamak için kullanılır. Geçiş için herhangi bir hedef bölge seçebilirsiniz. 
    - [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

8. **Oluştur**’u seçin.

   ![Proje ayarlarının giriş sayfası](./media/create-manage-projects/project-details.png)


Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin.

## <a name="create-a-project-in-a-specific-region"></a>Belirli bir bölgede proje oluşturma

Portalda, içinde projeyi oluşturmak istediğiniz coğrafi seçimi seçebilirsiniz. Projeyi oluşturmak için aşağıdaki API komutunu kullanarak belirli bir Azure bölgesi içinde proje oluşturmak istiyorsanız.

```rest
PUT /subscriptions/<subid>/resourceGroups/<rg>/providers/Microsoft.Migrate/MigrateProjects/<mymigrateprojectname>?api-version=2018-09-01-preview "{location: 'centralus', properties: {}}"
```

## <a name="create-additional-projects"></a>Ek projeler oluşturma

Zaten bir Azure geçişi projeniz varsa ve ek bir proje oluşturmak istiyorsanız aşağıdakileri yapın:  

1. Azure [genel portalı](https://portal.azure.com) veya [Azure Kamu](https://portal.azure.us)'da **Azure geçişi**' ni arayın.
2. Azure 'a Pano geçirme > **sunucularında** sağ üst köşedeki **Değiştir** ' i seçin.

   ![Azure geçişi projesini değiştirme](./media/create-manage-projects/switch-project.png)

3. Yeni bir proje oluşturmak için **buraya tıklayın ' ı** seçin.


## <a name="find-a-project"></a>Proje bulma

Projeyi aşağıdaki gibi bulun:

1. [Azure Portal](https://portal.azure.com) *Azure geçişi*' ni arayın.
2. Azure 'da Pano geçirme > **sunucularında** sağ üst köşedeki **Değiştir** ' i seçin.

    ![Mevcut bir Azure geçişi projesine geç](./media/create-manage-projects/switch-project.png)

3. Uygun aboneliği ve Azure geçişi projesini seçin.


### <a name="find-a-legacy-project"></a>Eski bir proje bulun

Projeyi Azure geçişi 'nin [önceki sürümünde](migrate-services-overview.md#azure-migrate-versions) oluşturduysanız, aşağıdaki gibi bulun:

1. [Azure Portal](https://portal.azure.com) *Azure geçişi*' ni arayın.
2. Azure geçişi panosunda, önceki sürümde bir proje oluşturduysanız, eski projelere başvuran bir başlık görüntülenir. Başlığı seçin.

    ![Mevcut projelere erişin](./media/create-manage-projects/access-existing-projects.png)

3. Eski projelerin listesini gözden geçirin.


## <a name="delete-a-project"></a>Projeyi silme

Aşağıdaki gibi silin:

1. Projenin oluşturulduğu Azure kaynak grubunu açın.
2. Kaynak grubu sayfasında **gizli türleri göster**' i seçin.
3. Silmek istediğiniz geçiş projesini ve ilişkili kaynaklarını seçin.
    - Kaynak türü **Microsoft. Migrate/migrateprojects**.
    - Kaynak grubu Azure geçişi projesi tarafından özel olarak kullanılıyorsa, tüm kaynak grubunu silebilirsiniz.

Şunlara dikkat edin:

- Sildiğinizde, hem proje hem de bulunan makineler ile ilgili meta veriler silinir.
- Azure geçişi 'nin eski sürümünü kullanıyorsanız, projenin oluşturulduğu Azure kaynak grubunu açın. Silmek istediğiniz geçişi projeyi (kaynak türü **geçiş projesi**) seçin.
- Bir Azure Log Analytics çalışma alanıyla bağımlılık Analizi kullanıyorsanız:
    - Sunucu değerlendirmesi aracına bir Log Analytics çalışma alanı eklediyseniz, çalışma alanı otomatik olarak silinmez. Aynı Log Analytics çalışma alanı birden çok senaryo için kullanılabilir.
    - Log Analytics çalışma alanını silmek istiyorsanız, bunu el ile yapın.
- Proje silme işlemi geri alınamaz. Silinen nesneler kurtarılamaz.

### <a name="delete-a-workspace-manually"></a>Çalışma alanını el ile silme

1. Projeye bağlı Log Analytics çalışma alanına gidin.

    - Azure geçişi projesini sildiyseniz, **Essentials**  >  **Sunucu değerlendirmesi**' nde çalışma alanının bağlantısını bulabilirsiniz.
       ![LA çalışma alanı ](./media/create-manage-projects/loganalytics-workspace.png) .
       
    - Azure geçişi projesini zaten sildiyseniz, Azure portal sol bölmesinde **kaynak grupları** ' nı seçin ve çalışma alanını bulun.
       
2. Çalışma alanını silmek için [yönergeleri izleyin](../azure-monitor/platform/delete-workspace.md) .

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi projelerine [değerlendirme](how-to-assess.md) veya [geçiş](how-to-migrate.md) araçları ekleyin.
