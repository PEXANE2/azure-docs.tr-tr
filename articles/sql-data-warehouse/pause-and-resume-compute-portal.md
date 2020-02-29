---
title: 'Hızlı başlangıç: duraklatma & devam eden işlem-Azure portal '
description: Maliyetleri kaydetmek üzere SQL havuzu için işlem duraklatmak için Azure portal kullanın. Veri ambarını kullanmaya hazırsanız işlem işlemine da izin verilmez.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 49684e6844c2d9f58e9b750b12991428218e4426
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78200663"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-synapse-analytics-sql-pool-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal Azure SYNAPSE Analytics SQL havuzunda işlem duraklatma ve devam ettirme

Maliyetleri kaydetmek üzere SQL havuzu için işlem duraklatmak için Azure portal kullanın. Veri ambarını kullanmaya hazırsanız [Işlem işlemine](sql-data-warehouse-manage-compute-overview.md) da izin verilmez.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

**Mysampledatawarehouse**ADLı bir SQL havuzu oluşturmak için [Create and Connect-Portal](create-data-warehouse-portal.md) ' i kullanın. 

## <a name="pause-compute"></a>İşlem Duraklat

Maliyetleri kaydetmek için, talep üzerine işlem kaynaklarını duraklatabilir ve sürdürebilirsiniz. Örneğin, gece ve hafta sonları sırasında veritabanını kullanmıyorsanız, bu zamanlarda duraklatıp gün içinde devam edebilirsiniz. Veritabanı duraklatıldığında işlem kaynakları için ücretlendirilmezsiniz. Ancak, depolama alanı için ücretlendirilmeye devam edersiniz. 

Bir SQL havuzunu duraklatmak için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portal sol gezinti sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
2. SQL havuzunu açmak için **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. 
3. **Mysampledatawarehouse** sayfasında, **durumu** **çevrimiçi**olarak görürsünüz.

    ![Çevrimiçi işlem](media/pause-and-resume-compute-portal/compute-online.png)

4. SQL havuzunu duraklatmak için **Duraklat** düğmesine tıklayın. 
5. Devam etmek isteyip istemediğinizi soran bir onay sorusu görüntülenir. **Evet**'e tıklayın.
6. Birkaç dakika bekleyin ve sonra **durumun** **duraklatıldığına**dikkat edin.

    ![Duraklatılıyor](media/pause-and-resume-compute-portal/pausing.png)

7. Duraklatma işlemi tamamlandığında durum **duraklatılır** ve seçenek düğmesi **devam**edilir.
8. SQL havuzunun işlem kaynakları artık çevrimdışı. Hizmeti sürdürülene kadar işlem için ücretlendirilmezsiniz.

    ![Çevrimdışı işlem](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>İşlem işlemine geri dön

SQL havuzunu yeniden başlatmak için aşağıdaki adımları izleyin.

1. Azure portal sol sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
2. SQL havuzu sayfasını açmak için **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. 
3. **Mysampledatawarehouse** sayfasında **durum** **duraklatıldı**' ya dikkat edin.

    ![Çevrimdışı işlem](media/pause-and-resume-compute-portal/compute-offline.png)

4. SQL havuzunu yeniden başlatmak için, geri ' **ye tıklayın.** 
5. Başlatmak isteyip istemediğinizi soran bir onay sorusu görüntülenir. **Evet**'e tıklayın.
6. **Durumun** **sürdürüldiğine**dikkat edin.

    ![Sürdürülüyor](media/pause-and-resume-compute-portal/resuming.png)

7. SQL havuzu yeniden çevrimiçi olduğunda durum **çevrimiçi** olur ve seçenek düğmesi **duraklatılır**.
8. SQL havuzu için işlem kaynakları artık çevrimiçi ve hizmeti kullanabilirsiniz. İşlem ücretleri sürdürüldü.

    ![Çevrimiçi işlem](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve veri Ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır. 

- Verileri depolama alanında tutmak istiyorsanız, işlem ' i duraklatın.
- Gelecekteki ücretleri kaldırmak istiyorsanız, SQL havuzunu silebilirsiniz. 

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com)oturum açın ve SQL havuzunuza tıklayın.

    ![Kaynakları temizleme](media/pause-and-resume-compute-portal/clean-up-resources.png)

1. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. 

2. İşlem veya depolama için ücretlendirilmemek üzere SQL havuzunu kaldırmak için **Sil**' e tıklayın.

3. Oluşturduğunuz SQL Server 'ı kaldırmak için **sqlpoolservername.Database.Windows.net**ve ardından **Sil**' e tıklayın.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

4. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Artık SQL havuzunuz için işlemi duraklattı ve sürdürdü. Sonraki adımlar için veri yükleme öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Verileri SQL havuzuna yükleme](load-data-from-azure-blob-storage-using-polybase.md)
