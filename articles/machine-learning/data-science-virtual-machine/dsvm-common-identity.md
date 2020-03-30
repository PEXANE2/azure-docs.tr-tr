---
title: Ortak bir kimlik ayarlama
titleSuffix: Azure Data Science Virtual Machine
description: Birden çok Veri Bilimi Sanal Makinesinde kullanılabilecek ortak kullanıcı hesaplarının nasıl oluşturulabileceğini öğrenin. Kullanıcıların Veri Bilimi Sanal Makinesi'ndeki kimlik doğrulamasını doğrulamak için Azure Active Directory'yi veya şirket içi Active Directory'yi kullanabilirsiniz.
keywords: derin öğrenme, Yapay Bilgi, veri bilimi araçları, veri bilimi sanal makine, jeouzamsal analitik, ekip veri bilimi süreci
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 05/08/2018
ms.openlocfilehash: 44f1f7ae3b290e1dbf01877f3881e1d95a238446
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70208150"
---
# <a name="set-up-a-common-identity-on-a-data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi'nde ortak bir kimlik ayarlama

Veri Bilimi Sanal Makinesi (DSVM) dahil olmak üzere bir Microsoft Azure sanal makinesinde (VM), VM'yi sağlarken yerel kullanıcı hesapları oluşturursunuz. Kullanıcılar daha sonra bu kimlik bilgilerini kullanarak VM'ye kimlik doğrulaması verir. Kullanıcılarınızın erişmeniz gereken birden çok VM'iniz varsa, kimlik bilgilerini yönetmek çok hantal olabilir. Mükemmel bir çözüm, standart tabanlı bir kimlik sağlayıcısı aracılığıyla ortak kullanıcı hesapları ve yönetimi dağıtmaktır. Bu yaklaşım sayesinde, birden çok DSVM dahil olmak üzere Azure'da birden çok kaynağa erişmek için tek bir kimlik bilgileri kümesi kullanabilirsiniz.

Active Directory popüler bir kimlik sağlayıcısıdır ve Azure'da hem bulut hizmeti hem de şirket içi dizini olarak desteklenir. Azure Etkin Dizin (Azure AD) veya şirket içi Active Directory'yi, kullanıcıların tek başına bir DSVM'de veya bir grup DSVM'de azure sanal makine ölçeği kümesinde kimliğini doğrulamak için kullanabilirsiniz. Bunu, DSVM örneklerini Etkin Dizin etki alanına katılarak yaparsınız.

