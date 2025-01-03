# Django μοντέλα (models)

Αυτό που θέλουμε να δημιουργήσουμε τώρα είναι κάτι που θα αποθηκεύει όλες τις αναρτήσεις του blog μας. Αλλά για να είμαστε σε θέση να το κάνουμε, πρέπει να μιλήσουμε λίγο για το τι ονομάζουμε `"αντικείμενα" (objects)`.

## Objects

Υπάρχει μια έννοια στον προγραμματισμό που ονομάζεται `"Αντικειμενοστραφής προγραμματισμός`. Η ιδέα είναι ότι αντί να γράφετε τα πάντα ως μια βαρετή ακολουθία από προγραμματιστικές εντολές, μπορούμε να μοντελοποιήσουμε τα πράγματα και να καθορίσουμε πως αλληλεπιδρούν μεταξύ τους.

Άρα, τι είναι ένα αντικείμενο; Είναι μια συλλογή από ιδιότητες και συμπεριφορές. Ακούγεται παράξενο, αλλά θα σας δώσουμε ένα παράδειγμα.

Εάν θέλουμε να μοντελοποιήσουμε μια γάτα, θα δημιουργήσουμε ένα αντικείμενο `Γάτα` το οποίο έχει κάποιες ιδιότητες όπως `χρώμα`, `ηλικία`, `διάθεση` (όπως καλή, κακή, νυσταγμένη :)), και `ιδιοκτήτης`(που θα μπορεί να ανατεθεί σε ένα αντικείμενο `άτομο` ή ίσως, στην περίπτωση μιας αδέσποτης γάτας, αυτή η ιδιότητα θα μπορούσε να είναι άδεια).

Έπειτα η `γάτα` έχει κάποιες δράσεις `γουργούρισμα`, `ξύσιμο` ή `τάισμα`( σε αυτή την περίπτωση, θα δώσουμε στην γάτα λίγη `γατοτροφή`, που θα μπορούσε να είναι ένα ξεχωριστό αντικείμενο με ιδιότητες, όπως `γεύση`).

    Cat
    --------
    color
    age
    mood
    owner
    purr()
    scratch()
    feed(cat_food)
    

    CatFood
    --------
    taste
    

Βασικά η ιδέα είναι να περιγράψουμε τα αληθινά πράγματα στον κώδικα με ιδιότητες (που ονομάζονται `object properties`) και συμπεριφορές (που ονομάζονται `methods`).

Πώς θα μοντελοποιήσουμε τότε τις αναρτήσεις του blog; Θέλουμε να κατασκευάσουμε ένα blog, σωστά;

Πρέπει να δώσουμε απάντηση στο ερώτημα: τι είναι μία ανάρτηση στο blog; Τι ιδιότητες πρέπει να έχει;

Λοιπόν, σίγουρα μία ανάρτηση στο blog μας χρειάζεται κάποιο κείμενο με το περιεχόμενό του και έναν τίτλο, σωστά; Θα ήταν επίσης ωραίο να ξέρουμε ποιος το έγραψε αυτό. Έτσι χρειαζόμαστε έναν συγγραφέα. Τέλος, θέλουμε να γνωρίζουμε πότε δημιουργήθηκε και δημοσιεύτηκε η συγκεκριμένη ανάρτηση.

    Post
    --------
    title
    text
    author
    created_date
    published_date
    

Τι είδους πράγματα θα μπορούσαν να γίνουν με μια ανάρτηση στο blog; Θα ήταν ωραίο να έχουμε κάποια `method` που δημοσιεύει την ανάρτηση, σωστά;

Έτσι, θα χρειαστούμε μια μέθοδο `publish`.

Δεδομένου ότι γνωρίζουμε ήδη τι θέλουμε να επιτύχουμε, ας ξεκινήσουμε τη μοντελοποίηση στο Django!

## Django model

Γνωρίζοντας τι είναι ένα αντικείμενο, μπορούμε να δημιουργήσουμε ένα μοντέλο Django για την ανάρτηση στο blog μας.

Ένα μοντέλο στο Django είναι ένα ιδιαίτερο είδος αντικειμένου το οποίο αποθηκεύεται στη βάση δεδομένων `database`. Η βάση δεδομένων είναι μια συλλογή δεδομένων. Είναι εκεί όπου θα αποθηκευτούν όλες οι πληροφορίες για τους χρήστες, τις αναρτήσεις στο blog, κλπ. Θα χρησιμοποιήσουμε την βάση δεδομένων SQLite για την αποθήκευση των δεδομένων μας. Αυτός είναι η προεπιλεγμένη βάση δεδομένων του Django. Είναι αρκετό για εμάς για την ώρα.

Μπορείτε να σκεφτείτε ένα μοντέλο της βάσης δεδομένων ως ένα υπολογιστικό φύλλο με στήλες (πεδία) και γραμμές (δεδομένα).

### Δημιουργία μιας εφαρμογής

