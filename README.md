# django_server

## django server açma : 

django'da server açamk için terminal kısımına gelip "python manage.py runserver" yazmamız lazım eğer sayfanın dilini türkçeye çevirmek istersek açtığın dosyanın
altındaki **settings.py** klosorüne gelip 106.satırdaki yere tr yazmamız gerekir. Eğer saatide türkiye saati ile değiştirmek istersek 108. kod yeribe **Europe/Istanbul**
yazmamız gerek.Daha sonrasında bunları kaydettiğimizde teriminalden bize "Starting development server at http://127.0.0.1:8000/" şu şekilde bir cevap gelicek . Bunu google **localhost:8000** şeklinde yazarsak sayfamızı açmış oluruz

----

## django hazır admin paneline gitme :

django'da admin paneline gitmek için **localhost:8000/admin** yazmamız lazım ondan sonra karşımıza bir kullanıcı adı ve şifre gelecek o şifreyi oluşturmak için ilk 
vsc teriminaline **python manage.py migrate** yazmamız lazım gerekli migrationları yaptıktan sonra giriş yapabilmek için bir superuser oluşturuyoruz. Onu da
yine vsc'ye gelip terminale **python manage.py createsuperuser** yazmamız lazım daha sonra terminal bizden kullanıcı adı e-mail ve paralo isteyecektir. Bunları yaptıktan sonra sayfaya gidip kullanıcı adı ve parolayı girerek panele erişebiliriz

----

## django uygulama kavramı ve uygulama oluşturma :

İlk başta app oluşturmak için vsc'da gelip terminale **python manage.py startapp uygulama_ismi** yazmamız lazım ve uygulamamız oluşacak 
oluşan uygulamanın dosyalrında
1)apps.py=uygulamamızın ismini gösterir.
2)test.py=burada uygulamanın testlerini yazarız
3)views.py=herhangi bir url mapine göre çalışan fonkisyonlarımızı barındıran dosyamız.

----

## oluşan app'e model oluşturma ve admin arayüzüne ekleme :

serverin modelini oluşturmak için öncelikle **models.py kısmına gelip**
**class Article(models.Model):**
    **author=models.ForeignKey("auth.user",on_delete=models.CASCADE)** ======> (kullanıcı silindiğinde oluşturduğu article'da otomatik olarak silinecek)
    **title=models.CharField(max_length=50)** ====>  (bizim başlığımızı simgeleyecek)
    **content=models.TextField()**  ======>   (bizim içeriğimizi belirletecek)
    **created_date = models.DateTimeField(auto_now=True)** ======>(makeleyi oluşturduğumuz zamanın tarihini otomatik olarak kaydeder)
kodlarını yazyoruz.    
daha sonra **admin.py** kısmına gelip 
**from .models importh  app_ismi** yazıyoruz
sonra
**admin.site.register app_ismi** yazıyoruz
sonra **settings.py** de **INSTALLED_APPS** kısmına gelip ""app ismini"" yazıyoruz
şuan sitede app kısmımız oluştu fakat girdiğimiz zaman hata alıcağız sebebi ise uygulamaların içindeki modelleri veritabanında oluşturmadık o yüzden bu hatayı almamız gayet normal. Bu hatayı almamak için ise şöyle bir yol izlememiz gerek. 
Terminale gelip **python manage.py makemigrations** yazıp appi oluşturmamız lazım daha sonra yine terminale gelip **python manage.py migrate** yazıp tabloyu oluşturmamız lazım.
Şuan her şey hazır sitemize gidip article ekleyebiliriz . Ayrıca herhangi bir kullanıcını yazdığı şeyleri biz kullanıcıyı sildiğimiz zaman yazdıkları article'da otomatik olarak silenecektir. Buna sebep olan kodumuz ise **author=models.ForeignKey("auth.user",on_delete=models.CASCADE)** budur.

----

## Admin panelini özelleştirmek 

1) Admin panelinde app kısmındaki yazdığımız title,content,date,author gibi ksımları özelleştirmek için **,verbose_name = tarih,başlık,yazar,içerik vb.** şeklinde değiştirebiliriz.

2) Oluşturduğumuz projeleri normalde article object şeklinde görürüz. Bunu değiştirmek istersek **models.py** kısmına gelip : 
 def __init__(self):
    **return self.title** yazarsak oluşan projelerin ismi başlık şeklinde oluşacak title yerine date yazarsak projenin ismi tarih şeklinde gözükecek.
    
3) Admin.py'de yazdığımız **admin.register(Article)'ı** silip bunu decarotor şeklinde yazıcağız . Yani **@admin.register(Article)** şeklinde yazıcağız.
bunu yazdıktan sonra aşağısına şu şekilde bir class oluşturmamız lazım **class ArticleAdmin(admin.ModelAdmin):** bunu yazıp aşağısına ise **class(Meta):** yazmamız bu django bize sağladığı bir komut yani metanın içine herhangibir şey yazamayız . **class(Meta):** yazdıktan sonra article admin ile article bağlamak için metanın içine **model=Article** yazmamız lazım

4) Sırada **list_display** özellğimiz var. Bu özellik sitemizde oluşturduğum makalelerin sadece başlığını değil tarih yazar vb. özellikleri de görmemizi sağlayacak . Bu kodu **admin.py ' de ilk oluşturduğumuz classın altına  list_display=["author","created_date"] şeklinde yazmamız gerek. **

