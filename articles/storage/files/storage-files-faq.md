---
title: Azure Dosyaları için sık sorulan sorular (SSS) | Microsoft Dokümanlar
description: Azure Dosyaları hakkında sık sorulan soruların yanıtlarını bulun.
author: roygara
ms.service: storage
ms.date: 02/23/2020
ms.author: rogarana
ms.subservice: files
ms.topic: conceptual
ms.openlocfilehash: 82c516eeac6d3e88ca7b6ac1c97ebb638ba27979
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383924"
---
# <a name="frequently-asked-questions-faq-about-azure-files"></a>Azure Dosyalar hakkında sık sorulan sorular (SSS)
[Azure Files,](storage-files-introduction.md) bulutta endüstri standardı Sunucu İleti Bloğu [(SMB) protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)aracılığıyla erişilebilen tam yönetilen dosya paylaşımları sunar. Azure dosya paylaşımlarını bulutveya Windows, Linux ve macOS'un şirket içi dağıtımlarına eş zamanlı olarak monte edebilirsiniz. Ayrıca, verilerin kullanıldığı yere yakın hızlı erişim için Azure Dosya Eşitlemeyi'ni kullanarak Windows Server makinelerinde Azure dosya paylaşımlarını önbelleğe alabilirsiniz.

Bu makalede, Azure Dosyaları özellikleri ve işlevleri yle ilgili sık sorulan soruları yanıtlar; Sorunuzun cevabını göremiyorsanız, aşağıdaki kanallardan (artan sırada) bizimle iletişime geçebilirsiniz:

