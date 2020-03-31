---
title: İşlerinizi zamanlayın
description: Görevlerinizi yönetmek için iş zamanlamasını kullanın.
services: batch
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.topic: article
ms.workload: big-compute
ms.date: 02/20/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 55ea8fb4cc0e65deaa89d718c4a46513716dcf54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672439"
---
# <a name="schedule-jobs-for-efficiency"></a>Verimlilik için işleri zamanlama

Toplu işlerini zamanlama, diğer görevlere bağımlı olan görevleri hesaba katarak önce çalıştırmak istediğiniz işlere öncelik vermenizi sağlar. İşlerinizi zamanlayarak, en az miktarda kaynak kullandığınızdan emin olabilirsiniz. Düğümler gerekli olmadığında devre dışı bırakılabilir, diğer görevlere bağımlı görevler tam zamanında iş akışlarını optimize eder. Aynı anda sadece bir iş çalışır. Yenisi, bir önceki tamamlanana kadar başlamaz. İşinizi otomatik tamamlama ya da otomatik tamamlama olarak ayarlayabilirsiniz. 

## <a name="benefit-of-job-scheduling"></a>İş planlamasının avantajı

İşleri zamanlamanın yararı, iş oluşturma için bir zamanlama belirtebilmektir. İş yöneticisi görevini kullanarak zamanladığınız görevler bir işle ilişkilidir. İş yöneticisi görevi iş için görevler oluşturur. Bunu yapmak için, iş yöneticisi görevinin Toplu İşlem hesabıyla kimlik doğrulaması gerekir. AZ_BATCH_AUTHENTICATION_TOKEN erişim jetonunu kullanın. Belirteç, işin geri kalanına erişim sağlar. 

## <a name="use-the-portal-to-schedule-a-job"></a>Bir iş planlamak için portalı kullanma

   1. [Azure portalında](https://portal.azure.com/)oturum açın.

   2. İşleri zamanlamak istediğiniz Toplu İş hesabını seçin.

   3. Yeni bir iş çizelgesi oluşturmak ve **Temel formu**tamamlamak için **Ekle'yi** seçin.



![Bir iş zamanlama][1]

**İş çizelgesi Kimliği**: Bu iş zamanlaması için benzersiz tanımlayıcı.

**Ekran adı**: İş için görüntü adı benzersiz olmak zorunda değildir, ancak en fazla 1024 karakter uzunluğundadır.

**Kadar çalıştırmayın**: İşin en erken ne zaman çalışacağını belirtir. Bunu ayarlamazsanız, zamanlama işleri hemen çalıştırmaya hazır hale gelir.

**Sonra çalışma :** Burada ayarladığınız süre den sonra hiçbir iş çalıştırın. Bir zaman belirtmezseniz, bunu açıkça sonlandırana kadar etkin kalan yinelenen bir iş zamanlamasını oluşturursunuz.

**Yineleme aralığı**: İşler arasındaki süreyi belirtebilirsiniz. Zamanlanan bir anda yalnızca bir iş olabilir, bu nedenle bir iş zamanlaması altında yeni bir iş oluşturma nın zamanı geldiyse, ancak önceki iş hala çalışıyorsa, Toplu İşlem hizmeti önceki iş bitene kadar yeni iş oluşturmaz.  

**Başlangıç penceresi**: Burada zamanlamanın bir işin oluşturulması gerektiğini gösterdiği andan başlayarak, tamamlanması gerekene kadar zaman aralığını belirtirsiniz. Geçerli iş penceresi sırasında tamamlanmazsa, bir sonraki iş başlamaz.

Temel formun alt kısmında, işin çalışmasını istediğiniz havuzu belirtirsiniz. Havuz kimliği bilgilerinizi bulmak için **Güncelleştir'i**seçin. 

![Havuzu belirtin][2]


**Havuz Kimliği**: İşi çalıştıracağınız havuzu belirleyin.

**İş yapılandırma görevi**: İş Yöneticisi görevini ve bunları kullanıyorsanız İş hazırlama ve bırakma görevlerini adlandırmak için **Güncelleştirme'yi** seçin.

**Öncelik**: İşe öncelik verin.

**Maksimum duvar saati süresi**: İşin çalıştırabileceği maksimum süreyi ayarlayın. Bu süre içinde tamamlanmamışsa, Toplu Iş işi sona erdirir. Bunu ayarlamazsanız, iş için bir zaman sınırı yoktur.

**Maksimum görev yeniden deneme sayısı**: Bir görevin en fazla dört kez yeniden denenme sayısını belirtin. Bu, bir API çağrısının sahip olabileceği yeniden deneme sayısıyla aynı değildir.

**Tüm görevler tamamlandığında**: Varsayılan eylem değildir.

**Bir görev başarısız olduğunda**: Varsayılan eylem değildir. Yeniden deneme sayısı tükenmişse veya görevi başlatırken bir hata varsa görev başarısız olur. 

**Kaydet'i**seçtikten sonra, sol navigasyonda **İş zamanlamalarına** giderseniz, **Yürütme bilgilerini**seçerek işin yürütülmesini izleyebilirsiniz.


## <a name="for-more-information"></a>Daha fazla bilgi edinmek için

Azure CLI'yi kullanarak bir işi yönetmek için [az toplu iş zamanlamasına](https://docs.microsoft.com/cli/azure/batch/job-schedule?view=azure-cli-latest)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Diğer görevlere bağlı görevleri çalıştırmak için görev bağımlılıkları oluşturun.](batch-task-dependencies.md)





[1]: ./media/batch-job-schedule/add_job_schedule-02.png
[2]: ./media/batch-job-schedule/add_job_schedule-03.png


