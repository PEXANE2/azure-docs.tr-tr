---
title: Veri aktarımı için bir Azure çözümü seçin| Microsoft Dokümanlar
description: Ortamınızdaki veri boyutlarına ve kullanılabilir ağ bant genişliğine bağlı olarak veri aktarımı için azure çözümünüzü nasıl seçeceğinizi öğrenin
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: ada96d0d4b167e30cb6e271aa02d0d399a9ae7d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303095"
---
# <a name="choose-an-azure-solution-for-data-transfer"></a>Veri aktarımı için bir Azure çözümü seçin

Bu makalede, bazı ortak Azure veri aktarım çözümlerine genel bir bakış sağlar. Makale ayrıca, ortamınızdaki ağ bant genişliğine ve aktarmak istediğiniz verilerin boyutuna bağlı olarak önerilen seçeneklere de bağlanır.

## <a name="types-of-data-movement"></a>Veri hareketi türleri

Veri aktarımı çevrimdışı veya ağ bağlantısı üzerinden olabilir. Aşağıdakilerinize bağlı olarak çözümünüzü seçin:

- **Veri boyutu** - Aktarıma yönelik verilerin boyutu,
- **Aktarım sıklığı** - Tek seferlik veya periyodik veri alımı ve
- **Ağ** – Çevrenizde veri aktarımı için bant genişliği kullanılabilir.

Veri hareketi aşağıdaki türlerden olabilir:

- **Sevk edilebilir aygıtlar kullanarak çevrimdışı aktarım** - Çevrimdışı tek seferlik toplu veri aktarımı yapmak istediğinizde fiziksel sevk edilebilir aygıtları kullanın. Microsoft size bir disk veya güvenli özel leştirilmiş bir aygıt gönderir. Alternatif olarak, kendi disklerinizi satın alabilir ve sevk edebilirsiniz. Verileri aygıta kopyalar ve ardından verilerin yüklendiği Azure'a aktarırsınız.  Bu servis talebi için kullanılabilir seçenekler Veri Kutusu Diski, Veri Kutusu, Veri Kutusu Ağır ve İçe Aktarma/Verme (kendi disklerinizi kullanın) dır.

- **Ağ Aktarımı** - Verilerinizi ağ bağlantınız üzerinden Azure'a aktarAbilirsiniz. Bu birçok şekilde yapılabilir.

    - **Grafik arabirim** - Zaman zaman yalnızca birkaç dosya aktarıyorsanız ve veri aktarımını otomatikleştirmeniz gerekmiyorsa, Azure Depolama Gezgini gibi bir grafik arabirim aracı veya Azure portalında web tabanlı bir arama aracı seçebilirsiniz.
    - **Komut dosyası veya programatik aktarım** - Sunduğumuz veya doğrudan REST API'lerimizi/SDK'larımızı adlandırdığımız optimize edilmiş yazılım araçlarını kullanabilirsiniz. Kullanılabilir komut dosyası araçları AzCopy, Azure PowerShell ve Azure CLI'dir. Programlı arayüz için .NET, Java, Python, Node/JS, C++, Go, PHP veya Ruby için SDK'lardan birini kullanın.
    - **Şirket içi aygıtlar** - Size veri merkezinizde bulunan ve ağ üzerinden veri aktarımını optimize eden fiziksel veya sanal bir cihaz sağlıyoruz. Bu aygıtlar, sık kullanılan dosyaların yerel önbelleği de sağlar. Fiziksel aygıt Veri Kutusu Kenarı ve sanal aygıt Veri Kutusu Ağ Geçidi'dir. Her ikisi de binalarınızda kalıcı olarak çalışır ve ağ üzerinden Azure'a bağlanır.
    - **Yönetilen veri ardışık lığı** - Dosyaları birkaç Azure hizmeti, şirket içi veya iki sinin birleşimi arasında düzenli olarak aktarmak için bir bulut ardışık düzenehattı ayarlayabilirsiniz. Veri ardışık hatlarını kurmak ve yönetmek ve verileri analiz için taşımak ve dönüştürmek için Azure Veri Fabrikası'nı kullanın.

