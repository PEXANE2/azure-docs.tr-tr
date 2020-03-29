---
title: Azure Veri Bilimi Sanal Makineleri Kullanma
description: Azure Notebook Önizleme'de kullanılabilen bilgi işlem gücünü genişletmek için bir Azure Veri Bilimi Sanal Makinesine (DSVM) nasıl bağlanabileceğinizi öğrenin.
author: getroyer
manager: andneil
ms.author: getroyer
ms.topic: how-to
ms.date: 06/13/2019
ms.openlocfilehash: b4da63b7b2a6da4316215b85a09ca7420745251c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898411"
---
# <a name="use-azure-data-science-virtual-machines"></a>Azure Veri Bilimi Sanal Makineleri Kullanma

Varsayılan olarak, projeler kötüye kullanımı önlemek için 4 GB bellek ve 1 GB veri ile sınırlı **olan Ücretsiz Bilgi İşlem** katmanında çalışır. Azure aboneliğinde sizin için sağlanan farklı bir sanal makine yi kullanarak bu sınırlamaları atlayabilirsiniz. Bu amaçla, en iyi **seçim, Linux için Veri Bilimi Sanal Makine (Ubuntu)** görüntüsünü kullanan bir Azure Veri Bilimi Sanal Makinesi (DSVM) seçeneğidir. Böyle bir DSVM, Azure Dizüstü Bilgisayarlar için ihtiyacınız olan her şeyle önceden yapılandırılmış olarak gelir ve Azure Not Defterleri'ndeki **Çalıştır** açılır listesinde otomatik olarak görünür.

> [!Note]
> Azure Dizüstü Bilgisayarlar yalnızca Linux Ubuntu görüntüsü yle oluşturulan DSVM'lerde desteklenir. Dizüstü bilgisayarlar Windows 2012, Windows 2016 veya Linux CentOS görüntülerinde desteklenmez.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="create-a-dsvm-instance"></a>DSVM örneği oluşturma

Yeni bir DSVM örneği oluşturmak [için, Ubuntu Veri Bilimi VM oluştur'daki](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)yönergeleri izleyin. Fiyatlandırma ayrıntıları da dahil olmak üzere daha fazla bilgi için [Data Science Sanal Makineler'e](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)bakın.

## <a name="connect-to-the-dsvm"></a>DSVM'ye bağlanın

DSVM oluşturulduktan sonra, Azure Not Defterleri proje panosundaki **Açılan** Çalışma listesini seçin ve uygun DSVM örneğini seçin. Açılan liste, aşağıdaki koşullar doğruysa DSVM örneklerini gösterir:

- Azure Dizüstü Bilgisayarlar'da şirket hesabı gibi Azure Active Directory (AAD) kullanan bir hesapla oturum açmışsınız.
- Hesabınız bir Azure aboneliğine bağlı.
- Linux (Ubuntu) görüntü için Veri Bilimi Sanal Makine kullanan, en az Reader erişimi ile, bu abonelik bir veya daha fazla sanal makine var.)

![Proje panosundaki açılır listede veri bilimi sanal makine örnekleri](media/project-compute-tier-dsvm.png)

Bir DSVM örneği seçtiğinizde, Azure Not Defterleri VM'yi oluşturduğunuzda kullanılan belirli makine kimlik bilgilerini sizden isteyebilir.

> [!Important]
> JupyterHub ile kullanmak için kullanıcı adı küçük olmalıdır.

Koşullardan herhangi biri yerine getirilmezse, Yine de DSVM'ye bağlanabilirsiniz. Açılan listede, bir ad (listede göstermek için), VM'nin IP adresi ve bağlantı noktası (genellikle 8000, JupyterHub'ın dinlediği varsayılan bağlantı noktası) ve VM kimlik bilgilerini gerektiren **Doğrudan İşlem** seçeneğini seçin:

![Doğrudan İşlem seçeneği için sunucu bilgilerini toplama istemi](media/project-compute-tier-direct.png)

Bu değerleri Azure portalındaki DSVM sayfasından elde edeyim.

## <a name="accessing-azure-notebooks-files-from-the-dsvm"></a>Azure Notebook dosyalarına DSVM'den erişim

Dosya sistemi erişimi DSVM sürümleri 19.06.15 veya daha sonra için desteklenir. Sürümü denetlemek için önce SSH üzerinden DSVM'nize bağlanın, `curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2018-10-01"` ardından aşağıdaki komutu çalıştırın: (burada gösterilen ip adresini tam olarak kullanmanız gerekir). Sürüm numarası "sürüm" için çıktı gösterilir.

Dosya yollarının **Serbest İşlem** katmanıyla eşitliğini korumak için, DSVM'de aynı anda yalnızca bir proje açabilirsiniz. Yeni bir proje açmak için önce açık projeyi kapatmanız gerekir.

Bir proje VM'de çalıştırıldığında, dosyalar varsayılan Azure Not Defteri dosyalarının yerine Jupyter sunucusunun (JupyterHub'da gösterilen dizini) kök dizinine monte edilir. Azure Notebook, not defterindeki **Kapatma** düğmesini kullanarak VM'yi kapattığınızda varsayılan dosyaları geri yükler.

![Azure Dizüstü Bilgisayarlarda kapatma düğmesi](media/shutdown.png)

## <a name="create-new-dsvm-users"></a>Yeni DSVM kullanıcıları oluşturun

Birden çok kullanıcı bir DSVM paylaşıyorsa, her dizüstü bilgisayar kullanıcısı için bir DSVM kullanıcısı oluşturup kullanarak birbirlerini engellemeyi önleyebilirsiniz:

1. Azure [portalında](https://portal.azure.com)sanal makinenize gidin.
1. Sol kenar boşluğunda **Destek + sorun giderme** altında **parolayı sıfırla'yı**seçin.
1. Yeni bir **kullanıcı adı**girin. JupyterHub ile kullanmak için kullanıcı adı küçük olmalıdır. Parola girin. Sonra **Güncelleştir'i**seçin. (Varolan kullanıcı adları etkilenmez.)
1. Ek kullanıcılar için önceki adımı yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Bilimi sanal [makinelerine giriş](/azure/machine-learning/data-science-virtual-machine/overview)konusunda DSVM'ler hakkında daha fazla bilgi edinin.
