---
title: 'Hızlı başlangıç: Başlarken-SYNAPSE çalışma alanı oluşturma'
description: Bu öğreticide, bir Synapse çalışma alanı, adanmış bir SQL havuzu ve sunucusuz Apache Spark havuzu oluşturmayı öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 03/17/2021
ms.openlocfilehash: fb30913d71df46f4c0afedd475d40205c1429258
ms.sourcegitcommit: 9f4510cb67e566d8dad9a7908fd8b58ade9da3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122212"
---
# <a name="creating-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

Bu öğreticide, bir Synapse çalışma alanı, adanmış bir SQL havuzu ve sunucusuz Apache Spark havuzu oluşturmayı öğreneceksiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticinin adımlarını tamamlayabilmeniz için, **sahip** rolü atadığınız bir kaynak grubuna erişiminizin olması gerekir. Bu kaynak grubunda SYNAPSE çalışma alanını oluşturun.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Azure portal bir Synapse çalışma alanı oluşturun

### <a name="start-the-process"></a>İşlemi Başlat
1. [Azure Portal](https://portal.azure.com)açın, arama çubuğunda, ENTER tuşuna basarak **SYNAPSE** girin.
1. Arama sonuçlarında **Hizmetler**' ın altında **Azure SYNAPSE Analytics**' i seçin.
1. Çalışma alanı oluşturmak için **Ekle** ' yi seçin.

## <a name="basics-tab--project-details"></a>Proje ayrıntıları > temel kavramlar sekmesi
Şu alanları doldurun:

1. **Abonelik** -herhangi bir abonelik seçin.
1. **Kaynak grubu** -herhangi bir kaynak grubunu kullanın.
1. **Kaynak grubu** -bu alanı boş bırakın.


## <a name="basics-tab--workspace-details"></a>> çalışma alanı ayrıntılarının temelleri sekmesi
Şu alanları doldurun:

1. **Çalışma alanı adı** -genel olarak benzersiz bir ad seçin. Bu öğreticide, **MyWorkspace** kullanacağız.
1. **Bölge** -herhangi bir bölgeyi seçin.
1. **Data Lake Storage Gen 2 ' yi seçin**
1. **Abonelikle** ilgili düğmeye tıklayın.
1. **Hesap adına** göre **Yeni oluştur** ' a tıklayın ve yeni depolama hesabını **contosogölü** veya buna benzer şekilde adlandırın.
1. **Dosya sistemi adına** göre, **Yeni oluştur** ' a tıklayın ve BT **kullanıcılarını** adlandırın. Bu, **Kullanıcılar** adlı bir depolama kapsayıcısı oluşturur. Çalışma alanı, bu depolama hesabını Spark tabloları ve Spark uygulama günlükleri için "birincil" depolama hesabı olarak kullanacaktır.
1. "Data Lake Storage 2. hesabında Depolama Blobu veri katılımcısı rolünü ata" kutusunu işaretleyin. 

## <a name="completing-the-process"></a>İşlem Tamamlanıyor
**Gözden geçir ve oluştur** > **Oluştur**'u seçin. Çalışma alanınız birkaç dakika içinde hazırlanıyor.

> [!NOTE]
> Mevcut ayrılmış bir SQL havuzundan (eski adıyla SQL DW) çalışma alanı özelliklerini etkinleştirmek için, [ADANMıŞ SQL havuzunuz (eski ADıYLA SQL DW) için bir çalışma alanı etkinleştirme](./sql-data-warehouse/workspace-connected-create.md)bölümüne bakın.


## <a name="open-synapse-studio"></a>SYNAPSE Studio 'Yu açın

Azure SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:

* SYNAPSE çalışma alanınızı [Azure Portal](https://portal.azure.com)açın, SYNAPSE çalışma alanının **genel bakış** bölümünde SYNAPSE Studio 'yu Aç kutusunda **Aç** ' ı seçin.
* Adresine gidin `https://web.azuresynapse.net` ve çalışma alanınızda oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sunucusuz SQL havuzu kullanarak çözümleme](get-started-analyze-sql-on-demand.md)
