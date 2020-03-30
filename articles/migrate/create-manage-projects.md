---
title: Azure Geçişi projelerini oluşturma ve yönetme
description: Azure Geçiş'te projeleri bulun, oluşturun, yönetin ve silin.
ms.topic: how-to
ms.date: 02/17/2020
ms.openlocfilehash: a49595f0580e71048239d5c5d8f4d1a66e24fe6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269646"
---
# <a name="create-and-manage-azure-migrate-projects"></a>Azure Geçişi projelerini oluşturma ve yönetme

Bu makalede, [Azure Geçiş](migrate-services-overview.md) projeleri nasıl oluşturulup yönetilmeye ve silinir.


## <a name="create-a-project-for-the-first-time"></a>İlk kez bir proje oluşturma

Azure Geçiş'i ilk kez ayarladığınızda, bir proje oluşturur ve bir değerlendirme veya geçiş aracı eklersiniz. İlk kez ayarlamak için [bu yönergeleri izleyin.](how-to-add-tool-first-time.md)

## <a name="create-additional-projects"></a>Ek projeler oluşturma

Zaten bir Azure Geçiş projeniz varsa ve ek bir proje oluşturmak istiyorsanız aşağıdakileri yapın:  

1. Azure [portalında](https://portal.azure.com)Azure **Geçir'i**arayın.
2. Azure Geçir panosu > **Sunucular'da**sağ üst köşede **değiştir'i** seçin.

   ![Azure Geçiş projesini değiştirme](./media/create-manage-projects/switch-project.png)

3. Yeni bir proje oluşturmak için **buraya tıklayın'** ı seçin.

   ![İkinci bir Azure Geçiş projesi oluşturma](./media/create-manage-projects/create-new-project.png)


## <a name="find-a-project"></a>Proje bulma

Aşağıdaki gibi bir proje bulun:

1. Azure [portalında](https://portal.azure.com)Azure **Geçir'i**arayın.
2. Azure Geçir panosu> **Sunucular'** da sağ üst köşede **değiştir'i** seçin.

    ![Varolan bir Azure Geçiş projesine geçiş](./media/create-manage-projects/switch-project.png)

3. Uygun aboneliği ve Azure Geçiş projesini seçin.


Projeyi Azure Geçiş'in [önceki sürümünde](migrate-services-overview.md#azure-migrate-versions) oluşturduysanız, aşağıdaki gibi bulabilirsiniz:

1. Azure [portalında](https://portal.azure.com)Azure **Geçir'i**arayın.
2. Azure Geçir panosunda, önceki sürümde bir proje oluşturduysanız, eski projelere atıfta bulunan bir başlık görüntülenir. Banner'ı seçin.

    ![Varolan projelere erişin](./media/create-manage-projects/access-existing-projects.png)

3. Eski projelerin listesini gözden geçirin.


## <a name="delete-a-project"></a>Projeyi silme

Aşağıdaki gibi silin:

1. Projenin oluşturulduğu Azure kaynak grubunu açın.
2. Kaynak grubu sayfasında, **gizli türleri göster'i**seçin.
3. Silmek istediğiniz geçiş projesini ve ilişkili kaynaklarını seçin.
    - Kaynak türü **Microsoft.Migrate/migrate projeleridir.**
    - Kaynak grubu yalnızca Azure Geçiş projesi tarafından kullanılıyorsa, tüm kaynak grubunu silebilirsiniz.


Şunlara dikkat edin:

- Sildiğinizde, hem proje hem de keşfedilen makinelerle ilgili meta veriler silinir.
- Azure Geçiş'in eski sürümünü kullanıyorsanız, projenin oluşturulduğu Azure kaynak grubunu açın. Silmek istediğiniz geçiş projesini seçin (kaynak türü **Geçiş projesidir).**
- Bir Azure Günlük Analizi çalışma alanıyla bağımlılık çözümlemesi kullanıyorsanız:
    - Sunucu Değerlendirmesi aracına bir Log Analytics çalışma alanı iliştirdiyseniz, çalışma alanı otomatik olarak silinmez. Aynı Log Analytics çalışma alanı birden çok senaryo için kullanılabilir.
    - Log Analytics çalışma alanını silmek istiyorsanız, bunu el ile yapın.

### <a name="delete-a-workspace-manually"></a>Çalışma alanını el ile silme

1. Projeye bağlı Log Analytics çalışma alanına göz atın.

    - Azure Geçiş projesini sildiyseniz, Çalışma alanının bağlantısını **Essentials** > **Server Assessment'da**bulabilirsiniz.
       ![LA Çalışma](./media/create-manage-projects/loganalytics-workspace.png)Alanı .
       
    - Azure Geçir projesini zaten sildiyseniz, Azure portalının sol bölmesinde **Kaynak Grupları'nı** seçin ve çalışma alanını bulun.
       
2. Çalışma alanını silmek için [yönergeleri izleyin.](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace)

## <a name="next-steps"></a>Sonraki adımlar

Azure Geçiş projelerine [değerlendirme](how-to-assess.md) veya [geçiş](how-to-migrate.md) araçları ekleyin.
