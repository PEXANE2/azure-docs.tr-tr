---
title: Kullanıcının dosya düzeyinde neler yapabileceğini denetleme-Azure dosya paylaşımları
description: Azure dosya paylaşımları için şirket içi AD DS kimlik doğrulaması için Windows ACL 'Leri izinlerini yapılandırmayı öğrenin. Ayrıntılı erişim denetiminden yararlanmanıza olanak tanır.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 05/29/2020
ms.author: rogarana
ms.openlocfilehash: 6e49201b0574e0a1235cc9e2cb313b40b0563f93
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84268495"
---
# <a name="part-three-configure-directory-and-file-level-permissions-over-smb"></a>Üçüncü kısım: SMB üzerinden dizin ve dosya düzeyi izinleri yapılandırma 

Bu makaleye başlamadan önce önceki makaleyi tamamladığınızdan, [bir kimliğe Share-Level Izinleri atadığınızdan](storage-files-identity-ad-ds-assign-permissions.md)emin olun. Paylaşma düzeyi izinlerinizin yapıldığından emin olmak için.

RBAC ile paylaşma düzeyi izinleri atadıktan sonra, ayrıntılı erişim denetiminden yararlanmak için kök, dizin veya dosya düzeyinde uygun Windows ACL 'Leri (NTFS izinleri olarak da bilinir) atamanız gerekir. Bir kullanıcının paylaşıma erişip erişemeyeceğini belirleyen üst düzey ağ geçidi olarak RBAC paylaşma düzeyi izinleri düşünün. Windows ACL 'Leri, kullanıcının dizin veya dosya düzeyinde hangi işlemleri yapabileceğini belirlemek için daha ayrıntılı bir düzeyde çalışır.

RBAC ile paylaşma düzeyi izinleri atadıktan sonra, ayrıntılı erişim denetiminden yararlanmak için kök, dizin veya dosya düzeyinde uygun Windows ACL 'Leri yapılandırmanız gerekir. Bir kullanıcının paylaşıma erişip erişemeyeceğini belirleyen üst düzey ağ geçidi olarak RBAC paylaşma düzeyi izinleri düşünün. Windows ACL 'Leri, kullanıcının dizin veya dosya düzeyinde hangi işlemleri yapabileceğini belirlemek için daha ayrıntılı bir düzeyde çalışır. Bir Kullanıcı bir dosya/dizine erişmeyi denediğinde hem paylaşma düzeyi hem de dosya/dizin düzeyi izinleri zorlanır. bu nedenle, aralarında fark varsa yalnızca en kısıtlayıcı bir değer uygulanır. Örneğin, bir kullanıcının dosya düzeyinde okuma/yazma erişimi varsa ancak yalnızca bir paylaşma düzeyinde salt okunurdur, bu dosyayı yalnızca okuyabilir. Aynı değer, geri çevrilirse ve bir kullanıcının Share-Level ' a okuma/yazma erişimi varsa ancak yalnızca dosya düzeyinde salt okuma yaptığı halde yalnızca dosyayı okuyabilecekleri şekilde true olur.

## <a name="supported-permissions"></a>Desteklenen izinler

Azure dosyaları, temel ve gelişmiş Windows ACL 'lerinin tam kümesini destekler. Windows ACL 'lerini, bir Azure dosya paylaşımındaki dizinler ve dosyalar üzerinde, Windows Dosya Gezgini 'ni kullanarak, Windows [ıacl](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) komutunu veya [set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-acl) komutunu çalıştırarak bir Azure dosya paylaşımında görüntüleyebilir ve yapılandırabilirsiniz. 

ACL 'Leri üst Kullanıcı izinleriyle yapılandırmak için, etki alanına katılmış VM 'nizden depolama hesabı anahtarınızı kullanarak paylaşıma bağlamanız gerekir. Bir sonraki bölümdeki yönergeleri izleyerek bir Azure dosya paylaşımından komut isteminden bağlama yapın ve Windows ACL 'Lerini yapılandırın.

Aşağıdaki izinler bir dosya paylaşımının kök dizinine dahildir:

- BUILTIN\Administrators: (OI) (CI) (F)
- NT AUTHORıTY\SYSTEM: (OI) (Cı) (F)
- BUILTIN\Users: (RX)
- BUILTIN\Users: (OI) (CI) (GÇ) (GR, GE)
- NT Authorıty\authenticated users: (OI) (CI) (ı)
- NT AUTHORıTY\SYSTEM: (F)
- OLUŞTURAN SAHIBI: (OI) (Cı) (GÇ) (F)