Για να κρατήσουμε τα πάντα τακτοποιημένα, θα δημιουργήσουμε μια ξεχωριστή εφαρμογή μέσα στο project μας. Είναι πολύ ωραίο να έχουμε τα πάντα οργανωμένα από την αρχή. Για την δημιουργία μιας εφαρμογής θα χρειαστεί να τρέξουμε την ακόλουθη εντολή στην κονσόλα (από τον φάκελο `djangogirls` όπου βρίσκεται το αρχείο `manage.py`):

{% filename %}macOS and Linux:{% endfilename %}

    (myvenv) ~/djangogirls$ python manage.py startapp blog
    

{% filename %}Windows:{% endfilename %}

    (myvenv) C:\Users\Name\djangogirls> python manage.py startapp blog
    

Θα παρατηρήσετε ότι ένας νέος φάκελος με το όνομα `blog` δημιουργήθηκε και περιέχει έναν αριθμό αρχείων. Οι φάκελοι και τα αρχεία στο project μας πρέπει να μοιάζουν κάπως έτσι:

    djangogirls
    ├── blog
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── apps.py
    │   ├── migrations
    │   │   └── __init__.py
    │   ├── models.py
    │   ├── tests.py
    │   └── views.py
    ├── db.sqlite3
    ├── manage.py
    ├── mysite
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    └── requirements.txt
    

Μετά την δημιουργία μίας εφαρμογής, πρέπει επίσης να πούμε στο Django ότι πρέπει να τη χρησιμοποιήσει. Το κάνουμε αυτό μέσα στο αρχείο `mysite/settings.py`. Ανοίξτε το. Πρέπει να βρούμε την λίστα `INSTALLED_APPS` και να προσθέσουμε εκεί το εξής: `'blog',` πάνω από το `]`. Έτσι το τελικό προϊόν πρέπει να μοιάζει κάπως έτσι:

{% filename %}mysite/settings.py{% endfilename %}

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```

### Δημιουργία ενός post model

Μέσα στο αρχείο `blog/models.py` ορίζουμε όλα τα objects με το όνομα `Models`. Αυτό είναι ένα μέρος όπου θα ορίσουμε το post μοντέλο μας.

Ας ανοίξουμε το αρχείο `blog/models.py`, διαγράψτε τα περιεχόμενα του και γράψτε τα εξής:

{% filename %}blog/models.py{% endfilename %}

```python
from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

> Σιγουρευτείτε ότι χρησιμοποιείτε δύο φορές την κάτω παύλα (`_`) σε κάθε πλευρά του `str`. Αυτό είναι απαραίτητο και συχνά θα το συναντάτε στην Python. Συνηθίζεται να ονομάζεται "dunder" (σύντμηση για "double-underscore").

Μη φοβάστε. Θα εξηγήσουμε τι συμβαίνει!

Όλες οι γραμμές που ξεκινούν με τις λέξεις `from` ή `import` είναι γραμμές που εισάγουν λειτουργίες από άλλα Python αρχεία (αυτά με την κατάληξη .py). Έτσι αντί να κάνουμε αντιγραφή-επικόλληση κώδικα, πολύ απλά συμπεριλαμβάνουμε κώδικα από άλλα αρχεία με τη χρήση του `from ... import ...`.

`class Post(models.Model):` – αυτή η γραμμή ορίζει το μοντέλο μας (είναι μια κλάση που με τη σειρά της είναι και αυτή ένα `object`).

- Η λέξη `class` είναι μια ιδιαίτερη λέξη-κλειδί που καθορίζει τον ορισμό μιας κλάσης.
- Η λέξη `Post` είναι το όνομα του μοντέλου μας. Μπορούμε να του δώσουμε όποιο όνομα θέλουμε (αλλά πρέπει να αποφεύγουμε ιδιαίτερους χαρακτήρες και κενά). Πάντα το όνομα μιας κλάσης να ξεκινάει με ένα κεφαλαίο γράμμα.
- Η γραμμή `models.Model` σημαίνει ότι το Post είναι ένα Django Model. Με αυτό τον τρόπο το Django θα ξέρει ότι θα πρέπει να αποθηκεύεται σε μια βάση δεδομένων.

Τώρα ορίζουμε τα properties που λέγαμε: `title`, `text`, `created_date`, `published_date` και `author`. Για να το κάνουμε αυτό θα πρέπει να ορίσουμε τον τύπο τιμών που θα δέχεται το κάθε πεδίο (είναι κείμενο; αριθμός; ημερομηνία; κάποια συσχέτιση με ένα άλλο object, όπως ένας χρήστης;)

- `models.CharField` – έτσι δηλώνετε ότι θέλετε να ορίσετε ένα κείμενο με συγκεκριμένο αριθμό χαρακτήρων.
- `models.TextField`- αυτό είναι για μεγάλα κείμενα χωρίς όριο. Ακούγεται ιδανικό για περιεχόμενο δημοσιεύσεων blog, έτσι δεν είναι;
- `models.DateTimeField`- αυτό είναι για ημερομηνία και ώρα.
- `models.ForeignKey`- αυτό είναι ένας σύνδεσμος για ένα άλλο μοντέλο.

