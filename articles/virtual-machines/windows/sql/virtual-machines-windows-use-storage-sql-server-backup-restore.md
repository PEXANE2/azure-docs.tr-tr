---
title: SQL Server yedeklemesi için Azure depolama nasıl kullanılır ve geri yükleme | Microsoft Dokümanlar
description: SQL Server'ı Azure Depolama'ya nasıl yedekleyin öğrenin. SQL veritabanlarını Azure Depolama'ya yedeklemenin avantajlarını açıklar.
services: virtual-machines-windows
documentationcenter: ''
author: MikeRayMSFT
manager: craigg
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mikeray
ms.openlocfilehash: cb19dc7262721e672bd3f54b32db9188dad7fee0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70101884"
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>SQL Server Yedekleme ve Geri Yükleme için Azure Depolama’yı Kullanma
## <a name="overview"></a>Genel Bakış
SQL Server 2012 SP1 CU2 ile başlayarak, artık SQL Server yedeklemelerini doğrudan Azure Blob depolama hizmetine yazabilirsiniz. Bu işlevi, azure sanal makinesinde şirket içi BIR SQL Server veritabanı veya SQL Server veritabanı ile Azure Blob hizmetine yedeklemek ve geri yüklemek için kullanabilirsiniz. Buluta yedekleme, kullanılabilirlik, sınırsız coğrafi olarak çoğaltılan site dışı depolama ve buluta ve buluttan veri geçişi kolaylığı avantajları sunar. Transact-SQL veya SMO kullanarak BACKUP veya RESTORE deyimleri verebilirsiniz.

