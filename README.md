# django_server

## django server açma : 

django'da server açamk için terminal kısımına gelip "python manage.py runserver" yazmamız lazım eğer sayfanın dilini türkçeye çevirmek istersek açtığın dosyanın
altındaki **settings.py** klosorüne gelip 106.satırdaki yere tr yazmamız gerekir. Eğer saatide türkiye saati ile değiştirmek istersek 108. kod yeribe **Europe/Istanbul**
yazmamız gerek.Daha sonrasında bunları kaydettiğimizde teriminalden bize "Starting development server at http://127.0.0.1:8000/" şu şekilde bir cevap gelicek . Bunu google **localhost:8000** şeklinde yazarsak sayfamızı açmış oluruz

## django hazır admin paneline gitme :

django'da admin paneline gitmek için **localhost:8000/admin** yazmamız lazım ondan sonra karşımıza bir kullanıcı adı ve şifre gelecek o şifreyi oluşturmak için ilk 
vsc teriminaline **python manage.py migrate** yazmamız lazım gerekli migrationları yaptıktan sonra giriş yapabilmek için bir superuser oluşturuyoruz. Onu da
yine vsc'ye gelip terminale **python manage.py createsuperuser** yazmamız lazım daha sonra terminal bizden kullanıcı adı e-mail ve paralo isteyecektir. Bunları yaptıktan sonra sayfaya gidip kullanıcı adı ve parolayı girerek panele erişebiliriz

## django uygulama kavramı ve uygulama oluşturma :

İlk başta app oluşturmak için vsc'da gelip terminale **python manage.py startapp uygulama_ismi** yazmamız lazım ve uygulamamız oluşacak 
oluşan uygulamanın dosyalrında
1)apps.py=uygulamamızın ismini gösterir.
2)test.py=burada uygulamanın testlerini yazarız
3)views.py=herhangi bir url mapine göre çalışan fonkisyonlarımızı barındıran dosyamız.

## oluşan app'e model oluşturma ve admin arayüzüne ekleme :




