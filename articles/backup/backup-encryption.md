---
title: Azure Backup'ta şifreleme
description: Azure Backup ' deki şifreleme özelliklerinin, yedekleme verilerinizi korumanıza ve işletmenizin güvenlik ihtiyaçlarını karşılamanıza nasıl yardımcı olduğunu öğrenin.
ms.topic: conceptual
ms.date: 08/04/2020
ms.custom: references_regions
ms.openlocfilehash: 71183e99522707737812096567d877df740c4bae
ms.sourcegitcommit: e2b36c60a53904ecf3b99b3f1d36be00fbde24fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88763652"
---
# <a name="encryption-in-azure-backup"></a>Azure Backup'ta şifreleme

Tüm yedeklenen verileriniz, Azure depolama şifrelemesi kullanılarak bulutta depolandığında, güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olacak şekilde otomatik olarak şifrelenir. Bekleyen bu veriler 256 bit AES şifrelemesi kullanılarak şifrelenir, en güçlü blok şifrelemeleri kullanılabilir ve FIPS 140-2 uyumludur. Bekleyen şifrelemeye ek olarak, yoldaki tüm yedekleme verileriniz HTTPS üzerinden aktarılır. Her zaman Azure omurga ağında kalır.

## <a name="levels-of-encryption-in-azure-backup"></a>Azure Backup şifreleme düzeyleri

Azure Backup iki düzeyde şifreleme içerir:

- **Kurtarma Hizmetleri kasasındaki verilerin şifrelenmesi**
  - **Platform tarafından yönetilen anahtarlar kullanılarak**: varsayılan olarak, tüm verileriniz platform tarafından yönetilen anahtarlar kullanılarak şifrelenir. Bu şifrelemeyi etkinleştirmek için uçtan herhangi bir açık işlem yapmanız gerekmez. Bu, kurtarma hizmetleri kasanıza yedeklenen tüm iş yükleri için geçerlidir.
  - **Müşteri tarafından yönetilen anahtarları kullanma**: Azure sanal makinelerinizi yedeklerken, size ait olan ve yönetilen şifreleme anahtarlarını kullanarak verilerinizi şifrelemeyi seçebilirsiniz. Azure Backup, yedeklemelerinizi şifrelemek için Azure Key Vault depolanan RSA anahtarlarınızı kullanmanıza olanak sağlar. Yedeklemeleri şifrelemek için kullanılan şifreleme anahtarı, kaynak için kullanılan birinden farklı olabilir. Veriler, sırasıyla anahtarlarınız kullanılarak korunan bir AES 256 tabanlı veri şifreleme anahtarı (DEK) kullanılarak korunur. Bu sayede veriler ve anahtarlar üzerinde tam denetim elde edersiniz. Şifrelemeye izin vermek için, kurtarma hizmetleri kasasını Azure Key Vault şifreleme anahtarına vermeniz gerekir. Her gerektiğinde anahtarı devre dışı bırakabilir veya erişimi iptal edebilirsiniz. Ancak, kasadaki herhangi bir öğeyi korumayı denemeden önce anahtarlarınızı kullanarak şifrelemeyi etkinleştirmeniz gerekir. [Daha fazla bilgi edinin](encryption-at-rest-with-cmk.md).
  - **Altyapı düzeyinde şifreleme**: müşteri tarafından yönetilen anahtarları kullanarak kurtarma hizmetleri kasasındaki verilerinizi şifrelemeye ek olarak, depolama altyapısında yapılandırılmış ek bir şifreleme katmanına sahip olmasını da tercih edebilirsiniz. Bu altyapı şifrelemesi platform tarafından yönetiliyor. Müşteri tarafından yönetilen anahtarlar kullanılarak, bekleyen şifrelemeyle birlikte, yedekleme verilerinizin iki katmanlı şifrelemesine izin verir. Altyapı şifrelemesi, yalnızca, bekleyen şifreleme için kendi anahtarlarınızı kullanmayı seçerseniz yapılandırılabilir. Altyapı şifrelemesi, verileri şifrelemek için platform tarafından yönetilen anahtarları kullanır.
- **Yedeklenmekte olan iş yüküne özgü şifreleme**  
  - **Azure sanal makine yedeklemesi**: Azure Backup [platform tarafından yönetilen anahtarlar](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#platform-managed-keys)kullanılarak şifrelenmiş disklere sahip VM 'lerin yedeklenmesini ve size ait ve sizin tarafınızdan yönetilen [müşteri tarafından yönetilen anahtarları](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys) destekler. Ayrıca, işletim sistemi veya veri disklerinin [Azure disk şifrelemesi](backup-azure-vms-encryption.md#encryption-support-using-ade)kullanılarak şifrelendiği Azure sanal makinelerinizi da yedekleyebilirsiniz. ADE, Konuk içi şifrelemeyi gerçekleştirmek için Windows VM 'Leri için BitLocker ve Linux VM 'Ler için DM-Crypt kullanır.

>[!NOTE]
>Altyapı şifrelemesi Şu anda sınırlı önizlemededir ve yalnızca ABD Doğu, US West2, ABD Orta Güney, US Gov Arizona ve ABD GOV Virginia bölgelerinde kullanılabilir. Bu bölgelerin herhangi birinde özelliğini kullanmak istiyorsanız, [Bu formu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUN0VHNEpJS0ZUWklUNVdJSTEzR0hIOVRMVC4u) doldurun ve bize e-posta gönderin [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) .

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure Depolama şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)
- Şifreleme hakkında karşılaşabileceğiniz sorular için [Azure Backup SSS](backup-azure-backup-faq.md#encryption)
