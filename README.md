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
class Article(models.Model):
    author=models.ForeignKey("auth.user",on_delete=models.CASCADE) ======> (kullanıcı silindiğinde oluşturduğu article'da silinecek)
    title=models.CharField(max_length=50) ====>  (bizim başlığımızı simgeleyecek)
    content=models.TextField()  ======>   (???)
    created_date = models.DateTimeField(auto_now=True) ======>(makeleyi oluşturduğumuz zamanın tarihini otomatik olarak kaydeder)
kodlarını yazyoruz.    
daha sonra **admin.py** kısmına gelip 
from models importh app'in ismini yazıyoruz
sonra
admin.site.register(app'in ismi) yazıyoruz
sonra **settings.py** de **INSTALLED_APPS** kısmına gelip ""app ismini"" yazıyoruz
şuan sitede app kısmımız oluştu fakat girdiğimiz zaman hata alıcağız sebebi ise uygulamaların içindeki modelleri veritabanında oluşturmadık o yüzden bu hatayı almamız gayet normal. Bu hatayı almamak için ise şöyle bir yol izlememiz gerek. 
Terminale gelip **python magrate.py makemigrations** yazıp appi oluşturmamız lazım daha sonra yine terminale gelip **python manage.py migrate** yazıp tabloyu oluşturmamız lazım.
Şuan her şey hazır sitemize gidip article ekleyebiliriz . Ayrıca herhangi bir kullanıcını yazdığı şeyleri biz kullanıcıyı sildiğimiz zaman yazdıkları article'da otomatik olarak silenecektir. Buna sebep olan kodumuz ise **author=models.ForeignKey("auth.user",on_delete=models.CASCADE)** budur.

----

## Admin panelini özelleştirmek 

1) Admin panelinde app kısmındaki yazdığımız title,content,date,author gibi ksımları özelleştirmek için **,verbose_name = tarih,başlık,yazar,içerik vb.** şeklinde değiştirebiliriz.

2) Oluşturduğumuz projeleri normalde article object şeklinde görürüz. Bunu değiştirmek istersek **models.py** kısmına gelip : 
 def __init__(self):
    return self.title yazarsak oluşan projelerin ismi başlık şeklinde oluşacak title yerine date yazarsak projenin ismi tarih şeklinde gözükecek.
    
3) Admin.py'de yazdığımız **admin.register(Article)'ı** silip bunu decarotor şeklinde yazıcağız . Yani **@admin.register(Article)** şeklinde yazıcağız.
bunu yazdıktan sonra aşağısına şu şekilde bir class oluşturmamız lazım **class ArticleAdmin(admin.ModelAdmin):** bunu yazıp aşağısına ise **class(Meta):** yazmamız bu django bize sağladığı bir komut yani metanın içine herhangibir şey yazamayız . class(Meta): yazdıktan sonra article admin ile article bağlamak için metanın içine **model=Article** yazmamız lazım

4) Sırada **list_display** özellğimiz var. Bu özellik sitemizde oluşturduğum makalelerin sadece başlığını değil tarih yazar vb. özellikleri de görmemizi sağlayacak . Bu kodu **admin.py ' de ilk oluşturduğumuz classın altına  list_display=["author","created_date"] şeklinde yazmamız gerek. **

5) Artık makeleler sadece başlıkları  değil tarih ve yazarları da gözükecek fakat bunlarada herhangi bir link olmayacak . Bunlara link eklemek için ise
 list_displayin altına **list_display_link=["author","created_date"]** şeklinde yazarsak artık tarih ve yazar üstünde de link olacak.
 
 6) Şimdi oluşturduğumuz makaleleri daha kolay oluşturmak için **admin.py kısmına search_fields = ["title"]** şeklinde yazarsak artık makaleleri başlığı aratarak kolaycak bulabileceğiz.
 
 7)  Makaleleri oluşturduğumuz tarihe göre filtrelemeyi ise **admin.py kısmına gelip list_filter = [""created date ]** yazarak sağlamış oluruz.
