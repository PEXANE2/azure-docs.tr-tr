---
title: Azure Backup şifreleme
description: Azure Backup ' deki şifreleme özelliklerinin, yedekleme verilerinizi korumanıza ve işletmenizin güvenlik ihtiyaçlarını karşılamanıza nasıl yardımcı olduğunu öğrenin.
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: 0a3f4db4d248d2534cfebd617be0f3ccc9647f15
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84807729"
---
# <a name="encryption-in-azure-backup"></a>Azure Backup şifreleme

Tüm yedeklenen verileriniz, Azure depolama şifrelemesi kullanılarak bulutta depolandığında, güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olacak şekilde otomatik olarak şifrelenir. Bekleyen bu veriler 256 bit AES şifrelemesi kullanılarak şifrelenir, en güçlü blok şifrelemeleri kullanılabilir ve FIPS 140-2 uyumludur.

Bekleyen şifrelemeye ek olarak, yoldaki tüm yedekleme verileriniz HTTPS üzerinden aktarılır. Her zaman Azure omurga ağında kalır.

Daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption). Şifreleme hakkında karşılaşabileceğiniz soruları yanıtlamak için [Azure Backup SSS](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#encryption) bölümüne bakın.

## <a name="encryption-of-backup-data-using-platform-managed-keys"></a>Platform tarafından yönetilen anahtarlar kullanılarak yedekleme verilerinin şifrelenmesi

Varsayılan olarak, tüm verileriniz platform tarafından yönetilen anahtarlar kullanılarak şifrelenir. Bu şifrelemeyi etkinleştirmek için herhangi bir açık işlem yapmanız gerekmez ve kurtarma hizmetleri kasanıza yedeklenen tüm iş yükleri için geçerlidir.

## <a name="encryption-of-backup-data-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinin şifrelenmesi

Azure sanal makinelerinizi yedeklerken artık size ait olan ve yönetilen anahtarları kullanarak verilerinizi şifreleyebilirsiniz. Azure Backup, yedeklemelerinizi şifrelemek için Azure Key Vault depolanan RSA anahtarlarınızı kullanmanıza olanak sağlar. Yedeklemeleri şifrelemek için kullanılan şifreleme anahtarı, kaynak için kullanılan birinden farklı olabilir. Veriler, sırasıyla anahtarlarınız kullanılarak korunan bir AES 256 tabanlı veri şifreleme anahtarı (DEK) kullanılarak korunur. Bu sayede veriler ve anahtarlar üzerinde tam denetim elde edersiniz. Şifrelemeye izin vermek için, kurtarma hizmetleri kasasının Azure Key Vault şifreleme anahtarına erişim verilmesi gerekir. Her gerektiğinde anahtarı devre dışı bırakabilir veya erişimi iptal edebilirsiniz. Ancak, kasadaki herhangi bir öğeyi korumayı denemeden önce anahtarlarınızı kullanarak şifrelemeyi etkinleştirmeniz gerekir.

>[!NOTE]
>Bu özellik şu anda sınırlı kullanılabilirliğe sahip. Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinizi şifrelemek istiyorsanız lütfen [Bu anketi](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapURE9TTDRIUEUyNFhNT1lZS1BNVDdZVllHWi4u) ve e-postayı doldurun AskAzureBackupTeam@microsoft.com . Bu özelliği kullanma yeteneğinin Azure Backup hizmetten onaya tabi olduğunu unutmayın.

## <a name="backup-of-managed-disk-vms-encrypted-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmiş yönetilen disk VM 'lerinin yedeklemesi

Azure Backup Ayrıca, [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)için anahtarınızı kullanan Azure VM 'lerinizi yedeklemenizi sağlar. Diskleri şifrelemek için kullanılan anahtar Azure Key Vault depolanır ve sizin tarafınızdan yönetilir. Depolama Hizmeti Şifrelemesi (SSE), müşteri tarafından yönetilen anahtarlar kullanılarak Azure disk şifrelemesi 'nden farklıdır, çünkü ADE, Konuk içi şifrelemeyi gerçekleştirmek için BitLocker (Windows için) ve DM-crypt (Linux için) için, SSE, depolama hizmetindeki verileri şifreler ve VM 'niz için herhangi bir işletim sistemi veya görüntü kullanmanıza olanak sağlar. Daha fazla ayrıntı için [, müşteri tarafından yönetilen anahtarlarla yönetilen disklerin şifrelenmesi](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys) bölümüne bakın.

## <a name="backup-of-vms-encrypted-using-ade"></a>ADE kullanılarak şifrelenen VM 'lerin yedeklenmesi

Azure Backup ile, işletim sistemi veya veri disklerinin Azure disk şifrelemesi kullanılarak şifrelendiği Azure sanal makinelerinizi da yedekleyebilirsiniz. ADE, Konuk içi şifrelemeyi gerçekleştirmek için Windows VM 'Leri için BitLocker ve Linux sanal makineleri için DM-Crypt kullanır. Ayrıntılar için bkz. [Azure Backup ile şifrelenmiş sanal makineleri yedekleme ve geri yükleme](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

## <a name="next-steps"></a>Sonraki adımlar

- [Şifrelenmiş bir Azure VM 'yi yedekleme ve geri yükleme](backup-azure-vms-encryption.md)
