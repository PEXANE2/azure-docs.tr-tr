---
title: Azure portal aracılığıyla SYNAPSE SQL havuzunda işlem duraklatma ve devam ettirme
description: Maliyetleri kaydetmek üzere SQL havuzu için işlem duraklatmak için Azure portal kullanın. Veri ambarını kullanmaya hazırsanız işlem işlemine da izin verilmez.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 55e3d5bf4fb63c35d484e4a764c7eeb2e2484fcf
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80350967"
---
# <a name="quickstart-pause-and-resume-compute-in-synapse-sql-pool-via-the-azure-portal"></a>Hızlı başlangıç: Azure portal aracılığıyla SYNAPSE SQL havuzunda işlem duraklatma ve devam ettirme

SYNAPSE SQL Havuzu (veri ambarı) işlem kaynaklarını duraklatmak ve devam etmek için Azure portal kullanabilirsiniz. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

**Mysampledatawarehouse**ADLı bir SQL havuzu oluşturmak için [Create and Connect-Portal](create-data-warehouse-portal.md) ' i kullanın. 

## <a name="pause-compute"></a>İşlem Duraklat

Maliyetleri azaltmak için, talep üzerine işlem kaynaklarını duraklatabilir ve sürdürebilirsiniz. Örneğin, gece ve hafta sonları sırasında veritabanını kullanmıyorsanız, bu zamanlarda duraklatıp gün içinde devam edebilirsiniz. 
>[!NOTE]
>Veritabanı duraklatıldığında işlem kaynakları için ücretlendirilmezsiniz. Ancak, depolama alanı için ücretlendirilmeye devam edersiniz. 

Bir SQL havuzunu duraklatmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portal sol gezinti sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
2. SQL havuzunu açmak için **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. 
3. **Mysampledatawarehouse** sayfasında, **durumu** **çevrimiçi**olarak görürsünüz.

    ![Çevrimiçi işlem](././media/pause-and-resume-compute-portal/compute-online.png)

4. SQL havuzunu duraklatmak için **Duraklat** düğmesine tıklayın. 
5. Devam etmek isteyip istemediğinizi soran bir onay sorusu görüntülenir. **Evet**' e tıklayın.
6. Birkaç dakika bekleyin ve sonra **durumun** **duraklatıldığına**dikkat edin.

    ![Duraklatılıyor](./media/pause-and-resume-compute-portal/pausing.png)

7. Duraklatma işlemi tamamlandığında durum **duraklatılır** ve seçenek düğmesi **devam**edilir.
8. SQL havuzunun işlem kaynakları artık çevrimdışı. Hizmeti sürdürülene kadar işlem için ücretlendirilmezsiniz.

    ![Çevrimdışı işlem](././media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>İşlem işlemine geri dön

SQL havuzunu yeniden başlatmak için aşağıdaki adımları izleyin.

1. Azure portal sol sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
2. SQL havuzu sayfasını açmak için **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. 
3. **Mysampledatawarehouse** sayfasında **durum** **duraklatıldı**' ya dikkat edin.

    ![Çevrimdışı işlem](././media/pause-and-resume-compute-portal/compute-offline.png)

4. SQL havuzunu yeniden başlatmak için, geri ' **ye tıklayın.** 
5. Başlatmak isteyip istemediğinizi soran bir onay sorusu görüntülenir. **Evet**' e tıklayın.
6. **Durumun** **sürdürüldiğine**dikkat edin.

    ![Sürdürülüyor](./media/pause-and-resume-compute-portal/resuming.png)

7. SQL havuzu yeniden çevrimiçi olduğunda durum **çevrimiçi** olur ve seçenek düğmesi **duraklatılır**.
8. SQL havuzu için işlem kaynakları artık çevrimiçi ve hizmeti kullanabilirsiniz. İşlem ücretleri sürdürüldü.

    ![Çevrimiçi işlem](././media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve SQL havuzunuzdaki depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır. 

- Verileri depolama alanında tutmak istiyorsanız, işlem ' i duraklatın.
- Gelecekteki ücretleri kaldırmak istiyorsanız, SQL havuzunu silebilirsiniz. 

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com)oturum açın ve SQL havuzunuza tıklayın.

    ![Kaynakları temizleme](./media/pause-and-resume-compute-portal/clean-up-resources.png)

1. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. 

2. İşlem veya depolama için ücretlendirilmemek üzere SQL havuzunu kaldırmak için **Sil**' e tıklayın.

3. Oluşturduğunuz SQL Server 'ı kaldırmak için **sqlpoolservername.Database.Windows.net**ve ardından **Sil**' e tıklayın.  

   > [!CAUTION]
   > Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Artık SQL havuzunuz için işlemi duraklattı ve sürdürdü. [SQL Pool 'a veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)hakkında daha fazla bilgi edinmek için sonraki makaleye geçin. İşlem yeteneklerini yönetme hakkında daha fazla bilgi için bkz. [işlem yönetimine genel bakış](sql-data-warehouse-manage-compute-overview.md) makalesi. 