1. Bu makalenin yorum bölümü.
2. [Azure Depolama Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [Azure Dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Microsoft Desteği. **Azure portalında, Yardım** sekmesinde yeni bir destek isteği oluşturmak için Yardım + **destek** düğmesini seçin ve ardından Yeni **destek isteğini**seçin.

## <a name="general"></a>Genel
* <a id="why-files-useful"></a>
  **Azure Dosyaları nasıl kullanışlıdır?**  
   Azure Dosyalarını, fiziksel bir sunucunun, aygıtın veya cihazın genel yükünü yönetmekten sorumlu olmadan bulutta dosya paylaşımları oluşturmak için kullanabilirsiniz. İşletim sistemi güncelleştirmelerini uygulamak ve kötü diskleri değiştirmek de dahil olmak üzere sizin için monoton işleri yapıyoruz. Azure Dosyaları'nın size yardımcı olabileceği senaryolar hakkında daha fazla bilgi edinmek için [Azure Dosyaları'nın neden yararlı olduğunu](storage-files-introduction.md#why-azure-files-is-useful)öğrenin.

* <a id="file-access-options"></a>
  **Azure Dosyalarındaki dosyalara erişmenin farklı yolları nelerdir?**  
    SMB 3.0 protokolünü kullanarak dosya paylaşımını yerel makinenize monte edebilir veya dosya paylaşımınızdaki dosyalara erişmek için [Depolama Gezgini](https://storageexplorer.com/) gibi araçları kullanabilirsiniz. Uygulamanızdan, Azure dosya paylaşımındaki dosyalarınıza erişmek için depolama istemcikitaplarını, REST API'lerini, PowerShell'i veya Azure CLI'yi kullanabilirsiniz.

* <a id="what-is-afs"></a>
  **Azure Dosya Eşitlemi nedir?**  
    Kurum içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu koruyarak kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda merkezileştirmek için Azure Dosya Eşitlemeyi'ni kullanabilirsiniz. Azure Dosya Eşitlemi, Windows Server makinelerinizi Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, Ağ Dosya Sistemi (NFS) ve Dosya Aktarım Protokolü Hizmeti (FTPS) dahil olmak üzere verilerinize yerel olarak erişmek için Windows Server'da kullanılabilen tüm protokolleri kullanabilirsiniz. Dünya çapında ihtiyacınız olduğu kadar önbelleke sahip olabilirsiniz.

* <a id="files-versus-blobs"></a>
  **Verilerim için azure dosya paylaşımı ile Azure Blob depolama alanını neden kullanayım?**  
    Azure Dosyaları ve Azure Blob depolama alanı, bulutta büyük miktarda veri depolamanın yollarını sunar, ancak bunlar biraz farklı amaçlar için yararlıdır. 
    
    Azure Blob depolama, yapılandırılmamış verileri depolaması gereken büyük ölçekli, bulut tabanlı uygulamalar için yararlıdır. Azure Blob depolama, performansı ve ölçeği en üst düzeye çıkarmak için gerçek bir dosya sisteminden daha basit bir depolama soyutlamadır. Azure Blob depolama alanına yalnızca REST tabanlı istemci kitaplıkları aracılığıyla (veya doğrudan REST tabanlı protokol aracılığıyla) erişebilirsiniz.

    Azure Files özellikle bir dosya sistemidir. Azure Files, şirket içi işletim sistemleriyle yıllarca çalıştığınız ve sevdiğiniz tüm dosya özetlerine sahiptir. Azure Blob depolama gibi Azure Files da REST arabirimi ve REST tabanlı istemci kitaplıkları sunar. Azure Blob depolama alanının aksine, Azure Dosyaları SMB'nin Azure dosya paylaşımlarına erişimini sağlar. SMB'yi kullanarak, herhangi bir kod yazmadan veya dosya sistemine özel sürücüler eklemeden doğrudan Windows, Linux veya macOS'a, şirket içinde veya bulut VM'lere bir Azure dosya paylaşımı monte edebilirsiniz. Ayrıca, verilerin kullanıldığı yere yakın, hızlı erişim için Azure Dosya Eşitlemeyi'ni kullanarak şirket içi dosya sunucularında Azure dosya paylaşımlarını önbelleğe alabilirsiniz. 
   
    Azure Dosyaları ve Azure Blob depolama alanı arasındaki farklar hakkında daha ayrıntılı bir açıklama için [temel Azure Depolama hizmetlerine Giriş'e](../common/storage-introduction.md)bakın. Azure Blob depolama alanı hakkında daha fazla bilgi edinmek [için Blob depolamaya Giriş 'e](../blobs/storage-blobs-introduction.md)bakın.

* <a id="files-versus-disks"></a>**Azure Diskleri yerine neden Azure dosya paylaşımı kullansam?**  
    Azure Diskler'deki bir disk yalnızca bir disktir. Azure Disklerinden değer almak için, Azure'da çalışan sanal bir makineye bir disk eklemeniz gerekir. Azure Diskler, şirket içi sunucuda disk kullanacağınız her şey için kullanılabilir. Bunu işletim sistemi sistem diski olarak, işletim sistemi için takas alanı olarak veya bir uygulama için ayrılmış depolama alanı olarak kullanabilirsiniz. Azure Diskleri için ilginç bir kullanım, bulutta azure dosya paylaşımı kullanabileceğiniz aynı yerlerde kullanmak üzere bir dosya sunucusu oluşturmaktır. Azure Sanal Makineler'de bir dosya sunucusu dağıtmak, şu anda Azure Dosyaları tarafından desteklenmeyen dağıtım seçeneklerine (NFS protokol desteği veya premium depolama gibi) ihtiyaç duyduğunuzda Azure'da dosya depolamasını elde etmenin yüksek performanslı bir yoludur. 

    Ancak, arka uç depolama olarak Azure Diskleri olan bir dosya sunucusu çalıştırmak genellikle birkaç nedenden dolayı Azure dosya paylaşımı nı kullanmaktan çok daha pahalıdır. İlk olarak, disk depolama için ödeme nin yanı sıra, bir veya daha fazla Azure VM'si çalıştırma giderini de ödemeniz gerekir. İkinci olarak, dosya sunucusuçalıştırmak için kullanılan VM'leri de yönetmeniz gerekir. Örneğin, işletim sistemi yükseltmelerinden siz sorumlusunuz. Son olarak, sonuçta verilerin şirket içinde önbelleğe alınmasını istiyorsanız, bunun gerçekleşmesi için Dağıtılmış Dosya Sistemi Çoğaltma (DFSR) gibi çoğaltma teknolojilerini kurmak ve yönetmek size bağlıdır.

    Hem Azure Dosyaları'ndan hem de Azure Sanal Makinelerde barındırılan bir dosya sunucusundan en iyi şekilde elde etmek için bir yaklaşım (Azure Disklerini arka uç depolama alanı olarak kullanmanın yanı sıra) bulut VM'de barındırılan bir dosya sunucusuna Azure Dosya Eşitlemi yüklemektir. Azure dosya paylaşımı dosya sunucunuzla aynı bölgedeyse, bulut katmanlamayı etkinleştirebilir ve boş alan yüzdesini maksimuma (%99) ayarlayabilirsiniz. Bu, verilerin en az çoğaltılmasını sağlar. Ayrıca, NFS protokol desteği gerektiren uygulamalar gibi dosya sunucularınızla istediğiniz uygulamaları da kullanabilirsiniz.

    Azure'da yüksek performanslı ve yüksek oranda kullanılabilir bir dosya sunucusu ayarlama seçeneği hakkında bilgi [için](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/)bkz. Azure Dosyaları ve Azure Diskleri arasındaki farkların daha ayrıntılı bir açıklaması için [temel Azure Depolama hizmetlerine Giriş bölümüne](../common/storage-introduction.md)bakın. Azure Diskleri hakkında daha fazla bilgi edinmek için [Azure Yönetilen Diskler'e genel bakış](../../virtual-machines/windows/managed-disks-overview.md)'a bakın.

* <a id="get-started"></a>
  **Azure Dosyalarını kullanmaya nasıl başlarım?**  
   Azure Dosyaları ile başlamak kolaydır. İlk olarak, [bir dosya paylaşımı oluşturun](storage-how-to-create-file-share.md)ve ardından tercih ettiğiniz işletim sistemine monte edin: 

  * [Windows'da Montaj](storage-how-to-use-files-windows.md)
  * [Linux'ta Montaj](storage-how-to-use-files-linux.md)
  * [macOS'ta montaj](storage-how-to-use-files-mac.md)

    Kuruluşunuzdaki üretim dosyası paylaşımlarını değiştirmek için Azure dosya paylaşımını dağıtma hakkında daha ayrıntılı [bir](storage-files-planning.md)kılavuz için bkz.

* <a id="redundancy-options"></a>
  **Hangi depolama artıklığı seçenekleri Azure Dosyaları tarafından desteklenir?**  
    Şu anda Azure Dosyaları yerel olarak yedekli depolamayı (LRS), bölge yedekli depolama (ZRS), coğrafi yedekli depolamayı (GRS) ve coğrafi bölge yedekli depolamayı (GZRS) (önizleme) destekler. Gelecekte okuma erişimi coğrafi yedekli (RA-GRS) depolamayı desteklemeyi planlıyoruz, ancak şu anda paylaşacak zaman çizelgemiz yok.

* <a id="tier-options"></a>
  **Azure Dosyalarında hangi depolama katmanları desteklenir?**  
    Azure Dosyaları iki depolama katmanını destekler: premium ve standart. Standart dosya paylaşımları genel amaçlı (GPv1 veya GPv2) depolama hesapları oluşturulur ve premium dosya paylaşımları FileStorage depolama hesaplarında oluşturulur. [Standart dosya paylaşımları](storage-how-to-create-file-share.md) ve [premium dosya paylaşımları](storage-how-to-create-premium-fileshare.md)oluşturma hakkında daha fazla bilgi edinin. 
    
    > [!NOTE]
    > Blob depolama hesaplarından veya *premium* genel amaçlı (GPv1 veya GPv2) depolama hesaplarından Azure dosya paylaşımları oluşturamazsınız. Standart Azure dosya paylaşımları yalnızca *standart* genel amaçlı hesaplarda oluşturulmalı ve premium Azure dosya paylaşımları yalnızca FileStorage depolama hesaplarında oluşturulmalıdır. *Premium* genel amaçlı (GPv1 ve GPv2) depolama hesapları yalnızca premium sayfa lekeleri içindir. 

* <a id="give-us-feedback"></a>
  **Azure Dosyaları'na eklenen belirli bir özelliği gerçekten görmek istiyorum. Ekleyebilir misin?**  
    Azure Files ekibi, hizmetimiz hakkında sahip olduğunuz tüm geri bildirimleri duymak ister. Lütfen Azure Files [UserVoice'ta](https://feedback.azure.com/forums/217298-storage/category/180670-files)özellik isteklerini oylayın! Birçok yeni özellik ile sizi memnun etmek için sabırsızlanıyoruz.

  **Azure Files dosya kilitlemeyi destekliyor mu?**  
    Evet, Azure Dosyaları SMB/Windows tarzı dosya kilitlemeyi tam olarak destekler, [ayrıntılara bakın.](https://docs.microsoft.com/rest/api/storageservices/managing-file-locks) 
    
## <a name="azure-file-sync"></a>Azure Dosya Eşitleme

* <a id="afs-region-availability"></a>
  **Azure Dosya Eşitlemi için hangi bölgeler desteklenir?**  
    Kullanılabilir bölgelerin listesi, Azure Dosya Eşitleme planlama kılavuzunun [Bölge kullanılabilirliği](storage-sync-files-planning.md#azure-file-sync-region-availability) bölümünde bulunabilir. Genel olmayan bölgeler de dahil olmak üzere ek bölgeler için sürekli destek ekleyeceğiz.

* <a id="cross-domain-sync"></a>
  **Aynı eşitleme grubunda etki alanına katılan ve etki alanına katılmayan sunucular olabilir miyim?**  
    Evet. Eşitleme grubu, etki alanına katılmamış olsalar bile, farklı Active Directory üyeliklerine sahip sunucu uç noktaları içerebilir. Bu yapılandırma teknik olarak çalışsa da, bir sunucudaki dosya ve klasörler için tanımlanan erişim denetim listeleri (ALA)'lar eşitleme grubundaki diğer sunucular tarafından zorlanamayabileceğinden, bunu tipik bir yapılandırma olarak önermiyoruz. En iyi sonuçlar için, aynı Active Directory ormanında bulunan sunucular arasında, farklı Active Directory ormanlarında bulunan ancak güven ilişkileri kuran sunucular veya etki alanında olmayan sunucular arasında eşitleme öneririz. Bu yapılandırmaların bir karışımını kullanmaktan kaçınmanızı öneririz.

* <a id="afs-change-detection"></a>
  **SMB kullanarak veya portalda doğrudan Azure dosya paylaşımımda bir dosya oluşturdum. Dosyanın eşitleme grubundaki sunuculara eşitlemesi ne kadar sürer?**  
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Aynı dosya yaklaşık olarak aynı anda iki sunucuda değiştirilirse, ne olur?**  
    Azure Dosya Eşitleme basit bir çakışma çözümleme stratejisi kullanır: her iki hatayı da aynı anda iki sunucuda değiştirilen dosyalarda saklarız. En son yazılan değişiklik özgün dosya adını tutar. Eski dosyada "kaynak" makinesi ve ada eklenen çakışma numarası vardır. Bu taksonomi izler: 
   
    \<FilenameWithoutExtension\>-\<\>\[-#\]MachineName . \<ext\>  

    Örneğin, CentralServer eski yazma oluştu nerede ise CompanyReport.docx ilk çakışma CompanyReport-CentralServer.docx olur. İkinci çakışma CompanyReport-CentralServer-1.docx olarak adlandırılır. Azure Dosya Eşitlemi, dosya başına 100 çakışma dosyalarını destekler. Çakışma dosyalarının en fazla sayısına ulaşıldıktan sonra, çakışma dosyası sayısı 100'den az olana kadar dosya eşitleme yapmaz.

* <a id="afs-storage-redundancy"></a>
  **Azure Dosya Eşitlemi için coğrafi yedekli depolama desteklenir mi?**  
    Evet, Azure Dosyaları hem yerel olarak yedekli depolamayı (LRS) hem de coğrafi yedekli depolamayı (GRS) destekler. GRS için yapılandırılan bir hesaptan eşleştirilmiş bölgeler arasında bir depolama hesabı başarısızlığı başlatırsanız, Microsoft yeni bölgeyi yalnızca veri yedeklemesi olarak ele almanı önerir. Azure Dosya Eşitleme, yeni birincil bölgeyle otomatik olarak eşitlemeye başlamaz. 

* <a id="sizeondisk-versus-size"></a>
  **Bir dosyanın *disk özelliğindeki Boyut,* Azure Dosya Eşitlemesi'ni kullandıktan sonra neden *Boyut* özelliğiyle eşleşmiyor?**  
  Bkz. [Bulut Katmanlama'yı Anlama.](storage-sync-cloud-tiering.md#sizeondisk-versus-size)

* <a id="is-my-file-tiered"></a>
  **Bir dosyanın katmanlı olup olmadığını nasıl anlayabilirim?**  
  Bkz. [Bulut Katmanlama'yı Anlama.](storage-sync-cloud-tiering.md#is-my-file-tiered)

* <a id="afs-recall-file"></a>**Kullanmak istediğim bir dosya katmanlı hale alındı. Yerel olarak kullanmak için dosyayı diske nasıl geri çağırabilirim?**  
  Bkz. [Bulut Katmanlama'yı Anlama.](storage-sync-cloud-tiering.md#afs-recall-file)

* <a id="afs-force-tiering"></a>
  **Bir dosyayı veya dizini katmanlı olmaya nasıl zorlayabilirim?**  
  Bkz. [Bulut Katmanlama'yı Anlama.](storage-sync-cloud-tiering.md#afs-force-tiering)

* <a id="afs-effective-vfs"></a>
  **Bir *birimde* birden çok sunucu uç noktam olduğunda birim boş alan nasıl yorumlanır?**  
  Bkz. [Bulut Katmanlama'yı Anlama.](storage-sync-cloud-tiering.md#afs-effective-vfs)

* <a id="afs-files-excluded"></a>
  **Azure Dosya Eşitlemi tarafından hangi dosya veya klasörler otomatik olarak dışlanır?**  
  Bkz. [Atlanan Dosyalar.](storage-sync-files-planning.md#files-skipped)

* <a id="afs-os-support"></a>
  **Azure Dosya Eşitlemeyi'ni Windows Server 2008 R2, Linux veya ağa bağlı depolama aygıtımla (NAS) kullanabilir miyim?**  
    Şu anda Azure Dosya Eşitlemi yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2'yi destekler. Şu anda paylaşabileceğimiz başka bir planımız yok, ancak müşteri talebine dayalı ek platformları desteklemeye açığız. Azure Files [UserVoice'ta](https://feedback.azure.com/forums/217298-storage/category/180670-files) hangi platformları desteklememizi istediğinizi bize bildirin.

* <a id="afs-tiered-files-out-of-endpoint"></a>
  **Katmanlı dosyalar neden sunucu bitiş noktası ad alanının dışında var?**  
    Azure Dosya Eşitleme aracısı sürüm 3'ten önce, Azure Dosya Eşitlemesi katmanlı dosyaların sunucu bitiş noktası dışında ancak sunucu bitiş noktasıyla aynı birimde taşınmasını engelledi. Kopyalama işlemleri, katmanlı olmayan dosyaların hareketleri ve katmanlı diğer birimlere yapılan hareketler etkilenmedi. Bu davranışın nedeni, Dosya Gezgini ve diğer Windows API'lerinin aynı birimdeki taşıma işlemlerine (neredeyse) anlık yeniden adlandırma işlemleri olduğu varsayımıydı. Bu, Azure Dosya Eşitlemi verileri buluttan çağırırken, aktarımların Dosya Gezgini'ni veya diğer taşıma yöntemlerini (komut satırı veya PowerShell gibi) yanıt vermiyor gibi yapacağı anlamına gelir. Azure [Dosya Eşitleme aracısı sürümü 3.0.12.0](storage-files-release-notes.md#supported-versions)ile başlayarak, Azure Dosya Eşitlemesi katmanlı bir dosyayı sunucu bitiş noktasının dışına taşımanızı sağlar. Katmanlı dosyanın sunucu bitiş noktası dışında katmanlı bir dosya olarak var olmasına izin vererek ve ardından arka planda dosyayı geri çağırarak daha önce bahsedilen olumsuz etkileri önleriz. Bu, aynı birimdeki hareketlerin anlık olduğu anlamına gelir ve taşıma tamamlandıktan sonra dosyayı diske geri çağırmak için tüm çalışmaları yaparız. 

* <a id="afs-do-not-delete-server-endpoint"></a>
  **Sunucumda Azure Dosya Eşitlemi ile ilgili bir sorun yaşıyorum (eşitleme, bulut katmanlama, vb.). Sunucu bitiş noktamı kaldırmalı ve yeniden oluşturmalı mıyım?**  
    [!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]
    
* <a id="afs-resource-move"></a>
  **Depolama eşitleme hizmetini ve/veya depolama hesabını farklı bir kaynak grubuna veya aboneye taşıyabilir miyim?**  
   Evet, depolama eşitleme hizmeti ve/veya depolama hesabı, varolan Azure AD kiracısı içinde farklı bir kaynak grubuna veya aboneye taşınabilir. Depolama hesabı taşınırsa, Karma Dosya Eşitleme Hizmeti'ne depolama hesabına erişim sağlamanız gerekir (bkz. [Azure Dosya Eşitlemesi'nin depolama hesabına erişmesini sağlayın).](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)

    > [!Note]  
    > Azure Dosya Eşitlemesi, aboneliğin farklı bir Azure AD kiracısına taşınmasını desteklemez.
    
* <a id="afs-ntfs-acls"></a>
  **Azure Dosya Eşitleme, Azure Dosyalarında depolanan verilerle birlikte dizin/dosya düzeyi NTFS ARG'larını korur mu?**

    24 Şubat 2020 itibariyle, Azure dosya eşitlemi tarafından katmanlı yeni ve mevcut ACL'ler NTFS biçiminde kalıcı hale getirilir ve doğrudan Azure dosya paylaşımına yapılan ACL değişiklikleri eşitleme grubundaki tüm sunucularla eşitlenir. Azure Dosyalarında Yapılan AK'larda yapılan tüm değişiklikler Azure dosya eşitlemi yoluyla eşitlenecektir. Verileri Azure Dosyaları'na kopyalarken, paylaşıma erişmek ve ALA'larınıza erişmek için SMB'yi kullandığınızdan emin olun. AzCopy veya Storage Explorer gibi mevcut REST tabanlı araçlar ACD'leri kalıcı olarak kullanmaz.

    Dosya eşitleme yönetilen dosya paylaşımlarınızda Azure Yedekleme'yi etkinleştirdiyseniz, dosya AçP'ları yedekleme geri yükleme iş akışının bir parçası olarak geri yüklenmeye devam edebilir. Bu, tüm paylaşım veya tek tek dosyalar/dizinler için çalışır.

    Dosya eşitlemesi tarafından yönetilen dosya paylaşımları için anlık görüntüleri kendi kendini yöneten yedekleme çözümünün bir parçası olarak kullanıyorsanız, anlık görüntüler 24 Şubat 2020 tarihinden önce alınmışsa, ALA'larınız NTFS ALA'larına düzgün bir şekilde geri yüklenmeyebilir. Bu durumda Azure Destek'e başvurmayı düşünün.
    
## <a name="security-authentication-and-access-control"></a>Güvenlik, kimlik doğrulama ve erişim denetimi
* <a id="ad-support"></a>
**Kimlik tabanlı kimlik doğrulama ve erişim denetimi Azure Dosyaları tarafından desteklenir mi?**  
    
    Evet, Azure Dosyaları kimlik tabanlı kimlik doğrulamave erişim denetimini destekler. Kimlik tabanlı erişim denetimini kullanmanın iki yolu arasından birini seçebilirsiniz: Etkin Dizin (AD) (önizleme) veya Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) (GA). AD, Azure dosya paylaşımlarına Kobİ üzerinden erişmek için şirket içinde veya Azure'da birleştirilmiş AD etki alanı birleştirme makinelerini kullanarak kimlik doğrulamasını destekler. Azure Dosyaları için Kobİ üzerinden Azure AD DS kimlik doğrulaması, Azure AD DS alan adı birleştirilmiş Windows VM'lerinin Azure AD kimlik bilgilerini kullanarak paylaşımlara, dizinlere ve dosyalara erişmesini sağlar. Daha fazla ayrıntı için, [Kobİ erişimi için Azure Dosyalarına Genel Bakış kimlik tabanlı kimlik doğrulama desteğine](storage-files-active-directory-overview.md)bakın. 

    Azure Dosyaları, erişim denetimini yönetmenin iki ek yolu sunar:

    - Belirli izinlere sahip ve belirli bir zaman aralığı için geçerli olan belirteçler oluşturmak için paylaşılan erişim imzalarını (SAS) kullanabilirsiniz. Örneğin, 10 dakikalık bir son kullanma süresi olan belirli bir dosyaya salt okunur erişimi olan bir belirteç oluşturabilirsiniz. Belirteç geçerliyken belirteci sahip herkes bu 10 dakika için bu dosyaya salt okunur erişimi vardır. Paylaşılan erişim imza anahtarları yalnızca REST API üzerinden veya istemci kitaplıklarında desteklenir. Depolama hesabı anahtarlarını kullanarak Azure dosya paylaşımını Kobİ'lerin üzerine monte edmelisiniz.

    - Azure Dosya Eşitlemesi, eşitlediği tüm sunucu uç noktalarına (Active Directory tabanlı veya yerel olsun) tüm isteğe bağlı ACD'leri veya DACL'leri korur ve çoğaltır. Windows Server Zaten Active Directory ile kimlik doğrulaması yapılabildiği için, Etkin Dizin tabanlı kimlik doğrulama ve ACL desteği için tam destek gelene kadar Azure Dosya Eşitlemi etkili bir ara verme seçeneğidir.
    
    Azure Depolama hizmetlerinde desteklenen tüm protokollerin kapsamlı bir gösterimi için [Azure Depolama'ya erişim yetkisine](https://docs.microsoft.com/azure/storage/common/storage-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) başvurabilirsiniz. 

* <a id="ad-support-devices"></a>
**Azure Dosyaları Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) Kimlik Doğrulaması, Azure AD'ye katılan veya Azure AD'ye kayıtlı cihazlardan Gelen Azure AD kimlik bilgilerini kullanarak Kobİ erişimini destekler mi?**

    Hayır, bu senaryo desteklenmiyor.

* <a id="ad-support-rest-apis"></a>
**Get/Set/Copy dizinini/dosya NTFS ALIK'lerini destekleyen REST API'leri var mı?**

    Evet, [2019-07-07](https://docs.microsoft.com/rest/api/storageservices/versioning-for-the-azure-storage-services#version-2019-07-07) (veya sonraki) REST API'sını kullanırken dizinler veya dosyalar için NTFS AK'larını alan, ayarlayan veya kopyalayan REST API'lerini destekliyoruz.

* <a id="ad-vm-subscription"></a>
**Azure REKLAM kimlik bilgilerine sahip Azure Dosyalarına farklı bir abonelik altında bir VM'den erişebilir miyim?**

    Dosya paylaşımının dağıtıldığı abonelik, VM'nin etki alanına katıldığı Azure AD Etki Alanı Hizmetleri dağıtımıyla aynı Azure AD kiracısıyla ilişkilendiriliyorsa, aynı Azure AD kimlik bilgilerini kullanarak Azure Dosyalarına erişebilirsiniz. Sınırlama, aboneye değil, ilişkili Azure AD kiracıya uygulanır.
    
* <a id="ad-support-subscription"></a>
**Azure Dosyaları Azure AD DS veya REKLAM kimlik doğrulamasını, dosya paylaşımının ilişkili olduğu birincil kiracıdan farklı bir Azure AD kiracısıyla etkinleştirebilir miyim?**

    Hayır, Azure Dosyaları yalnızca dosya paylaşımıyla aynı abonelikte bulunan bir Azure AD kiracısıyla Azure AD DS veya AD tümleştirmesini destekler. Yalnızca bir abonelik Bir Azure REKLAM kiracısıyla ilişkilendirilebilir. Bu sınırlama hem Azure AD DS hem de AD kimlik doğrulama yöntemleri için geçerlidir. Kimlik doğrulaması için AD kullanırken, AD kimlik bilgisi depolama hesabının ilişkili olduğu Azure AD ile eşitlenmelidir.

* <a id="ad-linux-vms"></a>
**Azure Files Azure AD DS veya AD kimlik doğrulaması Linux VM'lerini destekliyor mu?**

    Hayır, Linux VM'lerinden kimlik doğrulaması desteklenmez.

* <a id="ad-aad-smb-afs"></a>
**Azure Dosya Eşitleme tarafından yönetilen dosya paylaşımlarında Azure Dosyaları Azure AD DS kimlik doğrulaması veya Etkin Dizin (AD) kimlik doğrulaması (önizleme) kullanabilir miyim?**

    Evet, Azure dosya eşitlemi tarafından yönetilen bir dosya paylaşımında Azure AD DS veya AD kimlik doğrulamasını etkinleştirebilirsiniz. Yerel dosya sunucularında dizin/dosya NTFS AK'larında yapılan değişiklikler Azure Dosyalarına veya tam tersi olarak katmanlanır.

* <a id="ad-aad-smb-files"></a>
**Depolama hesabımda ve AD etki alanı bilgilerimde AD kimlik doğrulamasını etkinleştirip etkinleştirdiğimi nasıl kontrol edebilirim?**

    Azure Dosyaları AD Kimlik Doğrulaması'nın depolama hesabınızda etkinleştirilip etkinleştirilen ve AD etki alanı bilgilerini almak için [burada](https://docs.microsoft.com/azure/storage/files/storage-files-identity-auth-active-directory-enable#1-enable-ad-authentication-for-your-account) verilen yönergelere başvurabilirsiniz.

* <a id="encryption-at-rest"></a>
**Azure dosya paylaşımımın istirahatte şifrelendiğinden nasıl emin olabilirim?**  

    Evet. Daha fazla bilgi için Azure [Depolama Hizmeti Şifrelemesi'ne](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)bakın.

* <a id="access-via-browser"></a>
**Bir web tarayıcısı kullanarak belirli bir dosyaya nasıl erişebilirim?**  

    Belirli izinlere sahip ve belirli bir zaman aralığı için geçerli olan belirteçler oluşturmak için paylaşılan erişim imzalarını kullanabilirsiniz. Örneğin, belirli bir süre için belirli bir dosyaya salt okunur erişim sağlayan bir belirteç oluşturabilirsiniz. URL'ye sahip olan herkes, belirteç geçerliyken dosyaya doğrudan herhangi bir web tarayıcısından erişebilir. Depolama Gezgini gibi bir kullanıcı aracından paylaşılan erişim imza anahtarını kolayca oluşturabilirsiniz.

* <a id="file-level-permissions"></a>
**Paylaşım içindeki klasörlerde salt okunur veya yalnızca yazma izinleri belirtebilmek mümkün mü?**  

    Dosya paylaşımını SMB kullanarak bağlarsanız, izinler üzerinde klasör düzeyinde denetiminiz yoktur. Ancak, REST API veya istemci kitaplıklarını kullanarak paylaşılan bir erişim imzası oluşturursanız, paylaşım içindeki klasörlerde salt okunur veya yalnızca yazma izinleri belirtebilirsiniz.

* <a id="ip-restrictions"></a>
**Azure dosya paylaşımı için IP kısıtlamaları uygulayabilir miyim?**  

    Evet. Azure dosya paylaşımınıza erişim depolama hesabı düzeyinde kısıtlanabilir. Daha fazla bilgi için [bkz.](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

* <a id="data-compliance-policies"></a>
**Azure Dosyaları hangi veri uyumluluk ilkelerini destekler?**  

   Azure Dosyaları, Azure Depolama'daki diğer depolama hizmetlerinde kullanılan aynı depolama mimarisinin üzerinde çalışır. Azure Dosyaları, diğer Azure depolama hizmetlerinde kullanılan aynı veri uyumluluk ilkelerini uygular. Azure Depolama veri uyumluluğu hakkında daha fazla bilgi için [Azure Depolama uyumluluk tekliflerine](https://docs.microsoft.com/azure/storage/common/storage-compliance-offerings)başvurabilir ve [Microsoft Güven Merkezi'ne](https://microsoft.com/trustcenter/default.aspx)gidebilirsiniz.
   
### <a name="ad-authentication"></a>AD Kimlik Doğrulaması
* <a id=""></a>
**Azure Files Azure AD kimlik doğrulaması Linux VM'lerini destekliyor mu?**

    Hayır, Linux VM'lerinden kimlik doğrulaması desteklenmez.

* <a id="ad-multiple-forest"></a>
**Azure Files AD kimlik doğrulaması, birden çok orman kullanarak bir REKLAM ortamıyla tümleştirmeyi destekliyor mu?**    

    Azure Files AD kimlik doğrulaması yalnızca depolama hesabının kayıtlı olduğu AD etki alanı hizmetinin ormanıyla tümleşir. Başka bir AD ormanından kimlik doğrulamasını desteklemek için, ortamınızın orman güveninin düzgün şekilde yapılandırılması gerekir. Azure Dosyaları'nın bir AD etki alanı hizmetine kaydolması, kimlik doğrulama için AD'de bir kimlik (bilgisayar veya hizmet oturum açma hesabı) oluşturduğu normal bir dosya sunucusuyla çoğunlukla aynıdır. Tek fark, depolama hesabının kayıtlı SPN etki alanı soneki ile eşleşmeyen "file.core.windows.net" ile sona ermesidir. Farklı etki alanı soneki nedeniyle birden çok orman kimlik doğrulamasını etkinleştirmek için DNS yönlendirme ilkesinizde herhangi bir güncelleştirme gerekip gerekip gerekip gerekip gerekip gerekmeden görmek için etki alanı yöneticinize danışın.

* <a id=""></a>
**Azure Dosyaları AD kimlik doğrulaması (önizleme) için hangi bölgeler kullanılabilir?**

    Ayrıntılar için [AD bölgesel kullanılabilirliğine](storage-files-identity-auth-active-directory-enable.md#regional-availability) bakın.

* <a id="ad-aad-smb-afs"></a>
**Azure Dosya Eşitlemi tarafından yönetilen dosya paylaşımlarında Azure Dosyaları Etkin Dizin (AD) kimlik doğrulaması (önizleme) kullanılabilir miyim?**

    Evet, Azure dosya eşitlemi tarafından yönetilen bir dosya paylaşımında AD kimlik doğrulamasını etkinleştirebilirsiniz. Yerel dosya sunucularında dizin/dosya NTFS AK'larında yapılan değişiklikler Azure Dosyalarına veya tam tersi olarak katmanlanır.

* <a id="ad-aad-smb-files"></a>
**Depolama hesabımda ve AD etki alanı bilgilerimde AD kimlik doğrulamasını etkinleştirip etkinleştirdiğimi nasıl kontrol edebilirim?**

    Azure Dosyaları AD Kimlik Doğrulaması'nın depolama hesabınızda etkinleştirilip etkinleştirilen ve AD etki alanı bilgilerini almak için [burada](https://docs.microsoft.com/azure/storage/files/storage-files-identity-auth-active-directory-enable#enable-ad-authentication-for-your-account) verilen yönergelere başvurabilirsiniz.

* <a id="ad-aad-smb-files"></a>
**AD'deki depolama hesabımı temsil etmek için bir bilgisayar hesabı veya hizmet oturum açma hesabı oluşturmanın bir farkı var mı?**

    Bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) veya bir hizmet oturum [açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) oluşturmanın, kimlik doğrulamanın Azure Dosyaları ile nasıl çalışacağı yla ilgili hiçbir farkı yoktur. Bir depolama hesabını AD ortamınızda kimlik olarak nasıl temsil edeceğimiz konusunda kendi seçiminizi yapabilirsiniz. Join-AzStorageAccountForAuth cmdlet'te ayarlanan varsayılan DomainAccountType bilgisayar hesabıdır. Ancak, AD ortamınızda yapılandırılan parola son kullanma tarihi bilgisayar veya hizmet oturum açma hesabı için farklı olabilir ve [bunu AD'deki depolama hesabı kimliğinizin parolasını güncelleştirmek](https://docs.microsoft.com/azure/storage/files/storage-files-identity-auth-active-directory-enable#5-update-ad-account-password)için dikkate almanız gerekir.

## <a name="on-premises-access"></a>Şirket içi erişim

* <a id="port-445-blocked"></a>
**ISS veya BT'm, Azure Dosyaları'nın montajını engelleyen Port 445'i engeller. Ne yapmalıyım?**

    Burada [engellenen bağlantı noktası 445 geçici çözüm için çeşitli yollar](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems#cause-1-port-445-is-blocked)hakkında bilgi edinebilirsiniz. Azure Files yalnızca Bölge dışından veya veri merkezi dışından SMB 3.0 (şifreleme desteği ile) kullanan bağlantılara izin verir. SMB 3.0 protokolü internet üzerinden kullanımı çok güvenli kanal şifreleme dahil olmak üzere birçok güvenlik özellikleri tanıttı. Ancak, port 445'in alt Kobİ sürümlerinde bulunan güvenlik açıklarının tarihsel nedenleriyle engellenmesi mümkündür. İdeal durumda, bağlantı noktası sadece SMB 1.0 trafiği için bloke edilmeli ve SMB 1.0 tüm istemciler üzerinde kapatılmalıdır.

* <a id="expressroute-not-required"></a>
**Azure Dosyaları'na bağlanmak veya Azure Dosya Eşitlemeyi'ni şirket içinde kullanmak için Azure ExpressRoute'u kullanmam gerekiyor mu?**  

    Hayır. ExpressRoute'un Azure dosya paylaşımına erişmek için gerekli değildir. Bir Azure dosya paylaşımını doğrudan şirket içinde monte ediyorsanız, tek gereken internet erişimi için bağlantı noktası 445'in (TCP giden) açık olmasıdır (bu, SMB'nin iletişim kurmak için kullandığı bağlantı noktasıdır). Azure Dosya Eşitlemesi kullanıyorsanız, gerekli olan tek şey HTTPS erişimi için 443 numaralı bağlantı noktasıdır (SMB gerekmez). Ancak, expressroute'u bu erişim seçeneklerinden herhangi biriyle *kullanabilirsiniz.*

* <a id="mount-locally"></a>
**Yerel makineme nasıl Azure dosya paylaşımı monte edebilirim?**  

    Bağlantı noktası 445 (TCP giden) açıksa ve istemciniz SMB 3.0 protokolünü destekliyorsa (örneğin, Windows 10 veya Windows Server 2016 kullanıyorsanız) SMB protokolünü kullanarak dosya paylaşımını birleştirebilirsiniz. Port 445 kuruluşunuzun ilkesi veya ISS'niz tarafından engellenmişse, Azure dosya paylaşımınıza erişmek için Azure Dosya Eşitlemeyi'ni kullanabilirsiniz.

## <a name="backup"></a>Backup
* <a id="backup-share"></a>
**Azure dosya paylaşımımı nasıl yedeklerim?**  
    Yanlışlıkla [silmelere](storage-snapshots-files.md) karşı korunmak için periyodik paylaşım anlık görüntülerini kullanabilirsiniz. Ayrıca AzCopy, Robocopy veya monte edilmiş bir dosya paylaşımını yedeklenebilen bir üçüncü taraf yedekleme aracı da kullanabilirsiniz. Azure Yedekleme, Azure Dosyaları'nın yedeklemesini sunar. Azure Yedekleme ile [Azure dosya paylaşımlarını yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-files)hakkında daha fazla bilgi edinin.

## <a name="share-snapshots"></a>Anlık görüntüleri paylaşma

### <a name="share-snapshots-general"></a>Anlık görüntüleri paylaşın: Genel
* <a id="what-are-snaphots"></a>
**Dosya paylaşımı anlık görüntüleri nedir?**  
    Dosya paylaşımlarınızın salt okunur sürümünü oluşturmak için Azure dosya paylaşımı anlık görüntülerini kullanabilirsiniz. Ayrıca Azure Dosyalarını, içeriğinizin önceki bir sürümünü aynı paylaşıma, Azure'da alternatif bir konuma veya daha fazla değişiklik için şirket içinde kopyalamak için de kullanabilirsiniz. Anlık görüntü paylaş hakkında daha fazla bilgi edinmek için [Anlık Görüntü Paylaş'a](storage-snapshots-files.md)bakın.

* <a id="where-are-snapshots-stored"></a>
**Paylaşım anlık görüntülerim nerede depolanır?**  
    Paylaşım anlık görüntüleri, dosya paylaşımıyla aynı depolama hesabında depolanır.

* <a id="snapshot-consistency"></a>
**Paylaşım anlık görüntüleri uygulama tutarlı mı?**  
    Hayır, paylaşım anlık görüntüleri uygulama tutarlı değildir. Kullanıcı, paylaşım anlık görüntüsünü almadan önce yazarlarını uygulamadan paylaşıma doğru temizlemeli.

* <a id="snapshot-limits"></a>
**Kullanabileceğim hisse anlık görüntü sayısıyla ilgili sınırlar var mı?**  
    Evet. Azure Dosyaları en fazla 200 paylaşım anlık görüntüsünü saklayabilir. Anlık görüntü, hisse kotası için geçerli olmadığından, tüm hisse anlık görüntüleri tarafından kullanılan toplam alanda hisse başına sınır yoktur. Depolama hesabı sınırları hala geçerlidir. 200 paylaşım anlık görüntüsünden sonra, yeni paylaşım anlık görüntüleri oluşturmak için eski anlık görüntüleri silmeniz gerekir.

* <a id="snapshot-cost"></a>
**Hisse anlık görüntüleri ne kadara mal olur?**  
    Anlık görüntü için standart işlem ve standart depolama maliyeti uygulanır. Enstantaneler doğada artımlıdır. Temel anlık görüntü, paylaşımın kendisidir. Sonraki tüm anlık görüntüler artımlıdır ve yalnızca önceki anlık görüntüden diff'i depolar. Bu, iş yükünüz en az olduğunda faturada görülecek delta değişikliklerinin en az olacağı anlamına gelir. Standart Azure Dosyaları fiyatlandırma bilgileri için [Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/files/) bakın. Bugün hisse anlık görüntü tarafından tüketilen boyuta bakmanın yolu, faturalanan kapasiteile kullanılan kapasiteyi karşılaştırmaktır. Raporlamayı geliştirmek için araç üzerinde çalışıyoruz.

* <a id="ntfs-acls-snaphsots"></a>
**Dizinler ve dosyalardaki NTFS AK'lar paylaşım anlık görüntülerinde kalıcı mıdır?**  
    Dizinler ve dosyalardaki NTFS AKLAR'ı paylaşım anlık görüntülerinde kalıcıdır.

### <a name="create-share-snapshots"></a>Paylaşım anlık görüntüsü oluşturma
* <a id="file-snaphsots"></a>
**Tek tek dosyaların paylaşım anlık görüntüsünü oluşturabilir miyim?**  
    Paylaşım anlık görüntüleri dosya paylaşımı düzeyinde oluşturulur. Dosya paylaşımı anlık görüntüsünden tek tek dosyaları geri yükleyebilirsiniz, ancak dosya düzeyinde paylaşım anlık görüntüleri oluşturamazsınız. Ancak, bir hisse düzeyinde paylaşım anlık görüntüsü aldıysanız ve belirli bir dosyanın değiştiği anlık görüntüleri listelemek istiyorsanız, bunu Windows'a monte edilmiş bir paylaşımda **Önceki Sürümler** altında yapabilirsiniz. 
    
    Bir dosya anlık görüntü özelliğine ihtiyacınız varsa, [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)adresinden bize bildirin.

* <a id="encrypted-snapshots"></a>
**Şifreli bir dosya paylaşımının anlık görüntülerini oluşturabilir miyim?**  
    Şifreleme saolun da etkinleştirilmiş olan Azure dosya paylaşımlarının paylaşım anlık görüntüsünü alabilirsiniz. Dosyaları paylaşım anlık görüntüsünden şifrelenmiş bir dosya paylaşımına geri yükleyebilirsiniz. Paylaşımınız şifrelenmişse, paylaşım anlık görüntünüz de şifrelenir.

* <a id="geo-redundant-snaphsots"></a>
**Paylaşım anlık görüntülerim coğrafi olarak yedekli mi?**  
    Paylaşım anlık görüntüleri, alındıkları Azure dosya paylaşımıyla aynı artıklığa sahiptir. Hesabınız için coğrafi yedekli depolama alanını seçtiyseniz, paylaşım anlık görüntünüz de eşleşmiş bölgede gereksiz olarak depolanır.

### <a name="manage-share-snapshots"></a>Hisse anlık görüntülerini yönetme
* <a id="browse-snapshots-linux"></a>
**Linux'tan paylaşım anlık görüntülerime göz atabilir miyim?**  
    Linux'ta paylaşım anlık görüntülerini oluşturmak, listelemek, göz atmak ve geri yüklemek için Azure CLI'yi kullanabilirsiniz.

* <a id="copy-snapshots-to-other-storage-account"></a>
**Paylaşım anlık görüntülerini farklı bir depolama hesabına kopyalayabilir miyim?**  
    Dosyaları anlık görüntü paylaş'tan başka bir konuma kopyalayabilirsiniz, ancak paylaşım anlık görüntülerini kopyalayamazsınız.

### <a name="restore-data-from-share-snapshots"></a>Paylaşım anlık görüntülerinden verileri geri yükleme
* <a id="promote-share-snapshot"></a>
**Bir hisse anlık görüntüsünü temel paylaşıma tanıtabilir miyim?**  
    Verileri bir paylaşım anlık görüntüsünden başka bir hedefe kopyalayabilirsiniz. Bir hisse anlık görüntüsünü temel paya yükseltemezsiniz.

* <a id="restore-snapshotted-file-to-other-share"></a>
**Paylaşım anlık görüntümdeki verileri farklı bir depolama hesabına geri yükleyebilir miyim?**  
    Evet. Paylaşım anlık görüntüsündeki dosyalar özgün konuma veya aynı bölgede veya farklı bölgelerde aynı depolama hesabı veya farklı bir depolama hesabı içeren alternatif bir konuma kopyalanabilir. Ayrıca dosyaları şirket içi bir konuma veya başka bir buluta kopyalayabilirsiniz.    
  
### <a name="clean-up-share-snapshots"></a>Paylaşım anlık görüntülerini temizleme
* <a id="delete-share-keep-snapshots"></a>
**Payımı silebilir ama paylaşım anlık görüntülerimi silemez miyim?**  
    Paylaşımınızda etkin paylaşım anlık görüntüleri varsa, payınızı silemezsiniz. Paylaşım anlık görüntülerini ve paylaşımla birlikte silmek için api kullanabilirsiniz. Ayrıca, azure portalındaki hem paylaşım anlık görüntülerini hem de paylaşımı silebilirsiniz.

* <a id="delete-share-with-snapshots"></a>
**Depolama hesabımı silersem paylaşım anlık görüntülerime ne olur?**  
    Depolama hesabınızı silerseniz, paylaşım anlık görüntüleri de silinir.

## <a name="billing-and-pricing"></a>Faturalama ve fiyatlandırma
* <a id="vm-file-share-network-traffic"></a>
**Azure VM ile Azure dosya paylaşımı arasındaki ağ trafiği, aboneliğe yüklenen dış bant genişliği olarak sayılır mı?**  
    Dosya paylaşımı ve VM aynı Azure bölgesindeyse, dosya paylaşımı ile VM arasındaki trafik için ek bir ücret alınmaz. Dosya paylaşımı ve VM farklı bölgelerdeyse, aralarındaki trafik dış bant genişliği olarak ücretlendirilir.

* <a id="share-snapshot-price"></a>
**Hisse anlık görüntüleri ne kadara mal olur?**  
     Önizleme sırasında, hisse anlık görüntü kapasitesi için herhangi bir ücret alınmaz. Standart depolama çıkış ve işlem maliyetleri uygulanır. Genel kullanılabilirlik sonrasında, abonelikler kapasite ve hisse anlık görüntülerindeki hareketler için ücretlendirilir.
     
     Paylaşım anlık görüntüleri doğada artımlı. Temel paylaşım anlık görüntüsü, paylaşımın kendisidir. Sonraki tüm paylaşım anlık görüntüleri artımlı ve yalnızca önceki paylaşım anlık görüntüsünden farkı depolayın. Yalnızca değiştirilen içerik için faturalandırılırsınız. 100 GiB veriile bir paylaşımınız varsa ancak son paylaşım anlık görüntünüzden bu yana yalnızca 5 GiB değiştiyse, paylaşım anlık görüntüsü yalnızca 5 ek GiB tüketir ve 105 GiB için faturalandırılırsınız. İşlem ve standart çıkış ücretleri hakkında daha fazla bilgi için [Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/files/)bakın.

## <a name="scale-and-performance"></a>Ölçek ve performans
* <a id="files-scale-limits"></a>
**Azure Dosyaları'nın ölçek sınırları nelerdir?**  
    Azure Dosyaları için ölçeklenebilirlik ve performans hedefleri hakkında bilgi için [Azure Dosyaları ölçeklenebilirliği ve performans hedefleri'ne](storage-files-scale-targets.md)bakın.

* <a id="need-larger-share"></a>
**Azure dosya paylaşımları için hangi boyutlar kullanılabilir?**  
    Azure dosya paylaşım boyutları (premium ve standart) 100 TiB'ye kadar ölçeklenebilir. Standart katman için daha büyük dosya paylaşımlarına onboarding yönergeleri için planlama kılavuzunun [daha büyük dosya paylaşımları (standart katman)](storage-files-planning.md#enable-standard-file-shares-to-span-up-to-100-tib) bölümüne onboard bölümüne bakın.

* <a id="lfs-performance-impact"></a>
**Dosya paylaşım kotamı genişletmek iş yüklerimi veya Azure Dosya Eşitlememi etkiler mi?**
    
    Hayır. Kotayı genişletmek iş yüklerinizi veya Azure Dosya Eşitlemenizi etkilemez.

* <a id="open-handles-quota"></a>
**Aynı dosyaya aynı anda kaç istemci erişebilir?**   
    Tek bir dosyada 2.000 açık tutamaç kotası vardır. 2.000 açık tanıtıcınız olduğunda, kotaya ulaşıldığını belirten bir hata iletisi görüntülenir.

* <a id="zip-slow-performance"></a>
**Azure Dosyaları'ndaki dosyaların zip'ini kaldırdığınızda performansım yavaş. Ne yapmalıyım?**  
    Çok sayıda dosyayı Azure Dosyaları'na aktarmak için AzCopy (Windows için; Linux ve UNIX için önizlemede) veya Azure PowerShell kullanmanızı öneririz. Bu araçlar ağ aktarımı için optimize edilmiştir.

* <a id="slow-perf-windows-81-2012r2"></a>
**Azure dosya paylaşımımı Windows Server 2012 R2 veya Windows 8.1'e monte ettikten sonra performansım neden yavaş?**  
    Windows Server 2012 R2 ve Windows 8.1'de Azure dosya paylaşımı yaparken bilinen bir sorun vardır. Sorun, Windows 8.1 ve Windows Server 2012 R2 için Nisan 2014 kümülatif güncelleştirmesinde düzeltildi. Optimum performans için, Windows Server 2012 R2 ve Windows 8.1'in tüm örneklerinin bu düzeltme eki uygulandığından emin olun. (Windows Update aracılığıyla her zaman Windows yamaları almalısınız.) Daha fazla bilgi için, Windows [8.1 veya Server 2012 R2'den Azure Dosyalarına erişirken](https://support.microsoft.com/kb/3114025)ilişkili Microsoft Bilgi Bankası makalesi Yavaş performansa bakın.

## <a name="features-and-interoperability-with-other-services"></a>Diğer hizmetlerle birlikte çalışabilirlik özellikleri ve birlikte çalışabilirlik
* <a id="cluster-witness"></a>
**Azure dosya paylaşımımı Windows Server Failover Kümem için *Dosya Paylaşımı Tanığı* olarak kullanabilir miyim?**  
    Şu anda, bu yapılandırma bir Azure dosya paylaşımı için desteklenmez. Azure Blob depolama alanı için bu kurulumu niçin ayarlayınız hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)

* <a id="containers"></a>
**Azure Kapsayıcıörneğine Azure dosya paylaşımı ekleyebilir miyim?**  
    Evet, Azure dosya paylaşımları, bilgileri kapsayıcı örneğinin ömrü boyunca devam etmek istediğinizde iyi bir seçenektir. Daha fazla bilgi için Azure [Kapsayıcı örnekleriyle Azure dosya paylaşımını dağıt'a](../../container-instances/container-instances-mounting-azure-files-volume.md)bakın.

* <a id="rest-rename"></a>
**REST API'de yeniden adlandırma işlemi var mı?**  
    Şu anda değil.

* <a id="nested-shares"></a>
**İç içe hisseler ayarlayabilir miyim? Başka bir deyişle, bir hisse altında bir pay?**  
    Hayır. Dosya *paylaşımı,* monte edebilirsiniz sanal sürücü, bu nedenle iç içe paylaşımlar desteklenmez.

* <a id="ibm-mq"></a>
**IBM MQ ile Azure Dosyalarını nasıl kullanırım?**  
    IBM, IBM MQ müşterilerinin Azure Dosyalarını IBM hizmetiyle yapılandırmasını sağlayan bir belge yayımladı. Daha fazla bilgi için, [Microsoft Azure Dosyaları hizmetiyle ibm MQ çok örnekli sıra yöneticisi nasıl ayarlayabilirsiniz'a](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service)bakın.

## <a name="see-also"></a>Ayrıca bkz.
* [Windows'da Azure Dosyaları Sorun Giderme](storage-troubleshoot-windows-file-connection-problems.md)
* [Linux'ta Azure Dosyalarını Sorun Giderme](storage-troubleshoot-linux-file-connection-problems.md)
* [Azure Dosya Eşitleme ile ilgili sorunları giderme](storage-sync-files-troubleshoot.md)