Active Directory zaten varsa, ortak kimlik sağlayıcınız olarak kullanabilirsiniz. Etkin Dizin'iniz yoksa, Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) aracılığıyla Azure'da yönetilen bir Active [Directory örneğini](https://docs.microsoft.com/azure/active-directory-domain-services/) çalıştırabilirsiniz.

[Azure AD](https://docs.microsoft.com/azure/active-directory/) için belgeler, varsa Azure AD'yi şirket dizininize bağlama yla ilgili kılavuzlar da dahil olmak üzere ayrıntılı [yönetim yönergeleri](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution)sağlar.

Bu makalede, Azure AD DS kullanarak Azure'da tam olarak yönetilen bir Active Directory etki alanı hizmetinin nasıl ayarlanır. Daha sonra DSVM'lerinizi yönetilen Active Directory etki alanına katılabilirsiniz. Bu yaklaşım, kullanıcıların ortak bir kullanıcı hesabı ve kimlik bilgileri aracılığıyla Bir DSVM havuzuna (ve diğer Azure kaynaklarına) erişmesini sağlar.

## <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Azure'da tam olarak yönetilen bir Active Directory etki alanı ayarlama

Azure AD DS, Azure'da tam olarak yönetilen bir hizmet sunarak kimliklerinizi yönetmenikolaylaştırır. Bu Etkin Dizin etki alanında, kullanıcıları ve grupları yönetirsiniz. Dizininizde Azure tarafından barındırılan bir Etkin Dizin etki alanı ve kullanıcı hesapları ayarlamak için aşağıdaki adımları izleyin:

1. Azure portalında, kullanıcıyı Active Directory'ye ekleyin: 

   1. Dizin için genel bir yönetici olan bir hesap kullanarak [Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
    
   1. **Azure Active Directory**'yi ve ardından **Kullanıcılar ve gruplar**'ı seçin.
    
   1. **Kullanıcılar ve gruplarda,** **Tüm kullanıcıları**seçin ve ardından **Yeni kullanıcıyı**seçin.
   
           The **User** pane opens:
      
      !["Kullanıcı" bölmesi](./media/add-user.png)
    
   1. **Ad** ve **Kullanıcı adı** gibi kullanıcı ayrıntılarını girin. Kullanıcı adının alan adı bölümü ilk varsayılan alan adı "[etki alanı adı].onmicrosoft.com" veya "contoso.com" gibi doğrulanmış, federe olmayan [özel alan adı](../../active-directory/add-custom-domain.md) olmalıdır.
    
   1. Bu işlem tamamlandıktan sonra kullanıcıyla paylaşabilmek için oluşturulan kullanıcı parolasını kopyalayın veya bir yere not edin.
    
   1. İsteğe bağlı olarak kullanıcının **Profil**, **Gruplar** veya **Dizin rolü** bilgilerini doldurun. 
    
   1. **Kullanıcı**altında, **Oluştur'u**seçin.
    
   1. Oluşturulan parolayı yeni kullanıcıya güvenli bir şekilde dağıtın, böylece oturum açabilsinler.

1. Azure AD DS örneği oluşturun. [Azure portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini Etkinleştir'deki](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) yönergeleri izleyin ("Örnek oluşturma ve temel ayarları yapılandırma" bölümü). Azure AD DS'deki parolanın eşitlenolması için Active Directory'deki varolan kullanıcı parolalarını güncelleştirmek önemlidir. Bu bölümde "Azure REKLAM DS örneği oluşturmak için Azure portalının Temeller penceresindeki alanları tamamlayın" altında açıklandığı gibi, Azure AD DS'ye DNS eklemek de önemlidir.

1. Önceki adımın "Sanal ağ oluşturma ve yapılandırma" bölümünde oluşturulan sanal ağda ayrı bir DSVM alt ağı oluşturun.
1. DSVM alt netinde bir veya daha fazla DSVM örneği oluşturun.
1. DSVM'yi Active Directory'ye eklemek için [yönergeleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) izleyin. 
1. Çalışma alanınızın herhangi bir makineye monte edilebilmeleri için ev veya not defteri dizininizi barındırmak için Azure Dosyaları paylaşımını monte edin. (Sıkı dosya düzeyinde izinlere ihtiyacınız varsa, bir veya daha fazla VM'de çalışan Ağ Dosya Sistemi [NFS] gerekir.)

   1. [Azure Dosyaları paylaşımı oluşturun.](../../storage/files/storage-how-to-create-file-share.md)
    
   2.  Bu paylaşımı Linux DSVM'ye monte edin. Azure portalındaki depolama hesabınızda Azure Dosyaları paylaşımı için **Bağlan'ı** seçtiğinizde, Linux DSVM'de bash kabuğunda çalışma komutu görüntülenir. Komut şuna benzer:
   
   ```
   sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
   ```
1. Örneğin, Azure Dosyalarınızı /data/çalışma alanına monte ettiğinizi varsayalım. Şimdi, paylaşımdaki kullanıcılarınızın her biri için dizinler oluşturun: /data/workspace/user1, /data/workspace/user2, vesaire. Her `notebooks` kullanıcının çalışma alanında bir dizin oluşturun. 
1. için `notebooks` `$HOME/userx/notebooks/remote`sembolik bağlantılar oluşturun.   

Artık Azure'da barındırılan Active Directory örneğindeki kullanıcılara sahipsiniz. Etkin Dizin kimlik bilgilerini kullanarak, kullanıcılar Azure AD DS'ye katılan herhangi bir DSVM'de (SSH veya JupyterHub) oturum açabilir. Kullanıcı çalışma alanı Bir Azure Files paylaşımında olduğundan, kullanıcılar JupyterHub kullanırken not defterlerine ve diğer işlerinden herhangi bir DSVM'den erişebilirler.

Otomatik ölçeklendirme için, tüm bu şekilde etki alanına ve paylaşılan disk takılı vm'lerden oluşan bir havuz oluşturmak için sanal makine ölçeği ayarını kullanabilirsiniz. Kullanıcılar sanal makine ölçeği kümesinde mevcut herhangi bir makinede oturum açabilir ve not defterlerinin kaydedildiği paylaşılan diske erişebilir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde depolama](dsvm-secure-access-keys.md)