Aşağıdaki görsel, aktarım için kullanılabilir ağ bant genişliğine, aktarım için tasarlanan veri boyutuna ve aktarım sıklığına bağlı olarak çeşitli Azure veri aktarım araçlarını seçme yönergelerini göstermektedir.

![Azure veri aktarım araçları](media/storage-choose-data-transfer-solution/azure-data-transfer-options-3.png)

**Çevrimdışı aktarım aygıtlarının üst sınırları - Veri Kutusu Diski, Veri Kutusu ve Veri Kutusu Ağır, bir aygıt türünden birden fazla sipariş verilerek genişletilebilir.*

## <a name="selecting-a-data-transfer-solution"></a>Veri aktarım çözümü seçme

Veri aktarım çözümü seçmenize yardımcı olmak için aşağıdaki soruları yanıtlayın:

- Kullanılabilir ağ bant genişliğiniz sınırlı mı yoksa yok mu ve büyük veri kümelerini aktarmak mı istiyorsunuz?
  
    Evet ise, bakınız: [Senaryo 1: Büyük veri kümelerini ağ bant genişliği olmayan veya düşük olan aktarın.](storage-solution-large-dataset-low-network.md)
- Ağ üzerinden büyük veri kümeleri aktarmak istiyor musunuz ve orta ve yüksek ağ bant genişliği var mı?

    Evet ise, bakınız: [Senaryo 2: Büyük veri kümelerini orta ve yüksek ağ bant genişliğine aktarın.](storage-solution-large-dataset-moderate-high-network.md)
- Bazen ağ üzerinden sadece birkaç dosya aktarmak istiyor musunuz?

    Evet ise, [bkz. Senaryo 3: Küçük veri kümelerini sınırlı ve orta ağ bant genişliğiyle aktarın.](storage-solution-small-dataset-low-moderate-network.md)
- Düzenli aralıklarla zaman içinde veri aktarımı mı arıyorsunuz?

    Evet ise, Senaryo 4'te özetlenen komut dosyası/programlı seçenekleri [kullanın: Periyodik veri aktarımları.](storage-solution-periodic-data-transfer.md)
- Sürekli veri aktarımı mı arıyorsunuz?

    Evet ise, Senaryo 4'teki seçenekleri [kullanın: Periyodik veri aktarımları.](storage-solution-periodic-data-transfer.md)

## <a name="data-transfer-feature-in-azure-portal"></a>Azure portalında veri aktarım özelliği

Azure portalındaki Azure Depolama hesabınıza gidebilir ve **Veri aktarımı** özelliğini seçebilirsiniz. Ortamınızdaki ağ bant genişliğini, aktarmak istediğiniz verilerin boyutunu ve veri aktarım sıklığını sağlayın. Sağladığınız bilgilere karşılık gelen en uygun veri aktarım çözümlerini göreceksiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Depolama Gezgini'ne giriş alın.](https://azure.microsoft.com/resources/videos/introduction-to-microsoft-azure-storage-explorer/)
- [AzCopy'nin genel görünümünü okuyun.](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)
- [Azure Depolama ile Azure PowerShell'i kullanma](https://docs.microsoft.com/azure/storage/common/storage-powershell-guide-full)
- [Quickstart: Azure CLI ile blob oluşturma, indirme ve listele](../blobs/storage-quickstart-blobs-cli.md)
- Şunları öğrenin:

    - [Çevrimdışı aktarımlar için Azure Veri Kutusu, Azure Veri Kutusu Diski ve Azure Veri Kutusu Ağır.](https://docs.microsoft.com/azure/databox/)
    - [Çevrimiçi aktarımlar için Azure Veri Kutusu Ağ Geçidi ve Azure Veri Kutusu Kenarı.](https://docs.microsoft.com/azure/databox-online/)
- [Azure Veri Fabrikası nedir öğrenin.](https://docs.microsoft.com/azure/data-factory/copy-activity-overview)
- Veri aktarmak için REST API'lerini kullanın

    - [In .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
    - [Java üzerinde](https://docs.microsoft.com/java/api/overview/azure/storage)