SQL Server 2016 yeni özellikler sunar; neredeyse anlık yedeklemeler ve inanılmaz hızlı geri yüklemeler gerçekleştirmek için [dosya anlık yedekleme](https://msdn.microsoft.com/library/mt169363.aspx) kullanabilirsiniz.

Bu konu, SQL yedeklemeleri için neden Azure depolama alanını kullanmayı seçebileceğinizi açıklar ve sonra ilgili bileşenleri açıklar. Bu hizmeti SQL Server yedeklemelerinizle kullanmaya başlamak için gözden geçirme lere ve ek bilgilere erişmek için makalenin sonunda sağlanan kaynakları kullanabilirsiniz.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>SQL Server Yedeklemeleri için Azure Blob Hizmetini Kullanmanın Avantajları
SQL Server'ı yedeklerken karşılaştığınız çeşitli zorluklar vardır. Bu zorluklar arasında depolama yönetimi, depolama hatası riski, saha dışı depolama alanına erişim ve donanım yapılandırması yer almaktadır. Bu zorlukların çoğu, SQL Server yedeklemeleri için Azure Blob mağaza hizmetini kullanarak giderilir. Aşağıdaki avantajları göz önünde bulundurun:

* **Kullanım kolaylığı**: Yedeklemelerinizi Azure lekelerinde depolamak kullanışlı, esnek ve iş yeri dışında kolay bir seçenek olabilir. SQL Server yedeklemeleriniz için site dışı depolama alanı oluşturmak, **BACKUP TO URL** sözdizimini kullanmak için varolan komut dosyalarınızı/işlerinizi değiştirmek kadar kolay olabilir. Şirket dışı depolama, hem saha dışı hem de üretim veritabanı konumlarını etkileyebilecek tek bir felaketi önlemek için genellikle üretim veritabanı konumundan yeterince uzakta olmalıdır. [Azure lekelerinizi coğrafi olarak çoğaltmayı](../../../storage/common/storage-redundancy.md)seçerek, tüm bölgeyi etkileyebilecek bir felaket durumunda fazladan bir koruma katmanına sahip siniz.
* **Yedekleme arşivi**: Azure Blob Depolama hizmeti, yedeklemeleri arşivlemek için sık kullanılan teyp seçeneğine daha iyi bir alternatif sunar. Teyp depolama, tesis dışı bir tesise fiziksel taşıma gerektirebilir ve medyayı korumak için önlemler alabilir. Yedeklemelerinizi Azure Blob Depolama'da depolamak anında, yüksek oranda kullanılabilir ve dayanıklı bir arşivleme seçeneği sunar.
* **Yönetilen donanım**: Azure hizmetleriyle donanım yönetiminin ek yükü yoktur. Azure hizmetleri donanımı yönetir ve donanım hatalarına karşı artıklık ve koruma için coğrafi çoğaltma sağlar.
* **Sınırsız depolama**: Azure blobs doğrudan yedekleme etkinleştirerek, neredeyse sınırsız depolama erişimi vardır. Alternatif olarak, bir Azure sanal makine diskine yedeklemenin makine boyutuna bağlı olarak sınırları vardır. Yedeklemeler için bir Azure sanal makinesine ekebileceğiniz disk sayısının bir sınırı vardır. Bu sınır, ekstra büyük bir örnek için 16 disk ve daha küçük örnekler için daha azdır.
* **Yedekleme kullanılabilirliği**: Azure blob'larında depolanan yedeklemeler her yerden ve her zaman kullanılabilir ve veritabanı ekleme/ayırma veya VHD'yi indirmeye ve bağlamaya gerek kalmadan şirket içi bir SQL Server'a veya Azure Virtual Machine'de çalışan başka bir SQL Server'a geri yüklemeler için kolayca erişilebilir.
* **Maliyet**: Yalnızca kullanılan hizmet için ödeme. Bir off-site ve yedekleme arşiv seçeneği olarak maliyet etkin olabilir. Daha fazla bilgi için [Azure fiyatlandırma hesaplayıcısına](https://go.microsoft.com/fwlink/?LinkId=277060 "Fiyatlandırma Hesaplayıcı")ve [Azure Fiyatlandırması makalesine](https://go.microsoft.com/fwlink/?LinkId=277059 "Fiyatlandırma makalesi") bakın.
* **Depolama anlık görüntüleri**: Veritabanı dosyaları bir Azure blob'unda depolandığında ve SQL Server 2016 kullanıyorsanız, neredeyse anlık yedeklemeler ve inanılmaz hızlı geri yüklemeler gerçekleştirmek için [dosya anlık yedeklemeyi](https://msdn.microsoft.com/library/mt169363.aspx) kullanabilirsiniz.

Daha fazla bilgi için [Azure Blob Depolama Hizmeti ile SQL Server Yedekleme ve Geri Yükleme'ye](https://go.microsoft.com/fwlink/?LinkId=271617)bakın.

Aşağıdaki iki bölüm, gerekli SQL Server bileşenleri de dahil olmak üzere Azure Blob depolama hizmetini tanıtır. Azure Blob depolama hizmetinden yedeklemeyi başarıyla kullanmak ve geri yüklemek için bileşenleri ve etkileşimlerini anlamak önemlidir.

## <a name="azure-blob-storage-service-components"></a>Azure Blob Depolama Hizmeti Bileşenleri
Aşağıdaki Azure bileşenleri, Azure Blob depolama hizmetine yedekleme yaparken kullanılır.

| Bileşen | Açıklama |
| --- | --- |
| **Depolama Hesabı** |Depolama hesabı, tüm depolama hizmetleri için başlangıç noktasıdır. Bir Azure Blob Depolama hizmetine erişmek için önce bir Azure Depolama hesabı oluşturun. Azure Blob depolama hizmeti hakkında daha fazla bilgi için Azure [Blob Depolama Hizmeti'ni nasıl kullanacağınız](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Kapsayıcı** |Kapsayıcı, bir dizi blob'un gruplandırmasını sağlar ve sınırsız sayıda Blob depolayabilir. Bir Azure Blob hizmetine SQL Server yedeklemesi yazmak için en azından oluşturulan kök kapsayıcınız olmalıdır. |
| **Blob** |Herhangi bir tür ve boyutta bir dosya. Blobs aşağıdaki URL biçimi kullanılarak adreslenebilir: **https://[depolama hesabı].blob.core.windows.net/[konteyner]/[blob]**. Sayfa Blobs hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Sunucu Bileşenleri
Azure Blob depolama hizmetine yedekleme yaparken aşağıdaki SQL Server bileşenleri kullanılır.

| Bileşen | Açıklama |
| --- | --- |
| **URL** |URL, benzersiz bir yedekleme dosyasına Tek düzen Kaynak Tanımlayıcısı (URI) belirtir. URL, SQL Server yedekleme dosyasının konumunu ve adını sağlamak için kullanılır. URL, sadece bir kapsayıcıyı değil, gerçek bir lekeyi işaret etmelidir. Leke yoksa, oluşturulur. Varolan bir blob belirtilirse, FORMAT seçeneği yle > belirtilmedikçe BACKUP başarısız olur. Aşağıdaki YEDEKLEME komutunda belirteceğiniz URL örneği: **http[s]://[storageaccount].blob.core.windows.net/[konteyner]/[FILENAME.bak]**. HTTPS önerilir, ancak gerekli değildir. |
| **Kimlik Bilgisi** |Azure Blob depolama hizmetine bağlanmak ve kimlik doğrulaması yapmak için gereken bilgiler Kimlik Bilgisi olarak depolanır.  SQL Server'ın bir Azure Blob'una yedekleme yazabilmesi veya ondan geri yükleyebilmesi için bir SQL Server kimlik bilgisi oluşturulması gerekir. Daha fazla bilgi için [SQL Server Kimlik Bilgileri'ne](https://msdn.microsoft.com/library/ms189522.aspx)bakın. |

> [!NOTE]
> Sql Server 2016 blok blobs desteklemek için güncellendi. Daha fazla bilgi için [lütfen Öğretici: SQL Server 2016 veritabanlarına sahip Microsoft Azure Blob depolama hizmetini kullanma](https://msdn.microsoft.com/library/dn466438.aspx) tarihine bakın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
1. Zaten hesabınız yoksa bir Azure hesabı oluşturun. Azure'u değerlendiriyorsanız, [ücretsiz deneme sürümünü](https://azure.microsoft.com/free/)göz önünde bulundurun.
2. Ardından, bir depolama hesabı oluşturma ve geri yükleme gerçekleştirme de size yol gösteren aşağıdaki öğreticilerden birine bakın.
   
   * **SQL Server 2014**: [Tutorial: SQL Server 2014 Yedekleme ve Microsoft Azure Blob Depolama Hizmetine Geri Yükleme](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [Tutorial: SQL Server 2016 veritabanları ile Microsoft Azure Blob depolama hizmetini kullanma](https://msdn.microsoft.com/library/dn466438.aspx)
3. [Microsoft Azure Blob Depolama Hizmeti ile SQL Server Yedekleme ve Geri Yükleme ile](https://msdn.microsoft.com/library/jj919148.aspx)başlayan ek belgeleri gözden geçirin.

Herhangi bir sorununuz varsa, SQL Server Backup konusunu [URL En İyi Uygulamalar ve Sorun Giderme konusuna](https://msdn.microsoft.com/library/jj919149.aspx)gözden geçirin.

Diğer SQL Server yedekleme ve geri yükleme seçenekleri [için Azure Sanal Makinelerde SQL Server için Yedekleme ve Geri Yükleme'ye](virtual-machines-windows-sql-backup-recovery.md)bakın.

