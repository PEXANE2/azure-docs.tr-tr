---
title: Azure Backup'ta şifreleme
description: Azure Backup ' deki şifreleme özelliklerinin, yedekleme verilerinizi korumanıza ve işletmenizin güvenlik ihtiyaçlarını karşılamanıza nasıl yardımcı olduğunu öğrenin.
ms.topic: conceptual
ms.date: 04/30/2020
ms.custom: references_regions
ms.openlocfilehash: 099e736bfb321f0f92bd3a57f9c24e88293b42bb
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86538760"
---
# <a name="encryption-in-azure-backup"></a>Azure Backup'ta şifreleme

Tüm yedeklenen verileriniz, Azure depolama şifrelemesi kullanılarak bulutta depolandığında, güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olacak şekilde otomatik olarak şifrelenir. Bekleyen bu veriler 256 bit AES şifrelemesi kullanılarak şifrelenir, en güçlü blok şifrelemeleri kullanılabilir ve FIPS 140-2 uyumludur.

Bekleyen şifrelemeye ek olarak, yoldaki tüm yedekleme verileriniz HTTPS üzerinden aktarılır. Her zaman Azure omurga ağında kalır.

Daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md). Şifreleme hakkında karşılaşabileceğiniz soruları yanıtlamak için [Azure Backup SSS](./backup-azure-backup-faq.md#encryption) bölümüne bakın.

## <a name="encryption-of-backup-data-using-platform-managed-keys"></a>Platform tarafından yönetilen anahtarlar kullanılarak yedekleme verilerinin şifrelenmesi

Varsayılan olarak, tüm verileriniz platform tarafından yönetilen anahtarlar kullanılarak şifrelenir. Bu şifrelemeyi etkinleştirmek için herhangi bir açık işlem yapmanız gerekmez ve kurtarma hizmetleri kasanıza yedeklenen tüm iş yükleri için geçerlidir.

## <a name="encryption-of-backup-data-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinin şifrelenmesi

Azure sanal makinelerinizi yedeklerken artık size ait olan ve yönetilen anahtarları kullanarak verilerinizi şifreleyebilirsiniz. Azure Backup, yedeklemelerinizi şifrelemek için Azure Key Vault depolanan RSA anahtarlarınızı kullanmanıza olanak sağlar. Yedeklemeleri şifrelemek için kullanılan şifreleme anahtarı, kaynak için kullanılan birinden farklı olabilir. Veriler, sırasıyla anahtarlarınız kullanılarak korunan bir AES 256 tabanlı veri şifreleme anahtarı (DEK) kullanılarak korunur. Bu sayede veriler ve anahtarlar üzerinde tam denetim elde edersiniz. Şifrelemeye izin vermek için, kurtarma hizmetleri kasasının Azure Key Vault şifreleme anahtarına erişim verilmesi gerekir. Her gerektiğinde anahtarı devre dışı bırakabilir veya erişimi iptal edebilirsiniz. Ancak, kasadaki herhangi bir öğeyi korumayı denemeden önce anahtarlarınızı kullanarak şifrelemeyi etkinleştirmeniz gerekir.

[Burada](encryption-at-rest-with-cmk.md)müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinizi şifrelemek hakkında daha fazla bilgi edinin.

## <a name="backup-of-managed-disk-vms-encrypted-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmiş yönetilen disk VM 'lerinin yedeklemesi

Azure Backup Ayrıca, [depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md)için anahtarınızı kullanan Azure VM 'lerinizi yedeklemenizi sağlar. Diskleri şifrelemek için kullanılan anahtar Azure Key Vault depolanır ve sizin tarafınızdan yönetilir. Depolama Hizmeti Şifrelemesi (SSE), müşteri tarafından yönetilen anahtarlar kullanılarak Azure disk şifrelemesi 'nden farklıdır, çünkü ADE, Konuk içi şifrelemeyi gerçekleştirmek için BitLocker (Windows için) ve DM-crypt (Linux için) için, SSE, depolama hizmetindeki verileri şifreler ve VM 'niz için herhangi bir işletim sistemi veya görüntü kullanmanıza olanak sağlar. Daha fazla ayrıntı için [, müşteri tarafından yönetilen anahtarlarla yönetilen disklerin şifrelenmesi](../virtual-machines/windows/disk-encryption.md#customer-managed-keys) bölümüne bakın.

## <a name="infrastructure-level-encryption-for-backup-data"></a>Yedekleme verileri için altyapı düzeyinde şifreleme

Kurtarma Hizmetleri kasasındaki verilerinizi, müşteri tarafından yönetilen anahtarları kullanarak şifrelemeye ek olarak, depolama altyapısında yapılandırılmış ek bir şifreleme katmanına sahip olmasını da tercih edebilirsiniz. Bu altyapı şifrelemesi, platform tarafından yönetilir ve müşterinin yönettiği anahtarlar kullanılarak bekleyen şifrelemeyle birlikte, yedekleme verilerinizin iki katmanlı şifrelemesine izin verir. Altyapı şifrelemenin yalnızca, bekleyen şifreleme için kendi anahtarlarınızı kullanmayı tercih ederseniz yapılandırılabileceğini not edilmelidir. Altyapı şifrelemesi, verileri şifrelemek için platform tarafından yönetilen anahtarları kullanır.

>[!NOTE]
>Altyapı şifrelemesi Şu anda sınırlı önizlemededir ve yalnızca ABD Doğu, US West2, ABD Orta Güney, US Gov Arizona ve ABD GOV Virginia bölgelerinde kullanılabilir. Bu bölgelerin herhangi birinde özelliğini kullanmak istiyorsanız, lütfen [Bu formu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUN0VHNEpJS0ZUWklUNVdJSTEzR0hIOVRMVC4u) doldurun ve bize e-posta gönderin [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) .

## <a name="backup-of-vms-encrypted-using-ade"></a>ADE kullanılarak şifrelenen VM 'lerin yedeklenmesi

Azure Backup ile, işletim sistemi veya veri disklerinin Azure disk şifrelemesi kullanılarak şifrelendiği Azure sanal makinelerinizi da yedekleyebilirsiniz. ADE, Konuk içi şifrelemeyi gerçekleştirmek için Windows VM 'Leri için BitLocker ve Linux sanal makineleri için DM-Crypt kullanır. Ayrıntılar için bkz. [Azure Backup ile şifrelenmiş sanal makineleri yedekleme ve geri yükleme](./backup-azure-vms-encryption.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Şifrelenmiş bir Azure VM 'yi yedekleme ve geri yükleme](backup-azure-vms-encryption.md)