5) Artık makeleler sadece başlıkları  değil tarih ve yazarları da gözükecek fakat bunlarada herhangi bir link olmayacak . Bunlara link eklemek için ise
 list_displayin altına **list_display_links=["author","created_date"]** şeklinde yazarsak artık tarih ve yazar üstünde de link olacak.
 
 6) Şimdi oluşturduğumuz makaleleri daha kolay bulmak için **admin.py kısmına search_fields = ["title"]** şeklinde yazarsak artık makaleleri başlığı aratarak kolaycak bulabileceğiz.
 
 7)  Makaleleri oluşturduğumuz tarihe göre filtrelemeyi ise **admin.py kısmına gelip list_filter = [""created date ]** yazarak sağlamış oluruz.

----

## Django  Shell ile ORM sorgularını kullanma : 
1)ilk olarak yeni bir terminal açıyoruz. Ve djangonun bize verdiği shell'i açmak için terminale gelip **python manage.py shell** yazıyoruz.
2)django'nun kendi user modelini almak istersek bu  user modeli ise **from django.contrib.auth.models import User** uygulamasının içinde. Bunu terminale yazdıktan sonra **from article.models import Article** yazıyoruz. Bunun sebebi ise article uygulamasından models kısmını dahil edip ve bunun içindeki daha önceden oluşturduğumuz Article modelini dahil ediyoruz

3) şimdi User modelinden bir obje oluşturalım. Bunun farklı yöntemleri var ilk yöntemi hemen deneyelim:
    1)Terminale gelip **newUser = User(username="denemekullanıcı",password="123")** bunu veritabanına kaydetmek için ise **newUser.save()** dememiz lazım.
    2)**newUser2=User(username="denemekullanıcı2")** yazdıktan sonra **newUser2.set password("123)** bu ifade passwordumuzu alıcak ve bunu şifreleyerek bir password oluşturcak. En sonunda ise tekrardan **newUser2.save()** yazarak kaydebiliriz.
    3)başka bir şekilde oluşturmak istersek **newUser3 = User()** yazdıktan sonra **newUser3.username="denemekullanıcı3"** sonra **newUser3.set_password("123")** ve **newUser.firstname ="alperen"** diyebiliriz. Ve en sonunda kaydetmek için **newUser3.save()** diyerek kaydedebiliriz.
    4)başka bir şekli ise **article = Article(title = "django shell deneme",content= "içerik",author="newuser")** yazdıktan sonra **article.save()** diyip kaydedebiliriz.
  
4) Eğer bir article'ı değiştirmek istersek **article.title="django shell deneme değişti"** dersek article'ın title   "article.title="django shell deneme değişti" şeklinde değişir. Ama en sonunda **article.save()** yazmayı unutmamız lazım.
5) Eğer bütün projeleri görmek istersek **Article.objects.all()** yazmamız lazım.
6) Eğer bir title'a göre makale aramak istersek **Article.objects.get(title="makale_ismi")** yazmamız yeterli olur. Bulduğumuz article'ı silmek istersek **article_delete()** yazarsak silmiş oluruz. 

----

## Django URL  yapısı : 

Biz burada belli bir URL' ye göre belirli bir fonksiyon çalıştıracağız. Ancak onun öncesinde ilk olarak **settings.py** dosyamıza gelelim. Burada bulunan templates'leri bir yere koymak için 59.satırda bulunan DIRS ksımına **templates** yazalım. Artık django belirli templates aradoğı zaman yazdığımız templates klasörüne bakacak. Bu klasörüde blog kısmından new folder diyerek ekleyelim.  Artık django belli bir templates aradığı zaman templates klasörünün altına bakmak zorunda.  Daha sonra açtığımız templates klasörüne gelip new file diyerek **index.html** adlı bir dosya oluşturalım.
Sonra ! işareti yapıp bir templates ana yapsıın belirleyelim. Belirlenen ana yapını 7 satırına YbBlog diylelim sonra 10. satıra h3 etiketiyle anasayfa yazalım. Şimdi sıra URL mapini yazmakta. Blog kısmındaki **urls.py** dosyasına geldeiğimiz zaman daha önce yazılan bir admin URL'sini görüyoruz. Zaten biz daha önce localhost:8000/admin yazdığımızda site karşımıza geliyordu . Bunu değiştirmke için app'imizin altındaki **views.py** kısmına gelip URL gelince çalışacak fonksiyonumuzu yazmamız lazım . İlk olarak view kısmındaki ilk satırdaki render'den sonra **,HttpResponse** yazmamız lazım. Daha sonra
**def index(request):**
    **retrun HttpResponse("Anasayfa")
 
 yazmamız lazım.
 Buradaki request değişkeni bize django otomatik olarak gönderiyor ve her view fonksiyonunda kesinlikle bulunması gerekir.
 
  Daha sonra **urls.py kısmına views.py 'de** yazdığımız fonksiyonu kullanmamız lazım . Onun için urls.py kısmına gelip **from article.views import index** yazıp views'e dahil etmemiz lazım.
  sonra  **path('',index)** yazmamız gerek . Artık anasayfaya gittiğimiz zaman fonksiyonumuz çalışcak.
  
  ----
  
  ## Statik dosyaları kullanma :
  İlk olarak static dosyasının settings.py kısmının apps kısmında oluğundan emin olalım. Daha sonra biz article app'imizin altına new folder diyerek **static** adında bir dosya oluşturalım. Hemen sonra oluşturduğumuz static'in altına **style.css** bir dosya oluşturalım. Oluşturduğumuz dosyaya gelip anasayfadaki p elementlerinin rengini değiştirmek için şu kodu yazalım
  
 **p {
      background: red;
     }**
yazalım. Bunu yaptığımız zaman django otomatik olarak static dosyalarının altına bakıcak. Daha sonra **index.html** dosyasına gelip ilk satırın altına **{% load static %}** yazalım daha sonrasında şu kodumuzu  **style.css dosymızı index dosyamıza dahil etmek için şu kodu yazalım**  <link rel="stylesheet" href = "{% static 'style.css' %}"> yazalım.

----

##
  
  
  
  
  



