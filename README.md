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