## <a name="mount-a-file-share-from-the-command-prompt"></a>Komut isteminden bir dosya paylaşma bağlama

`net use`Azure dosya paylaşımından bağlamak Için Windows komutunu kullanın. Aşağıdaki örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Dosya paylaşımları bağlama hakkında daha fazla bilgi için bkz. [Windows Ile Azure dosya paylaşımı kullanma](storage-how-to-use-files-windows.md). 

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> /user:Azure\<storage-account-name> <storage-account-key>
```

Azure dosyalarına bağlanırken sorunlarla karşılaşırsanız, [Windows 'Da Azure dosyaları bağlama hataları için yayımladığımız sorun giderme aracına](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5)bakın. Ayrıca, 445 numaralı bağlantı noktası engellendiğinde senaryolar etrafında çalışmak için [rehberlik](https://docs.microsoft.com/azure/storage/files/storage-files-faq#on-premises-access) sunuyoruz. 

## <a name="configure-windows-acls"></a>Windows ACL 'Lerini yapılandırma

Dosya paylaşımınız depolama hesabı anahtarıyla bağlandıktan sonra, Windows ACL 'Lerini (NTFS izinleri olarak da bilinir) yapılandırmanız gerekir. Windows ACL 'Lerini Windows Dosya Gezgini veya ıcacl 'leri kullanarak yapılandırabilirsiniz.

AD DS kimliklerine göre yapılandırılmış Windows DACL 'Leri olan şirket içi dosya sunucularında dizinleriniz veya dosyalarınız varsa, ACL 'Leri Robocopy veya [Azure AzCopy v 10.4 +](https://github.com/Azure/azure-storage-azcopy/releases)gibi geleneksel dosya kopyalama araçları ile kalıcı olarak Azure dosyalarına kopyalayabilirsiniz. Dizinleriniz ve dosyalarınız Azure Dosya Eşitleme aracılığıyla Azure dosyaları ile katmanlandıysanız, ACL 'leriniz yerel biçimlerinde taşınır ve kalıcı hale getirilir.

### <a name="configure-windows-acls-with-windows-file-explorer"></a>Windows Dosya Gezgini ile Windows ACL 'Lerini yapılandırma

Kök dizin dahil olmak üzere dosya paylaşımındaki tüm dizinlere ve dosyalara tam izin vermek için Windows Dosya Gezgini 'ni kullanın.

1. Windows Dosya Gezgini 'ni açın ve dosya/dizine sağ tıklayıp **Özellikler**' i seçin.
1. **Güvenlik** sekmesini seçin.
1. Düzenle 'yi seçin **.** izinleri değiştirmek için.
1. Mevcut kullanıcıların izinlerini değiştirebilir veya yeni kullanıcılara izin vermek için **Ekle..** . seçeneğini belirleyebilirsiniz.
1. Yeni Kullanıcı eklemek için istem penceresinde, **Seçilecek nesne adlarını girin** kutusuna izin vermek istediğiniz hedef Kullanıcı adını girin ve hedef kullanıcının tam UPN adını bulmak Için **adları denetle** ' yi seçin.
1.    **Tamam**’ı seçin.
1.    **Güvenlik** sekmesinde, yeni kullanıcıya vermek istediğiniz tüm izinleri seçin.
1.    **Uygula**’yı seçin.

### <a name="configure-windows-acls-with-icacls"></a>Windows ACL 'lerini Icacls ile yapılandırma

Kök dizin dahil olmak üzere, dosya paylaşımındaki tüm dizin ve dosyalara tam izinler vermek için aşağıdaki Windows komutunu kullanın. Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

Windows ACL 'lerini ve farklı türlerdeki desteklenen izinleri ayarlamak için ıacl 'leri kullanma hakkında daha fazla bilgi için bkz. [ıacl 'ler için komut satırı başvurusu](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="next-steps"></a>Sonraki adımlar

Artık özellik etkin ve yapılandırılmış olduğuna göre, Azure dosya paylaşımınızı etki alanına katılmış bir VM 'den bağladığınızda sonraki makaleye devam edin.

[Dördüncü kısım: etki alanına katılmış bir VM 'den bir dosya paylaşma bağlama](storage-files-identity-ad-ds-mount-file-share.md)