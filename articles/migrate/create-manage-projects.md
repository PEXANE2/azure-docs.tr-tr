---
title: Azure Geçişi projelerini oluşturma ve yönetme
description: Azure geçişi 'nde projeleri bulun, oluşturun, yönetin ve silin.
ms.topic: how-to
ms.date: 04/19/2020
ms.openlocfilehash: f5079ed979d98f2c6f0c654c860c6f176f366497
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81676384"
---
# <a name="create-and-manage-azure-migrate-projects"></a>Azure Geçişi projelerini oluşturma ve yönetme

Bu makalede, [Azure geçişi](migrate-services-overview.md) projelerini oluşturma, yönetme ve silme işlemlerinin nasıl yapılacağı açıklanır.


## <a name="create-a-project-for-the-first-time"></a>İlk kez bir proje oluşturun

Azure geçişi 'ni ilk kez ayarladığınızda bir proje oluşturur ve bir değerlendirme ya da geçiş aracı eklersiniz. İlk kez ayarlamak için [Bu yönergeleri izleyin](how-to-add-tool-first-time.md) .

## <a name="create-additional-projects"></a>Ek projeler oluşturma

Zaten bir Azure geçişi projeniz varsa ve ek bir proje oluşturmak istiyorsanız aşağıdakileri yapın:  

1. Azure [genel portalı](https://portal.azure.com) veya [Azure Kamu](https://portal.azure.us)'da **Azure geçişi**' ni arayın.
2. Azure 'a Pano geçirme > **sunucularında**sağ üst köşedeki **Değiştir** ' i seçin.

   ![Azure geçişi projesini değiştirme](./media/create-manage-projects/switch-project.png)

3. Yeni bir proje oluşturmak için **buraya tıklayın ' ı**seçin.

   ![İkinci bir Azure geçişi projesi oluşturma](./media/create-manage-projects/create-new-project.png)


## <a name="find-a-project"></a>Proje bulma

Projeyi aşağıdaki gibi bulun:

1. [Azure Portal](https://portal.azure.com) **Azure geçişi**' ni arayın.
2. Azure 'da Pano geçirme > **sunucularında**sağ üst köşedeki **Değiştir** ' i seçin.

    ![Mevcut bir Azure geçişi projesine geç](./media/create-manage-projects/switch-project.png)

3. Uygun aboneliği ve Azure geçişi projesini seçin.


Projeyi Azure geçişi 'nin [önceki sürümünde](migrate-services-overview.md#azure-migrate-versions) oluşturduysanız, aşağıdaki gibi bulun:

1. [Azure Portal](https://portal.azure.com) **Azure geçişi**' ni arayın.
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

### <a name="delete-a-workspace-manually"></a>Çalışma alanını el ile silme

1. Projeye bağlı Log Analytics çalışma alanına gidin.

    - Azure geçişi projesini sildiyseniz, **Essentials**  >  **Sunucu değerlendirmesi**' nde çalışma alanının bağlantısını bulabilirsiniz.
       ![LA çalışma alanı ](./media/create-manage-projects/loganalytics-workspace.png) .
       
    - Azure geçişi projesini zaten sildiyseniz, Azure portal sol bölmesinde **kaynak grupları** ' nı seçin ve çalışma alanını bulun.
       
2. Çalışma alanını silmek için [yönergeleri izleyin](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) .

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi projelerine [değerlendirme](how-to-assess.md) veya [geçiş](how-to-migrate.md) araçları ekleyin.
