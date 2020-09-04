---
title: Kaynak Denetimi Tümleştirmesi
description: Azure Repos (git ve GitHub) kullanarak yerel kaynak denetimi tümleştirmesiyle SQL havuzu için kurumsal sınıf veritabanı DevOps deneyimi.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql-dw
ms.date: 08/23/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 84abfea39cb7311e7cd60346d936c08c28c334d4
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89441424"
---
# <a name="source-control-integration-for-sql-pool"></a>SQL havuzu için kaynak denetimi tümleştirmesi

Bu öğreticide, SQL Server veri araçları (SSDT) veritabanı projenizi kaynak denetimiyle tümleştirme özetlenmektedir.  Kaynak denetimi tümleştirmesi, Azure SYNAPSE Analytics 'te SQL havuzu kaynağıyla sürekli tümleştirme ve dağıtım işlem hattınızı oluşturmanın ilk adımıdır.

## <a name="before-you-begin"></a>Başlamadan önce

- [Azure DevOps organizasyonu](https://azure.microsoft.com/services/devops/) için kaydolun
- [Oluşturma ve bağlanma](create-data-warehouse-portal.md) öğreticisine gidin
- [Visual Studio 2019 'yi yükler](https://visualstudio.microsoft.com/vs/older-downloads/)

## <a name="set-up-and-connect-to-azure-devops"></a>Azure DevOps 'ı ayarlama ve bu sunucuya bağlanma

1. Azure DevOps kuruluşunuzda, bir Azure depo deposu aracılığıyla SSDT veritabanı projenizi barındıracak bir proje oluşturun.

   ![Proje oluştur](./media/sql-data-warehouse-source-control-integration/1-create-project-azure-devops.png "Proje oluştur")

2. Visual Studio 'Yu açın ve **Bağlantıyı Yönet**' i seçerek Azure DevOps kuruluşunuza ve projenize bir adım daha bağlanın.

   ![Bağlantıları Yönet](./media/sql-data-warehouse-source-control-integration/2-manage-connections.png "Bağlantıları Yönet")

3. **Bağlantıları Yönet**' i seçip **projeye bağlanın**.
 ![Connect1](./media/sql-data-warehouse-source-control-integration/3-connect-project.png "Bağlan")


4. Birinci adımda oluşturduğunuz projeyi bulun, **Bağlan**' ı seçin.
![Connect2](./media/sql-data-warehouse-source-control-integration/3.5-connect.png "Bağlan")


3. Azure DevOps deponuzu projenizden yerel makinenize kopyalayın.

   ![Depoyu Kopyala](./media/sql-data-warehouse-source-control-integration/4-clone-repo.png "Depoyu Kopyala")

Visual Studio kullanarak projeleri bağlama hakkında daha fazla bilgi için [Takım Gezgini içindeki projelere bağlanma](https://docs.microsoft.com/visualstudio/ide/connect-team-project?view=vs-2019)bölümüne bakın. Visual Studio kullanarak bir depoyu kopyalamaya yönelik yönergeler için, [Çıkış deposunu Kopyala](https://docs.microsoft.com/azure/devops/repos/git/clone?view=azure-devops&tabs=visual-studio) makalesini inceleyin. 

## <a name="create-and-connect-your-project"></a>Projenizi oluşturun ve bağlayın

1. Visual Studio 'da, **Yerel kopyalanmış deponuzda**hem dizin hem de yerel Git deposu ile yeni bir SQL Server veritabanı projesi oluşturun.

   ![Yeni proje oluşturma](./media/sql-data-warehouse-source-control-integration/5-create-new-project.png "Yeni proje oluşturma")  

2. Boş sqlprojenize sağ tıklayın ve veri Ambarınızı veritabanı projesine aktarın.

   ![Projeyi içeri aktar](./media/sql-data-warehouse-source-control-integration/6-import-new-project.png "Projeyi içeri aktar")  

3. Visual Studio 'da Takım Gezgini, değişikliklerinizi yerel git deponuza işleyin.

   ![Uygulayın](./media/sql-data-warehouse-source-control-integration/6.5-commit-push-changes.png "İşleme")  

4. Kopyalanan depoda yerel olarak işlenen değişikliklere sahip olduğunuza göre, Azure DevOps projenizdeki Azure depo deponuzda yaptığınız değişiklikleri eşitleyin ve yeniden gönderin.

   ![Eşitleme ve Itme hazırlama](./media/sql-data-warehouse-source-control-integration/7-commit-push-changes.png "Eşitleme ve itme hazırlama")

   ![Eşitle ve Gönder](./media/sql-data-warehouse-source-control-integration/7.5-commit-push-changes.png "Eşitle ve Gönder")  

## <a name="validation"></a>Doğrulama

1. Visual Studio SQL Server Veri Araçları (SSDT) veritabanı projenizdeki bir tablo sütununu güncelleştirerek Azure deponuzda yapılan değişiklikleri doğrulayın.

   ![Güncelleştirme sütununu doğrula](./media/sql-data-warehouse-source-control-integration/8-validation-update-column.png "Güncelleştirme sütununu doğrula")

2. Yerel deponuzdaki değişikliği Azure deposuna yürütün ve gönderin.

   ![Değişiklikleri gönderme](./media/sql-data-warehouse-source-control-integration/9-push-column-change.png "Değişiklikleri gönderme")

3. Azure depo deponuzda değişikliğin itildiğini doğrulayın.

   ![Doğrula](./media/sql-data-warehouse-source-control-integration/10-verify-column-change-pushed.png "Değişiklikleri doğrulama")

4. (**Isteğe bağlı**) Azure depo deponuzdaki nesne tanımlarının ve yerel deponun veri Ambarınızı yansıttığından emin olmak için, SSDT kullanarak hedef veri ambarınızdaki değişiklikleri güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL havuzu için geliştirme](sql-data-warehouse-overview-develop.md)