Δεν θα εξηγήσουμε κάθε κομμάτι του κώδικα διότι θα πάρει αρκετό χρόνο. Θα πρέπει να ρίξετε μια ματιά στo documentation του Django αν θέλετε να μάθετε περισσότερα σχετικά με τα πεδία των μοντέλων και πως να ορίζετε πράγματα εκτός από αυτά που αναφέρθηκαν παραπάνω (https://docs.djangoproject.com/en/2.0/ref/models/fields/#field-types).

Τι γίνεται με την μέθοδο `def publish(self):`; Αυτή είναι ακριβώς η μέθοδος `publish` για την οποία μιλούσαμε πριν. `def` σημαίνει ορίζουμε μια συνάρτηση/μέθοδο (ανάλογα αν είναι μέρος τηε κλάσης ή όχι) και `publish` είναι το όνομα της. Μπορείτε να αλλάξετε το όνομα της μεθόδου αν θέλετε. Ο κανόνας ονομασίας είναι ότι χρησιμοποιούμε πεζά γράμματα και κάτω παύλες αντί για κενά. Αν χρησιμοποιήσετε κενά τότε λάβετε σφάλμα. Για παράδειγμα, μια μέθοδος που υπολογίζει την μέση τιμή θα λεγόταν `calculate_average_price`.

Οι μέθοδοι συχνά κάνουν `return` κάτι, δηλαδή επιστρέφουν μια τιμή ή οτιδήποτε άλλο. Μπορεί όμως και όχι. Υπάρχει ένα παράδειγμα αυτού στη μέθοδο `__str__`. Σε αυτό το σενάριο, όταν καλούμε την `__str__()` θα λάβουμε ένα κείμενο (**string**) με τον τίτλο του post.

Επίσης προσέξτε ότι και το `def publish(self):` και το `def __str__(self):` είναι δηλωμένα μέσα στην κλάση μας (με κενά ή με tab). Επειδή η Python είναι ευαίσθητη στα κενά (στους κενούς χαρακτήρες), θα πρέπει να "βάλουμε" τις μεθόδους μας μέσα στην κλάση. Αλλιώς, οι μέθοδοι δεν θα ανήκουν στην κλάση και μπορεί να συναντήσετε απρόοπτη συμπεριφορά.

Εάν κάτι ακόμα δεν είναι ξεκάθαρο σχετικά με τα μοντέλα, μη διστάσετε να ρωτήσετε τον επιτηρητή σας! Ξέρουμε ότι είναι περίπλοκο, ειδικά όταν μαθαίνετε τι είναι τα αντικείμενα και συναρτήσεις ταυτόχρονα. Αλλά ελπίζουμε να μοιάζει λιγότερο μαγικό για εσάς τώρα!

### Δημιουργία πινάκων για μοντέλα στην βάση δεδομένων σας

Το τελευταίο βήμα εδώ είναι να προσθέσουμε το νέο μοντέλο μας στην βάση δεδομένων μας. Πρώτα πρέπει να ενημερώσουμε το Django ότι έχουμε κάποιες αλλαγές στο μοντέλο μας. (Μόλις το δημιουργήσαμε!) Πηγαίνετε στο παράθυρο της κονσόλας σας και πληκτρολογήστε `python manage.py makemigrations blog`. Θα μοιάζει κάπως έτσι:

{% filename %}command-line{% endfilename %}

    (myvenv) ~/djangogirls$ python manage.py makemigrations blog
    Migrations for 'blog':
      blog/migrations/0001_initial.py:
    
      - Create model Post
    

**Σημείωση:** Θυμηθείτε να αποθηκεύετε τα αρχεία που επεξεργάζεστε. Αλλιώς, ο υπολογιστής σας θα εκτελέσει τις προηγούμενες εκδόσεις που μπορεί να σας δώσει μηνύματα απρόοπτων σφαλμάτων.

Το Django προετοίμασε ένα αρχείο migration για εμάς που πρέπει τώρα να εφαρμόσουμε στην βάση δεδομένων μας. Πληκτρολογήστε `python manage.py migrate blog` και τo αποτελέσματα πρέπει να είναι όπως παρακάτω:

{% filename %}command-line{% endfilename %}

    (myvenv) ~/djangogirls$ python manage.py migrate blog
    Operations to perform:
      Apply all migrations: blog
    Running migrations:
      Applying blog.0001_initial... OK
    

Ζήτω! Το Post μοντέλο μας είναι στην βάση δεδομένων! Θα ήταν ωραίο να το δούμε, σωστά; Πηγαίνετε στο επόμενο κεφάλαιο για να δείτε πως μοιάζει η ανάρτηση σας!